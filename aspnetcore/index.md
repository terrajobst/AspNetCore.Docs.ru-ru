---
title: Введение в ASP.NET Core
author: rick-anderson
description: Введение в ASP.NET Core — кроссплатформенную высокопроизводительную платформу с открытым исходным кодом для создания современных облачных интернет-приложений.
ms.author: riande
ms.custom: mvc
ms.date: 11/16/2018
uid: index
ms.openlocfilehash: 13bb8fafe8d5d34185b3016630d5dd3df4212119
ms.sourcegitcommit: bdfba5e7575b2a786ef27c0edf688c7dbd09ee95
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/22/2018
ms.locfileid: "52288659"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="f9d88-103">Введение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f9d88-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="f9d88-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27), [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT) и [Шон Луттин (Shaun Luttin)](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="f9d88-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="f9d88-105">ASP.NET Core является кроссплатформенной, высокопроизводительной средой с [открытым исходным кодом](https://github.com/aspnet/home) для создания современных облачных приложений, подключенных к Интернету.</span><span class="sxs-lookup"><span data-stu-id="f9d88-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="f9d88-106">ASP.NET Core позволяет выполнять следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="f9d88-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="f9d88-107">Создавать веб-приложения и службы, приложения [IoT](https://www.microsoft.com/internet-of-things/) и серверные части для мобильных приложений.</span><span class="sxs-lookup"><span data-stu-id="f9d88-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="f9d88-108">Использовать избранные средства разработки в Windows, macOS и Linux.</span><span class="sxs-lookup"><span data-stu-id="f9d88-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="f9d88-109">Выполнять развертывания в облаке или локальной среде.</span><span class="sxs-lookup"><span data-stu-id="f9d88-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="f9d88-110">Работать в [.NET Core или .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="f9d88-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="f9d88-111">Зачем использовать ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="f9d88-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="f9d88-112">Миллионы разработчиков использовали и продолжают использовать [ASP.NET 4.x](/aspnet/overview) для создания веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="f9d88-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="f9d88-113">ASP.NET Core — это модификация ASP.NET 4.x с архитектурными изменениями, формирующими более рациональную и более модульную платформу.</span><span class="sxs-lookup"><span data-stu-id="f9d88-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="f9d88-114">Создание веб-API и пользовательского веб-интерфейса с помощью ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="f9d88-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="f9d88-115">ASP.NET Core MVC предоставляет функции, которые позволяют создавать [веб-интерфейсы API](xref:tutorials/first-web-api) и [веб-приложения](xref:tutorials/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="f9d88-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="f9d88-116">[Шаблон Model-View-Controller (MVC)](xref:mvc/overview) помогает сделать веб-API и веб-приложения тестируемыми.</span><span class="sxs-lookup"><span data-stu-id="f9d88-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="f9d88-117">[Страницы Razor](xref:razor-pages/index) (новый компонент в ASP.NET Core 2.0) — это основанная на страницах модель программирования, которая упрощает создание пользовательского веб-интерфейса и повышает его эффективность.</span><span class="sxs-lookup"><span data-stu-id="f9d88-117">[Razor Pages](xref:razor-pages/index) (new in ASP.NET Core 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="f9d88-118">[Разметка Razor](xref:mvc/views/razor) предоставляет эффективный синтаксис для [страниц Razor](xref:razor-pages/index) и [представлений MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="f9d88-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="f9d88-119">[Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) позволяют серверному коду участвовать в создании и отображении HTML-элементов в файлах Razor.</span><span class="sxs-lookup"><span data-stu-id="f9d88-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="f9d88-120">Благодаря встроенной поддержке [нескольких форматов данных и согласованию содержимого](xref:web-api/advanced/formatting) веб-API становятся доступными для множества клиентов, включая браузеры и мобильные устройства.</span><span class="sxs-lookup"><span data-stu-id="f9d88-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="f9d88-121">[Привязка модели](xref:mvc/models/model-binding) автоматически сопоставляет данные из HTTP-запросов с параметрами методов действия.</span><span class="sxs-lookup"><span data-stu-id="f9d88-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="f9d88-122">[Проверка модели](xref:mvc/models/validation) автоматически выполняется на стороне сервера и клиента.</span><span class="sxs-lookup"><span data-stu-id="f9d88-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="f9d88-123">Клиентская разработка</span><span class="sxs-lookup"><span data-stu-id="f9d88-123">Client-side development</span></span>

<span data-ttu-id="f9d88-124">ASP.NET Core легко интегрируется с распространенными клиентскими платформами и библиотеками, в том числе [Angular](xref:spa/angular), [React](xref:spa/react) и [Bootstrap](https://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="f9d88-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="f9d88-125">Дополнительные сведения см. в разделе о [клиентской разработке](xref:client-side/index).</span><span class="sxs-lookup"><span data-stu-id="f9d88-125">For more information, see [Client-side development](xref:client-side/index).</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="f9d88-126">ASP.NET Core для платформы .NET Framework</span><span class="sxs-lookup"><span data-stu-id="f9d88-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="f9d88-127">Приложения ASP.NET Core 2.x могут выполняться в .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f9d88-127">ASP.NET Core 2.x can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="f9d88-128">Приложения ASP.NET Core, предназначенные для .NET Framework, не являются кроссплатформенными &mdash; они выполняются только в Windows.</span><span class="sxs-lookup"><span data-stu-id="f9d88-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="f9d88-129">Как правило, ASP.NET Core 2.x состоит из библиотек [.NET Standard](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="f9d88-129">Generally, ASP.NET Core 2.x is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="f9d88-130">Приложения, написанные для .NET Standard 2.0 запускаются везде, где есть поддержка .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="f9d88-130">Apps written with .NET Standard 2.0 run anywhere that .NET Standard 2.0 is supported.</span></span>

<span data-ttu-id="f9d88-131">ASP.NET Core 2.x поддерживается в версиях .NET Framework, совместимых с .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="f9d88-131">ASP.NET Core 2.x is supported on .NET Framework versions compatible with .NET Standard 2.0:</span></span>

* <span data-ttu-id="f9d88-132">.NET Framework 4.7.1 и более поздних версий (рекомендуется).</span><span class="sxs-lookup"><span data-stu-id="f9d88-132">.NET Framework 4.7.1 and later is strongly recommended.</span></span>
* <span data-ttu-id="f9d88-133">.NET Framework 4.6.1 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="f9d88-133">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="f9d88-134">ASP.NET Core версии 3.0 и более поздних будут выполняться только в .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f9d88-134">ASP.NET Core 3.0 and later will only run on .NET Core.</span></span> <span data-ttu-id="f9d88-135">Дополнительные сведения об этом изменении см. в разделе [Первое знакомство с предстоящими изменениями в ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span><span class="sxs-lookup"><span data-stu-id="f9d88-135">For more details regarding this change, see [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<span data-ttu-id="f9d88-136">При использовании .NET Core существуют некоторые преимущества, и их число увеличивается с каждым выпуском.</span><span class="sxs-lookup"><span data-stu-id="f9d88-136">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="f9d88-137">Преимущества .NET Core по сравнению с .NET Framework включают:</span><span class="sxs-lookup"><span data-stu-id="f9d88-137">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="f9d88-138">Кроссплатформенность.</span><span class="sxs-lookup"><span data-stu-id="f9d88-138">Cross-platform.</span></span> <span data-ttu-id="f9d88-139">Выполняется на macOS, Linux и Windows.</span><span class="sxs-lookup"><span data-stu-id="f9d88-139">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="f9d88-140">Повышение производительности</span><span class="sxs-lookup"><span data-stu-id="f9d88-140">Improved performance</span></span>
* <span data-ttu-id="f9d88-141">Управление параллельными версиями</span><span class="sxs-lookup"><span data-stu-id="f9d88-141">Side-by-side versioning</span></span>
* <span data-ttu-id="f9d88-142">Новые интерфейсы API</span><span class="sxs-lookup"><span data-stu-id="f9d88-142">New APIs</span></span>
* <span data-ttu-id="f9d88-143">Открытый исходный код</span><span class="sxs-lookup"><span data-stu-id="f9d88-143">Open source</span></span>

<span data-ttu-id="f9d88-144">Мы прилагаем максимум усилий, чтобы устранить различия API между .NET Framework и .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f9d88-144">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="f9d88-145">Благодаря [пакету обеспечения совместимости Windows](/dotnet/core/porting/windows-compat-pack) в .NET Core доступны тысячи API-интерфейсов, созданных только для Windows.</span><span class="sxs-lookup"><span data-stu-id="f9d88-145">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="f9d88-146">Эти API-интерфейсы не были доступны в .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="f9d88-146">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="f9d88-147">Загрузка примера</span><span class="sxs-lookup"><span data-stu-id="f9d88-147">How to download a sample</span></span>

<span data-ttu-id="f9d88-148">Многие статьи и учебники содержат ссылки на примеры кода.</span><span class="sxs-lookup"><span data-stu-id="f9d88-148">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="f9d88-149">[Загрузите ZIP-файл репозитория ASP.NET](https://codeload.github.com/aspnet/Docs/zip/master).</span><span class="sxs-lookup"><span data-stu-id="f9d88-149">[Download the ASP.NET repository zip file](https://codeload.github.com/aspnet/Docs/zip/master).</span></span>
1. <span data-ttu-id="f9d88-150">Распакуйте файл *Docs-master.zip*.</span><span class="sxs-lookup"><span data-stu-id="f9d88-150">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="f9d88-151">Перейдите в папку примера по URL-адресу, указанному в примере.</span><span class="sxs-lookup"><span data-stu-id="f9d88-151">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

### <a name="preprocessor-directives-in-sample-code"></a><span data-ttu-id="f9d88-152">Директивы препроцессора в примере кода</span><span class="sxs-lookup"><span data-stu-id="f9d88-152">Preprocessor directives in sample code</span></span>

<span data-ttu-id="f9d88-153">Для демонстрации нескольких сценариев в примерах приложений используются инструкции C# `#define` и `#if-#else/#elif-#endif`, выборочно компилирующие и запускающие разные фрагменты примеров кода.</span><span class="sxs-lookup"><span data-stu-id="f9d88-153">To demonstrate multiple scenarios, sample apps use the `#define` and `#if-#else/#elif-#endif` C# statements to selectively compile and run different sections of sample code.</span></span> <span data-ttu-id="f9d88-154">В примерах, где применяется этот подход, задайте в начале файлов C# инструкцию `#define` для символа, связанного со сценарием, который нужно запустить.</span><span class="sxs-lookup"><span data-stu-id="f9d88-154">For those samples that make use of this approach, set the `#define` statement at the top of the C# files to the symbol associated with the scenario that you want to run.</span></span> <span data-ttu-id="f9d88-155">Для запуска сценария в некоторых примерах потребуется задать символ в начале нескольких файлов.</span><span class="sxs-lookup"><span data-stu-id="f9d88-155">Some samples require setting the symbol at the top of multiple files in order to run a scenario.</span></span>

<span data-ttu-id="f9d88-156">Например, в следующем списке символов `#define` видно, что доступно четыре сценария (один сценарий на символ).</span><span class="sxs-lookup"><span data-stu-id="f9d88-156">For example, the following `#define` symbol list indicates that four scenarios are available (one scenario per symbol).</span></span> <span data-ttu-id="f9d88-157">В текущем примере конфигурации запускается сценарий `TemplateCode`:</span><span class="sxs-lookup"><span data-stu-id="f9d88-157">The current sample configuration runs the `TemplateCode` scenario:</span></span>

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

<span data-ttu-id="f9d88-158">Чтобы запустить в примере сценарий `ExpandDefault`, задайте символ `ExpandDefault` и оставьте остальные символы раскомментированными:</span><span class="sxs-lookup"><span data-stu-id="f9d88-158">To change the sample to run the `ExpandDefault` scenario, define the `ExpandDefault` symbol and leave the remaining symbols commented-out:</span></span>

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

<span data-ttu-id="f9d88-159">Дополнительные сведения об использовании [директив препроцессора C#](/dotnet/csharp/language-reference/preprocessor-directives/) для выборочной компиляции фрагментов кода см. в разделах [#define (Справочник по C#)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) и [#if (Справочник по C#)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span><span class="sxs-lookup"><span data-stu-id="f9d88-159">For more information on using [C# preprocessor directives](/dotnet/csharp/language-reference/preprocessor-directives/) to selectively compile sections of code, see [#define (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) and [#if (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span></span>

### <a name="regions-in-sample-code"></a><span data-ttu-id="f9d88-160">Регионы в примере кода</span><span class="sxs-lookup"><span data-stu-id="f9d88-160">Regions in sample code</span></span>

<span data-ttu-id="f9d88-161">Некоторые примеры приложений содержат фрагменты кода внутри инструкций C# [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) и [#end-region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion).</span><span class="sxs-lookup"><span data-stu-id="f9d88-161">Some sample apps contain sections of code surrounded by [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) and [#end-region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# statements.</span></span> <span data-ttu-id="f9d88-162">Система сборки документации вставляет эти регионы в обработанные разделы документации.</span><span class="sxs-lookup"><span data-stu-id="f9d88-162">The documentation build system injects these regions into the rendered documentation topics.</span></span>  

<span data-ttu-id="f9d88-163">Названия регионов обычно содержат слово "фрагмент".</span><span class="sxs-lookup"><span data-stu-id="f9d88-163">Region names usually contain the word "snippet."</span></span> <span data-ttu-id="f9d88-164">В следующем примере показан регион с именем `snippet_FilterInCode`:</span><span class="sxs-lookup"><span data-stu-id="f9d88-164">The following example shows a region named `snippet_FilterInCode`:</span></span>

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

<span data-ttu-id="f9d88-165">На предыдущий фрагмент кода C# указывает ссылка в следующей строке в файле Markdown раздела:</span><span class="sxs-lookup"><span data-stu-id="f9d88-165">The preceding C# code snippet is referenced in the topic's markdown file with the following line:</span></span>

```
[!code-csharp[](sample/SampleApp/Program.cs?name=snippet_FilterInCode)]
```

<span data-ttu-id="f9d88-166">Вы можете спокойно проигнорировать или удалить инструкции `#region` и `#end-region` вокруг кода.</span><span class="sxs-lookup"><span data-stu-id="f9d88-166">You may safely ignore (or remove) the `#region` and `#end-region` statements that surround the code.</span></span> <span data-ttu-id="f9d88-167">Не изменяйте код внутри этих инструкций, если планируете запустить примеры сценариев, описанные в разделе.</span><span class="sxs-lookup"><span data-stu-id="f9d88-167">Don't alter the code within these statements if you plan to run the sample scenarios described in the topic.</span></span> <span data-ttu-id="f9d88-168">Вы можете изменить код, экспериментируя с другими сценариями.</span><span class="sxs-lookup"><span data-stu-id="f9d88-168">Feel free to alter the code when experimenting with other scenarios.</span></span>

<span data-ttu-id="f9d88-169">Дополнительные сведения см. в разделе [Вклад в документацию ASP.NET: фрагменты кода](https://github.com/aspnet/Docs/blob/master/CONTRIBUTING.md#code-snippets).</span><span class="sxs-lookup"><span data-stu-id="f9d88-169">For more information, see [Contribute to the ASP.NET documentation: Code snippets](https://github.com/aspnet/Docs/blob/master/CONTRIBUTING.md#code-snippets).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f9d88-170">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="f9d88-170">Next steps</span></span>

<span data-ttu-id="f9d88-171">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="f9d88-171">For more information, see the following resources:</span></span>

* [<span data-ttu-id="f9d88-172">Начало работы с Razor Pages</span><span class="sxs-lookup"><span data-stu-id="f9d88-172">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="f9d88-173">Основы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f9d88-173">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="f9d88-174">[В еженедельном выпуске ASP.NET Community Standup](https://live.asp.net/) рассматривается ход работы и планы команды.</span><span class="sxs-lookup"><span data-stu-id="f9d88-174">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="f9d88-175">Помимо этого, публикуются новые блоги и стороннее программное обеспечение.</span><span class="sxs-lookup"><span data-stu-id="f9d88-175">It features new blogs and third-party software.</span></span>

> [!NOTE]
> <span data-ttu-id="f9d88-176">Мы тестируем удобство использования новой предлагаемой структуры оглавления для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f9d88-176">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="f9d88-177">Если у вас есть несколько минут, и вы хотите попробовать найти семь разных тем в существующей или планируемой структуре оглавления, [щелкните здесь, чтобы принять участие в исследовании](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span><span class="sxs-lookup"><span data-stu-id="f9d88-177">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>