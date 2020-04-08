---
title: Объединение и минификация статических ресурсов в ASP.NET Core
author: scottaddie
description: Узнайте, как оптимизировать статические ресурсы в веб-приложении ASP.NET Core, применяя методы объединения и минификации.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/17/2019
uid: client-side/bundling-and-minification
ms.openlocfilehash: a7a5c40d6c31c4416212c02c1b491dd794f2a1d3
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2020
ms.locfileid: "78646786"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a>Объединение и минификация статических ресурсов в ASP.NET Core

Авторы: [Скотт Адди](https://twitter.com/Scott_Addie) (Scott Addie) и [Дэвид Пайн](https://twitter.com/davidpine7) (David Pine)

В этой статье объясняются преимущества применения объединения и минификации, в том числе сведения о том, как эти функции можно использовать для веб-приложений ASP.NET Core.

## <a name="what-is-bundling-and-minification"></a>Что такое объединение и минификация

Объединение и минификации — это два различных способа оптимизации производительности, которые можно применить в веб-приложении. Вместе объединение и минификация позволяют повысить производительность за счет сокращения числа запросов к серверу и уменьшения размера запрашиваемых статических ресурсов.

Объединение и минификация в первую очередь ускоряют загрузку первой страницы. После запроса веб-страницы браузер кэширует статические ресурсы (JavaScript, CSS и изображения). Следовательно, объединение и минификация не улучшают производительность при запросе одной и той же страницы или страниц на одном сайте, запрашивающем одни и те же ресурсы. Если заголовок Expires неправильно задан в ресурсах, а объединение и минификация не используются, эвристика обновления браузера помечает ресурсы как устаревшие через несколько дней. Кроме того, браузер требует запрос проверки для каждого ресурса. В этом случае объединение и минификация обеспечивают повышение производительности даже после запроса первой страницы.

### <a name="bundling"></a>Объединение

Объединение позволяет объединить несколько файлов в один файл. Объединение сокращает количество запросов к серверу, необходимых для преобразования веб-ресурса, например веб-страницы для просмотра. Можно создать любое количество отдельных пакетов специально для CSS, JavaScript и т. д. Чем меньше файлов, тем меньше количество HTTP-запросов от браузера к серверу или от службы, предоставляющей приложение. Это приводит к повышению производительности загрузки первой страницы.

### <a name="minification"></a>Минификация

Минификация удаляет из кода ненужные символы, не затрагивая его функциональность. В результате значительно уменьшается размер запрашиваемых ресурсов (например, CSS, изображений и файлов JavaScript). К обычным побочным эффектам минификации относятся сокращение имен переменных до одного символа и удаление комментариев и ненужных пробелов.

Рассмотрим следующую функцию JavaScript:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

Минификация сокращает функцию до следующего:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

Помимо удаления комментариев и ненужных пробелов, были переименованы следующие имена параметров и переменных:

До преобразования | Переименовано
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a>Влияние объединения и минификации

В следующей таблице описаны различия между загрузкой каждого ресурса отдельно и применением объединения и минификации:

Действие | С О/М | Без О/М | Изменение
--- | :---: | :---: | :---:
Запросы файлов  | 7   | 18     | 157 %
Объем переданных КБ | 156 | 264,68 | 70 %
Время загрузки (мс) | 885 | 2360   | 167 %

Браузеры не протоколируют заголовки HTTP-запросов подробно. При объединении заметно уменьшается общий объем отправленных байтов. Время загрузки значительное уменьшается, однако этот пример выполнялся локально. Значительное повышение производительности достигается при использовании объединения и минификации в отношении ресурсов, передаваемых по сети.

## <a name="choose-a-bundling-and-minification-strategy"></a>Выбор стратегии объединения и минификации

Шаблоны проектов MVC и Razor Pages предоставляют готовые решения для объединения и минификации, состоящие из файла конфигурации JSON. Сторонние средства, такие как запускатель задач [Grunt](xref:client-side/using-grunt), выполняют те же задачи, но с несколько более сложной реализацией. Сторонние средства отлично подходят в случаях, когда в рамках рабочего процесса разработки, помимо объединения и минификации, требуется другая обработка, &mdash; такая как контроль качества кода и оптимизация изображений. С помощью объединения и минификации во время разработки уменьшенные файлы создаются до развертывания приложения. Объединение и минификация до развертывания обеспечивают преимущества снижения нагрузки на сервер. Однако важно понимать, что объединение и минификация во время разработки повышает сложность сборки и применяется только со статическими файлами.

## <a name="configure-bundling-and-minification"></a>Настройка объединения и минификации

::: moniker range="<= aspnetcore-2.0"

В ASP.NET Core 2.0 или более ранних версиях шаблоны проектов MVC и Razor Pages предоставляют файл конфигурации *bundleconfig.json*, который определяет параметры для каждого пакета:

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

В ASP.NET Core 2.1 или более поздних версиях в корневой каталог проекта MVC или Razor Pages следует добавить файл *bundleconfig.json*. Для начала включите в этот файл следующий код JSON:

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

В файле *bundleconfig.json* определяются параметры для каждого пакета. В предыдущем примере для настраиваемого файла JavaScript (*wwwroot/JS/site.js*) и таблицы стилей (*wwwroot/CSS/site. CSS*) определена конфигурация одного пакета.

Доступные варианты конфигурации:

* `outputFileName`. имя файла пакета для вывода. Может содержать относительный путь из файла *bundleconfig.json*. **обязательный параметр**
* `inputFiles`. массив файлов для объединения. Это относительные пути к файлу конфигурации. **Необязательный параметр**; *пустое значение приводит к пустому выходному файлу. Поддерживаются шаблоны [глобализации](https://www.tldp.org/LDP/abs/html/globbingref.html).
* `minify`. параметры минификации для типа выходных данных. **Необязательный параметр**; *значение по умолчанию — `minify: { enabled: true }`*
  * Параметры конфигурации доступны для каждого типа выходного файла.
    * [Уменьшитель CSS](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [Уменьшитель JavaScript](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [Уменьшитель HTML](https://github.com/madskristensen/BundlerMinifier/wiki)
* `includeInProject`. флаг, указывающий, добавлять ли созданные файлы в файл проекта. **Необязательный параметр**; *значение по умолчанию — false*
* `sourceMap`. флаг, указывающий, создавать ли сопоставитель с исходным кодом для объединенного файла. **Необязательный параметр**; *значение по умолчанию — false*
* `sourceMapRootPath`. корневой путь для хранения созданного файла сопоставителя с исходным кодом.

## <a name="build-time-execution-of-bundling-and-minification"></a>Выполнение объединения и минификации во время сборки

Пакет NuGet [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) обеспечивает выполнение объединения и минификации во время сборки. Пакет внедряет [целевые объекты MSBuild](/visualstudio/msbuild/msbuild-targets), которые выполняются во время сборки и очистки. Файл *bundleconfig.json* анализируется процессом сборки для получения выходных файлов на основе определенной конфигурации.

> [!NOTE]
> BuildBundlerMinifier принадлежит к проекту сообщества в GitHub, в отношении которого корпорация Майкрософт не предоставляет поддержку. О проблемах следует сообщать [сюда](https://github.com/madskristensen/BundlerMinifier/issues).

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Добавьте пакет *BuildBundlerMinifier* в свой проект.

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

# <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Добавьте пакет *BuildBundlerMinifier* в проект:

```dotnetcli
dotnet add package BuildBundlerMinifier
```

При использовании ASP.NET Core 1.x восстановите только что добавленный пакет:

```dotnetcli
dotnet restore
```

Скомпилируйте проект.

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

Очистите проект.

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

Задачи объединения и минификации можно выполнять напрямую, не создавая проект. Добавьте пакет NuGet [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) в свой проект:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> BundlerMinifier.Core принадлежит к проекту сообщества в GitHub, в отношении которого корпорация Майкрософт не предоставляет поддержку. О проблемах следует сообщать [сюда](https://github.com/madskristensen/BundlerMinifier/issues).

Этот пакет расширяет .NET Core CLI для включения средства *dotnet-bundle*. Следующую команду можно выполнить в окне консоли диспетчера пакетов (PMC) или в командной оболочке:

```dotnetcli
dotnet bundle
```

> [!IMPORTANT]
> Диспетчер пакетов NuGet добавляет зависимости в файл CSPROJ в качестве узлов `<PackageReference />`. Команда `dotnet bundle` регистрируется в .NET Core CLI только при использовании узла `<DotNetCliToolReference />`. Внесите соответствующие изменения в файл CSPROJ.

## <a name="add-files-to-workflow"></a>Добавление файлов в рабочий процесс

Рассмотрим пример, в котором добавляется дополнительный файл *custom.css*, похожий на следующий:

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

Чтобы уменьшить *custom.css* и объединить его с файлом *site.css* в файл *site.min.css*, добавьте относительный путь к файлу *bundleconfig.json*:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> Кроме того, можно использовать следующий шаблон глобализации:
>
> ```json
> "inputFiles": ["wwwroot/**/!(*.min).css" ]
> ```
>
> Этот шаблон глобализации сопоставляет все файлы CSS и исключает шаблон уменьшенного файла.

Постройте приложение. Откройте *site.min.css* и обратите внимание, что в конец файла добавлено содержимое файла *custom.css*.

## <a name="environment-based-bundling-and-minification"></a>Объединение и минификация на основе среды

Объединенные и минифицированные файлы приложения рекомендуется использовать в рабочей среде. Во время разработки исходные файлы упрощают отладку приложения.

Укажите, какие файлы следует включить на страницы, используя в своих представлениях [вспомогательное приложение тегов среды](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper). Вспомогательное приложение тегов среды отображает свое содержимое только при выполнении в определенных [средах](xref:fundamentals/environments).

Следующий тег `environment` отображает необработанные файлы CSS при выполнении в среде `Development`:

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

Следующий тег `environment` отображает объединенные и минифицированные файлы CSS при выполнении в среде, отличной от `Development`. Например, выполнение в `Production` или `Staging` запускает визуализацию этих таблиц стилей:

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a>Использование bundleconfig.json из Gulp

В некоторых случаях для объединения и минификации рабочего процесса приложения требуется дополнительная обработка. Примеры: оптимизация изображений, отключение кэша и обработка ресурсов CDN. Для удовлетворения этих требований можно преобразовать рабочий процесс объединения и минификации для использования gulp.

### <a name="use-the-bundler--minifier-extension"></a>Использование расширения Bundler & Minifier

Расширение [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) в Visual Studio выполняет преобразование в gulp.

> [!NOTE]
> Расширение Bundler & Minifier принадлежит к проекту сообщества в GitHub, в отношении которого корпорация Майкрософт не предоставляет поддержку. О проблемах следует сообщать [сюда](https://github.com/madskristensen/BundlerMinifier/issues).

Щелкните правой кнопкой мыши файл *bundleconfig.json* в обозревателе решений и выберите **Bundler & Minifier** > **Convert To Gulp** (Преобразовать в Gulp):

![Пункт контекстного меню Convert To Gulp (Преобразовать в Gulp)](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

В проект добавляются файлы *gulpfile.js* и *package.json*. Устанавливаются вспомогательные пакеты [npm](https://www.npmjs.com/), перечисленные в разделе `devDependencies` файла *package.json*.

Выполните следующую команду в окне PMC, чтобы установить CLI Gulp в качестве глобальной зависимости:

```console
npm i -g gulp-cli
```

Файл *gulpfile.js* считывает файл *bundleconfig.json* для получения входных данных, выходных данных и параметров.

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a>Преобразование вручную

Если Visual Studio и (или) расширение Bundler & Minifier недоступны, выполните преобразование вручную.

Добавьте в корневой каталог проекта файл *package.json* со следующим `devDependencies`:

> [!WARNING]
> Модуль `gulp-uglify` не поддерживает ECMAScript (ES) 2015/ES6 и более поздних версий. Вместо `gulp-uglify` установите [gulp-terser](https://www.npmjs.com/package/gulp-terser) для использования ES2015/ES6 или более поздней версии.

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

Установите зависимости, выполнив следующую команду на том же уровне, что и файл *package.json*:

```console
npm i
```

Установите CLI Gulp в качестве глобальной зависимости:

```console
npm i -g gulp-cli
```

Скопируйте файл *gulpfile.js*, представленный ниже, в корневую папку проекта:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a>Выполнение задач Gulp

Чтобы запустить задачу минификации Gulp перед построением проекта в Visual Studio, добавьте следующий [целевой объект MSBuild](/visualstudio/msbuild/msbuild-targets) в файл CSPROJ:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

В этом примере все задачи, определенные в целевом объекте `MyPreCompileTarget`, выполняются до предварительно заданного целевого объекта `Build`. В окне выходных данных Visual Studio отображаются данные, аналогичные приведенным ниже:

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
