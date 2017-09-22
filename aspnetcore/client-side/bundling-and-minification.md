---
title: "Объединение и Минификация в ASP.NET Core"
author: spboyer
description: 
keywords: "ASP.NET Core, объединение и Минификация, CSS, JavaScript, уменьшения, BuildBundlerMinifier"
ms.author: riande
manager: wpickett
ms.date: 02/28/2017
ms.topic: article
ms.assetid: d54230f9-8e5f-4861-a29c-1d3a14e0b0d9
ms.technology: aspnet
ms.prod: aspnet-core
uid: client-side/bundling-and-minification
ms.openlocfilehash: 11528cb2067ced79a09845f9ff78d897da033438
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="bundling-and-minification-in-aspnet-core"></a><span data-ttu-id="ed2bc-103">Объединение и Минификация в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ed2bc-103">Bundling and minification in ASP.NET Core</span></span>

<span data-ttu-id="ed2bc-104">Объединение и Минификация — это два метода можно использовать в ASP.NET для повышения производительности загрузки страницы для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-104">Bundling and minification are two techniques you can use in ASP.NET to improve page load performance for your web application.</span></span> <span data-ttu-id="ed2bc-105">Объединение объединяет несколько файлов в один файл.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-105">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="ed2bc-106">Минификации выполняет различные оптимизации кода различные сценарии и CSS, что приводит в полезных данных меньшего размера.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-106">Minification performs a variety of different code optimizations to scripts and CSS, which results in smaller payloads.</span></span> <span data-ttu-id="ed2bc-107">Использовать совместно, объединение и Минификация повышает производительность во время загрузки, уменьшая число запросов к серверу и уменьшения размера запрошенных ресурсов (например, файлов CSS и JavaScript).</span><span class="sxs-lookup"><span data-stu-id="ed2bc-107">Used together, bundling and minification improves load time performance by reducing the number of requests to the server and reducing the size of the requested assets (such as CSS and JavaScript files).</span></span>

<span data-ttu-id="ed2bc-108">В этой статье объясняется преимущества использования объединение и Минификация, включая использование этих функций с приложениями ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-108">This article explains the benefits of using bundling and minification, including how these features can be used with ASP.NET Core applications.</span></span>

## <a name="overview"></a><span data-ttu-id="ed2bc-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="ed2bc-109">Overview</span></span>

<span data-ttu-id="ed2bc-110">В приложениях ASP.NET Core существует несколько вариантов объединение и Минификация ресурсами на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-110">In ASP.NET Core apps, there are multiple options for bundling and minifying client-side resources.</span></span> <span data-ttu-id="ed2bc-111">Основные шаблоны для MVC и предоставления решения с помощью файла конфигурации и пакет BuildBundlerMinifier NuGet out of box.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-111">The core templates for MVC provide an out-of-the-box solution using a configuration file and BuildBundlerMinifier NuGet package.</span></span> <span data-ttu-id="ed2bc-112">Сторонние средства, такие как [Gulp](using-gulp.md) и [Grunt](using-grunt.md) также могут использоваться для выполнения тех же задач сложности и дополнительных рабочего процесса требуется процессу.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-112">Third party tools, such as [Gulp](using-gulp.md) and [Grunt](using-grunt.md) are also available to accomplish the same tasks should your processes require additional workflow or complexities.</span></span> <span data-ttu-id="ed2bc-113">С помощью разработки объединение и Минификация, уменьшенный файлы создаются до развертывания приложения.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-113">By using design-time bundling and minification, the minified files are created prior to the application’s deployment.</span></span> <span data-ttu-id="ed2bc-114">Объединение и Минификация перед развертыванием предоставляет преимущество снижение нагрузки.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-114">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="ed2bc-115">Однако следует отметить, что объединение во время разработки и иными увеличивает сложность сборки и работает только с статических файлов.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-115">However, it’s important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

