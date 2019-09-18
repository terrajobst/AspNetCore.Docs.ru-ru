---
title: Объединять и уменьшение статические ресурсы в ASP.NET Core
author: scottaddie
description: Узнайте, как оптимизировать статические ресурсы в веб-приложении ASP.NET Core, применяя методы объединения и минификации.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/17/2019
uid: client-side/bundling-and-minification
ms.openlocfilehash: 7499381a24a2513a4fbd1205f245e624c86647c3
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080557"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a><span data-ttu-id="e1c37-103">Объединять и уменьшение статические ресурсы в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e1c37-103">Bundle and minify static assets in ASP.NET Core</span></span>

<span data-ttu-id="e1c37-104">[Скотт Эдди (](https://twitter.com/Scott_Addie) и [Дэвид сосна](https://twitter.com/davidpine7)</span><span class="sxs-lookup"><span data-stu-id="e1c37-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [David Pine](https://twitter.com/davidpine7)</span></span>

<span data-ttu-id="e1c37-105">В этой статье объясняются преимущества применения объединения и минификации, в том числе сведения о том, как эти функции можно использовать с веб-приложениями ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e1c37-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="e1c37-106">Что такое объединение и минификации</span><span class="sxs-lookup"><span data-stu-id="e1c37-106">What is bundling and minification</span></span>

<span data-ttu-id="e1c37-107">Объединение и минификации — это две различные оптимизации производительности, которые можно применить в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="e1c37-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="e1c37-108">Совместное использование, объединение и минификации повышение производительности за счет сокращения числа запросов сервера и уменьшения размера запрошенных статических ресурсов.</span><span class="sxs-lookup"><span data-stu-id="e1c37-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="e1c37-109">Объединение и минификации в первую очередь повышает время загрузки первого запроса страницы.</span><span class="sxs-lookup"><span data-stu-id="e1c37-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="e1c37-110">После запроса веб-страницы браузер кэширует статические ресурсы (JavaScript, CSS и изображения).</span><span class="sxs-lookup"><span data-stu-id="e1c37-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="e1c37-111">Следовательно, объединение и минификации не улучшают производительность при запросе одной и той же страницы или страниц на одном сайте, запрашивающем одни и те же активы.</span><span class="sxs-lookup"><span data-stu-id="e1c37-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="e1c37-112">Если заголовок Expires неправильно задан в ресурсах, а объединение и минификации не используются, эвристика обновления обозревателя помечает активы как устаревшие через несколько дней.</span><span class="sxs-lookup"><span data-stu-id="e1c37-112">If the expires header isn't set correctly on the assets and if bundling and minification isn't used, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="e1c37-113">Кроме того, браузер требует запрос проверки для каждого ресурса.</span><span class="sxs-lookup"><span data-stu-id="e1c37-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="e1c37-114">В этом случае объединение и минификации обеспечивают повышение производительности даже после первого запроса страницы.</span><span class="sxs-lookup"><span data-stu-id="e1c37-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="e1c37-115">объединения</span><span class="sxs-lookup"><span data-stu-id="e1c37-115">Bundling</span></span>

<span data-ttu-id="e1c37-116">Объединение нескольких файлов в один файл.</span><span class="sxs-lookup"><span data-stu-id="e1c37-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="e1c37-117">Объединение сокращает количество запросов к серверу, необходимых для подготовки к просмотру веб-ресурса, например веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="e1c37-117">Bundling reduces the number of server requests that are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="e1c37-118">Можно создать любое количество отдельных пакетов специально для CSS, JavaScript и т. д. Меньшее число файлов означает меньшее количество HTTP-запросов от браузера к серверу или от службы, предоставляющей приложение.</span><span class="sxs-lookup"><span data-stu-id="e1c37-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="e1c37-119">Это приводит к повышению производительности первой загрузки страницы.</span><span class="sxs-lookup"><span data-stu-id="e1c37-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="e1c37-120">Минификации</span><span class="sxs-lookup"><span data-stu-id="e1c37-120">Minification</span></span>

<span data-ttu-id="e1c37-121">Минификации удаляет ненужные символы из кода без изменения функциональности.</span><span class="sxs-lookup"><span data-stu-id="e1c37-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="e1c37-122">В результате значительно уменьшается размер запрошенных ресурсов (например, CSS, изображений и файлов JavaScript).</span><span class="sxs-lookup"><span data-stu-id="e1c37-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="e1c37-123">Обычные побочные эффекты минификации включают сокращение имен переменных в один символ и удаление комментариев и ненужных пробелов.</span><span class="sxs-lookup"><span data-stu-id="e1c37-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="e1c37-124">Рассмотрим следующую функцию JavaScript:</span><span class="sxs-lookup"><span data-stu-id="e1c37-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="e1c37-125">Минификации сокращает функцию до следующего:</span><span class="sxs-lookup"><span data-stu-id="e1c37-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="e1c37-126">Помимо удаления комментариев и ненужных пробелов, следующие имена параметров и переменных были переименованы следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e1c37-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="e1c37-127">До преобразования</span><span class="sxs-lookup"><span data-stu-id="e1c37-127">Original</span></span> | <span data-ttu-id="e1c37-128">Переименовано</span><span class="sxs-lookup"><span data-stu-id="e1c37-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="e1c37-129">Влияние объединения и минификации</span><span class="sxs-lookup"><span data-stu-id="e1c37-129">Impact of bundling and minification</span></span>

<span data-ttu-id="e1c37-130">В следующей таблице описаны различия между индивидуальной загрузкой ресурсов и использованием объединения и минификации:</span><span class="sxs-lookup"><span data-stu-id="e1c37-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="e1c37-131">Действие</span><span class="sxs-lookup"><span data-stu-id="e1c37-131">Action</span></span> | <span data-ttu-id="e1c37-132">С B/M</span><span class="sxs-lookup"><span data-stu-id="e1c37-132">With B/M</span></span> | <span data-ttu-id="e1c37-133">Без B/M</span><span class="sxs-lookup"><span data-stu-id="e1c37-133">Without B/M</span></span> | <span data-ttu-id="e1c37-134">Изменение</span><span class="sxs-lookup"><span data-stu-id="e1c37-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="e1c37-135">Запросы файлов</span><span class="sxs-lookup"><span data-stu-id="e1c37-135">File Requests</span></span>  | <span data-ttu-id="e1c37-136">7</span><span class="sxs-lookup"><span data-stu-id="e1c37-136">7</span></span>   | <span data-ttu-id="e1c37-137">18</span><span class="sxs-lookup"><span data-stu-id="e1c37-137">18</span></span>     | <span data-ttu-id="e1c37-138">157%</span><span class="sxs-lookup"><span data-stu-id="e1c37-138">157%</span></span>
<span data-ttu-id="e1c37-139">Передано КБ</span><span class="sxs-lookup"><span data-stu-id="e1c37-139">KB Transferred</span></span> | <span data-ttu-id="e1c37-140">156</span><span class="sxs-lookup"><span data-stu-id="e1c37-140">156</span></span> | <span data-ttu-id="e1c37-141">264.68</span><span class="sxs-lookup"><span data-stu-id="e1c37-141">264.68</span></span> | <span data-ttu-id="e1c37-142">70%</span><span class="sxs-lookup"><span data-stu-id="e1c37-142">70%</span></span>
<span data-ttu-id="e1c37-143">Время загрузки (МС)</span><span class="sxs-lookup"><span data-stu-id="e1c37-143">Load Time (ms)</span></span> | <span data-ttu-id="e1c37-144">885</span><span class="sxs-lookup"><span data-stu-id="e1c37-144">885</span></span> | <span data-ttu-id="e1c37-145">2360</span><span class="sxs-lookup"><span data-stu-id="e1c37-145">2360</span></span>   | <span data-ttu-id="e1c37-146">167%</span><span class="sxs-lookup"><span data-stu-id="e1c37-146">167%</span></span>

<span data-ttu-id="e1c37-147">Обозреватели являются довольно подробными в отношении заголовков HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="e1c37-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="e1c37-148">Метрика "всего Отправлено байт" заметно уменьшается при объединении.</span><span class="sxs-lookup"><span data-stu-id="e1c37-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="e1c37-149">Время загрузки показывает значительное улучшение, но этот пример выполнялся локально.</span><span class="sxs-lookup"><span data-stu-id="e1c37-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="e1c37-150">Повышение производительности достигается при использовании объединения и минификации с ресурсами, передаваемыми по сети.</span><span class="sxs-lookup"><span data-stu-id="e1c37-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="e1c37-151">Выбор стратегии объединения и минификации</span><span class="sxs-lookup"><span data-stu-id="e1c37-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="e1c37-152">Шаблоны проектов MVC и Razor Pages предоставляют готовые решения для объединения и минификации, состоящие из файла конфигурации JSON.</span><span class="sxs-lookup"><span data-stu-id="e1c37-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="e1c37-153">Сторонние средства, такие как средство выполнения задач [grunt](xref:client-side/using-grunt) , выполняют те же задачи с более сложными задачами.</span><span class="sxs-lookup"><span data-stu-id="e1c37-153">Third-party tools, such as the [Grunt](xref:client-side/using-grunt) task runner, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="e1c37-154">Средство стороннего производителя отлично подходит, если рабочему процессу требуется обработка, не связанная с объединением и&mdash;минификации, например linting и оптимизация изображений.</span><span class="sxs-lookup"><span data-stu-id="e1c37-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="e1c37-155">С помощью объединения и минификации во время разработки файлы минифицированные создаются до развертывания приложения.</span><span class="sxs-lookup"><span data-stu-id="e1c37-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="e1c37-156">Объединение и Минификация до развертывания обеспечивает преимущества снижения нагрузки на сервер.</span><span class="sxs-lookup"><span data-stu-id="e1c37-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="e1c37-157">Однако важно понимать, что объединение во время разработки и минификации повышает сложность сборки и работает только со статическими файлами.</span><span class="sxs-lookup"><span data-stu-id="e1c37-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="e1c37-158">Настройка объединения и минификации</span><span class="sxs-lookup"><span data-stu-id="e1c37-158">Configure bundling and minification</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="e1c37-159">В ASP.NET Core 2,0 или более ранней версии шаблоны проектов MVC и Razor Pages предоставляют файл конфигурации *бундлеконфиг. JSON* , который определяет параметры для каждого пакета:</span><span class="sxs-lookup"><span data-stu-id="e1c37-159">In ASP.NET Core 2.0 or earlier, the MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file that defines the options for each bundle:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="e1c37-160">В ASP.NET Core 2,1 или более поздней версии добавьте новый JSON-файл с именем *бундлеконфиг. JSON*в корневой каталог проекта MVC или Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="e1c37-160">In ASP.NET Core 2.1 or later, add a new JSON file, named *bundleconfig.json*, to the MVC or Razor Pages project root.</span></span> <span data-ttu-id="e1c37-161">Включить следующий код JSON в этот файл в качестве отправной точки:</span><span class="sxs-lookup"><span data-stu-id="e1c37-161">Include the following JSON in that file as a starting point:</span></span>

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="e1c37-162">Файл *бундлеконфиг. JSON* определяет параметры для каждого пакета.</span><span class="sxs-lookup"><span data-stu-id="e1c37-162">The *bundleconfig.json* file defines the options for each bundle.</span></span> <span data-ttu-id="e1c37-163">В предыдущем примере для пользовательских файлов JavaScript (*wwwroot/JS/site. js*) и таблиц стилей (*wwwroot/CSS/site. CSS*) определена конфигурация одного пакета.</span><span class="sxs-lookup"><span data-stu-id="e1c37-163">In the preceding example, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files.</span></span>

<span data-ttu-id="e1c37-164">Параметры конфигурации включают:</span><span class="sxs-lookup"><span data-stu-id="e1c37-164">Configuration options include:</span></span>

* <span data-ttu-id="e1c37-165">`outputFileName`: Имя выходного файла пакета.</span><span class="sxs-lookup"><span data-stu-id="e1c37-165">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="e1c37-166">Может содержать относительный путь из файла *бундлеконфиг. JSON* .</span><span class="sxs-lookup"><span data-stu-id="e1c37-166">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="e1c37-167">**Обязательно**</span><span class="sxs-lookup"><span data-stu-id="e1c37-167">**required**</span></span>
* <span data-ttu-id="e1c37-168">`inputFiles`: Массив файлов для объединения.</span><span class="sxs-lookup"><span data-stu-id="e1c37-168">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="e1c37-169">Это относительные пути к файлу конфигурации.</span><span class="sxs-lookup"><span data-stu-id="e1c37-169">These are relative paths to the configuration file.</span></span> <span data-ttu-id="e1c37-170">**необязательно**, \* пустое значение приводит к пустому выходному файлу.</span><span class="sxs-lookup"><span data-stu-id="e1c37-170">**optional**, \*an empty value results in an empty output file.</span></span> <span data-ttu-id="e1c37-171">поддерживаются шаблоны [глобализации](https://www.tldp.org/LDP/abs/html/globbingref.html) .</span><span class="sxs-lookup"><span data-stu-id="e1c37-171">[globbing](https://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="e1c37-172">`minify`: Параметры минификации для типа выходных данных.</span><span class="sxs-lookup"><span data-stu-id="e1c37-172">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="e1c37-173">**необязательный**, *по `minify: { enabled: true }` умолчанию —*</span><span class="sxs-lookup"><span data-stu-id="e1c37-173">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="e1c37-174">Параметры конфигурации доступны для каждого типа выходного файла.</span><span class="sxs-lookup"><span data-stu-id="e1c37-174">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="e1c37-175">Minifier CSS</span><span class="sxs-lookup"><span data-stu-id="e1c37-175">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="e1c37-176">Minifier JavaScript</span><span class="sxs-lookup"><span data-stu-id="e1c37-176">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="e1c37-177">Minifier HTML</span><span class="sxs-lookup"><span data-stu-id="e1c37-177">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="e1c37-178">`includeInProject`: Флаг, указывающий, добавлять ли созданные файлы в файл проекта.</span><span class="sxs-lookup"><span data-stu-id="e1c37-178">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="e1c37-179">**необязательный**, *по умолчанию — false*</span><span class="sxs-lookup"><span data-stu-id="e1c37-179">**optional**, *default - false*</span></span>
* <span data-ttu-id="e1c37-180">`sourceMap`: Флаг, указывающий, создавать ли исходную карту для объединенного файла.</span><span class="sxs-lookup"><span data-stu-id="e1c37-180">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="e1c37-181">**необязательный**, *по умолчанию — false*</span><span class="sxs-lookup"><span data-stu-id="e1c37-181">**optional**, *default - false*</span></span>
* <span data-ttu-id="e1c37-182">`sourceMapRootPath`: Корневой путь для хранения созданного исходного файла отображения.</span><span class="sxs-lookup"><span data-stu-id="e1c37-182">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="e1c37-183">Выполнение объединения и минификации во время сборки</span><span class="sxs-lookup"><span data-stu-id="e1c37-183">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="e1c37-184">Пакет NuGet [буилдбундлерминифиер](https://www.nuget.org/packages/BuildBundlerMinifier/) обеспечивает выполнение объединения и минификации во время сборки.</span><span class="sxs-lookup"><span data-stu-id="e1c37-184">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="e1c37-185">Пакет внедряет [целевые объекты MSBuild](/visualstudio/msbuild/msbuild-targets) , которые выполняются при сборке и очистке времени.</span><span class="sxs-lookup"><span data-stu-id="e1c37-185">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="e1c37-186">Файл *бундлеконфиг. JSON* анализируется процессом сборки для получения выходных файлов на основе определенной конфигурации.</span><span class="sxs-lookup"><span data-stu-id="e1c37-186">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="e1c37-187">Буилдбундлерминифиер принадлежит к управляемому сообществом проекту в GitHub, для которого корпорация Майкрософт не предоставляет поддержку.</span><span class="sxs-lookup"><span data-stu-id="e1c37-187">BuildBundlerMinifier belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="e1c37-188">[Здесь](https://github.com/madskristensen/BundlerMinifier/issues)должны быть зарегистрированы проблемы.</span><span class="sxs-lookup"><span data-stu-id="e1c37-188">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1c37-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1c37-189">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e1c37-190">Добавьте пакет *буилдбундлерминифиер* в проект.</span><span class="sxs-lookup"><span data-stu-id="e1c37-190">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="e1c37-191">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="e1c37-191">Build the project.</span></span> <span data-ttu-id="e1c37-192">В окне вывода появится следующее:</span><span class="sxs-lookup"><span data-stu-id="e1c37-192">The following appears in the Output window:</span></span>

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

<span data-ttu-id="e1c37-193">Очистите проект.</span><span class="sxs-lookup"><span data-stu-id="e1c37-193">Clean the project.</span></span> <span data-ttu-id="e1c37-194">В окне вывода появится следующее:</span><span class="sxs-lookup"><span data-stu-id="e1c37-194">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e1c37-195">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="e1c37-195">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e1c37-196">Добавьте пакет *буилдбундлерминифиер* в проект:</span><span class="sxs-lookup"><span data-stu-id="e1c37-196">Add the *BuildBundlerMinifier* package to your project:</span></span>

```dotnetcli
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="e1c37-197">При использовании ASP.NET Core 1. x восстановите только что добавленный пакет:</span><span class="sxs-lookup"><span data-stu-id="e1c37-197">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```dotnetcli
dotnet restore
```

<span data-ttu-id="e1c37-198">Выполните сборку проекта:</span><span class="sxs-lookup"><span data-stu-id="e1c37-198">Build the project:</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="e1c37-199">Появится следующее:</span><span class="sxs-lookup"><span data-stu-id="e1c37-199">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="e1c37-200">Очистите проект:</span><span class="sxs-lookup"><span data-stu-id="e1c37-200">Clean the project:</span></span>

```dotnetcli
dotnet clean
```

<span data-ttu-id="e1c37-201">Появится следующий результат:</span><span class="sxs-lookup"><span data-stu-id="e1c37-201">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="e1c37-202">Прямое выполнение объединения и минификации</span><span class="sxs-lookup"><span data-stu-id="e1c37-202">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="e1c37-203">Задачи объединения и минификации можно выполнять на нерегламентированном уровне, не создавая проект.</span><span class="sxs-lookup"><span data-stu-id="e1c37-203">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="e1c37-204">Добавьте в проект пакет NuGet [бундлерминифиер. Core](https://www.nuget.org/packages/BundlerMinifier.Core/) :</span><span class="sxs-lookup"><span data-stu-id="e1c37-204">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> <span data-ttu-id="e1c37-205">Бундлерминифиер. Core принадлежит к управляемому сообществом проекту в GitHub, для которого корпорация Майкрософт не предоставляет поддержку.</span><span class="sxs-lookup"><span data-stu-id="e1c37-205">BundlerMinifier.Core belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="e1c37-206">[Здесь](https://github.com/madskristensen/BundlerMinifier/issues)должны быть зарегистрированы проблемы.</span><span class="sxs-lookup"><span data-stu-id="e1c37-206">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="e1c37-207">Этот пакет расширяет .NET Core CLI для включения средства *DotNet-* Package.</span><span class="sxs-lookup"><span data-stu-id="e1c37-207">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="e1c37-208">Следующую команду можно выполнить в окне консоли диспетчера пакетов (PMC) или в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="e1c37-208">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```dotnetcli
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="e1c37-209">Диспетчер пакетов NuGet добавляет зависимости в файл \*. csproj в качестве `<PackageReference />` узлов.</span><span class="sxs-lookup"><span data-stu-id="e1c37-209">NuGet Package Manager adds dependencies to the \*.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="e1c37-210">Команда регистрируется в .NET Core CLI только `<DotNetCliToolReference />` при использовании узла. `dotnet bundle`</span><span class="sxs-lookup"><span data-stu-id="e1c37-210">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="e1c37-211">Внесите соответствующие изменения в файл \*. csproj.</span><span class="sxs-lookup"><span data-stu-id="e1c37-211">Modify the \*.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="e1c37-212">Добавить файлы в рабочий процесс</span><span class="sxs-lookup"><span data-stu-id="e1c37-212">Add files to workflow</span></span>

<span data-ttu-id="e1c37-213">Рассмотрим пример, в котором добавляется дополнительный файл *. CSS* , похожий на следующий:</span><span class="sxs-lookup"><span data-stu-id="e1c37-213">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="e1c37-214">Чтобы уменьшение *Custom. CSS* и объединить его с *site. CSS* в файл *site. min. CSS* , добавьте относительный путь в *бундлеконфиг. JSON*:</span><span class="sxs-lookup"><span data-stu-id="e1c37-214">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="e1c37-215">Кроме того, можно использовать следующий шаблон глобализации:</span><span class="sxs-lookup"><span data-stu-id="e1c37-215">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css))"]
> ```
>
> <span data-ttu-id="e1c37-216">Этот шаблон глобализации соответствует всем файлам CSS и исключает шаблон файла минифицированные.</span><span class="sxs-lookup"><span data-stu-id="e1c37-216">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="e1c37-217">Постройте приложение.</span><span class="sxs-lookup"><span data-stu-id="e1c37-217">Build the application.</span></span> <span data-ttu-id="e1c37-218">Откройте файл *site. min. CSS* и обратите внимание, что содержимое *Custom. CSS* добавляется в конец файла.</span><span class="sxs-lookup"><span data-stu-id="e1c37-218">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="e1c37-219">Объединение и минификации на основе среды</span><span class="sxs-lookup"><span data-stu-id="e1c37-219">Environment-based bundling and minification</span></span>

<span data-ttu-id="e1c37-220">Рекомендуется использовать объединенные и минифицированные файлы приложения в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="e1c37-220">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="e1c37-221">Во время разработки исходные файлы упрощают отладку приложения.</span><span class="sxs-lookup"><span data-stu-id="e1c37-221">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="e1c37-222">Укажите, какие файлы следует включить в страницы с помощью [вспомогательной функции тега среды](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) в представлениях.</span><span class="sxs-lookup"><span data-stu-id="e1c37-222">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="e1c37-223">Вспомогательная функция тега среды отображает свое содержимое только при выполнении в конкретных [средах](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="e1c37-223">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="e1c37-224">Следующий `environment` тег отображает необработанные файлы CSS при выполнении `Development` в среде:</span><span class="sxs-lookup"><span data-stu-id="e1c37-224">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

<span data-ttu-id="e1c37-225">Следующий `environment` тег отображает Объединенные и минифицированные CSS файлы при работе в среде, отличной от `Development`.</span><span class="sxs-lookup"><span data-stu-id="e1c37-225">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="e1c37-226">Например, запуск в `Production` или `Staging` запускает визуализацию этих таблиц стилей:</span><span class="sxs-lookup"><span data-stu-id="e1c37-226">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="e1c37-227">Использование бундлеконфиг. JSON из gulp</span><span class="sxs-lookup"><span data-stu-id="e1c37-227">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="e1c37-228">В некоторых случаях для объединения и минификации рабочего процесса приложения требуется дополнительная обработка.</span><span class="sxs-lookup"><span data-stu-id="e1c37-228">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="e1c37-229">Примеры: Оптимизация изображений, отключения кэша и обработка ресурсов CDN.</span><span class="sxs-lookup"><span data-stu-id="e1c37-229">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="e1c37-230">Для удовлетворения этих требований можно преобразовать рабочий процесс объединения и минификации для использования gulp.</span><span class="sxs-lookup"><span data-stu-id="e1c37-230">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="e1c37-231">Использование модуля увязки & расширение Minifier</span><span class="sxs-lookup"><span data-stu-id="e1c37-231">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="e1c37-232">Модуль упаковки Visual Studio [& расширение Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) обрабатывает преобразование в gulp.</span><span class="sxs-lookup"><span data-stu-id="e1c37-232">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="e1c37-233">Модуль & расширения Minifier принадлежит к управляемому сообществом проекту на GitHub, для которого корпорация Майкрософт не предоставляет поддержку.</span><span class="sxs-lookup"><span data-stu-id="e1c37-233">The Bundler & Minifier extension belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="e1c37-234">[Здесь](https://github.com/madskristensen/BundlerMinifier/issues)должны быть зарегистрированы проблемы.</span><span class="sxs-lookup"><span data-stu-id="e1c37-234">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="e1c37-235">Щелкните правой кнопкой мыши файл *бундлеконфиг. JSON* в Обозреватель решений и выберите Группа **& Minifier** > **преобразовать в gulp...** :</span><span class="sxs-lookup"><span data-stu-id="e1c37-235">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Преобразовать в элемент контекстного меню gulp](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="e1c37-237">Файлы *gulpfile. js* и *Package. JSON* добавляются в проект.</span><span class="sxs-lookup"><span data-stu-id="e1c37-237">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="e1c37-238">Вспомогательные пакеты [NPM](https://www.npmjs.com/) , перечисленные в `devDependencies` разделе файла *Package. JSON* , установлены.</span><span class="sxs-lookup"><span data-stu-id="e1c37-238">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="e1c37-239">Выполните следующую команду в окне PMC, чтобы установить gulp CLI в качестве глобальной зависимости:</span><span class="sxs-lookup"><span data-stu-id="e1c37-239">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="e1c37-240">Файл *gulpfile. js* считывает файл *бундлеконфиг. JSON* для входных, выходных данных и параметров.</span><span class="sxs-lookup"><span data-stu-id="e1c37-240">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="e1c37-241">Преобразование вручную</span><span class="sxs-lookup"><span data-stu-id="e1c37-241">Convert manually</span></span>

<span data-ttu-id="e1c37-242">Если Visual Studio и (или) пакет & расширение Minifier недоступен, преобразуйте их вручную.</span><span class="sxs-lookup"><span data-stu-id="e1c37-242">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="e1c37-243">Добавьте в корневой каталог проекта файл *Package. JSON* со следующим `devDependencies`параметром:</span><span class="sxs-lookup"><span data-stu-id="e1c37-243">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

> [!WARNING]
> <span data-ttu-id="e1c37-244">`gulp-uglify` Модуль не поддерживает ECMAScript (ES) 2015/ES6 и более поздние версии.</span><span class="sxs-lookup"><span data-stu-id="e1c37-244">The `gulp-uglify` module doesn't support ECMAScript (ES) 2015 / ES6 and later.</span></span> <span data-ttu-id="e1c37-245">Установите [gulp-сжатый](https://www.npmjs.com/package/gulp-terser) вместо `gulp-uglify` использования ES2015/ES6 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="e1c37-245">Install [gulp-terser](https://www.npmjs.com/package/gulp-terser) instead of `gulp-uglify` to use ES2015 / ES6 or later.</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="e1c37-246">Установите зависимости, выполнив следующую команду на том же уровне, что и *Package. JSON*:</span><span class="sxs-lookup"><span data-stu-id="e1c37-246">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="e1c37-247">Установите CLI gulp в качестве глобальной зависимости:</span><span class="sxs-lookup"><span data-stu-id="e1c37-247">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="e1c37-248">Скопируйте файл *gulpfile. js* ниже в корневую папку проекта:</span><span class="sxs-lookup"><span data-stu-id="e1c37-248">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="e1c37-249">Выполнение задач gulp</span><span class="sxs-lookup"><span data-stu-id="e1c37-249">Run Gulp tasks</span></span>

<span data-ttu-id="e1c37-250">Чтобы активировать задачу gulp минификации перед построением проекта в Visual Studio, добавьте следующий [целевой объект MSBuild](/visualstudio/msbuild/msbuild-targets) в файл \*. csproj:</span><span class="sxs-lookup"><span data-stu-id="e1c37-250">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the \*.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="e1c37-251">В этом примере все задачи, определенные в `MyPreCompileTarget` целевом объекте, выполняются до предопределенного `Build` целевого объекта.</span><span class="sxs-lookup"><span data-stu-id="e1c37-251">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="e1c37-252">Выходные данные, аналогичные приведенным ниже, отображаются в окне Вывод Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="e1c37-252">Output similar to the following appears in Visual Studio's Output window:</span></span>

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


## <a name="additional-resources"></a><span data-ttu-id="e1c37-253">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e1c37-253">Additional resources</span></span>

* [<span data-ttu-id="e1c37-254">Использование Grunt</span><span class="sxs-lookup"><span data-stu-id="e1c37-254">Use Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="e1c37-255">Использование нескольких сред</span><span class="sxs-lookup"><span data-stu-id="e1c37-255">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="e1c37-256">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="e1c37-256">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
