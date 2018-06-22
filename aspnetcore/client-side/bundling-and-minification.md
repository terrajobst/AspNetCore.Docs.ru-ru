---
title: Пакет и minifiy статических активы ASP.NET Core
author: scottaddie
description: Сведения об оптимизации статические ресурсы в веб-приложение ASP.NET Core, применяя объединение и Минификация методы.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2018
uid: client-side/bundling-and-minification
ms.openlocfilehash: ae9836a6ad0ff0bc834bf2eb10ff5fd97c3c659a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279575"
---
# <a name="bundle-and-minifiy-static-assets-in-aspnet-core"></a>Пакет и minifiy статических активы ASP.NET Core

Автор: [Скотт Адди](https://twitter.com/Scott_Addie) (Scott Addie)

В этой статье объясняется выгода применения объединение и Минификация, включая использование этих функций с веб-приложений ASP.NET Core.

## <a name="what-is-bundling-and-minification"></a>Что такое объединение и Минификация

Объединении и Минификация являются два способа оптимизации производительности для применения в веб-приложения. Использовать совместно, объединение и Минификация повышения производительности путем сокращения числа запросов к серверу и уменьшения размера запрошенных ресурсов статический.

Объединение и Минификация главным образом улучшить время загрузки первого запроса страницы. После запросил веб-страницы браузер кэширует статические активы (JavaScript, CSS и изображения). Следовательно объединение и Минификация не повысить производительность при запросе же страницу или страницы на одном сайте, запрашивает же активы. Если срок действия истекает заголовка указано неверно по средствам и если объединение и Минификация не используется, эвристики актуальность браузера пометить активы устаревших через несколько дней. Кроме того браузеру запроса на проверку для каждого ресурса. В этом случае объединение и Минификация обеспечивают более высокую производительность даже после первого запроса страницы.

### <a name="bundling"></a>Объединение

Объединение объединяет несколько файлов в один файл. Объединение уменьшает количество запросов сервера, необходимых для отрисовки на web средства, такие как веб-страницы. Можно создать любое количество отдельных наборов специально для CSS, JavaScript и т. д. Меньшее число файлов означает меньшее количество HTTP-запросов из браузера на сервер или службой, предоставляющей приложение. Результатом является повышение производительности загрузки первой страницы.

### <a name="minification"></a>Минификации

Минификации удаляет ненужные символы из кода без изменения функциональности. Результатом является уменьшение значительного размера запрошенных ресурсов (например, CSS, изображения и файлы JavaScript). Распространенные побочные эффекты минификации включают сокращение имена переменных в один символ и удаление комментариев и ненужные пробелы.

Рассмотрим следующую функцию JavaScript:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

Иными уменьшает функция следующее:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

Помимо удаление комментариев и ненужные пробелы, следующие имена параметра и переменной изменены следующим образом:

До преобразования | Переименовано
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a>Влияние объединение и Минификация

В следующей таблице приведены различия между по отдельности загрузка средств и использование объединение и Минификация:

Действие | С B и M | Без B и M | Изменение
--- | :---: | :---: | :---:
Запросы файлов  | 7   | 18     | 157%
Передать КБ | 156 | 264.68 | 70%
Время загрузки (мс) | 885 | 2360   | 167%

Обозреватели являются достаточно подробных сведений по отношению к заголовков HTTP-запроса. Всего байт отправлено метрика видели значительное сокращение при объединении. Время загрузки показывает значительные улучшения, однако в этом примере запускался локально. Больше повысить производительность реализуются при помощи активы объединение и Минификация передаваемых по сети.

## <a name="choose-a-bundling-and-minification-strategy"></a>Выбор стратегии объединение и Минификация

Шаблоны проектов MVC и страниц Razor и предоставления решения out of box для объединение и Минификация, состоящий из файла конфигурации JSON. Сторонние средства, такие как [Gulp](xref:client-side/using-gulp) и [Grunt](xref:client-side/using-grunt) задачи средства запуска, выполнения тех же задач с немного сложнее. Средства стороннего является отлично подходит при разработке рабочего процесса требуется обработка за объединение и Минификация&mdash;например оптимизация linting и изображения. С помощью разработки объединение и Минификация, уменьшенный файлы создаются до развертывания приложения. Объединение и Минификация перед развертыванием предоставляет преимущество снижение нагрузки. Однако следует отметить, что объединение во время разработки и иными увеличивает сложность сборки и работает только с статических файлов.

## <a name="configure-bundling-and-minification"></a>Настроить объединение и Минификация

Шаблоны проектов MVC и страниц Razor содержат *bundleconfig.json* файл конфигурации, который определяет параметры для каждого пакета. По умолчанию, определенное конфигурации один комплект для пользовательским кодом JavaScript (*wwwroot/js/site.js*) и таблицы стилей (*wwwroot/css/site.css*) файлов:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

Следующие параметры конфигурации.

* `outputFileName`: Имя файла набора для вывода. Может содержать относительный путь от *bundleconfig.json* файла. **Обязательно**
* `inputFiles`: Массив файлов, которые будут объединены. Это относительные пути к файлу конфигурации. **Необязательный**, * пустое значение преобразуется в пустой выходной файл. [Этот режим](http://www.tldp.org/LDP/abs/html/globbingref.html) поддерживаются шаблоны.
* `minify`: Параметры минификации тип выходных данных. **Необязательный**, *по умолчанию — `minify: { enabled: true }`*
  * Параметры конфигурации доступны на тип выходного файла.
    * [Уменьшитель CSS](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [Уменьшитель JavaScript](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [Уменьшитель HTML](https://github.com/madskristensen/BundlerMinifier/wiki)
* `includeInProject`: Флаг, указывающий, добавьте в файл проекта созданные файлы. **Необязательный**, *по умолчанию - false*
* `sourceMap`: Флаг, указывающий, сформируйте карту источника для объединенный файл. **Необязательный**, *по умолчанию - false*
* `sourceMapRootPath`: Корневой путь для сохранения карты сформированном исходном файле.

## <a name="build-time-execution-of-bundling-and-minification"></a>Объединение и Минификация выполнения во время сборки

[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) пакет NuGet разрешает выполнение объединение и Минификация во время построения. Внедряет пакет [целевые объекты MSBuild](/visualstudio/msbuild/msbuild-targets) которого сборки и очистить времени выполнения. *Bundleconfig.json* файл анализируется в процессе построения для создания выходных файлов, на основе определенной конфигурации.

> [!NOTE]
> BuildBundlerMinifier относится к проекту сообщество ведет на GitHub, для которых корпорация Майкрософт не поддерживает. Должна быть заполнена проблемы [здесь](https://github.com/madskristensen/BundlerMinifier/issues).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Добавить *BuildBundlerMinifier* пакета в проект.

Выполните построение проекта. В окне «Вывод» отображаются следующие сведения.

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

Очистите проект. В окне «Вывод» отображаются следующие сведения.

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

При использовании ASP.NET Core 1.x, восстановите добавленные пакеты:

```console
dotnet restore
```

Постройте проект:

```console
dotnet build
```

Отображаются следующие сведения.

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

Появляется следующий результат:

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a>Выполнение нерегламентированных объединение и Минификация

Это можно выполнить задачи объединение и Минификация нерегламентированные, без построения проекта. Добавить [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) пакет NuGet для проекта:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> BundlerMinifier.Core относится к проекту сообщество ведет на GitHub, для которых корпорация Майкрософт не поддерживает. Должна быть заполнена проблемы [здесь](https://github.com/madskristensen/BundlerMinifier/issues).

Этот пакет расширяет .NET Core CLI для включения *dotnet пакета* средства. В окне консоли диспетчера пакетов (PMC) или в командной оболочке можно выполнить следующую команду:

```console
dotnet bundle
```

> [!IMPORTANT]
> Диспетчер пакетов NuGet добавляет зависимости файл *.csproj в качестве `<PackageReference />` узлов. `dotnet bundle` Команда зарегистрирован .NET Core CLI только если `<DotNetCliToolReference />` используется узел. Измените файл *.csproj соответствующим образом.

## <a name="add-files-to-workflow"></a>Добавление файлов в рабочий процесс

Рассмотрим пример, в котором еще *custom.css* будет добавлен файл, аналогичный следующему:

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

Для уменьшения *custom.css* и объединить его с *site.css* в *site.min.css* файл, добавьте относительный путь к *bundleconfig.json*:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> Кроме того может использоваться следующий шаблон этот режим:
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> Этот шаблон глобализацию поиск всех файлов CSS и исключает уменьшенный файла шаблона.

Постройте приложение. Откройте *site.min.css* и обратите внимание, содержимое *custom.css* добавляется в конец файла.

## <a name="environment-based-bundling-and-minification"></a>На основе среды объединение и Минификация

Рекомендуется распространение и уменьшенный файлы приложения следует использовать в рабочей среде. Во время разработки исходные файлы убедитесь, чтобы упростить отладку приложения.

Указать, какие файлы для включения в страницы с помощью [вспомогательный тега среды](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) в представления. Вспомогательный тега среды Подготовка содержимого только при работе в конкретных [средах](xref:fundamentals/environments).

Следующие `environment` тег отображает необработанных файлов CSS при работе в `Development` среде:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

---

Следующие `environment` тег отображает распространение и уменьшенный CSS-файл при выполнении в среде, отличный от `Development`. Например, на котором работают `Production` или `Staging` триггеры для подготовки к просмотру эти таблицы стилей:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

---

## <a name="consume-bundleconfigjson-from-gulp"></a>Использовать bundleconfig.json из Gulp

Существуют случаи, в которых приложение объединение и Минификация рабочего процесса требуется дополнительная обработка. Примеры Оптимизация изображения, busting кэша и обработка активов CDN. Чтобы удовлетворить этим требованиям, можно преобразовать объединение и Минификация рабочий процесс для использования Gulp.

### <a name="use-the-bundler--minifier-extension"></a>Использовать расширение Bundler & Уменьшитель

Visual Studio [Bundler & Уменьшитель](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) расширения выполняет преобразование на Gulp.

> [!NOTE]
> Расширение Bundler & Уменьшитель принадлежит сообщество ведет проекта на GitHub, для которых корпорация Майкрософт не поддерживает. Должна быть заполнена проблемы [здесь](https://github.com/madskristensen/BundlerMinifier/issues).

Щелкните правой кнопкой мыши *bundleconfig.json* в обозревателе решений и выберите **Bundler & Уменьшитель** > **преобразовать Gulp...** :

![Преобразовать Gulp пункт контекстного меню](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

*Gulpfile.js* и *package.json* файлы добавляются в проект. Поддержка [npm](https://www.npmjs.com/) пакеты, перечисленные в *package.json* файла `devDependencies` устанавливаются раздела.

Выполните следующую команду в окне PMC установить Gulp CLI в качестве глобального зависимостей:

```console
npm i -g gulp-cli
```

*Gulpfile.js* операции чтения файла *bundleconfig.json* файл для входов, выходов и параметры.

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a>Преобразовать вручную

Если Visual Studio или расширение Bundler & Уменьшитель недоступны, преобразуйте вручную.

Добавить *package.json* файл со следующими `devDependencies`, в корне проекта:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

Установить зависимости, выполнив следующую команду на том же уровне, как *package.json*:

```console
npm i
```

Установите Gulp CLI как глобальные зависимостей:

```console
npm i -g gulp-cli
```

Копировать *gulpfile.js* файла ниже в корне проекта:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a>Выполнение задач Gulp

Для запуска задачи минификации Gulp до сборки проекта в Visual Studio, добавьте следующий [MSBuild: целевая](/visualstudio/msbuild/msbuild-targets) *.csproj файл:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

В этом примере все задачи, определенные в `MyPreCompileTarget` целевые запуска перед стандартных `Build` цели. В окне вывода Visual Studio появляется результат, аналогичный приведенному ниже:

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

Кроме того Visual Studio Task Runner Explorer может использоваться для связи Gulp задач для конкретных событий в Visual Studio. В разделе [выполнения задач по умолчанию](xref:client-side/using-gulp#running-default-tasks) инструкции по этим способом.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Использование Gulp](xref:client-side/using-gulp)
* [Использование Grunt](xref:client-side/using-grunt)
* [Использование нескольких сред](xref:fundamentals/environments)
* [Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro)