<span data-ttu-id="ed2bc-116">Объединение и Минификация главным образом улучшить время загрузки первого запроса страницы.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-116">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="ed2bc-117">После запросил веб-страницы браузер кэширует активы (JavaScript, CSS и изображения), объединение и Минификация не даст любое повышение производительности при запросе ту же страницу или сайт страниц на том же запросе же активы.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-117">Once a web page has been requested, the browser caches the assets (JavaScript, CSS and images) so bundling and minification won’t provide any performance boost when requesting the same page, or pages on the same site requesting the same assets.</span></span> <span data-ttu-id="ed2bc-118">Если вы не задали заголовок правильно на Ваши активы истечения срока действия и не используйте объединение и Минификация, эвристики актуальность обозревателя будут отмечены как активы устаревших через несколько дней и требуется браузер запроса на проверку для каждого ресурса.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-118">If you don’t set the expires header correctly on your assets, and you don’t use bundling and minification, the browser's freshness heuristics will mark the assets stale after a few days and the browser will require a validation request for each asset.</span></span> <span data-ttu-id="ed2bc-119">В этом случае объединение и Минификация обеспечивают повышение производительности даже после первого запроса страницы.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-119">In this case, bundling and minification provide a performance increase even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="ed2bc-120">Объединение</span><span class="sxs-lookup"><span data-stu-id="ed2bc-120">Bundling</span></span>

<span data-ttu-id="ed2bc-121">Объединение — это компонент, упрощающий для объединения или объединить несколько файлов в один файл.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-121">Bundling is a feature that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="ed2bc-122">Так как объединение объединяет несколько файлов в один файл, это уменьшает количество запросов к серверу, которые необходимы для получения и отображения web средств, таких как веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-122">Because bundling combines multiple files into a single file, it reduces the number of requests to the server that are required to retrieve and display a web asset, such as a web page.</span></span> <span data-ttu-id="ed2bc-123">Можно создать CSS, JavaScript и другие пакеты.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-123">You can create CSS, JavaScript and other bundles.</span></span> <span data-ttu-id="ed2bc-124">Меньшее число файлов означает меньшее количество HTTP-запросов из браузера на сервер или из службы, предоставляющей приложение.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-124">Fewer files means fewer HTTP requests from your browser to the server or from the service providing your application.</span></span> <span data-ttu-id="ed2bc-125">Результатом является повышение производительности загрузки первой страницы.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-125">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="ed2bc-126">Минификации</span><span class="sxs-lookup"><span data-stu-id="ed2bc-126">Minification</span></span>

<span data-ttu-id="ed2bc-127">Минификации выполняет различные оптимизации различных кода для уменьшения размера запрошенных ресурсов (например, CSS, изображения, файлы JavaScript).</span><span class="sxs-lookup"><span data-stu-id="ed2bc-127">Minification performs a variety of different code optimizations to reduce the size of requested assets (such as CSS, images, JavaScript files).</span></span> <span data-ttu-id="ed2bc-128">Общие результаты минификации включают Удаление лишних пробелов и комментарии и сокращать имена переменных в один символ.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-128">Common results of minification include removing unnecessary white space and comments, and shortening variable names to one character.</span></span>

<span data-ttu-id="ed2bc-129">Рассмотрим следующую функцию JavaScript:</span><span class="sxs-lookup"><span data-stu-id="ed2bc-129">Consider the following JavaScript function:</span></span>

```javascript
AddAltToImg = function (imageTagAndImageID, imageContext) {
  ///<signature>
  ///<summary> Adds an alt tab to the image
  // </summary>
  //<param name="imgElement" type="String">The image selector.</param>
  //<param name="ContextForImage" type="String">The image context.</param>
  ///</signature>
  var imageElement = $(imageTagAndImageID, imageContext);
  imageElement.attr('alt', imageElement.attr('id').replace(/ID/, ''));
}
```

<span data-ttu-id="ed2bc-130">После минификации функция сокращается до следующее:</span><span class="sxs-lookup"><span data-stu-id="ed2bc-130">After minification, the function is reduced to the following:</span></span>

```javascript
AddAltToImg=function(t,a){var r=$(t,a);r.attr("alt",r.attr("id").replace(/ID/,""))};
```

