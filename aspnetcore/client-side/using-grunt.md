---
title: "С помощью Grunt в ASP.NET Core"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 471112e9-2c33-454b-96fc-32916102ce73
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-grunt
ms.openlocfilehash: 8ae50514ce24c7f9e3bb1e347d5d860e1de43c5f
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="using-grunt-in-aspnet-core"></a><span data-ttu-id="24d6e-103">С помощью Grunt в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="24d6e-103">Using Grunt in ASP.NET Core</span></span> 

<span data-ttu-id="24d6e-104">По [Ноэл Риса](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span><span class="sxs-lookup"><span data-stu-id="24d6e-104">By [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span></span>

<span data-ttu-id="24d6e-105">Grunt-это средство запуска задач JavaScript, который автоматизирует минификации сценария, компиляцию TypeScript, средства «корпус» качество кода, предварительной процессоров CSS и практически любой повторяющихся тяжелой задачей, должен образом для поддержки разработки клиентских.</span><span class="sxs-lookup"><span data-stu-id="24d6e-105">Grunt is a JavaScript task runner that automates script minification, TypeScript compilation, code quality "lint" tools, CSS pre-processors, and just about any repetitive chore that needs doing to support client development.</span></span> <span data-ttu-id="24d6e-106">Grunt полностью поддерживается в Visual Studio на то, что шаблоны проектов ASP.NET по умолчанию используется Gulp (см. [с помощью Gulp](using-gulp.md)).</span><span class="sxs-lookup"><span data-stu-id="24d6e-106">Grunt is fully supported in Visual Studio, though the ASP.NET project templates use Gulp by default (see [Using Gulp](using-gulp.md)).</span></span>

<span data-ttu-id="24d6e-107">В этом примере использует пустой проект ASP.NET Core начальной точкой, чтобы показать, как автоматизировать процесс построения клиента с нуля.</span><span class="sxs-lookup"><span data-stu-id="24d6e-107">This example uses an empty ASP.NET Core project as its starting point, to show how to automate the client build process from scratch.</span></span>

<span data-ttu-id="24d6e-108">Готовый пример удаляет целевой каталог развертывания, объединяет файлы JavaScript, проверяет качество кода, также сжимает содержимое файла JavaScript и развертывает в корневой каталог веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="24d6e-108">The finished example cleans the target deployment directory, combines JavaScript files, checks code quality, condenses JavaScript file content and deploys to the root of your web application.</span></span> <span data-ttu-id="24d6e-109">Мы будем использовать следующие пакеты:</span><span class="sxs-lookup"><span data-stu-id="24d6e-109">We will use the following packages:</span></span>

* <span data-ttu-id="24d6e-110">**grunt**: средство запуска задач Grunt пакет.</span><span class="sxs-lookup"><span data-stu-id="24d6e-110">**grunt**: The Grunt task runner package.</span></span>

* <span data-ttu-id="24d6e-111">**Очистка grunt contrib**: подключаемый модуль, который удаляет файлы или каталоги.</span><span class="sxs-lookup"><span data-stu-id="24d6e-111">**grunt-contrib-clean**: A plugin that removes files or directories.</span></span>

* <span data-ttu-id="24d6e-112">**grunt-contrib-jshint**: подключаемый модуль, который просматривает качества кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="24d6e-112">**grunt-contrib-jshint**: A plugin that reviews JavaScript code quality.</span></span>

* <span data-ttu-id="24d6e-113">**grunt-contrib-concat**: подключаемый модуль, который объединяет файлы в один файл.</span><span class="sxs-lookup"><span data-stu-id="24d6e-113">**grunt-contrib-concat**: A plugin that joins files into a single file.</span></span>

* <span data-ttu-id="24d6e-114">**grunt contrib uglify**: подключаемый модуль, который уменьшает JavaScript для уменьшения размера.</span><span class="sxs-lookup"><span data-stu-id="24d6e-114">**grunt-contrib-uglify**: A plugin that minifies JavaScript to reduce size.</span></span>

* <span data-ttu-id="24d6e-115">**grunt contrib Контрольные значения**: подключаемый модуль, который отслеживает действия файла.</span><span class="sxs-lookup"><span data-stu-id="24d6e-115">**grunt-contrib-watch**: A plugin that watches file activity.</span></span>

## <a name="preparing-the-application"></a><span data-ttu-id="24d6e-116">Подготовка приложения</span><span class="sxs-lookup"><span data-stu-id="24d6e-116">Preparing the application</span></span>

<span data-ttu-id="24d6e-117">Чтобы начать, задать новый пустой веб-приложения и добавьте файлы пример TypeScript.</span><span class="sxs-lookup"><span data-stu-id="24d6e-117">To begin, set up a new empty web application and add TypeScript example files.</span></span> <span data-ttu-id="24d6e-118">Файлы typeScript автоматически компилируется в JavaScript с использованием параметров по умолчанию Visual Studio и будут нашей сырье для обработки с помощью Grunt.</span><span class="sxs-lookup"><span data-stu-id="24d6e-118">TypeScript files are automatically compiled into JavaScript using default Visual Studio settings and will be our raw material to process using Grunt.</span></span>

1.  <span data-ttu-id="24d6e-119">В Visual Studio создайте новый `ASP.NET Web Application`.</span><span class="sxs-lookup"><span data-stu-id="24d6e-119">In Visual Studio, create a new `ASP.NET Web Application`.</span></span>

2.  <span data-ttu-id="24d6e-120">В **новый проект ASP.NET** диалоговое окно, выберите ASP.NET Core **пустой** шаблона и нажмите кнопку "ОК".</span><span class="sxs-lookup"><span data-stu-id="24d6e-120">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template and click the OK button.</span></span>

3.  <span data-ttu-id="24d6e-121">Проверьте структура проекта в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="24d6e-121">In the Solution Explorer, review the project structure.</span></span> <span data-ttu-id="24d6e-122">`\src` Папка содержит пустой `wwwroot` и `Dependencies` узлов.</span><span class="sxs-lookup"><span data-stu-id="24d6e-122">The `\src` folder includes empty `wwwroot` and `Dependencies` nodes.</span></span>

    ![пустой веб-решений](using-grunt/_static/grunt-solution-explorer.png)

4.  <span data-ttu-id="24d6e-124">Добавить новую папку с именем `TypeScript` в каталог проекта.</span><span class="sxs-lookup"><span data-stu-id="24d6e-124">Add a new folder named `TypeScript` to your project directory.</span></span>

5.  <span data-ttu-id="24d6e-125">Перед добавлением все файлы, убедитесь, что Visual Studio имеет параметр "компиляции при сохранении" для возвращенных файлов TypeScript.</span><span class="sxs-lookup"><span data-stu-id="24d6e-125">Before adding any files, let’s make sure that Visual Studio has the option 'compile on save' for TypeScript files checked.</span></span> <span data-ttu-id="24d6e-126">*Сервис > Параметры > текстовый редактор > Typescript > проекта*</span><span class="sxs-lookup"><span data-stu-id="24d6e-126">*Tools > Options > Text Editor > Typescript > Project*</span></span>

    ![Параметры автоматического compliation файлов TypeScript](using-grunt/_static/typescript-options.png)

6.  <span data-ttu-id="24d6e-128">Щелкните правой кнопкой мыши `TypeScript` каталог и выберите **Добавить > новый элемент** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="24d6e-128">Right-click the `TypeScript` directory and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="24d6e-129">Выберите **файл JavaScript** элемента и присвойте файлу имя *Tastes.ts* (Примечание \*.ts расширения).</span><span class="sxs-lookup"><span data-stu-id="24d6e-129">Select the **JavaScript file** item and name the file *Tastes.ts* (note the \*.ts extension).</span></span> <span data-ttu-id="24d6e-130">Скопируйте строку кода TypeScript под в файле (при сохранении, новый *Tastes.js* появится файл с исходного кода JavaScript).</span><span class="sxs-lookup"><span data-stu-id="24d6e-130">Copy the line of TypeScript code below into the file (when you save, a new *Tastes.js* file will appear with the JavaScript source).</span></span>
    
    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7.  <span data-ttu-id="24d6e-131">Добавьте второй файл, чтобы **TypeScript** каталог и назовите его `Food.ts`.</span><span class="sxs-lookup"><span data-stu-id="24d6e-131">Add a second file to the **TypeScript** directory and name it `Food.ts`.</span></span> <span data-ttu-id="24d6e-132">Скопируйте приведенный ниже код в файл.</span><span class="sxs-lookup"><span data-stu-id="24d6e-132">Copy the code below into the file.</span></span>

    ```typescript
    class Food {
      constructor(name: string, calories: number) {
        this._name = name;
        this._calories = calories;
      }
    
      private _name: string;
      get Name() {
        return this._name;
      }
    
      private _calories: number;
      get Calories() {
        return this._calories;
      }
    
      private _taste: Tastes;
      get Taste(): Tastes { return this._taste }
      set Taste(value: Tastes) {
        this._taste = value;
      }
    }
    ```

## <a name="configuring-npm"></a><span data-ttu-id="24d6e-133">Настройка NPM</span><span class="sxs-lookup"><span data-stu-id="24d6e-133">Configuring NPM</span></span>

<span data-ttu-id="24d6e-134">Настройте NPM для загрузки grunt и grunt задачи.</span><span class="sxs-lookup"><span data-stu-id="24d6e-134">Next, configure NPM to download grunt and grunt-tasks.</span></span>

1. <span data-ttu-id="24d6e-135">В обозревателе решений щелкните правой кнопкой мыши проект и выберите **Добавить > новый элемент** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="24d6e-135">In the Solution Explorer, right-click the project and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="24d6e-136">Выберите **файл конфигурации NPM** товара, оставьте имя по умолчанию *package.json*и нажмите кнопку **добавить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="24d6e-136">Select the **NPM configuration file** item, leave the default name, *package.json*, and click the **Add** button.</span></span>

2. <span data-ttu-id="24d6e-137">В *package.json* файла внутри `devDependencies` объекта фигурные скобки, введите «grunt».</span><span class="sxs-lookup"><span data-stu-id="24d6e-137">In the *package.json* file, inside the `devDependencies` object braces, enter "grunt".</span></span> <span data-ttu-id="24d6e-138">Выберите `grunt` из Intellisense списка и нажмите клавишу ВВОД.</span><span class="sxs-lookup"><span data-stu-id="24d6e-138">Select `grunt` from the Intellisense list and press the Enter key.</span></span> <span data-ttu-id="24d6e-139">Visual Studio Квота grunt имя пакета и добавьте двоеточие.</span><span class="sxs-lookup"><span data-stu-id="24d6e-139">Visual Studio will quote the grunt package name, and add a colon.</span></span> <span data-ttu-id="24d6e-140">Справа от запятой выберите последняя стабильная версия пакета в верхней части списка Intellisense (нажмите клавишу `Ctrl-Space` Если Intellisense не отображается).</span><span class="sxs-lookup"><span data-stu-id="24d6e-140">To the right of the colon, select the latest stable version of the package from the top of the Intellisense list (press `Ctrl-Space` if Intellisense does not appear).</span></span>

    ![grun Intellisense](using-grunt/_static/devdependencies-grunt.png)
    
    > [!NOTE]
    > <span data-ttu-id="24d6e-142">Использует NPM [семантического управления версиями](http://semver.org/) организовать зависимостей.</span><span class="sxs-lookup"><span data-stu-id="24d6e-142">NPM uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="24d6e-143">Семантического управления версиями, также известный как SemVer определяет пакеты с формирование <major>.<minor>. <patch>. IntelliSense упрощает семантического управления версиями, отображая только несколько распространенных вариантов.</span><span class="sxs-lookup"><span data-stu-id="24d6e-143">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme <major>.<minor>.<patch>. Intellisense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="24d6e-144">Верхний элемент в списке Intellisense (0.4.5 в приведенном выше примере) считается последняя стабильная версия пакета.</span><span class="sxs-lookup"><span data-stu-id="24d6e-144">The top item in the Intellisense list (0.4.5 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="24d6e-145">Знак вставки (^) соответствует самой последней основной номер версии, а тильда (~) соответствует последнему дополнительному номеру версии.</span><span class="sxs-lookup"><span data-stu-id="24d6e-145">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span> <span data-ttu-id="24d6e-146">В разделе [NPM semver версия средства синтаксического анализа ссылки](https://www.npmjs.com/package/semver) в соответствии с полной функциональность, предоставляющий SemVer.</span><span class="sxs-lookup"><span data-stu-id="24d6e-146">See the [NPM semver version parser reference](https://www.npmjs.com/package/semver) as a guide to the full expressivity that SemVer provides.</span></span>

3. <span data-ttu-id="24d6e-147">Добавьте дополнительные зависимости, для загрузки grunt-contrib -\* пакетов для *чистой*, *jshint*, *concat*, *uglify*и *Контрольные значения* как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="24d6e-147">Add more dependencies to load grunt-contrib-\* packages for *clean*, *jshint*, *concat*, *uglify*, and *watch* as shown in the example below.</span></span> <span data-ttu-id="24d6e-148">Версии не обязательно в соответствии с примером.</span><span class="sxs-lookup"><span data-stu-id="24d6e-148">The versions do not need to match the example.</span></span>

    ```json
    "devDependencies": {
      "grunt": "0.4.5",
      "grunt-contrib-clean": "0.6.0",
      "grunt-contrib-jshint": "0.11.0",
      "grunt-contrib-concat": "0.5.1",
      "grunt-contrib-uglify": "0.8.0",
      "grunt-contrib-watch": "0.6.1"
    }
    ```

4. <span data-ttu-id="24d6e-149">Сохранить *package.json* файла.</span><span class="sxs-lookup"><span data-stu-id="24d6e-149">Save the *package.json* file.</span></span>

<span data-ttu-id="24d6e-150">Пакеты для каждого элемента devDependencies будут загружены и все файлы, необходимые для каждого пакета.</span><span class="sxs-lookup"><span data-stu-id="24d6e-150">The packages for each devDependencies item will download, along with any files that each package requires.</span></span> <span data-ttu-id="24d6e-151">Можно найти файлы пакета в `node_modules` каталога, позволяя **Показать все файлы** кнопку в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="24d6e-151">You can find the package files in the `node_modules` directory by enabling the **Show All Files** button in the Solution Explorer.</span></span>

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> <span data-ttu-id="24d6e-153">Если необходимо, можно восстановить вручную, зависимости в обозревателе решений щелкните правой кнопкой мыши на `Dependencies\NPM` и выбрав **восстановить пакеты** пункт меню.</span><span class="sxs-lookup"><span data-stu-id="24d6e-153">If you need to, you can manually restore dependencies in Solution Explorer by right-clicking on `Dependencies\NPM` and selecting the **Restore Packages** menu option.</span></span>

![Восстановление пакетов](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a><span data-ttu-id="24d6e-155">Настройка Grunt</span><span class="sxs-lookup"><span data-stu-id="24d6e-155">Configuring Grunt</span></span>

<span data-ttu-id="24d6e-156">Grunt настраивается с помощью манифест с именем *Gruntfile.js* , определяет, загружает и регистрирует задачи, которые можно запускать вручную или с запуском автоматически на основе событий в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="24d6e-156">Grunt is configured using a manifest named *Gruntfile.js* that defines, loads and registers tasks that can be run manually or configured to run automatically based on events in Visual Studio.</span></span>

1.  <span data-ttu-id="24d6e-157">Щелкните правой кнопкой мыши проект и выберите **Добавить > новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="24d6e-157">Right-click the project and select **Add > New Item**.</span></span> <span data-ttu-id="24d6e-158">Выберите **файла конфигурации Grunt** , то оставьте имя по умолчанию *Gruntfile.js*и нажмите кнопку **добавить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="24d6e-158">Select the **Grunt Configuration file** option, leave the default name, *Gruntfile.js*, and click the **Add** button.</span></span>

    <span data-ttu-id="24d6e-159">Исходный код включает определение модуля и `grunt.initConfig()` метод.</span><span class="sxs-lookup"><span data-stu-id="24d6e-159">The initial code includes a module definition and the `grunt.initConfig()` method.</span></span> <span data-ttu-id="24d6e-160">`initConfig()` Используется для задания параметров для каждого пакета и остальная часть модуль будет загрузить и зарегистрировать задачи.</span><span class="sxs-lookup"><span data-stu-id="24d6e-160">The `initConfig()` is used to set options for each package, and the remainder of the module will load and register tasks.</span></span>
    
    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
      });
    };
    ```

2. <span data-ttu-id="24d6e-161">Внутри `initConfig()` метод, добавьте параметры для `clean` задач, как показано в примере *Gruntfile.js* ниже.</span><span class="sxs-lookup"><span data-stu-id="24d6e-161">Inside the `initConfig()` method, add options for the `clean` task as shown in the example *Gruntfile.js* below.</span></span> <span data-ttu-id="24d6e-162">Задачу "Очистить" принимает массив строк каталогов.</span><span class="sxs-lookup"><span data-stu-id="24d6e-162">The clean task accepts an array of directory strings.</span></span> <span data-ttu-id="24d6e-163">Эта задача удаляет файлы из wwwroot/lib и удаляет весь/временный каталог.</span><span class="sxs-lookup"><span data-stu-id="24d6e-163">This task removes files from wwwroot/lib and removes the entire /temp directory.</span></span>

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. <span data-ttu-id="24d6e-164">Под методом initConfig(), добавьте вызов `grunt.loadNpmTasks()`.</span><span class="sxs-lookup"><span data-stu-id="24d6e-164">Below the initConfig() method, add a call to `grunt.loadNpmTasks()`.</span></span> <span data-ttu-id="24d6e-165">Это сделает задачи запускаемых из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="24d6e-165">This will make the task runnable from Visual Studio.</span></span>

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. <span data-ttu-id="24d6e-166">Сохранить *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="24d6e-166">Save *Gruntfile.js*.</span></span> <span data-ttu-id="24d6e-167">Файл должен выглядеть примерно на снимке экрана ниже.</span><span class="sxs-lookup"><span data-stu-id="24d6e-167">The file should look something like the screenshot below.</span></span>

    ![Начальное gruntfile](using-grunt/_static/gruntfile-js-initial.png)

5. <span data-ttu-id="24d6e-169">Щелкните правой кнопкой мыши *Gruntfile.js* и выберите **Task Runner Explorer** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="24d6e-169">Right-click *Gruntfile.js* and select **Task Runner Explorer** from the context menu.</span></span> <span data-ttu-id="24d6e-170">Откроется окно Task Runner Explorer.</span><span class="sxs-lookup"><span data-stu-id="24d6e-170">The Task Runner Explorer window will open.</span></span>

    ![меню обозревателя средство запуска задач](using-grunt/_static/task-runner-explorer-menu.png)

6. <span data-ttu-id="24d6e-172">Убедитесь, что `clean` области отображается **задачи** в Task Runner Explorer.</span><span class="sxs-lookup"><span data-stu-id="24d6e-172">Verify that `clean` shows under **Tasks** in the Task Runner Explorer.</span></span>

    ![Список задач задач runner explorer](using-grunt/_static/task-runner-explorer-tasks.png)

7. <span data-ttu-id="24d6e-174">Щелкните правой кнопкой мыши задачу "Очистить" и выберите **запуска** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="24d6e-174">Right-click the clean task and select **Run** from the context menu.</span></span> <span data-ttu-id="24d6e-175">Окно командной строки отображает ход выполнения задачи.</span><span class="sxs-lookup"><span data-stu-id="24d6e-175">A command window displays progress of the task.</span></span>

    ![Задача runner explorer запустите задачу "Очистить"](using-grunt/_static/task-runner-explorer-run-clean.png)
    
    > [!NOTE]
    > <span data-ttu-id="24d6e-177">Отсутствуют файлы или каталоги для очистки еще.</span><span class="sxs-lookup"><span data-stu-id="24d6e-177">There are no files or directories to clean yet.</span></span> <span data-ttu-id="24d6e-178">При желании можно создать их вручную в обозревателе решений и запустите задачу "Очистить" в качестве теста.</span><span class="sxs-lookup"><span data-stu-id="24d6e-178">If you like, you can manually create them in the Solution Explorer and then run the clean task as a test.</span></span>
    
8. <span data-ttu-id="24d6e-179">В этом методе initConfig(), добавьте запись для `concat` ниже коде.</span><span class="sxs-lookup"><span data-stu-id="24d6e-179">In the initConfig() method, add an entry for `concat` using the code below.</span></span>

    <span data-ttu-id="24d6e-180">`src` Массива свойства перечислены файлы для объединения, в том порядке, они должны быть объединены.</span><span class="sxs-lookup"><span data-stu-id="24d6e-180">The `src` property array lists files to combine, in the order that they should be combined.</span></span> <span data-ttu-id="24d6e-181">`dest` Свойство путь присваивается объединенного файла, который создается.</span><span class="sxs-lookup"><span data-stu-id="24d6e-181">The `dest` property assigns the path to the combined file that is produced.</span></span>

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```
    
    > [!NOTE]
    > <span data-ttu-id="24d6e-182">`all` Свойство в приведенном выше коде это имя целевого объекта.</span><span class="sxs-lookup"><span data-stu-id="24d6e-182">The `all` property in the code above is the name of a target.</span></span> <span data-ttu-id="24d6e-183">Целевые объекты используются в некоторых задачах Grunt для разрешения нескольких средах построения.</span><span class="sxs-lookup"><span data-stu-id="24d6e-183">Targets are used in some Grunt tasks to allow multiple build environments.</span></span> <span data-ttu-id="24d6e-184">Позволяет просмотреть встроенных конечных объектов, с помощью Intellisense или назначить собственные.</span><span class="sxs-lookup"><span data-stu-id="24d6e-184">You can view the built-in targets using Intellisense or assign your own.</span></span>
    
9. <span data-ttu-id="24d6e-185">Добавить `jshint` задачи, используя приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="24d6e-185">Add the `jshint` task using the code below.</span></span>

    <span data-ttu-id="24d6e-186">Программа jshint качества кода выполняется для каждого файла JavaScript, найденные в каталоге temp.</span><span class="sxs-lookup"><span data-stu-id="24d6e-186">The jshint code-quality utility is run against every JavaScript file found in the temp directory.</span></span>
    
    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="24d6e-187">Параметр «-W069» ошибка произведенное jshint при JavaScript используется квадратная скобка синтаксис, т. е. присвоение свойства вместо точечной нотации `Tastes["Sweet"]` вместо `Tastes.Sweet`.</span><span class="sxs-lookup"><span data-stu-id="24d6e-187">The option "-W069" is an error produced by jshint when JavaScript uses bracket syntax to assign a property instead of dot notation, i.e. `Tastes["Sweet"]` instead of `Tastes.Sweet`.</span></span> <span data-ttu-id="24d6e-188">Этот параметр отключает предупреждения, чтобы разрешить оставшаяся часть процедуры, чтобы продолжить.</span><span class="sxs-lookup"><span data-stu-id="24d6e-188">The option turns off the warning to allow the rest of the process to continue.</span></span>

10.  <span data-ttu-id="24d6e-189">Добавить `uglify` задачи, используя приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="24d6e-189">Add the `uglify` task using the code below.</span></span>

    <span data-ttu-id="24d6e-190">Задача уменьшает *combined.js* файл находится в каталоге temp и создает файл результатов в wwwroot/lib после стандартного соглашения об именовании * \<имя файла\>. min.js*.</span><span class="sxs-lookup"><span data-stu-id="24d6e-190">The task minifies the *combined.js* file found in the temp directory and creates the result file in wwwroot/lib following the standard naming convention *\<file name\>.min.js*.</span></span>
    
    ```javascript
    uglify: {
      all: {
        src: ['temp/combined.js'],
        dest: 'wwwroot/lib/combined.min.js'
      }
    },
    ```

11. <span data-ttu-id="24d6e-191">В разделе grunt.loadNpmTasks() вызова, загружающий grunt contrib очистки включать одного вызова для jshint concat и uglify ниже коде.</span><span class="sxs-lookup"><span data-stu-id="24d6e-191">Under the call grunt.loadNpmTasks() that loads grunt-contrib-clean, include the same call for jshint, concat and uglify using the code below.</span></span>
    
    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. <span data-ttu-id="24d6e-192">Сохранить *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="24d6e-192">Save *Gruntfile.js*.</span></span> <span data-ttu-id="24d6e-193">Файл должен выглядеть примерно в приведенном ниже примере.</span><span class="sxs-lookup"><span data-stu-id="24d6e-193">The file should look something like the example below.</span></span>

    ![Пример файла grunt завершения](using-grunt/_static/gruntfile-js-complete.png)

13. <span data-ttu-id="24d6e-195">Обратите внимание, что в списке задач Runner Explorer задач включает в себя `clean`, `concat`, `jshint` и `uglify` задачи.</span><span class="sxs-lookup"><span data-stu-id="24d6e-195">Notice that the Task Runner Explorer Tasks list includes `clean`, `concat`, `jshint` and `uglify` tasks.</span></span> <span data-ttu-id="24d6e-196">Выполнение каждой задачи в порядке и просмотрите результаты в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="24d6e-196">Run each task in order and observe the results in Solution Explorer.</span></span> <span data-ttu-id="24d6e-197">Каждая задача должна выполняться без ошибок.</span><span class="sxs-lookup"><span data-stu-id="24d6e-197">Each task should run without errors.</span></span>
    
    ![Диспетчер выполнения задач выполнения каждой задачи](using-grunt/_static/task-runner-explorer-run-each-task.png)
    
    <span data-ttu-id="24d6e-199">Создает новую задачу concat *combined.js* файл и помещает его в каталоге temp.</span><span class="sxs-lookup"><span data-stu-id="24d6e-199">The concat task creates a new *combined.js* file and places it into the temp directory.</span></span> <span data-ttu-id="24d6e-200">Задача jshint просто работает и не создает выходных данных.</span><span class="sxs-lookup"><span data-stu-id="24d6e-200">The jshint task simply runs and doesn’t produce output.</span></span> <span data-ttu-id="24d6e-201">Создает новую задачу uglify *combined.min.js* файл и помещает его в wwwroot/lib.</span><span class="sxs-lookup"><span data-stu-id="24d6e-201">The uglify task creates a new *combined.min.js* file and places it into wwwroot/lib.</span></span> <span data-ttu-id="24d6e-202">По завершении решение должно выглядеть примерно как на снимке экрана ниже:</span><span class="sxs-lookup"><span data-stu-id="24d6e-202">On completion, the solution should look something like the screenshot below:</span></span>
    
    ![После выполнения всех задач в обозревателе решений](using-grunt/_static/solution-explorer-after-all-tasks.png)
    
    > [!NOTE]
    > <span data-ttu-id="24d6e-204">Дополнительные сведения о параметрах для каждого пакета [https://www.npmjs.com/](https://www.npmjs.com/) и поиск имени пакета, в поле поиска на главной странице.</span><span class="sxs-lookup"><span data-stu-id="24d6e-204">For more information on the options for each package, visit [https://www.npmjs.com/](https://www.npmjs.com/) and lookup the package name in the search box on the main page.</span></span> <span data-ttu-id="24d6e-205">Например можно найти пакет grunt contrib очистки для получения документации ссылку, которая объясняет все его параметры.</span><span class="sxs-lookup"><span data-stu-id="24d6e-205">For example, you can look up the grunt-contrib-clean package to get a documentation link that explains all of its parameters.</span></span>

### <a name="all-together-now"></a><span data-ttu-id="24d6e-206">Теперь все вместе</span><span class="sxs-lookup"><span data-stu-id="24d6e-206">All together now</span></span>

<span data-ttu-id="24d6e-207">Используйте Grunt `registerTask()` метода для выполнения ряда задач в определенной последовательности.</span><span class="sxs-lookup"><span data-stu-id="24d6e-207">Use the Grunt `registerTask()` method to run a series of tasks in a particular sequence.</span></span> <span data-ttu-id="24d6e-208">Например, для запуска этого примера, приведенные выше действия в том порядке, очистить "->" concat "->" jshint -> uglify, добавьте приведенный ниже код в модуль.</span><span class="sxs-lookup"><span data-stu-id="24d6e-208">For example, to run the example steps above in the order clean -> concat -> jshint -> uglify, add the code below to the module.</span></span> <span data-ttu-id="24d6e-209">Код должны быть добавлены как вызовы loadNpmTasks(), за пределами initConfig того же уровня.</span><span class="sxs-lookup"><span data-stu-id="24d6e-209">The code should be added to the same level as the loadNpmTasks() calls, outside initConfig.</span></span>

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

<span data-ttu-id="24d6e-210">Новая задача отображается в диспетчер выполнения задач в группе задач псевдоним.</span><span class="sxs-lookup"><span data-stu-id="24d6e-210">The new task shows up in Task Runner Explorer under Alias Tasks.</span></span> <span data-ttu-id="24d6e-211">Правой кнопкой мыши и запустите его, так же, как и другие задачи.</span><span class="sxs-lookup"><span data-stu-id="24d6e-211">You can right-click and run it just as you would other tasks.</span></span> <span data-ttu-id="24d6e-212">`all` Задача будет выполняться `clean`, `concat`, `jshint` и `uglify`, в порядке.</span><span class="sxs-lookup"><span data-stu-id="24d6e-212">The `all` task will run `clean`, `concat`, `jshint` and `uglify`, in order.</span></span>

![псевдоним grunt задачи](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a><span data-ttu-id="24d6e-214">Контроль изменений</span><span class="sxs-lookup"><span data-stu-id="24d6e-214">Watching for changes</span></span>

<span data-ttu-id="24d6e-215">Объект `watch` задач следят файлов и каталогов.</span><span class="sxs-lookup"><span data-stu-id="24d6e-215">A `watch` task keeps an eye on files and directories.</span></span> <span data-ttu-id="24d6e-216">Контрольное значение автоматически запускает задачи при обнаружении изменений.</span><span class="sxs-lookup"><span data-stu-id="24d6e-216">The watch triggers tasks automatically if it detects changes.</span></span> <span data-ttu-id="24d6e-217">Добавьте приведенный ниже код для initConfig для отслеживания изменений для \*JS-файлов в каталоге TypeScript.</span><span class="sxs-lookup"><span data-stu-id="24d6e-217">Add the code below to initConfig to watch for changes to \*.js files in the TypeScript directory.</span></span> <span data-ttu-id="24d6e-218">При изменении файла JavaScript `watch` будет выполняться `all` задачи.</span><span class="sxs-lookup"><span data-stu-id="24d6e-218">If a JavaScript file is changed, `watch` will run the `all` task.</span></span>

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

<span data-ttu-id="24d6e-219">Добавьте вызов `loadNpmTasks()` для отображения `watch` задач в Task Runner Explorer.</span><span class="sxs-lookup"><span data-stu-id="24d6e-219">Add a call to `loadNpmTasks()` to show the `watch` task in Task Runner Explorer.</span></span>

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

<span data-ttu-id="24d6e-220">Щелкните правой кнопкой мыши задачу «Контрольные значения» в Task Runner Explorer и выберите в контекстном меню запуска.</span><span class="sxs-lookup"><span data-stu-id="24d6e-220">Right-click the watch task in Task Runner Explorer and select Run from the context menu.</span></span> <span data-ttu-id="24d6e-221">Командное окно, показывающий выполняемую задачу Контрольные значения будет отображаться «ожидание...»</span><span class="sxs-lookup"><span data-stu-id="24d6e-221">The command window that shows the watch task running will display a "Waiting…"</span></span> <span data-ttu-id="24d6e-222">!".</span><span class="sxs-lookup"><span data-stu-id="24d6e-222">message.</span></span> <span data-ttu-id="24d6e-223">Откройте один из файлов TypeScript, добавьте пробел и затем сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="24d6e-223">Open one of the TypeScript files, add a space, and then save the file.</span></span> <span data-ttu-id="24d6e-224">Это активация задачи контрольного значения и запускает другие задачи для выполнения в порядке.</span><span class="sxs-lookup"><span data-stu-id="24d6e-224">This will trigger the watch task and trigger the other tasks to run in order.</span></span> <span data-ttu-id="24d6e-225">На следующем снимке экрана показано выполнения образца.</span><span class="sxs-lookup"><span data-stu-id="24d6e-225">The screenshot below shows a sample run.</span></span>

![под управлением выходных данных задачи](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a><span data-ttu-id="24d6e-227">Привязка к событиям в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="24d6e-227">Binding to Visual Studio events</span></span>

<span data-ttu-id="24d6e-228">Если вы хотите запустить задач вручную каждый раз при работе в Visual Studio, можно привязать задач **перед построить**, **после построения**, **Очистить**, и ** Откройте проект** события.</span><span class="sxs-lookup"><span data-stu-id="24d6e-228">Unless you want to manually start your tasks every time you work in Visual Studio, you can bind tasks to **Before Build**, **After Build**, **Clean**, and **Project Open** events.</span></span>

<span data-ttu-id="24d6e-229">Следует привязать `watch` , чтобы он запускался при каждом открытии Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="24d6e-229">Let’s bind `watch` so that it runs every time Visual Studio opens.</span></span> <span data-ttu-id="24d6e-230">В Task Runner Explorer, щелкните правой кнопкой мыши задачи «Контрольные значения» и выберите **привязки > Открыть проект** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="24d6e-230">In Task Runner Explorer, right-click the watch task and select **Bindings > Project Open** from the context menu.</span></span>

![Привязка задачи к Открытие проекта](using-grunt/_static/bindings-project-open.png)

<span data-ttu-id="24d6e-232">Выгрузите и перезагрузите проект.</span><span class="sxs-lookup"><span data-stu-id="24d6e-232">Unload and reload the project.</span></span> <span data-ttu-id="24d6e-233">При загрузке снова проекта, Контрольные значения начала задачи выполняются автоматически.</span><span class="sxs-lookup"><span data-stu-id="24d6e-233">When the project loads again, the watch task will start running automatically.</span></span>

## <a name="summary"></a><span data-ttu-id="24d6e-234">Сводка</span><span class="sxs-lookup"><span data-stu-id="24d6e-234">Summary</span></span>

<span data-ttu-id="24d6e-235">Grunt является runner мощные задачи, который может использоваться для большинства задач сборку клиента автоматизации.</span><span class="sxs-lookup"><span data-stu-id="24d6e-235">Grunt is a powerful task runner that can be used to automate most client-build tasks.</span></span> <span data-ttu-id="24d6e-236">Grunt использует NPM для доставки пакетов и компонентов, интеграция с Visual Studio для работы с проектами.</span><span class="sxs-lookup"><span data-stu-id="24d6e-236">Grunt leverages NPM to deliver its packages, and features tooling integration with Visual Studio.</span></span> <span data-ttu-id="24d6e-237">Visual Studio Task Runner Explorer обнаруживает изменения в файлах конфигурации и обеспечивает удобный интерфейс для выполнения задач, просмотр запущенных задач и привязывание задач к событиям в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="24d6e-237">Visual Studio's Task Runner Explorer detects changes to configuration files and provides a convenient interface to run tasks, view running tasks, and bind tasks to Visual Studio events.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="24d6e-238">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="24d6e-238">Additional resources</span></span>

   * [<span data-ttu-id="24d6e-239">Использование Gulp</span><span class="sxs-lookup"><span data-stu-id="24d6e-239">Using Gulp</span></span>](using-gulp.md)
