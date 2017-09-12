---
title: "Меньше Sass и шрифтов, здорово в ASP.NET Core"
author: ardalis
description: "Сведения об использовании меньше Sass и здорово шрифтов в приложениях ASP.NET Core."
keywords: "ASP.NET Core меньше Sass, здорово шрифта, препроцессорами"
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 94c988f9-95fd-425d-b37e-7f846598c6d4
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/less-sass-fa
ms.openlocfilehash: 128612eb2f7c6c8fdd0cc01f10b8e522df46dcf6
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-styling-applications-with-less-sass-and-font-awesome-in-aspnet-core"></a><span data-ttu-id="0c274-104">Общие сведения о стилях приложений с меньшим, Sass и здорово шрифта в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c274-104">Introduction to styling applications with Less, Sass, and Font Awesome in ASP.NET Core</span></span>

<span data-ttu-id="0c274-105">По [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="0c274-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="0c274-106">Пользователи веб-приложений имеются все более высокого уровня ожидания, когда дело доходит до стиля и опыта в целом.</span><span class="sxs-lookup"><span data-stu-id="0c274-106">Users of web applications have increasingly high expectations when it comes to style and overall experience.</span></span> <span data-ttu-id="0c274-107">Современных веб-приложений часто используют форматированного инструменты и платформы для определения и управления их внешний вид согласованным образом.</span><span class="sxs-lookup"><span data-stu-id="0c274-107">Modern web applications frequently leverage rich tools and frameworks for defining and managing their look and feel in a consistent manner.</span></span> <span data-ttu-id="0c274-108">Платформы, например [начальной загрузки](http://getbootstrap.com/) могут сыграть большую относительно определения общего набора стили и параметры макета для веб-сайтов.</span><span class="sxs-lookup"><span data-stu-id="0c274-108">Frameworks like [Bootstrap](http://getbootstrap.com/) can go a long way toward defining a common set of styles and layout options for web sites.</span></span> <span data-ttu-id="0c274-109">Однако большинство нетривиальной сайтов преимущества также возможность эффективно определять и поддерживать стили и файлы каскадной стилей-стилей (CSS), а также простой доступ к не изображения значков, которые помогают сделать более интуитивно понятный интерфейс веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="0c274-109">However, most non-trivial sites also benefit from being able to effectively define and maintain styles and cascading style sheet (CSS) files, as well as having easy access to non-image icons that help make the site's interface more intuitive.</span></span> <span data-ttu-id="0c274-110">Именно так языки и средства, которые поддерживают [меньше](http://lesscss.org/) и [Sass](http://sass-lang.com/), и библиотеки, например [шрифта здорово](http://fontawesome.io/), бывают.</span><span class="sxs-lookup"><span data-stu-id="0c274-110">That's where languages and tools that support [Less](http://lesscss.org/) and [Sass](http://sass-lang.com/), and libraries like [Font Awesome](http://fontawesome.io/), come in.</span></span>

## <a name="css-preprocessor-languages"></a><span data-ttu-id="0c274-111">Языки препроцессора CSS</span><span class="sxs-lookup"><span data-stu-id="0c274-111">CSS preprocessor languages</span></span>

<span data-ttu-id="0c274-112">Языки, которые компилируются в других языках, для повышения удобства работы с базовый язык, называются препроцессорами.</span><span class="sxs-lookup"><span data-stu-id="0c274-112">Languages that are compiled into other languages, in order to improve the experience of working with the underlying language, are referred to as preprocessors.</span></span> <span data-ttu-id="0c274-113">Существует два популярных препроцессорами для CSS: менее и Sass.</span><span class="sxs-lookup"><span data-stu-id="0c274-113">There are two popular preprocessors for CSS: Less and Sass.</span></span>  <span data-ttu-id="0c274-114">Эти препроцессорами добавить компоненты в CSS, таких как поддержка переменных и вложенных правила, что повышает удобство поддержки больших и сложных таблиц стилей.</span><span class="sxs-lookup"><span data-stu-id="0c274-114">These preprocessors add features to CSS, such as support for variables and nested rules, which improve the maintainability of large, complex stylesheets.</span></span> <span data-ttu-id="0c274-115">CSS, как язык является очень простым, отсутствует поддержка даже такой простой как переменные, а это, как правило, чтобы повторяющихся и перегруженными CSS-файлах.</span><span class="sxs-lookup"><span data-stu-id="0c274-115">CSS as a language is very basic, lacking support even for something as simple as variables, and this tends to make CSS files repetitive and bloated.</span></span> <span data-ttu-id="0c274-116">Добавление функции real программирования языка через препроцессорами может помочь сократить дублирование и обеспечить более эффективной организации правила стиля.</span><span class="sxs-lookup"><span data-stu-id="0c274-116">Adding real programming language features via preprocessors can help reduce duplication and provide better organization of styling rules.</span></span> <span data-ttu-id="0c274-117">Visual Studio предоставляет встроенную поддержку для обоих менее Sass, а также расширения, которые могут увеличить процесс разработки, при работе с помощью этих языков.</span><span class="sxs-lookup"><span data-stu-id="0c274-117">Visual Studio provides built-in support for both Less and Sass, as well as extensions that can further improve the development experience when working with these languages.</span></span>

<span data-ttu-id="0c274-118">Рассмотрим краткий пример как препроцессорами может повысить удобочитаемость и обслуживаемость информации о стилях CSS этот:</span><span class="sxs-lookup"><span data-stu-id="0c274-118">As a quick example of how preprocessors can improve readability and maintainability of style information, consider this CSS:</span></span>

```css
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    color: black;
    font-weight: bold;
    font-size: 14px;
    font-family: Helvetica, Arial, sans-serif;
}
```

<span data-ttu-id="0c274-119">С помощью меньше, это можно переписать, чтобы исключить все дубликаты, с помощью *mixin* (с разделителями «mix» позволяет свойства из одного класса или набор правил в другой):</span><span class="sxs-lookup"><span data-stu-id="0c274-119">Using Less, this can be rewritten to eliminate all of the duplication, using a *mixin* (so named because it allows you to "mix in" properties from one class or rule-set into another):</span></span>

```less
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    .header;
    font-size: 14px;
}
```

## <a name="less"></a><span data-ttu-id="0c274-120">Меньше</span><span class="sxs-lookup"><span data-stu-id="0c274-120">Less</span></span>

<span data-ttu-id="0c274-121">CSS менее препроцессора выполняется с помощью Node.js.</span><span class="sxs-lookup"><span data-stu-id="0c274-121">The Less CSS preprocessor runs using Node.js.</span></span> <span data-ttu-id="0c274-122">Чтобы установить меньше, используйте диспетчер пакетов Node (npm-файл) из командной строки (-g означает «глобальные»):</span><span class="sxs-lookup"><span data-stu-id="0c274-122">To install Less, use Node Package Manager (npm) from a command prompt (-g means "global"):</span></span>

```console
npm install -g less
```

<span data-ttu-id="0c274-123">Если вы используете Visual Studio, вы можете начать с меньшим путем добавления в проект одного или нескольких файлов, меньше и последующей настройки Gulp (или Grunt) для их обработки во время компиляции.</span><span class="sxs-lookup"><span data-stu-id="0c274-123">If you're using Visual Studio, you can get started with Less by adding one or more Less files to your project, and then configuring Gulp (or Grunt) to process them at compile-time.</span></span> <span data-ttu-id="0c274-124">Добавить *стили* папки в проект, а затем добавьте новый меньше файл с именем *main.less* в эту папку.</span><span class="sxs-lookup"><span data-stu-id="0c274-124">Add a *Styles* folder to your project, and then add a new Less file named *main.less* to this folder.</span></span>

![Добавить файл](less-sass-fa/_static/add-less-file.png)

<span data-ttu-id="0c274-126">После добавления структура папки должна выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0c274-126">Once added, your folder structure should look something like this:</span></span>

![Структура папок](less-sass-fa/_static/folder-structure.png)

<span data-ttu-id="0c274-128">Теперь можно добавить в файл, который компилируется в CSS и развертываются в папку wwwroot по Gulp некоторые основные стили.</span><span class="sxs-lookup"><span data-stu-id="0c274-128">Now you can add some basic styling to the file, which will be compiled into CSS and deployed to the wwwroot folder by Gulp.</span></span>

<span data-ttu-id="0c274-129">Изменить *main.less* для включения следующим содержимым, который создает простой цветовой палитры из одного основного цвета.</span><span class="sxs-lookup"><span data-stu-id="0c274-129">Modify *main.less* to include the following content, which creates a simple color palette from a single base color.</span></span>

```less
@base: #663333;
@background: spin(@base, 180);
@lighter: lighten(spin(@base, 5), 10%);
@lighter2: lighten(spin(@base, 10), 20%);
@darker: darken(spin(@base, -5), 10%);
@darker2: darken(spin(@base, -10), 20%);

body {
    background-color:@background;
}
.baseColor  {color:@base}
.bgLight    {color:@lighter}
.bgLight2   {color:@lighter2}
.bgDark     {color:@darker}
.bgDark2    {color:@darker2}
```

<span data-ttu-id="0c274-130">`@base`а другой @-prefixed элементы являются переменными.</span><span class="sxs-lookup"><span data-stu-id="0c274-130">`@base` and the other @-prefixed items are variables.</span></span> <span data-ttu-id="0c274-131">Каждый из них представляет цвет.</span><span class="sxs-lookup"><span data-stu-id="0c274-131">Each of them represents a color.</span></span> <span data-ttu-id="0c274-132">За исключением `@base`, они устанавливаются с использованием функции цвета: светлее затемнению и счетчик.</span><span class="sxs-lookup"><span data-stu-id="0c274-132">Except for `@base`, they are set using color functions: lighten, darken, and spin.</span></span> <span data-ttu-id="0c274-133">Упростить и затемнению выполните во многом ожидаемого; Счетчик изменяет оттенок цвета число градусов (относительно цветовой круг).</span><span class="sxs-lookup"><span data-stu-id="0c274-133">Lighten and darken do pretty much what you would expect; spin adjusts the hue of a color by a number of degrees (around the color wheel).</span></span> <span data-ttu-id="0c274-134">Меньше процессор может игнорировать переменные, которые не используются, чтобы продемонстрировать, как работают эти переменные, нам нужно использовать их в любом месте.</span><span class="sxs-lookup"><span data-stu-id="0c274-134">The Less processor is smart enough to ignore variables that aren't used, so to demonstrate how these variables work, we need to use them somewhere.</span></span> <span data-ttu-id="0c274-135">Классы `.baseColor`, т. д. продемонстрируют вычисляемые значения всех переменных в CSS-файл, который создается.</span><span class="sxs-lookup"><span data-stu-id="0c274-135">The classes `.baseColor`, etc. will demonstrate the calculated values of each of the variables in the CSS file that is produced.</span></span>

### <a name="getting-started"></a><span data-ttu-id="0c274-136">Начало работы</span><span class="sxs-lookup"><span data-stu-id="0c274-136">Getting started</span></span>

<span data-ttu-id="0c274-137">Создание **файл конфигурации npm** (*package.json*) в папку проекта и изменить его, чтобы ссылаться на `gulp` и `gulp-less`:</span><span class="sxs-lookup"><span data-stu-id="0c274-137">Create an **npm Configuration File** (*package.json*) in your project folder and edit it to reference `gulp` and `gulp-less`:</span></span>

```json
{
  "version": "1.0.0",
  "name": "asp.net",
  "private": true,
  "devDependencies": {
    "gulp": "3.9.1",
    "gulp-less": "3.3.0"
  }
}
```

<span data-ttu-id="0c274-138">Установка зависимостей, либо в командной строке, в папку проекта или в Visual Studio **обозревателе решений** (**зависимости > npm > Восстановить пакеты**).</span><span class="sxs-lookup"><span data-stu-id="0c274-138">Install the dependencies either at a command prompt in your project folder, or in Visual Studio **Solution Explorer** (**Dependencies > npm > Restore packages**).</span></span>

```console
npm install
```

![VS восстановления пакетов](less-sass-fa/_static/restore-packages.png)

<span data-ttu-id="0c274-140">Создайте в папке проекта **Gulp файла конфигурации** (*gulpfile.js*) для определения автоматизированного процесса.</span><span class="sxs-lookup"><span data-stu-id="0c274-140">In the project folder, create a **Gulp Configuration File** (*gulpfile.js*) to define the automated process.</span></span>  <span data-ttu-id="0c274-141">Добавьте переменную в верхней части файла для представления менее и менее запуск задания:</span><span class="sxs-lookup"><span data-stu-id="0c274-141">Add a variable at the top of the file to represent Less, and a task to run Less:</span></span>

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less");

gulp.task("less", function () {
  return gulp.src('Styles/main.less')
    .pipe(less())
    .pipe(gulp.dest('wwwroot/css'));
});
```

<span data-ttu-id="0c274-142">Откройте **задач Runner Explorer** (**представление > других окон > задач Runner Explorer**).</span><span class="sxs-lookup"><span data-stu-id="0c274-142">Open the **Task Runner Explorer** (**View > Other Windows > Task Runner Explorer**).</span></span> <span data-ttu-id="0c274-143">Среди задач, вы увидите новую задачу с именем `less`.</span><span class="sxs-lookup"><span data-stu-id="0c274-143">Among the tasks, you should see a new task named `less`.</span></span> <span data-ttu-id="0c274-144">Может потребоваться обновить окно.</span><span class="sxs-lookup"><span data-stu-id="0c274-144">You might have to refresh the window.</span></span>

<span data-ttu-id="0c274-145">Запустите `less` задачей и увидеть результаты, аналогичные как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="0c274-145">Run the `less` task, and you see output similar to what is shown here:</span></span>

![меньше средство запуска задач](less-sass-fa/_static/less-task-runner.png)

<span data-ttu-id="0c274-147">*Wwwroot/css* папка теперь содержит новый файл *main.css*:</span><span class="sxs-lookup"><span data-stu-id="0c274-147">The *wwwroot/css* folder now contains a new file, *main.css*:</span></span>

![основной css создан](less-sass-fa/_static/main-css-created.png)

<span data-ttu-id="0c274-149">Откройте *main.css* и вы увидите примерно следующее:</span><span class="sxs-lookup"><span data-stu-id="0c274-149">Open *main.css* and you see something like the following:</span></span>

```css
body {
    background-color: #336666;
}
.baseColor {
    color: #663333;
}
.bgLight {
    color: #884a44;
}
.bgLight2 {
    color: #aa6355;
}
.bgDark {
    color: #442225;
}
.bgDark2 {
    color: #221114;
}
```

<span data-ttu-id="0c274-150">Добавление простой HTML-страницу для *wwwroot* папки и ссылку *main.css* для просмотра цветовую палитру в действии.</span><span class="sxs-lookup"><span data-stu-id="0c274-150">Add a simple HTML page to the *wwwroot* folder, and reference *main.css* to see the color palette in action.</span></span>

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <link href="css/main.css" rel="stylesheet" />
    <title></title>
</head>
<body>
    <div>
        <div class="baseColor">BaseColor</div>
        <div class="bgLight">Light</div>
        <div class="bgLight2">Light2</div>
        <div class="bgDark">Dark</div>
        <div class="bgDark2">Dark2</div>
    </div>
</body>
</html>
```

<span data-ttu-id="0c274-151">Видно, что 180 градусов вращаться во `@base` используется для создания `@background` завершился противоположных цвет из цветовой круг `@base`:</span><span class="sxs-lookup"><span data-stu-id="0c274-151">You can see that the 180 degree spin on `@base` used to produce `@background` resulted in the color wheel opposing color of `@base`:</span></span>

![Пример менее тестирования](less-sass-fa/_static/less-test-screenshot.png)

<span data-ttu-id="0c274-153">Меньше также поддерживает вложенные правила, а также вложенные медиа-запросами.</span><span class="sxs-lookup"><span data-stu-id="0c274-153">Less also provides support for nested rules, as well as nested media queries.</span></span> <span data-ttu-id="0c274-154">Например определяющей вложенные иерархии, как меню может привести к Подробные правила CSS, такие как следующие:</span><span class="sxs-lookup"><span data-stu-id="0c274-154">For example, defining nested hierarchies like menus can result in verbose CSS rules like these:</span></span>

```css
nav {
    height: 40px;
    width: 100%;
}
nav li {
    height: 38px;
    width: 100px;
}
nav li a:link {
    color: #000;
    text-decoration: none;
}
nav li a:visited {
    text-decoration: none;
    color: #CC3333;
}
nav li a:hover {
    text-decoration: underline;
    font-weight: bold;
}
nav li a:active {
    text-decoration: underline;
}
```

<span data-ttu-id="0c274-155">В идеале все правила стиля, связанных с будут располагаться друг с другом в CSS-файл, но на практике нет ничего применение этого правила, за исключением соглашением и возможно блок комментариев.</span><span class="sxs-lookup"><span data-stu-id="0c274-155">Ideally all of the related style rules will be placed together within the CSS file, but in practice there is nothing enforcing this rule except convention and perhaps block comments.</span></span>

<span data-ttu-id="0c274-156">Определение эти же правила, используя меньше выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0c274-156">Defining these same rules using Less looks like this:</span></span>

```less
nav {
    height: 40px;
    width: 100%;
    li {
        height: 38px;
        width: 100px;
        a {
            color: #000;
            &:link { text-decoration:none}
            &:visited { color: #CC3333; text-decoration:none}
            &:hover { text-decoration:underline; font-weight:bold}
            &:active {text-decoration:underline}
        }
    }
}
```

<span data-ttu-id="0c274-157">Обратите внимание, что в этом случае все подчиненные элементы `nav` находятся в его области.</span><span class="sxs-lookup"><span data-stu-id="0c274-157">Note that in this case, all of the subordinate elements of `nav` are contained within its scope.</span></span> <span data-ttu-id="0c274-158">Больше не все повторения родительских элементов (`nav`, `li`, `a`), и число строк, общее опустилось также (хотя некоторые из, является результатом размещения значений на одной строки во втором примере).</span><span class="sxs-lookup"><span data-stu-id="0c274-158">There is no longer any repetition of parent elements (`nav`, `li`, `a`), and the total line count has dropped as well (though some of that is a result of putting values on the same lines in the second example).</span></span> <span data-ttu-id="0c274-159">Может быть очень полезным, устройствам, для просмотра всех правил для данного элемента пользовательского интерфейса в явно выделенной области, в этом случае отстоящую от остальной части файла в фигурные скобки.</span><span class="sxs-lookup"><span data-stu-id="0c274-159">It can be very helpful, organizationally, to see all of the rules for a given UI element within an explicitly bounded scope, in this case set off from the rest of the file by curly braces.</span></span>

<span data-ttu-id="0c274-160">`&` Синтаксис является функцией селектора меньше с & представляющий родительский объект текущего выбора.</span><span class="sxs-lookup"><span data-stu-id="0c274-160">The `&` syntax is a Less selector feature, with & representing the current selector parent.</span></span> <span data-ttu-id="0c274-161">В этом случае в {...}</span><span class="sxs-lookup"><span data-stu-id="0c274-161">So, within the a {...}</span></span> <span data-ttu-id="0c274-162">блок, `&` представляет `a` тега и, следовательно, `&:link` эквивалентно `a:link`.</span><span class="sxs-lookup"><span data-stu-id="0c274-162">block, `&` represents an `a` tag, and thus `&:link` is equivalent to `a:link`.</span></span>

<span data-ttu-id="0c274-163">Медиа-запросами, очень полезно при создании Дизайн отвечать на запросы, можно также учитывается сильно повторов и сложности в CSS.</span><span class="sxs-lookup"><span data-stu-id="0c274-163">Media queries, extremely useful in creating responsive designs, can also contribute heavily to repetition and complexity in CSS.</span></span> <span data-ttu-id="0c274-164">Меньше позволяет медиа-запросами вкладываются в классах, чтобы определение класса полностью не нужно повторять в разных верхнего уровня `@media` элементов.</span><span class="sxs-lookup"><span data-stu-id="0c274-164">Less allows media queries to be nested within classes, so that the entire class definition doesn't need to be repeated within different top-level `@media` elements.</span></span> <span data-ttu-id="0c274-165">Например вот CSS для быстрого реагирования меню.</span><span class="sxs-lookup"><span data-stu-id="0c274-165">For example, here is CSS for a responsive menu:</span></span>

```css
.navigation {
    margin-top: 30%;
    width: 100%;
}
@media screen and (min-width: 40em) {
    .navigation {
        margin: 0;
    }
}
@media screen and (min-width: 62em) {
    .navigation {
        width: 960px;
        margin: 0;
    }
}
```

<span data-ttu-id="0c274-166">Это может лучше определить менее как:</span><span class="sxs-lookup"><span data-stu-id="0c274-166">This can be better defined in Less as:</span></span>

```less
.navigation {
    margin-top: 30%;
    width: 100%;
    @media screen and (min-width: 40em) {
        margin: 0;
    }
    @media screen and (min-width: 62em) {
        width: 960px;
        margin: 0;
    }
}
```

<span data-ttu-id="0c274-167">Другой функцией меньше, мы уже видели заключается в поддержке математические операции, атрибуты стиля были сконструированы из предопределенных переменных.</span><span class="sxs-lookup"><span data-stu-id="0c274-167">Another feature of Less that we have already seen is its support for mathematical operations, allowing style attributes to be constructed from pre-defined variables.</span></span> <span data-ttu-id="0c274-168">Эта возможность упрощает обновление связанных стили гораздо проще, поскольку базовый переменной могут быть изменены, и все зависимые значения изменяются автоматически.</span><span class="sxs-lookup"><span data-stu-id="0c274-168">This makes updating related styles much easier, since the base variable can be modified and all dependent values change automatically.</span></span>

<span data-ttu-id="0c274-169">Файлов CSS, особенно для больших сайтов (и особенно если используются медиа-запросами), как правило, иметь достаточно большой объем со временем, что работа с ними громоздким.</span><span class="sxs-lookup"><span data-stu-id="0c274-169">CSS files, especially for large sites (and especially if media queries are being used), tend to get quite large over time, making working with them unwieldy.</span></span> <span data-ttu-id="0c274-170">Less-файлы можно задать отдельно, затем извлечено друг с другом с помощью `@import` директивы.</span><span class="sxs-lookup"><span data-stu-id="0c274-170">Less files can be defined separately, then pulled together using `@import` directives.</span></span> <span data-ttu-id="0c274-171">Меньше может также использоваться для импорта отдельных файлов CSS, а также при необходимости.</span><span class="sxs-lookup"><span data-stu-id="0c274-171">Less can also be used to import individual CSS files, as well, if desired.</span></span>

<span data-ttu-id="0c274-172">*Mixins* могут принимать параметры, и меньше поддерживает условную логику в виде mixin условия, которые предоставляют декларативным способом для определения, когда некоторые mixins вступают в силу.</span><span class="sxs-lookup"><span data-stu-id="0c274-172">*Mixins* can accept parameters, and Less supports conditional logic in the form of mixin guards, which provide a declarative way to define when certain mixins take effect.</span></span> <span data-ttu-id="0c274-173">Обычно условия mixin используется для настройки цветов на основе света или является темный цвет источника.</span><span class="sxs-lookup"><span data-stu-id="0c274-173">A common use for mixin guards is to adjust colors based on how light or dark the source color is.</span></span> <span data-ttu-id="0c274-174">Имея mixin, который принимает параметр для цвета, mixin условие можно использовать для изменения mixin, основанного на цвете:</span><span class="sxs-lookup"><span data-stu-id="0c274-174">Given a mixin that accepts a parameter for color, a mixin guard can be used to modify the mixin based on that color:</span></span>

```less
.box (@color) when (lightness(@color) >= 50%) {
    background-color: #000;
}
.box (@color) when (lightness(@color) < 50%) {
    background-color: #FFF;
}
.box (@color) {
    color: @color;
}

.feature {
    .box (@base);
}
```

<span data-ttu-id="0c274-175">Получает нашей текущей `@base` значение `#663333`, этот скрипт не сформирует следующий код CSS:</span><span class="sxs-lookup"><span data-stu-id="0c274-175">Given our current `@base` value of `#663333`, this Less script will produce the following CSS:</span></span>

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

<span data-ttu-id="0c274-176">Меньше предоставляет ряд дополнительных возможностей, но это следует предоставить вам разобраться в степень предварительной обработки языка.</span><span class="sxs-lookup"><span data-stu-id="0c274-176">Less provides a number of additional features, but this should give you some idea of the power of this preprocessing language.</span></span>

## <a name="sass"></a><span data-ttu-id="0c274-177">Sass</span><span class="sxs-lookup"><span data-stu-id="0c274-177">Sass</span></span>

<span data-ttu-id="0c274-178">Sass аналогична меньше, предоставляя поддержку для многих функций, но с небольшими различиями в синтаксисе.</span><span class="sxs-lookup"><span data-stu-id="0c274-178">Sass is similar to Less, providing support for many of the same features, but with slightly different syntax.</span></span> <span data-ttu-id="0c274-179">Она построена на основе Ruby, а не JavaScript и поэтому имеет требования к настройке различных.</span><span class="sxs-lookup"><span data-stu-id="0c274-179">It is built using Ruby, rather than JavaScript, and so has different setup requirements.</span></span> <span data-ttu-id="0c274-180">Исходный язык Sass не использовали фигурных скобок или точкой с запятой, но вместо этого определенные области с помощью пробелов и отступов.</span><span class="sxs-lookup"><span data-stu-id="0c274-180">The original Sass language did not use curly braces or semicolons, but instead defined scope using white space and indentation.</span></span> <span data-ttu-id="0c274-181">В версии 3 Sass был представлен новый синтаксис, **SCSS** («Sassy CSS»).</span><span class="sxs-lookup"><span data-stu-id="0c274-181">In version 3 of Sass, a new syntax was introduced, **SCSS** ("Sassy CSS").</span></span> <span data-ttu-id="0c274-182">SCSS аналогична CSS, поскольку она уровней отступа и пробелы не учитываются и вместо этого использует точки с запятой и фигурные скобки.</span><span class="sxs-lookup"><span data-stu-id="0c274-182">SCSS is similar to CSS in that it ignores indentation levels and whitespace, and instead uses semicolons and curly braces.</span></span>

<span data-ttu-id="0c274-183">Чтобы установить Sass, обычно сначала установить Ruby (предустановлен на Mac) и затем запустите:</span><span class="sxs-lookup"><span data-stu-id="0c274-183">To install Sass, typically you would first install Ruby (pre-installed on Mac), and then run:</span></span>

```console
gem install sass
```

<span data-ttu-id="0c274-184">Тем не менее при запуске Visual Studio, вы можете начать с Sass во многом так же, как и с меньшим.</span><span class="sxs-lookup"><span data-stu-id="0c274-184">However, if you're running Visual Studio, you can get started with Sass in much the same way as you would with Less.</span></span> <span data-ttu-id="0c274-185">Откройте *package.json* и добавить пакет «gulp sass» `devDependencies`:</span><span class="sxs-lookup"><span data-stu-id="0c274-185">Open *package.json* and add the "gulp-sass" package to `devDependencies`:</span></span>

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

<span data-ttu-id="0c274-186">Затем измените *gulpfile.js* Добавление переменной sass и задач для компиляции файлов Sass и поместить результаты в папку wwwroot:</span><span class="sxs-lookup"><span data-stu-id="0c274-186">Next, modify *gulpfile.js* to add a sass variable and a task to compile your Sass files and place the results in the wwwroot folder:</span></span>

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less"),
  sass = require("gulp-sass");

// other content removed

gulp.task("sass", function () {
  return gulp.src('Styles/main2.scss')
    .pipe(sass())
    .pipe(gulp.dest('wwwroot/css'));
});
```

<span data-ttu-id="0c274-187">Теперь можно добавить файл Sass *main2.scss* для *стили* папку в корневой папке проекта:</span><span class="sxs-lookup"><span data-stu-id="0c274-187">Now you can add the Sass file *main2.scss* to the *Styles* folder in the root of the project:</span></span>

![Добавьте файл scss](less-sass-fa/_static/add-scss-file.png)

<span data-ttu-id="0c274-189">Откройте *main2.scss* и добавьте следующий код:</span><span class="sxs-lookup"><span data-stu-id="0c274-189">Open *main2.scss* and add the following:</span></span>

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

<span data-ttu-id="0c274-190">Сохраните все файлы.</span><span class="sxs-lookup"><span data-stu-id="0c274-190">Save all of your files.</span></span> <span data-ttu-id="0c274-191">Теперь при обновлении **Task Runner Explorer**, вы видите `sass` задачи.</span><span class="sxs-lookup"><span data-stu-id="0c274-191">Now when you refresh **Task Runner Explorer**, you see a `sass` task.</span></span> <span data-ttu-id="0c274-192">Запустите его, а в */wwwroot/css* папки.</span><span class="sxs-lookup"><span data-stu-id="0c274-192">Run it, and look in the */wwwroot/css* folder.</span></span> <span data-ttu-id="0c274-193">Теперь есть *main2.css* файл с этого содержимого:</span><span class="sxs-lookup"><span data-stu-id="0c274-193">There is now a *main2.css* file, with these contents:</span></span>

```css
body {
    background-color: #CC0000;
}
```

<span data-ttu-id="0c274-194">Sass поддерживает вложение так же не меньше обозревателем, обеспечивая выгоды как.</span><span class="sxs-lookup"><span data-stu-id="0c274-194">Sass supports nesting in much the same was that Less does, providing similar benefits.</span></span> <span data-ttu-id="0c274-195">Файлы можно разделить по функциям и включены с помощью `@import` директиву:</span><span class="sxs-lookup"><span data-stu-id="0c274-195">Files can be split up by function and included using the `@import` directive:</span></span>

```sass
@import 'anotherfile';
```

<span data-ttu-id="0c274-196">Sass поддерживает mixins, с помощью `@mixin` ключевое слово для их определения и `@include` Чтобы включить их, как показано в примере из [sass lang.com](http://sass-lang.com):</span><span class="sxs-lookup"><span data-stu-id="0c274-196">Sass supports mixins as well, using the `@mixin` keyword to define them and `@include` to include them, as in this example from [sass-lang.com](http://sass-lang.com):</span></span>

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

<span data-ttu-id="0c274-197">В дополнение к mixins Sass также поддерживает концепцию наследования, позволяя расширить другой один класс.</span><span class="sxs-lookup"><span data-stu-id="0c274-197">In addition to mixins, Sass also supports the concept of inheritance, allowing one class to extend another.</span></span> <span data-ttu-id="0c274-198">Он аналогичен сочетание, но результаты в меньший объем кода CSS.</span><span class="sxs-lookup"><span data-stu-id="0c274-198">It's conceptually similar to a mixin, but results in less CSS code.</span></span> <span data-ttu-id="0c274-199">Выполняется с помощью `@extend` ключевое слово.</span><span class="sxs-lookup"><span data-stu-id="0c274-199">It's accomplished using the `@extend` keyword.</span></span> <span data-ttu-id="0c274-200">Чтобы проверить mixins, добавьте следующий код в вашей *main2.scss* файла:</span><span class="sxs-lookup"><span data-stu-id="0c274-200">To try out mixins, add the following to your *main2.scss* file:</span></span>

```sass
@mixin alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @include alert;
    border-color: green;
}

