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
ms.openlocfilehash: d8512bdd49b61019f22a49900bdd65086d821a6b
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="bundling-and-minification-in-aspnet-core"></a>Объединение и Минификация в ASP.NET Core

Объединение и Минификация — это два метода можно использовать в ASP.NET для повышения производительности загрузки страницы для веб-приложения. Объединение объединяет несколько файлов в один файл. Минификации выполняет различные оптимизации кода различные сценарии и CSS, что приводит в полезных данных меньшего размера. Использовать совместно, объединение и Минификация повышает производительность во время загрузки, уменьшая число запросов к серверу и уменьшения размера запрошенных ресурсов (например, файлов CSS и JavaScript).

В этой статье объясняется преимущества использования объединение и Минификация, включая использование этих функций с приложениями ASP.NET Core.

## <a name="overview"></a>Обзор

В приложениях ASP.NET Core существует несколько вариантов объединение и Минификация ресурсами на стороне клиента. Основные шаблоны для MVC и предоставления решения с помощью файла конфигурации и пакет BuildBundlerMinifier NuGet out of box. Сторонние средства, такие как [Gulp](using-gulp.md) и [Grunt](using-grunt.md) также могут использоваться для выполнения тех же задач сложности и дополнительных рабочего процесса требуется процессу. С помощью разработки объединение и Минификация, уменьшенный файлы создаются до развертывания приложения. Объединение и Минификация перед развертыванием предоставляет преимущество снижение нагрузки. Однако следует отметить, что объединение во время разработки и иными увеличивает сложность сборки и работает только с статических файлов.

Объединение и Минификация главным образом улучшить время загрузки первого запроса страницы. После запросил веб-страницы браузер кэширует активы (JavaScript, CSS и изображения), объединение и Минификация не даст любое повышение производительности при запросе ту же страницу или сайт страниц на том же запросе же активы. Если вы не задали заголовок правильно на Ваши активы истечения срока действия и не используйте объединение и Минификация, эвристики актуальность обозревателя будут отмечены как активы устаревших через несколько дней и требуется браузер запроса на проверку для каждого ресурса. В этом случае объединение и Минификация обеспечивают повышение производительности даже после первого запроса страницы.

### <a name="bundling"></a>Объединение

Объединение — это компонент, упрощающий для объединения или объединить несколько файлов в один файл. Так как объединение объединяет несколько файлов в один файл, это уменьшает количество запросов к серверу, которые необходимы для получения и отображения web средств, таких как веб-страницы. Можно создать CSS, JavaScript и другие пакеты. Меньшее число файлов означает меньшее количество HTTP-запросов из браузера на сервер или из службы, предоставляющей приложение. Результатом является повышение производительности загрузки первой страницы.

### <a name="minification"></a>Минификации

Минификации выполняет различные оптимизации различных кода для уменьшения размера запрошенных ресурсов (например, CSS, изображения, файлы JavaScript). Общие результаты минификации включают Удаление лишних пробелов и комментарии и сокращать имена переменных в один символ.

Рассмотрим следующую функцию JavaScript:

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

После минификации функция сокращается до следующее:

```javascript
AddAltToImg=function(t,a){var r=$(t,a);r.attr("alt",r.attr("id").replace(/ID/,""))};
```

Помимо удаление комментариев и ненужные пробелы, следующие параметры и имена переменных были переименованы (сокращено) следующим образом:

До преобразования | Переименовано
--- | :---:
imageTagAndImageID | т
в пределах изображения | пример
imageElement | r

## <a name="impact-of-bundling-and-minification"></a>Влияние объединение и Минификация

В следующей таблице показаны несколько важных различий между все активы по отдельности и объединение и Минификация простой веб-страницы.

Действие | С B и M | Без B и M | Изменение
--- | :---: | :---: | :---:
Запросы файлов |7 | 18 | 157%
Передать КБ | 156 | 264.68 | 70%
Время загрузки (мс) | 885 | 2360 | 167%

Отправлено байт было значительное сокращение с объединением как браузеры довольно verbose с заголовки HTTP, которые применяются в запросах. Время загрузки показывает значительным усовершенствованием по, однако в этом примере была запущена локально. Вы получите больше повысить производительность при помощи активы объединение и Минификация передаваемых по сети.

## <a name="using-bundling-and-minification-in-a-project"></a>С помощью объединение и Минификация в проекте

Шаблон проекта MVC предоставляет `bundleconfig.json` файл конфигурации, который определяет параметры для каждого пакета. По умолчанию, определенное конфигурации один комплект для пользовательским кодом JavaScript (`wwwroot/js/site.js`) и таблицы стилей (`wwwroot/css/site.css`) файлы.

[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig.json)]

Параметры пакета:

* outputFileName - имя файла пакета для вывода. Может содержать относительный путь от `bundleconfig.json` файла. **Обязательно**
* inputFiles - массив файлов, которые будут объединены. Это относительные пути к файлу конфигурации. **Необязательный**, * пустое значение преобразуется в пустой выходной файл. [Этот режим](http://www.tldp.org/LDP/abs/html/globbingref.html) поддерживаются шаблоны.
* уменьшения - введите минификации параметры вывода. **Необязательный**, *по умолчанию —`minify: { enabled: true }`*
  * Параметры конфигурации доступны на тип выходного файла.
    * [Уменьшитель CSS](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [Уменьшитель JavaScript](https://github.com/madskristensen/BundlerMinifier/wiki)
    * [Уменьшитель HTML](https://github.com/madskristensen/BundlerMinifier/wiki)
* includeInProject - добавить созданные файлы в файл проекта. **Необязательный**, *по умолчанию - false*
* Сопоставления - формирования сопоставлений источника для объединенный файл. **Необязательный**, *по умолчанию - false*

### <a name="visual-studio-2015--2017"></a>Visual Studio 2015 / 2017

Откройте `bundleconfig.json` в Visual Studio, если среде не имеет расширение установлено; запрос выводится что предполагает отсутствие, может помочь с этим типом файла.

![Расширение BuildBundlerMinifier предложений](../client-side/bundling-and-minification/_static/bundler-extension-suggestion.png)

Выберите представление расширений и установите **Bundler & Уменьшитель** расширения (Visual Studio, требуется перезагрузка).

![Расширение BuildBundlerMinifier предложений](../client-side/bundling-and-minification/_static/view-extension.png)

После завершения перезагрузки необходимо настроить сборку для запуска процессов минификации и объединения клиентских средств. Щелкните правой кнопкой мыши `bundleconfig.json` файла и выберите *Enable пакета на сборки... *.

Выполните построение проекта и `bundleconfig.json` включается в процесс построения для создания выходных файлов, на основе конфигурации.

```console
1>------ Build started: Project: BuildBundlerMinifierExample, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierExample -> C:\BuildBundlerMinifierExample\bin\Debug\netcoreapp1.1\BuildBundlerMinifierExample.dll
========== Build: 1 succeeded or up-to-date, 0 failed, 0 skipped ==========
```

### <a name="visual-studio-code-or-command-line"></a>Код Visual Studio или командной строки

Visual Studio и расширение управления процессом объединение и Минификация, с помощью жестов графического пользовательского интерфейса; Однако те же возможности доступны с `dotnet` CLI и BuildBundlerMinifier NuGet пакет.

Добавьте пакет NuGet в проект.

```console
dotnet add package BuildBundlerMinifier
```

Восстановление зависимости.

```console
dotnet restore
```

Построение приложения:

```console
dotnet build
```

Выходные данные команды построения показаны результаты минификации и/или объединении в соответствии с настроенным.

```console
Microsoft (R) Build Engine version 15.1.545.13942
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Begin processing bundleconfig.json
     Minified wwwroot/css/site.min.css
  Bundler: Done processing bundleconfig.json
  BuildBundlerMinifierExample -> /BuildBundlerMinifierExample/bin/Debug/netcoreapp1.0/BuildBundlerMinifierExample.dll
```

## <a name="adding-files"></a>Добавление файлов

В этом примере добавляется вызываемой дополнительный файл CSS `custom.css` и настроен для объединение и Минификация с `site.css`, полученный в одном `site.min.css`.

Custom.CSS

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

Добавьте относительный путь к `bundleconfig.json`.

[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig2.json)]

> [!NOTE]
> Кроме того, можно использовать шаблон глобализацию - `"inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]` получающий CSS все файлы и исключает шаблон уменьшенный файла.

Построение приложения и при открытии `site.min.css`, вы теперь Обратите внимание, что содержимое `custom.css` добавляются в конец файла.

## <a name="controlling-bundling-and-minification"></a>Управление объединение и Минификация

Как правило вы хотите использовать пакетные и уменьшенный файлы приложения только в рабочей среде. Во время разработки вы хотите использовать исходные файлы, чтобы упростить процесс отладки приложения.

Можно указать, какие сценарии и файлы CSS для включения в использование вспомогательного метода тега среды на страницах макета страниц (в разделе [вспомогательных функций тегов](../mvc/views/tag-helpers/index.md)). При запуске в определенных средах вспомогательный тега среды будет отображать только его содержимое. В разделе [работа с несколькими средами](../fundamentals/environments.md) подробные сведения об указании текущей среды.

Следующие теги среды будет отображаться необработанных файлов CSS, при работе в `Development` среде:

[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=3&range=9-12)]

Этот тег среды будет отображаться распространение и уменьшенный CSS-файл только при работе в `Production` или `Staging`:

[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=5&range=13-18)]

## <a name="consuming-bundleconfigjson-from-gulp"></a>Использование bundleconfig.json из Gulp

Если рабочий процесс приложения объединение и Минификация требуются дополнительные действия, такие как обработка изображений, busting кэша, обработки assest CDN, т. д., можно преобразовать в процессе пакета и Minify в Gulp.

> [!NOTE]
> Преобразование параметр доступен только в Visual Studio 2015 и 2017 г.

Щелкните правой кнопкой мыши `bundleconfig.json` и выберите **преобразовать Gulp... **. Это создаст `gulpfile.js` и установите пакеты необходимых npm.

![Преобразовать в Gulp](../client-side/bundling-and-minification/_static/convert-togulp.png)

`gulpfile.js` Полученных операций чтения `bundleconfig.json` файла конфигурации, поэтому он может продолжать использоваться для ввода вывода и параметры.

[!code-javascript[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/gulpfile.js)]

Чтобы включить Gulp при построении проекта в Visual Studio 2017 г, добавьте следующий файл *.csproj:

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
    <Exec Command="gulp min" />
</Target>
```

Чтобы включить Gulp при построении проекта в Visual Studio 2015, добавьте следующий код в `project.json` файла:

```json
"scripts": {
    "precompile": "gulp min"
}
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Использование Gulp](using-gulp.md)
* [С помощью Grunt](using-grunt.md)
* [Работа с несколькими средами](../fundamentals/environments.md)
* [Вспомогательных функций тегов](../mvc/views/tag-helpers/index.md)