<span data-ttu-id="ed2bc-131">Помимо удаление комментариев и ненужные пробелы, следующие параметры и имена переменных были переименованы (сокращено) следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ed2bc-131">In addition to removing the comments and unnecessary whitespace, the following parameters and variable names were renamed (shortened) as follows:</span></span>

<span data-ttu-id="ed2bc-132">До преобразования</span><span class="sxs-lookup"><span data-stu-id="ed2bc-132">Original</span></span> | <span data-ttu-id="ed2bc-133">Переименовано</span><span class="sxs-lookup"><span data-stu-id="ed2bc-133">Renamed</span></span>
--- | :---:
<span data-ttu-id="ed2bc-134">imageTagAndImageID</span><span class="sxs-lookup"><span data-stu-id="ed2bc-134">imageTagAndImageID</span></span> | <span data-ttu-id="ed2bc-135">т</span><span class="sxs-lookup"><span data-stu-id="ed2bc-135">t</span></span>
<span data-ttu-id="ed2bc-136">в пределах изображения</span><span class="sxs-lookup"><span data-stu-id="ed2bc-136">imageContext</span></span> | <span data-ttu-id="ed2bc-137">пример</span><span class="sxs-lookup"><span data-stu-id="ed2bc-137">a</span></span>
<span data-ttu-id="ed2bc-138">imageElement</span><span class="sxs-lookup"><span data-stu-id="ed2bc-138">imageElement</span></span> | <span data-ttu-id="ed2bc-139">r</span><span class="sxs-lookup"><span data-stu-id="ed2bc-139">r</span></span>

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="ed2bc-140">Влияние объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="ed2bc-140">Impact of bundling and minification</span></span>

<span data-ttu-id="ed2bc-141">В следующей таблице показаны несколько важных различий между все активы по отдельности и объединение и Минификация простой веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-141">The following table shows several important differences between listing all the assets individually and using bundling and minification on a simple web page:</span></span>

<span data-ttu-id="ed2bc-142">Действие</span><span class="sxs-lookup"><span data-stu-id="ed2bc-142">Action</span></span> | <span data-ttu-id="ed2bc-143">С B и M</span><span class="sxs-lookup"><span data-stu-id="ed2bc-143">With B/M</span></span> | <span data-ttu-id="ed2bc-144">Без B и M</span><span class="sxs-lookup"><span data-stu-id="ed2bc-144">Without B/M</span></span> | <span data-ttu-id="ed2bc-145">Изменение</span><span class="sxs-lookup"><span data-stu-id="ed2bc-145">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="ed2bc-146">Запросы файлов</span><span class="sxs-lookup"><span data-stu-id="ed2bc-146">File Requests</span></span> |<span data-ttu-id="ed2bc-147">7</span><span class="sxs-lookup"><span data-stu-id="ed2bc-147">7</span></span> | <span data-ttu-id="ed2bc-148">18</span><span class="sxs-lookup"><span data-stu-id="ed2bc-148">18</span></span> | <span data-ttu-id="ed2bc-149">157%</span><span class="sxs-lookup"><span data-stu-id="ed2bc-149">157%</span></span>
<span data-ttu-id="ed2bc-150">Передать КБ</span><span class="sxs-lookup"><span data-stu-id="ed2bc-150">KB Transferred</span></span> | <span data-ttu-id="ed2bc-151">156</span><span class="sxs-lookup"><span data-stu-id="ed2bc-151">156</span></span> | <span data-ttu-id="ed2bc-152">264.68</span><span class="sxs-lookup"><span data-stu-id="ed2bc-152">264.68</span></span> | <span data-ttu-id="ed2bc-153">70%</span><span class="sxs-lookup"><span data-stu-id="ed2bc-153">70%</span></span>
<span data-ttu-id="ed2bc-154">Время загрузки (мс)</span><span class="sxs-lookup"><span data-stu-id="ed2bc-154">Load Time (MS)</span></span> | <span data-ttu-id="ed2bc-155">885</span><span class="sxs-lookup"><span data-stu-id="ed2bc-155">885</span></span> | <span data-ttu-id="ed2bc-156">2360</span><span class="sxs-lookup"><span data-stu-id="ed2bc-156">2360</span></span> | <span data-ttu-id="ed2bc-157">167%</span><span class="sxs-lookup"><span data-stu-id="ed2bc-157">167%</span></span>

