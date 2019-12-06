---
title: Использование grunt в ASP.NET Core
author: rick-anderson
description: Использование grunt в ASP.NET Core
ms.author: riande
ms.date: 12/05/2019
uid: client-side/using-grunt
ms.openlocfilehash: e516b85da7e94d0c93be642086fede0a11fea3c2
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2019
ms.locfileid: "74879796"
---
# <a name="use-grunt-in-aspnet-core"></a><span data-ttu-id="9af56-103">Использование grunt в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9af56-103">Use Grunt in ASP.NET Core</span></span>

<span data-ttu-id="9af56-104">Grunt — это средство выполнения задач JavaScript, которое автоматизирует скрипты минификации, компиляцию TypeScript, качества кода "Lint", предварительные процессоры CSS и практически любую рутинную работу, которая необходима для поддержки разработки клиентов.</span><span class="sxs-lookup"><span data-stu-id="9af56-104">Grunt is a JavaScript task runner that automates script minification, TypeScript compilation, code quality "lint" tools, CSS pre-processors, and just about any repetitive chore that needs doing to support client development.</span></span> <span data-ttu-id="9af56-105">Grunt полностью поддерживается в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9af56-105">Grunt is fully supported in Visual Studio.</span></span>

<span data-ttu-id="9af56-106">В этом примере в качестве начальной точки используется пустой проект ASP.NET Core, чтобы продемонстрировать, как автоматизировать процесс сборки клиента с нуля.</span><span class="sxs-lookup"><span data-stu-id="9af56-106">This example uses an empty ASP.NET Core project as its starting point, to show how to automate the client build process from scratch.</span></span>

<span data-ttu-id="9af56-107">В завершенном примере очищается целевой каталог развертывания, объединяются файлы JavaScript, проверяется качество кода, сжимается содержимое файлов JavaScript и развертывается в корне веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="9af56-107">The finished example cleans the target deployment directory, combines JavaScript files, checks code quality, condenses JavaScript file content and deploys to the root of your web application.</span></span> <span data-ttu-id="9af56-108">Мы будем использовать следующие пакеты:</span><span class="sxs-lookup"><span data-stu-id="9af56-108">We will use the following packages:</span></span>

* <span data-ttu-id="9af56-109">**grunt**: пакет средства выполнения задач grunt.</span><span class="sxs-lookup"><span data-stu-id="9af56-109">**grunt**: The Grunt task runner package.</span></span>

* <span data-ttu-id="9af56-110">**grunt-от участников сообщества-Clean**: подключаемый модуль, удаляющий файлы или каталоги.</span><span class="sxs-lookup"><span data-stu-id="9af56-110">**grunt-contrib-clean**: A plugin that removes files or directories.</span></span>

* <span data-ttu-id="9af56-111">**grunt-от участников сообщества-жшинт**— подключаемый модуль, который проверяет качество кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9af56-111">**grunt-contrib-jshint**: A plugin that reviews JavaScript code quality.</span></span>

* <span data-ttu-id="9af56-112">**grunt-от участников сообщества-Concat**: подключаемый модуль, объединяющий файлы в один файл.</span><span class="sxs-lookup"><span data-stu-id="9af56-112">**grunt-contrib-concat**: A plugin that joins files into a single file.</span></span>

* <span data-ttu-id="9af56-113">**grunt-от участников сообщества-углифи**— подключаемый модуль, который уменьшает JavaScript для уменьшения размера.</span><span class="sxs-lookup"><span data-stu-id="9af56-113">**grunt-contrib-uglify**: A plugin that minifies JavaScript to reduce size.</span></span>

* <span data-ttu-id="9af56-114">**grunt-от участников сообщества-Watch**: подключаемый модуль, отслеживающий активность файлов.</span><span class="sxs-lookup"><span data-stu-id="9af56-114">**grunt-contrib-watch**: A plugin that watches file activity.</span></span>

## <a name="preparing-the-application"></a><span data-ttu-id="9af56-115">Подготовка приложения</span><span class="sxs-lookup"><span data-stu-id="9af56-115">Preparing the application</span></span>