.error {
    @include alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

<span data-ttu-id="0c274-201">Изучите выходные данные в *main2.css* после выполнения команды `sass` задач в **Task Runner Explorer**:</span><span class="sxs-lookup"><span data-stu-id="0c274-201">Examine the output in *main2.css* after running the `sass` task in **Task Runner Explorer**:</span></span>

```css
.success {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    border-color: green;
 }

.error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    color: red;
    border-color: red;
    font-weight: bold;
}
```

<span data-ttu-id="0c274-202">Обратите внимание, что все общие свойства предупреждения mixin повторяются в каждом классе.</span><span class="sxs-lookup"><span data-stu-id="0c274-202">Notice that all of the common properties of the alert mixin are repeated in each class.</span></span> <span data-ttu-id="0c274-203">Mixin была хорошие помогает избежать двойной во время разработки, но по-прежнему является создание CSS с большим количеством дубликатов, большего, чем необходимые файлы CSS - потенциальных проблем производительности в результате чего.</span><span class="sxs-lookup"><span data-stu-id="0c274-203">The mixin did a good job of helping eliminate duplication at development time, but it's still creating CSS with a lot of duplication in it, resulting in larger than necessary CSS files - a potential performance issue.</span></span>

<span data-ttu-id="0c274-204">Теперь замените предупреждения сочетание с `.alert` класса и измените `@include` для `@extend` (Обратите внимание расширить `.alert`, а не `alert`):</span><span class="sxs-lookup"><span data-stu-id="0c274-204">Now replace the alert mixin with a `.alert` class, and change `@include` to `@extend` (remembering to extend `.alert`, not `alert`):</span></span>

```sass
.alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @extend .alert;
    border-color: green;
}

.error {
    @extend .alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

<span data-ttu-id="0c274-205">Еще раз запустите Sass и изучите полученный CSS.</span><span class="sxs-lookup"><span data-stu-id="0c274-205">Run Sass once more, and examine the resulting CSS:</span></span>

```css
.alert, .success, .error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    border-color: green;
}

