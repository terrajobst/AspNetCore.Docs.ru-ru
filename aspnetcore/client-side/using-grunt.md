---
title: Использование Grunt в ASP.NET Core
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/14/2016
uid: client-side/using-grunt
ms.openlocfilehash: fc912974fb6ed3c65bb46a7d616d9e531587d946
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208883"
---
# <a name="use-grunt-in-aspnet-core"></a><span data-ttu-id="7faf4-102">Использование Grunt в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7faf4-102">Use Grunt in ASP.NET Core</span></span>

<span data-ttu-id="7faf4-103">По [Райс Ноэл](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span><span class="sxs-lookup"><span data-stu-id="7faf4-103">By [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span></span>

<span data-ttu-id="7faf4-104">Grunt является запускателя задач JavaScript, автоматизирующего минификации скрипт, компиляции TypeScript, средства «lint» качество кода, предварительной процессоров CSS и практически любые повторяющихся рутинную работу, необходимо, выполнив для поддержки разработки клиентских приложений.</span><span class="sxs-lookup"><span data-stu-id="7faf4-104">Grunt is a JavaScript task runner that automates script minification, TypeScript compilation, code quality "lint" tools, CSS pre-processors, and just about any repetitive chore that needs doing to support client development.</span></span> <span data-ttu-id="7faf4-105">Grunt полностью поддерживается в Visual Studio, то, что шаблоны проектов ASP.NET с помощью средства Gulp по умолчанию (см. в разделе [использование Gulp](using-gulp.md)).</span><span class="sxs-lookup"><span data-stu-id="7faf4-105">Grunt is fully supported in Visual Studio, though the ASP.NET project templates use Gulp by default (see [Use Gulp](using-gulp.md)).</span></span>

<span data-ttu-id="7faf4-106">В этом примере использует пустой проект ASP.NET Core в качестве начальной точки, демонстрирует, как автоматизировать процесс построения клиента с нуля.</span><span class="sxs-lookup"><span data-stu-id="7faf4-106">This example uses an empty ASP.NET Core project as its starting point, to show how to automate the client build process from scratch.</span></span>

<span data-ttu-id="7faf4-107">Готовый пример Очищает целевой каталог развертывания, объединяет файлы JavaScript, проверяет качество кода, также, объединяя содержимое файла JavaScript и развертывает в корень веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="7faf4-107">The finished example cleans the target deployment directory, combines JavaScript files, checks code quality, condenses JavaScript file content and deploys to the root of your web application.</span></span> <span data-ttu-id="7faf4-108">Мы будем использовать следующие пакеты:</span><span class="sxs-lookup"><span data-stu-id="7faf4-108">We will use the following packages:</span></span>

* <span data-ttu-id="7faf4-109">**grunt**: Пакет runner задачи Grunt.</span><span class="sxs-lookup"><span data-stu-id="7faf4-109">**grunt**: The Grunt task runner package.</span></span>

* <span data-ttu-id="7faf4-110">**grunt-contrib-clean**: Подключаемый модуль, который удаляет файлы или каталоги.</span><span class="sxs-lookup"><span data-stu-id="7faf4-110">**grunt-contrib-clean**: A plugin that removes files or directories.</span></span>

* <span data-ttu-id="7faf4-111">**grunt-contrib-jshint**: Подключаемый модуль, который просматривает качества кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7faf4-111">**grunt-contrib-jshint**: A plugin that reviews JavaScript code quality.</span></span>

* <span data-ttu-id="7faf4-112">**grunt-contrib-concat**: Подключаемый модуль, который объединяет файлы в один файл.</span><span class="sxs-lookup"><span data-stu-id="7faf4-112">**grunt-contrib-concat**: A plugin that joins files into a single file.</span></span>

* <span data-ttu-id="7faf4-113">**grunt-contrib-uglify**: Подключаемый модуль, который уменьшает JavaScript для уменьшения размера.</span><span class="sxs-lookup"><span data-stu-id="7faf4-113">**grunt-contrib-uglify**: A plugin that minifies JavaScript to reduce size.</span></span>

* <span data-ttu-id="7faf4-114">**grunt-contrib-watch**: Подключаемый модуль, который отслеживает действия файла.</span><span class="sxs-lookup"><span data-stu-id="7faf4-114">**grunt-contrib-watch**: A plugin that watches file activity.</span></span>

## <a name="preparing-the-application"></a><span data-ttu-id="7faf4-115">Подготовка приложения</span><span class="sxs-lookup"><span data-stu-id="7faf4-115">Preparing the application</span></span>

<span data-ttu-id="7faf4-116">Чтобы начать, настройте новый пустой веб-приложения и добавить файлы TypeScript пример.</span><span class="sxs-lookup"><span data-stu-id="7faf4-116">To begin, set up a new empty web application and add TypeScript example files.</span></span> <span data-ttu-id="7faf4-117">Файлы TypeScript компилируются в JavaScript с использованием параметров по умолчанию Visual Studio автоматически и будет наш исходный материал для обработки с помощью Grunt.</span><span class="sxs-lookup"><span data-stu-id="7faf4-117">TypeScript files are automatically compiled into JavaScript using default Visual Studio settings and will be our raw material to process using Grunt.</span></span>

1. <span data-ttu-id="7faf4-118">В Visual Studio создайте новое `ASP.NET Web Application`.</span><span class="sxs-lookup"><span data-stu-id="7faf4-118">In Visual Studio, create a new `ASP.NET Web Application`.</span></span>

2. <span data-ttu-id="7faf4-119">В **новый проект ASP.NET** диалоговое окно, выберите ASP.NET Core **пустой** шаблона и нажмите кнопку "ОК".</span><span class="sxs-lookup"><span data-stu-id="7faf4-119">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template and click the OK button.</span></span>

3. <span data-ttu-id="7faf4-120">В обозревателе решений просмотрите структуру этого проекта.</span><span class="sxs-lookup"><span data-stu-id="7faf4-120">In the Solution Explorer, review the project structure.</span></span> <span data-ttu-id="7faf4-121">`\src` Папка содержит пустой `wwwroot` и `Dependencies` узлов.</span><span class="sxs-lookup"><span data-stu-id="7faf4-121">The `\src` folder includes empty `wwwroot` and `Dependencies` nodes.</span></span>

    ![пустой веб-решений](using-grunt/_static/grunt-solution-explorer.png)

4. <span data-ttu-id="7faf4-123">Добавьте новую папку с именем `TypeScript` в каталог проекта.</span><span class="sxs-lookup"><span data-stu-id="7faf4-123">Add a new folder named `TypeScript` to your project directory.</span></span>

5. <span data-ttu-id="7faf4-124">Прежде чем добавить любые файлы, убедитесь, что Visual Studio имеет параметр "компилировать при сохранении" для возвращенных файлов TypeScript.</span><span class="sxs-lookup"><span data-stu-id="7faf4-124">Before adding any files, make sure that Visual Studio has the option 'compile on save' for TypeScript files checked.</span></span> <span data-ttu-id="7faf4-125">Перейдите к **средства** > **параметры** > **текстовый редактор** > **Typescript**  >  **Проекта**:</span><span class="sxs-lookup"><span data-stu-id="7faf4-125">Navigate to **Tools** > **Options** > **Text Editor** > **Typescript** > **Project**:</span></span>

    ![Параметры автоматической компиляции TypeScript файлов](using-grunt/_static/typescript-options.png)

6. <span data-ttu-id="7faf4-127">Щелкните правой кнопкой мыши `TypeScript` каталог и выберите **Добавить > новый элемент** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="7faf4-127">Right-click the `TypeScript` directory and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="7faf4-128">Выберите **файл JavaScript** элемента и назовите файл *Tastes.ts* (Примечание \*расширением .ts).</span><span class="sxs-lookup"><span data-stu-id="7faf4-128">Select the **JavaScript file** item and name the file *Tastes.ts* (note the \*.ts extension).</span></span> <span data-ttu-id="7faf4-129">Скопируйте строку кода TypeScript ниже в файл (при сохранении, новый *Tastes.js* появится файл с источником JavaScript).</span><span class="sxs-lookup"><span data-stu-id="7faf4-129">Copy the line of TypeScript code below into the file (when you save, a new *Tastes.js* file will appear with the JavaScript source).</span></span>

    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7. <span data-ttu-id="7faf4-130">Добавьте второй файл в **TypeScript** каталог и назовите его `Food.ts`.</span><span class="sxs-lookup"><span data-stu-id="7faf4-130">Add a second file to the **TypeScript** directory and name it `Food.ts`.</span></span> <span data-ttu-id="7faf4-131">Скопируйте приведенный ниже код в файл.</span><span class="sxs-lookup"><span data-stu-id="7faf4-131">Copy the code below into the file.</span></span>

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

## <a name="configuring-npm"></a><span data-ttu-id="7faf4-132">Настройка NPM</span><span class="sxs-lookup"><span data-stu-id="7faf4-132">Configuring NPM</span></span>

<span data-ttu-id="7faf4-133">Настройте NPM для загрузки grunt и grunt задачи.</span><span class="sxs-lookup"><span data-stu-id="7faf4-133">Next, configure NPM to download grunt and grunt-tasks.</span></span>

1. <span data-ttu-id="7faf4-134">В обозревателе решений щелкните правой кнопкой мыши проект и выберите **Добавить > новый элемент** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="7faf4-134">In the Solution Explorer, right-click the project and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="7faf4-135">Выберите **файл конфигурации NPM** товара, оставьте имя по умолчанию *package.json*и нажмите кнопку **добавить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="7faf4-135">Select the **NPM configuration file** item, leave the default name, *package.json*, and click the **Add** button.</span></span>

2. <span data-ttu-id="7faf4-136">В *package.json* файл внутри `devDependencies` объекта фигурные скобки, введите «grunt».</span><span class="sxs-lookup"><span data-stu-id="7faf4-136">In the *package.json* file, inside the `devDependencies` object braces, enter "grunt".</span></span> <span data-ttu-id="7faf4-137">Выберите `grunt` из Intellisense списка и нажмите клавишу ВВОД.</span><span class="sxs-lookup"><span data-stu-id="7faf4-137">Select `grunt` from the Intellisense list and press the Enter key.</span></span> <span data-ttu-id="7faf4-138">Visual Studio Квота имя пакета grunt и добавьте двоеточие.</span><span class="sxs-lookup"><span data-stu-id="7faf4-138">Visual Studio will quote the grunt package name, and add a colon.</span></span> <span data-ttu-id="7faf4-139">Справа от двоеточия, выберите последнюю стабильную версию пакета в верхней части списка Intellisense (нажмите клавишу `Ctrl-Space` Если Intellisense не отображается).</span><span class="sxs-lookup"><span data-stu-id="7faf4-139">To the right of the colon, select the latest stable version of the package from the top of the Intellisense list (press `Ctrl-Space` if Intellisense doesn't appear).</span></span>

    ![grunt Intellisense](using-grunt/_static/devdependencies-grunt.png)

    > [!NOTE]
    > <span data-ttu-id="7faf4-141">NPM использует [семантического управления версиями](http://semver.org/) для упорядочения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="7faf4-141">NPM uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="7faf4-142">Семантическое управление версиями, также известный как SemVer, определяет пакеты с формирование \<основных >.\< дополнительный номер >. \<patch >.</span><span class="sxs-lookup"><span data-stu-id="7faf4-142">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="7faf4-143">IntelliSense упрощает семантического управления версиями, отображая только несколько распространенных вариантов действий.</span><span class="sxs-lookup"><span data-stu-id="7faf4-143">Intellisense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="7faf4-144">Верхний элемент в списке Intellisense (0.4.5 в приведенном выше примере) считается последняя стабильная версия пакета.</span><span class="sxs-lookup"><span data-stu-id="7faf4-144">The top item in the Intellisense list (0.4.5 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="7faf4-145">Символ крышки (^) соответствует самой последней основной версии и тильда (~) соответствует самой последней дополнительный номер версии.</span><span class="sxs-lookup"><span data-stu-id="7faf4-145">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span> <span data-ttu-id="7faf4-146">См. в разделе [NPM semver версии средства синтаксического анализа ссылки](https://www.npmjs.com/package/semver) в соответствии с полной выразительной, предоставляющий SemVer.</span><span class="sxs-lookup"><span data-stu-id="7faf4-146">See the [NPM semver version parser reference](https://www.npmjs.com/package/semver) as a guide to the full expressivity that SemVer provides.</span></span>

3. <span data-ttu-id="7faf4-147">Добавить дополнительные зависимости, чтобы загрузить grunt-contrib -\* пакеты для *чистой*, *jshint*, *concat*, *uglify*и *watch* как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="7faf4-147">Add more dependencies to load grunt-contrib-\* packages for *clean*, *jshint*, *concat*, *uglify*, and *watch* as shown in the example below.</span></span> <span data-ttu-id="7faf4-148">Версии нет необходимости в примере.</span><span class="sxs-lookup"><span data-stu-id="7faf4-148">The versions don't need to match the example.</span></span>

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

4. <span data-ttu-id="7faf4-149">Сохранить *package.json* файла.</span><span class="sxs-lookup"><span data-stu-id="7faf4-149">Save the *package.json* file.</span></span>

<span data-ttu-id="7faf4-150">Пакеты для каждого элемента devDependencies загрузит вместе с файлами, которые требуются для каждого пакета.</span><span class="sxs-lookup"><span data-stu-id="7faf4-150">The packages for each devDependencies item will download, along with any files that each package requires.</span></span> <span data-ttu-id="7faf4-151">Можно найти файлы пакета в `node_modules` каталог, включив **Показать все файлы** кнопка в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="7faf4-151">You can find the package files in the `node_modules` directory by enabling the **Show All Files** button in the Solution Explorer.</span></span>

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> <span data-ttu-id="7faf4-153">Если вам нужно, можно вручную восстановить зависимости в обозревателе решений щелкните правой кнопкой мыши `Dependencies\NPM` и выбрав **восстановить пакеты** пункт меню.</span><span class="sxs-lookup"><span data-stu-id="7faf4-153">If you need to, you can manually restore dependencies in Solution Explorer by right-clicking on `Dependencies\NPM` and selecting the **Restore Packages** menu option.</span></span>

![Восстановление пакетов](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a><span data-ttu-id="7faf4-155">Настройка Grunt</span><span class="sxs-lookup"><span data-stu-id="7faf4-155">Configuring Grunt</span></span>

<span data-ttu-id="7faf4-156">Grunt настраивается с помощью манифеста с именем *Gruntfile.js* , определяет, загружает и регистрирует задачи, которые можно запускать вручную или настроить для запуска автоматически в зависимости о событиях в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7faf4-156">Grunt is configured using a manifest named *Gruntfile.js* that defines, loads and registers tasks that can be run manually or configured to run automatically based on events in Visual Studio.</span></span>

1. <span data-ttu-id="7faf4-157">Щелкните правой кнопкой мыши проект и выберите **Добавить > новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="7faf4-157">Right-click the project and select **Add > New Item**.</span></span> <span data-ttu-id="7faf4-158">Выберите **файл конфигурации Grunt** , то оставьте имя по умолчанию *Gruntfile.js*и нажмите кнопку **добавить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="7faf4-158">Select the **Grunt Configuration file** option, leave the default name, *Gruntfile.js*, and click the **Add** button.</span></span>

   <span data-ttu-id="7faf4-159">Исходный код включает определение модуля и `grunt.initConfig()` метод.</span><span class="sxs-lookup"><span data-stu-id="7faf4-159">The initial code includes a module definition and the `grunt.initConfig()` method.</span></span> <span data-ttu-id="7faf4-160">`initConfig()` Используется для задания параметров для каждого пакета и остальная часть модуля будет загрузить и зарегистрировать задачи.</span><span class="sxs-lookup"><span data-stu-id="7faf4-160">The `initConfig()` is used to set options for each package, and the remainder of the module will load and register tasks.</span></span>

   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

2. <span data-ttu-id="7faf4-161">Внутри `initConfig()` метод, параметры для добавления `clean` задач, как показано в примере *Gruntfile.js* ниже.</span><span class="sxs-lookup"><span data-stu-id="7faf4-161">Inside the `initConfig()` method, add options for the `clean` task as shown in the example *Gruntfile.js* below.</span></span> <span data-ttu-id="7faf4-162">Задачу "Очистить" принимает массив строк каталога.</span><span class="sxs-lookup"><span data-stu-id="7faf4-162">The clean task accepts an array of directory strings.</span></span> <span data-ttu-id="7faf4-163">Эта задача удаляет файлы из wwwroot/lib и удаляет всю/временный каталог.</span><span class="sxs-lookup"><span data-stu-id="7faf4-163">This task removes files from wwwroot/lib and removes the entire /temp directory.</span></span>

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. <span data-ttu-id="7faf4-164">После метода initConfig(), добавьте вызов `grunt.loadNpmTasks()`.</span><span class="sxs-lookup"><span data-stu-id="7faf4-164">Below the initConfig() method, add a call to `grunt.loadNpmTasks()`.</span></span> <span data-ttu-id="7faf4-165">Это сделает задачи запускаемых из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7faf4-165">This will make the task runnable from Visual Studio.</span></span>

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. <span data-ttu-id="7faf4-166">Сохранить *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="7faf4-166">Save *Gruntfile.js*.</span></span> <span data-ttu-id="7faf4-167">Файл должен выглядеть как на снимке экрана ниже.</span><span class="sxs-lookup"><span data-stu-id="7faf4-167">The file should look something like the screenshot below.</span></span>

    ![Начальное gruntfile](using-grunt/_static/gruntfile-js-initial.png)

5. <span data-ttu-id="7faf4-169">Щелкните правой кнопкой мыши *Gruntfile.js* и выберите **Task Runner Explorer** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="7faf4-169">Right-click *Gruntfile.js* and select **Task Runner Explorer** from the context menu.</span></span> <span data-ttu-id="7faf4-170">Откроется окно Task Runner Explorer.</span><span class="sxs-lookup"><span data-stu-id="7faf4-170">The Task Runner Explorer window will open.</span></span>

    ![меню обозревателя средство выполнения задачи](using-grunt/_static/task-runner-explorer-menu.png)

6. <span data-ttu-id="7faf4-172">Убедитесь, что `clean` отображается в области **задачи** в Task Runner Explorer.</span><span class="sxs-lookup"><span data-stu-id="7faf4-172">Verify that `clean` shows under **Tasks** in the Task Runner Explorer.</span></span>

    ![Список задач explorer средство запуска задач](using-grunt/_static/task-runner-explorer-tasks.png)

7. <span data-ttu-id="7faf4-174">Щелкните правой кнопкой мыши задачу "Очистить" и выберите **запуска** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="7faf4-174">Right-click the clean task and select **Run** from the context menu.</span></span> <span data-ttu-id="7faf4-175">Окно командной строки отображает ход выполнения задачи.</span><span class="sxs-lookup"><span data-stu-id="7faf4-175">A command window displays progress of the task.</span></span>

    ![Задача runner explorer выполните задачу "Очистить"](using-grunt/_static/task-runner-explorer-run-clean.png)

    > [!NOTE]
    > <span data-ttu-id="7faf4-177">Нет файлов или каталогов для очистки еще.</span><span class="sxs-lookup"><span data-stu-id="7faf4-177">There are no files or directories to clean yet.</span></span> <span data-ttu-id="7faf4-178">При желании можно создать их вручную в обозревателе решений и запустите задачу "Очистить" для проверки.</span><span class="sxs-lookup"><span data-stu-id="7faf4-178">If you like, you can manually create them in the Solution Explorer and then run the clean task as a test.</span></span>

8. <span data-ttu-id="7faf4-179">В методе initConfig(), добавьте запись для `concat` с помощью следующего кода.</span><span class="sxs-lookup"><span data-stu-id="7faf4-179">In the initConfig() method, add an entry for `concat` using the code below.</span></span>

    <span data-ttu-id="7faf4-180">`src` Массива свойств перечислены файлы для объединения, в том порядке, что они должны быть объединены.</span><span class="sxs-lookup"><span data-stu-id="7faf4-180">The `src` property array lists files to combine, in the order that they should be combined.</span></span> <span data-ttu-id="7faf4-181">`dest` Свойство назначает путь объединенный файл, который создается.</span><span class="sxs-lookup"><span data-stu-id="7faf4-181">The `dest` property assigns the path to the combined file that's produced.</span></span>

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="7faf4-182">`all` Свойство в приведенном выше коде — имя целевого объекта.</span><span class="sxs-lookup"><span data-stu-id="7faf4-182">The `all` property in the code above is the name of a target.</span></span> <span data-ttu-id="7faf4-183">Целевые объекты используются в некоторые задачи Grunt, чтобы разрешить несколько сред сборки.</span><span class="sxs-lookup"><span data-stu-id="7faf4-183">Targets are used in some Grunt tasks to allow multiple build environments.</span></span> <span data-ttu-id="7faf4-184">Можно просмотреть встроенные целевые объекты, с помощью Intellisense или назначить свои собственные.</span><span class="sxs-lookup"><span data-stu-id="7faf4-184">You can view the built-in targets using Intellisense or assign your own.</span></span>

9. <span data-ttu-id="7faf4-185">Добавление `jshint` задач с помощью следующего кода.</span><span class="sxs-lookup"><span data-stu-id="7faf4-185">Add the `jshint` task using the code below.</span></span>

    <span data-ttu-id="7faf4-186">Программа jshint качества кода выполняется для каждого файла JavaScript, найденные в каталоге temp.</span><span class="sxs-lookup"><span data-stu-id="7faf4-186">The jshint code-quality utility is run against every JavaScript file found in the temp directory.</span></span>

    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="7faf4-187">Параметр «-W069» ошибка создается путем jshint при JavaScript используется скобкой синтаксис, чтобы назначить свойство вместо точечной нотации, т. е. `Tastes["Sweet"]` вместо `Tastes.Sweet`.</span><span class="sxs-lookup"><span data-stu-id="7faf4-187">The option "-W069" is an error produced by jshint when JavaScript uses bracket syntax to assign a property instead of dot notation, i.e. `Tastes["Sweet"]` instead of `Tastes.Sweet`.</span></span> <span data-ttu-id="7faf4-188">Параметр отключает предупреждение, чтобы разрешить остальная часть процесса, чтобы продолжить.</span><span class="sxs-lookup"><span data-stu-id="7faf4-188">The option turns off the warning to allow the rest of the process to continue.</span></span>

10. <span data-ttu-id="7faf4-189">Добавление `uglify` задач с помощью следующего кода.</span><span class="sxs-lookup"><span data-stu-id="7faf4-189">Add the `uglify` task using the code below.</span></span>

    <span data-ttu-id="7faf4-190">Задача уменьшает *combined.js* файл найден во временном каталоге и создает файл результатов в стандартные соглашения об именовании wwwroot/lib  *\<имя файла\>. min.js*.</span><span class="sxs-lookup"><span data-stu-id="7faf4-190">The task minifies the *combined.js* file found in the temp directory and creates the result file in wwwroot/lib following the standard naming convention *\<file name\>.min.js*.</span></span>

    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

11. <span data-ttu-id="7faf4-191">В разделе grunt.loadNpmTasks() вызов, который загружает grunt-contrib-clean включать один и тот же вызов для jshint, concat и uglify, с помощью следующего кода.</span><span class="sxs-lookup"><span data-stu-id="7faf4-191">Under the call grunt.loadNpmTasks() that loads grunt-contrib-clean, include the same call for jshint, concat and uglify using the code below.</span></span>

    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. <span data-ttu-id="7faf4-192">Сохранить *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="7faf4-192">Save *Gruntfile.js*.</span></span> <span data-ttu-id="7faf4-193">Файл должен выглядеть как в примере ниже.</span><span class="sxs-lookup"><span data-stu-id="7faf4-193">The file should look something like the example below.</span></span>

    ![Пример файла полный grunt](using-grunt/_static/gruntfile-js-complete.png)

13. <span data-ttu-id="7faf4-195">Обратите внимание, что список задач Explorer Runner задача включает в себя `clean`, `concat`, `jshint` и `uglify` задачи.</span><span class="sxs-lookup"><span data-stu-id="7faf4-195">Notice that the Task Runner Explorer Tasks list includes `clean`, `concat`, `jshint` and `uglify` tasks.</span></span> <span data-ttu-id="7faf4-196">Выполнение каждой задачи в порядке и просмотрите результаты в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="7faf4-196">Run each task in order and observe the results in Solution Explorer.</span></span> <span data-ttu-id="7faf4-197">Каждая задача должно работать без ошибок.</span><span class="sxs-lookup"><span data-stu-id="7faf4-197">Each task should run without errors.</span></span>

    ![Диспетчер выполнения задач, выполнять каждую задачу](using-grunt/_static/task-runner-explorer-run-each-task.png)

    <span data-ttu-id="7faf4-199">Создает новую задачу concat *combined.js* файл и помещает его в папку temp.</span><span class="sxs-lookup"><span data-stu-id="7faf4-199">The concat task creates a new *combined.js* file and places it into the temp directory.</span></span> <span data-ttu-id="7faf4-200">Задача jshint просто выполняется и не создает выходных данных.</span><span class="sxs-lookup"><span data-stu-id="7faf4-200">The jshint task simply runs and doesn't produce output.</span></span> <span data-ttu-id="7faf4-201">Создает новую задачу uglify *combined.min.js* файл и помещает его в wwwroot/lib.</span><span class="sxs-lookup"><span data-stu-id="7faf4-201">The uglify task creates a new *combined.min.js* file and places it into wwwroot/lib.</span></span> <span data-ttu-id="7faf4-202">По завершении решения должен выглядеть примерно как на снимке экрана ниже:</span><span class="sxs-lookup"><span data-stu-id="7faf4-202">On completion, the solution should look something like the screenshot below:</span></span>

    ![в конце концов задачи в обозревателе решений](using-grunt/_static/solution-explorer-after-all-tasks.png)

    > [!NOTE]
    > <span data-ttu-id="7faf4-204">Дополнительные сведения о параметрах для каждого пакета [ https://www.npmjs.com/ ](https://www.npmjs.com/) и lookup имя пакета в поле поиска на главной странице.</span><span class="sxs-lookup"><span data-stu-id="7faf4-204">For more information on the options for each package, visit [https://www.npmjs.com/](https://www.npmjs.com/) and lookup the package name in the search box on the main page.</span></span> <span data-ttu-id="7faf4-205">Например можно выполнять поиск пакет grunt-contrib-clean, чтобы получить ссылку на документацию, объясняющее, все его параметры.</span><span class="sxs-lookup"><span data-stu-id="7faf4-205">For example, you can look up the grunt-contrib-clean package to get a documentation link that explains all of its parameters.</span></span>

### <a name="all-together-now"></a><span data-ttu-id="7faf4-206">Теперь все вместе</span><span class="sxs-lookup"><span data-stu-id="7faf4-206">All together now</span></span>

<span data-ttu-id="7faf4-207">Использование Grunt `registerTask()` метод для выполнения ряда задач в определенной последовательности.</span><span class="sxs-lookup"><span data-stu-id="7faf4-207">Use the Grunt `registerTask()` method to run a series of tasks in a particular sequence.</span></span> <span data-ttu-id="7faf4-208">Например, для выполнения этого примера, действия, описанные в чистой порядок "->" concat "->" jshint "->" uglify, добавьте приведенный ниже код в модуль.</span><span class="sxs-lookup"><span data-stu-id="7faf4-208">For example, to run the example steps above in the order clean -> concat -> jshint -> uglify, add the code below to the module.</span></span> <span data-ttu-id="7faf4-209">Код необходимо добавить как вызовы loadNpmTasks(), за пределами initConfig того же уровня.</span><span class="sxs-lookup"><span data-stu-id="7faf4-209">The code should be added to the same level as the loadNpmTasks() calls, outside initConfig.</span></span>

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

<span data-ttu-id="7faf4-210">Новая задача отображается в Task Runner Explorer под задачи псевдонимов.</span><span class="sxs-lookup"><span data-stu-id="7faf4-210">The new task shows up in Task Runner Explorer under Alias Tasks.</span></span> <span data-ttu-id="7faf4-211">Можно правой кнопкой мыши и запустите его, так же, как и другие задачи.</span><span class="sxs-lookup"><span data-stu-id="7faf4-211">You can right-click and run it just as you would other tasks.</span></span> <span data-ttu-id="7faf4-212">`all` Задача будет выполняться `clean`, `concat`, `jshint` и `uglify`, в порядке.</span><span class="sxs-lookup"><span data-stu-id="7faf4-212">The `all` task will run `clean`, `concat`, `jshint` and `uglify`, in order.</span></span>

![задачи grunt псевдонимов](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a><span data-ttu-id="7faf4-214">Просмотр изменений</span><span class="sxs-lookup"><span data-stu-id="7faf4-214">Watching for changes</span></span>

<span data-ttu-id="7faf4-215">Объект `watch` задач следят над файлами и каталогами.</span><span class="sxs-lookup"><span data-stu-id="7faf4-215">A `watch` task keeps an eye on files and directories.</span></span> <span data-ttu-id="7faf4-216">Часы автоматически активирует задачи при обнаружении изменений.</span><span class="sxs-lookup"><span data-stu-id="7faf4-216">The watch triggers tasks automatically if it detects changes.</span></span> <span data-ttu-id="7faf4-217">Добавьте приведенный ниже код для initConfig для отслеживания изменений для \*JS-файлов в каталоге TypeScript.</span><span class="sxs-lookup"><span data-stu-id="7faf4-217">Add the code below to initConfig to watch for changes to \*.js files in the TypeScript directory.</span></span> <span data-ttu-id="7faf4-218">Если изменяется файл JavaScript, `watch` будет выполняться `all` задачи.</span><span class="sxs-lookup"><span data-stu-id="7faf4-218">If a JavaScript file is changed, `watch` will run the `all` task.</span></span>

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

<span data-ttu-id="7faf4-219">Добавьте вызов `loadNpmTasks()` Показать `watch` задач в Task Runner Explorer.</span><span class="sxs-lookup"><span data-stu-id="7faf4-219">Add a call to `loadNpmTasks()` to show the `watch` task in Task Runner Explorer.</span></span>

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

<span data-ttu-id="7faf4-220">Щелкните правой кнопкой мыши задачу «watch» в Task Runner Explorer и выберите в контекстном меню запуска.</span><span class="sxs-lookup"><span data-stu-id="7faf4-220">Right-click the watch task in Task Runner Explorer and select Run from the context menu.</span></span> <span data-ttu-id="7faf4-221">Командное окно, показывающий выполняемую задачу Контрольные значения будет отображаться «ожидание...» Сообщение.</span><span class="sxs-lookup"><span data-stu-id="7faf4-221">The command window that shows the watch task running will display a "Waiting…" message.</span></span> <span data-ttu-id="7faf4-222">Откройте один из файлов TypeScript, добавьте пробел и затем сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="7faf4-222">Open one of the TypeScript files, add a space, and then save the file.</span></span> <span data-ttu-id="7faf4-223">Это будет запустить задачу, Контрольные значения и активируйте другие задачи, чтобы выполняются по порядку.</span><span class="sxs-lookup"><span data-stu-id="7faf4-223">This will trigger the watch task and trigger the other tasks to run in order.</span></span> <span data-ttu-id="7faf4-224">На следующем снимке экрана показан пример запуска.</span><span class="sxs-lookup"><span data-stu-id="7faf4-224">The screenshot below shows a sample run.</span></span>

![под управлением выходных данных задачи](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a><span data-ttu-id="7faf4-226">Привязка к событиям Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7faf4-226">Binding to Visual Studio events</span></span>

<span data-ttu-id="7faf4-227">Если вам не нужно вручную запустить задачи, каждый раз при работе в Visual Studio, можно привязать задач **перед построить**, **после построения**, **Очистить**, и  **Проект открытым** события.</span><span class="sxs-lookup"><span data-stu-id="7faf4-227">Unless you want to manually start your tasks every time you work in Visual Studio, you can bind tasks to **Before Build**, **After Build**, **Clean**, and **Project Open** events.</span></span>

<span data-ttu-id="7faf4-228">Следует привязать `watch` таким образом, чтобы он выполняется при каждом запуске Visual Studio открывает.</span><span class="sxs-lookup"><span data-stu-id="7faf4-228">Let’s bind `watch` so that it runs every time Visual Studio opens.</span></span> <span data-ttu-id="7faf4-229">В Task Runner Explorer правой кнопкой мыши задачу контрольных значений и выберите **привязки > Открыть проект** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="7faf4-229">In Task Runner Explorer, right-click the watch task and select **Bindings > Project Open** from the context menu.</span></span>

![Привязать задачу к Открытие проекта](using-grunt/_static/bindings-project-open.png)

<span data-ttu-id="7faf4-231">Выгрузите и перезагрузите проект.</span><span class="sxs-lookup"><span data-stu-id="7faf4-231">Unload and reload the project.</span></span> <span data-ttu-id="7faf4-232">Когда снова загружает проект, задачи «watch» запущен автоматически.</span><span class="sxs-lookup"><span data-stu-id="7faf4-232">When the project loads again, the watch task will start running automatically.</span></span>

## <a name="summary"></a><span data-ttu-id="7faf4-233">Сводка</span><span class="sxs-lookup"><span data-stu-id="7faf4-233">Summary</span></span>

<span data-ttu-id="7faf4-234">Grunt является запускатель задач мощные, позволяют автоматизировать большинство задач сборки клиента.</span><span class="sxs-lookup"><span data-stu-id="7faf4-234">Grunt is a powerful task runner that can be used to automate most client-build tasks.</span></span> <span data-ttu-id="7faf4-235">Grunt использует NPM для доставки пакетов и функций, средств интеграции с Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7faf4-235">Grunt leverages NPM to deliver its packages, and features tooling integration with Visual Studio.</span></span> <span data-ttu-id="7faf4-236">Visual Studio Task Runner Explorer обнаруживает изменения в файлах конфигурации и предоставляет удобный интерфейс для выполнения задач, просмотр выполняющихся задач и привязывание задач к событиям Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7faf4-236">Visual Studio's Task Runner Explorer detects changes to configuration files and provides a convenient interface to run tasks, view running tasks, and bind tasks to Visual Studio events.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7faf4-237">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="7faf4-237">Additional resources</span></span>

* [<span data-ttu-id="7faf4-238">Использование Gulp</span><span class="sxs-lookup"><span data-stu-id="7faf4-238">Use Gulp</span></span>](using-gulp.md)