<span data-ttu-id="ed2bc-158">Отправлено байт было значительное сокращение с объединением как браузеры довольно verbose с заголовки HTTP, которые применяются в запросах.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-158">The bytes sent had a significant reduction with bundling as browsers are fairly verbose with the HTTP headers that they apply on requests.</span></span> <span data-ttu-id="ed2bc-159">Время загрузки показывает значительным усовершенствованием по, однако в этом примере была запущена локально.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-159">The load time shows a big improvement, however this example was run locally.</span></span> <span data-ttu-id="ed2bc-160">Вы получите больше повысить производительность при помощи активы объединение и Минификация передаваемых по сети.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-160">You will get greater gains in performance when using bundling and minification with assets transferred over a network.</span></span>

## <a name="using-bundling-and-minification-in-a-project"></a><span data-ttu-id="ed2bc-161">С помощью объединение и Минификация в проекте</span><span class="sxs-lookup"><span data-stu-id="ed2bc-161">Using bundling and minification in a project</span></span>

<span data-ttu-id="ed2bc-162">Шаблон проекта MVC предоставляет `bundleconfig.json` файл конфигурации, который определяет параметры для каждого пакета.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-162">The MVC project template provides a `bundleconfig.json` configuration file which defines the options for each bundle.</span></span> <span data-ttu-id="ed2bc-163">По умолчанию, определенное конфигурации один комплект для пользовательским кодом JavaScript (`wwwroot/js/site.js`) и таблицы стилей (`wwwroot/css/site.css`) файлы.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-163">By default, a single bundle configuration is defined for the custom JavaScript (`wwwroot/js/site.js`) and Stylesheet (`wwwroot/css/site.css`) files.</span></span>

[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig.json)]

<span data-ttu-id="ed2bc-164">Параметры пакета:</span><span class="sxs-lookup"><span data-stu-id="ed2bc-164">Bundle options include:</span></span>

