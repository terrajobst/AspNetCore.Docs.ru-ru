---
title: В ASP.NET Core с помощью средства Gulp
author: rick-anderson
description: Узнайте, как с помощью средства Gulp в ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/04/2018
uid: client-side/using-gulp
ms.openlocfilehash: 9f6d03a1e8a81bceca15cb1e1aa664c22c31e1d3
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209876"
---
# <a name="use-gulp-in-aspnet-core"></a><span data-ttu-id="c8e9d-103">В ASP.NET Core с помощью средства Gulp</span><span class="sxs-lookup"><span data-stu-id="c8e9d-103">Use Gulp in ASP.NET Core</span></span>

<span data-ttu-id="c8e9d-104">По [Scott Addie](https://scottaddie.com), [Шейн Бойер](https://twitter.com/spboyer), и [сосну Дэвид](https://twitter.com/davidpine7)</span><span class="sxs-lookup"><span data-stu-id="c8e9d-104">By [Scott Addie](https://scottaddie.com), [Shayne Boyer](https://twitter.com/spboyer), and [David Pine](https://twitter.com/davidpine7)</span></span>

<span data-ttu-id="c8e9d-105">В типичных современных веб-приложения процесс построения может сделать следующее.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-105">In a typical modern web app, the build process might:</span></span>

* <span data-ttu-id="c8e9d-106">Объединение и Минификация файлов JavaScript и CSS.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-106">Bundle and minify JavaScript and CSS files.</span></span>
* <span data-ttu-id="c8e9d-107">Запуск средств для вызова задачи объединения и минификации перед каждым построением.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-107">Run tools to call the bundling and minification tasks before each build.</span></span>
* <span data-ttu-id="c8e9d-108">Скомпилируйте МЕНЕЕ или SASS файлов CSS.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-108">Compile LESS or SASS files to CSS.</span></span>
* <span data-ttu-id="c8e9d-109">Скомпилируйте файлы CoffeeScript и TypeScript для JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-109">Compile CoffeeScript or TypeScript files to JavaScript.</span></span>

<span data-ttu-id="c8e9d-110">Объект *запускатель задач* — это средство, которое позволяет автоматизировать выполнение этих задач текущей разработки и многое другое.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-110">A *task runner* is a tool which automates these routine development tasks and more.</span></span> <span data-ttu-id="c8e9d-111">Visual Studio предоставляет встроенную поддержку для двух средства выполнения распространенных задач на основе JavaScript: [Gulp](https://gulpjs.com/) и [Grunt](using-grunt.md).</span><span class="sxs-lookup"><span data-stu-id="c8e9d-111">Visual Studio provides built-in support for two popular JavaScript-based task runners: [Gulp](https://gulpjs.com/) and [Grunt](using-grunt.md).</span></span>

## <a name="gulp"></a><span data-ttu-id="c8e9d-112">Gulp</span><span class="sxs-lookup"><span data-stu-id="c8e9d-112">Gulp</span></span>

<span data-ttu-id="c8e9d-113">Gulp — это на базе JavaScript потоковой передачи построения набор средств для клиентского кода.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-113">Gulp is a JavaScript-based streaming build toolkit for client-side code.</span></span> <span data-ttu-id="c8e9d-114">Обычно он используется для потоковую передачу файлов на стороне клиента через ряд процессов, при активации определенного события в среде построения.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-114">It's commonly used to stream client-side files through a series of processes when a specific event is triggered in a build environment.</span></span> <span data-ttu-id="c8e9d-115">Например, Gulp может использоваться для автоматизации [объединение и Минификация](bundling-and-minification.md) или очистка среды разработки до новой сборки.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-115">For instance, Gulp can be used to automate [bundling and minification](bundling-and-minification.md) or the cleaning of a development environment before a new build.</span></span>

<span data-ttu-id="c8e9d-116">Набор задач Gulp определяется в *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-116">A set of Gulp tasks is defined in *gulpfile.js*.</span></span> <span data-ttu-id="c8e9d-117">Включает в себя модули Gulp следующий код JavaScript с указанием пути к файлам, чтобы ссылаться на будущей задачи:</span><span class="sxs-lookup"><span data-stu-id="c8e9d-117">The following JavaScript includes Gulp modules and specifies file paths to be referenced within the forthcoming tasks:</span></span>

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

<span data-ttu-id="c8e9d-118">Приведенный выше код указывает, какие модули узла являются обязательными.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-118">The above code specifies which Node modules are required.</span></span> <span data-ttu-id="c8e9d-119">`require` Импорты функций каждого модуля, чтобы зависимые задачи можно использовать предоставляемые им возможности.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-119">The `require` function imports each module so that the dependent tasks can utilize their features.</span></span> <span data-ttu-id="c8e9d-120">Каждый из импортированных модулей присваивается переменной.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-120">Each of the imported modules is assigned to a variable.</span></span> <span data-ttu-id="c8e9d-121">Модули могут располагаться либо по имени или пути.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-121">The modules can be located either by name or path.</span></span> <span data-ttu-id="c8e9d-122">В этом примере имя модули `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, и `gulp-uglify` извлекаются по имени.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-122">In this example, the modules named `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, and `gulp-uglify` are retrieved by name.</span></span> <span data-ttu-id="c8e9d-123">Кроме того ряд пути создаются, чтобы расположения файлов CSS и JavaScript можно использовать повторно и ссылки на задачи.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-123">Additionally, a series of paths are created so that the locations of CSS and JavaScript files can be reused and referenced within the tasks.</span></span> <span data-ttu-id="c8e9d-124">В следующей таблице приведены описания модулей, включенных в *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-124">The following table provides descriptions of the modules included in *gulpfile.js*.</span></span>

| <span data-ttu-id="c8e9d-125">Имя модуля</span><span class="sxs-lookup"><span data-stu-id="c8e9d-125">Module Name</span></span> | <span data-ttu-id="c8e9d-126">Описание:</span><span class="sxs-lookup"><span data-stu-id="c8e9d-126">Description</span></span> |
| ----------- | ----------- |
| <span data-ttu-id="c8e9d-127">Gulp</span><span class="sxs-lookup"><span data-stu-id="c8e9d-127">gulp</span></span>        | <span data-ttu-id="c8e9d-128">Система сборки Gulp потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-128">The Gulp streaming build system.</span></span> <span data-ttu-id="c8e9d-129">Дополнительные сведения см. в разделе [gulp](https://www.npmjs.com/package/gulp).</span><span class="sxs-lookup"><span data-stu-id="c8e9d-129">For more information, see [gulp](https://www.npmjs.com/package/gulp).</span></span> |
| <span data-ttu-id="c8e9d-130">rimraf</span><span class="sxs-lookup"><span data-stu-id="c8e9d-130">rimraf</span></span>      | <span data-ttu-id="c8e9d-131">Модуль удаления узла.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-131">A Node deletion module.</span></span> <span data-ttu-id="c8e9d-132">Дополнительные сведения см. в разделе [rimraf](https://www.npmjs.com/package/rimraf).</span><span class="sxs-lookup"><span data-stu-id="c8e9d-132">For more information, see [rimraf](https://www.npmjs.com/package/rimraf).</span></span> |
| <span data-ttu-id="c8e9d-133">gulp concat</span><span class="sxs-lookup"><span data-stu-id="c8e9d-133">gulp-concat</span></span> | <span data-ttu-id="c8e9d-134">Модуль, который объединяет файлы в зависимости от операционной системы символ новой строки.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-134">A module that concatenates files based on the operating system's newline character.</span></span> <span data-ttu-id="c8e9d-135">Дополнительные сведения см. в разделе [gulp concat](https://www.npmjs.com/package/gulp-concat).</span><span class="sxs-lookup"><span data-stu-id="c8e9d-135">For more information, see [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span></span> |
| <span data-ttu-id="c8e9d-136">gulp cssmin</span><span class="sxs-lookup"><span data-stu-id="c8e9d-136">gulp-cssmin</span></span> | <span data-ttu-id="c8e9d-137">Модуль, который уменьшает файлов CSS.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-137">A module that minifies CSS files.</span></span> <span data-ttu-id="c8e9d-138">Дополнительные сведения см. в разделе [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin).</span><span class="sxs-lookup"><span data-stu-id="c8e9d-138">For more information, see [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin).</span></span> |
| <span data-ttu-id="c8e9d-139">gulp uglify</span><span class="sxs-lookup"><span data-stu-id="c8e9d-139">gulp-uglify</span></span> | <span data-ttu-id="c8e9d-140">Модуль, который уменьшает *.js* файлов.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-140">A module that minifies *.js* files.</span></span> <span data-ttu-id="c8e9d-141">Дополнительные сведения см. в разделе [gulp uglify](https://www.npmjs.com/package/gulp-uglify).</span><span class="sxs-lookup"><span data-stu-id="c8e9d-141">For more information, see [gulp-uglify](https://www.npmjs.com/package/gulp-uglify).</span></span> |

<span data-ttu-id="c8e9d-142">После импорта необходимых модулей задачи можно указать.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-142">Once the requisite modules are imported, the tasks can be specified.</span></span> <span data-ttu-id="c8e9d-143">Здесь есть шесть задач зарегистрирован, представлен в следующем примере кода:</span><span class="sxs-lookup"><span data-stu-id="c8e9d-143">Here there are six tasks registered, represented by the following code:</span></span>

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

<span data-ttu-id="c8e9d-144">Ниже приводится объяснение того, задачи, указанные в приведенном выше коде:</span><span class="sxs-lookup"><span data-stu-id="c8e9d-144">The following table provides an explanation of the tasks specified in the code above:</span></span>

|<span data-ttu-id="c8e9d-145">Имя задачи</span><span class="sxs-lookup"><span data-stu-id="c8e9d-145">Task Name</span></span>|<span data-ttu-id="c8e9d-146">Описание</span><span class="sxs-lookup"><span data-stu-id="c8e9d-146">Description</span></span>|
|--- |--- |
|<span data-ttu-id="c8e9d-147">Очистить: js</span><span class="sxs-lookup"><span data-stu-id="c8e9d-147">clean:js</span></span>|<span data-ttu-id="c8e9d-148">Задача, которая использует модуль удаления rimraf узел для удаления минифицированные версию файла site.js.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-148">A task that uses the rimraf Node deletion module to remove the minified version of the site.js file.</span></span>|
|<span data-ttu-id="c8e9d-149">Очистить: css</span><span class="sxs-lookup"><span data-stu-id="c8e9d-149">clean:css</span></span>|<span data-ttu-id="c8e9d-150">Задача, которая использует модуль удаления rimraf узел для удаления минифицированные версию файла site.css.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-150">A task that uses the rimraf Node deletion module to remove the minified version of the site.css file.</span></span>|
|<span data-ttu-id="c8e9d-151">Очистить</span><span class="sxs-lookup"><span data-stu-id="c8e9d-151">clean</span></span>|<span data-ttu-id="c8e9d-152">Задача, которая вызывает `clean:js` задачи, за которым следует `clean:css` задачи.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-152">A task that calls the `clean:js` task, followed by the `clean:css` task.</span></span>|
|<span data-ttu-id="c8e9d-153">MIN:js</span><span class="sxs-lookup"><span data-stu-id="c8e9d-153">min:js</span></span>|<span data-ttu-id="c8e9d-154">Задача, которая уменьшает и Сцепляет всех JS-файлов в папке js.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-154">A task that minifies and concatenates all .js files within the js folder.</span></span> <span data-ttu-id="c8e9d-155">. Файлы min.js исключаются.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-155">The .min.js files are excluded.</span></span>|
|<span data-ttu-id="c8e9d-156">MIN:CSS</span><span class="sxs-lookup"><span data-stu-id="c8e9d-156">min:css</span></span>|<span data-ttu-id="c8e9d-157">Задача, которая уменьшает и объединяет все файлы .css в папке css.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-157">A task that minifies and concatenates all .css files within the css folder.</span></span> <span data-ttu-id="c8e9d-158">. Файлы min.css исключаются.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-158">The .min.css files are excluded.</span></span>|
|<span data-ttu-id="c8e9d-159">min</span><span class="sxs-lookup"><span data-stu-id="c8e9d-159">min</span></span>|<span data-ttu-id="c8e9d-160">Задача, которая вызывает `min:js` задачи, за которым следует `min:css` задачи.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-160">A task that calls the `min:js` task, followed by the `min:css` task.</span></span>|

## <a name="running-default-tasks"></a><span data-ttu-id="c8e9d-161">Выполнение задач по умолчанию</span><span class="sxs-lookup"><span data-stu-id="c8e9d-161">Running default tasks</span></span>

<span data-ttu-id="c8e9d-162">Если вы уже создавали новое веб-приложение, создайте новый проект веб-приложения ASP.NET в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-162">If you haven't already created a new Web app, create a new ASP.NET Web Application project in Visual Studio.</span></span>

1. <span data-ttu-id="c8e9d-163">Откройте *package.json* файла (Добавление Если не существует) и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-163">Open the *package.json* file (add if not there) and add the following.</span></span>

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

2. <span data-ttu-id="c8e9d-164">Добавьте новый файл JavaScript в проект и назовите его *gulpfile.js*, затем скопируйте следующий код.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-164">Add a new JavaScript file to your project and name it *gulpfile.js*, then copy the following code.</span></span>

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

3. <span data-ttu-id="c8e9d-165">В **обозревателе решений**, щелкните правой кнопкой мыши *gulpfile.js*и выберите **Task Runner Explorer**.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-165">In **Solution Explorer**, right-click *gulpfile.js*, and select **Task Runner Explorer**.</span></span>

    ![Откройте диспетчер выполнения задач из обозревателя решений](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)

    <span data-ttu-id="c8e9d-167">**Task Runner Explorer** представлен список задач Gulp.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-167">**Task Runner Explorer** shows the list of Gulp tasks.</span></span> <span data-ttu-id="c8e9d-168">(Может потребоваться щелкнуть **обновить** кнопка, которая появляется слева от имени проекта.)</span><span class="sxs-lookup"><span data-stu-id="c8e9d-168">(You might have to click the **Refresh** button that appears to the left of the project name.)</span></span>

    ![Task Runner Explorer](using-gulp/_static/03-TaskRunnerExplorer.png)

    > [!IMPORTANT]
    > <span data-ttu-id="c8e9d-170">**Task Runner Explorer** пункт контекстного меню отображается только в том случае, если *gulpfile.js* находится в корневом каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-170">The **Task Runner Explorer** context menu item appears only if *gulpfile.js* is in the root project directory.</span></span>

4. <span data-ttu-id="c8e9d-171">Под **задачи** в **Task Runner Explorer**, щелкните правой кнопкой мыши **чистой**и выберите **запуска** во всплывающем меню.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-171">Underneath **Tasks** in **Task Runner Explorer**, right-click **clean**, and select **Run** from the pop-up menu.</span></span>

    ![Задачу "Очистить" Диспетчер выполнения задач](using-gulp/_static/04-TaskRunner-clean.png)

    <span data-ttu-id="c8e9d-173">**Task Runner Explorer** создаст новую вкладку, называемую **чистой** и выполните задачу "Очистить", как оно определено в *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-173">**Task Runner Explorer** will create a new tab named **clean** and execute the clean task as it's defined in *gulpfile.js*.</span></span>

5. <span data-ttu-id="c8e9d-174">Щелкните правой кнопкой мыши **чистой** задач, а затем выберите **привязки** > **перед построить**.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-174">Right-click the **clean** task, then select **Bindings** > **Before Build**.</span></span>

    ![Task Runner Explorer привязки BeforeBuild](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    <span data-ttu-id="c8e9d-176">**Перед построить** привязки настраивает задачу "Очистить" для автоматического запуска перед каждым построением проекта.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-176">The **Before Build** binding configures the clean task to run automatically before each build of the project.</span></span>

<span data-ttu-id="c8e9d-177">Привязки настраиваются с помощью **Task Runner Explorer** хранятся в виде комментария в верхней части вашего *gulpfile.js* и вступают в силу только в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-177">The bindings you set up with **Task Runner Explorer** are stored in the form of a comment at the top of your *gulpfile.js* and are effective only in Visual Studio.</span></span> <span data-ttu-id="c8e9d-178">Другой способ, который не требует Visual Studio — настроить автоматическое выполнение задачи gulp в вашей *.csproj* файл.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-178">An alternative that doesn't require Visual Studio is to configure automatic execution of gulp tasks in your *.csproj* file.</span></span> <span data-ttu-id="c8e9d-179">Например, разместить их в вашей *.csproj* файла:</span><span class="sxs-lookup"><span data-stu-id="c8e9d-179">For example, put this in your *.csproj* file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

<span data-ttu-id="c8e9d-180">Теперь задачу "Очистить" выполняется при запуске проекта в Visual Studio или из командной строки с помощью [запуска dotnet](/dotnet/core/tools/dotnet-run) команды (запустить `npm install` первый).</span><span class="sxs-lookup"><span data-stu-id="c8e9d-180">Now the clean task is executed when you run the project in Visual Studio or from a command prompt using the [dotnet run](/dotnet/core/tools/dotnet-run) command (run `npm install` first).</span></span>

## <a name="defining-and-running-a-new-task"></a><span data-ttu-id="c8e9d-181">Определение и выполнение новой задачи</span><span class="sxs-lookup"><span data-stu-id="c8e9d-181">Defining and running a new task</span></span>

<span data-ttu-id="c8e9d-182">Чтобы определить новая задача Gulp, измените *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-182">To define a new Gulp task, modify *gulpfile.js*.</span></span>

1. <span data-ttu-id="c8e9d-183">Добавьте следующий код JavaScript в конец *gulpfile.js*:</span><span class="sxs-lookup"><span data-stu-id="c8e9d-183">Add the following JavaScript to the end of *gulpfile.js*:</span></span>

    ```javascript
    gulp.task('first', done => {
      console.log('first task! <-----');
      done(); // signal completion
    });
    ```

    <span data-ttu-id="c8e9d-184">Эта задача имеет имя `first`, и он просто отображает строку.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-184">This task is named `first`, and it simply displays a string.</span></span>

2. <span data-ttu-id="c8e9d-185">Сохранить *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-185">Save *gulpfile.js*.</span></span>

3. <span data-ttu-id="c8e9d-186">В **обозревателе решений**, щелкните правой кнопкой мыши *gulpfile.js*и выберите *Task Runner Explorer*.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-186">In **Solution Explorer**, right-click *gulpfile.js*, and select *Task Runner Explorer*.</span></span>

4. <span data-ttu-id="c8e9d-187">В **Task Runner Explorer**, щелкните правой кнопкой мыши **первый**и выберите **запуска**.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-187">In **Task Runner Explorer**, right-click **first**, and select **Run**.</span></span>

    ![Task Runner Explorer выполнения первой задачи](using-gulp/_static/06-TaskRunner-First.png)

    <span data-ttu-id="c8e9d-189">Выходной текст.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-189">The output text is displayed.</span></span> <span data-ttu-id="c8e9d-190">Для просмотра примеров на основе общих сценариев, см. в разделе [Gulp рецепты](#gulp-recipes).</span><span class="sxs-lookup"><span data-stu-id="c8e9d-190">To see examples based on common scenarios, see [Gulp Recipes](#gulp-recipes).</span></span>

## <a name="defining-and-running-tasks-in-a-series"></a><span data-ttu-id="c8e9d-191">Определение и выполнение задач в последовательность</span><span class="sxs-lookup"><span data-stu-id="c8e9d-191">Defining and running tasks in a series</span></span>

<span data-ttu-id="c8e9d-192">При выполнении нескольких задач, задачи выполняются параллельно по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-192">When you run multiple tasks, the tasks run concurrently by default.</span></span> <span data-ttu-id="c8e9d-193">Тем не менее, если необходимо выполнить задачи в определенном порядке, необходимо указать когда каждая задача будет завершена, а также как какие задачи зависят от завершения другой задачи.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-193">However, if you need to run tasks in a specific order, you must specify when each task is complete, as well as which tasks depend on the completion of another task.</span></span>

1. <span data-ttu-id="c8e9d-194">Чтобы определить последовательность задач для выполнения в порядке, замените `first` задачу, которая добавляется выше в *gulpfile.js* следующим:</span><span class="sxs-lookup"><span data-stu-id="c8e9d-194">To define a series of tasks to run in order, replace the `first` task that you added above in *gulpfile.js* with the following:</span></span>

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

    <span data-ttu-id="c8e9d-195">Теперь у вас есть три задачи: `series:first`, `series:second`, и `series`.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-195">You now have three tasks: `series:first`, `series:second`, and `series`.</span></span> <span data-ttu-id="c8e9d-196">`series:second` Задача включает второй параметр, указывающий массив задач запуска и завершения перед `series:second` задача будет выполняться.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-196">The `series:second` task includes a second parameter which specifies an array of tasks to be run and completed before the `series:second` task will run.</span></span> <span data-ttu-id="c8e9d-197">Как указано в коде выше, только `series:first` задача должна быть выполнена перед `series:second` задача будет выполняться.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-197">As specified in the code above, only the `series:first` task must be completed before the `series:second` task will run.</span></span>

2. <span data-ttu-id="c8e9d-198">Сохранить *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-198">Save *gulpfile.js*.</span></span>

3. <span data-ttu-id="c8e9d-199">В **обозревателе решений**, щелкните правой кнопкой мыши *gulpfile.js* и выберите **Task Runner Explorer** если он еще не открыт.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-199">In **Solution Explorer**, right-click *gulpfile.js* and select **Task Runner Explorer** if it isn't already open.</span></span>

4. <span data-ttu-id="c8e9d-200">В **Task Runner Explorer**, щелкните правой кнопкой мыши **серии** и выберите **запуска**.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-200">In **Task Runner Explorer**, right-click **series** and select **Run**.</span></span>

    ![Task Runner Explorer выполнения ряда задач](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a><span data-ttu-id="c8e9d-202">IntelliSense</span><span class="sxs-lookup"><span data-stu-id="c8e9d-202">IntelliSense</span></span>

<span data-ttu-id="c8e9d-203">IntelliSense обеспечивает Автозавершение кода, описания параметров и другие возможности для повышения производительности и сокращения ошибок.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-203">IntelliSense provides code completion, parameter descriptions, and other features to boost productivity and to decrease errors.</span></span> <span data-ttu-id="c8e9d-204">Задачи gulp создаются на языке JavaScript; Таким образом IntelliSense можно предоставить помощь при разработке.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-204">Gulp tasks are written in JavaScript; therefore, IntelliSense can provide assistance while developing.</span></span> <span data-ttu-id="c8e9d-205">При работе с JavaScript, IntelliSense выводит списки объектов, функций, свойств и параметров, доступных на основании текущего контекста.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-205">As you work with JavaScript, IntelliSense lists the objects, functions, properties, and parameters that are available based on your current context.</span></span> <span data-ttu-id="c8e9d-206">Выберите вариант кода из всплывающего списка, формируемого IntelliSense для завершения кода.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-206">Select a coding option from the pop-up list provided by IntelliSense to complete the code.</span></span>

![gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

<span data-ttu-id="c8e9d-208">Дополнительные сведения о технологии IntelliSense см. в разделе [IntelliSense для JavaScript](/visualstudio/ide/javascript-intellisense).</span><span class="sxs-lookup"><span data-stu-id="c8e9d-208">For more information about IntelliSense, see [JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense).</span></span>

## <a name="development-staging-and-production-environments"></a><span data-ttu-id="c8e9d-209">Разработки, промежуточной и рабочей средах</span><span class="sxs-lookup"><span data-stu-id="c8e9d-209">Development, staging, and production environments</span></span>

<span data-ttu-id="c8e9d-210">При Gulp используется для оптимизации клиентские файлы для промежуточных и производственных, обработанные файлы сохраняются в локальной папке промежуточной и рабочей.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-210">When Gulp is used to optimize client-side files for staging and production, the processed files are saved to a local staging and production location.</span></span> <span data-ttu-id="c8e9d-211">*_Layout.cshtml* файле используется **среды** предоставляют две различные версии файлов CSS вспомогательная функция тега.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-211">The *_Layout.cshtml* file uses the **environment** tag helper to provide two different versions of CSS files.</span></span> <span data-ttu-id="c8e9d-212">Одна версия файлов CSS предназначен для разработки и другая версия оптимизирована для промежуточной и рабочей.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-212">One version of CSS files is for development and the other version is optimized for both staging and production.</span></span> <span data-ttu-id="c8e9d-213">В Visual Studio 2017, при изменении **ASPNETCORE_ENVIRONMENT** переменную среды, чтобы `Production`, Visual Studio создаст веб-приложения и ссылку для свернутого CSS-файл.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-213">In Visual Studio 2017, when you change the **ASPNETCORE_ENVIRONMENT** environment variable to `Production`, Visual Studio will build the Web app and link to the minimized CSS files.</span></span> <span data-ttu-id="c8e9d-214">Следующая разметка показывает **среды** содержащий теги ссылок для вспомогательных функций тегов `Development` CSS файлов и минифицированные `Staging, Production` файлов CSS.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-214">The following markup shows the **environment** tag helpers containing link tags to the `Development` CSS files and the minified `Staging, Production` CSS files.</span></span>

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

## <a name="switching-between-environments"></a><span data-ttu-id="c8e9d-215">Переключение между средами</span><span class="sxs-lookup"><span data-stu-id="c8e9d-215">Switching between environments</span></span>

<span data-ttu-id="c8e9d-216">Чтобы переключиться между компиляция для разных сред, измените **ASPNETCORE_ENVIRONMENT** значение переменной среды.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-216">To switch between compiling for different environments, modify the **ASPNETCORE_ENVIRONMENT** environment variable's value.</span></span>

1. <span data-ttu-id="c8e9d-217">В **Task Runner Explorer**, убедитесь, что **min** задача настроена для запуска **перед построить**.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-217">In **Task Runner Explorer**, verify that the **min** task has been set to run **Before Build**.</span></span>

2. <span data-ttu-id="c8e9d-218">В **обозревателе решений**, щелкните правой кнопкой мыши имя проекта и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-218">In **Solution Explorer**, right-click the project name and select **Properties**.</span></span>

    <span data-ttu-id="c8e9d-219">Откроется окно свойств веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-219">The property sheet for the Web app is displayed.</span></span>

3. <span data-ttu-id="c8e9d-220">Откройте вкладку **Отладка**.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-220">Click the **Debug** tab.</span></span>

4. <span data-ttu-id="c8e9d-221">Установите для параметра **: среда внешнего размещения** переменную среды, чтобы `Production`.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-221">Set the value of the **Hosting:Environment** environment variable to `Production`.</span></span>

5. <span data-ttu-id="c8e9d-222">Нажмите клавишу **F5** для запуска приложения в браузере.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-222">Press **F5** to run the application in a browser.</span></span>

6. <span data-ttu-id="c8e9d-223">В окне браузера щелкните страницу правой кнопкой мыши и выберите **Просмотр исходного кода** для просмотра HTML-код для страницы.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-223">In the browser window, right-click the page and select **View Source** to view the HTML for the page.</span></span>

    <span data-ttu-id="c8e9d-224">Обратите внимание, что ссылки на таблицы стилей пункт минифицированные CSS-файл.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-224">Notice that the stylesheet links point to the minified CSS files.</span></span>

7. <span data-ttu-id="c8e9d-225">Закройте браузер, чтобы остановить веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-225">Close the browser to stop the Web app.</span></span>

8. <span data-ttu-id="c8e9d-226">В Visual Studio, вернитесь в окно свойств веб-приложения и измените **: среда внешнего размещения** переменной среды обратно в `Development`.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-226">In Visual Studio, return to the property sheet for the Web app and change the **Hosting:Environment** environment variable back to `Development`.</span></span>

9. <span data-ttu-id="c8e9d-227">Нажмите клавишу **F5** для повторного запуска приложения в браузере.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-227">Press **F5** to run the application in a browser again.</span></span>

10. <span data-ttu-id="c8e9d-228">В окне браузера щелкните страницу правой кнопкой мыши и выберите **Просмотр исходного кода** для HTML-код для страницы.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-228">In the browser window, right-click the page and select **View Source** to see the HTML for the page.</span></span>

    <span data-ttu-id="c8e9d-229">Обратите внимание, что таблица стилей ссылки указывают на unminified версиям файлов CSS.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-229">Notice that the stylesheet links point to the unminified versions of the CSS files.</span></span>

<span data-ttu-id="c8e9d-230">Дополнительные сведения о средах в ASP.NET Core, см. в разделе [использование нескольких сред](../fundamentals/environments.md).</span><span class="sxs-lookup"><span data-stu-id="c8e9d-230">For more information related to environments in ASP.NET Core, see [Use multiple environments](../fundamentals/environments.md).</span></span>

## <a name="task-and-module-details"></a><span data-ttu-id="c8e9d-231">Сведения о задаче и модуля</span><span class="sxs-lookup"><span data-stu-id="c8e9d-231">Task and module details</span></span>

<span data-ttu-id="c8e9d-232">Задачу Gulp регистрируется с именем функции.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-232">A Gulp task is registered with a function name.</span></span> <span data-ttu-id="c8e9d-233">Зависимости можно указать, если другие задачи должна быть запущена перед текущей задачи.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-233">You can specify dependencies if other tasks must run before the current task.</span></span> <span data-ttu-id="c8e9d-234">Дополнительные функции дают возможность запуска и просмотрите задачи Gulp, а также в качестве источника указывается (*src*) и назначения (*dest*) изменения файлов.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-234">Additional functions allow you to run and watch the Gulp tasks, as well as set the source (*src*) and destination (*dest*) of the files being modified.</span></span> <span data-ttu-id="c8e9d-235">Ниже перечислены основные функции Gulp API.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-235">The following are the primary Gulp API functions:</span></span>

|<span data-ttu-id="c8e9d-236">Функция gulp</span><span class="sxs-lookup"><span data-stu-id="c8e9d-236">Gulp Function</span></span>|<span data-ttu-id="c8e9d-237">Синтаксис</span><span class="sxs-lookup"><span data-stu-id="c8e9d-237">Syntax</span></span>|<span data-ttu-id="c8e9d-238">Описание</span><span class="sxs-lookup"><span data-stu-id="c8e9d-238">Description</span></span>|
|---   |--- |--- |
|<span data-ttu-id="c8e9d-239">задача</span><span class="sxs-lookup"><span data-stu-id="c8e9d-239">task</span></span>  |`gulp.task(name[, deps], fn) { }`|<span data-ttu-id="c8e9d-240">`task` Функция создает задачу.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-240">The `task` function creates a task.</span></span> <span data-ttu-id="c8e9d-241">`name` Параметр определяет имя задачи.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-241">The `name` parameter defines the name of the task.</span></span> <span data-ttu-id="c8e9d-242">`deps` Параметр содержит массив задачи должны быть выполнены перед запуском этой задачи.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-242">The `deps` parameter contains an array of tasks to be completed before this task runs.</span></span> <span data-ttu-id="c8e9d-243">`fn` Параметр представляет функцию обратного вызова, который выполняет операции задачи.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-243">The `fn` parameter represents a callback function which performs the operations of the task.</span></span>|
|<span data-ttu-id="c8e9d-244">Контрольное значение</span><span class="sxs-lookup"><span data-stu-id="c8e9d-244">watch</span></span> |`gulp.watch(glob [, opts], tasks) { }`|<span data-ttu-id="c8e9d-245">`watch` Функция отслеживает файлы и выполнения задач при изменении файла.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-245">The `watch` function monitors files and runs tasks when a file change occurs.</span></span> <span data-ttu-id="c8e9d-246">`glob` Параметр `string` или `array` , определяющий, какие файлы для просмотра.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-246">The `glob` parameter is a `string` or `array` that determines which files to watch.</span></span> <span data-ttu-id="c8e9d-247">`opts` Параметр предоставляет дополнительный файл просмотра параметры.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-247">The `opts` parameter provides additional file watching options.</span></span>|
|<span data-ttu-id="c8e9d-248">src</span><span class="sxs-lookup"><span data-stu-id="c8e9d-248">src</span></span>   |`gulp.src(globs[, options]) { }`|<span data-ttu-id="c8e9d-249">`src` Функция предоставляет файлы, соответствующие значения glob.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-249">The `src` function provides files that match the glob value(s).</span></span> <span data-ttu-id="c8e9d-250">`glob` Параметр `string` или `array` , определяющий, какие файлы для чтения.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-250">The `glob` parameter is a `string` or `array` that determines which files to read.</span></span> <span data-ttu-id="c8e9d-251">`options` Параметр предоставляет дополнительными параметрами файла.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-251">The `options` parameter provides additional file options.</span></span>|
|<span data-ttu-id="c8e9d-252">dest</span><span class="sxs-lookup"><span data-stu-id="c8e9d-252">dest</span></span>  |`gulp.dest(path[, options]) { }`|<span data-ttu-id="c8e9d-253">`dest` Функция определяет расположение, к которому файлы могут быть записаны.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-253">The `dest` function defines a location to which files can be written.</span></span> <span data-ttu-id="c8e9d-254">`path` Параметр — это строка или функция, которая определяет конечную папку.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-254">The `path` parameter is a string or function that determines the destination folder.</span></span> <span data-ttu-id="c8e9d-255">`options` Параметр — это объект, задающий свойства папки выходных данных.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-255">The `options` parameter is an object that specifies output folder options.</span></span>|

<span data-ttu-id="c8e9d-256">Дополнительные справочные сведения об API, Gulp, см. в разделе [Gulp Документация API](https://gulpjs.org/API.html).</span><span class="sxs-lookup"><span data-stu-id="c8e9d-256">For additional Gulp API reference information, see [Gulp Docs API](https://gulpjs.org/API.html).</span></span>

## <a name="gulp-recipes"></a><span data-ttu-id="c8e9d-257">Рецепты gulp</span><span class="sxs-lookup"><span data-stu-id="c8e9d-257">Gulp recipes</span></span>

<span data-ttu-id="c8e9d-258">Gulp сообщества предоставляет Gulp [рецепты](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span><span class="sxs-lookup"><span data-stu-id="c8e9d-258">The Gulp community provides Gulp [Recipes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span></span> <span data-ttu-id="c8e9d-259">Этими рецептами состоят из задач Gulp для решения распространенных сценариев.</span><span class="sxs-lookup"><span data-stu-id="c8e9d-259">These recipes consist of Gulp tasks to address common scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c8e9d-260">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c8e9d-260">Additional resources</span></span>

* [<span data-ttu-id="c8e9d-261">Документация по gulp</span><span class="sxs-lookup"><span data-stu-id="c8e9d-261">Gulp documentation</span></span>](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [<span data-ttu-id="c8e9d-262">Объединение и Минификация в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c8e9d-262">Bundling and minification in ASP.NET Core</span></span>](bundling-and-minification.md)
* [<span data-ttu-id="c8e9d-263">Использование Grunt в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c8e9d-263">Use Grunt in ASP.NET Core</span></span>](using-grunt.md)
