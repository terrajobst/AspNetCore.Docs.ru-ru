---
title: Введение в ASP.NET Core
author: rick-anderson
description: "Введение в ASP.NET Core\_— кроссплатформенную высокопроизводительную платформу с открытым исходным кодом для создания современных облачных интернет-приложений."
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2019
uid: index
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="937fd-103">Введение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="937fd-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="937fd-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27), [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT) и [Шон Луттин (Shaun Luttin)](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="937fd-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="937fd-105">ASP.NET Core является кроссплатформенной, высокопроизводительной средой с [открытым исходным кодом](https://github.com/aspnet/home) для создания современных облачных приложений, подключенных к Интернету.</span><span class="sxs-lookup"><span data-stu-id="937fd-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="937fd-106">ASP.NET Core позволяет выполнять следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="937fd-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="937fd-107">Создавать веб-приложения и службы, приложения [IoT](https://www.microsoft.com/internet-of-things/) и серверные части для мобильных приложений.</span><span class="sxs-lookup"><span data-stu-id="937fd-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="937fd-108">Использовать избранные средства разработки в Windows, macOS и Linux.</span><span class="sxs-lookup"><span data-stu-id="937fd-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="937fd-109">Выполнять развертывания в облаке или локальной среде.</span><span class="sxs-lookup"><span data-stu-id="937fd-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="937fd-110">Работать в [.NET Core или .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="937fd-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-to-use-aspnet-core"></a><span data-ttu-id="937fd-111">Зачем использовать ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="937fd-111">Why to use ASP.NET Core</span></span>

<span data-ttu-id="937fd-112">Миллионы разработчиков использовали и продолжают использовать [ASP.NET 4.x](/aspnet/overview) для создания веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="937fd-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="937fd-113">ASP.NET Core — это модификация ASP.NET 4.x с архитектурными изменениями, формирующими более рациональную и более модульную платформу.</span><span class="sxs-lookup"><span data-stu-id="937fd-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="937fd-114">Создание веб-API и пользовательского веб-интерфейса с помощью ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="937fd-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="937fd-115">ASP.NET Core MVC предоставляет функции, которые позволяют создавать [веб-интерфейсы API](xref:tutorials/first-web-api) и [веб-приложения](xref:tutorials/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="937fd-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="937fd-116">[Шаблон Model-View-Controller (MVC)](xref:mvc/overview) помогает сделать веб-API и веб-приложения тестируемыми.</span><span class="sxs-lookup"><span data-stu-id="937fd-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="937fd-117">[Razor Pages](xref:razor-pages/index) — это основанная на страницах модель программирования, которая упрощает и повышает эффективность создания пользовательского веб-интерфейса.</span><span class="sxs-lookup"><span data-stu-id="937fd-117">[Razor Pages](xref:razor-pages/index) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="937fd-118">[Разметка Razor](xref:mvc/views/razor) предоставляет эффективный синтаксис для [страниц Razor](xref:razor-pages/index) и [представлений MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="937fd-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="937fd-119">[Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) позволяют серверному коду участвовать в создании и отображении HTML-элементов в файлах Razor.</span><span class="sxs-lookup"><span data-stu-id="937fd-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="937fd-120">Благодаря встроенной поддержке [нескольких форматов данных и согласованию содержимого](xref:web-api/advanced/formatting) веб-API становятся доступными для множества клиентов, включая браузеры и мобильные устройства.</span><span class="sxs-lookup"><span data-stu-id="937fd-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="937fd-121">[Привязка модели](xref:mvc/models/model-binding) автоматически сопоставляет данные из HTTP-запросов с параметрами методов действия.</span><span class="sxs-lookup"><span data-stu-id="937fd-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="937fd-122">[Проверка модели](xref:mvc/models/validation) автоматически выполняется на стороне сервера и клиента.</span><span class="sxs-lookup"><span data-stu-id="937fd-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="937fd-123">Клиентская разработка</span><span class="sxs-lookup"><span data-stu-id="937fd-123">Client-side development</span></span>

<span data-ttu-id="937fd-124">ASP.NET Core легко интегрируется с распространенными клиентскими платформами и библиотеками, в том числе [компонентами Razor](xref:razor-components/index), [Angular](xref:spa/angular), [React](xref:spa/react) и [Bootstrap](https://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="937fd-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Razor Components](xref:razor-components/index), [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="937fd-125">Дополнительные сведения см. в разделе [Компоненты Razor](xref:razor-components/index), а также в похожих темах в разделе *Разработка на стороне клиента*.</span><span class="sxs-lookup"><span data-stu-id="937fd-125">For more information, see [Razor Components](xref:razor-components/index) and related topics under *Client-side development*.</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="937fd-126">ASP.NET Core для платформы .NET Framework</span><span class="sxs-lookup"><span data-stu-id="937fd-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="937fd-127">Приложения ASP.NET Core 2.x могут выполняться в .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="937fd-127">ASP.NET Core 2.x can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="937fd-128">Приложения ASP.NET Core, предназначенные для .NET Framework, не являются кроссплатформенными &mdash; они выполняются только в Windows.</span><span class="sxs-lookup"><span data-stu-id="937fd-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="937fd-129">Как правило, ASP.NET Core 2.x состоит из библиотек [.NET Standard](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="937fd-129">Generally, ASP.NET Core 2.x is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="937fd-130">Приложения, написанные для .NET Standard 2.0 запускаются везде, где есть поддержка .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="937fd-130">Apps written with .NET Standard 2.0 run anywhere that .NET Standard 2.0 is supported.</span></span>

<span data-ttu-id="937fd-131">ASP.NET Core 2.x поддерживается в версиях .NET Framework, совместимых с .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="937fd-131">ASP.NET Core 2.x is supported on .NET Framework versions compatible with .NET Standard 2.0:</span></span>

* <span data-ttu-id="937fd-132">.NET Framework 4.7.1 и более поздних версий (рекомендуется).</span><span class="sxs-lookup"><span data-stu-id="937fd-132">.NET Framework 4.7.1 and later is strongly recommended.</span></span>
* <span data-ttu-id="937fd-133">.NET Framework 4.6.1 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="937fd-133">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="937fd-134">ASP.NET Core версии 3.0 и более поздних будут выполняться только в .NET Core.</span><span class="sxs-lookup"><span data-stu-id="937fd-134">ASP.NET Core 3.0 and later will only run on .NET Core.</span></span> <span data-ttu-id="937fd-135">Дополнительные сведения об этом изменении см. в разделе [Первое знакомство с предстоящими изменениями в ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span><span class="sxs-lookup"><span data-stu-id="937fd-135">For more details regarding this change, see [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<span data-ttu-id="937fd-136">При использовании .NET Core существуют некоторые преимущества, и их число увеличивается с каждым выпуском.</span><span class="sxs-lookup"><span data-stu-id="937fd-136">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="937fd-137">Преимущества .NET Core по сравнению с .NET Framework включают:</span><span class="sxs-lookup"><span data-stu-id="937fd-137">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="937fd-138">Кроссплатформенность.</span><span class="sxs-lookup"><span data-stu-id="937fd-138">Cross-platform.</span></span> <span data-ttu-id="937fd-139">Выполняется на macOS, Linux и Windows.</span><span class="sxs-lookup"><span data-stu-id="937fd-139">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="937fd-140">Повышение производительности</span><span class="sxs-lookup"><span data-stu-id="937fd-140">Improved performance</span></span>
* <span data-ttu-id="937fd-141">Управление параллельными версиями</span><span class="sxs-lookup"><span data-stu-id="937fd-141">Side-by-side versioning</span></span>
* <span data-ttu-id="937fd-142">Новые интерфейсы API</span><span class="sxs-lookup"><span data-stu-id="937fd-142">New APIs</span></span>
* <span data-ttu-id="937fd-143">Открытый исходный код</span><span class="sxs-lookup"><span data-stu-id="937fd-143">Open source</span></span>

<span data-ttu-id="937fd-144">Мы прилагаем максимум усилий, чтобы устранить различия API между .NET Framework и .NET Core.</span><span class="sxs-lookup"><span data-stu-id="937fd-144">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="937fd-145">Благодаря [пакету обеспечения совместимости Windows](/dotnet/core/porting/windows-compat-pack) в .NET Core доступны тысячи API-интерфейсов, созданных только для Windows.</span><span class="sxs-lookup"><span data-stu-id="937fd-145">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="937fd-146">Эти API-интерфейсы не были доступны в .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="937fd-146">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="recommended-learning-path"></a><span data-ttu-id="937fd-147">Рекомендуемая схема обучения</span><span class="sxs-lookup"><span data-stu-id="937fd-147">Recommended learning path</span></span>

<span data-ttu-id="937fd-148">Для знакомства с разработкой приложений ASP.NET Core рекомендуется изучить следующую последовательность учебников и статей.</span><span class="sxs-lookup"><span data-stu-id="937fd-148">We recommend the following sequence of tutorials and articles for an introduction to developing ASP.NET Core apps:</span></span>

1. <span data-ttu-id="937fd-149">Пройдите учебник по тому типу приложения, которое вы собираетесь разрабатывать или обслуживать:</span><span class="sxs-lookup"><span data-stu-id="937fd-149">Follow a tutorial for the type of app you want to develop or maintain:</span></span>

   |<span data-ttu-id="937fd-150">Тип приложения</span><span class="sxs-lookup"><span data-stu-id="937fd-150">App type</span></span>  |<span data-ttu-id="937fd-151">Сценарий</span><span class="sxs-lookup"><span data-stu-id="937fd-151">Scenario</span></span>  |<span data-ttu-id="937fd-152">Учебник</span><span class="sxs-lookup"><span data-stu-id="937fd-152">Tutorial</span></span>  |
   |----------|----------|----------|
   |<span data-ttu-id="937fd-153">Веб-приложение</span><span class="sxs-lookup"><span data-stu-id="937fd-153">Web app</span></span>       | <span data-ttu-id="937fd-154">Разработка нового приложения</span><span class="sxs-lookup"><span data-stu-id="937fd-154">For new development</span></span>        |[<span data-ttu-id="937fd-155">Начало работы с Razor Pages</span><span class="sxs-lookup"><span data-stu-id="937fd-155">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start) |
   |<span data-ttu-id="937fd-156">Веб-приложение</span><span class="sxs-lookup"><span data-stu-id="937fd-156">Web app</span></span>       | <span data-ttu-id="937fd-157">Обслуживание приложения MVC</span><span class="sxs-lookup"><span data-stu-id="937fd-157">For maintaining an MVC app</span></span> |[<span data-ttu-id="937fd-158">Начало работы с MVC</span><span class="sxs-lookup"><span data-stu-id="937fd-158">Get started with MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)|
   |<span data-ttu-id="937fd-159">Веб-интерфейс API</span><span class="sxs-lookup"><span data-stu-id="937fd-159">Web API</span></span>       |                            |<span data-ttu-id="937fd-160">[Создание веб-API](xref:tutorials/first-web-api)\*</span><span class="sxs-lookup"><span data-stu-id="937fd-160">[Create a web API](xref:tutorials/first-web-api)\*</span></span>  |
   |<span data-ttu-id="937fd-161">Приложение режима реального времени</span><span class="sxs-lookup"><span data-stu-id="937fd-161">Real-time app</span></span> |                            |[<span data-ttu-id="937fd-162">Начало работы с SignalR</span><span class="sxs-lookup"><span data-stu-id="937fd-162">Get started with SignalR</span></span>](xref:tutorials/signalr) |

1. <span data-ttu-id="937fd-163">Пройдите учебник, посвященный основам доступа к данным:</span><span class="sxs-lookup"><span data-stu-id="937fd-163">Follow a tutorial that shows how to do basic data access:</span></span>

   |<span data-ttu-id="937fd-164">Сценарий</span><span class="sxs-lookup"><span data-stu-id="937fd-164">Scenario</span></span>  |<span data-ttu-id="937fd-165">Учебник</span><span class="sxs-lookup"><span data-stu-id="937fd-165">Tutorial</span></span>  |
   |----------|----------|
   | <span data-ttu-id="937fd-166">Разработка нового приложения</span><span class="sxs-lookup"><span data-stu-id="937fd-166">For new development</span></span>        |[<span data-ttu-id="937fd-167">Razor Pages с Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="937fd-167">Razor Pages with Entity Framework Core</span></span>](xref:data/ef-rp/intro) |
   | <span data-ttu-id="937fd-168">Обслуживание приложения MVC</span><span class="sxs-lookup"><span data-stu-id="937fd-168">For maintaining an MVC app</span></span> |[<span data-ttu-id="937fd-169">MVC с Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="937fd-169">MVC with Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

1. <span data-ttu-id="937fd-170">Прочтите обзор функций ASP.NET Core, относящихся ко всем типам приложений:</span><span class="sxs-lookup"><span data-stu-id="937fd-170">Read an overview of ASP.NET Core features that apply to all app types:</span></span>

   * [<span data-ttu-id="937fd-171">Основы</span><span class="sxs-lookup"><span data-stu-id="937fd-171">Fundamentals</span></span>](xref:fundamentals/index)

1. <span data-ttu-id="937fd-172">Просмотрите содержание, чтобы найти другие интересующие вас темы.</span><span class="sxs-lookup"><span data-stu-id="937fd-172">Browse the Table of Contents for other topics of interest.</span></span>

<span data-ttu-id="937fd-173">\* Доступен новый [учебник по веб-API с прохождением в браузере](https://docs.microsoft.com/learn/modules/build-web-api-net-core), не требующий установки локальной интегрированной среды разработки.</span><span class="sxs-lookup"><span data-stu-id="937fd-173">\* There is a new [web API tutorial that you follow entirely in the browser](https://docs.microsoft.com/learn/modules/build-web-api-net-core), no local IDE installation required.</span></span>  <span data-ttu-id="937fd-174">Код выполняется в [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/), а для тестирования используется [curl](https://curl.haxx.se/).</span><span class="sxs-lookup"><span data-stu-id="937fd-174">The code runs in an [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/), and [curl](https://curl.haxx.se/) is used for testing.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="937fd-175">Загрузка примера</span><span class="sxs-lookup"><span data-stu-id="937fd-175">How to download a sample</span></span>

<span data-ttu-id="937fd-176">Многие статьи и учебники содержат ссылки на примеры кода.</span><span class="sxs-lookup"><span data-stu-id="937fd-176">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="937fd-177">[Загрузите ZIP-файл репозитория ASP.NET](https://codeload.github.com/aspnet/Docs/zip/master).</span><span class="sxs-lookup"><span data-stu-id="937fd-177">[Download the ASP.NET repository zip file](https://codeload.github.com/aspnet/Docs/zip/master).</span></span>
1. <span data-ttu-id="937fd-178">Распакуйте файл *Docs-master.zip*.</span><span class="sxs-lookup"><span data-stu-id="937fd-178">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="937fd-179">Перейдите в папку примера по URL-адресу, указанному в примере.</span><span class="sxs-lookup"><span data-stu-id="937fd-179">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

### <a name="preprocessor-directives-in-sample-code"></a><span data-ttu-id="937fd-180">Директивы препроцессора в примере кода</span><span class="sxs-lookup"><span data-stu-id="937fd-180">Preprocessor directives in sample code</span></span>

<span data-ttu-id="937fd-181">Для демонстрации нескольких сценариев в примерах приложений используются инструкции C# `#define` и `#if-#else/#elif-#endif`, выборочно компилирующие и запускающие разные фрагменты примеров кода.</span><span class="sxs-lookup"><span data-stu-id="937fd-181">To demonstrate multiple scenarios, sample apps use the `#define` and `#if-#else/#elif-#endif` C# statements to selectively compile and run different sections of sample code.</span></span> <span data-ttu-id="937fd-182">В примерах, где применяется этот подход, задайте в начале файлов C# инструкцию `#define` для символа, связанного со сценарием, который нужно запустить.</span><span class="sxs-lookup"><span data-stu-id="937fd-182">For those samples that make use of this approach, set the `#define` statement at the top of the C# files to the symbol associated with the scenario that you want to run.</span></span> <span data-ttu-id="937fd-183">Для запуска сценария в некоторых примерах потребуется задать символ в начале нескольких файлов.</span><span class="sxs-lookup"><span data-stu-id="937fd-183">Some samples require setting the symbol at the top of multiple files in order to run a scenario.</span></span>

<span data-ttu-id="937fd-184">Например, в следующем списке символов `#define` видно, что доступно четыре сценария (один сценарий на символ).</span><span class="sxs-lookup"><span data-stu-id="937fd-184">For example, the following `#define` symbol list indicates that four scenarios are available (one scenario per symbol).</span></span> <span data-ttu-id="937fd-185">В текущем примере конфигурации запускается сценарий `TemplateCode`:</span><span class="sxs-lookup"><span data-stu-id="937fd-185">The current sample configuration runs the `TemplateCode` scenario:</span></span>

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

<span data-ttu-id="937fd-186">Чтобы запустить в примере сценарий `ExpandDefault`, задайте символ `ExpandDefault` и оставьте остальные символы раскомментированными:</span><span class="sxs-lookup"><span data-stu-id="937fd-186">To change the sample to run the `ExpandDefault` scenario, define the `ExpandDefault` symbol and leave the remaining symbols commented-out:</span></span>

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

<span data-ttu-id="937fd-187">Дополнительные сведения об использовании [директив препроцессора C#](/dotnet/csharp/language-reference/preprocessor-directives/) для выборочной компиляции фрагментов кода см. в разделах [#define (Справочник по C#)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) и [#if (Справочник по C#)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span><span class="sxs-lookup"><span data-stu-id="937fd-187">For more information on using [C# preprocessor directives](/dotnet/csharp/language-reference/preprocessor-directives/) to selectively compile sections of code, see [#define (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) and [#if (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span></span>

### <a name="regions-in-sample-code"></a><span data-ttu-id="937fd-188">Регионы в примере кода</span><span class="sxs-lookup"><span data-stu-id="937fd-188">Regions in sample code</span></span>

<span data-ttu-id="937fd-189">Некоторые примеры приложений содержат фрагменты кода внутри инструкций C# [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) и [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion).</span><span class="sxs-lookup"><span data-stu-id="937fd-189">Some sample apps contain sections of code surrounded by [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) and [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# statements.</span></span> <span data-ttu-id="937fd-190">Система сборки документации вставляет эти регионы в обработанные разделы документации.</span><span class="sxs-lookup"><span data-stu-id="937fd-190">The documentation build system injects these regions into the rendered documentation topics.</span></span>  

<span data-ttu-id="937fd-191">Названия регионов обычно содержат слово "фрагмент".</span><span class="sxs-lookup"><span data-stu-id="937fd-191">Region names usually contain the word "snippet."</span></span> <span data-ttu-id="937fd-192">В следующем примере показан регион с именем `snippet_FilterInCode`:</span><span class="sxs-lookup"><span data-stu-id="937fd-192">The following example shows a region named `snippet_FilterInCode`:</span></span>

```csharp
#region snippet_FilterInCode
WebHost.CreateDefaultBuilder(args)
    .UseStartup<Startup>()
    .ConfigureLogging(logging =>
        logging.AddFilter("System", LogLevel.Debug)
            .AddFilter<DebugLoggerProvider>("Microsoft", LogLevel.Trace))
            .Build();
#endregion
```

<span data-ttu-id="937fd-193">На предыдущий фрагмент кода C# указывает ссылка в следующей строке в файле Markdown раздела:</span><span class="sxs-lookup"><span data-stu-id="937fd-193">The preceding C# code snippet is referenced in the topic's markdown file with the following line:</span></span>

```md
[!code-csharp[](sample/SampleApp/Program.cs?name=snippet_FilterInCode)]
```

<span data-ttu-id="937fd-194">Вы можете спокойно проигнорировать или удалить инструкции `#region` и `#endregion` вокруг кода.</span><span class="sxs-lookup"><span data-stu-id="937fd-194">You may safely ignore (or remove) the `#region` and `#endregion` statements that surround the code.</span></span> <span data-ttu-id="937fd-195">Не изменяйте код внутри этих инструкций, если планируете запустить примеры сценариев, описанные в разделе.</span><span class="sxs-lookup"><span data-stu-id="937fd-195">Don't alter the code within these statements if you plan to run the sample scenarios described in the topic.</span></span> <span data-ttu-id="937fd-196">Вы можете изменить код, экспериментируя с другими сценариями.</span><span class="sxs-lookup"><span data-stu-id="937fd-196">Feel free to alter the code when experimenting with other scenarios.</span></span>

<span data-ttu-id="937fd-197">Дополнительные сведения см. в разделе[Участие в написании документации ASP.NET: Фрагменты кода](https://github.com/aspnet/Docs/blob/master/CONTRIBUTING.md#code-snippets).</span><span class="sxs-lookup"><span data-stu-id="937fd-197">For more information, see [Contribute to the ASP.NET documentation: Code snippets](https://github.com/aspnet/Docs/blob/master/CONTRIBUTING.md#code-snippets).</span></span>

## <a name="next-steps"></a><span data-ttu-id="937fd-198">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="937fd-198">Next steps</span></span>

<span data-ttu-id="937fd-199">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="937fd-199">For more information, see the following resources:</span></span>

* <xref:getting-started>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="937fd-200">Основы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="937fd-200">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="937fd-201">[В еженедельном выпуске ASP.NET Community Standup](https://live.asp.net/) рассматривается ход работы и планы команды.</span><span class="sxs-lookup"><span data-stu-id="937fd-201">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="937fd-202">Помимо этого, публикуются новые блоги и стороннее программное обеспечение.</span><span class="sxs-lookup"><span data-stu-id="937fd-202">It features new blogs and third-party software.</span></span>