* <span data-ttu-id="ed2bc-165">outputFileName - имя файла пакета для вывода.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-165">outputFileName - name of the bundle file to output.</span></span> <span data-ttu-id="ed2bc-166">Может содержать относительный путь от `bundleconfig.json` файла.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-166">Can contain a relative path from the `bundleconfig.json` file.</span></span> <span data-ttu-id="ed2bc-167">**Обязательно**</span><span class="sxs-lookup"><span data-stu-id="ed2bc-167">**required**</span></span>
* <span data-ttu-id="ed2bc-168">inputFiles - массив файлов, которые будут объединены.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-168">inputFiles - array of files to bundle together.</span></span> <span data-ttu-id="ed2bc-169">Это относительные пути к файлу конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-169">These are relative paths to the configuration file.</span></span> <span data-ttu-id="ed2bc-170">**Необязательный**, * пустое значение преобразуется в пустой выходной файл.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-170">**optional**, *an empty value results in an empty output file.</span></span> <span data-ttu-id="ed2bc-171">[Этот режим](http://www.tldp.org/LDP/abs/html/globbingref.html) поддерживаются шаблоны.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-171">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="ed2bc-172">уменьшения - введите минификации параметры вывода.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-172">minify - minification options for the output type.</span></span> <span data-ttu-id="ed2bc-173">**Необязательный**, *по умолчанию —`minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="ed2bc-173">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="ed2bc-174">Параметры конфигурации доступны на тип выходного файла.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-174">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="ed2bc-175">Уменьшитель CSS</span><span class="sxs-lookup"><span data-stu-id="ed2bc-175">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="ed2bc-176">Уменьшитель JavaScript</span><span class="sxs-lookup"><span data-stu-id="ed2bc-176">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
    * [<span data-ttu-id="ed2bc-177">Уменьшитель HTML</span><span class="sxs-lookup"><span data-stu-id="ed2bc-177">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="ed2bc-178">includeInProject - добавить созданные файлы в файл проекта.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-178">includeInProject - add generated files to project file.</span></span> <span data-ttu-id="ed2bc-179">**Необязательный**, *по умолчанию - false*</span><span class="sxs-lookup"><span data-stu-id="ed2bc-179">**optional**, *default - false*</span></span>
* <span data-ttu-id="ed2bc-180">Сопоставления - формирования сопоставлений источника для объединенный файл.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-180">sourceMaps - generate source maps for the bundled file.</span></span> <span data-ttu-id="ed2bc-181">**Необязательный**, *по умолчанию - false*</span><span class="sxs-lookup"><span data-stu-id="ed2bc-181">**optional**, *default - false*</span></span>

### <a name="visual-studio-2015--2017"></a><span data-ttu-id="ed2bc-182">Visual Studio 2015 / 2017</span><span class="sxs-lookup"><span data-stu-id="ed2bc-182">Visual Studio 2015 / 2017</span></span>

<span data-ttu-id="ed2bc-183">Откройте `bundleconfig.json` в Visual Studio, если среде не имеет расширение установлено; запрос выводится что предполагает отсутствие, может помочь с этим типом файла.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-183">Open `bundleconfig.json` in Visual Studio, if your environment does not have the extension installed; a prompt is presented suggesting that there is one that could assist with this file type.</span></span>

![Расширение BuildBundlerMinifier предложений](../client-side/bundling-and-minification/_static/bundler-extension-suggestion.png)

<span data-ttu-id="ed2bc-185">Выберите представление расширений и установите **Bundler & Уменьшитель** расширения (Visual Studio, требуется перезагрузка).</span><span class="sxs-lookup"><span data-stu-id="ed2bc-185">Select View Extensions, and install the **Bundler & Minifier** extension (Requires Visual Studio restart).</span></span>

![Расширение BuildBundlerMinifier предложений](../client-side/bundling-and-minification/_static/view-extension.png)

<span data-ttu-id="ed2bc-187">После завершения перезагрузки необходимо настроить сборку для запуска процессов минификации и объединения клиентских средств.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-187">When the restart is complete, you need to configure the build to run the processes of minifying and bundling the client-side assets.</span></span> <span data-ttu-id="ed2bc-188">Щелкните правой кнопкой мыши `bundleconfig.json` файла и выберите *Enable пакета на сборки... *.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-188">Right-click the `bundleconfig.json` file and select *Enable bundle on build...*.</span></span>

<span data-ttu-id="ed2bc-189">Выполните построение проекта и `bundleconfig.json` включается в процесс построения для создания выходных файлов, на основе конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-189">Build the project, and the `bundleconfig.json` is included in the build process to produce the output files based on the configuration.</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierExample, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierExample -> C:\BuildBundlerMinifierExample\bin\Debug\netcoreapp1.1\BuildBundlerMinifierExample.dll
========== Build: 1 succeeded or up-to-date, 0 failed, 0 skipped ==========
```

### <a name="visual-studio-code-or-command-line"></a><span data-ttu-id="ed2bc-190">Код Visual Studio или командной строки</span><span class="sxs-lookup"><span data-stu-id="ed2bc-190">Visual Studio Code or Command Line</span></span>

<span data-ttu-id="ed2bc-191">Visual Studio и расширение управления процессом объединение и Минификация, с помощью жестов графического пользовательского интерфейса; Однако те же возможности доступны с `dotnet` CLI и BuildBundlerMinifier NuGet пакет.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-191">Visual Studio and the extension drive the bundling and minification process using GUI gestures; however, the same capabilities are available with the `dotnet` CLI and BuildBundlerMinifier NuGet package.</span></span>

<span data-ttu-id="ed2bc-192">Добавьте пакет NuGet в проект.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-192">Add the NuGet package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="ed2bc-193">Восстановление зависимости.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-193">Restore the dependencies:</span></span>

```console
dotnet restore
```

<span data-ttu-id="ed2bc-194">Построение приложения:</span><span class="sxs-lookup"><span data-stu-id="ed2bc-194">Build the app:</span></span>

```console
dotnet build
```

<span data-ttu-id="ed2bc-195">Выходные данные команды построения показаны результаты минификации и/или объединении в соответствии с настроенным.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-195">The output from the build command shows the results of the minification and/or bundling according to what is configured.</span></span>

```console
Microsoft (R) Build Engine version 15.1.545.13942
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Begin processing bundleconfig.json
     Minified wwwroot/css/site.min.css
  Bundler: Done processing bundleconfig.json
  BuildBundlerMinifierExample -> /BuildBundlerMinifierExample/bin/Debug/netcoreapp1.0/BuildBundlerMinifierExample.dll
```

## <a name="adding-files"></a><span data-ttu-id="ed2bc-196">Добавление файлов</span><span class="sxs-lookup"><span data-stu-id="ed2bc-196">Adding files</span></span>

<span data-ttu-id="ed2bc-197">В этом примере добавляется вызываемой дополнительный файл CSS `custom.css` и настроен для объединение и Минификация с `site.css`, полученный в одном `site.min.css`.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-197">In this example, an additional CSS file is added called `custom.css` and configured for bundling and minification with `site.css`, resulting in a single `site.min.css`.</span></span>

<span data-ttu-id="ed2bc-198">Custom.CSS</span><span class="sxs-lookup"><span data-stu-id="ed2bc-198">custom.css</span></span>

```css
.about, [role=main], [role=complementary]
{
    margin-top: 60px;
}

footer
{
    margin-top: 10px;
}
```

<span data-ttu-id="ed2bc-199">Добавьте относительный путь к `bundleconfig.json`.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-199">Add the relative path to `bundleconfig.json`.</span></span>

[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig2.json)]

> [!NOTE]
> <span data-ttu-id="ed2bc-200">Кроме того, можно использовать шаблон глобализацию - `"inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]` получающий CSS все файлы и исключает шаблон уменьшенный файла.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-200">Alternatively, the globbing pattern could be used - `"inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]` which gets all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="ed2bc-201">Построение приложения и при открытии `site.min.css`, вы теперь Обратите внимание, что содержимое `custom.css` добавляются в конец файла.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-201">Build the application and if you open `site.min.css`, you'll now notice that contents of `custom.css` has been appended to the end of the file.</span></span>

## <a name="controlling-bundling-and-minification"></a><span data-ttu-id="ed2bc-202">Управление объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="ed2bc-202">Controlling bundling and minification</span></span>

<span data-ttu-id="ed2bc-203">Как правило вы хотите использовать пакетные и уменьшенный файлы приложения только в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-203">In general, you want to use the bundled and minified files of your app only in a production environment.</span></span> <span data-ttu-id="ed2bc-204">Во время разработки вы хотите использовать исходные файлы, чтобы упростить процесс отладки приложения.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-204">During development, you want to use your original files so your app is easier to debug.</span></span>

<span data-ttu-id="ed2bc-205">Можно указать, какие сценарии и файлы CSS для включения в использование вспомогательного метода тега среды на страницах макета страниц (в разделе [вспомогательных функций тегов](../mvc/views/tag-helpers/index.md)).</span><span class="sxs-lookup"><span data-stu-id="ed2bc-205">You can specify which scripts and CSS files to include in your pages using the environment tag helper in your layout pages (see [Tag Helpers](../mvc/views/tag-helpers/index.md)).</span></span> <span data-ttu-id="ed2bc-206">При запуске в определенных средах вспомогательный тега среды будет отображать только его содержимое.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-206">The environment tag helper will only render its contents when running in specific environments.</span></span> <span data-ttu-id="ed2bc-207">В разделе [работа с несколькими средами](../fundamentals/environments.md) подробные сведения об указании текущей среды.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-207">See [Working with Multiple Environments](../fundamentals/environments.md) for details on specifying the current environment.</span></span>

<span data-ttu-id="ed2bc-208">Следующие теги среды будет отображаться необработанных файлов CSS, при работе в `Development` среде:</span><span class="sxs-lookup"><span data-stu-id="ed2bc-208">The following environment tag will render the unprocessed CSS files when running in the `Development` environment:</span></span>

[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=3&range=9-12)]

