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
# <a name="introduction-to-using-gulp-in-aspnet-core"></a><span data-ttu-id="e2b1f-103">Общие сведения об использовании Gulp в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e2b1f-103">Introduction to using Gulp in ASP.NET Core</span></span> 

<span data-ttu-id="e2b1f-104">По [Reitan Эрик](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [рот Daniel](https://github.com/danroth27), и [Бойера Shayne](https://twitter.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="e2b1f-104">By [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Daniel Roth](https://github.com/danroth27), and [Shayne Boyer](https://twitter.com/spboyer)</span></span>

<span data-ttu-id="e2b1f-105">В обычной современных веб-приложения процесс построения может:</span><span class="sxs-lookup"><span data-stu-id="e2b1f-105">In a typical modern web app, the build process might:</span></span>

* <span data-ttu-id="e2b1f-106">Объединение и уменьшения файлы JavaScript и CSS.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-106">Bundle and minify JavaScript and CSS files.</span></span>
* <span data-ttu-id="e2b1f-107">Запуск средств для вызова объединение и Минификация задачи перед каждой сборкой.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-107">Run tools to call the bundling and minification tasks before each build.</span></span>
* <span data-ttu-id="e2b1f-108">Скомпилируйте МЕНЕЕ или SASS файлов CSS.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-108">Compile LESS or SASS files to CSS.</span></span>
* <span data-ttu-id="e2b1f-109">Скомпилируйте файлы CoffeeScript и TypeScript в код JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-109">Compile CoffeeScript or TypeScript files to JavaScript.</span></span>

<span data-ttu-id="e2b1f-110">Объект *средство запуска задач* — это средство, автоматизирующее этих задач процедуры разработки и многое другое.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-110">A *task runner* is a tool which automates these routine development tasks and more.</span></span> <span data-ttu-id="e2b1f-111">Visual Studio предоставляет встроенную поддержку для средства запуска два распространенных задач на основе JavaScript: [Gulp](https://gulpjs.com/) и [Grunt](using-grunt.md).</span><span class="sxs-lookup"><span data-stu-id="e2b1f-111">Visual Studio provides built-in support for two popular JavaScript-based task runners: [Gulp](https://gulpjs.com/) and [Grunt](using-grunt.md).</span></span>

## <a name="gulp"></a><span data-ttu-id="e2b1f-112">gulp</span><span class="sxs-lookup"><span data-stu-id="e2b1f-112">Gulp</span></span>

<span data-ttu-id="e2b1f-113">Gulp — это на базе JavaScript потоковой передачи построения набор средств для клиентского кода.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-113">Gulp is a JavaScript-based streaming build toolkit for client-side code.</span></span> <span data-ttu-id="e2b1f-114">Обычно он используется для потоковой передачи клиентские файлы через ряд процессов при активации определенного события в среде построения.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-114">It's commonly used to stream client-side files through a series of processes when a specific event is triggered in a build environment.</span></span> <span data-ttu-id="e2b1f-115">Например, Gulp можно использовать для автоматизации [объединение и Минификация](bundling-and-minification.md) или очистки среде разработки до новой сборки.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-115">For instance, Gulp can be used to automate [bundling and minification](bundling-and-minification.md) or the cleansing of a development environment before a new build.</span></span>

<span data-ttu-id="e2b1f-116">Набор задач Gulp определяется в *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-116">A set of Gulp tasks is defined in *gulpfile.js*.</span></span> <span data-ttu-id="e2b1f-117">Следующие JavaScript включает Gulp модулей, а также задает пути к файлам, должно быть указано в дальнейшем будет обеспечена поддержка задачи:</span><span class="sxs-lookup"><span data-stu-id="e2b1f-117">The following JavaScript includes Gulp modules and specifies file paths to be referenced within the forthcoming tasks:</span></span>

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

<span data-ttu-id="e2b1f-118">Приведенный выше код указывает, какой узел модули являются обязательными.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-118">The above code specifies which Node modules are required.</span></span> <span data-ttu-id="e2b1f-119">`require` Импорта функций каждого модуля, чтобы зависимые задачи можно использовать предоставляемые им возможности.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-119">The `require` function imports each module so that the dependent tasks can utilize their features.</span></span> <span data-ttu-id="e2b1f-120">Каждый из импортированных модулей присваивается переменной.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-120">Each of the imported modules is assigned to a variable.</span></span> <span data-ttu-id="e2b1f-121">Модули могут располагаться по имени или пути.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-121">The modules can be located either by name or path.</span></span> <span data-ttu-id="e2b1f-122">В этом примере имя модули `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, и `gulp-uglify` получить по имени.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-122">In this example, the modules named `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, and `gulp-uglify` are retrieved by name.</span></span> <span data-ttu-id="e2b1f-123">Кроме того ряд пути создаются, расположения файлов CSS и JavaScript можно использовать повторно и ссылки на задачи.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-123">Additionally, a series of paths are created so that the locations of CSS and JavaScript files can be reused and referenced within the tasks.</span></span> <span data-ttu-id="e2b1f-124">В следующей таблице приведены описания модулей, включенных в *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-124">The following table provides descriptions of the modules included in *gulpfile.js*.</span></span>

|<span data-ttu-id="e2b1f-125">Имя модуля</span><span class="sxs-lookup"><span data-stu-id="e2b1f-125">Module Name</span></span>|<span data-ttu-id="e2b1f-126">Описание:</span><span class="sxs-lookup"><span data-stu-id="e2b1f-126">Description</span></span>|
|---|---|
|<span data-ttu-id="e2b1f-127">gulp</span><span class="sxs-lookup"><span data-stu-id="e2b1f-127">gulp</span></span>|<span data-ttu-id="e2b1f-128">Gulp потоковой передачи системы сборки.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-128">The Gulp streaming build system.</span></span> <span data-ttu-id="e2b1f-129">Дополнительные сведения см. в разделе [gulp](https://www.npmjs.com/package/gulp).</span><span class="sxs-lookup"><span data-stu-id="e2b1f-129">For more information, see [gulp](https://www.npmjs.com/package/gulp).</span></span>|
|<span data-ttu-id="e2b1f-130">rimraf</span><span class="sxs-lookup"><span data-stu-id="e2b1f-130">rimraf</span></span>|<span data-ttu-id="e2b1f-131">Модуль удаления узла.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-131">A Node deletion module.</span></span> <span data-ttu-id="e2b1f-132">Дополнительные сведения см. в разделе [rimraf](https://www.npmjs.com/package/rimraf).</span><span class="sxs-lookup"><span data-stu-id="e2b1f-132">For more information, see [rimraf](https://www.npmjs.com/package/rimraf).</span></span>|
|<span data-ttu-id="e2b1f-133">gulp concat</span><span class="sxs-lookup"><span data-stu-id="e2b1f-133">gulp-concat</span></span>|<span data-ttu-id="e2b1f-134">Модуль, который объединяет файлы в зависимости от операционной системы символ перевода строки.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-134">A module that concatenates files based on the operating system’s newline character.</span></span> <span data-ttu-id="e2b1f-135">Дополнительные сведения см. в разделе [gulp concat](https://www.npmjs.com/package/gulp-concat).</span><span class="sxs-lookup"><span data-stu-id="e2b1f-135">For more information, see [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span></span>|
|<span data-ttu-id="e2b1f-136">gulp cssmin</span><span class="sxs-lookup"><span data-stu-id="e2b1f-136">gulp-cssmin</span></span>|<span data-ttu-id="e2b1f-137">Модуль, который уменьшает CSS-файлах.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-137">A module that minifies CSS files.</span></span> <span data-ttu-id="e2b1f-138">Дополнительные сведения см. в разделе [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin).</span><span class="sxs-lookup"><span data-stu-id="e2b1f-138">For more information, see [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin).</span></span>|
|<span data-ttu-id="e2b1f-139">gulp uglify</span><span class="sxs-lookup"><span data-stu-id="e2b1f-139">gulp-uglify</span></span>|<span data-ttu-id="e2b1f-140">Модуль, который уменьшает *.js* файлов.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-140">A module that minifies *.js* files.</span></span> <span data-ttu-id="e2b1f-141">Дополнительные сведения см. в разделе [gulp uglify](https://www.npmjs.com/package/gulp-uglify).</span><span class="sxs-lookup"><span data-stu-id="e2b1f-141">For more information, see [gulp-uglify](https://www.npmjs.com/package/gulp-uglify).</span></span>|

<span data-ttu-id="e2b1f-142">После импорта модулей необходимые задачи может быть указан.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-142">Once the requisite modules are imported, the tasks can be specified.</span></span> <span data-ttu-id="e2b1f-143">Ниже приведены шесть задач зарегистрирован, представленный в следующем примере кода:</span><span class="sxs-lookup"><span data-stu-id="e2b1f-143">Here there are six tasks registered, represented by the following code:</span></span>

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

<span data-ttu-id="e2b1f-144">В следующей таблице приведены пояснения задачи, указанные в приведенном выше коде:</span><span class="sxs-lookup"><span data-stu-id="e2b1f-144">The following table provides an explanation of the tasks specified in the code above:</span></span>

|<span data-ttu-id="e2b1f-145">Имя задачи</span><span class="sxs-lookup"><span data-stu-id="e2b1f-145">Task Name</span></span>|<span data-ttu-id="e2b1f-146">Описание:</span><span class="sxs-lookup"><span data-stu-id="e2b1f-146">Description</span></span>|
|--- |--- |
|<span data-ttu-id="e2b1f-147">Очистить: js</span><span class="sxs-lookup"><span data-stu-id="e2b1f-147">clean:js</span></span>|<span data-ttu-id="e2b1f-148">Задача, которая использует модуль удаления rimraf узел для удаления уменьшенная версия файла site.js.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-148">A task that uses the rimraf Node deletion module to remove the minified version of the site.js file.</span></span>|
|<span data-ttu-id="e2b1f-149">Очистить: css</span><span class="sxs-lookup"><span data-stu-id="e2b1f-149">clean:css</span></span>|<span data-ttu-id="e2b1f-150">Задача, которая использует модуль удаления rimraf узел для удаления уменьшенная версия файла site.css.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-150">A task that uses the rimraf Node deletion module to remove the minified version of the site.css file.</span></span>|
|<span data-ttu-id="e2b1f-151">Очистка</span><span class="sxs-lookup"><span data-stu-id="e2b1f-151">clean</span></span>|<span data-ttu-id="e2b1f-152">Задача, которая вызывает `clean:js` задач, за которым следует `clean:css` задачи.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-152">A task that calls the `clean:js` task, followed by the `clean:css` task.</span></span>|
|<span data-ttu-id="e2b1f-153">MIN:js</span><span class="sxs-lookup"><span data-stu-id="e2b1f-153">min:js</span></span>|<span data-ttu-id="e2b1f-154">Задача, которая уменьшает и объединяет все JS-файлов в папке js.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-154">A task that minifies and concatenates all .js files within the js folder.</span></span> <span data-ttu-id="e2b1f-155">. Min.js файлы исключаются.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-155">The .min.js files are excluded.</span></span>|
|<span data-ttu-id="e2b1f-156">MIN:CSS</span><span class="sxs-lookup"><span data-stu-id="e2b1f-156">min:css</span></span>|<span data-ttu-id="e2b1f-157">Задача, которая уменьшает и объединение всех файлов CSS-файлах в папке css.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-157">A task that minifies and concatenates all .css files within the css folder.</span></span> <span data-ttu-id="e2b1f-158">. Min.css файлы исключаются.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-158">The .min.css files are excluded.</span></span>|
|<span data-ttu-id="e2b1f-159">min</span><span class="sxs-lookup"><span data-stu-id="e2b1f-159">min</span></span>|<span data-ttu-id="e2b1f-160">Задача, которая вызывает `min:js` задач, за которым следует `min:css` задачи.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-160">A task that calls the `min:js` task, followed by the `min:css` task.</span></span>|

## <a name="running-default-tasks"></a><span data-ttu-id="e2b1f-161">Выполнение задач по умолчанию</span><span class="sxs-lookup"><span data-stu-id="e2b1f-161">Running default tasks</span></span>

<span data-ttu-id="e2b1f-162">Если вы еще не создали уже новое веб-приложение, создайте новый проект веб-приложения ASP.NET в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-162">If you haven’t already created a new Web app, create a new ASP.NET Web Application project in Visual Studio.</span></span>

1.  <span data-ttu-id="e2b1f-163">Добавьте новый файл JavaScript в проект и назовите его *gulpfile.js*, затем скопируйте следующий код.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-163">Add a new JavaScript file to your project and name it *gulpfile.js*, then copy the following code.</span></span>

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

2.  <span data-ttu-id="e2b1f-164">Откройте *package.json* файла (Добавление Если не существует) и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-164">Open the *package.json* file (add if not there) and add the following.</span></span>

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

3.  <span data-ttu-id="e2b1f-165">В **обозревателе решений**, щелкните правой кнопкой мыши *gulpfile.js*и выберите **Task Runner Explorer**.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-165">In **Solution Explorer**, right-click *gulpfile.js*, and select **Task Runner Explorer**.</span></span>
    
    ![Откройте диспетчер выполнения задач с помощью обозревателя решений](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    <span data-ttu-id="e2b1f-167">**Задача Runner Explorer** представлен список задач Gulp.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-167">**Task Runner Explorer** shows the list of Gulp tasks.</span></span> <span data-ttu-id="e2b1f-168">(Может потребоваться нажать кнопку **обновление** , расположенную слева от имени проекта.)</span><span class="sxs-lookup"><span data-stu-id="e2b1f-168">(You might have to click the **Refresh** button that appears to the left of the project name.)</span></span>
    
    ![Диспетчер выполнения задач](using-gulp/_static/03-TaskRunnerExplorer.png)

4.  <span data-ttu-id="e2b1f-170">Под разделом **задачи** в **Task Runner Explorer**, щелкните правой кнопкой мыши **чистой**и выберите **запуска** из всплывающего меню.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-170">Underneath **Tasks** in **Task Runner Explorer**, right-click **clean**, and select **Run** from the pop-up menu.</span></span>

    ![Задачу "Очистить" Диспетчер выполнения задач](using-gulp/_static/04-TaskRunner-clean.png)

    <span data-ttu-id="e2b1f-172">**Задача Runner Explorer** создается новая вкладка с именем **чистой** и выполните задачу "Очистить", как оно определено в *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-172">**Task Runner Explorer** will create a new tab named **clean** and execute the clean task as it's defined in *gulpfile.js*.</span></span>

5.  <span data-ttu-id="e2b1f-173">Щелкните правой кнопкой мыши **чистой** задач, а затем выберите **привязки** > **перед построить**.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-173">Right-click the **clean** task, then select **Bindings** > **Before Build**.</span></span>

    ![Привязка BeforeBuild Task Runner Explorer](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    <span data-ttu-id="e2b1f-175">**Перед построить** привязки настраивает задачу "Очистить" для автоматического запуска перед каждой сборке проекта.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-175">The **Before Build** binding configures the clean task to run automatically before each build of the project.</span></span>

<span data-ttu-id="e2b1f-176">Привязки настраиваются с помощью **Task Runner Explorer** хранятся в виде комментариев в верхней части вашего *gulpfile.js* и вступают в силу только в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-176">The bindings you set up with **Task Runner Explorer** are stored in the form of a comment at the top of your *gulpfile.js* and are effective only in Visual Studio.</span></span> <span data-ttu-id="e2b1f-177">Альтернатива, которой не требуется Visual Studio — Настройка автоматического выполнения задач gulp в вашей *.csproj* файла.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-177">An alternative that doesn't require Visual Studio is to configure automatic execution of gulp tasks in your *.csproj* file.</span></span> <span data-ttu-id="e2b1f-178">Например, поместите ее ваш *.csproj* файла:</span><span class="sxs-lookup"><span data-stu-id="e2b1f-178">For example, put this in your *.csproj* file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

<span data-ttu-id="e2b1f-179">Теперь задачу "Очистить" выполняется при запуске проекта в Visual Studio или из командной строки с помощью `dotnet run` команда (запустить `npm install` первой).</span><span class="sxs-lookup"><span data-stu-id="e2b1f-179">Now the clean task is executed when you run the project in Visual Studio or from a command prompt using the `dotnet run` command (run `npm install` first).</span></span>

## <a name="defining-and-running-a-new-task"></a><span data-ttu-id="e2b1f-180">Определение и выполнение нового задания</span><span class="sxs-lookup"><span data-stu-id="e2b1f-180">Defining and running a new task</span></span>

<span data-ttu-id="e2b1f-181">Чтобы определить новую задачу Gulp, измените *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-181">To define a new Gulp task, modify *gulpfile.js*.</span></span>

1.  <span data-ttu-id="e2b1f-182">Добавление следующего кода JavaScript в конец *gulpfile.js*:</span><span class="sxs-lookup"><span data-stu-id="e2b1f-182">Add the following JavaScript to the end of *gulpfile.js*:</span></span>

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    <span data-ttu-id="e2b1f-183">Эта задача называется `first`, и он просто отображает строку.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-183">This task is named `first`, and it simply displays a string.</span></span>

2.  <span data-ttu-id="e2b1f-184">Сохранить *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-184">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="e2b1f-185">В **обозревателе решений**, щелкните правой кнопкой мыши *gulpfile.js*и выберите *Task Runner Explorer*.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-185">In **Solution Explorer**, right-click *gulpfile.js*, and select *Task Runner Explorer*.</span></span>

4.  <span data-ttu-id="e2b1f-186">В **Task Runner Explorer**, щелкните правой кнопкой мыши **первый**и выберите **запуска**.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-186">In **Task Runner Explorer**, right-click **first**, and select **Run**.</span></span>

    ![Task Runner Explorer выполнения первой задачи](using-gulp/_static/06-TaskRunner-First.png)

    <span data-ttu-id="e2b1f-188">Вы увидите, что отображается выходного текста.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-188">You’ll see that the output text is displayed.</span></span> <span data-ttu-id="e2b1f-189">Если вы заинтересованы в примерах, основанные на общих сценариях, в разделе Gulp рецептами.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-189">If you are interested in examples based on a common scenario, see Gulp Recipes.</span></span>

## <a name="defining-and-running-tasks-in-a-series"></a><span data-ttu-id="e2b1f-190">Определение и выполнение задач в ряд</span><span class="sxs-lookup"><span data-stu-id="e2b1f-190">Defining and running tasks in a series</span></span>

<span data-ttu-id="e2b1f-191">При выполнении нескольких задач одновременно выполняются задачи по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-191">When you run multiple tasks, the tasks run concurrently by default.</span></span> <span data-ttu-id="e2b1f-192">Тем не менее, если необходимо выполнить задачи в определенном порядке, необходимо указать каждую задачу после завершения операции, а также как какие задачи зависят от завершения другой задачи.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-192">However, if you need to run tasks in a specific order, you must specify when each task is complete, as well as which tasks depend on the completion of another task.</span></span>

1.  <span data-ttu-id="e2b1f-193">Чтобы определить последовательность задач для выполнения в порядке, замените `first` задачи, добавленные в *gulpfile.js* со следующим:</span><span class="sxs-lookup"><span data-stu-id="e2b1f-193">To define a series of tasks to run in order, replace the `first` task that you added above in *gulpfile.js* with the following:</span></span>

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    <span data-ttu-id="e2b1f-194">Теперь у вас есть три задачи: `series:first`, `series:second`, и `series`.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-194">You now have three tasks: `series:first`, `series:second`, and `series`.</span></span> <span data-ttu-id="e2b1f-195">`series:second` Задача включает второй параметр, указывающий массив задачи должен быть запущен и завершена до `series:second` задача будет выполняться.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-195">The `series:second` task includes a second parameter which specifies an array of tasks to be run and completed before the `series:second` task will run.</span></span> <span data-ttu-id="e2b1f-196">Как указано в коде выше, только `series:first` задача должна быть выполнена перед `series:second` задача будет выполняться.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-196">As specified in the code above, only the `series:first` task must be completed before the `series:second` task will run.</span></span>

2.  <span data-ttu-id="e2b1f-197">Сохранить *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-197">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="e2b1f-198">В **обозревателе решений**, щелкните правой кнопкой мыши *gulpfile.js* и выберите **Task Runner Explorer** , если он еще не открыт.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-198">In **Solution Explorer**, right-click *gulpfile.js* and select **Task Runner Explorer** if it isn't already open.</span></span>

4.  <span data-ttu-id="e2b1f-199">В **Task Runner Explorer**, щелкните правой кнопкой мыши **серии** и выберите **запуска**.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-199">In **Task Runner Explorer**, right-click **series** and select **Run**.</span></span>

    ![Task Runner Explorer выполнения ряда задач](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a><span data-ttu-id="e2b1f-201">IntelliSense</span><span class="sxs-lookup"><span data-stu-id="e2b1f-201">IntelliSense</span></span>

<span data-ttu-id="e2b1f-202">IntelliSense предоставляет дополнение кода, описания параметров и другие возможности для повышения производительности и сокращения ошибок.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-202">IntelliSense provides code completion, parameter descriptions, and other features to boost productivity and to decrease errors.</span></span> <span data-ttu-id="e2b1f-203">Gulp задачи создаются на языке JavaScript; Таким образом технология IntelliSense может оказывать помощь при разработке.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-203">Gulp tasks are written in JavaScript; therefore, IntelliSense can provide assistance while developing.</span></span> <span data-ttu-id="e2b1f-204">При работе с кодом JavaScript, IntelliSense выводит списки объектов, функций, свойств и параметров, доступных на основании текущего контекста.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-204">As you work with JavaScript, IntelliSense lists the objects, functions, properties, and parameters that are available based on your current context.</span></span> <span data-ttu-id="e2b1f-205">Вариант кода можно выберите из всплывающего списка, формируемого IntelliSense для завершения кода.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-205">Select a coding option from the pop-up list provided by IntelliSense to complete the code.</span></span>

![gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

<span data-ttu-id="e2b1f-207">Дополнительные сведения о технологии IntelliSense см. в разделе [IntelliSense для JavaScript](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense).</span><span class="sxs-lookup"><span data-stu-id="e2b1f-207">For more information about IntelliSense, see [JavaScript IntelliSense](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense).</span></span>

## <a name="development-staging-and-production-environments"></a><span data-ttu-id="e2b1f-208">Разработки, промежуточной и производственной сред</span><span class="sxs-lookup"><span data-stu-id="e2b1f-208">Development, staging, and production environments</span></span>

<span data-ttu-id="e2b1f-209">Использование Gulp для оптимизации клиентские файлы для промежуточных и производственных обработанных файлов сохраняются в локальной папке промежуточной и производственной сред.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-209">When Gulp is used to optimize client-side files for staging and production, the processed files are saved to a local staging and production location.</span></span> <span data-ttu-id="e2b1f-210">*_Layout.cshtml* файле используется **среды** тег вспомогательный метод для предоставления двух разных версий файлов CSS.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-210">The *_Layout.cshtml* file uses the **environment** tag helper to provide two different versions of CSS files.</span></span> <span data-ttu-id="e2b1f-211">Является одной версии файлов CSS для разработки и другую версию оптимизирован для промежуточной и производственной сред.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-211">One version of CSS files is for development and the other version is optimized for both staging and production.</span></span> <span data-ttu-id="e2b1f-212">В Visual Studio 2017 г., при изменении **ASPNETCORE_ENVIRONMENT** переменную среды, чтобы `Production`, Visual Studio построит веб-приложения и ссылки в свернутом виде CSS-файлах.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-212">In Visual Studio 2017, when you change the **ASPNETCORE_ENVIRONMENT** environment variable to `Production`, Visual Studio will build the Web app and link to the minimized CSS files.</span></span> <span data-ttu-id="e2b1f-213">В следующем разметки показан **среды** содержащий теги ссылок для вспомогательных функций тегов `Development` CSS файлов и уменьшенное `Staging, Production` CSS-файлах.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-213">The following markup shows the **environment** tag helpers containing link tags to the `Development` CSS files and the minified `Staging, Production` CSS files.</span></span>

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

## <a name="switching-between-environments"></a><span data-ttu-id="e2b1f-214">Переключение между средами</span><span class="sxs-lookup"><span data-stu-id="e2b1f-214">Switching between environments</span></span>

<span data-ttu-id="e2b1f-215">Чтобы переключиться между компиляции для разных сред, измените **ASPNETCORE_ENVIRONMENT** значение переменной среды.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-215">To switch between compiling for different environments, modify the **ASPNETCORE_ENVIRONMENT** environment variable's value.</span></span>

1.  <span data-ttu-id="e2b1f-216">В **Task Runner Explorer**, убедитесь, что **min** задача настроена для запуска **перед построить**.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-216">In **Task Runner Explorer**, verify that the **min** task has been set to run **Before Build**.</span></span>

2.  <span data-ttu-id="e2b1f-217">В **обозревателе решений**, щелкните правой кнопкой мыши имя проекта и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-217">In **Solution Explorer**, right-click the project name and select **Properties**.</span></span>

    <span data-ttu-id="e2b1f-218">Откроется окно свойств веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-218">The property sheet for the Web app is displayed.</span></span>

3.  <span data-ttu-id="e2b1f-219">Откройте вкладку **Отладка**.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-219">Click the **Debug** tab.</span></span>

4.  <span data-ttu-id="e2b1f-220">Установите для параметра **: среда внешнего размещения** переменную среды, чтобы `Production`.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-220">Set the value of the **Hosting:Environment** environment variable to `Production`.</span></span>

5.  <span data-ttu-id="e2b1f-221">Нажмите клавишу **F5** для запуска приложения в браузере.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-221">Press **F5** to run the application in a browser.</span></span>

6.  <span data-ttu-id="e2b1f-222">В окне браузера щелкните страницу правой кнопкой мыши и выберите **источник** Просмотр HTML для страницы.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-222">In the browser window, right-click the page and select **View Source** to view the HTML for the page.</span></span>

    <span data-ttu-id="e2b1f-223">Обратите внимание, что таблица стилей ссылки указывают на уменьшенный CSS-файл.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-223">Notice that the stylesheet links point to the minified CSS files.</span></span>

7.  <span data-ttu-id="e2b1f-224">Закройте браузер, чтобы остановить веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-224">Close the browser to stop the Web app.</span></span>

8.  <span data-ttu-id="e2b1f-225">В Visual Studio, вернуться к странице свойств веб-приложения и изменить **: среда внешнего размещения** обратно переменной среды `Development`.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-225">In Visual Studio, return to the property sheet for the Web app and change the **Hosting:Environment** environment variable back to `Development`.</span></span>

9.  <span data-ttu-id="e2b1f-226">Нажмите клавишу **F5** Чтобы снова запустить приложение в браузере.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-226">Press **F5** to run the application in a browser again.</span></span>

10. <span data-ttu-id="e2b1f-227">В окне браузера щелкните страницу правой кнопкой мыши и выберите **источник** в HTML-код для этой страницы.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-227">In the browser window, right-click the page and select **View Source** to see the HTML for the page.</span></span>

    <span data-ttu-id="e2b1f-228">Обратите внимание, что таблица стилей ссылки указывают на unminified версии файлов CSS.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-228">Notice that the stylesheet links point to the unminified versions of the CSS files.</span></span>

<span data-ttu-id="e2b1f-229">Дополнительные сведения, относящиеся к средам в ASP.NET Core см. в разделе [работа с несколькими средами](../fundamentals/environments.md).</span><span class="sxs-lookup"><span data-stu-id="e2b1f-229">For more information related to environments in ASP.NET Core, see [Working with Multiple Environments](../fundamentals/environments.md).</span></span>

## <a name="task-and-module-details"></a><span data-ttu-id="e2b1f-230">Сведения о задаче и модуль</span><span class="sxs-lookup"><span data-stu-id="e2b1f-230">Task and module details</span></span>

<span data-ttu-id="e2b1f-231">Задание Gulp зарегистрировано с именем функции.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-231">A Gulp task is registered with a function name.</span></span> <span data-ttu-id="e2b1f-232">Если других задач должна быть запущена перед текущей задачи, можно указать зависимости.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-232">You can specify dependencies if other tasks must run before the current task.</span></span> <span data-ttu-id="e2b1f-233">Дополнительные функции позволяют выполнить и посмотреть Gulp задачи, а также в качестве источника задайте (*src*) и назначения (*dest*) выполняется изменение файлов.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-233">Additional functions allow you to run and watch the Gulp tasks, as well as set the source (*src*) and destination (*dest*) of the files being modified.</span></span> <span data-ttu-id="e2b1f-234">Ниже перечислены основные функции Gulp API.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-234">The following are the primary Gulp API functions:</span></span>

|<span data-ttu-id="e2b1f-235">Функция gulp</span><span class="sxs-lookup"><span data-stu-id="e2b1f-235">Gulp Function</span></span>|<span data-ttu-id="e2b1f-236">Синтаксис</span><span class="sxs-lookup"><span data-stu-id="e2b1f-236">Syntax</span></span>|<span data-ttu-id="e2b1f-237">Описание:</span><span class="sxs-lookup"><span data-stu-id="e2b1f-237">Description</span></span>|
|---   |--- |--- |
|<span data-ttu-id="e2b1f-238">задача</span><span class="sxs-lookup"><span data-stu-id="e2b1f-238">task</span></span>  |`gulp.task(name[, deps], fn) { }`|<span data-ttu-id="e2b1f-239">`task` Функция создает задачу.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-239">The `task` function creates a task.</span></span> <span data-ttu-id="e2b1f-240">`name` Параметр определяет имя задачи.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-240">The `name` parameter defines the name of the task.</span></span> <span data-ttu-id="e2b1f-241">`deps` Параметр содержит массив задачи должны быть выполнены до выполнения этой задачи.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-241">The `deps` parameter contains an array of tasks to be completed before this task runs.</span></span> <span data-ttu-id="e2b1f-242">`fn` Представляет параметр функции обратного вызова, которая выполняет операции задачи.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-242">The `fn` parameter represents a callback function which performs the operations of the task.</span></span>|
|<span data-ttu-id="e2b1f-243">Контрольное значение</span><span class="sxs-lookup"><span data-stu-id="e2b1f-243">watch</span></span> |`gulp.watch(glob [, opts], tasks) { }`|<span data-ttu-id="e2b1f-244">`watch` Функция отслеживает файлы и выполняет задачи, когда происходит изменение файла.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-244">The `watch` function monitors files and runs tasks when a file change occurs.</span></span> <span data-ttu-id="e2b1f-245">`glob` Параметр `string` или `array` , определяющий, какие файлы для наблюдения.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-245">The `glob` parameter is a `string` or `array` that determines which files to watch.</span></span> <span data-ttu-id="e2b1f-246">`opts` Параметр предоставляет дополнительный файл просмотра параметры.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-246">The `opts` parameter provides additional file watching options.</span></span>|
|<span data-ttu-id="e2b1f-247">src</span><span class="sxs-lookup"><span data-stu-id="e2b1f-247">src</span></span>   |`gulp.src(globs[, options]) { }`|<span data-ttu-id="e2b1f-248">`src` Функция предоставляет файлы, которые совпадают со значениями glob.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-248">The `src` function provides files that match the glob value(s).</span></span> <span data-ttu-id="e2b1f-249">`glob` Параметр `string` или `array` , определяющий, какие файлы для чтения.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-249">The `glob` parameter is a `string` or `array` that determines which files to read.</span></span> <span data-ttu-id="e2b1f-250">`options` Параметр предоставляет дополнительные параметры файла.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-250">The `options` parameter provides additional file options.</span></span>|
|<span data-ttu-id="e2b1f-251">dest</span><span class="sxs-lookup"><span data-stu-id="e2b1f-251">dest</span></span>  |`gulp.dest(path[, options]) { }`|<span data-ttu-id="e2b1f-252">`dest` Функция определяет расположение, к которому файлы могут быть записаны.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-252">The `dest` function defines a location to which files can be written.</span></span> <span data-ttu-id="e2b1f-253">`path` Параметр — это строка или функция, которая определяет конечную папку.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-253">The `path` parameter is a string or function that determines the destination folder.</span></span> <span data-ttu-id="e2b1f-254">`options` Параметр представляет собой объект, указывающий параметры выходной папки.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-254">The `options` parameter is an object that specifies output folder options.</span></span>|

<span data-ttu-id="e2b1f-255">Дополнительные справочные сведения Gulp API см. в разделе [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span><span class="sxs-lookup"><span data-stu-id="e2b1f-255">For additional Gulp API reference information, see [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span></span>

## <a name="gulp-recipes"></a><span data-ttu-id="e2b1f-256">Gulp рецепты</span><span class="sxs-lookup"><span data-stu-id="e2b1f-256">Gulp recipes</span></span>

<span data-ttu-id="e2b1f-257">Gulp сообщества предоставляет Gulp [рецепты](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span><span class="sxs-lookup"><span data-stu-id="e2b1f-257">The Gulp community provides Gulp [recipes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span></span> <span data-ttu-id="e2b1f-258">Этими рецептами состоят из Gulp задачи для распространенных сценариев.</span><span class="sxs-lookup"><span data-stu-id="e2b1f-258">These recipes consist of Gulp tasks to address common scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e2b1f-259">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e2b1f-259">Additional resources</span></span>

* [<span data-ttu-id="e2b1f-260">Gulp документации</span><span class="sxs-lookup"><span data-stu-id="e2b1f-260">Gulp documentation</span></span>](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [<span data-ttu-id="e2b1f-261">Объединение и Минификация в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e2b1f-261">Bundling and minification in ASP.NET Core</span></span>](bundling-and-minification.md)
* [<span data-ttu-id="e2b1f-262">С помощью Grunt в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e2b1f-262">Using Grunt in ASP.NET Core</span></span>](using-grunt.md)
