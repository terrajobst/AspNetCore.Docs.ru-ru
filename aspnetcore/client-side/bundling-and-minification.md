---
title: Объединение и минификация статических ресурсов в ASP.NET Core
author: scottaddie
description: Узнайте, как оптимизировать статические ресурсы в веб-приложении ASP.NET Core, применяя методы объединения и минификации.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/17/2019
uid: client-side/bundling-and-minification
ms.openlocfilehash: a7a5c40d6c31c4416212c02c1b491dd794f2a1d3
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78646786"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a><span data-ttu-id="952af-103">Объединение и минификация статических ресурсов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="952af-103">Bundle and minify static assets in ASP.NET Core</span></span>

<span data-ttu-id="952af-104">Авторы: [Скотт Адди](https://twitter.com/Scott_Addie) (Scott Addie) и [Дэвид Пайн](https://twitter.com/davidpine7) (David Pine)</span><span class="sxs-lookup"><span data-stu-id="952af-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [David Pine](https://twitter.com/davidpine7)</span></span>

<span data-ttu-id="952af-105">В этой статье объясняются преимущества применения объединения и минификации, в том числе сведения о том, как эти функции можно использовать для веб-приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="952af-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="952af-106">Что такое объединение и минификация</span><span class="sxs-lookup"><span data-stu-id="952af-106">What is bundling and minification</span></span>

<span data-ttu-id="952af-107">Объединение и минификации — это два различных способа оптимизации производительности, которые можно применить в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="952af-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="952af-108">Вместе объединение и минификация позволяют повысить производительность за счет сокращения числа запросов к серверу и уменьшения размера запрашиваемых статических ресурсов.</span><span class="sxs-lookup"><span data-stu-id="952af-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="952af-109">Объединение и минификация в первую очередь ускоряют загрузку первой страницы.</span><span class="sxs-lookup"><span data-stu-id="952af-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="952af-110">После запроса веб-страницы браузер кэширует статические ресурсы (JavaScript, CSS и изображения).</span><span class="sxs-lookup"><span data-stu-id="952af-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="952af-111">Следовательно, объединение и минификация не улучшают производительность при запросе одной и той же страницы или страниц на одном сайте, запрашивающем одни и те же ресурсы.</span><span class="sxs-lookup"><span data-stu-id="952af-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="952af-112">Если заголовок Expires неправильно задан в ресурсах, а объединение и минификация не используются, эвристика обновления браузера помечает ресурсы как устаревшие через несколько дней.</span><span class="sxs-lookup"><span data-stu-id="952af-112">If the expires header isn't set correctly on the assets and if bundling and minification isn't used, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="952af-113">Кроме того, браузер требует запрос проверки для каждого ресурса.</span><span class="sxs-lookup"><span data-stu-id="952af-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="952af-114">В этом случае объединение и минификация обеспечивают повышение производительности даже после запроса первой страницы.</span><span class="sxs-lookup"><span data-stu-id="952af-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="952af-115">Объединение</span><span class="sxs-lookup"><span data-stu-id="952af-115">Bundling</span></span>

<span data-ttu-id="952af-116">Объединение позволяет объединить несколько файлов в один файл.</span><span class="sxs-lookup"><span data-stu-id="952af-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="952af-117">Объединение сокращает количество запросов к серверу, необходимых для преобразования веб-ресурса, например веб-страницы для просмотра.</span><span class="sxs-lookup"><span data-stu-id="952af-117">Bundling reduces the number of server requests that are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="952af-118">Можно создать любое количество отдельных пакетов специально для CSS, JavaScript и т. д. Чем меньше файлов, тем меньше количество HTTP-запросов от браузера к серверу или от службы, предоставляющей приложение.</span><span class="sxs-lookup"><span data-stu-id="952af-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="952af-119">Это приводит к повышению производительности загрузки первой страницы.</span><span class="sxs-lookup"><span data-stu-id="952af-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="952af-120">Минификация</span><span class="sxs-lookup"><span data-stu-id="952af-120">Minification</span></span>

<span data-ttu-id="952af-121">Минификация удаляет из кода ненужные символы, не затрагивая его функциональность.</span><span class="sxs-lookup"><span data-stu-id="952af-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="952af-122">В результате значительно уменьшается размер запрашиваемых ресурсов (например, CSS, изображений и файлов JavaScript).</span><span class="sxs-lookup"><span data-stu-id="952af-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="952af-123">К обычным побочным эффектам минификации относятся сокращение имен переменных до одного символа и удаление комментариев и ненужных пробелов.</span><span class="sxs-lookup"><span data-stu-id="952af-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="952af-124">Рассмотрим следующую функцию JavaScript:</span><span class="sxs-lookup"><span data-stu-id="952af-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="952af-125">Минификация сокращает функцию до следующего:</span><span class="sxs-lookup"><span data-stu-id="952af-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="952af-126">Помимо удаления комментариев и ненужных пробелов, были переименованы следующие имена параметров и переменных:</span><span class="sxs-lookup"><span data-stu-id="952af-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="952af-127">До преобразования</span><span class="sxs-lookup"><span data-stu-id="952af-127">Original</span></span> | <span data-ttu-id="952af-128">Переименовано</span><span class="sxs-lookup"><span data-stu-id="952af-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="952af-129">Влияние объединения и минификации</span><span class="sxs-lookup"><span data-stu-id="952af-129">Impact of bundling and minification</span></span>

<span data-ttu-id="952af-130">В следующей таблице описаны различия между загрузкой каждого ресурса отдельно и применением объединения и минификации:</span><span class="sxs-lookup"><span data-stu-id="952af-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="952af-131">Действие</span><span class="sxs-lookup"><span data-stu-id="952af-131">Action</span></span> | <span data-ttu-id="952af-132">С О/М</span><span class="sxs-lookup"><span data-stu-id="952af-132">With B/M</span></span> | <span data-ttu-id="952af-133">Без О/М</span><span class="sxs-lookup"><span data-stu-id="952af-133">Without B/M</span></span> | <span data-ttu-id="952af-134">Изменение</span><span class="sxs-lookup"><span data-stu-id="952af-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="952af-135">Запросы файлов</span><span class="sxs-lookup"><span data-stu-id="952af-135">File Requests</span></span>  | <span data-ttu-id="952af-136">7</span><span class="sxs-lookup"><span data-stu-id="952af-136">7</span></span>   | <span data-ttu-id="952af-137">18</span><span class="sxs-lookup"><span data-stu-id="952af-137">18</span></span>     | <span data-ttu-id="952af-138">157 %</span><span class="sxs-lookup"><span data-stu-id="952af-138">157%</span></span>
<span data-ttu-id="952af-139">Объем переданных КБ</span><span class="sxs-lookup"><span data-stu-id="952af-139">KB Transferred</span></span> | <span data-ttu-id="952af-140">156</span><span class="sxs-lookup"><span data-stu-id="952af-140">156</span></span> | <span data-ttu-id="952af-141">264,68</span><span class="sxs-lookup"><span data-stu-id="952af-141">264.68</span></span> | <span data-ttu-id="952af-142">70 %</span><span class="sxs-lookup"><span data-stu-id="952af-142">70%</span></span>
<span data-ttu-id="952af-143">Время загрузки (мс)</span><span class="sxs-lookup"><span data-stu-id="952af-143">Load Time (ms)</span></span> | <span data-ttu-id="952af-144">885</span><span class="sxs-lookup"><span data-stu-id="952af-144">885</span></span> | <span data-ttu-id="952af-145">2360</span><span class="sxs-lookup"><span data-stu-id="952af-145">2360</span></span>   | <span data-ttu-id="952af-146">167 %</span><span class="sxs-lookup"><span data-stu-id="952af-146">167%</span></span>

<span data-ttu-id="952af-147">Браузеры не протоколируют заголовки HTTP-запросов подробно.</span><span class="sxs-lookup"><span data-stu-id="952af-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="952af-148">При объединении заметно уменьшается общий объем отправленных байтов.</span><span class="sxs-lookup"><span data-stu-id="952af-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="952af-149">Время загрузки значительное уменьшается, однако этот пример выполнялся локально.</span><span class="sxs-lookup"><span data-stu-id="952af-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="952af-150">Значительное повышение производительности достигается при использовании объединения и минификации в отношении ресурсов, передаваемых по сети.</span><span class="sxs-lookup"><span data-stu-id="952af-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="952af-151">Выбор стратегии объединения и минификации</span><span class="sxs-lookup"><span data-stu-id="952af-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="952af-152">Шаблоны проектов MVC и Razor Pages предоставляют готовые решения для объединения и минификации, состоящие из файла конфигурации JSON.</span><span class="sxs-lookup"><span data-stu-id="952af-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="952af-153">Сторонние средства, такие как запускатель задач [Grunt](xref:client-side/using-grunt), выполняют те же задачи, но с несколько более сложной реализацией.</span><span class="sxs-lookup"><span data-stu-id="952af-153">Third-party tools, such as the [Grunt](xref:client-side/using-grunt) task runner, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="952af-154">Сторонние средства отлично подходят в случаях, когда в рамках рабочего процесса разработки, помимо объединения и минификации, требуется другая обработка, &mdash; такая как контроль качества кода и оптимизация изображений.</span><span class="sxs-lookup"><span data-stu-id="952af-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="952af-155">С помощью объединения и минификации во время разработки уменьшенные файлы создаются до развертывания приложения.</span><span class="sxs-lookup"><span data-stu-id="952af-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="952af-156">Объединение и минификация до развертывания обеспечивают преимущества снижения нагрузки на сервер.</span><span class="sxs-lookup"><span data-stu-id="952af-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="952af-157">Однако важно понимать, что объединение и минификация во время разработки повышает сложность сборки и применяется только со статическими файлами.</span><span class="sxs-lookup"><span data-stu-id="952af-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="952af-158">Настройка объединения и минификации</span><span class="sxs-lookup"><span data-stu-id="952af-158">Configure bundling and minification</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="952af-159">В ASP.NET Core 2.0 или более ранних версиях шаблоны проектов MVC и Razor Pages предоставляют файл конфигурации *bundleconfig.json*, который определяет параметры для каждого пакета:</span><span class="sxs-lookup"><span data-stu-id="952af-159">In ASP.NET Core 2.0 or earlier, the MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file that defines the options for each bundle:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="952af-160">В ASP.NET Core 2.1 или более поздних версиях в корневой каталог проекта MVC или Razor Pages следует добавить файл *bundleconfig.json*.</span><span class="sxs-lookup"><span data-stu-id="952af-160">In ASP.NET Core 2.1 or later, add a new JSON file, named *bundleconfig.json*, to the MVC or Razor Pages project root.</span></span> <span data-ttu-id="952af-161">Для начала включите в этот файл следующий код JSON:</span><span class="sxs-lookup"><span data-stu-id="952af-161">Include the following JSON in that file as a starting point:</span></span>

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="952af-162">В файле *bundleconfig.json* определяются параметры для каждого пакета.</span><span class="sxs-lookup"><span data-stu-id="952af-162">The *bundleconfig.json* file defines the options for each bundle.</span></span> <span data-ttu-id="952af-163">В предыдущем примере для настраиваемого файла JavaScript (*wwwroot/JS/site.js*) и таблицы стилей (*wwwroot/CSS/site. CSS*) определена конфигурация одного пакета.</span><span class="sxs-lookup"><span data-stu-id="952af-163">In the preceding example, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files.</span></span>

<span data-ttu-id="952af-164">Доступные варианты конфигурации:</span><span class="sxs-lookup"><span data-stu-id="952af-164">Configuration options include:</span></span>

* <span data-ttu-id="952af-165">`outputFileName`. имя файла пакета для вывода.</span><span class="sxs-lookup"><span data-stu-id="952af-165">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="952af-166">Может содержать относительный путь из файла *bundleconfig.json*.</span><span class="sxs-lookup"><span data-stu-id="952af-166">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="952af-167">**обязательный параметр**</span><span class="sxs-lookup"><span data-stu-id="952af-167">**required**</span></span>
* <span data-ttu-id="952af-168">`inputFiles`. массив файлов для объединения.</span><span class="sxs-lookup"><span data-stu-id="952af-168">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="952af-169">Это относительные пути к файлу конфигурации.</span><span class="sxs-lookup"><span data-stu-id="952af-169">These are relative paths to the configuration file.</span></span> <span data-ttu-id="952af-170">**Необязательный параметр**; \*пустое значение приводит к пустому выходному файлу.</span><span class="sxs-lookup"><span data-stu-id="952af-170">**optional**, \*an empty value results in an empty output file.</span></span> <span data-ttu-id="952af-171">Поддерживаются шаблоны [глобализации](https://www.tldp.org/LDP/abs/html/globbingref.html).</span><span class="sxs-lookup"><span data-stu-id="952af-171">[globbing](https://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="952af-172">`minify`. параметры минификации для типа выходных данных.</span><span class="sxs-lookup"><span data-stu-id="952af-172">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="952af-173">**Необязательный параметр**; *значение по умолчанию — `minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="952af-173">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="952af-174">Параметры конфигурации доступны для каждого типа выходного файла.</span><span class="sxs-lookup"><span data-stu-id="952af-174">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="952af-175">Уменьшитель CSS</span><span class="sxs-lookup"><span data-stu-id="952af-175">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="952af-176">Уменьшитель JavaScript</span><span class="sxs-lookup"><span data-stu-id="952af-176">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="952af-177">Уменьшитель HTML</span><span class="sxs-lookup"><span data-stu-id="952af-177">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="952af-178">`includeInProject`. флаг, указывающий, добавлять ли созданные файлы в файл проекта.</span><span class="sxs-lookup"><span data-stu-id="952af-178">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="952af-179">**Необязательный параметр**; *значение по умолчанию — false*</span><span class="sxs-lookup"><span data-stu-id="952af-179">**optional**, *default - false*</span></span>
* <span data-ttu-id="952af-180">`sourceMap`. флаг, указывающий, создавать ли сопоставитель с исходным кодом для объединенного файла.</span><span class="sxs-lookup"><span data-stu-id="952af-180">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="952af-181">**Необязательный параметр**; *значение по умолчанию — false*</span><span class="sxs-lookup"><span data-stu-id="952af-181">**optional**, *default - false*</span></span>
* <span data-ttu-id="952af-182">`sourceMapRootPath`. корневой путь для хранения созданного файла сопоставителя с исходным кодом.</span><span class="sxs-lookup"><span data-stu-id="952af-182">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="952af-183">Выполнение объединения и минификации во время сборки</span><span class="sxs-lookup"><span data-stu-id="952af-183">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="952af-184">Пакет NuGet [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) обеспечивает выполнение объединения и минификации во время сборки.</span><span class="sxs-lookup"><span data-stu-id="952af-184">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="952af-185">Пакет внедряет [целевые объекты MSBuild](/visualstudio/msbuild/msbuild-targets), которые выполняются во время сборки и очистки.</span><span class="sxs-lookup"><span data-stu-id="952af-185">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="952af-186">Файл *bundleconfig.json* анализируется процессом сборки для получения выходных файлов на основе определенной конфигурации.</span><span class="sxs-lookup"><span data-stu-id="952af-186">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="952af-187">BuildBundlerMinifier принадлежит к проекту сообщества в GitHub, в отношении которого корпорация Майкрософт не предоставляет поддержку.</span><span class="sxs-lookup"><span data-stu-id="952af-187">BuildBundlerMinifier belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="952af-188">О проблемах следует сообщать [сюда](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="952af-188">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="952af-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="952af-189">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="952af-190">Добавьте пакет *BuildBundlerMinifier* в свой проект.</span><span class="sxs-lookup"><span data-stu-id="952af-190">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="952af-191">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="952af-191">Build the project.</span></span> <span data-ttu-id="952af-192">В окне вывода появится следующее:</span><span class="sxs-lookup"><span data-stu-id="952af-192">The following appears in the Output window:</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>  Minified wwwroot/css/site.min.css
1>  Minified wwwroot/js/site.min.js
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

<span data-ttu-id="952af-193">Очистите проект.</span><span class="sxs-lookup"><span data-stu-id="952af-193">Clean the project.</span></span> <span data-ttu-id="952af-194">В окне вывода появится следующее:</span><span class="sxs-lookup"><span data-stu-id="952af-194">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-cli"></a>[<span data-ttu-id="952af-195">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="952af-195">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="952af-196">Добавьте пакет *BuildBundlerMinifier* в проект:</span><span class="sxs-lookup"><span data-stu-id="952af-196">Add the *BuildBundlerMinifier* package to your project:</span></span>

```dotnetcli
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="952af-197">При использовании ASP.NET Core 1.x восстановите только что добавленный пакет:</span><span class="sxs-lookup"><span data-stu-id="952af-197">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```dotnetcli
dotnet restore
```

<span data-ttu-id="952af-198">Скомпилируйте проект.</span><span class="sxs-lookup"><span data-stu-id="952af-198">Build the project:</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="952af-199">Появится следующее:</span><span class="sxs-lookup"><span data-stu-id="952af-199">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="952af-200">Очистите проект.</span><span class="sxs-lookup"><span data-stu-id="952af-200">Clean the project:</span></span>

```dotnetcli
dotnet clean
```

<span data-ttu-id="952af-201">Появится следующий результат:</span><span class="sxs-lookup"><span data-stu-id="952af-201">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="952af-202">Прямое выполнение объединения и минификации</span><span class="sxs-lookup"><span data-stu-id="952af-202">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="952af-203">Задачи объединения и минификации можно выполнять напрямую, не создавая проект.</span><span class="sxs-lookup"><span data-stu-id="952af-203">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="952af-204">Добавьте пакет NuGet [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) в свой проект:</span><span class="sxs-lookup"><span data-stu-id="952af-204">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> <span data-ttu-id="952af-205">BundlerMinifier.Core принадлежит к проекту сообщества в GitHub, в отношении которого корпорация Майкрософт не предоставляет поддержку.</span><span class="sxs-lookup"><span data-stu-id="952af-205">BundlerMinifier.Core belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="952af-206">О проблемах следует сообщать [сюда](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="952af-206">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="952af-207">Этот пакет расширяет .NET Core CLI для включения средства *dotnet-bundle*.</span><span class="sxs-lookup"><span data-stu-id="952af-207">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="952af-208">Следующую команду можно выполнить в окне консоли диспетчера пакетов (PMC) или в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="952af-208">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```dotnetcli
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="952af-209">Диспетчер пакетов NuGet добавляет зависимости в файл CSPROJ в качестве узлов `<PackageReference />`.</span><span class="sxs-lookup"><span data-stu-id="952af-209">NuGet Package Manager adds dependencies to the \*.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="952af-210">Команда `dotnet bundle` регистрируется в .NET Core CLI только при использовании узла `<DotNetCliToolReference />`.</span><span class="sxs-lookup"><span data-stu-id="952af-210">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="952af-211">Внесите соответствующие изменения в файл CSPROJ.</span><span class="sxs-lookup"><span data-stu-id="952af-211">Modify the \*.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="952af-212">Добавление файлов в рабочий процесс</span><span class="sxs-lookup"><span data-stu-id="952af-212">Add files to workflow</span></span>

<span data-ttu-id="952af-213">Рассмотрим пример, в котором добавляется дополнительный файл *custom.css*, похожий на следующий:</span><span class="sxs-lookup"><span data-stu-id="952af-213">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="952af-214">Чтобы уменьшить *custom.css* и объединить его с файлом *site.css* в файл *site.min.css*, добавьте относительный путь к файлу *bundleconfig.json*:</span><span class="sxs-lookup"><span data-stu-id="952af-214">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="952af-215">Кроме того, можно использовать следующий шаблон глобализации:</span><span class="sxs-lookup"><span data-stu-id="952af-215">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/!(*.min).css" ]
> ```
>
> <span data-ttu-id="952af-216">Этот шаблон глобализации сопоставляет все файлы CSS и исключает шаблон уменьшенного файла.</span><span class="sxs-lookup"><span data-stu-id="952af-216">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="952af-217">Постройте приложение.</span><span class="sxs-lookup"><span data-stu-id="952af-217">Build the application.</span></span> <span data-ttu-id="952af-218">Откройте *site.min.css* и обратите внимание, что в конец файла добавлено содержимое файла *custom.css*.</span><span class="sxs-lookup"><span data-stu-id="952af-218">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="952af-219">Объединение и минификация на основе среды</span><span class="sxs-lookup"><span data-stu-id="952af-219">Environment-based bundling and minification</span></span>

<span data-ttu-id="952af-220">Объединенные и минифицированные файлы приложения рекомендуется использовать в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="952af-220">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="952af-221">Во время разработки исходные файлы упрощают отладку приложения.</span><span class="sxs-lookup"><span data-stu-id="952af-221">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="952af-222">Укажите, какие файлы следует включить на страницы, используя в своих представлениях [вспомогательное приложение тегов среды](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="952af-222">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="952af-223">Вспомогательное приложение тегов среды отображает свое содержимое только при выполнении в определенных [средах](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="952af-223">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="952af-224">Следующий тег `environment` отображает необработанные файлы CSS при выполнении в среде `Development`:</span><span class="sxs-lookup"><span data-stu-id="952af-224">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

<span data-ttu-id="952af-225">Следующий тег `environment` отображает объединенные и минифицированные файлы CSS при выполнении в среде, отличной от `Development`.</span><span class="sxs-lookup"><span data-stu-id="952af-225">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="952af-226">Например, выполнение в `Production` или `Staging` запускает визуализацию этих таблиц стилей:</span><span class="sxs-lookup"><span data-stu-id="952af-226">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="952af-227">Использование bundleconfig.json из Gulp</span><span class="sxs-lookup"><span data-stu-id="952af-227">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="952af-228">В некоторых случаях для объединения и минификации рабочего процесса приложения требуется дополнительная обработка.</span><span class="sxs-lookup"><span data-stu-id="952af-228">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="952af-229">Примеры: оптимизация изображений, отключение кэша и обработка ресурсов CDN.</span><span class="sxs-lookup"><span data-stu-id="952af-229">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="952af-230">Для удовлетворения этих требований можно преобразовать рабочий процесс объединения и минификации для использования gulp.</span><span class="sxs-lookup"><span data-stu-id="952af-230">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="952af-231">Использование расширения Bundler & Minifier</span><span class="sxs-lookup"><span data-stu-id="952af-231">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="952af-232">Расширение [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) в Visual Studio выполняет преобразование в gulp.</span><span class="sxs-lookup"><span data-stu-id="952af-232">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="952af-233">Расширение Bundler & Minifier принадлежит к проекту сообщества в GitHub, в отношении которого корпорация Майкрософт не предоставляет поддержку.</span><span class="sxs-lookup"><span data-stu-id="952af-233">The Bundler & Minifier extension belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="952af-234">О проблемах следует сообщать [сюда](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="952af-234">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="952af-235">Щелкните правой кнопкой мыши файл *bundleconfig.json* в обозревателе решений и выберите **Bundler & Minifier** > **Convert To Gulp** (Преобразовать в Gulp):</span><span class="sxs-lookup"><span data-stu-id="952af-235">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Пункт контекстного меню Convert To Gulp (Преобразовать в Gulp)](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="952af-237">В проект добавляются файлы *gulpfile.js* и *package.json*.</span><span class="sxs-lookup"><span data-stu-id="952af-237">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="952af-238">Устанавливаются вспомогательные пакеты [npm](https://www.npmjs.com/), перечисленные в разделе `devDependencies` файла *package.json*.</span><span class="sxs-lookup"><span data-stu-id="952af-238">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="952af-239">Выполните следующую команду в окне PMC, чтобы установить CLI Gulp в качестве глобальной зависимости:</span><span class="sxs-lookup"><span data-stu-id="952af-239">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="952af-240">Файл *gulpfile.js* считывает файл *bundleconfig.json* для получения входных данных, выходных данных и параметров.</span><span class="sxs-lookup"><span data-stu-id="952af-240">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="952af-241">Преобразование вручную</span><span class="sxs-lookup"><span data-stu-id="952af-241">Convert manually</span></span>

<span data-ttu-id="952af-242">Если Visual Studio и (или) расширение Bundler & Minifier недоступны, выполните преобразование вручную.</span><span class="sxs-lookup"><span data-stu-id="952af-242">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="952af-243">Добавьте в корневой каталог проекта файл *package.json* со следующим `devDependencies`:</span><span class="sxs-lookup"><span data-stu-id="952af-243">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

> [!WARNING]
> <span data-ttu-id="952af-244">Модуль `gulp-uglify` не поддерживает ECMAScript (ES) 2015/ES6 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="952af-244">The `gulp-uglify` module doesn't support ECMAScript (ES) 2015 / ES6 and later.</span></span> <span data-ttu-id="952af-245">Вместо `gulp-uglify` установите [gulp-terser](https://www.npmjs.com/package/gulp-terser) для использования ES2015/ES6 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="952af-245">Install [gulp-terser](https://www.npmjs.com/package/gulp-terser) instead of `gulp-uglify` to use ES2015 / ES6 or later.</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="952af-246">Установите зависимости, выполнив следующую команду на том же уровне, что и файл *package.json*:</span><span class="sxs-lookup"><span data-stu-id="952af-246">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="952af-247">Установите CLI Gulp в качестве глобальной зависимости:</span><span class="sxs-lookup"><span data-stu-id="952af-247">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="952af-248">Скопируйте файл *gulpfile.js*, представленный ниже, в корневую папку проекта:</span><span class="sxs-lookup"><span data-stu-id="952af-248">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="952af-249">Выполнение задач Gulp</span><span class="sxs-lookup"><span data-stu-id="952af-249">Run Gulp tasks</span></span>

<span data-ttu-id="952af-250">Чтобы запустить задачу минификации Gulp перед построением проекта в Visual Studio, добавьте следующий [целевой объект MSBuild](/visualstudio/msbuild/msbuild-targets) в файл CSPROJ:</span><span class="sxs-lookup"><span data-stu-id="952af-250">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the \*.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="952af-251">В этом примере все задачи, определенные в целевом объекте `MyPreCompileTarget`, выполняются до предварительно заданного целевого объекта `Build`.</span><span class="sxs-lookup"><span data-stu-id="952af-251">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="952af-252">В окне выходных данных Visual Studio отображаются данные, аналогичные приведенным ниже:</span><span class="sxs-lookup"><span data-stu-id="952af-252">Output similar to the following appears in Visual Studio's Output window:</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
1>[14:17:49] Using gulpfile C:\BuildBundlerMinifierApp\gulpfile.js
1>[14:17:49] Starting 'min:js'...
1>[14:17:49] Starting 'min:css'...
1>[14:17:49] Starting 'min:html'...
1>[14:17:49] Finished 'min:js' after 83 ms
1>[14:17:49] Finished 'min:css' after 88 ms
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

## <a name="additional-resources"></a><span data-ttu-id="952af-253">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="952af-253">Additional resources</span></span>

* [<span data-ttu-id="952af-254">Использование Grunt</span><span class="sxs-lookup"><span data-stu-id="952af-254">Use Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="952af-255">Использование нескольких сред</span><span class="sxs-lookup"><span data-stu-id="952af-255">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="952af-256">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="952af-256">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
