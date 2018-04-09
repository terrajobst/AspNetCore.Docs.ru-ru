---
title: Пакет и minifiy статических активы ASP.NET Core
author: scottaddie
description: Сведения об оптимизации статические ресурсы в веб-приложение ASP.NET Core, применяя объединение и Минификация методы.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bundling-and-minification
ms.openlocfilehash: a155422c0fd638f46fe4a9d8a77faebc0b2a5681
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="bundle-and-minifiy-static-assets-in-aspnet-core"></a><span data-ttu-id="e14aa-103">Пакет и minifiy статических активы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e14aa-103">Bundle and minifiy static assets in ASP.NET Core</span></span>

<span data-ttu-id="e14aa-104">Автор: [Скотт Адди](https://twitter.com/Scott_Addie) (Scott Addie)</span><span class="sxs-lookup"><span data-stu-id="e14aa-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="e14aa-105">В этой статье объясняется выгода применения объединение и Минификация, включая использование этих функций с веб-приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e14aa-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="e14aa-106">Что такое объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="e14aa-106">What is bundling and minification?</span></span>

<span data-ttu-id="e14aa-107">Объединении и Минификация являются два способа оптимизации производительности для применения в веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="e14aa-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="e14aa-108">Использовать совместно, объединение и Минификация повышения производительности путем сокращения числа запросов к серверу и уменьшения размера запрошенных ресурсов статический.</span><span class="sxs-lookup"><span data-stu-id="e14aa-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="e14aa-109">Объединение и Минификация главным образом улучшить время загрузки первого запроса страницы.</span><span class="sxs-lookup"><span data-stu-id="e14aa-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="e14aa-110">После запросил веб-страницы браузер кэширует статические активы (JavaScript, CSS и изображения).</span><span class="sxs-lookup"><span data-stu-id="e14aa-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="e14aa-111">Следовательно объединение и Минификация не повысить производительность при запросе же страницу или страницы на одном сайте, запрашивает же активы.</span><span class="sxs-lookup"><span data-stu-id="e14aa-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="e14aa-112">Если срок действия истекает заголовка указано неверно по средствам и если объединение и Минификация не используется, эвристики актуальность браузера пометить активы устаревших через несколько дней.</span><span class="sxs-lookup"><span data-stu-id="e14aa-112">If the expires header isn't set correctly on the assets and if bundling and minification isn't used, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="e14aa-113">Кроме того браузеру запроса на проверку для каждого ресурса.</span><span class="sxs-lookup"><span data-stu-id="e14aa-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="e14aa-114">В этом случае объединение и Минификация обеспечивают более высокую производительность даже после первого запроса страницы.</span><span class="sxs-lookup"><span data-stu-id="e14aa-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="e14aa-115">Объединение</span><span class="sxs-lookup"><span data-stu-id="e14aa-115">Bundling</span></span>

<span data-ttu-id="e14aa-116">Объединение объединяет несколько файлов в один файл.</span><span class="sxs-lookup"><span data-stu-id="e14aa-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="e14aa-117">Объединение уменьшает количество запросов сервера, необходимых для отрисовки на web средства, такие как веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="e14aa-117">Bundling reduces the number of server requests which are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="e14aa-118">Можно создать любое количество отдельных наборов специально для CSS, JavaScript и т. д. Меньшее число файлов означает меньшее количество HTTP-запросов из браузера на сервер или службой, предоставляющей приложение.</span><span class="sxs-lookup"><span data-stu-id="e14aa-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="e14aa-119">Результатом является повышение производительности загрузки первой страницы.</span><span class="sxs-lookup"><span data-stu-id="e14aa-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="e14aa-120">Минификации</span><span class="sxs-lookup"><span data-stu-id="e14aa-120">Minification</span></span>

<span data-ttu-id="e14aa-121">Минификации удаляет ненужные символы из кода без изменения функциональности.</span><span class="sxs-lookup"><span data-stu-id="e14aa-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="e14aa-122">Результатом является уменьшение значительного размера запрошенных ресурсов (например, CSS, изображения и файлы JavaScript).</span><span class="sxs-lookup"><span data-stu-id="e14aa-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="e14aa-123">Распространенные побочные эффекты минификации включают сокращение имена переменных в один символ и удаление комментариев и ненужные пробелы.</span><span class="sxs-lookup"><span data-stu-id="e14aa-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="e14aa-124">Рассмотрим следующую функцию JavaScript:</span><span class="sxs-lookup"><span data-stu-id="e14aa-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="e14aa-125">Иными уменьшает функция следующее:</span><span class="sxs-lookup"><span data-stu-id="e14aa-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="e14aa-126">Помимо удаление комментариев и ненужные пробелы, следующие имена параметра и переменной изменены следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e14aa-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="e14aa-127">До преобразования</span><span class="sxs-lookup"><span data-stu-id="e14aa-127">Original</span></span> | <span data-ttu-id="e14aa-128">Переименовано</span><span class="sxs-lookup"><span data-stu-id="e14aa-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="e14aa-129">Влияние объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="e14aa-129">Impact of bundling and minification</span></span>

<span data-ttu-id="e14aa-130">В следующей таблице приведены различия между по отдельности загрузка средств и использование объединение и Минификация:</span><span class="sxs-lookup"><span data-stu-id="e14aa-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="e14aa-131">Действие</span><span class="sxs-lookup"><span data-stu-id="e14aa-131">Action</span></span> | <span data-ttu-id="e14aa-132">С B и M</span><span class="sxs-lookup"><span data-stu-id="e14aa-132">With B/M</span></span> | <span data-ttu-id="e14aa-133">Без B и M</span><span class="sxs-lookup"><span data-stu-id="e14aa-133">Without B/M</span></span> | <span data-ttu-id="e14aa-134">Изменение</span><span class="sxs-lookup"><span data-stu-id="e14aa-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="e14aa-135">Запросы файлов</span><span class="sxs-lookup"><span data-stu-id="e14aa-135">File Requests</span></span>  | <span data-ttu-id="e14aa-136">7</span><span class="sxs-lookup"><span data-stu-id="e14aa-136">7</span></span>   | <span data-ttu-id="e14aa-137">18</span><span class="sxs-lookup"><span data-stu-id="e14aa-137">18</span></span>     | <span data-ttu-id="e14aa-138">157%</span><span class="sxs-lookup"><span data-stu-id="e14aa-138">157%</span></span>
<span data-ttu-id="e14aa-139">Передать КБ</span><span class="sxs-lookup"><span data-stu-id="e14aa-139">KB Transferred</span></span> | <span data-ttu-id="e14aa-140">156</span><span class="sxs-lookup"><span data-stu-id="e14aa-140">156</span></span> | <span data-ttu-id="e14aa-141">264.68</span><span class="sxs-lookup"><span data-stu-id="e14aa-141">264.68</span></span> | <span data-ttu-id="e14aa-142">70%</span><span class="sxs-lookup"><span data-stu-id="e14aa-142">70%</span></span>
<span data-ttu-id="e14aa-143">Время загрузки (мс)</span><span class="sxs-lookup"><span data-stu-id="e14aa-143">Load Time (ms)</span></span> | <span data-ttu-id="e14aa-144">885</span><span class="sxs-lookup"><span data-stu-id="e14aa-144">885</span></span> | <span data-ttu-id="e14aa-145">2360</span><span class="sxs-lookup"><span data-stu-id="e14aa-145">2360</span></span>   | <span data-ttu-id="e14aa-146">167%</span><span class="sxs-lookup"><span data-stu-id="e14aa-146">167%</span></span>

<span data-ttu-id="e14aa-147">Обозреватели являются достаточно подробных сведений по отношению к заголовков HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="e14aa-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="e14aa-148">Всего байт отправлено метрика видели значительное сокращение при объединении.</span><span class="sxs-lookup"><span data-stu-id="e14aa-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="e14aa-149">Время загрузки показывает значительные улучшения, однако в этом примере запускался локально.</span><span class="sxs-lookup"><span data-stu-id="e14aa-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="e14aa-150">Больше повысить производительность реализуются при помощи активы объединение и Минификация передаваемых по сети.</span><span class="sxs-lookup"><span data-stu-id="e14aa-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="e14aa-151">Выбор стратегии объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="e14aa-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="e14aa-152">Шаблоны проектов MVC и страниц Razor и предоставления решения out of box для объединение и Минификация, состоящий из файла конфигурации JSON.</span><span class="sxs-lookup"><span data-stu-id="e14aa-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="e14aa-153">Сторонние средства, такие как [Gulp](xref:client-side/using-gulp) и [Grunt](xref:client-side/using-grunt) задачи средства запуска, выполнения тех же задач с немного сложнее.</span><span class="sxs-lookup"><span data-stu-id="e14aa-153">Third-party tools, such as the [Gulp](xref:client-side/using-gulp) and [Grunt](xref:client-side/using-grunt) task runners, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="e14aa-154">Средства стороннего является отлично подходит при разработке рабочего процесса требуется обработка за объединение и Минификация&mdash;например оптимизация linting и изображения.</span><span class="sxs-lookup"><span data-stu-id="e14aa-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="e14aa-155">С помощью разработки объединение и Минификация, уменьшенный файлы создаются до развертывания приложения.</span><span class="sxs-lookup"><span data-stu-id="e14aa-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="e14aa-156">Объединение и Минификация перед развертыванием предоставляет преимущество снижение нагрузки.</span><span class="sxs-lookup"><span data-stu-id="e14aa-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="e14aa-157">Однако следует отметить, что объединение во время разработки и иными увеличивает сложность сборки и работает только с статических файлов.</span><span class="sxs-lookup"><span data-stu-id="e14aa-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="e14aa-158">Настроить объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="e14aa-158">Configure bundling and minification</span></span>

<span data-ttu-id="e14aa-159">Шаблоны проектов MVC и страниц Razor содержат *bundleconfig.json* файл конфигурации, который определяет параметры для каждого пакета.</span><span class="sxs-lookup"><span data-stu-id="e14aa-159">The MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file which defines the options for each bundle.</span></span> <span data-ttu-id="e14aa-160">По умолчанию, определенное конфигурации один комплект для пользовательским кодом JavaScript (*wwwroot/js/site.js*) и таблицы стилей (*wwwroot/css/site.css*) файлов:</span><span class="sxs-lookup"><span data-stu-id="e14aa-160">By default, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="e14aa-161">Следующие параметры конфигурации.</span><span class="sxs-lookup"><span data-stu-id="e14aa-161">Configuration options include:</span></span>

* <span data-ttu-id="e14aa-162">`outputFileName`: Имя файла набора для вывода.</span><span class="sxs-lookup"><span data-stu-id="e14aa-162">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="e14aa-163">Может содержать относительный путь от *bundleconfig.json* файла.</span><span class="sxs-lookup"><span data-stu-id="e14aa-163">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="e14aa-164">**Обязательно**</span><span class="sxs-lookup"><span data-stu-id="e14aa-164">**required**</span></span>
* <span data-ttu-id="e14aa-165">`inputFiles`: Массив файлов, которые будут объединены.</span><span class="sxs-lookup"><span data-stu-id="e14aa-165">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="e14aa-166">Это относительные пути к файлу конфигурации.</span><span class="sxs-lookup"><span data-stu-id="e14aa-166">These are relative paths to the configuration file.</span></span> <span data-ttu-id="e14aa-167">**Необязательный**, \* пустое значение преобразуется в пустой выходной файл.</span><span class="sxs-lookup"><span data-stu-id="e14aa-167">**optional**, \*an empty value results in an empty output file.</span></span> <span data-ttu-id="e14aa-168">[Этот режим](http://www.tldp.org/LDP/abs/html/globbingref.html) поддерживаются шаблоны.</span><span class="sxs-lookup"><span data-stu-id="e14aa-168">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="e14aa-169">`minify`: Параметры минификации тип выходных данных.</span><span class="sxs-lookup"><span data-stu-id="e14aa-169">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="e14aa-170">**Необязательный**, *по умолчанию — `minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="e14aa-170">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="e14aa-171">Параметры конфигурации доступны на тип выходного файла.</span><span class="sxs-lookup"><span data-stu-id="e14aa-171">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="e14aa-172">Уменьшитель CSS</span><span class="sxs-lookup"><span data-stu-id="e14aa-172">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="e14aa-173">Уменьшитель JavaScript</span><span class="sxs-lookup"><span data-stu-id="e14aa-173">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="e14aa-174">Уменьшитель HTML</span><span class="sxs-lookup"><span data-stu-id="e14aa-174">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="e14aa-175">`includeInProject`: Флаг, указывающий, добавьте в файл проекта созданные файлы.</span><span class="sxs-lookup"><span data-stu-id="e14aa-175">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="e14aa-176">**Необязательный**, *по умолчанию - false*</span><span class="sxs-lookup"><span data-stu-id="e14aa-176">**optional**, *default - false*</span></span>
* <span data-ttu-id="e14aa-177">`sourceMap`: Флаг, указывающий, сформируйте карту источника для объединенный файл.</span><span class="sxs-lookup"><span data-stu-id="e14aa-177">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="e14aa-178">**Необязательный**, *по умолчанию - false*</span><span class="sxs-lookup"><span data-stu-id="e14aa-178">**optional**, *default - false*</span></span>
* <span data-ttu-id="e14aa-179">`sourceMapRootPath`: Корневой путь для сохранения карты сформированном исходном файле.</span><span class="sxs-lookup"><span data-stu-id="e14aa-179">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="e14aa-180">Объединение и Минификация выполнения во время сборки</span><span class="sxs-lookup"><span data-stu-id="e14aa-180">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="e14aa-181">[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) пакет NuGet разрешает выполнение объединение и Минификация во время построения.</span><span class="sxs-lookup"><span data-stu-id="e14aa-181">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="e14aa-182">Внедряет пакет [целевые объекты MSBuild](/visualstudio/msbuild/msbuild-targets) которого сборки и очистить времени выполнения.</span><span class="sxs-lookup"><span data-stu-id="e14aa-182">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="e14aa-183">*Bundleconfig.json* файл анализируется в процессе построения для создания выходных файлов, на основе определенной конфигурации.</span><span class="sxs-lookup"><span data-stu-id="e14aa-183">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="e14aa-184">BuildBundlerMinifier относится к проекту сообщество ведет на GitHub, для которых корпорация Майкрософт не поддерживает.</span><span class="sxs-lookup"><span data-stu-id="e14aa-184">BuildBundlerMinifier belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="e14aa-185">Должна быть заполнена проблемы [здесь](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="e14aa-185">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e14aa-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e14aa-186">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="e14aa-187">Добавить *BuildBundlerMinifier* пакета в проект.</span><span class="sxs-lookup"><span data-stu-id="e14aa-187">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="e14aa-188">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="e14aa-188">Build the project.</span></span> <span data-ttu-id="e14aa-189">В окне «Вывод» отображаются следующие сведения.</span><span class="sxs-lookup"><span data-stu-id="e14aa-189">The following appears in the Output window:</span></span>

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

<span data-ttu-id="e14aa-190">Очистите проект.</span><span class="sxs-lookup"><span data-stu-id="e14aa-190">Clean the project.</span></span> <span data-ttu-id="e14aa-191">В окне «Вывод» отображаются следующие сведения.</span><span class="sxs-lookup"><span data-stu-id="e14aa-191">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e14aa-192">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="e14aa-192">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="e14aa-193">Добавить *BuildBundlerMinifier* пакета в проект:</span><span class="sxs-lookup"><span data-stu-id="e14aa-193">Add the *BuildBundlerMinifier* package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="e14aa-194">При использовании ASP.NET Core 1.x, восстановите добавленные пакеты:</span><span class="sxs-lookup"><span data-stu-id="e14aa-194">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```console
dotnet restore
```

<span data-ttu-id="e14aa-195">Постройте проект:</span><span class="sxs-lookup"><span data-stu-id="e14aa-195">Build the project:</span></span>

```console
dotnet build
```

<span data-ttu-id="e14aa-196">Отображаются следующие сведения.</span><span class="sxs-lookup"><span data-stu-id="e14aa-196">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="e14aa-197">Очистите проект:</span><span class="sxs-lookup"><span data-stu-id="e14aa-197">Clean the project:</span></span>

```console
dotnet clean
```

<span data-ttu-id="e14aa-198">Появляется следующий результат:</span><span class="sxs-lookup"><span data-stu-id="e14aa-198">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="e14aa-199">Выполнение нерегламентированных объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="e14aa-199">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="e14aa-200">Это можно выполнить задачи объединение и Минификация нерегламентированные, без построения проекта.</span><span class="sxs-lookup"><span data-stu-id="e14aa-200">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="e14aa-201">Добавить [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) пакет NuGet для проекта:</span><span class="sxs-lookup"><span data-stu-id="e14aa-201">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> <span data-ttu-id="e14aa-202">BundlerMinifier.Core относится к проекту сообщество ведет на GitHub, для которых корпорация Майкрософт не поддерживает.</span><span class="sxs-lookup"><span data-stu-id="e14aa-202">BundlerMinifier.Core belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="e14aa-203">Должна быть заполнена проблемы [здесь](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="e14aa-203">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="e14aa-204">Этот пакет расширяет .NET Core CLI для включения *dotnet пакета* средства.</span><span class="sxs-lookup"><span data-stu-id="e14aa-204">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="e14aa-205">В окне консоли диспетчера пакетов (PMC) или в командной оболочке можно выполнить следующую команду:</span><span class="sxs-lookup"><span data-stu-id="e14aa-205">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```console
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="e14aa-206">Диспетчер пакетов NuGet добавляет зависимости файл \*.csproj в качестве `<PackageReference />` узлов.</span><span class="sxs-lookup"><span data-stu-id="e14aa-206">NuGet Package Manager adds dependencies to the \*.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="e14aa-207">`dotnet bundle` Команда зарегистрирован .NET Core CLI только если `<DotNetCliToolReference />` используется узел.</span><span class="sxs-lookup"><span data-stu-id="e14aa-207">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="e14aa-208">Измените файл \*.csproj соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="e14aa-208">Modify the \*.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="e14aa-209">Добавление файлов в рабочий процесс</span><span class="sxs-lookup"><span data-stu-id="e14aa-209">Add files to workflow</span></span>

<span data-ttu-id="e14aa-210">Рассмотрим пример, в котором еще *custom.css* будет добавлен файл, аналогичный следующему:</span><span class="sxs-lookup"><span data-stu-id="e14aa-210">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="e14aa-211">Для уменьшения *custom.css* и объединить его с *site.css* в *site.min.css* файл, добавьте относительный путь к *bundleconfig.json*:</span><span class="sxs-lookup"><span data-stu-id="e14aa-211">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="e14aa-212">Кроме того может использоваться следующий шаблон этот режим:</span><span class="sxs-lookup"><span data-stu-id="e14aa-212">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> <span data-ttu-id="e14aa-213">Этот шаблон глобализацию поиск всех файлов CSS и исключает уменьшенный файла шаблона.</span><span class="sxs-lookup"><span data-stu-id="e14aa-213">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="e14aa-214">Постройте приложение.</span><span class="sxs-lookup"><span data-stu-id="e14aa-214">Build the application.</span></span> <span data-ttu-id="e14aa-215">Откройте *site.min.css* и обратите внимание, содержимое *custom.css* добавляется в конец файла.</span><span class="sxs-lookup"><span data-stu-id="e14aa-215">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="e14aa-216">На основе среды объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="e14aa-216">Environment-based bundling and minification</span></span>

<span data-ttu-id="e14aa-217">Рекомендуется распространение и уменьшенный файлы приложения следует использовать в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="e14aa-217">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="e14aa-218">Во время разработки исходные файлы убедитесь, чтобы упростить отладку приложения.</span><span class="sxs-lookup"><span data-stu-id="e14aa-218">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="e14aa-219">Указать, какие файлы для включения в страницы с помощью [вспомогательный тега среды](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) в представления.</span><span class="sxs-lookup"><span data-stu-id="e14aa-219">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="e14aa-220">Вспомогательный тега среды Подготовка содержимого только при работе в конкретных [средах](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="e14aa-220">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="e14aa-221">Следующие `environment` тег отображает необработанных файлов CSS при работе в `Development` среде:</span><span class="sxs-lookup"><span data-stu-id="e14aa-221">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e14aa-222">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e14aa-222">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e14aa-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e14aa-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

* * *
<span data-ttu-id="e14aa-224">Следующие `environment` тег отображает распространение и уменьшенный CSS-файл при выполнении в среде, отличный от `Development`.</span><span class="sxs-lookup"><span data-stu-id="e14aa-224">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="e14aa-225">Например, на котором работают `Production` или `Staging` триггеры для подготовки к просмотру эти таблицы стилей:</span><span class="sxs-lookup"><span data-stu-id="e14aa-225">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e14aa-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e14aa-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e14aa-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e14aa-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

* * *
## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="e14aa-228">Использовать bundleconfig.json из Gulp</span><span class="sxs-lookup"><span data-stu-id="e14aa-228">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="e14aa-229">Существуют случаи, в которых приложение объединение и Минификация рабочего процесса требуется дополнительная обработка.</span><span class="sxs-lookup"><span data-stu-id="e14aa-229">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="e14aa-230">Примеры Оптимизация изображения, busting кэша и обработка активов CDN.</span><span class="sxs-lookup"><span data-stu-id="e14aa-230">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="e14aa-231">Чтобы удовлетворить этим требованиям, можно преобразовать объединение и Минификация рабочий процесс для использования Gulp.</span><span class="sxs-lookup"><span data-stu-id="e14aa-231">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="e14aa-232">Использовать расширение Bundler & Уменьшитель</span><span class="sxs-lookup"><span data-stu-id="e14aa-232">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="e14aa-233">Visual Studio [Bundler & Уменьшитель](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) расширения выполняет преобразование на Gulp.</span><span class="sxs-lookup"><span data-stu-id="e14aa-233">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="e14aa-234">Расширение Bundler & Уменьшитель принадлежит сообщество ведет проекта на GitHub, для которых корпорация Майкрософт не поддерживает.</span><span class="sxs-lookup"><span data-stu-id="e14aa-234">The Bundler & Minifier extension belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="e14aa-235">Должна быть заполнена проблемы [здесь](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="e14aa-235">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="e14aa-236">Щелкните правой кнопкой мыши *bundleconfig.json* в обозревателе решений и выберите **Bundler & Уменьшитель** > **преобразовать Gulp...** :</span><span class="sxs-lookup"><span data-stu-id="e14aa-236">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Преобразовать Gulp пункт контекстного меню](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="e14aa-238">*Gulpfile.js* и *package.json* файлы добавляются в проект.</span><span class="sxs-lookup"><span data-stu-id="e14aa-238">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="e14aa-239">Поддержка [npm](https://www.npmjs.com/) пакеты, перечисленные в *package.json* файла `devDependencies` устанавливаются раздела.</span><span class="sxs-lookup"><span data-stu-id="e14aa-239">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="e14aa-240">Выполните следующую команду в окне PMC установить Gulp CLI в качестве глобального зависимостей:</span><span class="sxs-lookup"><span data-stu-id="e14aa-240">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="e14aa-241">*Gulpfile.js* операции чтения файла *bundleconfig.json* файл для входов, выходов и параметры.</span><span class="sxs-lookup"><span data-stu-id="e14aa-241">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="e14aa-242">Преобразовать вручную</span><span class="sxs-lookup"><span data-stu-id="e14aa-242">Convert manually</span></span>

<span data-ttu-id="e14aa-243">Если Visual Studio или расширение Bundler & Уменьшитель недоступны, преобразуйте вручную.</span><span class="sxs-lookup"><span data-stu-id="e14aa-243">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="e14aa-244">Добавить *package.json* файл со следующими `devDependencies`, в корне проекта:</span><span class="sxs-lookup"><span data-stu-id="e14aa-244">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="e14aa-245">Установить зависимости, выполнив следующую команду на том же уровне, как *package.json*:</span><span class="sxs-lookup"><span data-stu-id="e14aa-245">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="e14aa-246">Установите Gulp CLI как глобальные зависимостей:</span><span class="sxs-lookup"><span data-stu-id="e14aa-246">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="e14aa-247">Копировать *gulpfile.js* файла ниже в корне проекта:</span><span class="sxs-lookup"><span data-stu-id="e14aa-247">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="e14aa-248">Выполнение задач Gulp</span><span class="sxs-lookup"><span data-stu-id="e14aa-248">Run Gulp tasks</span></span>

<span data-ttu-id="e14aa-249">Для запуска задачи минификации Gulp до сборки проекта в Visual Studio, добавьте следующий [MSBuild: целевая](/visualstudio/msbuild/msbuild-targets) \*.csproj файл:</span><span class="sxs-lookup"><span data-stu-id="e14aa-249">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the \*.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="e14aa-250">В этом примере все задачи, определенные в `MyPreCompileTarget` целевые запуска перед стандартных `Build` цели.</span><span class="sxs-lookup"><span data-stu-id="e14aa-250">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="e14aa-251">В окне вывода Visual Studio появляется результат, аналогичный приведенному ниже:</span><span class="sxs-lookup"><span data-stu-id="e14aa-251">Output similar to the following appears in Visual Studio's Output window:</span></span>

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

<span data-ttu-id="e14aa-252">Кроме того Visual Studio Task Runner Explorer может использоваться для связи Gulp задач для конкретных событий в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e14aa-252">Alternatively, Visual Studio's Task Runner Explorer may be used to bind Gulp tasks to specific Visual Studio events.</span></span> <span data-ttu-id="e14aa-253">В разделе [выполнения задач по умолчанию](xref:client-side/using-gulp#running-default-tasks) инструкции по этим способом.</span><span class="sxs-lookup"><span data-stu-id="e14aa-253">See [Running default tasks](xref:client-side/using-gulp#running-default-tasks) for instructions on doing that.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e14aa-254">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e14aa-254">Additional resources</span></span>

* [<span data-ttu-id="e14aa-255">Использование Gulp</span><span class="sxs-lookup"><span data-stu-id="e14aa-255">Use Gulp</span></span>](xref:client-side/using-gulp)
* [<span data-ttu-id="e14aa-256">Использование Grunt</span><span class="sxs-lookup"><span data-stu-id="e14aa-256">Use Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="e14aa-257">Работать с несколькими средами</span><span class="sxs-lookup"><span data-stu-id="e14aa-257">Work with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="e14aa-258">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="e14aa-258">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
