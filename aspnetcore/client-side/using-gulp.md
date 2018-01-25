---
title: "Использование Gulp для ASP.NET Core"
author: rick-anderson
description: "Сведения об использовании Gulp в ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 02/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-gulp
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2ccfed42d66ea49c5f2745bc8653d8fb12bf707a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-using-gulp-in-aspnet-core"></a>Общие сведения об использовании Gulp в ASP.NET Core 

По [Reitan Эрик](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [рот Daniel](https://github.com/danroth27), и [Бойера Shayne](https://twitter.com/spboyer)

В обычной современных веб-приложения процесс построения может:

* Объединение и уменьшения файлы JavaScript и CSS.
* Запуск средств для вызова объединение и Минификация задачи перед каждой сборкой.
* Скомпилируйте МЕНЕЕ или SASS файлов CSS.
* Скомпилируйте файлы CoffeeScript и TypeScript в код JavaScript.

Объект *средство запуска задач* — это средство, автоматизирующее этих задач процедуры разработки и многое другое. Visual Studio предоставляет встроенную поддержку для средства запуска два распространенных задач на основе JavaScript: [Gulp](https://gulpjs.com/) и [Grunt](using-grunt.md).

## <a name="gulp"></a>gulp

Gulp — это на базе JavaScript потоковой передачи построения набор средств для клиентского кода. Обычно он используется для потоковой передачи клиентские файлы через ряд процессов при активации определенного события в среде построения. Например, Gulp можно использовать для автоматизации [объединение и Минификация](bundling-and-minification.md) или очистки среде разработки до новой сборки.

Набор задач Gulp определяется в *gulpfile.js*. Следующие JavaScript включает Gulp модулей, а также задает пути к файлам, должно быть указано в дальнейшем будет обеспечена поддержка задачи:

```javascript
/// <binding Clean='clean' />
"use strict";

var gulp = require("gulp"),
  rimraf = require("rimraf"),
  concat = require("gulp-concat"),
  cssmin = require("gulp-cssmin"),
  uglify = require("gulp-uglify");

var paths = {
  webroot: "./wwwroot/"
};

paths.js = paths.webroot + "js/**/*.js";
paths.minJs = paths.webroot + "js/**/*.min.js";
paths.css = paths.webroot + "css/**/*.css";
paths.minCss = paths.webroot + "css/**/*.min.css";
paths.concatJsDest = paths.webroot + "js/site.min.js";
paths.concatCssDest = paths.webroot + "css/site.min.css";
```

Приведенный выше код указывает, какой узел модули являются обязательными. `require` Импорта функций каждого модуля, чтобы зависимые задачи можно использовать предоставляемые им возможности. Каждый из импортированных модулей присваивается переменной. Модули могут располагаться по имени или пути. В этом примере имя модули `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, и `gulp-uglify` получить по имени. Кроме того ряд пути создаются, расположения файлов CSS и JavaScript можно использовать повторно и ссылки на задачи. В следующей таблице приведены описания модулей, включенных в *gulpfile.js*.

|Имя модуля|Описание:|
|---|---|
|gulp|Gulp потоковой передачи системы сборки. Дополнительные сведения см. в разделе [gulp](https://www.npmjs.com/package/gulp).|
|rimraf|Модуль удаления узла. Дополнительные сведения см. в разделе [rimraf](https://www.npmjs.com/package/rimraf).|
|gulp concat|Модуль, который объединяет файлы в зависимости от операционной системы символ перевода строки. Дополнительные сведения см. в разделе [gulp concat](https://www.npmjs.com/package/gulp-concat).|
|gulp cssmin|Модуль, который уменьшает CSS-файлах. Дополнительные сведения см. в разделе [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin).|
|gulp uglify|Модуль, который уменьшает *.js* файлов. Дополнительные сведения см. в разделе [gulp uglify](https://www.npmjs.com/package/gulp-uglify).|

После импорта модулей необходимые задачи может быть указан. Ниже приведены шесть задач зарегистрирован, представленный в следующем примере кода:

```javascript
gulp.task("clean:js", function (cb) {
  rimraf(paths.concatJsDest, cb);
});

gulp.task("clean:css", function (cb) {
  rimraf(paths.concatCssDest, cb);
});

gulp.task("clean", ["clean:js", "clean:css"]);

gulp.task("min:js", function () {
  return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
    .pipe(concat(paths.concatJsDest))
    .pipe(uglify())
    .pipe(gulp.dest("."));
});

gulp.task("min:css", function () {
  return gulp.src([paths.css, "!" + paths.minCss])
    .pipe(concat(paths.concatCssDest))
    .pipe(cssmin())
    .pipe(gulp.dest("."));
});

gulp.task("min", ["min:js", "min:css"]);
```

В следующей таблице приведены пояснения задачи, указанные в приведенном выше коде:

|Имя задачи|Описание:|
|--- |--- |
|Очистить: js|Задача, которая использует модуль удаления rimraf узел для удаления уменьшенная версия файла site.js.|
|Очистить: css|Задача, которая использует модуль удаления rimraf узел для удаления уменьшенная версия файла site.css.|
|Очистка|Задача, которая вызывает `clean:js` задач, за которым следует `clean:css` задачи.|
|MIN:js|Задача, которая уменьшает и объединяет все JS-файлов в папке js. . Min.js файлы исключаются.|
|MIN:CSS|Задача, которая уменьшает и объединение всех файлов CSS-файлах в папке css. . Min.css файлы исключаются.|
|min|Задача, которая вызывает `min:js` задач, за которым следует `min:css` задачи.|

## <a name="running-default-tasks"></a>Выполнение задач по умолчанию

Если вы еще не создали уже новое веб-приложение, создайте новый проект веб-приложения ASP.NET в Visual Studio.

1.  Добавьте новый файл JavaScript в проект и назовите его *gulpfile.js*, затем скопируйте следующий код.

    ```javascript
    /// <binding Clean='clean' />
    "use strict";
    
    var gulp = require("gulp"),
      rimraf = require("rimraf"),
      concat = require("gulp-concat"),
      cssmin = require("gulp-cssmin"),
      uglify = require("gulp-uglify");
    
    var paths = {
      webroot: "./wwwroot/"
    };
    
    paths.js = paths.webroot + "js/**/*.js";
    paths.minJs = paths.webroot + "js/**/*.min.js";
    paths.css = paths.webroot + "css/**/*.css";
    paths.minCss = paths.webroot + "css/**/*.min.css";
    paths.concatJsDest = paths.webroot + "js/site.min.js";
    paths.concatCssDest = paths.webroot + "css/site.min.css";
    
    gulp.task("clean:js", function (cb) {
      rimraf(paths.concatJsDest, cb);
    });
    
    gulp.task("clean:css", function (cb) {
      rimraf(paths.concatCssDest, cb);
    });
    
    gulp.task("clean", ["clean:js", "clean:css"]);
    
    gulp.task("min:js", function () {
      return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
        .pipe(concat(paths.concatJsDest))
        .pipe(uglify())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min:css", function () {
      return gulp.src([paths.css, "!" + paths.minCss])
        .pipe(concat(paths.concatCssDest))
        .pipe(cssmin())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min", ["min:js", "min:css"]);
    ```

2.  Откройте *package.json* файла (Добавление Если не существует) и добавьте следующий код.

    ```json
    {
      "devDependencies": {
        "gulp": "3.9.1",
        "gulp-concat": "2.6.1",
        "gulp-cssmin": "0.1.7",
        "gulp-uglify": "2.0.1",
        "rimraf": "2.6.1"
      }
    }
    ```

3.  В **обозревателе решений**, щелкните правой кнопкой мыши *gulpfile.js*и выберите **Task Runner Explorer**.
    
    ![Откройте диспетчер выполнения задач с помощью обозревателя решений](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    **Задача Runner Explorer** представлен список задач Gulp. (Может потребоваться нажать кнопку **обновление** , расположенную слева от имени проекта.)
    
    ![Диспетчер выполнения задач](using-gulp/_static/03-TaskRunnerExplorer.png)

4.  Под разделом **задачи** в **Task Runner Explorer**, щелкните правой кнопкой мыши **чистой**и выберите **запуска** из всплывающего меню.

    ![Задачу "Очистить" Диспетчер выполнения задач](using-gulp/_static/04-TaskRunner-clean.png)

    **Задача Runner Explorer** создается новая вкладка с именем **чистой** и выполните задачу "Очистить", как оно определено в *gulpfile.js*.

5.  Щелкните правой кнопкой мыши **чистой** задач, а затем выберите **привязки** > **перед построить**.

    ![Привязка BeforeBuild Task Runner Explorer](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    **Перед построить** привязки настраивает задачу "Очистить" для автоматического запуска перед каждой сборке проекта.

Привязки настраиваются с помощью **Task Runner Explorer** хранятся в виде комментариев в верхней части вашего *gulpfile.js* и вступают в силу только в Visual Studio. Альтернатива, которой не требуется Visual Studio — Настройка автоматического выполнения задач gulp в вашей *.csproj* файла. Например, поместите ее ваш *.csproj* файла:

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

Теперь задачу "Очистить" выполняется при запуске проекта в Visual Studio или из командной строки с помощью `dotnet run` команда (запустить `npm install` первой).

## <a name="defining-and-running-a-new-task"></a>Определение и выполнение нового задания

Чтобы определить новую задачу Gulp, измените *gulpfile.js*.

1.  Добавление следующего кода JavaScript в конец *gulpfile.js*:

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    Эта задача называется `first`, и он просто отображает строку.

2.  Сохранить *gulpfile.js*.

3.  В **обозревателе решений**, щелкните правой кнопкой мыши *gulpfile.js*и выберите *Task Runner Explorer*.

4.  В **Task Runner Explorer**, щелкните правой кнопкой мыши **первый**и выберите **запуска**.

    ![Task Runner Explorer выполнения первой задачи](using-gulp/_static/06-TaskRunner-First.png)

    Вы увидите, что отображается выходного текста. Если вы заинтересованы в примерах, основанные на общих сценариях, в разделе Gulp рецептами.

## <a name="defining-and-running-tasks-in-a-series"></a>Определение и выполнение задач в ряд

При выполнении нескольких задач одновременно выполняются задачи по умолчанию. Тем не менее, если необходимо выполнить задачи в определенном порядке, необходимо указать каждую задачу после завершения операции, а также как какие задачи зависят от завершения другой задачи.

1.  Чтобы определить последовательность задач для выполнения в порядке, замените `first` задачи, добавленные в *gulpfile.js* со следующим:

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    Теперь у вас есть три задачи: `series:first`, `series:second`, и `series`. `series:second` Задача включает второй параметр, указывающий массив задачи должен быть запущен и завершена до `series:second` задача будет выполняться. Как указано в коде выше, только `series:first` задача должна быть выполнена перед `series:second` задача будет выполняться.

2.  Сохранить *gulpfile.js*.

3.  В **обозревателе решений**, щелкните правой кнопкой мыши *gulpfile.js* и выберите **Task Runner Explorer** , если он еще не открыт.

4.  В **Task Runner Explorer**, щелкните правой кнопкой мыши **серии** и выберите **запуска**.

    ![Task Runner Explorer выполнения ряда задач](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a>IntelliSense

IntelliSense предоставляет дополнение кода, описания параметров и другие возможности для повышения производительности и сокращения ошибок. Gulp задачи создаются на языке JavaScript; Таким образом технология IntelliSense может оказывать помощь при разработке. При работе с кодом JavaScript, IntelliSense выводит списки объектов, функций, свойств и параметров, доступных на основании текущего контекста. Вариант кода можно выберите из всплывающего списка, формируемого IntelliSense для завершения кода.

![gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

Дополнительные сведения о технологии IntelliSense см. в разделе [IntelliSense для JavaScript](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense).

## <a name="development-staging-and-production-environments"></a>Разработки, промежуточной и производственной сред

Использование Gulp для оптимизации клиентские файлы для промежуточных и производственных обработанных файлов сохраняются в локальной папке промежуточной и производственной сред. *_Layout.cshtml* файле используется **среды** тег вспомогательный метод для предоставления двух разных версий файлов CSS. Является одной версии файлов CSS для разработки и другую версию оптимизирован для промежуточной и производственной сред. В Visual Studio 2017 г., при изменении **ASPNETCORE_ENVIRONMENT** переменную среды, чтобы `Production`, Visual Studio построит веб-приложения и ссылки в свернутом виде CSS-файлах. В следующем разметки показан **среды** содержащий теги ссылок для вспомогательных функций тегов `Development` CSS файлов и уменьшенное `Staging, Production` CSS-файлах.

```html
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/js/site.js" asp-append-version="true"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.2.0.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery"
            crossorigin="anonymous"
            integrity="sha384-K+ctZQ+LL8q6tP7I94W+qzQsfRV2a+AfHIi9k8z8l9ggpc8X+Ytst4yBo/hH+8Fk">
    </script>
    <script src="https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js"
            asp-fallback-src="~/lib/bootstrap/dist/js/bootstrap.min.js"
            asp-fallback-test="window.jQuery && window.jQuery.fn && window.jQuery.fn.modal"
            crossorigin="anonymous"
            integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa">
    </script>
    <script src="~/js/site.min.js" asp-append-version="true"></script>
</environment>
```

## <a name="switching-between-environments"></a>Переключение между средами

Чтобы переключиться между компиляции для разных сред, измените **ASPNETCORE_ENVIRONMENT** значение переменной среды.

1.  В **Task Runner Explorer**, убедитесь, что **min** задача настроена для запуска **перед построить**.

2.  В **обозревателе решений**, щелкните правой кнопкой мыши имя проекта и выберите **свойства**.

    Откроется окно свойств веб-приложения.

3.  Откройте вкладку **Отладка**.

4.  Установите для параметра **: среда внешнего размещения** переменную среды, чтобы `Production`.

5.  Нажмите клавишу **F5** для запуска приложения в браузере.

6.  В окне браузера щелкните страницу правой кнопкой мыши и выберите **источник** Просмотр HTML для страницы.

    Обратите внимание, что таблица стилей ссылки указывают на уменьшенный CSS-файл.

7.  Закройте браузер, чтобы остановить веб-приложение.

8.  В Visual Studio, вернуться к странице свойств веб-приложения и изменить **: среда внешнего размещения** обратно переменной среды `Development`.

9.  Нажмите клавишу **F5** Чтобы снова запустить приложение в браузере.

10. В окне браузера щелкните страницу правой кнопкой мыши и выберите **источник** в HTML-код для этой страницы.

    Обратите внимание, что таблица стилей ссылки указывают на unminified версии файлов CSS.

Дополнительные сведения, относящиеся к средам в ASP.NET Core см. в разделе [работа с несколькими средами](../fundamentals/environments.md).

## <a name="task-and-module-details"></a>Сведения о задаче и модуль

Задание Gulp зарегистрировано с именем функции. Если других задач должна быть запущена перед текущей задачи, можно указать зависимости. Дополнительные функции позволяют выполнить и посмотреть Gulp задачи, а также в качестве источника задайте (*src*) и назначения (*dest*) выполняется изменение файлов. Ниже перечислены основные функции Gulp API.

|Функция gulp|Синтаксис|Описание:|
|---   |--- |--- |
|задача  |`gulp.task(name[, deps], fn) { }`|`task` Функция создает задачу. `name` Параметр определяет имя задачи. `deps` Параметр содержит массив задачи должны быть выполнены до выполнения этой задачи. `fn` Представляет параметр функции обратного вызова, которая выполняет операции задачи.|
|Контрольное значение |`gulp.watch(glob [, opts], tasks) { }`|`watch` Функция отслеживает файлы и выполняет задачи, когда происходит изменение файла. `glob` Параметр `string` или `array` , определяющий, какие файлы для наблюдения. `opts` Параметр предоставляет дополнительный файл просмотра параметры.|
|src   |`gulp.src(globs[, options]) { }`|`src` Функция предоставляет файлы, которые совпадают со значениями glob. `glob` Параметр `string` или `array` , определяющий, какие файлы для чтения. `options` Параметр предоставляет дополнительные параметры файла.|
|dest  |`gulp.dest(path[, options]) { }`|`dest` Функция определяет расположение, к которому файлы могут быть записаны. `path` Параметр — это строка или функция, которая определяет конечную папку. `options` Параметр представляет собой объект, указывающий параметры выходной папки.|

Дополнительные справочные сведения Gulp API см. в разделе [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).

## <a name="gulp-recipes"></a>Gulp рецепты

Gulp сообщества предоставляет Gulp [рецепты](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md). Этими рецептами состоят из Gulp задачи для распространенных сценариев.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Gulp документации](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [Объединение и Минификация в ASP.NET Core](bundling-and-minification.md)
* [С помощью Grunt в ASP.NET Core](using-grunt.md)
