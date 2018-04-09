---
title: Использовать Grunt в ASP.NET Core
author: rick-anderson
description: ''
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-grunt
ms.openlocfilehash: 169552e9b5dd811884ce1c65952677ba83626b58
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="use-grunt-in-aspnet-core"></a><span data-ttu-id="b57c9-102">Использовать Grunt в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b57c9-102">Use Grunt in ASP.NET Core</span></span>

<span data-ttu-id="b57c9-103">По [Ноэл Риса](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span><span class="sxs-lookup"><span data-stu-id="b57c9-103">By [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span></span>

<span data-ttu-id="b57c9-104">Grunt-это средство запуска задач JavaScript, который автоматизирует минификации сценария, компиляцию TypeScript, средства «корпус» качество кода, предварительной процессоров CSS и практически любой повторяющихся тяжелой задачей, должен образом для поддержки разработки клиентских.</span><span class="sxs-lookup"><span data-stu-id="b57c9-104">Grunt is a JavaScript task runner that automates script minification, TypeScript compilation, code quality "lint" tools, CSS pre-processors, and just about any repetitive chore that needs doing to support client development.</span></span> <span data-ttu-id="b57c9-105">Grunt полностью поддерживается в Visual Studio на то, что шаблоны проектов ASP.NET по умолчанию используется Gulp (см. [использовать Gulp](using-gulp.md)).</span><span class="sxs-lookup"><span data-stu-id="b57c9-105">Grunt is fully supported in Visual Studio, though the ASP.NET project templates use Gulp by default (see [Use Gulp](using-gulp.md)).</span></span>

<span data-ttu-id="b57c9-106">В этом примере использует пустой проект ASP.NET Core начальной точкой, чтобы показать, как автоматизировать процесс построения клиента с нуля.</span><span class="sxs-lookup"><span data-stu-id="b57c9-106">This example uses an empty ASP.NET Core project as its starting point, to show how to automate the client build process from scratch.</span></span>

<span data-ttu-id="b57c9-107">Готовый пример удаляет целевой каталог развертывания, объединяет файлы JavaScript, проверяет качество кода, также сжимает содержимое файла JavaScript и развертывает в корневой каталог веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="b57c9-107">The finished example cleans the target deployment directory, combines JavaScript files, checks code quality, condenses JavaScript file content and deploys to the root of your web application.</span></span> <span data-ttu-id="b57c9-108">Мы будем использовать следующие пакеты:</span><span class="sxs-lookup"><span data-stu-id="b57c9-108">We will use the following packages:</span></span>

* <span data-ttu-id="b57c9-109">**grunt**: средство запуска задач Grunt пакет.</span><span class="sxs-lookup"><span data-stu-id="b57c9-109">**grunt**: The Grunt task runner package.</span></span>

* <span data-ttu-id="b57c9-110">**Очистка grunt contrib**: подключаемый модуль, который удаляет файлы или каталоги.</span><span class="sxs-lookup"><span data-stu-id="b57c9-110">**grunt-contrib-clean**: A plugin that removes files or directories.</span></span>

* <span data-ttu-id="b57c9-111">**grunt-contrib-jshint**: подключаемый модуль, который просматривает качества кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b57c9-111">**grunt-contrib-jshint**: A plugin that reviews JavaScript code quality.</span></span>

* <span data-ttu-id="b57c9-112">**grunt-contrib-concat**: подключаемый модуль, который объединяет файлы в один файл.</span><span class="sxs-lookup"><span data-stu-id="b57c9-112">**grunt-contrib-concat**: A plugin that joins files into a single file.</span></span>

* <span data-ttu-id="b57c9-113">**grunt contrib uglify**: подключаемый модуль, который уменьшает JavaScript для уменьшения размера.</span><span class="sxs-lookup"><span data-stu-id="b57c9-113">**grunt-contrib-uglify**: A plugin that minifies JavaScript to reduce size.</span></span>

* <span data-ttu-id="b57c9-114">**grunt contrib Контрольные значения**: подключаемый модуль, который отслеживает действия файла.</span><span class="sxs-lookup"><span data-stu-id="b57c9-114">**grunt-contrib-watch**: A plugin that watches file activity.</span></span>

## <a name="preparing-the-application"></a><span data-ttu-id="b57c9-115">Подготовка приложения</span><span class="sxs-lookup"><span data-stu-id="b57c9-115">Preparing the application</span></span>

<span data-ttu-id="b57c9-116">Чтобы начать, задать новый пустой веб-приложения и добавьте файлы пример TypeScript.</span><span class="sxs-lookup"><span data-stu-id="b57c9-116">To begin, set up a new empty web application and add TypeScript example files.</span></span> <span data-ttu-id="b57c9-117">Файлы typeScript автоматически компилируется в JavaScript с использованием параметров по умолчанию Visual Studio и будут нашей сырье для обработки с помощью Grunt.</span><span class="sxs-lookup"><span data-stu-id="b57c9-117">TypeScript files are automatically compiled into JavaScript using default Visual Studio settings and will be our raw material to process using Grunt.</span></span>

1.  <span data-ttu-id="b57c9-118">В Visual Studio создайте новый `ASP.NET Web Application`.</span><span class="sxs-lookup"><span data-stu-id="b57c9-118">In Visual Studio, create a new `ASP.NET Web Application`.</span></span>

2.  <span data-ttu-id="b57c9-119">В **новый проект ASP.NET** диалоговое окно, выберите ASP.NET Core **пустой** шаблона и нажмите кнопку "ОК".</span><span class="sxs-lookup"><span data-stu-id="b57c9-119">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template and click the OK button.</span></span>

3.  <span data-ttu-id="b57c9-120">Проверьте структура проекта в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="b57c9-120">In the Solution Explorer, review the project structure.</span></span> <span data-ttu-id="b57c9-121">`\src` Папка содержит пустой `wwwroot` и `Dependencies` узлов.</span><span class="sxs-lookup"><span data-stu-id="b57c9-121">The `\src` folder includes empty `wwwroot` and `Dependencies` nodes.</span></span>

    ![пустой веб-решений](using-grunt/_static/grunt-solution-explorer.png)

4.  <span data-ttu-id="b57c9-123">Добавить новую папку с именем `TypeScript` в каталог проекта.</span><span class="sxs-lookup"><span data-stu-id="b57c9-123">Add a new folder named `TypeScript` to your project directory.</span></span>

5.  <span data-ttu-id="b57c9-124">Перед добавлением все файлы, убедитесь, что Visual Studio имеет параметр "компиляции при сохранении" для возвращенных файлов TypeScript.</span><span class="sxs-lookup"><span data-stu-id="b57c9-124">Before adding any files, make sure that Visual Studio has the option 'compile on save' for TypeScript files checked.</span></span> <span data-ttu-id="b57c9-125">Перейдите к **средства** > **параметры** > **текстовый редактор** > **Typescript**  >  **Проекта**:</span><span class="sxs-lookup"><span data-stu-id="b57c9-125">Navigate to **Tools** > **Options** > **Text Editor** > **Typescript** > **Project**:</span></span>

    ![Параметры автоматического compliation файлов TypeScript](using-grunt/_static/typescript-options.png)

6.  <span data-ttu-id="b57c9-127">Щелкните правой кнопкой мыши `TypeScript` каталог и выберите **Добавить > новый элемент** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="b57c9-127">Right-click the `TypeScript` directory and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="b57c9-128">Выберите **файл JavaScript** элемента и присвойте файлу имя *Tastes.ts* (Примечание \*.ts расширения).</span><span class="sxs-lookup"><span data-stu-id="b57c9-128">Select the **JavaScript file** item and name the file *Tastes.ts* (note the \*.ts extension).</span></span> <span data-ttu-id="b57c9-129">Скопируйте строку кода TypeScript под в файле (при сохранении, новый *Tastes.js* появится файл с исходного кода JavaScript).</span><span class="sxs-lookup"><span data-stu-id="b57c9-129">Copy the line of TypeScript code below into the file (when you save, a new *Tastes.js* file will appear with the JavaScript source).</span></span>
    
    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7.  <span data-ttu-id="b57c9-130">Добавьте второй файл, чтобы **TypeScript** каталог и назовите его `Food.ts`.</span><span class="sxs-lookup"><span data-stu-id="b57c9-130">Add a second file to the **TypeScript** directory and name it `Food.ts`.</span></span> <span data-ttu-id="b57c9-131">Скопируйте приведенный ниже код в файл.</span><span class="sxs-lookup"><span data-stu-id="b57c9-131">Copy the code below into the file.</span></span>

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

## <a name="configuring-npm"></a><span data-ttu-id="b57c9-132">Настройка NPM</span><span class="sxs-lookup"><span data-stu-id="b57c9-132">Configuring NPM</span></span>

<span data-ttu-id="b57c9-133">Настройте NPM для загрузки grunt и grunt задачи.</span><span class="sxs-lookup"><span data-stu-id="b57c9-133">Next, configure NPM to download grunt and grunt-tasks.</span></span>

1. <span data-ttu-id="b57c9-134">В обозревателе решений щелкните правой кнопкой мыши проект и выберите **Добавить > новый элемент** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="b57c9-134">In the Solution Explorer, right-click the project and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="b57c9-135">Выберите **файл конфигурации NPM** товара, оставьте имя по умолчанию *package.json*и нажмите кнопку **добавить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="b57c9-135">Select the **NPM configuration file** item, leave the default name, *package.json*, and click the **Add** button.</span></span>

2. <span data-ttu-id="b57c9-136">В *package.json* файла внутри `devDependencies` объекта фигурные скобки, введите «grunt».</span><span class="sxs-lookup"><span data-stu-id="b57c9-136">In the *package.json* file, inside the `devDependencies` object braces, enter "grunt".</span></span> <span data-ttu-id="b57c9-137">Выберите `grunt` из Intellisense списка и нажмите клавишу ВВОД.</span><span class="sxs-lookup"><span data-stu-id="b57c9-137">Select `grunt` from the Intellisense list and press the Enter key.</span></span> <span data-ttu-id="b57c9-138">Visual Studio Квота grunt имя пакета и добавьте двоеточие.</span><span class="sxs-lookup"><span data-stu-id="b57c9-138">Visual Studio will quote the grunt package name, and add a colon.</span></span> <span data-ttu-id="b57c9-139">Справа от запятой выберите последняя стабильная версия пакета в верхней части списка Intellisense (нажмите клавишу `Ctrl-Space` Если Intellisense не отображается).</span><span class="sxs-lookup"><span data-stu-id="b57c9-139">To the right of the colon, select the latest stable version of the package from the top of the Intellisense list (press `Ctrl-Space` if Intellisense doesn't appear).</span></span>

    ![grun Intellisense](using-grunt/_static/devdependencies-grunt.png)
    
    > [!NOTE]
    > <span data-ttu-id="b57c9-141">Использует NPM [семантического управления версиями](http://semver.org/) организовать зависимостей.</span><span class="sxs-lookup"><span data-stu-id="b57c9-141">NPM uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="b57c9-142">Семантического управления версиями, также известный как SemVer определяет пакеты с формирование <major>.<minor>. <patch>. IntelliSense упрощает семантического управления версиями, отображая только несколько распространенных вариантов.</span><span class="sxs-lookup"><span data-stu-id="b57c9-142">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme <major>.<minor>.<patch>. Intellisense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="b57c9-143">Верхний элемент в списке Intellisense (0.4.5 в приведенном выше примере) считается последняя стабильная версия пакета.</span><span class="sxs-lookup"><span data-stu-id="b57c9-143">The top item in the Intellisense list (0.4.5 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="b57c9-144">Знак вставки (^) соответствует самой последней основной номер версии, а тильда (~) соответствует последнему дополнительному номеру версии.</span><span class="sxs-lookup"><span data-stu-id="b57c9-144">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span> <span data-ttu-id="b57c9-145">В разделе [NPM semver версия средства синтаксического анализа ссылки](https://www.npmjs.com/package/semver) в соответствии с полной функциональность, предоставляющий SemVer.</span><span class="sxs-lookup"><span data-stu-id="b57c9-145">See the [NPM semver version parser reference](https://www.npmjs.com/package/semver) as a guide to the full expressivity that SemVer provides.</span></span>

3. <span data-ttu-id="b57c9-146">Добавьте дополнительные зависимости, для загрузки grunt-contrib -\* пакетов для *чистой*, *jshint*, *concat*, *uglify*и *Контрольные значения* как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="b57c9-146">Add more dependencies to load grunt-contrib-\* packages for *clean*, *jshint*, *concat*, *uglify*, and *watch* as shown in the example below.</span></span> <span data-ttu-id="b57c9-147">Версии не требуется в соответствии с примером.</span><span class="sxs-lookup"><span data-stu-id="b57c9-147">The versions don't need to match the example.</span></span>

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

4. <span data-ttu-id="b57c9-148">Сохранить *package.json* файла.</span><span class="sxs-lookup"><span data-stu-id="b57c9-148">Save the *package.json* file.</span></span>

<span data-ttu-id="b57c9-149">Пакеты для каждого элемента devDependencies будут загружены и все файлы, необходимые для каждого пакета.</span><span class="sxs-lookup"><span data-stu-id="b57c9-149">The packages for each devDependencies item will download, along with any files that each package requires.</span></span> <span data-ttu-id="b57c9-150">Можно найти файлы пакета в `node_modules` каталога, позволяя **Показать все файлы** кнопку в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="b57c9-150">You can find the package files in the `node_modules` directory by enabling the **Show All Files** button in the Solution Explorer.</span></span>

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> <span data-ttu-id="b57c9-152">Если необходимо, можно восстановить вручную, зависимости в обозревателе решений щелкните правой кнопкой мыши на `Dependencies\NPM` и выбрав **восстановить пакеты** пункт меню.</span><span class="sxs-lookup"><span data-stu-id="b57c9-152">If you need to, you can manually restore dependencies in Solution Explorer by right-clicking on `Dependencies\NPM` and selecting the **Restore Packages** menu option.</span></span>

![Восстановление пакетов](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a><span data-ttu-id="b57c9-154">Настройка Grunt</span><span class="sxs-lookup"><span data-stu-id="b57c9-154">Configuring Grunt</span></span>

<span data-ttu-id="b57c9-155">Grunt настраивается с помощью манифест с именем *Gruntfile.js* , определяет, загружает и регистрирует задачи, которые можно запускать вручную или с запуском автоматически на основе событий в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b57c9-155">Grunt is configured using a manifest named *Gruntfile.js* that defines, loads and registers tasks that can be run manually or configured to run automatically based on events in Visual Studio.</span></span>

1. <span data-ttu-id="b57c9-156">Щелкните правой кнопкой мыши проект и выберите **Добавить > новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="b57c9-156">Right-click the project and select **Add > New Item**.</span></span> <span data-ttu-id="b57c9-157">Выберите **файла конфигурации Grunt** , то оставьте имя по умолчанию *Gruntfile.js*и нажмите кнопку **добавить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="b57c9-157">Select the **Grunt Configuration file** option, leave the default name, *Gruntfile.js*, and click the **Add** button.</span></span>

   <span data-ttu-id="b57c9-158">Исходный код включает определение модуля и `grunt.initConfig()` метод.</span><span class="sxs-lookup"><span data-stu-id="b57c9-158">The initial code includes a module definition and the `grunt.initConfig()` method.</span></span> <span data-ttu-id="b57c9-159">`initConfig()` Используется для задания параметров для каждого пакета и остальная часть модуль будет загрузить и зарегистрировать задачи.</span><span class="sxs-lookup"><span data-stu-id="b57c9-159">The `initConfig()` is used to set options for each package, and the remainder of the module will load and register tasks.</span></span>
    
   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

2. <span data-ttu-id="b57c9-160">Внутри `initConfig()` метод, добавьте параметры для `clean` задач, как показано в примере *Gruntfile.js* ниже.</span><span class="sxs-lookup"><span data-stu-id="b57c9-160">Inside the `initConfig()` method, add options for the `clean` task as shown in the example *Gruntfile.js* below.</span></span> <span data-ttu-id="b57c9-161">Задачу "Очистить" принимает массив строк каталогов.</span><span class="sxs-lookup"><span data-stu-id="b57c9-161">The clean task accepts an array of directory strings.</span></span> <span data-ttu-id="b57c9-162">Эта задача удаляет файлы из wwwroot/lib и удаляет весь/временный каталог.</span><span class="sxs-lookup"><span data-stu-id="b57c9-162">This task removes files from wwwroot/lib and removes the entire /temp directory.</span></span>

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. <span data-ttu-id="b57c9-163">Под методом initConfig(), добавьте вызов `grunt.loadNpmTasks()`.</span><span class="sxs-lookup"><span data-stu-id="b57c9-163">Below the initConfig() method, add a call to `grunt.loadNpmTasks()`.</span></span> <span data-ttu-id="b57c9-164">Это сделает задачи запускаемых из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b57c9-164">This will make the task runnable from Visual Studio.</span></span>

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. <span data-ttu-id="b57c9-165">Сохранить *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="b57c9-165">Save *Gruntfile.js*.</span></span> <span data-ttu-id="b57c9-166">Файл должен выглядеть примерно на снимке экрана ниже.</span><span class="sxs-lookup"><span data-stu-id="b57c9-166">The file should look something like the screenshot below.</span></span>

    ![Начальное gruntfile](using-grunt/_static/gruntfile-js-initial.png)

5. <span data-ttu-id="b57c9-168">Щелкните правой кнопкой мыши *Gruntfile.js* и выберите **Task Runner Explorer** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="b57c9-168">Right-click *Gruntfile.js* and select **Task Runner Explorer** from the context menu.</span></span> <span data-ttu-id="b57c9-169">Откроется окно Task Runner Explorer.</span><span class="sxs-lookup"><span data-stu-id="b57c9-169">The Task Runner Explorer window will open.</span></span>

    ![меню обозревателя средство запуска задач](using-grunt/_static/task-runner-explorer-menu.png)

6. <span data-ttu-id="b57c9-171">Убедитесь, что `clean` области отображается **задачи** в Task Runner Explorer.</span><span class="sxs-lookup"><span data-stu-id="b57c9-171">Verify that `clean` shows under **Tasks** in the Task Runner Explorer.</span></span>

    ![Список задач задач runner explorer](using-grunt/_static/task-runner-explorer-tasks.png)

7. <span data-ttu-id="b57c9-173">Щелкните правой кнопкой мыши задачу "Очистить" и выберите **запуска** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="b57c9-173">Right-click the clean task and select **Run** from the context menu.</span></span> <span data-ttu-id="b57c9-174">Окно командной строки отображает ход выполнения задачи.</span><span class="sxs-lookup"><span data-stu-id="b57c9-174">A command window displays progress of the task.</span></span>

    ![Задача runner explorer запустите задачу "Очистить"](using-grunt/_static/task-runner-explorer-run-clean.png)
    
    > [!NOTE]
    > <span data-ttu-id="b57c9-176">Отсутствуют файлы или каталоги для очистки еще.</span><span class="sxs-lookup"><span data-stu-id="b57c9-176">There are no files or directories to clean yet.</span></span> <span data-ttu-id="b57c9-177">При желании можно создать их вручную в обозревателе решений и запустите задачу "Очистить" в качестве теста.</span><span class="sxs-lookup"><span data-stu-id="b57c9-177">If you like, you can manually create them in the Solution Explorer and then run the clean task as a test.</span></span>
    
8. <span data-ttu-id="b57c9-178">В этом методе initConfig(), добавьте запись для `concat` ниже коде.</span><span class="sxs-lookup"><span data-stu-id="b57c9-178">In the initConfig() method, add an entry for `concat` using the code below.</span></span>

    <span data-ttu-id="b57c9-179">`src` Массива свойства перечислены файлы для объединения, в том порядке, они должны быть объединены.</span><span class="sxs-lookup"><span data-stu-id="b57c9-179">The `src` property array lists files to combine, in the order that they should be combined.</span></span> <span data-ttu-id="b57c9-180">`dest` Свойство путь присваивается объединенного файла, который создается.</span><span class="sxs-lookup"><span data-stu-id="b57c9-180">The `dest` property assigns the path to the combined file that's produced.</span></span>

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```
    
    > [!NOTE]
    > <span data-ttu-id="b57c9-181">`all` Свойство в приведенном выше коде это имя целевого объекта.</span><span class="sxs-lookup"><span data-stu-id="b57c9-181">The `all` property in the code above is the name of a target.</span></span> <span data-ttu-id="b57c9-182">Целевые объекты используются в некоторых задачах Grunt для разрешения нескольких средах построения.</span><span class="sxs-lookup"><span data-stu-id="b57c9-182">Targets are used in some Grunt tasks to allow multiple build environments.</span></span> <span data-ttu-id="b57c9-183">Позволяет просмотреть встроенных конечных объектов, с помощью Intellisense или назначить собственные.</span><span class="sxs-lookup"><span data-stu-id="b57c9-183">You can view the built-in targets using Intellisense or assign your own.</span></span>
    
9. <span data-ttu-id="b57c9-184">Добавить `jshint` задачи, используя приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="b57c9-184">Add the `jshint` task using the code below.</span></span>

    <span data-ttu-id="b57c9-185">Программа jshint качества кода выполняется для каждого файла JavaScript, найденные в каталоге temp.</span><span class="sxs-lookup"><span data-stu-id="b57c9-185">The jshint code-quality utility is run against every JavaScript file found in the temp directory.</span></span>
    
    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="b57c9-186">Параметр «-W069» ошибка произведенное jshint при JavaScript используется квадратная скобка синтаксис, т. е. присвоение свойства вместо точечной нотации `Tastes["Sweet"]` вместо `Tastes.Sweet`.</span><span class="sxs-lookup"><span data-stu-id="b57c9-186">The option "-W069" is an error produced by jshint when JavaScript uses bracket syntax to assign a property instead of dot notation, i.e. `Tastes["Sweet"]` instead of `Tastes.Sweet`.</span></span> <span data-ttu-id="b57c9-187">Этот параметр отключает предупреждения, чтобы разрешить оставшаяся часть процедуры, чтобы продолжить.</span><span class="sxs-lookup"><span data-stu-id="b57c9-187">The option turns off the warning to allow the rest of the process to continue.</span></span>

10. <span data-ttu-id="b57c9-188">Добавить `uglify` задачи, используя приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="b57c9-188">Add the `uglify` task using the code below.</span></span>

    <span data-ttu-id="b57c9-189">Задача уменьшает *combined.js* файл находится в каталоге temp и создает файл результатов в wwwroot/lib после стандартного соглашения об именовании  *\<имя файла\>. min.js*.</span><span class="sxs-lookup"><span data-stu-id="b57c9-189">The task minifies the *combined.js* file found in the temp directory and creates the result file in wwwroot/lib following the standard naming convention *\<file name\>.min.js*.</span></span>
    
    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

11. <span data-ttu-id="b57c9-190">В разделе grunt.loadNpmTasks() вызова, загружающий grunt contrib очистки включать одного вызова для jshint concat и uglify ниже коде.</span><span class="sxs-lookup"><span data-stu-id="b57c9-190">Under the call grunt.loadNpmTasks() that loads grunt-contrib-clean, include the same call for jshint, concat and uglify using the code below.</span></span>
    
    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. <span data-ttu-id="b57c9-191">Сохранить *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="b57c9-191">Save *Gruntfile.js*.</span></span> <span data-ttu-id="b57c9-192">Файл должен выглядеть примерно в приведенном ниже примере.</span><span class="sxs-lookup"><span data-stu-id="b57c9-192">The file should look something like the example below.</span></span>

    ![Пример файла grunt завершения](using-grunt/_static/gruntfile-js-complete.png)

13. <span data-ttu-id="b57c9-194">Обратите внимание, что в списке задач Runner Explorer задач включает в себя `clean`, `concat`, `jshint` и `uglify` задачи.</span><span class="sxs-lookup"><span data-stu-id="b57c9-194">Notice that the Task Runner Explorer Tasks list includes `clean`, `concat`, `jshint` and `uglify` tasks.</span></span> <span data-ttu-id="b57c9-195">Выполнение каждой задачи в порядке и просмотрите результаты в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="b57c9-195">Run each task in order and observe the results in Solution Explorer.</span></span> <span data-ttu-id="b57c9-196">Каждая задача должна выполняться без ошибок.</span><span class="sxs-lookup"><span data-stu-id="b57c9-196">Each task should run without errors.</span></span>
    
    ![Диспетчер выполнения задач выполнения каждой задачи](using-grunt/_static/task-runner-explorer-run-each-task.png)
    
    <span data-ttu-id="b57c9-198">Создает новую задачу concat *combined.js* файл и помещает его в каталоге temp.</span><span class="sxs-lookup"><span data-stu-id="b57c9-198">The concat task creates a new *combined.js* file and places it into the temp directory.</span></span> <span data-ttu-id="b57c9-199">Задача jshint просто работает и не создает выходных данных.</span><span class="sxs-lookup"><span data-stu-id="b57c9-199">The jshint task simply runs and doesn't produce output.</span></span> <span data-ttu-id="b57c9-200">Создает новую задачу uglify *combined.min.js* файл и помещает его в wwwroot/lib.</span><span class="sxs-lookup"><span data-stu-id="b57c9-200">The uglify task creates a new *combined.min.js* file and places it into wwwroot/lib.</span></span> <span data-ttu-id="b57c9-201">По завершении решение должно выглядеть примерно как на снимке экрана ниже:</span><span class="sxs-lookup"><span data-stu-id="b57c9-201">On completion, the solution should look something like the screenshot below:</span></span>
    
    ![После выполнения всех задач в обозревателе решений](using-grunt/_static/solution-explorer-after-all-tasks.png)
    
    > [!NOTE]
    > <span data-ttu-id="b57c9-203">Дополнительные сведения о параметрах для каждого пакета [ https://www.npmjs.com/ ](https://www.npmjs.com/) и поиск имени пакета, в поле поиска на главной странице.</span><span class="sxs-lookup"><span data-stu-id="b57c9-203">For more information on the options for each package, visit [https://www.npmjs.com/](https://www.npmjs.com/) and lookup the package name in the search box on the main page.</span></span> <span data-ttu-id="b57c9-204">Например можно найти пакет grunt contrib очистки для получения документации ссылку, которая объясняет все его параметры.</span><span class="sxs-lookup"><span data-stu-id="b57c9-204">For example, you can look up the grunt-contrib-clean package to get a documentation link that explains all of its parameters.</span></span>

### <a name="all-together-now"></a><span data-ttu-id="b57c9-205">Теперь все вместе</span><span class="sxs-lookup"><span data-stu-id="b57c9-205">All together now</span></span>

<span data-ttu-id="b57c9-206">Используйте Grunt `registerTask()` метода для выполнения ряда задач в определенной последовательности.</span><span class="sxs-lookup"><span data-stu-id="b57c9-206">Use the Grunt `registerTask()` method to run a series of tasks in a particular sequence.</span></span> <span data-ttu-id="b57c9-207">Например, для запуска этого примера, приведенные выше действия в том порядке, очистить "->" concat "->" jshint -> uglify, добавьте приведенный ниже код в модуль.</span><span class="sxs-lookup"><span data-stu-id="b57c9-207">For example, to run the example steps above in the order clean -> concat -> jshint -> uglify, add the code below to the module.</span></span> <span data-ttu-id="b57c9-208">Код должны быть добавлены как вызовы loadNpmTasks(), за пределами initConfig того же уровня.</span><span class="sxs-lookup"><span data-stu-id="b57c9-208">The code should be added to the same level as the loadNpmTasks() calls, outside initConfig.</span></span>

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

<span data-ttu-id="b57c9-209">Новая задача отображается в диспетчер выполнения задач в группе задач псевдоним.</span><span class="sxs-lookup"><span data-stu-id="b57c9-209">The new task shows up in Task Runner Explorer under Alias Tasks.</span></span> <span data-ttu-id="b57c9-210">Правой кнопкой мыши и запустите его, так же, как и другие задачи.</span><span class="sxs-lookup"><span data-stu-id="b57c9-210">You can right-click and run it just as you would other tasks.</span></span> <span data-ttu-id="b57c9-211">`all` Задача будет выполняться `clean`, `concat`, `jshint` и `uglify`, в порядке.</span><span class="sxs-lookup"><span data-stu-id="b57c9-211">The `all` task will run `clean`, `concat`, `jshint` and `uglify`, in order.</span></span>

![псевдоним grunt задачи](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a><span data-ttu-id="b57c9-213">Просмотр изменений</span><span class="sxs-lookup"><span data-stu-id="b57c9-213">Watching for changes</span></span>

<span data-ttu-id="b57c9-214">Объект `watch` задач следят файлов и каталогов.</span><span class="sxs-lookup"><span data-stu-id="b57c9-214">A `watch` task keeps an eye on files and directories.</span></span> <span data-ttu-id="b57c9-215">Контрольное значение автоматически запускает задачи при обнаружении изменений.</span><span class="sxs-lookup"><span data-stu-id="b57c9-215">The watch triggers tasks automatically if it detects changes.</span></span> <span data-ttu-id="b57c9-216">Добавьте приведенный ниже код для initConfig для отслеживания изменений для \*JS-файлов в каталоге TypeScript.</span><span class="sxs-lookup"><span data-stu-id="b57c9-216">Add the code below to initConfig to watch for changes to \*.js files in the TypeScript directory.</span></span> <span data-ttu-id="b57c9-217">При изменении файла JavaScript `watch` будет выполняться `all` задачи.</span><span class="sxs-lookup"><span data-stu-id="b57c9-217">If a JavaScript file is changed, `watch` will run the `all` task.</span></span>

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

<span data-ttu-id="b57c9-218">Добавьте вызов `loadNpmTasks()` для отображения `watch` задач в Task Runner Explorer.</span><span class="sxs-lookup"><span data-stu-id="b57c9-218">Add a call to `loadNpmTasks()` to show the `watch` task in Task Runner Explorer.</span></span>

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

<span data-ttu-id="b57c9-219">Щелкните правой кнопкой мыши задачу «Контрольные значения» в Task Runner Explorer и выберите в контекстном меню запуска.</span><span class="sxs-lookup"><span data-stu-id="b57c9-219">Right-click the watch task in Task Runner Explorer and select Run from the context menu.</span></span> <span data-ttu-id="b57c9-220">Командное окно, показывающий выполняемую задачу Контрольные значения будет отображаться «ожидание...»</span><span class="sxs-lookup"><span data-stu-id="b57c9-220">The command window that shows the watch task running will display a "Waiting…"</span></span> <span data-ttu-id="b57c9-221">!".</span><span class="sxs-lookup"><span data-stu-id="b57c9-221">message.</span></span> <span data-ttu-id="b57c9-222">Откройте один из файлов TypeScript, добавьте пробел и затем сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="b57c9-222">Open one of the TypeScript files, add a space, and then save the file.</span></span> <span data-ttu-id="b57c9-223">Это активация задачи контрольного значения и запускает другие задачи для выполнения в порядке.</span><span class="sxs-lookup"><span data-stu-id="b57c9-223">This will trigger the watch task and trigger the other tasks to run in order.</span></span> <span data-ttu-id="b57c9-224">На следующем снимке экрана показано выполнения образца.</span><span class="sxs-lookup"><span data-stu-id="b57c9-224">The screenshot below shows a sample run.</span></span>

![под управлением выходных данных задачи](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a><span data-ttu-id="b57c9-226">Привязка к событиям в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b57c9-226">Binding to Visual Studio events</span></span>

<span data-ttu-id="b57c9-227">Если вы хотите запустить задач вручную каждый раз при работе в Visual Studio, можно привязать задач **перед построить**, **после построения**, **Очистить**, и  **Откройте проект** события.</span><span class="sxs-lookup"><span data-stu-id="b57c9-227">Unless you want to manually start your tasks every time you work in Visual Studio, you can bind tasks to **Before Build**, **After Build**, **Clean**, and **Project Open** events.</span></span>

<span data-ttu-id="b57c9-228">Следует привязать `watch` , чтобы он запускался при каждом открытии Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b57c9-228">Let’s bind `watch` so that it runs every time Visual Studio opens.</span></span> <span data-ttu-id="b57c9-229">В Task Runner Explorer, щелкните правой кнопкой мыши задачи «Контрольные значения» и выберите **привязки > Открыть проект** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="b57c9-229">In Task Runner Explorer, right-click the watch task and select **Bindings > Project Open** from the context menu.</span></span>

![Привязка задачи к Открытие проекта](using-grunt/_static/bindings-project-open.png)

<span data-ttu-id="b57c9-231">Выгрузите и перезагрузите проект.</span><span class="sxs-lookup"><span data-stu-id="b57c9-231">Unload and reload the project.</span></span> <span data-ttu-id="b57c9-232">При загрузке снова проекта, Контрольные значения начала задачи выполняются автоматически.</span><span class="sxs-lookup"><span data-stu-id="b57c9-232">When the project loads again, the watch task will start running automatically.</span></span>

## <a name="summary"></a><span data-ttu-id="b57c9-233">Сводка</span><span class="sxs-lookup"><span data-stu-id="b57c9-233">Summary</span></span>

<span data-ttu-id="b57c9-234">Grunt является runner мощные задачи, который может использоваться для большинства задач сборку клиента автоматизации.</span><span class="sxs-lookup"><span data-stu-id="b57c9-234">Grunt is a powerful task runner that can be used to automate most client-build tasks.</span></span> <span data-ttu-id="b57c9-235">Grunt использует NPM для доставки пакетов и компонентов, интеграция с Visual Studio для работы с проектами.</span><span class="sxs-lookup"><span data-stu-id="b57c9-235">Grunt leverages NPM to deliver its packages, and features tooling integration with Visual Studio.</span></span> <span data-ttu-id="b57c9-236">Visual Studio Task Runner Explorer обнаруживает изменения в файлах конфигурации и обеспечивает удобный интерфейс для выполнения задач, просмотр запущенных задач и привязывание задач к событиям в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b57c9-236">Visual Studio's Task Runner Explorer detects changes to configuration files and provides a convenient interface to run tasks, view running tasks, and bind tasks to Visual Studio events.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b57c9-237">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="b57c9-237">Additional resources</span></span>

   * [<span data-ttu-id="b57c9-238">Использование Gulp</span><span class="sxs-lookup"><span data-stu-id="b57c9-238">Use Gulp</span></span>](using-gulp.md)
