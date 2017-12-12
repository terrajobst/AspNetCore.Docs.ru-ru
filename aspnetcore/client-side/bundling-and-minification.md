---
title: "Объединение и Минификация в ASP.NET Core"
author: scottaddie
description: "Сведения об оптимизации статические ресурсы в веб-приложение ASP.NET Core, применяя объединение и Минификация методы."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/01/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bundling-and-minification
ms.openlocfilehash: c271b7ef386bacedbd45fbe9f62c9c486db55b36
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/05/2017
---
# <a name="bundling-and-minification"></a><span data-ttu-id="0a988-103">Объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="0a988-103">Bundling and minification</span></span>

<span data-ttu-id="0a988-104">Автор: [Скотт Адди](https://twitter.com/Scott_Addie) (Scott Addie)</span><span class="sxs-lookup"><span data-stu-id="0a988-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="0a988-105">В этой статье объясняется выгода применения объединение и Минификация, включая использование этих функций с веб-приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0a988-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="0a988-106">Что такое объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="0a988-106">What is bundling and minification?</span></span>

<span data-ttu-id="0a988-107">Объединении и Минификация являются два способа оптимизации производительности для применения в веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="0a988-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="0a988-108">Использовать совместно, объединение и Минификация повышения производительности путем сокращения числа запросов к серверу и уменьшения размера запрошенных ресурсов статический.</span><span class="sxs-lookup"><span data-stu-id="0a988-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="0a988-109">Объединение и Минификация главным образом улучшить время загрузки первого запроса страницы.</span><span class="sxs-lookup"><span data-stu-id="0a988-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="0a988-110">После запросил веб-страницы браузер кэширует статические активы (JavaScript, CSS и изображения).</span><span class="sxs-lookup"><span data-stu-id="0a988-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="0a988-111">Следовательно объединение и Минификация не повысить производительность при запросе же страницу или страницы на одном сайте, запрашивает же активы.</span><span class="sxs-lookup"><span data-stu-id="0a988-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="0a988-112">Если вы не задали заголовок правильно на средствах истечения срока действия, и если вы не используете объединение и Минификация, эвристики актуальность браузера пометить активы устаревших через несколько дней.</span><span class="sxs-lookup"><span data-stu-id="0a988-112">If you don't set the expires header correctly on your assets, and if you don’t use bundling and minification, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="0a988-113">Кроме того браузеру запроса на проверку для каждого ресурса.</span><span class="sxs-lookup"><span data-stu-id="0a988-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="0a988-114">В этом случае объединение и Минификация обеспечивают более высокую производительность даже после первого запроса страницы.</span><span class="sxs-lookup"><span data-stu-id="0a988-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="0a988-115">Объединение</span><span class="sxs-lookup"><span data-stu-id="0a988-115">Bundling</span></span>

<span data-ttu-id="0a988-116">Объединение объединяет несколько файлов в один файл.</span><span class="sxs-lookup"><span data-stu-id="0a988-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="0a988-117">Объединение уменьшает количество запросов сервера, необходимых для отрисовки на web средства, такие как веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="0a988-117">Bundling reduces the number of server requests which are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="0a988-118">Можно создать любое количество отдельных наборов специально для CSS, JavaScript и т. д. Меньшее число файлов означает меньшее количество HTTP-запросов из браузера на сервер или службой, предоставляющей приложение.</span><span class="sxs-lookup"><span data-stu-id="0a988-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="0a988-119">Результатом является повышение производительности загрузки первой страницы.</span><span class="sxs-lookup"><span data-stu-id="0a988-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="0a988-120">Минификации</span><span class="sxs-lookup"><span data-stu-id="0a988-120">Minification</span></span>

<span data-ttu-id="0a988-121">Минификации удаляет ненужные символы из кода без изменения функциональности.</span><span class="sxs-lookup"><span data-stu-id="0a988-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="0a988-122">Результатом является уменьшение значительного размера запрошенных ресурсов (например, CSS, изображения и файлы JavaScript).</span><span class="sxs-lookup"><span data-stu-id="0a988-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="0a988-123">Распространенные побочные эффекты минификации включают сокращение имена переменных в один символ и удаление комментариев и ненужные пробелы.</span><span class="sxs-lookup"><span data-stu-id="0a988-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="0a988-124">Рассмотрим следующую функцию JavaScript:</span><span class="sxs-lookup"><span data-stu-id="0a988-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="0a988-125">Иными уменьшает функция следующее:</span><span class="sxs-lookup"><span data-stu-id="0a988-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="0a988-126">Помимо удаление комментариев и ненужные пробелы, следующие имена параметра и переменной изменены следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0a988-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="0a988-127">До преобразования</span><span class="sxs-lookup"><span data-stu-id="0a988-127">Original</span></span> | <span data-ttu-id="0a988-128">Переименовано</span><span class="sxs-lookup"><span data-stu-id="0a988-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="0a988-129">Влияние объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="0a988-129">Impact of bundling and minification</span></span>

<span data-ttu-id="0a988-130">В следующей таблице приведены различия между по отдельности загрузка средств и использование объединение и Минификация:</span><span class="sxs-lookup"><span data-stu-id="0a988-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="0a988-131">Действие</span><span class="sxs-lookup"><span data-stu-id="0a988-131">Action</span></span> | <span data-ttu-id="0a988-132">С B и M</span><span class="sxs-lookup"><span data-stu-id="0a988-132">With B/M</span></span> | <span data-ttu-id="0a988-133">Без B и M</span><span class="sxs-lookup"><span data-stu-id="0a988-133">Without B/M</span></span> | <span data-ttu-id="0a988-134">Изменение</span><span class="sxs-lookup"><span data-stu-id="0a988-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="0a988-135">Запросы файлов</span><span class="sxs-lookup"><span data-stu-id="0a988-135">File Requests</span></span>  | <span data-ttu-id="0a988-136">7</span><span class="sxs-lookup"><span data-stu-id="0a988-136">7</span></span>   | <span data-ttu-id="0a988-137">18</span><span class="sxs-lookup"><span data-stu-id="0a988-137">18</span></span>     | <span data-ttu-id="0a988-138">157%</span><span class="sxs-lookup"><span data-stu-id="0a988-138">157%</span></span>
<span data-ttu-id="0a988-139">Передать КБ</span><span class="sxs-lookup"><span data-stu-id="0a988-139">KB Transferred</span></span> | <span data-ttu-id="0a988-140">156</span><span class="sxs-lookup"><span data-stu-id="0a988-140">156</span></span> | <span data-ttu-id="0a988-141">264.68</span><span class="sxs-lookup"><span data-stu-id="0a988-141">264.68</span></span> | <span data-ttu-id="0a988-142">70%</span><span class="sxs-lookup"><span data-stu-id="0a988-142">70%</span></span>
<span data-ttu-id="0a988-143">Время загрузки (мс)</span><span class="sxs-lookup"><span data-stu-id="0a988-143">Load Time (ms)</span></span> | <span data-ttu-id="0a988-144">885</span><span class="sxs-lookup"><span data-stu-id="0a988-144">885</span></span> | <span data-ttu-id="0a988-145">2360</span><span class="sxs-lookup"><span data-stu-id="0a988-145">2360</span></span>   | <span data-ttu-id="0a988-146">167%</span><span class="sxs-lookup"><span data-stu-id="0a988-146">167%</span></span>

<span data-ttu-id="0a988-147">Обозреватели являются достаточно подробных сведений по отношению к заголовков HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="0a988-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="0a988-148">Всего байт отправлено метрика видели значительное сокращение при объединении.</span><span class="sxs-lookup"><span data-stu-id="0a988-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="0a988-149">Время загрузки показывает значительные улучшения, однако в этом примере запускался локально.</span><span class="sxs-lookup"><span data-stu-id="0a988-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="0a988-150">Больше повысить производительность реализуются при помощи активы объединение и Минификация передаваемых по сети.</span><span class="sxs-lookup"><span data-stu-id="0a988-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="0a988-151">Выбор стратегии объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="0a988-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="0a988-152">Шаблоны проектов MVC и страниц Razor и предоставления решения out of box для объединение и Минификация, состоящий из файла конфигурации JSON.</span><span class="sxs-lookup"><span data-stu-id="0a988-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="0a988-153">Сторонние средства, такие как [Gulp](xref:client-side/using-gulp) и [Grunt](xref:client-side/using-grunt) задачи средства запуска, выполнения тех же задач с немного сложнее.</span><span class="sxs-lookup"><span data-stu-id="0a988-153">Third-party tools, such as the [Gulp](xref:client-side/using-gulp) and [Grunt](xref:client-side/using-grunt) task runners, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="0a988-154">Средства стороннего является отлично подходит при разработке рабочего процесса требуется обработка за объединение и Минификация&mdash;например оптимизация linting и изображения.</span><span class="sxs-lookup"><span data-stu-id="0a988-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="0a988-155">С помощью разработки объединение и Минификация, уменьшенный файлы создаются до развертывания приложения.</span><span class="sxs-lookup"><span data-stu-id="0a988-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="0a988-156">Объединение и Минификация перед развертыванием предоставляет преимущество снижение нагрузки.</span><span class="sxs-lookup"><span data-stu-id="0a988-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="0a988-157">Однако следует отметить, что объединение во время разработки и иными увеличивает сложность сборки и работает только с статических файлов.</span><span class="sxs-lookup"><span data-stu-id="0a988-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="0a988-158">Настроить объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="0a988-158">Configure bundling and minification</span></span>

<span data-ttu-id="0a988-159">Шаблоны проектов MVC и страниц Razor содержат *bundleconfig.json* файл конфигурации, который определяет параметры для каждого пакета.</span><span class="sxs-lookup"><span data-stu-id="0a988-159">The MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file which defines the options for each bundle.</span></span> <span data-ttu-id="0a988-160">По умолчанию, определенное конфигурации один комплект для пользовательским кодом JavaScript (*wwwroot/js/site.js*) и таблицы стилей (*wwwroot/css/site.css*) файлов:</span><span class="sxs-lookup"><span data-stu-id="0a988-160">By default, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="0a988-161">Параметры пакета:</span><span class="sxs-lookup"><span data-stu-id="0a988-161">Bundle options include:</span></span>

* <span data-ttu-id="0a988-162">`outputFileName`: Имя файла набора для вывода.</span><span class="sxs-lookup"><span data-stu-id="0a988-162">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="0a988-163">Может содержать относительный путь от *bundleconfig.json* файла.</span><span class="sxs-lookup"><span data-stu-id="0a988-163">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="0a988-164">**Обязательно**</span><span class="sxs-lookup"><span data-stu-id="0a988-164">**required**</span></span>
* <span data-ttu-id="0a988-165">`inputFiles`: Массив файлов, которые будут объединены.</span><span class="sxs-lookup"><span data-stu-id="0a988-165">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="0a988-166">Это относительные пути к файлу конфигурации.</span><span class="sxs-lookup"><span data-stu-id="0a988-166">These are relative paths to the configuration file.</span></span> <span data-ttu-id="0a988-167">**Необязательный**, * пустое значение преобразуется в пустой выходной файл.</span><span class="sxs-lookup"><span data-stu-id="0a988-167">**optional**, *an empty value results in an empty output file.</span></span> <span data-ttu-id="0a988-168">[Этот режим](http://www.tldp.org/LDP/abs/html/globbingref.html) поддерживаются шаблоны.</span><span class="sxs-lookup"><span data-stu-id="0a988-168">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="0a988-169">`minify`: Параметры минификации тип выходных данных.</span><span class="sxs-lookup"><span data-stu-id="0a988-169">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="0a988-170">**Необязательный**, *по умолчанию —`minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="0a988-170">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="0a988-171">Параметры конфигурации доступны на тип выходного файла.</span><span class="sxs-lookup"><span data-stu-id="0a988-171">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="0a988-172">Уменьшитель CSS</span><span class="sxs-lookup"><span data-stu-id="0a988-172">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="0a988-173">Уменьшитель JavaScript</span><span class="sxs-lookup"><span data-stu-id="0a988-173">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="0a988-174">Уменьшитель HTML</span><span class="sxs-lookup"><span data-stu-id="0a988-174">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="0a988-175">`includeInProject`: Флаг, указывающий, добавьте в файл проекта созданные файлы.</span><span class="sxs-lookup"><span data-stu-id="0a988-175">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="0a988-176">**Необязательный**, *по умолчанию - false*</span><span class="sxs-lookup"><span data-stu-id="0a988-176">**optional**, *default - false*</span></span>
* <span data-ttu-id="0a988-177">`sourceMap`: Флаг, указывающий, сформируйте карту источника для объединенный файл.</span><span class="sxs-lookup"><span data-stu-id="0a988-177">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="0a988-178">**Необязательный**, *по умолчанию - false*</span><span class="sxs-lookup"><span data-stu-id="0a988-178">**optional**, *default - false*</span></span>
* <span data-ttu-id="0a988-179">`sourceMapRootPath`: Корневой путь для сохранения карты сформированном исходном файле.</span><span class="sxs-lookup"><span data-stu-id="0a988-179">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="0a988-180">Объединение и Минификация выполнения во время сборки</span><span class="sxs-lookup"><span data-stu-id="0a988-180">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="0a988-181">[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) пакет NuGet разрешает выполнение объединение и Минификация во время построения.</span><span class="sxs-lookup"><span data-stu-id="0a988-181">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="0a988-182">Внедряет пакет [целевые объекты MSBuild](/visualstudio/msbuild/msbuild-targets) которого сборки и очистить времени выполнения.</span><span class="sxs-lookup"><span data-stu-id="0a988-182">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="0a988-183">*Bundleconfig.json* файл анализируется в процессе построения для создания выходных файлов, на основе определенной конфигурации.</span><span class="sxs-lookup"><span data-stu-id="0a988-183">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a988-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a988-184">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="0a988-185">Добавить *BuildBundlerMinifier* пакета в проект.</span><span class="sxs-lookup"><span data-stu-id="0a988-185">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="0a988-186">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="0a988-186">Build the project.</span></span> <span data-ttu-id="0a988-187">В окне «Вывод» отображаются следующие сведения.</span><span class="sxs-lookup"><span data-stu-id="0a988-187">The following appears in the Output window:</span></span>

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

<span data-ttu-id="0a988-188">Очистите проект.</span><span class="sxs-lookup"><span data-stu-id="0a988-188">Clean the project.</span></span> <span data-ttu-id="0a988-189">В окне «Вывод» отображаются следующие сведения.</span><span class="sxs-lookup"><span data-stu-id="0a988-189">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0a988-190">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="0a988-190">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="0a988-191">Добавить *BuildBundlerMinifier* пакета в проект:</span><span class="sxs-lookup"><span data-stu-id="0a988-191">Add the *BuildBundlerMinifier* package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="0a988-192">При использовании ASP.NET Core 1.x, восстановите добавленные пакеты:</span><span class="sxs-lookup"><span data-stu-id="0a988-192">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```console
dotnet restore
```

<span data-ttu-id="0a988-193">Постройте проект:</span><span class="sxs-lookup"><span data-stu-id="0a988-193">Build the project:</span></span>

```console
dotnet build
```

<span data-ttu-id="0a988-194">Отображаются следующие сведения.</span><span class="sxs-lookup"><span data-stu-id="0a988-194">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="0a988-195">Очистите проект:</span><span class="sxs-lookup"><span data-stu-id="0a988-195">Clean the project:</span></span>

```console
dotnet clean
```

<span data-ttu-id="0a988-196">Появляется следующий результат:</span><span class="sxs-lookup"><span data-stu-id="0a988-196">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="0a988-197">Выполнение нерегламентированных объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="0a988-197">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="0a988-198">Это можно выполнить задачи объединение и Минификация нерегламентированные, без построения проекта.</span><span class="sxs-lookup"><span data-stu-id="0a988-198">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="0a988-199">Добавить [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) пакет NuGet для проекта:</span><span class="sxs-lookup"><span data-stu-id="0a988-199">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

<span data-ttu-id="0a988-200">Этот пакет расширяет .NET Core CLI для включения *dotnet пакета* средства.</span><span class="sxs-lookup"><span data-stu-id="0a988-200">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="0a988-201">В окне консоли диспетчера пакетов (PMC) или в командной оболочке можно выполнить следующую команду:</span><span class="sxs-lookup"><span data-stu-id="0a988-201">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```console
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="0a988-202">Диспетчер пакетов NuGet добавляет зависимости файл *.csproj в качестве `<PackageReference />` узлов.</span><span class="sxs-lookup"><span data-stu-id="0a988-202">NuGet Package Manager adds dependencies to the *.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="0a988-203">`dotnet bundle` Команда зарегистрирован .NET Core CLI только если `<DotNetCliToolReference />` используется узел.</span><span class="sxs-lookup"><span data-stu-id="0a988-203">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="0a988-204">Измените файл *.csproj соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="0a988-204">Modify the *.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="0a988-205">Добавление файлов в рабочий процесс</span><span class="sxs-lookup"><span data-stu-id="0a988-205">Add files to workflow</span></span>

<span data-ttu-id="0a988-206">Рассмотрим пример, в котором еще *custom.css* будет добавлен файл, аналогичный следующему:</span><span class="sxs-lookup"><span data-stu-id="0a988-206">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="0a988-207">Для уменьшения *custom.css* и объединить его с *site.css* в *site.min.css* файл, добавьте относительный путь к *bundleconfig.json*:</span><span class="sxs-lookup"><span data-stu-id="0a988-207">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="0a988-208">Кроме того может использоваться следующий шаблон этот режим:</span><span class="sxs-lookup"><span data-stu-id="0a988-208">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> <span data-ttu-id="0a988-209">Этот шаблон глобализацию поиск всех файлов CSS и исключает уменьшенный файла шаблона.</span><span class="sxs-lookup"><span data-stu-id="0a988-209">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="0a988-210">Постройте приложение.</span><span class="sxs-lookup"><span data-stu-id="0a988-210">Build the application.</span></span> <span data-ttu-id="0a988-211">Откройте *site.min.css* и обратите внимание, содержимое *custom.css* добавляется в конец файла.</span><span class="sxs-lookup"><span data-stu-id="0a988-211">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="0a988-212">На основе среды объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="0a988-212">Environment-based bundling and minification</span></span>

<span data-ttu-id="0a988-213">Рекомендуется распространение и уменьшенный файлы приложения следует использовать в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="0a988-213">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="0a988-214">Во время разработки исходные файлы убедитесь, чтобы упростить отладку приложения.</span><span class="sxs-lookup"><span data-stu-id="0a988-214">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="0a988-215">Указать, какие файлы для включения в страницы с помощью [вспомогательный тега среды](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) в представления.</span><span class="sxs-lookup"><span data-stu-id="0a988-215">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="0a988-216">Вспомогательный тега среды Подготовка содержимого только при работе в конкретных [средах](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="0a988-216">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="0a988-217">Следующие `environment` тег отображает необработанных файлов CSS при работе в `Development` среде:</span><span class="sxs-lookup"><span data-stu-id="0a988-217">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0a988-218">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0a988-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0a988-219">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0a988-219">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

---

<span data-ttu-id="0a988-220">Следующие `environment` тег отображает распространение и уменьшенный CSS-файл при выполнении в среде, отличный от `Development`.</span><span class="sxs-lookup"><span data-stu-id="0a988-220">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="0a988-221">Например, на котором работают `Production` или `Staging` триггеры для подготовки к просмотру эти таблицы стилей:</span><span class="sxs-lookup"><span data-stu-id="0a988-221">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0a988-222">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0a988-222">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0a988-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0a988-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

---

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="0a988-224">Использовать bundleconfig.json из Gulp</span><span class="sxs-lookup"><span data-stu-id="0a988-224">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="0a988-225">Существуют случаи, в которых приложение объединение и Минификация рабочего процесса требуется дополнительная обработка.</span><span class="sxs-lookup"><span data-stu-id="0a988-225">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="0a988-226">Примеры Оптимизация изображения, busting кэша и обработка активов CDN.</span><span class="sxs-lookup"><span data-stu-id="0a988-226">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="0a988-227">Чтобы удовлетворить этим требованиям, можно преобразовать объединение и Минификация рабочий процесс для использования Gulp.</span><span class="sxs-lookup"><span data-stu-id="0a988-227">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="0a988-228">Использовать расширение Bundler & Уменьшитель</span><span class="sxs-lookup"><span data-stu-id="0a988-228">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="0a988-229">Visual Studio [Bundler & Уменьшитель](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) расширения выполняет преобразование на Gulp.</span><span class="sxs-lookup"><span data-stu-id="0a988-229">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

<span data-ttu-id="0a988-230">Щелкните правой кнопкой мыши *bundleconfig.json* в обозревателе решений и выберите **Bundler & Уменьшитель** > **преобразовать Gulp...** :</span><span class="sxs-lookup"><span data-stu-id="0a988-230">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Преобразовать Gulp пункт контекстного меню](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="0a988-232">*Gulpfile.js* и *package.json* файлы добавляются в проект.</span><span class="sxs-lookup"><span data-stu-id="0a988-232">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="0a988-233">Поддержка [npm](https://www.npmjs.com/) пакеты, перечисленные в *package.json* файла `devDependencies` устанавливаются раздела.</span><span class="sxs-lookup"><span data-stu-id="0a988-233">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="0a988-234">Выполните следующую команду в окне PMC установить Gulp CLI в качестве глобального зависимостей:</span><span class="sxs-lookup"><span data-stu-id="0a988-234">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="0a988-235">*Gulpfile.js* операции чтения файла *bundleconfig.json* файл для входов, выходов и параметры.</span><span class="sxs-lookup"><span data-stu-id="0a988-235">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="0a988-236">Преобразовать вручную</span><span class="sxs-lookup"><span data-stu-id="0a988-236">Convert manually</span></span>

<span data-ttu-id="0a988-237">Если Visual Studio или расширение Bundler & Уменьшитель недоступны, преобразуйте вручную.</span><span class="sxs-lookup"><span data-stu-id="0a988-237">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="0a988-238">Добавить *package.json* файл со следующими `devDependencies`, в корне проекта:</span><span class="sxs-lookup"><span data-stu-id="0a988-238">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="0a988-239">Установить зависимости, выполнив следующую команду на том же уровне, как *package.json*:</span><span class="sxs-lookup"><span data-stu-id="0a988-239">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="0a988-240">Установите Gulp CLI как глобальные зависимостей:</span><span class="sxs-lookup"><span data-stu-id="0a988-240">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="0a988-241">Копировать *gulpfile.js* файла ниже в корне проекта:</span><span class="sxs-lookup"><span data-stu-id="0a988-241">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="0a988-242">Выполнение задач Gulp</span><span class="sxs-lookup"><span data-stu-id="0a988-242">Run Gulp tasks</span></span>

<span data-ttu-id="0a988-243">Для запуска задачи минификации Gulp до сборки проекта в Visual Studio, добавьте следующий [MSBuild: целевая](/visualstudio/msbuild/msbuild-targets) *.csproj файл:</span><span class="sxs-lookup"><span data-stu-id="0a988-243">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the *.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="0a988-244">В этом примере все задачи, определенные в `MyPreCompileTarget` целевые запуска перед стандартных `Build` цели.</span><span class="sxs-lookup"><span data-stu-id="0a988-244">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="0a988-245">В окне вывода Visual Studio появляется результат, аналогичный приведенному ниже:</span><span class="sxs-lookup"><span data-stu-id="0a988-245">Output similar to the following appears in Visual Studio's Output window:</span></span>

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

<span data-ttu-id="0a988-246">Кроме того Visual Studio Task Runner Explorer может использоваться для связи Gulp задач для конкретных событий в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0a988-246">Alternatively, Visual Studio's Task Runner Explorer may be used to bind Gulp tasks to specific Visual Studio events.</span></span> <span data-ttu-id="0a988-247">В разделе [выполнения задач по умолчанию](xref:client-side/using-gulp#running-default-tasks) инструкции по этим способом.</span><span class="sxs-lookup"><span data-stu-id="0a988-247">See [Running default tasks](xref:client-side/using-gulp#running-default-tasks) for instructions on doing that.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0a988-248">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0a988-248">Additional resources</span></span>

* [<span data-ttu-id="0a988-249">Использование Gulp</span><span class="sxs-lookup"><span data-stu-id="0a988-249">Using Gulp</span></span>](xref:client-side/using-gulp)
* [<span data-ttu-id="0a988-250">Использование Grunt</span><span class="sxs-lookup"><span data-stu-id="0a988-250">Using Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="0a988-251">Работа с несколькими средами</span><span class="sxs-lookup"><span data-stu-id="0a988-251">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="0a988-252">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="0a988-252">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