<span data-ttu-id="ed2bc-209">Этот тег среды будет отображаться распространение и уменьшенный CSS-файл только при работе в `Production` или `Staging`:</span><span class="sxs-lookup"><span data-stu-id="ed2bc-209">This environment tag will render the bundled and minified CSS files only when running in `Production` or `Staging`:</span></span>

[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=5&range=13-18)]

## <a name="consuming-bundleconfigjson-from-gulp"></a><span data-ttu-id="ed2bc-210">Использование bundleconfig.json из Gulp</span><span class="sxs-lookup"><span data-stu-id="ed2bc-210">Consuming bundleconfig.json from Gulp</span></span>

<span data-ttu-id="ed2bc-211">Если рабочий процесс приложения объединение и Минификация требуются дополнительные действия, такие как обработка изображений, busting кэша, обработки assest CDN, т. д., можно преобразовать в процессе пакета и Minify в Gulp.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-211">If your app bundling and minification workflow requires additional processes such as image processing, cache busting, CDN assest processing, etc., then you can convert the Bundle and Minify process to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="ed2bc-212">Преобразование параметр доступен только в Visual Studio 2015 и 2017 г.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-212">Conversion option only available in Visual Studio 2015 and 2017.</span></span>

<span data-ttu-id="ed2bc-213">Щелкните правой кнопкой мыши `bundleconfig.json` и выберите **преобразовать Gulp... **. Это создаст `gulpfile.js` и установите пакеты необходимых npm.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-213">Right-click the `bundleconfig.json` and select **Convert to Gulp...**. This will generate the `gulpfile.js` and install the necessary npm packages.</span></span>