.error {
    color: red;
    border-color: red;
    font-weight: bold;
}
```

<span data-ttu-id="0c274-206">Теперь свойства определяются только как необходимое число раз и качественного CSS.</span><span class="sxs-lookup"><span data-stu-id="0c274-206">Now the properties are defined only as many times as needed, and better CSS is generated.</span></span>

<span data-ttu-id="0c274-207">Sass также включает функции и условной логики операций, аналогично меньше.</span><span class="sxs-lookup"><span data-stu-id="0c274-207">Sass also includes functions and conditional logic operations, similar to Less.</span></span> <span data-ttu-id="0c274-208">На самом деле возможности двух языках очень похожи.</span><span class="sxs-lookup"><span data-stu-id="0c274-208">In fact, the two languages' capabilities are very similar.</span></span>

## <a name="less-or-sass"></a><span data-ttu-id="0c274-209">Меньше или Sass?</span><span class="sxs-lookup"><span data-stu-id="0c274-209">Less or Sass?</span></span>

<span data-ttu-id="0c274-210">По-прежнему не согласие относительно будь то обычно лучше использовать меньше или Sass (или даже следует ли использовать исходный Sass или новых правил синтаксиса SCSS в Sass).</span><span class="sxs-lookup"><span data-stu-id="0c274-210">There is still no consensus as to whether it's generally better to use Less or Sass (or even whether to prefer the original Sass or the newer SCSS syntax within Sass).</span></span> <span data-ttu-id="0c274-211">Вероятно, является наиболее важным параметром для **используйте один из этих средств**, в отличие от только что программирование вручную CSS-файлах.</span><span class="sxs-lookup"><span data-stu-id="0c274-211">Probably the most important decision is to **use one of these tools**, as opposed to just hand-coding your CSS files.</span></span> <span data-ttu-id="0c274-212">После внесения, принятия решений, оба из них меньше и Sass можно выбрать.</span><span class="sxs-lookup"><span data-stu-id="0c274-212">Once you've made that decision, both Less and Sass are good choices.</span></span>

## <a name="font-awesome"></a><span data-ttu-id="0c274-213">Здорово шрифта</span><span class="sxs-lookup"><span data-stu-id="0c274-213">Font Awesome</span></span>

<span data-ttu-id="0c274-214">Помимо CSS-препроцессоры весьма полезным ресурсом для современных веб-приложений стиля замечательно шрифта.</span><span class="sxs-lookup"><span data-stu-id="0c274-214">In addition to CSS preprocessors, another great resource for styling modern web applications is Font Awesome.</span></span> <span data-ttu-id="0c274-215">Шрифт Awesome — это набор средств, который предоставляет более 500 масштабируемой векторной значков, которые можно свободно использовать в веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="0c274-215">Font Awesome is a toolkit that provides over 500 scalable vector icons that can be freely used in your web applications.</span></span> <span data-ttu-id="0c274-216">Он изначально был разработан для работы с начальной загрузки, но имеет никакой зависимости на этой структуре или на любой библиотек JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0c274-216">It was originally designed to work with Bootstrap, but it has no dependency on that framework or on any JavaScript libraries.</span></span>

<span data-ttu-id="0c274-217">Чтобы приступить к работе с шрифта здорово проще всего добавить ссылку на него, с помощью своего расположения в сети (CDN) открытые доставки содержимого:</span><span class="sxs-lookup"><span data-stu-id="0c274-217">The easiest way to get started with Font Awesome is to add a reference to it, using its public content delivery network (CDN) location:</span></span>

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

<span data-ttu-id="0c274-218">Его можно также добавить в проект Visual Studio, добавьте его в «зависимости» в *bower.json*:</span><span class="sxs-lookup"><span data-stu-id="0c274-218">You can also add it to your Visual Studio project by adding it to the "dependencies" in *bower.json*:</span></span>

```json
{
  "name": "ASP.NET",
  "private": true,
  "dependencies": {
    "bootstrap": "3.0.0",
    "jquery": "1.10.2",
    "jquery-validation": "1.11.1",
    "jquery-validation-unobtrusive": "3.2.2",
    "hammer.js": "2.0.4",
    "bootstrap-touch-carousel": "0.8.0",
    "Font-Awesome": "4.3.0"
  }
}
```

<span data-ttu-id="0c274-219">Получив ссылку на здорово шрифта на странице, можно добавить значки приложения путем применения шрифта здорово классов, обычно с префиксом «ОС-», для элементов встроенный HTML (например, `<span>` или `<i>`).</span><span class="sxs-lookup"><span data-stu-id="0c274-219">Once you have a reference to Font Awesome on a page, you can add icons to your application by applying Font Awesome classes, typically prefixed with "fa-", to your inline HTML elements (such as `<span>` or `<i>`).</span></span>  <span data-ttu-id="0c274-220">Например можно добавить значки для простых списков и меню, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="0c274-220">For example, you can add icons to simple lists and menus using code like this:</span></span>

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title></title>
    <link href="lib/font-awesome/css/font-awesome.css" rel="stylesheet" />
</head>
<body>
    <ul class="fa-ul">
        <li><i class="fa fa-li fa-home"></i> Home</li>
        <li><i class="fa fa-li fa-cog"></i> Settings</li>
    </ul>
</body>
</html>
```

