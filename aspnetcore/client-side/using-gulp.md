---
title: В ASP.NET Core с помощью средства Gulp
author: rick-anderson
description: Узнайте, как с помощью средства Gulp в ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/04/2018
uid: client-side/using-gulp
ms.openlocfilehash: 43277dc5910971374187f49031e74769c9e29e1f
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665630"
---
# <a name="use-gulp-in-aspnet-core"></a>В ASP.NET Core с помощью средства Gulp

По [Scott Addie](https://scottaddie.com), [Шейн Бойер](https://twitter.com/spboyer), и [сосну Дэвид](https://twitter.com/davidpine7)

В типичных современных веб-приложения процесс построения может сделать следующее.

* Объединение и Минификация файлов JavaScript и CSS.
* Запуск средств для вызова задачи объединения и минификации перед каждым построением.
* Скомпилируйте МЕНЕЕ или SASS файлов CSS.
* Скомпилируйте файлы CoffeeScript и TypeScript для JavaScript.

Объект *запускатель задач* — это средство, которое позволяет автоматизировать выполнение этих задач текущей разработки и многое другое. Visual Studio предоставляет встроенную поддержку для двух средства выполнения распространенных задач на основе JavaScript: [Gulp](https://gulpjs.com/) и [Grunt](using-grunt.md).

## <a name="gulp"></a>Gulp

Gulp — это на базе JavaScript потоковой передачи построения набор средств для клиентского кода. Обычно он используется для потоковую передачу файлов на стороне клиента через ряд процессов, при активации определенного события в среде построения. Например, Gulp может использоваться для автоматизации [объединение и Минификация](bundling-and-minification.md) или очистка среды разработки до новой сборки.

Набор задач Gulp определяется в *gulpfile.js*. Включает в себя модули Gulp следующий код JavaScript с указанием пути к файлам, чтобы ссылаться на будущей задачи:

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

Приведенный выше код указывает, какие модули узла являются обязательными. `require` Импорты функций каждого модуля, чтобы зависимые задачи можно использовать предоставляемые им возможности. Каждый из импортированных модулей присваивается переменной. Модули могут располагаться либо по имени или пути. В этом примере имя модули `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, и `gulp-uglify` извлекаются по имени. Кроме того ряд пути создаются, чтобы расположения файлов CSS и JavaScript можно использовать повторно и ссылки на задачи. В следующей таблице приведены описания модулей, включенных в *gulpfile.js*.

| Имя модуля | Описание |
| ----------- | ----------- |
| Gulp        | Система сборки Gulp потоковой передачи. Дополнительные сведения см. в разделе [gulp](https://www.npmjs.com/package/gulp). |
| rimraf      | Модуль удаления узла. Дополнительные сведения см. в разделе [rimraf](https://www.npmjs.com/package/rimraf). |
| gulp concat | Модуль, который объединяет файлы в зависимости от операционной системы символ новой строки. Дополнительные сведения см. в разделе [gulp concat](https://www.npmjs.com/package/gulp-concat). |
| gulp cssmin | Модуль, который уменьшает файлов CSS. Дополнительные сведения см. в разделе [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin). |
| gulp uglify | Модуль, который уменьшает *.js* файлов. Дополнительные сведения см. в разделе [gulp uglify](https://www.npmjs.com/package/gulp-uglify). |

После импорта необходимых модулей задачи можно указать. Здесь есть шесть задач зарегистрирован, представлен в следующем примере кода:

```javascript
gulp.task("clean:js", done => rimraf(paths.concatJsDest, done));
gulp.task("clean:css", done => rimraf(paths.concatCssDest, done));
gulp.task("clean", gulp.series(["clean:js", "clean:css"]));

gulp.task("min:js", () => {
  return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
    .pipe(concat(paths.concatJsDest))
    .pipe(uglify())
    .pipe(gulp.dest("."));
});

gulp.task("min:css", () => {
  return gulp.src([paths.css, "!" + paths.minCss])
    .pipe(concat(paths.concatCssDest))
    .pipe(cssmin())
    .pipe(gulp.dest("."));
});

gulp.task("min", gulp.series(["min:js", "min:css"]));
    
// A 'default' task is required by Gulp v4
gulp.task("default", gulp.series(["min"]));
```

---

Ниже приводится объяснение того, задачи, указанные в приведенном выше коде:

|Имя задачи|Описание:|
|--- |--- |
|Очистить: js|Задача, которая использует модуль удаления rimraf узел для удаления минифицированные версию файла site.js.|
|Очистить: css|Задача, которая использует модуль удаления rimraf узел для удаления минифицированные версию файла site.css.|
|Очистить|Задача, которая вызывает `clean:js` задачи, за которым следует `clean:css` задачи.|
|MIN:js|Задача, которая уменьшает и Сцепляет всех JS-файлов в папке js. . Файлы min.js исключаются.|
|MIN:CSS|Задача, которая уменьшает и объединяет все файлы .css в папке css. . Файлы min.css исключаются.|
|min|Задача, которая вызывает `min:js` задачи, за которым следует `min:css` задачи.|

## <a name="running-default-tasks"></a>Выполнение задач по умолчанию

Если вы уже создавали новое веб-приложение, создайте новый проект веб-приложения ASP.NET в Visual Studio.

1.  Откройте *package.json* файла (Добавление Если не существует) и добавьте следующий код.

    ```json
    {
      "devDependencies": {
        "gulp": "^4.0.0",
        "gulp-concat": "2.6.1",
        "gulp-cssmin": "0.2.0",
        "gulp-uglify": "3.0.0",
        "rimraf": "2.6.1"
      }
    }
    ```

2.  Добавьте новый файл JavaScript в проект и назовите его *gulpfile.js*, затем скопируйте следующий код.

    ```javascript
    /// <binding Clean='clean' />
    "use strict";
    
    const gulp = require("gulp"),
          rimraf = require("rimraf"),
          concat = require("gulp-concat"),
          cssmin = require("gulp-cssmin"),
          uglify = require("gulp-uglify");
    
    const paths = {
      webroot: "./wwwroot/"
    };
    
    paths.js = paths.webroot + "js/**/*.js";
    paths.minJs = paths.webroot + "js/**/*.min.js";
    paths.css = paths.webroot + "css/**/*.css";
    paths.minCss = paths.webroot + "css/**/*.min.css";
    paths.concatJsDest = paths.webroot + "js/site.min.js";
    paths.concatCssDest = paths.webroot + "css/site.min.css";
    
    gulp.task("clean:js", done => rimraf(paths.concatJsDest, done));
    gulp.task("clean:css", done => rimraf(paths.concatCssDest, done));
    gulp.task("clean", gulp.series(["clean:js", "clean:css"]));

    gulp.task("min:js", () => {
      return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
        .pipe(concat(paths.concatJsDest))
        .pipe(uglify())
        .pipe(gulp.dest("."));
    });

    gulp.task("min:css", () => {
      return gulp.src([paths.css, "!" + paths.minCss])
        .pipe(concat(paths.concatCssDest))
        .pipe(cssmin())
        .pipe(gulp.dest("."));
    });

    gulp.task("min", gulp.series(["min:js", "min:css"]));
    
    // A 'default' task is required by Gulp v4
    gulp.task("default", gulp.series(["min"]));
    ```

3.  В **обозревателе решений**, щелкните правой кнопкой мыши *gulpfile.js*и выберите **Task Runner Explorer**.
    
    ![Откройте диспетчер выполнения задач из обозревателя решений](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    **Task Runner Explorer** представлен список задач Gulp. (Может потребоваться щелкнуть **обновить** кнопка, которая появляется слева от имени проекта.)
    
    ![Task Runner Explorer](using-gulp/_static/03-TaskRunnerExplorer.png)
    
    > [!IMPORTANT]
    > **Task Runner Explorer** пункт контекстного меню отображается только в том случае, если *gulpfile.js* находится в корневом каталоге проекта.

4.  Под **задачи** в **Task Runner Explorer**, щелкните правой кнопкой мыши **чистой**и выберите **запуска** во всплывающем меню.

    ![Задачу "Очистить" Диспетчер выполнения задач](using-gulp/_static/04-TaskRunner-clean.png)

    **Task Runner Explorer** создаст новую вкладку, называемую **чистой** и выполните задачу "Очистить", как оно определено в *gulpfile.js*.

5.  Щелкните правой кнопкой мыши **чистой** задач, а затем выберите **привязки** > **перед построить**.

    ![Task Runner Explorer привязки BeforeBuild](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    **Перед построить** привязки настраивает задачу "Очистить" для автоматического запуска перед каждым построением проекта.

Привязки настраиваются с помощью **Task Runner Explorer** хранятся в виде комментария в верхней части вашего *gulpfile.js* и вступают в силу только в Visual Studio. Другой способ, который не требует Visual Studio — настроить автоматическое выполнение задачи gulp в вашей *.csproj* файл. Например, разместить их в вашей *.csproj* файла:

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

Теперь задачу "Очистить" выполняется при запуске проекта в Visual Studio или из командной строки с помощью [запуска dotnet](/dotnet/core/tools/dotnet-run) команды (запустить `npm install` первый).

## <a name="defining-and-running-a-new-task"></a>Определение и выполнение новой задачи

Чтобы определить новая задача Gulp, измените *gulpfile.js*.

1.  Добавьте следующий код JavaScript в конец *gulpfile.js*:

    ```javascript
    gulp.task('first', done => {
      console.log('first task! <-----');
      done(); // signal completion
    });
    ```

    Эта задача имеет имя `first`, и он просто отображает строку.

2.  Сохранить *gulpfile.js*.

3.  В **обозревателе решений**, щелкните правой кнопкой мыши *gulpfile.js*и выберите *Task Runner Explorer*.

4.  В **Task Runner Explorer**, щелкните правой кнопкой мыши **первый**и выберите **запуска**.

    ![Task Runner Explorer выполнения первой задачи](using-gulp/_static/06-TaskRunner-First.png)

    Выходной текст. Для просмотра примеров на основе общих сценариев, см. в разделе [Gulp рецепты](#gulp-recipes).

## <a name="defining-and-running-tasks-in-a-series"></a>Определение и выполнение задач в последовательность

При выполнении нескольких задач, задачи выполняются параллельно по умолчанию. Тем не менее, если необходимо выполнить задачи в определенном порядке, необходимо указать когда каждая задача будет завершена, а также как какие задачи зависят от завершения другой задачи.

1.  Чтобы определить последовательность задач для выполнения в порядке, замените `first` задачу, которая добавляется выше в *gulpfile.js* следующим:

    ```javascript
    gulp.task('series:first', done => {
      console.log('first task! <-----');
      done(); // signal completion
    });
    gulp.task('series:second', done => {
      console.log('second task! <-----');
      done(); // signal completion
    });

    gulp.task('series', gulp.series(['series:first', 'series:second']), () => { });

    // A 'default' task is required by Gulp v4
    gulp.task('default', gulp.series('series'));
    ```
 
    Теперь у вас есть три задачи: `series:first`, `series:second`, и `series`. `series:second` Задача включает второй параметр, указывающий массив задач запуска и завершения перед `series:second` задача будет выполняться. Как указано в коде выше, только `series:first` задача должна быть выполнена перед `series:second` задача будет выполняться.

2.  Сохранить *gulpfile.js*.

3.  В **обозревателе решений**, щелкните правой кнопкой мыши *gulpfile.js* и выберите **Task Runner Explorer** если он еще не открыт.

4.  В **Task Runner Explorer**, щелкните правой кнопкой мыши **серии** и выберите **запуска**.

    ![Task Runner Explorer выполнения ряда задач](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a>IntelliSense

IntelliSense обеспечивает Автозавершение кода, описания параметров и другие возможности для повышения производительности и сокращения ошибок. Задачи gulp создаются на языке JavaScript; Таким образом IntelliSense можно предоставить помощь при разработке. При работе с JavaScript, IntelliSense выводит списки объектов, функций, свойств и параметров, доступных на основании текущего контекста. Выберите вариант кода из всплывающего списка, формируемого IntelliSense для завершения кода.

![gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

Дополнительные сведения о технологии IntelliSense см. в разделе [IntelliSense для JavaScript](/visualstudio/ide/javascript-intellisense).

## <a name="development-staging-and-production-environments"></a>Разработки, промежуточной и рабочей средах

При Gulp используется для оптимизации клиентские файлы для промежуточных и производственных, обработанные файлы сохраняются в локальной папке промежуточной и рабочей. *_Layout.cshtml* файле используется **среды** предоставляют две различные версии файлов CSS вспомогательная функция тега. Одна версия файлов CSS предназначен для разработки и другая версия оптимизирована для промежуточной и рабочей. В Visual Studio 2017, при изменении **ASPNETCORE_ENVIRONMENT** переменную среды, чтобы `Production`, Visual Studio создаст веб-приложения и ссылку для свернутого CSS-файл. Следующая разметка показывает **среды** содержащий теги ссылок для вспомогательных функций тегов `Development` CSS файлов и минифицированные `Staging, Production` файлов CSS.

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

Чтобы переключиться между компиляция для разных сред, измените **ASPNETCORE_ENVIRONMENT** значение переменной среды.

1.  В **Task Runner Explorer**, убедитесь, что **min** задача настроена для запуска **перед построить**.

2.  В **обозревателе решений**, щелкните правой кнопкой мыши имя проекта и выберите **свойства**.

    Откроется окно свойств веб-приложения.

3.  Откройте вкладку **Отладка**.

4.  Установите для параметра **: среда внешнего размещения** переменную среды, чтобы `Production`.

5.  Нажмите клавишу **F5** для запуска приложения в браузере.

6.  В окне браузера щелкните страницу правой кнопкой мыши и выберите **Просмотр исходного кода** для просмотра HTML-код для страницы.

    Обратите внимание, что ссылки на таблицы стилей пункт минифицированные CSS-файл.

7.  Закройте браузер, чтобы остановить веб-приложение.

8.  В Visual Studio, вернитесь в окно свойств веб-приложения и измените **: среда внешнего размещения** переменной среды обратно в `Development`.

9.  Нажмите клавишу **F5** для повторного запуска приложения в браузере.

10. В окне браузера щелкните страницу правой кнопкой мыши и выберите **Просмотр исходного кода** для HTML-код для страницы.

    Обратите внимание, что таблица стилей ссылки указывают на unminified версиям файлов CSS.

Дополнительные сведения о средах в ASP.NET Core, см. в разделе [использование нескольких сред](../fundamentals/environments.md).

## <a name="task-and-module-details"></a>Сведения о задаче и модуля

Задачу Gulp регистрируется с именем функции. Зависимости можно указать, если другие задачи должна быть запущена перед текущей задачи. Дополнительные функции дают возможность запуска и просмотрите задачи Gulp, а также в качестве источника указывается (*src*) и назначения (*dest*) изменения файлов. Ниже перечислены основные функции Gulp API.

|Функция gulp|Синтаксис|Описание|
|---   |--- |--- |
|задача  |`gulp.task(name[, deps], fn) { }`|`task` Функция создает задачу. `name` Параметр определяет имя задачи. `deps` Параметр содержит массив задачи должны быть выполнены перед запуском этой задачи. `fn` Параметр представляет функцию обратного вызова, который выполняет операции задачи.|
|Контрольное значение |`gulp.watch(glob [, opts], tasks) { }`|`watch` Функция отслеживает файлы и выполнения задач при изменении файла. `glob` Параметр `string` или `array` , определяющий, какие файлы для просмотра. `opts` Параметр предоставляет дополнительный файл просмотра параметры.|
|src   |`gulp.src(globs[, options]) { }`|`src` Функция предоставляет файлы, соответствующие значения glob. `glob` Параметр `string` или `array` , определяющий, какие файлы для чтения. `options` Параметр предоставляет дополнительными параметрами файла.|
|dest  |`gulp.dest(path[, options]) { }`|`dest` Функция определяет расположение, к которому файлы могут быть записаны. `path` Параметр — это строка или функция, которая определяет конечную папку. `options` Параметр — это объект, задающий свойства папки выходных данных.|

Дополнительные справочные сведения об API, Gulp, см. в разделе [Gulp Документация API](https://gulpjs.org/API.html).

## <a name="gulp-recipes"></a>Рецепты gulp

Gulp сообщества предоставляет Gulp [рецепты](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md). Этими рецептами состоят из задач Gulp для решения распространенных сценариев.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Документация по gulp](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [Объединение и Минификация в ASP.NET Core](bundling-and-minification.md)
* [Использование Grunt в ASP.NET Core](using-grunt.md)