<span data-ttu-id="9af56-116">Чтобы начать, настройте новое пустое веб-приложение и добавьте файлы примеров TypeScript.</span><span class="sxs-lookup"><span data-stu-id="9af56-116">To begin, set up a new empty web application and add TypeScript example files.</span></span> <span data-ttu-id="9af56-117">Файлы TypeScript автоматически компилируются в JavaScript с помощью параметров по умолчанию Visual Studio и будут рассматриваться как необработанные материалы для обработки с использованием grunt.</span><span class="sxs-lookup"><span data-stu-id="9af56-117">TypeScript files are automatically compiled into JavaScript using default Visual Studio settings and will be our raw material to process using Grunt.</span></span>

1. <span data-ttu-id="9af56-118">В Visual Studio создайте новый `ASP.NET Web Application`.</span><span class="sxs-lookup"><span data-stu-id="9af56-118">In Visual Studio, create a new `ASP.NET Web Application`.</span></span>

2. <span data-ttu-id="9af56-119">В диалоговом окне **Новый проект ASP.NET** выберите ASP.NET Core **пустой** шаблон и нажмите кнопку ОК.</span><span class="sxs-lookup"><span data-stu-id="9af56-119">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template and click the OK button.</span></span>

3. <span data-ttu-id="9af56-120">В обозреватель решений проверьте структуру проекта.</span><span class="sxs-lookup"><span data-stu-id="9af56-120">In the Solution Explorer, review the project structure.</span></span> <span data-ttu-id="9af56-121">Папка `\src` содержит пустые узлы `wwwroot` и `Dependencies`.</span><span class="sxs-lookup"><span data-stu-id="9af56-121">The `\src` folder includes empty `wwwroot` and `Dependencies` nodes.</span></span>

    ![пустое веб-решение](using-grunt/_static/grunt-solution-explorer.png)

4. <span data-ttu-id="9af56-123">Добавьте новую папку с именем `TypeScript` в каталог проекта.</span><span class="sxs-lookup"><span data-stu-id="9af56-123">Add a new folder named `TypeScript` to your project directory.</span></span>

5. <span data-ttu-id="9af56-124">Перед добавлением файлов убедитесь, что в Visual Studio установлен параметр "компилировать при сохранении" для файлов TypeScript.</span><span class="sxs-lookup"><span data-stu-id="9af56-124">Before adding any files, make sure that Visual Studio has the option 'compile on save' for TypeScript files checked.</span></span> <span data-ttu-id="9af56-125">Последовательно выберите **сервис** > **Параметры** > **текстовый редактор** > **проект** **typescript** > .</span><span class="sxs-lookup"><span data-stu-id="9af56-125">Navigate to **Tools** > **Options** > **Text Editor** > **Typescript** > **Project**:</span></span>

    ![параметры, устанавливающие автоматическую компиляцию файлов TypeScript](using-grunt/_static/typescript-options.png)

6. <span data-ttu-id="9af56-127">Щелкните правой кнопкой мыши каталог `TypeScript` и выберите в контекстном меню команду **добавить > новый элемент** .</span><span class="sxs-lookup"><span data-stu-id="9af56-127">Right-click the `TypeScript` directory and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="9af56-128">Выберите элемент **файл JavaScript** и назовите файл *предпочтений. TS* (Обратите внимание на расширение \*. TS).</span><span class="sxs-lookup"><span data-stu-id="9af56-128">Select the **JavaScript file** item and name the file *Tastes.ts* (note the \*.ts extension).</span></span> <span data-ttu-id="9af56-129">Скопируйте строку кода TypeScript ниже в файл (при сохранении будет создан новый файл *предпочтений. js* с исходным кодом JavaScript).</span><span class="sxs-lookup"><span data-stu-id="9af56-129">Copy the line of TypeScript code below into the file (when you save, a new *Tastes.js* file will appear with the JavaScript source).</span></span>

    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7. <span data-ttu-id="9af56-130">Добавьте второй файл в каталог **TypeScript** и назовите его `Food.ts`.</span><span class="sxs-lookup"><span data-stu-id="9af56-130">Add a second file to the **TypeScript** directory and name it `Food.ts`.</span></span> <span data-ttu-id="9af56-131">Скопируйте приведенный ниже код в файл.</span><span class="sxs-lookup"><span data-stu-id="9af56-131">Copy the code below into the file.</span></span>

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

## <a name="configuring-npm"></a><span data-ttu-id="9af56-132">Настройка NPM</span><span class="sxs-lookup"><span data-stu-id="9af56-132">Configuring NPM</span></span>

