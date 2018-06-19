---
title: Использование Gulp в ASP.NET Core
author: rick-anderson
description: Сведения об использовании Gulp в ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-gulp
ms.openlocfilehash: 13f30be7670983bd65f8402404b841039bdacb09
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/07/2018
ms.locfileid: "33849992"
---
# <a name="use-gulp-in-aspnet-core"></a><span data-ttu-id="822a2-103">Использование Gulp в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="822a2-103">Use Gulp in ASP.NET Core</span></span>

<span data-ttu-id="822a2-104">По [Reitan Эрик](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [рот Daniel](https://github.com/danroth27), и [Бойера Shayne](https://twitter.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="822a2-104">By [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Daniel Roth](https://github.com/danroth27), and [Shayne Boyer](https://twitter.com/spboyer)</span></span>

<span data-ttu-id="822a2-105">В обычной современных веб-приложения процесс построения может:</span><span class="sxs-lookup"><span data-stu-id="822a2-105">In a typical modern web app, the build process might:</span></span>

* <span data-ttu-id="822a2-106">Объединение и уменьшения файлы JavaScript и CSS.</span><span class="sxs-lookup"><span data-stu-id="822a2-106">Bundle and minify JavaScript and CSS files.</span></span>
* <span data-ttu-id="822a2-107">Запуск средств для вызова объединение и Минификация задачи перед каждой сборкой.</span><span class="sxs-lookup"><span data-stu-id="822a2-107">Run tools to call the bundling and minification tasks before each build.</span></span>
* <span data-ttu-id="822a2-108">Скомпилируйте МЕНЕЕ или SASS файлов CSS.</span><span class="sxs-lookup"><span data-stu-id="822a2-108">Compile LESS or SASS files to CSS.</span></span>
* <span data-ttu-id="822a2-109">Скомпилируйте файлы CoffeeScript и TypeScript в код JavaScript.</span><span class="sxs-lookup"><span data-stu-id="822a2-109">Compile CoffeeScript or TypeScript files to JavaScript.</span></span>

<span data-ttu-id="822a2-110">Объект *средство запуска задач* — это средство, автоматизирующее этих задач процедуры разработки и многое другое.</span><span class="sxs-lookup"><span data-stu-id="822a2-110">A *task runner* is a tool which automates these routine development tasks and more.</span></span> <span data-ttu-id="822a2-111">Visual Studio предоставляет встроенную поддержку для средства запуска два распространенных задач на основе JavaScript: [Gulp](https://gulpjs.com/) и [Grunt](using-grunt.md).</span><span class="sxs-lookup"><span data-stu-id="822a2-111">Visual Studio provides built-in support for two popular JavaScript-based task runners: [Gulp](https://gulpjs.com/) and [Grunt](using-grunt.md).</span></span>

## <a name="gulp"></a><span data-ttu-id="822a2-112">gulp</span><span class="sxs-lookup"><span data-stu-id="822a2-112">Gulp</span></span>

<span data-ttu-id="822a2-113">Gulp — это на базе JavaScript потоковой передачи построения набор средств для клиентского кода.</span><span class="sxs-lookup"><span data-stu-id="822a2-113">Gulp is a JavaScript-based streaming build toolkit for client-side code.</span></span> <span data-ttu-id="822a2-114">Обычно он используется для потоковой передачи клиентские файлы через ряд процессов при активации определенного события в среде построения.</span><span class="sxs-lookup"><span data-stu-id="822a2-114">It's commonly used to stream client-side files through a series of processes when a specific event is triggered in a build environment.</span></span> <span data-ttu-id="822a2-115">Например, Gulp можно использовать для автоматизации [объединение и Минификация](bundling-and-minification.md) или очистки среде разработки до новой сборки.</span><span class="sxs-lookup"><span data-stu-id="822a2-115">For instance, Gulp can be used to automate [bundling and minification](bundling-and-minification.md) or the cleansing of a development environment before a new build.</span></span>

<span data-ttu-id="822a2-116">Набор задач Gulp определяется в *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="822a2-116">A set of Gulp tasks is defined in *gulpfile.js*.</span></span> <span data-ttu-id="822a2-117">Следующие JavaScript включает Gulp модулей, а также задает пути к файлам, должно быть указано в дальнейшем будет обеспечена поддержка задачи:</span><span class="sxs-lookup"><span data-stu-id="822a2-117">The following JavaScript includes Gulp modules and specifies file paths to be referenced within the forthcoming tasks:</span></span>

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

<span data-ttu-id="822a2-118">Приведенный выше код указывает, какой узел модули являются обязательными.</span><span class="sxs-lookup"><span data-stu-id="822a2-118">The above code specifies which Node modules are required.</span></span> <span data-ttu-id="822a2-119">`require` Импорта функций каждого модуля, чтобы зависимые задачи можно использовать предоставляемые им возможности.</span><span class="sxs-lookup"><span data-stu-id="822a2-119">The `require` function imports each module so that the dependent tasks can utilize their features.</span></span> <span data-ttu-id="822a2-120">Каждый из импортированных модулей присваивается переменной.</span><span class="sxs-lookup"><span data-stu-id="822a2-120">Each of the imported modules is assigned to a variable.</span></span> <span data-ttu-id="822a2-121">Модули могут располагаться по имени или пути.</span><span class="sxs-lookup"><span data-stu-id="822a2-121">The modules can be located either by name or path.</span></span> <span data-ttu-id="822a2-122">В этом примере имя модули `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, и `gulp-uglify` получить по имени.</span><span class="sxs-lookup"><span data-stu-id="822a2-122">In this example, the modules named `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, and `gulp-uglify` are retrieved by name.</span></span> <span data-ttu-id="822a2-123">Кроме того ряд пути создаются, расположения файлов CSS и JavaScript можно использовать повторно и ссылки на задачи.</span><span class="sxs-lookup"><span data-stu-id="822a2-123">Additionally, a series of paths are created so that the locations of CSS and JavaScript files can be reused and referenced within the tasks.</span></span> <span data-ttu-id="822a2-124">В следующей таблице приведены описания модулей, включенных в *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="822a2-124">The following table provides descriptions of the modules included in *gulpfile.js*.</span></span>

| <span data-ttu-id="822a2-125">Имя модуля</span><span class="sxs-lookup"><span data-stu-id="822a2-125">Module Name</span></span> | <span data-ttu-id="822a2-126">Описание</span><span class="sxs-lookup"><span data-stu-id="822a2-126">Description</span></span> |
| ----------- | ----------- |
| <span data-ttu-id="822a2-127">gulp</span><span class="sxs-lookup"><span data-stu-id="822a2-127">gulp</span></span>        | <span data-ttu-id="822a2-128">Gulp потоковой передачи системы сборки.</span><span class="sxs-lookup"><span data-stu-id="822a2-128">The Gulp streaming build system.</span></span> <span data-ttu-id="822a2-129">Дополнительные сведения см. в разделе [gulp](https://www.npmjs.com/package/gulp).</span><span class="sxs-lookup"><span data-stu-id="822a2-129">For more information, see [gulp](https://www.npmjs.com/package/gulp).</span></span> |
| <span data-ttu-id="822a2-130">rimraf</span><span class="sxs-lookup"><span data-stu-id="822a2-130">rimraf</span></span>      | <span data-ttu-id="822a2-131">Модуль удаления узла.</span><span class="sxs-lookup"><span data-stu-id="822a2-131">A Node deletion module.</span></span> <span data-ttu-id="822a2-132">Дополнительные сведения см. в разделе [rimraf](https://www.npmjs.com/package/rimraf).</span><span class="sxs-lookup"><span data-stu-id="822a2-132">For more information, see [rimraf](https://www.npmjs.com/package/rimraf).</span></span> |
| <span data-ttu-id="822a2-133">gulp concat</span><span class="sxs-lookup"><span data-stu-id="822a2-133">gulp-concat</span></span> | <span data-ttu-id="822a2-134">Модуль, который объединяет файлы в зависимости от операционной системы символ перевода строки.</span><span class="sxs-lookup"><span data-stu-id="822a2-134">A module that concatenates files based on the operating system's newline character.</span></span> <span data-ttu-id="822a2-135">Дополнительные сведения см. в разделе [gulp concat](https://www.npmjs.com/package/gulp-concat).</span><span class="sxs-lookup"><span data-stu-id="822a2-135">For more information, see [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span></span> |
| <span data-ttu-id="822a2-136">gulp cssmin</span><span class="sxs-lookup"><span data-stu-id="822a2-136">gulp-cssmin</span></span> | <span data-ttu-id="822a2-137">Модуль, который уменьшает CSS-файлах.</span><span class="sxs-lookup"><span data-stu-id="822a2-137">A module that minifies CSS files.</span></span> <span data-ttu-id="822a2-138">Дополнительные сведения см. в разделе [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin).</span><span class="sxs-lookup"><span data-stu-id="822a2-138">For more information, see [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin).</span></span> |
| <span data-ttu-id="822a2-139">gulp uglify</span><span class="sxs-lookup"><span data-stu-id="822a2-139">gulp-uglify</span></span> | <span data-ttu-id="822a2-140">Модуль, который уменьшает *.js* файлов.</span><span class="sxs-lookup"><span data-stu-id="822a2-140">A module that minifies *.js* files.</span></span> <span data-ttu-id="822a2-141">Дополнительные сведения см. в разделе [gulp uglify](https://www.npmjs.com/package/gulp-uglify).</span><span class="sxs-lookup"><span data-stu-id="822a2-141">For more information, see [gulp-uglify](https://www.npmjs.com/package/gulp-uglify).</span></span> |

<span data-ttu-id="822a2-142">После импорта модулей необходимые задачи может быть указан.</span><span class="sxs-lookup"><span data-stu-id="822a2-142">Once the requisite modules are imported, the tasks can be specified.</span></span> <span data-ttu-id="822a2-143">Ниже приведены шесть задач зарегистрирован, представленный в следующем примере кода:</span><span class="sxs-lookup"><span data-stu-id="822a2-143">Here there are six tasks registered, represented by the following code:</span></span>

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

<span data-ttu-id="822a2-144">В следующей таблице приведены пояснения задачи, указанные в приведенном выше коде:</span><span class="sxs-lookup"><span data-stu-id="822a2-144">The following table provides an explanation of the tasks specified in the code above:</span></span>

|<span data-ttu-id="822a2-145">Имя задачи</span><span class="sxs-lookup"><span data-stu-id="822a2-145">Task Name</span></span>|<span data-ttu-id="822a2-146">Описание</span><span class="sxs-lookup"><span data-stu-id="822a2-146">Description</span></span>|
|--- |--- |
|<span data-ttu-id="822a2-147">Очистить: js</span><span class="sxs-lookup"><span data-stu-id="822a2-147">clean:js</span></span>|<span data-ttu-id="822a2-148">Задача, которая использует модуль удаления rimraf узел для удаления уменьшенная версия файла site.js.</span><span class="sxs-lookup"><span data-stu-id="822a2-148">A task that uses the rimraf Node deletion module to remove the minified version of the site.js file.</span></span>|
|<span data-ttu-id="822a2-149">Очистить: css</span><span class="sxs-lookup"><span data-stu-id="822a2-149">clean:css</span></span>|<span data-ttu-id="822a2-150">Задача, которая использует модуль удаления rimraf узел для удаления уменьшенная версия файла site.css.</span><span class="sxs-lookup"><span data-stu-id="822a2-150">A task that uses the rimraf Node deletion module to remove the minified version of the site.css file.</span></span>|
|<span data-ttu-id="822a2-151">Очистка</span><span class="sxs-lookup"><span data-stu-id="822a2-151">clean</span></span>|<span data-ttu-id="822a2-152">Задача, которая вызывает `clean:js` задач, за которым следует `clean:css` задачи.</span><span class="sxs-lookup"><span data-stu-id="822a2-152">A task that calls the `clean:js` task, followed by the `clean:css` task.</span></span>|
|<span data-ttu-id="822a2-153">MIN:js</span><span class="sxs-lookup"><span data-stu-id="822a2-153">min:js</span></span>|<span data-ttu-id="822a2-154">Задача, которая уменьшает и объединяет все JS-файлов в папке js.</span><span class="sxs-lookup"><span data-stu-id="822a2-154">A task that minifies and concatenates all .js files within the js folder.</span></span> <span data-ttu-id="822a2-155">. Min.js файлы исключаются.</span><span class="sxs-lookup"><span data-stu-id="822a2-155">The .min.js files are excluded.</span></span>|
|<span data-ttu-id="822a2-156">MIN:CSS</span><span class="sxs-lookup"><span data-stu-id="822a2-156">min:css</span></span>|<span data-ttu-id="822a2-157">Задача, которая уменьшает и объединение всех файлов CSS-файлах в папке css.</span><span class="sxs-lookup"><span data-stu-id="822a2-157">A task that minifies and concatenates all .css files within the css folder.</span></span> <span data-ttu-id="822a2-158">. Min.css файлы исключаются.</span><span class="sxs-lookup"><span data-stu-id="822a2-158">The .min.css files are excluded.</span></span>|
|<span data-ttu-id="822a2-159">min</span><span class="sxs-lookup"><span data-stu-id="822a2-159">min</span></span>|<span data-ttu-id="822a2-160">Задача, которая вызывает `min:js` задач, за которым следует `min:css` задачи.</span><span class="sxs-lookup"><span data-stu-id="822a2-160">A task that calls the `min:js` task, followed by the `min:css` task.</span></span>|

## <a name="running-default-tasks"></a><span data-ttu-id="822a2-161">Выполнение задач по умолчанию</span><span class="sxs-lookup"><span data-stu-id="822a2-161">Running default tasks</span></span>

<span data-ttu-id="822a2-162">Если вы еще не создали уже новое веб-приложение, создайте новый проект веб-приложения ASP.NET в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="822a2-162">If you haven't already created a new Web app, create a new ASP.NET Web Application project in Visual Studio.</span></span>

1.  <span data-ttu-id="822a2-163">Добавьте новый файл JavaScript в проект и назовите его *gulpfile.js*, затем скопируйте следующий код.</span><span class="sxs-lookup"><span data-stu-id="822a2-163">Add a new JavaScript file to your project and name it *gulpfile.js*, then copy the following code.</span></span>

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

2.  <span data-ttu-id="822a2-164">Откройте *package.json* файла (Добавление Если не существует) и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="822a2-164">Open the *package.json* file (add if not there) and add the following.</span></span>

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

3.  <span data-ttu-id="822a2-165">В **обозревателе решений**, щелкните правой кнопкой мыши *gulpfile.js*и выберите **Task Runner Explorer**.</span><span class="sxs-lookup"><span data-stu-id="822a2-165">In **Solution Explorer**, right-click *gulpfile.js*, and select **Task Runner Explorer**.</span></span>
    
    ![Откройте диспетчер выполнения задач с помощью обозревателя решений](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    <span data-ttu-id="822a2-167">**Задача Runner Explorer** представлен список задач Gulp.</span><span class="sxs-lookup"><span data-stu-id="822a2-167">**Task Runner Explorer** shows the list of Gulp tasks.</span></span> <span data-ttu-id="822a2-168">(Может потребоваться нажать кнопку **обновление** , расположенную слева от имени проекта.)</span><span class="sxs-lookup"><span data-stu-id="822a2-168">(You might have to click the **Refresh** button that appears to the left of the project name.)</span></span>
    
    ![Диспетчер выполнения задач](using-gulp/_static/03-TaskRunnerExplorer.png)
    
    > [!IMPORTANT]
    > <span data-ttu-id="822a2-170">**Task Runner Explorer** пункт контекстного меню отображается только в том случае, если *gulpfile.js* находится в корневом каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="822a2-170">The **Task Runner Explorer** context menu item appears only if *gulpfile.js* is in the root project directory.</span></span>

4.  <span data-ttu-id="822a2-171">Под разделом **задачи** в **Task Runner Explorer**, щелкните правой кнопкой мыши **чистой**и выберите **запуска** из всплывающего меню.</span><span class="sxs-lookup"><span data-stu-id="822a2-171">Underneath **Tasks** in **Task Runner Explorer**, right-click **clean**, and select **Run** from the pop-up menu.</span></span>

    ![Задачу "Очистить" Диспетчер выполнения задач](using-gulp/_static/04-TaskRunner-clean.png)

    <span data-ttu-id="822a2-173">**Задача Runner Explorer** создается новая вкладка с именем **чистой** и выполните задачу "Очистить", как оно определено в *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="822a2-173">**Task Runner Explorer** will create a new tab named **clean** and execute the clean task as it's defined in *gulpfile.js*.</span></span>

5.  <span data-ttu-id="822a2-174">Щелкните правой кнопкой мыши **чистой** задач, а затем выберите **привязки** > **перед построить**.</span><span class="sxs-lookup"><span data-stu-id="822a2-174">Right-click the **clean** task, then select **Bindings** > **Before Build**.</span></span>

    ![Привязка BeforeBuild Task Runner Explorer](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    <span data-ttu-id="822a2-176">**Перед построить** привязки настраивает задачу "Очистить" для автоматического запуска перед каждой сборке проекта.</span><span class="sxs-lookup"><span data-stu-id="822a2-176">The **Before Build** binding configures the clean task to run automatically before each build of the project.</span></span>

<span data-ttu-id="822a2-177">Привязки настраиваются с помощью **Task Runner Explorer** хранятся в виде комментариев в верхней части вашего *gulpfile.js* и вступают в силу только в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="822a2-177">The bindings you set up with **Task Runner Explorer** are stored in the form of a comment at the top of your *gulpfile.js* and are effective only in Visual Studio.</span></span> <span data-ttu-id="822a2-178">Альтернатива, которой не требуется Visual Studio — Настройка автоматического выполнения задач gulp в вашей *.csproj* файла.</span><span class="sxs-lookup"><span data-stu-id="822a2-178">An alternative that doesn't require Visual Studio is to configure automatic execution of gulp tasks in your *.csproj* file.</span></span> <span data-ttu-id="822a2-179">Например, поместите ее ваш *.csproj* файла:</span><span class="sxs-lookup"><span data-stu-id="822a2-179">For example, put this in your *.csproj* file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

<span data-ttu-id="822a2-180">Теперь задачу "Очистить" выполняется при запуске проекта в Visual Studio или из командной строки с помощью [dotnet запуска](/dotnet/core/tools/dotnet-run) команда (запустить `npm install` первой).</span><span class="sxs-lookup"><span data-stu-id="822a2-180">Now the clean task is executed when you run the project in Visual Studio or from a command prompt using the [dotnet run](/dotnet/core/tools/dotnet-run) command (run `npm install` first).</span></span>

## <a name="defining-and-running-a-new-task"></a><span data-ttu-id="822a2-181">Определение и выполнение нового задания</span><span class="sxs-lookup"><span data-stu-id="822a2-181">Defining and running a new task</span></span>

<span data-ttu-id="822a2-182">Чтобы определить новую задачу Gulp, измените *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="822a2-182">To define a new Gulp task, modify *gulpfile.js*.</span></span>

1.  <span data-ttu-id="822a2-183">Добавление следующего кода JavaScript в конец *gulpfile.js*:</span><span class="sxs-lookup"><span data-stu-id="822a2-183">Add the following JavaScript to the end of *gulpfile.js*:</span></span>

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    <span data-ttu-id="822a2-184">Эта задача называется `first`, и он просто отображает строку.</span><span class="sxs-lookup"><span data-stu-id="822a2-184">This task is named `first`, and it simply displays a string.</span></span>

2.  <span data-ttu-id="822a2-185">Сохранить *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="822a2-185">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="822a2-186">В **обозревателе решений**, щелкните правой кнопкой мыши *gulpfile.js*и выберите *Task Runner Explorer*.</span><span class="sxs-lookup"><span data-stu-id="822a2-186">In **Solution Explorer**, right-click *gulpfile.js*, and select *Task Runner Explorer*.</span></span>

4.  <span data-ttu-id="822a2-187">В **Task Runner Explorer**, щелкните правой кнопкой мыши **первый**и выберите **запуска**.</span><span class="sxs-lookup"><span data-stu-id="822a2-187">In **Task Runner Explorer**, right-click **first**, and select **Run**.</span></span>

    ![Task Runner Explorer выполнения первой задачи](using-gulp/_static/06-TaskRunner-First.png)

    <span data-ttu-id="822a2-189">Выходной текст отображается.</span><span class="sxs-lookup"><span data-stu-id="822a2-189">The output text is displayed.</span></span> <span data-ttu-id="822a2-190">Примеры, основываясь на общих сценариях см [Gulp рецепты](#gulp-recipes).</span><span class="sxs-lookup"><span data-stu-id="822a2-190">To see examples based on common scenarios, see [Gulp Recipes](#gulp-recipes).</span></span>

## <a name="defining-and-running-tasks-in-a-series"></a><span data-ttu-id="822a2-191">Определение и выполнение задач в ряд</span><span class="sxs-lookup"><span data-stu-id="822a2-191">Defining and running tasks in a series</span></span>

<span data-ttu-id="822a2-192">При выполнении нескольких задач одновременно выполняются задачи по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="822a2-192">When you run multiple tasks, the tasks run concurrently by default.</span></span> <span data-ttu-id="822a2-193">Тем не менее, если необходимо выполнить задачи в определенном порядке, необходимо указать каждую задачу после завершения операции, а также как какие задачи зависят от завершения другой задачи.</span><span class="sxs-lookup"><span data-stu-id="822a2-193">However, if you need to run tasks in a specific order, you must specify when each task is complete, as well as which tasks depend on the completion of another task.</span></span>

1.  <span data-ttu-id="822a2-194">Чтобы определить последовательность задач для выполнения в порядке, замените `first` задачи, добавленные в *gulpfile.js* со следующим:</span><span class="sxs-lookup"><span data-stu-id="822a2-194">To define a series of tasks to run in order, replace the `first` task that you added above in *gulpfile.js* with the following:</span></span>

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    <span data-ttu-id="822a2-195">Теперь у вас есть три задачи: `series:first`, `series:second`, и `series`.</span><span class="sxs-lookup"><span data-stu-id="822a2-195">You now have three tasks: `series:first`, `series:second`, and `series`.</span></span> <span data-ttu-id="822a2-196">`series:second` Задача включает второй параметр, указывающий массив задачи должен быть запущен и завершена до `series:second` задача будет выполняться.</span><span class="sxs-lookup"><span data-stu-id="822a2-196">The `series:second` task includes a second parameter which specifies an array of tasks to be run and completed before the `series:second` task will run.</span></span> <span data-ttu-id="822a2-197">Как указано в коде выше, только `series:first` задача должна быть выполнена перед `series:second` задача будет выполняться.</span><span class="sxs-lookup"><span data-stu-id="822a2-197">As specified in the code above, only the `series:first` task must be completed before the `series:second` task will run.</span></span>

2.  <span data-ttu-id="822a2-198">Сохранить *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="822a2-198">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="822a2-199">В **обозревателе решений**, щелкните правой кнопкой мыши *gulpfile.js* и выберите **Task Runner Explorer** , если он еще не открыт.</span><span class="sxs-lookup"><span data-stu-id="822a2-199">In **Solution Explorer**, right-click *gulpfile.js* and select **Task Runner Explorer** if it isn't already open.</span></span>

4.  <span data-ttu-id="822a2-200">В **Task Runner Explorer**, щелкните правой кнопкой мыши **серии** и выберите **запуска**.</span><span class="sxs-lookup"><span data-stu-id="822a2-200">In **Task Runner Explorer**, right-click **series** and select **Run**.</span></span>

    ![Task Runner Explorer выполнения ряда задач](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a><span data-ttu-id="822a2-202">IntelliSense</span><span class="sxs-lookup"><span data-stu-id="822a2-202">IntelliSense</span></span>

<span data-ttu-id="822a2-203">IntelliSense предоставляет дополнение кода, описания параметров и другие возможности для повышения производительности и сокращения ошибок.</span><span class="sxs-lookup"><span data-stu-id="822a2-203">IntelliSense provides code completion, parameter descriptions, and other features to boost productivity and to decrease errors.</span></span> <span data-ttu-id="822a2-204">Gulp задачи создаются на языке JavaScript; Таким образом технология IntelliSense может оказывать помощь при разработке.</span><span class="sxs-lookup"><span data-stu-id="822a2-204">Gulp tasks are written in JavaScript; therefore, IntelliSense can provide assistance while developing.</span></span> <span data-ttu-id="822a2-205">При работе с кодом JavaScript, IntelliSense выводит списки объектов, функций, свойств и параметров, доступных на основании текущего контекста.</span><span class="sxs-lookup"><span data-stu-id="822a2-205">As you work with JavaScript, IntelliSense lists the objects, functions, properties, and parameters that are available based on your current context.</span></span> <span data-ttu-id="822a2-206">Вариант кода можно выберите из всплывающего списка, формируемого IntelliSense для завершения кода.</span><span class="sxs-lookup"><span data-stu-id="822a2-206">Select a coding option from the pop-up list provided by IntelliSense to complete the code.</span></span>

![gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

<span data-ttu-id="822a2-208">Дополнительные сведения о технологии IntelliSense см. в разделе [IntelliSense для JavaScript](/visualstudio/ide/javascript-intellisense).</span><span class="sxs-lookup"><span data-stu-id="822a2-208">For more information about IntelliSense, see [JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense).</span></span>

## <a name="development-staging-and-production-environments"></a><span data-ttu-id="822a2-209">Разработки, промежуточной и производственной сред</span><span class="sxs-lookup"><span data-stu-id="822a2-209">Development, staging, and production environments</span></span>

<span data-ttu-id="822a2-210">Использование Gulp для оптимизации клиентские файлы для промежуточных и производственных обработанных файлов сохраняются в локальной папке промежуточной и производственной сред.</span><span class="sxs-lookup"><span data-stu-id="822a2-210">When Gulp is used to optimize client-side files for staging and production, the processed files are saved to a local staging and production location.</span></span> <span data-ttu-id="822a2-211">*_Layout.cshtml* файле используется **среды** тег вспомогательный метод для предоставления двух разных версий файлов CSS.</span><span class="sxs-lookup"><span data-stu-id="822a2-211">The *_Layout.cshtml* file uses the **environment** tag helper to provide two different versions of CSS files.</span></span> <span data-ttu-id="822a2-212">Является одной версии файлов CSS для разработки и другую версию оптимизирован для промежуточной и производственной сред.</span><span class="sxs-lookup"><span data-stu-id="822a2-212">One version of CSS files is for development and the other version is optimized for both staging and production.</span></span> <span data-ttu-id="822a2-213">В Visual Studio 2017 г., при изменении **ASPNETCORE_ENVIRONMENT** переменную среды, чтобы `Production`, Visual Studio построит веб-приложения и ссылки в свернутом виде CSS-файлах.</span><span class="sxs-lookup"><span data-stu-id="822a2-213">In Visual Studio 2017, when you change the **ASPNETCORE_ENVIRONMENT** environment variable to `Production`, Visual Studio will build the Web app and link to the minimized CSS files.</span></span> <span data-ttu-id="822a2-214">В следующем разметки показан **среды** содержащий теги ссылок для вспомогательных функций тегов `Development` CSS файлов и уменьшенное `Staging, Production` CSS-файлах.</span><span class="sxs-lookup"><span data-stu-id="822a2-214">The following markup shows the **environment** tag helpers containing link tags to the `Development` CSS files and the minified `Staging, Production` CSS files.</span></span>

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

## <a name="switching-between-environments"></a><span data-ttu-id="822a2-215">Переключение между средами</span><span class="sxs-lookup"><span data-stu-id="822a2-215">Switching between environments</span></span>

<span data-ttu-id="822a2-216">Чтобы переключиться между компиляции для разных сред, измените **ASPNETCORE_ENVIRONMENT** значение переменной среды.</span><span class="sxs-lookup"><span data-stu-id="822a2-216">To switch between compiling for different environments, modify the **ASPNETCORE_ENVIRONMENT** environment variable's value.</span></span>

1.  <span data-ttu-id="822a2-217">В **Task Runner Explorer**, убедитесь, что **min** задача настроена для запуска **перед построить**.</span><span class="sxs-lookup"><span data-stu-id="822a2-217">In **Task Runner Explorer**, verify that the **min** task has been set to run **Before Build**.</span></span>

2.  <span data-ttu-id="822a2-218">В **обозревателе решений**, щелкните правой кнопкой мыши имя проекта и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="822a2-218">In **Solution Explorer**, right-click the project name and select **Properties**.</span></span>

    <span data-ttu-id="822a2-219">Откроется окно свойств веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="822a2-219">The property sheet for the Web app is displayed.</span></span>

3.  <span data-ttu-id="822a2-220">Откройте вкладку **Отладка**.</span><span class="sxs-lookup"><span data-stu-id="822a2-220">Click the **Debug** tab.</span></span>

4.  <span data-ttu-id="822a2-221">Установите для параметра **: среда внешнего размещения** переменную среды, чтобы `Production`.</span><span class="sxs-lookup"><span data-stu-id="822a2-221">Set the value of the **Hosting:Environment** environment variable to `Production`.</span></span>

5.  <span data-ttu-id="822a2-222">Нажмите клавишу **F5** для запуска приложения в браузере.</span><span class="sxs-lookup"><span data-stu-id="822a2-222">Press **F5** to run the application in a browser.</span></span>

6.  <span data-ttu-id="822a2-223">В окне браузера щелкните страницу правой кнопкой мыши и выберите **источник** Просмотр HTML для страницы.</span><span class="sxs-lookup"><span data-stu-id="822a2-223">In the browser window, right-click the page and select **View Source** to view the HTML for the page.</span></span>

    <span data-ttu-id="822a2-224">Обратите внимание, что таблица стилей ссылки указывают на уменьшенный CSS-файл.</span><span class="sxs-lookup"><span data-stu-id="822a2-224">Notice that the stylesheet links point to the minified CSS files.</span></span>

7.  <span data-ttu-id="822a2-225">Закройте браузер, чтобы остановить веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="822a2-225">Close the browser to stop the Web app.</span></span>

8.  <span data-ttu-id="822a2-226">В Visual Studio, вернуться к странице свойств веб-приложения и изменить **: среда внешнего размещения** обратно переменной среды `Development`.</span><span class="sxs-lookup"><span data-stu-id="822a2-226">In Visual Studio, return to the property sheet for the Web app and change the **Hosting:Environment** environment variable back to `Development`.</span></span>

9.  <span data-ttu-id="822a2-227">Нажмите клавишу **F5** Чтобы снова запустить приложение в браузере.</span><span class="sxs-lookup"><span data-stu-id="822a2-227">Press **F5** to run the application in a browser again.</span></span>

10. <span data-ttu-id="822a2-228">В окне браузера щелкните страницу правой кнопкой мыши и выберите **источник** в HTML-код для этой страницы.</span><span class="sxs-lookup"><span data-stu-id="822a2-228">In the browser window, right-click the page and select **View Source** to see the HTML for the page.</span></span>

    <span data-ttu-id="822a2-229">Обратите внимание, что таблица стилей ссылки указывают на unminified версии файлов CSS.</span><span class="sxs-lookup"><span data-stu-id="822a2-229">Notice that the stylesheet links point to the unminified versions of the CSS files.</span></span>

<span data-ttu-id="822a2-230">Дополнительные сведения, относящиеся к средам в ASP.NET Core см. в разделе [использовать несколько сред](../fundamentals/environments.md).</span><span class="sxs-lookup"><span data-stu-id="822a2-230">For more information related to environments in ASP.NET Core, see [Use multiple environments](../fundamentals/environments.md).</span></span>

## <a name="task-and-module-details"></a><span data-ttu-id="822a2-231">Сведения о задаче и модуль</span><span class="sxs-lookup"><span data-stu-id="822a2-231">Task and module details</span></span>

<span data-ttu-id="822a2-232">Задание Gulp зарегистрировано с именем функции.</span><span class="sxs-lookup"><span data-stu-id="822a2-232">A Gulp task is registered with a function name.</span></span> <span data-ttu-id="822a2-233">Если других задач должна быть запущена перед текущей задачи, можно указать зависимости.</span><span class="sxs-lookup"><span data-stu-id="822a2-233">You can specify dependencies if other tasks must run before the current task.</span></span> <span data-ttu-id="822a2-234">Дополнительные функции позволяют выполнить и посмотреть Gulp задачи, а также в качестве источника задайте (*src*) и назначения (*dest*) выполняется изменение файлов.</span><span class="sxs-lookup"><span data-stu-id="822a2-234">Additional functions allow you to run and watch the Gulp tasks, as well as set the source (*src*) and destination (*dest*) of the files being modified.</span></span> <span data-ttu-id="822a2-235">Ниже перечислены основные функции Gulp API.</span><span class="sxs-lookup"><span data-stu-id="822a2-235">The following are the primary Gulp API functions:</span></span>

|<span data-ttu-id="822a2-236">Функция gulp</span><span class="sxs-lookup"><span data-stu-id="822a2-236">Gulp Function</span></span>|<span data-ttu-id="822a2-237">Синтаксис</span><span class="sxs-lookup"><span data-stu-id="822a2-237">Syntax</span></span>|<span data-ttu-id="822a2-238">Описание</span><span class="sxs-lookup"><span data-stu-id="822a2-238">Description</span></span>|
|---   |--- |--- |
|<span data-ttu-id="822a2-239">задача</span><span class="sxs-lookup"><span data-stu-id="822a2-239">task</span></span>  |`gulp.task(name[, deps], fn) { }`|<span data-ttu-id="822a2-240">`task` Функция создает задачу.</span><span class="sxs-lookup"><span data-stu-id="822a2-240">The `task` function creates a task.</span></span> <span data-ttu-id="822a2-241">`name` Параметр определяет имя задачи.</span><span class="sxs-lookup"><span data-stu-id="822a2-241">The `name` parameter defines the name of the task.</span></span> <span data-ttu-id="822a2-242">`deps` Параметр содержит массив задачи должны быть выполнены до выполнения этой задачи.</span><span class="sxs-lookup"><span data-stu-id="822a2-242">The `deps` parameter contains an array of tasks to be completed before this task runs.</span></span> <span data-ttu-id="822a2-243">`fn` Представляет параметр функции обратного вызова, которая выполняет операции задачи.</span><span class="sxs-lookup"><span data-stu-id="822a2-243">The `fn` parameter represents a callback function which performs the operations of the task.</span></span>|
|<span data-ttu-id="822a2-244">Контрольное значение</span><span class="sxs-lookup"><span data-stu-id="822a2-244">watch</span></span> |`gulp.watch(glob [, opts], tasks) { }`|<span data-ttu-id="822a2-245">`watch` Функция отслеживает файлы и выполняет задачи, когда происходит изменение файла.</span><span class="sxs-lookup"><span data-stu-id="822a2-245">The `watch` function monitors files and runs tasks when a file change occurs.</span></span> <span data-ttu-id="822a2-246">`glob` Параметр `string` или `array` , определяющий, какие файлы для наблюдения.</span><span class="sxs-lookup"><span data-stu-id="822a2-246">The `glob` parameter is a `string` or `array` that determines which files to watch.</span></span> <span data-ttu-id="822a2-247">`opts` Параметр предоставляет дополнительный файл просмотра параметры.</span><span class="sxs-lookup"><span data-stu-id="822a2-247">The `opts` parameter provides additional file watching options.</span></span>|
|<span data-ttu-id="822a2-248">src</span><span class="sxs-lookup"><span data-stu-id="822a2-248">src</span></span>   |`gulp.src(globs[, options]) { }`|<span data-ttu-id="822a2-249">`src` Функция предоставляет файлы, которые совпадают со значениями glob.</span><span class="sxs-lookup"><span data-stu-id="822a2-249">The `src` function provides files that match the glob value(s).</span></span> <span data-ttu-id="822a2-250">`glob` Параметр `string` или `array` , определяющий, какие файлы для чтения.</span><span class="sxs-lookup"><span data-stu-id="822a2-250">The `glob` parameter is a `string` or `array` that determines which files to read.</span></span> <span data-ttu-id="822a2-251">`options` Параметр предоставляет дополнительные параметры файла.</span><span class="sxs-lookup"><span data-stu-id="822a2-251">The `options` parameter provides additional file options.</span></span>|
|<span data-ttu-id="822a2-252">dest</span><span class="sxs-lookup"><span data-stu-id="822a2-252">dest</span></span>  |`gulp.dest(path[, options]) { }`|<span data-ttu-id="822a2-253">`dest` Функция определяет расположение, к которому файлы могут быть записаны.</span><span class="sxs-lookup"><span data-stu-id="822a2-253">The `dest` function defines a location to which files can be written.</span></span> <span data-ttu-id="822a2-254">`path` Параметр — это строка или функция, которая определяет конечную папку.</span><span class="sxs-lookup"><span data-stu-id="822a2-254">The `path` parameter is a string or function that determines the destination folder.</span></span> <span data-ttu-id="822a2-255">`options` Параметр представляет собой объект, указывающий параметры выходной папки.</span><span class="sxs-lookup"><span data-stu-id="822a2-255">The `options` parameter is an object that specifies output folder options.</span></span>|

<span data-ttu-id="822a2-256">Дополнительные справочные сведения Gulp API см. в разделе [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span><span class="sxs-lookup"><span data-stu-id="822a2-256">For additional Gulp API reference information, see [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span></span>

## <a name="gulp-recipes"></a><span data-ttu-id="822a2-257">Gulp рецепты</span><span class="sxs-lookup"><span data-stu-id="822a2-257">Gulp recipes</span></span>

<span data-ttu-id="822a2-258">Gulp сообщества предоставляет Gulp [рецепты](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span><span class="sxs-lookup"><span data-stu-id="822a2-258">The Gulp community provides Gulp [Recipes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span></span> <span data-ttu-id="822a2-259">Этими рецептами состоят из Gulp задачи для распространенных сценариев.</span><span class="sxs-lookup"><span data-stu-id="822a2-259">These recipes consist of Gulp tasks to address common scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="822a2-260">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="822a2-260">Additional resources</span></span>

* [<span data-ttu-id="822a2-261">Gulp документации</span><span class="sxs-lookup"><span data-stu-id="822a2-261">Gulp documentation</span></span>](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [<span data-ttu-id="822a2-262">Объединение и Минификация в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="822a2-262">Bundling and minification in ASP.NET Core</span></span>](bundling-and-minification.md)
* [<span data-ttu-id="822a2-263">Использовать Grunt в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="822a2-263">Use Grunt in ASP.NET Core</span></span>](using-grunt.md)