![Преобразовать в Gulp](../client-side/bundling-and-minification/_static/convert-togulp.png)

<span data-ttu-id="ed2bc-215">`gulpfile.js` Полученных операций чтения `bundleconfig.json` файла конфигурации, поэтому он может продолжать использоваться для ввода вывода и параметры.</span><span class="sxs-lookup"><span data-stu-id="ed2bc-215">The `gulpfile.js` produced reads the `bundleconfig.json` file for the configuration, therefore it can continue to be used for the inputs/outputs and settings.</span></span>

[!code-javascript[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/gulpfile.js)]

<span data-ttu-id="ed2bc-216">Чтобы включить Gulp при построении проекта в Visual Studio 2017 г, добавьте следующий файл *.csproj:</span><span class="sxs-lookup"><span data-stu-id="ed2bc-216">To enable Gulp when the project builds in Visual Studio 2017, add the following to the *.csproj file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
    <Exec Command="gulp min" />
</Target>
```

<span data-ttu-id="ed2bc-217">Чтобы включить Gulp при построении проекта в Visual Studio 2015, добавьте следующий код в `project.json` файла:</span><span class="sxs-lookup"><span data-stu-id="ed2bc-217">To enable Gulp when the project builds in Visual Studio 2015, add the following to the `project.json` file:</span></span>

```json
"scripts": {
    "precompile": "gulp min"
}
```

## <a name="additional-resources"></a><span data-ttu-id="ed2bc-218">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ed2bc-218">Additional resources</span></span>

* [<span data-ttu-id="ed2bc-219">Использование Gulp</span><span class="sxs-lookup"><span data-stu-id="ed2bc-219">Using Gulp</span></span>](using-gulp.md)
* [<span data-ttu-id="ed2bc-220">Использование Grunt</span><span class="sxs-lookup"><span data-stu-id="ed2bc-220">Using Grunt</span></span>](using-grunt.md)
* [<span data-ttu-id="ed2bc-221">Работа с несколькими средами</span><span class="sxs-lookup"><span data-stu-id="ed2bc-221">Working with Multiple Environments</span></span>](../fundamentals/environments.md)
* [<span data-ttu-id="ed2bc-222">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="ed2bc-222">Tag Helpers</span></span>](../mvc/views/tag-helpers/index.md)