<span data-ttu-id="0c274-221">В результате получается следующее в браузере — Обратите внимание на значок рядом с каждого элемента:</span><span class="sxs-lookup"><span data-stu-id="0c274-221">This produces the following in the browser - note the icon beside each item:</span></span>

![значки списка](less-sass-fa/_static/list-icons-screenshot.png)

<span data-ttu-id="0c274-223">Можно просмотреть полный список доступных значков здесь:</span><span class="sxs-lookup"><span data-stu-id="0c274-223">You can view a complete list of the available icons here:</span></span>

<span data-ttu-id="0c274-224">http://fontawesome.IO/icons/</span><span class="sxs-lookup"><span data-stu-id="0c274-224">http://fontawesome.io/icons/</span></span>

## <a name="summary"></a><span data-ttu-id="0c274-225">Сводка</span><span class="sxs-lookup"><span data-stu-id="0c274-225">Summary</span></span>

<span data-ttu-id="0c274-226">Все более современных веб-приложений требуют отвечать на запросы, жидкости макеты чистой, интуитивно понятный и простой в использовании из различных устройств.</span><span class="sxs-lookup"><span data-stu-id="0c274-226">Modern web applications increasingly demand responsive, fluid designs that are clean, intuitive, and easy to use from a variety of devices.</span></span> <span data-ttu-id="0c274-227">Управление сложности таблицы стилей CSS, необходимые для достижения этих целей лучше всего проводить с помощью препроцессора like менее или Sass.</span><span class="sxs-lookup"><span data-stu-id="0c274-227">Managing the complexity of the CSS stylesheets required to achieve these goals is best done using a preprocessor like Less or Sass.</span></span> <span data-ttu-id="0c274-228">Кроме того наборы средств, как шрифт здорово быстро предоставлять хорошо известных значков в меню навигации текстовое и взаимодействие с кнопками, повышая общую пользователя приложения.</span><span class="sxs-lookup"><span data-stu-id="0c274-228">In addition, toolkits like Font Awesome quickly provide well-known icons to textual navigation menus and buttons, improving the overall user experience of your application.</span></span>