<span data-ttu-id="9af56-133">Затем настройте NPM для загрузки grunt и grunt-Tasks.</span><span class="sxs-lookup"><span data-stu-id="9af56-133">Next, configure NPM to download grunt and grunt-tasks.</span></span>

1. <span data-ttu-id="9af56-134">В обозреватель решений щелкните правой кнопкой мыши проект и выберите в контекстном меню команду **добавить > новый элемент** .</span><span class="sxs-lookup"><span data-stu-id="9af56-134">In the Solution Explorer, right-click the project and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="9af56-135">Выберите элемент **файл конфигурации NPM** , оставьте имя файла *Package. JSON*по умолчанию и нажмите кнопку **добавить** .</span><span class="sxs-lookup"><span data-stu-id="9af56-135">Select the **NPM configuration file** item, leave the default name, *package.json*, and click the **Add** button.</span></span>

2. <span data-ttu-id="9af56-136">В файле *Package. JSON* в фигурных скобках объекта `devDependencies` введите «grunt».</span><span class="sxs-lookup"><span data-stu-id="9af56-136">In the *package.json* file, inside the `devDependencies` object braces, enter "grunt".</span></span> <span data-ttu-id="9af56-137">Выберите `grunt` из списка IntelliSense и нажмите клавишу ВВОД.</span><span class="sxs-lookup"><span data-stu-id="9af56-137">Select `grunt` from the Intellisense list and press the Enter key.</span></span> <span data-ttu-id="9af56-138">Visual Studio поставит в кавычки имя пакета grunt и добавить двоеточие.</span><span class="sxs-lookup"><span data-stu-id="9af56-138">Visual Studio will quote the grunt package name, and add a colon.</span></span> <span data-ttu-id="9af56-139">Справа от двоеточия выберите последнюю стабильную версию пакета в верхней части списка IntelliSense (нажмите `Ctrl-Space`, если IntelliSense не отображается).</span><span class="sxs-lookup"><span data-stu-id="9af56-139">To the right of the colon, select the latest stable version of the package from the top of the Intellisense list (press `Ctrl-Space` if Intellisense doesn't appear).</span></span>

    ![grunt IntelliSense](using-grunt/_static/devdependencies-grunt.png)

    > [!NOTE]
    > <span data-ttu-id="9af56-141">NPM использует [семантическое управление версиями](https://semver.org/) для Организации зависимостей.</span><span class="sxs-lookup"><span data-stu-id="9af56-141">NPM uses [semantic versioning](https://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="9af56-142">Семантическое управление версиями, также известное как SemVer, определяет пакеты со схемой нумерации \<основном >.\<дополнительный >. >\<исправлений.</span><span class="sxs-lookup"><span data-stu-id="9af56-142">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="9af56-143">IntelliSense упрощает семантическое управление версиями, отображая лишь несколько распространенных вариантов.</span><span class="sxs-lookup"><span data-stu-id="9af56-143">Intellisense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="9af56-144">Верхний элемент в списке IntelliSense (0.4.5 в примере выше) считается последней стабильной версией пакета.</span><span class="sxs-lookup"><span data-stu-id="9af56-144">The top item in the Intellisense list (0.4.5 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="9af56-145">Символ каретки (^) соответствует самой последней основной версии, а тильда (~) соответствует самой последней дополнительной версии.</span><span class="sxs-lookup"><span data-stu-id="9af56-145">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span> <span data-ttu-id="9af56-146">Ознакомьтесь со [справочником по анализатору версий NPM semver](https://www.npmjs.com/package/semver) , как описано в полной версии выразительности, предоставляемой semver.</span><span class="sxs-lookup"><span data-stu-id="9af56-146">See the [NPM semver version parser reference](https://www.npmjs.com/package/semver) as a guide to the full expressivity that SemVer provides.</span></span>

3. <span data-ttu-id="9af56-147">Добавьте дополнительные зависимости для загрузки пакетов grunt-от участников сообщества-\* для *очистки*, *жшинт*, *concat*, *углифи*и *просмотра* , как показано в примере ниже.</span><span class="sxs-lookup"><span data-stu-id="9af56-147">Add more dependencies to load grunt-contrib-\* packages for *clean*, *jshint*, *concat*, *uglify*, and *watch* as shown in the example below.</span></span> <span data-ttu-id="9af56-148">Версии не обязательно должны соответствовать примеру.</span><span class="sxs-lookup"><span data-stu-id="9af56-148">The versions don't need to match the example.</span></span>

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

4. <span data-ttu-id="9af56-149">Сохраните файл *Package. JSON* .</span><span class="sxs-lookup"><span data-stu-id="9af56-149">Save the *package.json* file.</span></span>

<span data-ttu-id="9af56-150">Пакеты для каждого `devDependencies` элемента будут скачаны вместе с любыми файлами, которые требуются для каждого пакета.</span><span class="sxs-lookup"><span data-stu-id="9af56-150">The packages for each `devDependencies` item will download, along with any files that each package requires.</span></span> <span data-ttu-id="9af56-151">Файлы пакета можно найти в каталоге *node_modules* , включив кнопку " **Показывать все файлы** " в **Обозреватель решений**.</span><span class="sxs-lookup"><span data-stu-id="9af56-151">You can find the package files in the *node_modules* directory by enabling the **Show All Files** button in **Solution Explorer**.</span></span>

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> <span data-ttu-id="9af56-153">При необходимости можно вручную восстановить зависимости в **Обозреватель решений** , щелкнув `Dependencies\NPM` правой кнопкой мыши и выбрав пункт меню **восстановить пакеты** .</span><span class="sxs-lookup"><span data-stu-id="9af56-153">If you need to, you can manually restore dependencies in **Solution Explorer** by right-clicking on `Dependencies\NPM` and selecting the **Restore Packages** menu option.</span></span>

![восстановление пакетов](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a><span data-ttu-id="9af56-155">Настройка grunt</span><span class="sxs-lookup"><span data-stu-id="9af56-155">Configuring Grunt</span></span>

<span data-ttu-id="9af56-156">Grunt настраивается с помощью манифеста с именем *грунтфиле. js* , который определяет, загружает и регистрирует задачи, которые могут выполняться вручную или настроены для автоматического запуска на основе событий в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9af56-156">Grunt is configured using a manifest named *Gruntfile.js* that defines, loads and registers tasks that can be run manually or configured to run automatically based on events in Visual Studio.</span></span>

1. <span data-ttu-id="9af56-157">Щелкните правой кнопкой мыши проект и выберите **добавить** > **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="9af56-157">Right-click the project and select **Add** > **New Item**.</span></span> <span data-ttu-id="9af56-158">Выберите шаблон элемента **файла JavaScript** , измените имя на *грунтфиле. js*и нажмите кнопку **добавить** .</span><span class="sxs-lookup"><span data-stu-id="9af56-158">Select the **JavaScript File** item template, change the name to *Gruntfile.js*, and click the **Add** button.</span></span>

1. <span data-ttu-id="9af56-159">Добавьте следующий код в *грунтфиле. js*.</span><span class="sxs-lookup"><span data-stu-id="9af56-159">Add the following code to *Gruntfile.js*.</span></span> <span data-ttu-id="9af56-160">Функция `initConfig` задает параметры для каждого пакета, а оставшаяся часть модуля загружает и регистрирует задачи.</span><span class="sxs-lookup"><span data-stu-id="9af56-160">The `initConfig` function sets options for each package, and the remainder of the module loads and register tasks.</span></span>

   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

1. <span data-ttu-id="9af56-161">В функции `initConfig` добавьте параметры для задачи `clean`, как показано в примере *грунтфиле. js* ниже.</span><span class="sxs-lookup"><span data-stu-id="9af56-161">Inside the `initConfig` function, add options for the `clean` task as shown in the example *Gruntfile.js* below.</span></span> <span data-ttu-id="9af56-162">Задача `clean` принимает массив строк каталога.</span><span class="sxs-lookup"><span data-stu-id="9af56-162">The `clean` task accepts an array of directory strings.</span></span> <span data-ttu-id="9af56-163">Эта задача удаляет файлы из *wwwroot/lib* и удаляет весь каталог */TEMP* .</span><span class="sxs-lookup"><span data-stu-id="9af56-163">This task removes files from *wwwroot/lib* and removes the entire */temp* directory.</span></span>

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

1. <span data-ttu-id="9af56-164">Под функцией `initConfig` добавьте вызов `grunt.loadNpmTasks`.</span><span class="sxs-lookup"><span data-stu-id="9af56-164">Below the `initConfig` function, add a call to `grunt.loadNpmTasks`.</span></span> <span data-ttu-id="9af56-165">Это сделает задачу готовой к запуску из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9af56-165">This will make the task runnable from Visual Studio.</span></span>

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

1. <span data-ttu-id="9af56-166">Сохраните *грунтфиле. js*.</span><span class="sxs-lookup"><span data-stu-id="9af56-166">Save *Gruntfile.js*.</span></span> <span data-ttu-id="9af56-167">Файл должен выглядеть примерно так, как показано на снимке экрана ниже.</span><span class="sxs-lookup"><span data-stu-id="9af56-167">The file should look something like the screenshot below.</span></span>

    ![Начальная грунтфиле](using-grunt/_static/gruntfile-js-initial.png)

1. <span data-ttu-id="9af56-169">Щелкните правой кнопкой мыши *грунтфиле. js* и выберите **Task Runner Explorer (обозреватель выполнения задач** ) в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="9af56-169">Right-click *Gruntfile.js* and select **Task Runner Explorer** from the context menu.</span></span> <span data-ttu-id="9af56-170">Откроется окно **Обозреватель выполнения задач** .</span><span class="sxs-lookup"><span data-stu-id="9af56-170">The **Task Runner Explorer** window will open.</span></span>

    ![меню обозревателя выполнения задач](using-grunt/_static/task-runner-explorer-menu.png)

1. <span data-ttu-id="9af56-172">Убедитесь, что `clean` отображается в разделе **задачи** в обозревателе средств выполнения **задач**.</span><span class="sxs-lookup"><span data-stu-id="9af56-172">Verify that `clean` shows under **Tasks** in the **Task Runner Explorer**.</span></span>

    ![Список задач обозревателя выполнения задач](using-grunt/_static/task-runner-explorer-tasks.png)

1. <span data-ttu-id="9af56-174">Щелкните правой кнопкой мыши задачу очистить и выберите пункт **выполнить** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="9af56-174">Right-click the clean task and select **Run** from the context menu.</span></span> <span data-ttu-id="9af56-175">В командном окне отображается ход выполнения задачи.</span><span class="sxs-lookup"><span data-stu-id="9af56-175">A command window displays progress of the task.</span></span>

    ![Обозреватель выполнения задач "выполнить задачу очистки"](using-grunt/_static/task-runner-explorer-run-clean.png)

    > [!NOTE]
    > <span data-ttu-id="9af56-177">Нет файлов или каталогов для очистки.</span><span class="sxs-lookup"><span data-stu-id="9af56-177">There are no files or directories to clean yet.</span></span> <span data-ttu-id="9af56-178">При желании вы можете вручную создать их в обозреватель решений а затем запустить задачу очистки в качестве теста.</span><span class="sxs-lookup"><span data-stu-id="9af56-178">If you like, you can manually create them in the Solution Explorer and then run the clean task as a test.</span></span>

1. <span data-ttu-id="9af56-179">В функции `initConfig` добавьте запись для `concat`, используя приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="9af56-179">In the `initConfig` function, add an entry for `concat` using the code below.</span></span>

    <span data-ttu-id="9af56-180">Массив свойств `src` перечисляет файлы, которые нужно объединить, в том порядке, в котором они должны быть объединены.</span><span class="sxs-lookup"><span data-stu-id="9af56-180">The `src` property array lists files to combine, in the order that they should be combined.</span></span> <span data-ttu-id="9af56-181">Свойство `dest` присваивает путь к созданному Объединенному файлу.</span><span class="sxs-lookup"><span data-stu-id="9af56-181">The `dest` property assigns the path to the combined file that's produced.</span></span>

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="9af56-182">Свойство `all` в приведенном выше коде является именем целевого объекта.</span><span class="sxs-lookup"><span data-stu-id="9af56-182">The `all` property in the code above is the name of a target.</span></span> <span data-ttu-id="9af56-183">Целевые объекты используются в некоторых задачах grunt для разрешения нескольких сред сборки.</span><span class="sxs-lookup"><span data-stu-id="9af56-183">Targets are used in some Grunt tasks to allow multiple build environments.</span></span> <span data-ttu-id="9af56-184">Вы можете просматривать встроенные целевые объекты с помощью IntelliSense или назначать собственные.</span><span class="sxs-lookup"><span data-stu-id="9af56-184">You can view the built-in targets using IntelliSense or assign your own.</span></span>

1. <span data-ttu-id="9af56-185">Добавьте `jshint` задачу, используя приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="9af56-185">Add the `jshint` task using the code below.</span></span>

    <span data-ttu-id="9af56-186">Служебная программа жшинт `code-quality` выполняется для каждого файла JavaScript, найденного во *временном* каталоге.</span><span class="sxs-lookup"><span data-stu-id="9af56-186">The jshint `code-quality` utility is run against every JavaScript file found in the *temp* directory.</span></span>

    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="9af56-187">Параметр "-W069" является ошибкой, созданной жшинт, когда JavaScript использует синтаксис скобок для назначения свойства вместо точечной нотации, т. е. `Tastes["Sweet"]` вместо `Tastes.Sweet`.</span><span class="sxs-lookup"><span data-stu-id="9af56-187">The option "-W069" is an error produced by jshint when JavaScript uses bracket syntax to assign a property instead of dot notation, i.e. `Tastes["Sweet"]` instead of `Tastes.Sweet`.</span></span> <span data-ttu-id="9af56-188">Параметр отключает предупреждение, разрешающее продолжение оставшейся части процесса.</span><span class="sxs-lookup"><span data-stu-id="9af56-188">The option turns off the warning to allow the rest of the process to continue.</span></span>

1. <span data-ttu-id="9af56-189">Добавьте `uglify` задачу, используя приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="9af56-189">Add the `uglify` task using the code below.</span></span>

    <span data-ttu-id="9af56-190">Задача уменьшает *Объединенный JS* -файл, находящийся в каталоге Temp, и создает файл результатов в wwwroot/lib, следуя стандартному соглашению об именовании *\<имя файла\>. min. js*.</span><span class="sxs-lookup"><span data-stu-id="9af56-190">The task minifies the *combined.js* file found in the temp directory and creates the result file in wwwroot/lib following the standard naming convention *\<file name\>.min.js*.</span></span>

    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

1. <span data-ttu-id="9af56-191">При вызове `grunt.loadNpmTasks`, который загружает `grunt-contrib-clean`, включите тот же вызов жшинт, concat и углифи, используя приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="9af56-191">Under the call to `grunt.loadNpmTasks` that loads `grunt-contrib-clean`, include the same call for jshint, concat, and uglify using the code below.</span></span>

    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

1. <span data-ttu-id="9af56-192">Сохраните *грунтфиле. js*.</span><span class="sxs-lookup"><span data-stu-id="9af56-192">Save *Gruntfile.js*.</span></span> <span data-ttu-id="9af56-193">Файл должен выглядеть примерно так, как показано в примере ниже.</span><span class="sxs-lookup"><span data-stu-id="9af56-193">The file should look something like the example below.</span></span>

    ![Полный пример файла grunt](using-grunt/_static/gruntfile-js-complete.png)

1. <span data-ttu-id="9af56-195">Обратите внимание, что список задачи **обозревателя выполнения задач** содержит `clean`, `concat`, `jshint` и `uglify` задач.</span><span class="sxs-lookup"><span data-stu-id="9af56-195">Notice that the **Task Runner Explorer** Tasks list includes `clean`, `concat`, `jshint` and `uglify` tasks.</span></span> <span data-ttu-id="9af56-196">Выполните каждую задачу по порядку и просмотрите результаты в **Обозреватель решений**.</span><span class="sxs-lookup"><span data-stu-id="9af56-196">Run each task in order and observe the results in **Solution Explorer**.</span></span> <span data-ttu-id="9af56-197">Каждая задача должна выполняться без ошибок.</span><span class="sxs-lookup"><span data-stu-id="9af56-197">Each task should run without errors.</span></span>

    ![Обозреватель выполнения задач выполнение каждой задачи](using-grunt/_static/task-runner-explorer-run-each-task.png)

    <span data-ttu-id="9af56-199">Задача Concat создает новый *Объединенный JS* -файл и помещает его во временный каталог.</span><span class="sxs-lookup"><span data-stu-id="9af56-199">The concat task creates a new *combined.js* file and places it into the temp directory.</span></span> <span data-ttu-id="9af56-200">Задача `jshint` просто выполняется и не создает выходные данные.</span><span class="sxs-lookup"><span data-stu-id="9af56-200">The `jshint` task simply runs and doesn't produce output.</span></span> <span data-ttu-id="9af56-201">Задача `uglify` создает новый *объединенный файл. min. js* и помещает его в *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="9af56-201">The `uglify` task creates a new *combined.min.js* file and places it into *wwwroot/lib*.</span></span> <span data-ttu-id="9af56-202">По завершении решение должно выглядеть примерно так, как показано на снимке экрана ниже:</span><span class="sxs-lookup"><span data-stu-id="9af56-202">On completion, the solution should look something like the screenshot below:</span></span>

    ![Обозреватель решений после всех задач](using-grunt/_static/solution-explorer-after-all-tasks.png)

    > [!NOTE]
    > <span data-ttu-id="9af56-204">Дополнительные сведения о параметрах для каждого пакета см. в [https://www.npmjs.com/](https://www.npmjs.com/) и поиске имени пакета в поле поиска на главной странице.</span><span class="sxs-lookup"><span data-stu-id="9af56-204">For more information on the options for each package, visit [https://www.npmjs.com/](https://www.npmjs.com/) and lookup the package name in the search box on the main page.</span></span> <span data-ttu-id="9af56-205">Например, можно найти пакет grunt-от участников сообщества-Clean, чтобы получить ссылку на документацию с описанием всех ее параметров.</span><span class="sxs-lookup"><span data-stu-id="9af56-205">For example, you can look up the grunt-contrib-clean package to get a documentation link that explains all of its parameters.</span></span>

### <a name="all-together-now"></a><span data-ttu-id="9af56-206">Итоги</span><span class="sxs-lookup"><span data-stu-id="9af56-206">All together now</span></span>

<span data-ttu-id="9af56-207">Используйте метод `registerTask()` grunt для выполнения ряда задач в определенной последовательности.</span><span class="sxs-lookup"><span data-stu-id="9af56-207">Use the Grunt `registerTask()` method to run a series of tasks in a particular sequence.</span></span> <span data-ttu-id="9af56-208">Например, чтобы выполнить приведенные выше примеры шагов в разделе Порядок очистки > Concat-> жшинт-> углифи, добавьте приведенный ниже код в модуль.</span><span class="sxs-lookup"><span data-stu-id="9af56-208">For example, to run the example steps above in the order clean -> concat -> jshint -> uglify, add the code below to the module.</span></span> <span data-ttu-id="9af56-209">Код должен быть добавлен на тот же уровень, что и вызовы Лоаднпмтаскс (), за пределами Инитконфиг.</span><span class="sxs-lookup"><span data-stu-id="9af56-209">The code should be added to the same level as the loadNpmTasks() calls, outside initConfig.</span></span>

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

<span data-ttu-id="9af56-210">Новая задача отображается в обозревателе выполнения задач в разделе задачи псевдонима.</span><span class="sxs-lookup"><span data-stu-id="9af56-210">The new task shows up in Task Runner Explorer under Alias Tasks.</span></span> <span data-ttu-id="9af56-211">Можно щелкнуть правой кнопкой мыши и запустить его так же, как и другие задачи.</span><span class="sxs-lookup"><span data-stu-id="9af56-211">You can right-click and run it just as you would other tasks.</span></span> <span data-ttu-id="9af56-212">Задача `all` будет выполняться `clean`, `concat`, `jshint` и `uglify`по порядку.</span><span class="sxs-lookup"><span data-stu-id="9af56-212">The `all` task will run `clean`, `concat`, `jshint` and `uglify`, in order.</span></span>

![псевдонимы задач grunt](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a><span data-ttu-id="9af56-214">Просмотр изменений</span><span class="sxs-lookup"><span data-stu-id="9af56-214">Watching for changes</span></span>

<span data-ttu-id="9af56-215">Задача `watch` следит за файлами и каталогами.</span><span class="sxs-lookup"><span data-stu-id="9af56-215">A `watch` task keeps an eye on files and directories.</span></span> <span data-ttu-id="9af56-216">Контрольное значение активирует задачи автоматически, если обнаруживает изменения.</span><span class="sxs-lookup"><span data-stu-id="9af56-216">The watch triggers tasks automatically if it detects changes.</span></span> <span data-ttu-id="9af56-217">Добавьте приведенный ниже код в Инитконфиг для отслеживания изменений в файлах \*. js в каталоге TypeScript.</span><span class="sxs-lookup"><span data-stu-id="9af56-217">Add the code below to initConfig to watch for changes to \*.js files in the TypeScript directory.</span></span> <span data-ttu-id="9af56-218">При изменении файла JavaScript `watch` запустит задачу `all`.</span><span class="sxs-lookup"><span data-stu-id="9af56-218">If a JavaScript file is changed, `watch` will run the `all` task.</span></span>

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

<span data-ttu-id="9af56-219">Добавьте вызов `loadNpmTasks()`, чтобы отобразить задачу `watch` в обозревателе выполнения задач.</span><span class="sxs-lookup"><span data-stu-id="9af56-219">Add a call to `loadNpmTasks()` to show the `watch` task in Task Runner Explorer.</span></span>

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

<span data-ttu-id="9af56-220">Щелкните правой кнопкой мыши задачу контрольные значения в обозревателе выполнения задач и выберите пункт запустить в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="9af56-220">Right-click the watch task in Task Runner Explorer and select Run from the context menu.</span></span> <span data-ttu-id="9af56-221">В командном окне, отображающем выполняемую задачу Watch, будет отображаться "ожидание..." Сообщение.</span><span class="sxs-lookup"><span data-stu-id="9af56-221">The command window that shows the watch task running will display a "Waiting…" message.</span></span> <span data-ttu-id="9af56-222">Откройте один из файлов TypeScript, добавьте пробел, а затем сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="9af56-222">Open one of the TypeScript files, add a space, and then save the file.</span></span> <span data-ttu-id="9af56-223">Это приведет к активации задачи наблюдения и активации выполнения других задач в указанном порядке.</span><span class="sxs-lookup"><span data-stu-id="9af56-223">This will trigger the watch task and trigger the other tasks to run in order.</span></span> <span data-ttu-id="9af56-224">На следующем снимке экрана показан пример запуска.</span><span class="sxs-lookup"><span data-stu-id="9af56-224">The screenshot below shows a sample run.</span></span>

![выходные данные выполняемых задач](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a><span data-ttu-id="9af56-226">Привязка к событиям Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9af56-226">Binding to Visual Studio events</span></span>

<span data-ttu-id="9af56-227">Если вы не хотите запускать задачи вручную при каждой работе в Visual Studio, привяжите задачи **перед сборкой**, после событий **сборки**, **очистки**и **открытия проектов** .</span><span class="sxs-lookup"><span data-stu-id="9af56-227">Unless you want to manually start your tasks every time you work in Visual Studio, bind tasks to **Before Build**, **After Build**, **Clean**, and **Project Open** events.</span></span>

<span data-ttu-id="9af56-228">Привяжите `watch`, чтобы он выполнялся при каждом открытии Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9af56-228">Bind `watch` so that it runs every time Visual Studio opens.</span></span> <span data-ttu-id="9af56-229">В обозревателе выполнения задач щелкните правой кнопкой мыши задачу контрольные значения и выберите пункт **привязки** > **Открыть проект** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="9af56-229">In Task Runner Explorer, right-click the watch task and select **Bindings** > **Project Open** from the context menu.</span></span>

![Привязка задачи к открытию проекта](using-grunt/_static/bindings-project-open.png)

<span data-ttu-id="9af56-231">Выгрузите и перезагрузите проект.</span><span class="sxs-lookup"><span data-stu-id="9af56-231">Unload and reload the project.</span></span> <span data-ttu-id="9af56-232">При повторной загрузке проекта задача Watch начинает выполняться автоматически.</span><span class="sxs-lookup"><span data-stu-id="9af56-232">When the project loads again, the watch task starts running automatically.</span></span>

## <a name="summary"></a><span data-ttu-id="9af56-233">Сводка</span><span class="sxs-lookup"><span data-stu-id="9af56-233">Summary</span></span>

<span data-ttu-id="9af56-234">Grunt — это мощный исполнитель задач, который можно использовать для автоматизации большинства задач сборки клиента.</span><span class="sxs-lookup"><span data-stu-id="9af56-234">Grunt is a powerful task runner that can be used to automate most client-build tasks.</span></span> <span data-ttu-id="9af56-235">Grunt использует NPM для доставки своих пакетов, а также обеспечивает интеграцию средств с Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9af56-235">Grunt leverages NPM to deliver its packages, and features tooling integration with Visual Studio.</span></span> <span data-ttu-id="9af56-236">Обозреватель выполнения задач Visual Studio обнаруживает изменения в файлах конфигурации и предоставляет удобный интерфейс для выполнения задач, просмотра выполняемых задач и привязки задач к событиям Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9af56-236">Visual Studio's Task Runner Explorer detects changes to configuration files and provides a convenient interface to run tasks, view running tasks, and bind tasks to Visual Studio events.</span></span>
