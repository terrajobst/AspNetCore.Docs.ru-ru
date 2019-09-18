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
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a>Объединять и уменьшение статические ресурсы в ASP.NET Core

[Скотт Эдди (](https://twitter.com/Scott_Addie) и [Дэвид сосна](https://twitter.com/davidpine7)

В этой статье объясняются преимущества применения объединения и минификации, в том числе сведения о том, как эти функции можно использовать с веб-приложениями ASP.NET Core.

## <a name="what-is-bundling-and-minification"></a>Что такое объединение и минификации

Объединение и минификации — это две различные оптимизации производительности, которые можно применить в веб-приложении. Совместное использование, объединение и минификации повышение производительности за счет сокращения числа запросов сервера и уменьшения размера запрошенных статических ресурсов.

Объединение и минификации в первую очередь повышает время загрузки первого запроса страницы. После запроса веб-страницы браузер кэширует статические ресурсы (JavaScript, CSS и изображения). Следовательно, объединение и минификации не улучшают производительность при запросе одной и той же страницы или страниц на одном сайте, запрашивающем одни и те же активы. Если заголовок Expires неправильно задан в ресурсах, а объединение и минификации не используются, эвристика обновления обозревателя помечает активы как устаревшие через несколько дней. Кроме того, браузер требует запрос проверки для каждого ресурса. В этом случае объединение и минификации обеспечивают повышение производительности даже после первого запроса страницы.

### <a name="bundling"></a>объединения

Объединение нескольких файлов в один файл. Объединение сокращает количество запросов к серверу, необходимых для подготовки к просмотру веб-ресурса, например веб-страницы. Можно создать любое количество отдельных пакетов специально для CSS, JavaScript и т. д. Меньшее число файлов означает меньшее количество HTTP-запросов от браузера к серверу или от службы, предоставляющей приложение. Это приводит к повышению производительности первой загрузки страницы.

### <a name="minification"></a>Минификации

Минификации удаляет ненужные символы из кода без изменения функциональности. В результате значительно уменьшается размер запрошенных ресурсов (например, CSS, изображений и файлов JavaScript). Обычные побочные эффекты минификации включают сокращение имен переменных в один символ и удаление комментариев и ненужных пробелов.

Рассмотрим следующую функцию JavaScript:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

Минификации сокращает функцию до следующего:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

Помимо удаления комментариев и ненужных пробелов, следующие имена параметров и переменных были переименованы следующим образом:

До преобразования | Переименовано
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a>Влияние объединения и минификации

В следующей таблице описаны различия между индивидуальной загрузкой ресурсов и использованием объединения и минификации:

Действие | С B/M | Без B/M | Изменение
--- | :---: | :---: | :---:
Запросы файлов  | 7   | 18     | 157%
Передано КБ | 156 | 264.68 | 70%
Время загрузки (МС) | 885 | 2360   | 167%

Обозреватели являются довольно подробными в отношении заголовков HTTP-запросов. Метрика "всего Отправлено байт" заметно уменьшается при объединении. Время загрузки показывает значительное улучшение, но этот пример выполнялся локально. Повышение производительности достигается при использовании объединения и минификации с ресурсами, передаваемыми по сети.

## <a name="choose-a-bundling-and-minification-strategy"></a>Выбор стратегии объединения и минификации

Шаблоны проектов MVC и Razor Pages предоставляют готовые решения для объединения и минификации, состоящие из файла конфигурации JSON. Сторонние средства, такие как средство выполнения задач [grunt](xref:client-side/using-grunt) , выполняют те же задачи с более сложными задачами. Средство стороннего производителя отлично подходит, если рабочему процессу требуется обработка, не связанная с объединением и&mdash;минификации, например linting и оптимизация изображений. С помощью объединения и минификации во время разработки файлы минифицированные создаются до развертывания приложения. Объединение и Минификация до развертывания обеспечивает преимущества снижения нагрузки на сервер. Однако важно понимать, что объединение во время разработки и минификации повышает сложность сборки и работает только со статическими файлами.

## <a name="configure-bundling-and-minification"></a>Настройка объединения и минификации

::: moniker range="<= aspnetcore-2.0"

В ASP.NET Core 2,0 или более ранней версии шаблоны проектов MVC и Razor Pages предоставляют файл конфигурации *бундлеконфиг. JSON* , который определяет параметры для каждого пакета:

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

В ASP.NET Core 2,1 или более поздней версии добавьте новый JSON-файл с именем *бундлеконфиг. JSON*в корневой каталог проекта MVC или Razor Pages. Включить следующий код JSON в этот файл в качестве отправной точки:

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

Файл *бундлеконфиг. JSON* определяет параметры для каждого пакета. В предыдущем примере для пользовательских файлов JavaScript (*wwwroot/JS/site. js*) и таблиц стилей (*wwwroot/CSS/site. CSS*) определена конфигурация одного пакета.

Параметры конфигурации включают:

* `outputFileName`: Имя выходного файла пакета. Может содержать относительный путь из файла *бундлеконфиг. JSON* . **Обязательно**
* `inputFiles`: Массив файлов для объединения. Это относительные пути к файлу конфигурации. **необязательно**, * пустое значение приводит к пустому выходному файлу. поддерживаются шаблоны [глобализации](https://www.tldp.org/LDP/abs/html/globbingref.html) .
* `minify`: Параметры минификации для типа выходных данных. **необязательный**, *по `minify: { enabled: true }` умолчанию —*
  * Параметры конфигурации доступны для каждого типа выходного файла.
    * [Minifier CSS](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [Minifier JavaScript](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [Minifier HTML](https://github.com/madskristensen/BundlerMinifier/wiki)
* `includeInProject`: Флаг, указывающий, добавлять ли созданные файлы в файл проекта. **необязательный**, *по умолчанию — false*
* `sourceMap`: Флаг, указывающий, создавать ли исходную карту для объединенного файла. **необязательный**, *по умолчанию — false*
* `sourceMapRootPath`: Корневой путь для хранения созданного исходного файла отображения.

## <a name="build-time-execution-of-bundling-and-minification"></a>Выполнение объединения и минификации во время сборки

Пакет NuGet [буилдбундлерминифиер](https://www.nuget.org/packages/BuildBundlerMinifier/) обеспечивает выполнение объединения и минификации во время сборки. Пакет внедряет [целевые объекты MSBuild](/visualstudio/msbuild/msbuild-targets) , которые выполняются при сборке и очистке времени. Файл *бундлеконфиг. JSON* анализируется процессом сборки для получения выходных файлов на основе определенной конфигурации.

> [!NOTE]
> Буилдбундлерминифиер принадлежит к управляемому сообществом проекту в GitHub, для которого корпорация Майкрософт не предоставляет поддержку. [Здесь](https://github.com/madskristensen/BundlerMinifier/issues)должны быть зарегистрированы проблемы.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Добавьте пакет *буилдбундлерминифиер* в проект.

Выполните построение проекта. В окне вывода появится следующее:

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

Очистите проект. В окне вывода появится следующее:

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Добавьте пакет *буилдбундлерминифиер* в проект:

```dotnetcli
dotnet add package BuildBundlerMinifier
```

При использовании ASP.NET Core 1. x восстановите только что добавленный пакет:

```dotnetcli
dotnet restore
```

Выполните сборку проекта:

```dotnetcli
dotnet build
```

Появится следующее:

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

Очистите проект:

```dotnetcli
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

## <a name="ad-hoc-execution-of-bundling-and-minification"></a>Прямое выполнение объединения и минификации

Задачи объединения и минификации можно выполнять на нерегламентированном уровне, не создавая проект. Добавьте в проект пакет NuGet [бундлерминифиер. Core](https://www.nuget.org/packages/BundlerMinifier.Core/) :

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> Бундлерминифиер. Core принадлежит к управляемому сообществом проекту в GitHub, для которого корпорация Майкрософт не предоставляет поддержку. [Здесь](https://github.com/madskristensen/BundlerMinifier/issues)должны быть зарегистрированы проблемы.

Этот пакет расширяет .NET Core CLI для включения средства *DotNet-* Package. Следующую команду можно выполнить в окне консоли диспетчера пакетов (PMC) или в командной оболочке:

```dotnetcli
dotnet bundle
```

> [!IMPORTANT]
> Диспетчер пакетов NuGet добавляет зависимости в файл *. csproj в качестве `<PackageReference />` узлов. Команда регистрируется в .NET Core CLI только `<DotNetCliToolReference />` при использовании узла. `dotnet bundle` Внесите соответствующие изменения в файл *. csproj.

## <a name="add-files-to-workflow"></a>Добавить файлы в рабочий процесс

Рассмотрим пример, в котором добавляется дополнительный файл *. CSS* , похожий на следующий:

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

Чтобы уменьшение *Custom. CSS* и объединить его с *site. CSS* в файл *site. min. CSS* , добавьте относительный путь в *бундлеконфиг. JSON*:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> Кроме того, можно использовать следующий шаблон глобализации:
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css))"]
> ```
>
> Этот шаблон глобализации соответствует всем файлам CSS и исключает шаблон файла минифицированные.

Постройте приложение. Откройте файл *site. min. CSS* и обратите внимание, что содержимое *Custom. CSS* добавляется в конец файла.

## <a name="environment-based-bundling-and-minification"></a>Объединение и минификации на основе среды

Рекомендуется использовать объединенные и минифицированные файлы приложения в рабочей среде. Во время разработки исходные файлы упрощают отладку приложения.

Укажите, какие файлы следует включить в страницы с помощью [вспомогательной функции тега среды](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) в представлениях. Вспомогательная функция тега среды отображает свое содержимое только при выполнении в конкретных [средах](xref:fundamentals/environments).

Следующий `environment` тег отображает необработанные файлы CSS при выполнении `Development` в среде:

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

Следующий `environment` тег отображает Объединенные и минифицированные CSS файлы при работе в среде, отличной от `Development`. Например, запуск в `Production` или `Staging` запускает визуализацию этих таблиц стилей:

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a>Использование бундлеконфиг. JSON из gulp

В некоторых случаях для объединения и минификации рабочего процесса приложения требуется дополнительная обработка. Примеры: Оптимизация изображений, отключения кэша и обработка ресурсов CDN. Для удовлетворения этих требований можно преобразовать рабочий процесс объединения и минификации для использования gulp.

### <a name="use-the-bundler--minifier-extension"></a>Использование модуля увязки & расширение Minifier

Модуль упаковки Visual Studio [& расширение Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) обрабатывает преобразование в gulp.

> [!NOTE]
> Модуль & расширения Minifier принадлежит к управляемому сообществом проекту на GitHub, для которого корпорация Майкрософт не предоставляет поддержку. [Здесь](https://github.com/madskristensen/BundlerMinifier/issues)должны быть зарегистрированы проблемы.

Щелкните правой кнопкой мыши файл *бундлеконфиг. JSON* в Обозреватель решений и выберите Группа **& Minifier** > **преобразовать в gulp...** :

![Преобразовать в элемент контекстного меню gulp](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

Файлы *gulpfile. js* и *Package. JSON* добавляются в проект. Вспомогательные пакеты [NPM](https://www.npmjs.com/) , перечисленные в `devDependencies` разделе файла *Package. JSON* , установлены.

Выполните следующую команду в окне PMC, чтобы установить gulp CLI в качестве глобальной зависимости:

```console
npm i -g gulp-cli
```

Файл *gulpfile. js* считывает файл *бундлеконфиг. JSON* для входных, выходных данных и параметров.

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a>Преобразование вручную

Если Visual Studio и (или) пакет & расширение Minifier недоступен, преобразуйте их вручную.

Добавьте в корневой каталог проекта файл *Package. JSON* со следующим `devDependencies`параметром:

> [!WARNING]
> `gulp-uglify` Модуль не поддерживает ECMAScript (ES) 2015/ES6 и более поздние версии. Установите [gulp-сжатый](https://www.npmjs.com/package/gulp-terser) вместо `gulp-uglify` использования ES2015/ES6 или более поздней версии.

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

Установите зависимости, выполнив следующую команду на том же уровне, что и *Package. JSON*:

```console
npm i
```

Установите CLI gulp в качестве глобальной зависимости:

```console
npm i -g gulp-cli
```

Скопируйте файл *gulpfile. js* ниже в корневую папку проекта:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a>Выполнение задач gulp

Чтобы активировать задачу gulp минификации перед построением проекта в Visual Studio, добавьте следующий [целевой объект MSBuild](/visualstudio/msbuild/msbuild-targets) в файл *. csproj:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

В этом примере все задачи, определенные в `MyPreCompileTarget` целевом объекте, выполняются до предопределенного `Build` целевого объекта. Выходные данные, аналогичные приведенным ниже, отображаются в окне Вывод Visual Studio:

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
