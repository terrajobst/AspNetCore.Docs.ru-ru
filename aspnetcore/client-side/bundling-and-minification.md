---
title: Объединение и Минификация статических ресурсов в ASP.NET Core
author: scottaddie
description: Узнайте, как оптимизировать статические ресурсы в веб-приложении ASP.NET Core, применяя методы объединения и минификации.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/17/2019
uid: client-side/bundling-and-minification
ms.openlocfilehash: c3ec67d9f5c2b8eb44b5ced911ccfd0fa40a52a7
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/17/2019
ms.locfileid: "67152778"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a>Объединение и Минификация статических ресурсов в ASP.NET Core

По [Scott Addie](https://twitter.com/Scott_Addie) и [сосну Дэвид](https://twitter.com/davidpine7)

В этой статье описаны преимущества применения объединения и минификации, включая использование этих функций с веб-приложений ASP.NET Core.

## <a name="what-is-bundling-and-minification"></a>Что такое объединение и Минификация

Объединение и Минификация являются два различных производительности оптимизации, которые можно применить в веб-приложения. Использовать совместно, объединение и Минификация повысить производительность, сокращение количества запросов к серверу и уменьшения размера запрошенных статических ресурсов.

Объединение и Минификация в основном улучшить время загрузки первого запроса страницы. Когда запрошен веб-страницы, обозреватель кэширует статических ресурсов (JavaScript, CSS и изображений). Следовательно объединение и Минификация не повысить производительность при запросе на одной странице, то есть страницы, на том же сайте, запрашивает эти ресурсы. Если срок действия истекает заголовка указано неверно по средствам и, если не используется объединение и Минификация, эвристики актуальность браузера пометьте ресурсы устаревших через несколько дней. Кроме того браузер требует запроса на проверку для каждого ресурса. В этом случае объединение и Минификация улучшает производительность даже после первого запроса страницы.

### <a name="bundling"></a>Объединение

Объединение позволяет объединить несколько файлов в один файл. Объединение сокращает число запросов к серверу, которые необходимы для подготовки к просмотру web активов, таких как веб-страницы. Можно создать любое количество отдельных наборов специально для CSS, JavaScript и т. д. Меньшее количество файлов означает меньшее количество HTTP-запросы из браузера на сервер или службой, предоставляющей приложение. В результате повышение производительности загрузки первой страницы.

### <a name="minification"></a>Минификация

Минификация удаляет ненужные символы из кода, не затрагивая их функциональность. Результатом является снижение большого размера запрошенных ресурсов (например, CSS, изображения и файлы JavaScript). Распространенные побочные эффекты минификации включают сокращение имен переменных в один символ и удаление комментариев и лишних пробелов.

Рассмотрим следующую функцию JavaScript:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

Минификация уменьшает функция следующее:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

Помимо удаления комментариев и ненужные пробелы, следующие имена параметра и переменной изменены следующим образом:

До преобразования | Переименовано
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a>Влияние объединение и Минификация

В следующей таблице приведены различия между по отдельности загрузка ресурсов и с помощью объединения и минификации:

Действие | С B в минуту | Без B в минуту | Изменение
--- | :---: | :---: | :---:
Файл запросов  | 7   | 18     | 157%
Передано КБ | 156 | 264.68 | 70%
Время загрузки (мс) | 885 | 2360   | 167%

Обозреватели являются довольно подробного по отношению к заголовков HTTP-запроса. Общее количество байтов отправлено метрика видели значительное сокращение при объединении. Время загрузки показано значительное улучшение, однако в этом примере выполнялись локально. Большей производительности реализуются в том случае, когда с помощью объединения и минификации с активами, передаваемых по сети.

## <a name="choose-a-bundling-and-minification-strategy"></a>Выберите стратегию объединение и Минификация

Шаблоны проектов MVC и Razor Pages предоставить решение out of box для объединения и минификации, состоящий из JSON-файлу конфигурации. Сторонние средства, такие как [Grunt](xref:client-side/using-grunt) задач запуска, выполнения аналогичных задач немного более сложных. Сторонний инструмент является прекрасным решением при рабочем процессе разработки требуется обработка за объединение и Минификация&mdash;например оптимизации выделения и изображения. С помощью разработки объединения и минификации, минифицированные файлы создаются перед развертыванием приложения. Объединение и Минификация перед развертыванием предоставляет преимущество нагрузки сокращению объема сервера. Тем не менее очень важно для распознавания, во время разработки объединение и Минификация увеличивает сложность сборки и работает только со статическими файлами.

## <a name="configure-bundling-and-minification"></a>Настроить объединение и Минификация

::: moniker range="<= aspnetcore-2.0"

В ASP.NET Core 2.0 или более ранней версии, предоставляют шаблоны проектов MVC и Razor Pages *bundleconfig.json* файл конфигурации, который определяет параметры для каждого пакета:

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

В ASP.NET Core 2.1 или более поздней версии, добавьте новый файл JSON с именем *bundleconfig.json*, в корень проекта MVC и Razor Pages. Включите приведенный ниже код JSON в этот файл в качестве отправной точки:

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

*Bundleconfig.json* файл определяет параметры для каждого пакета. В приведенном выше примере конфигурации одного пакета определяется пользовательский код JavaScript (*wwwroot/js/site.js*) и таблицы стилей (*wwwroot/css/site.css*) файлы.

Варианты конфигурации:

* `outputFileName`: Имя файла пакета для вывода. Может содержать относительный путь от *bundleconfig.json* файла. **Обязательно**
* `inputFiles`: Массив файлов, чтобы объединить. Это относительные пути к файлу конфигурации. **Необязательный**, * пустое значение приводит к пустой выходной файл. [Этот режим](http://www.tldp.org/LDP/abs/html/globbingref.html) поддерживаются шаблоны.
* `minify`: Параметры минификации тип выходных данных. **Необязательный**, *по умолчанию — `minify: { enabled: true }`*
  * Параметры конфигурации доступны на тип выходного файла.
    * [Уменьшитель CSS](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [Уменьшитель JavaScript](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [Уменьшитель HTML](https://github.com/madskristensen/BundlerMinifier/wiki)
* `includeInProject`: Флаг, указывающий, следует ли добавить созданные файлы в файл проекта. **Необязательный**, *по умолчанию - false*
* `sourceMap`: Флаг, указывающий, следует ли создавать карту источника для объединенный файл. **Необязательный**, *по умолчанию - false*
* `sourceMapRootPath`: Корневой путь для хранения созданного исходного файла карты.

## <a name="build-time-execution-of-bundling-and-minification"></a>Выполнение во время сборки объединение и Минификация

[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) выполнения объединения и минификации во время сборки с помощью пакета NuGet. Внедряет пакет [целевых объектов MSBuild](/visualstudio/msbuild/msbuild-targets) которого сборки и чистой времени выполнения. *Bundleconfig.json* файла анализируется в процессе построения для создания выходных файлов, на основе определенной конфигурации.

> [!NOTE]
> BuildBundlerMinifier принадлежит проекту сообщества на сайте GitHub, для которой корпорация Майкрософт не поддерживает. Должна быть заполнена проблемы [здесь](https://github.com/madskristensen/BundlerMinifier/issues).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Добавить *BuildBundlerMinifier* пакета в проект.

Выполните построение проекта. В окне вывода отображаются следующие сведения:

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

Очистите проект. В окне вывода отображаются следующие сведения:

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Добавить *BuildBundlerMinifier* пакета в проект:

```console
dotnet add package BuildBundlerMinifier
```

При использовании ASP.NET Core 1.x, восстановить вновь добавленный пакет:

```console
dotnet restore
```

Постройте проект:

```console
dotnet build
```

Отображаются следующие сведения:

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

Очистите проект:

```console
dotnet clean
```

Появится следующий результат:

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a>Выполнение ad-hoc объединение и Минификация

Это можно выполнить объединение и Минификация задачи на основе ad-hoc без построения проекта. Добавить [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) свой проект пакет NuGet:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> BundlerMinifier.Core принадлежит проекту сообщества на сайте GitHub, для которой корпорация Майкрософт не поддерживает. Должна быть заполнена проблемы [здесь](https://github.com/madskristensen/BundlerMinifier/issues).

Этот пакет расширяет CLI .NET Core для включения *пакета dotnet* средство. В окне консоли диспетчера пакетов (PMC) или в командной строке можно выполнить следующую команду:

```console
dotnet bundle
```

> [!IMPORTANT]
> Диспетчер пакетов NuGet добавляет зависимости в файл *.csproj как `<PackageReference />` узлов. `dotnet bundle` Команда регистрируется с помощью .NET Core CLI только тогда, когда `<DotNetCliToolReference />` используется узел. Измените файл *.csproj соответствующим образом.

## <a name="add-files-to-workflow"></a>Добавить файлы в рабочий процесс

Рассмотрим пример, в котором еще *custom.css* добавляется файл, аналогичный следующему:

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

Для уменьшения *custom.css* и объединять его с *site.css* в *site.min.css* добавьте относительный путь к *bundleconfig.json*:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> Кроме того можно использовать следующие маски:
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css))"]
> ```
>
> Этот шаблон глобализации поиск всех файлов CSS и исключает шаблон минифицированные файла.

Постройте приложение. Откройте *site.min.css* и обратите внимание, что содержимое *custom.css* добавляется в конец файла.

## <a name="environment-based-bundling-and-minification"></a>На основе среды — объединение и Минификация

Рекомендуется объединенные в пакет и минифицированные файлов приложения должны использоваться в рабочей среде. Во время разработки исходные файлы сделать для упрощения отладки приложения.

Указать, какие файлы для включения в страницы с помощью [вспомогательная функция тега среды](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) в представлениях. Вспомогательная функция тега среды Подготовка содержимого только при работе в конкретных [сред](xref:fundamentals/environments).

Следующие `environment` отображения тега необработанных файлов CSS, при работе в `Development` среды:

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

Следующие `environment` отображения тега объединенные в пакет и минифицированные CSS-файл, при работе в среде, отличное от `Development`. Например, на котором работают `Production` или `Staging` инициирует отрисовку эти таблицы стилей:

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a>Использовать bundleconfig.json из Gulp

Бывают случаи, в которых приложения рабочего процесса объединения и минификации требуется дополнительная обработка. Примеры включают оптимизации изображения, отключения кэша и обработки ресурсов CDN. Для выполнения этих требований, можно преобразовать объединения и минификации рабочего процесса с помощью средства Gulp.

### <a name="use-the-bundler--minifier-extension"></a>Использование расширения Bundler & Minifier

В Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) расширение обрабатывает преобразование в Gulp.

> [!NOTE]
> Расширение Bundler & Minifier принадлежит проекту сообщества на сайте GitHub, для которой корпорация Майкрософт не поддерживает. Должна быть заполнена проблемы [здесь](https://github.com/madskristensen/BundlerMinifier/issues).

Щелкните правой кнопкой мыши *bundleconfig.json* в обозревателе решений и выберите **Bundler & Minifier** > **преобразовать для инструмента Gulp...** :

![Преобразовать для инструмента Gulp пункт контекстного меню](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

*Gulpfile.js* и *package.json* файлы добавляются в проект. Поддержка [npm](https://www.npmjs.com/) пакеты, перечисленные в *package.json* файла `devDependencies` разделе установлены.

Выполните следующую команду в окне консоли диспетчера пакетов для установки Gulp CLI в качестве глобального зависимость:

```console
npm i -g gulp-cli
```

*Gulpfile.js* файл операций чтения *bundleconfig.json* файл для входов, выходов и параметры.

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a>Преобразовать вручную

Если Visual Studio и (или) расширения Bundler & Minifier недоступны, преобразуйте вручную.

Добавить *package.json* файл со следующими `devDependencies`, в корневую папку проекта:

> [!WARNING]
> `gulp-uglify` Модуль не поддерживает ECMAScript (ES) 2015 / ES6 и более поздних версий. Установка [более четким ваш gulp](https://www.npmjs.com/package/gulp-terser) вместо `gulp-uglify` использовать ES2015 / ES6 или более поздней версии.

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

Установите зависимости, выполнив следующую команду на том же уровне, что *package.json*:

```console
npm i
```

Установите Gulp CLI в качестве глобального зависимости:

```console
npm i -g gulp-cli
```

Копировать *gulpfile.js* файл ниже в корневую папку проекта:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a>Выполнение задач Gulp

Чтобы запустить задачу Gulp, Минификация, до сборки проекта в Visual Studio, добавьте следующий код [целевой объект MSBuild](/visualstudio/msbuild/msbuild-targets) файл *.csproj:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

В этом примере задач, определенные в `MyPreCompileTarget` целевого выполнения перед предопределенный `Build` целевой объект. В окне вывода Visual Studio появится результат, аналогичный приведенному ниже:

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


## <a name="additional-resources"></a>Дополнительные ресурсы

* [Использование Grunt](xref:client-side/using-grunt)
* [Использование нескольких сред](xref:fundamentals/environments)
* [Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro)
