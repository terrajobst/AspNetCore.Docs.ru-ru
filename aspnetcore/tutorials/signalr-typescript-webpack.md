---
title: Использование ASP.NET Core SignalR с TypeScript и Webpack
author: ssougnez
description: В рамках этого учебника вы настроите средство Webpack для создания пакета и сборки веб-приложения ASP.NET Core SignalR, клиент которого написан на языке TypeScript.
ms.author: bradyg
ms.custom: mvc
ms.date: 04/23/2019
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: 628fbb9940ad14cb15e3abd88b8b6a524b24d70a
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773949"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a><span data-ttu-id="a8f0d-103">Использование ASP.NET Core SignalR с TypeScript и Webpack</span><span class="sxs-lookup"><span data-stu-id="a8f0d-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="a8f0d-104">Авторы: [Себастьен Сунье (Sébastien Sougnez)](https://twitter.com/ssougnez) и [Скотт Эдди (Scott Addie)](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="a8f0d-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="a8f0d-105">С помощью средства [Webpack](https://webpack.js.org/) разработчики могут создавать пакеты и выполнять сборку ресурсов на стороне клиента для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="a8f0d-106">В этом руководстве демонстрируется использование средства Webpack для создания пакета и сборки веб-приложения ASP.NET Core SignalR, клиент которого написан на языке [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="a8f0d-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="a8f0d-107">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a8f0d-108">Сформировать шаблон для начального приложения ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="a8f0d-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="a8f0d-109">Настроить клиент TypeScript SignalR</span><span class="sxs-lookup"><span data-stu-id="a8f0d-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="a8f0d-110">Настроить конвейер сборки с использованием Webpack</span><span class="sxs-lookup"><span data-stu-id="a8f0d-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="a8f0d-111">Настроить сервер SignalR</span><span class="sxs-lookup"><span data-stu-id="a8f0d-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="a8f0d-112">Обеспечить взаимодействие между клиентом и сервером</span><span class="sxs-lookup"><span data-stu-id="a8f0d-112">Enable communication between client and server</span></span>

<span data-ttu-id="a8f0d-113">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a8f0d-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="a8f0d-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="a8f0d-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8f0d-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8f0d-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a8f0d-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) с рабочей нагрузкой **ASP.NET и веб-разработка**</span><span class="sxs-lookup"><span data-stu-id="a8f0d-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="a8f0d-117">Пакет SDK для .NET Core 3.0 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="a8f0d-117">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="a8f0d-118">[Node.js](https://nodejs.org/) с [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="a8f0d-118">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8f0d-119">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="a8f0d-120">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="a8f0d-121">Пакет SDK для .NET Core 3.0 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="a8f0d-121">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="a8f0d-122">[C# для Visual Studio Code версии 1.17.1 или более поздней](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="a8f0d-122">[C# for Visual Studio Code version 1.17.1 or later](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>
* <span data-ttu-id="a8f0d-123">[Node.js](https://nodejs.org/) с [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="a8f0d-123">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="a8f0d-124">Создание веб-приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8f0d-124">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8f0d-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8f0d-125">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a8f0d-126">Настройте Visual Studio на поиск npm в переменной среды *PATH*.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-126">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="a8f0d-127">По умолчанию Visual Studio использует версию npm, которая находится в его каталоге установки.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-127">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="a8f0d-128">В Visual Studio выполните следующие инструкции:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-128">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="a8f0d-129">Выберите **Сервис** > **Параметры** > **Проекты и решения** > **Управление веб-пакетами** > **Внешние веб-инструменты**.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-129">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="a8f0d-130">Выберите элемент *$(PATH)* в списке.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-130">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="a8f0d-131">Щелкните стрелку вверх, чтобы переместить этот элемент на вторую позицию в списке.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-131">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Конфигурация Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="a8f0d-133">Настройка Visual Studio завершена.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-133">Visual Studio configuration is completed.</span></span> <span data-ttu-id="a8f0d-134">Теперь следует создать проект.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-134">It's time to create the project.</span></span>

1. <span data-ttu-id="a8f0d-135">Перейдите в меню **Файл** > **Создать** > **Проект** и выберите шаблон **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-135">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="a8f0d-136">Присвойте проекту имя *SignalRWebPack* и щелкните **Создать**.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-136">Name the project *SignalRWebPack*, and select **Create**.</span></span>
1. <span data-ttu-id="a8f0d-137">Выберите *.NET Core* в раскрывающемся списке целевых платформ и *ASP.NET Core 3.0* в раскрывающемся списке версий платформ.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-137">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 3.0* from the framework selector drop-down.</span></span> <span data-ttu-id="a8f0d-138">Выберите шаблон **Пустой** и щелкните **Создать**.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-138">Select the **Empty** template, and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8f0d-139">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-139">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a8f0d-140">Во **встроенном терминале** выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-140">Run the following command in the **Integrated Terminal**:</span></span>

```console
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="a8f0d-141">В каталоге *SignalRWebPack* создается пустое веб-приложение ASP.NET Core, ориентированное на платформу .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-141">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="a8f0d-142">Настройка Webpack и TypeScript</span><span class="sxs-lookup"><span data-stu-id="a8f0d-142">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="a8f0d-143">Далее следует настроить преобразование TypeScript в JavaScript и создание пакета ресурсов на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-143">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="a8f0d-144">В корневом элементе проекта выполните следующую команду, чтобы создать файл *package.json*:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-144">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="a8f0d-145">Добавьте выделенное свойство в файл *package.json*:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-145">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/3.x/snippets/package1.json?highlight=4)]

    <span data-ttu-id="a8f0d-146">Если свойству `private` присвоено значение `true`, на следующем шаге не будут отображаться предупреждения об установке пакета.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-146">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="a8f0d-147">Установите необходимые пакеты npm.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-147">Install the required npm packages.</span></span> <span data-ttu-id="a8f0d-148">Выполните следующую команду в корневом элементе проекта:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-148">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="a8f0d-149">Сведения о команде, на которые следует обратить внимание:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-149">Some command details to note:</span></span>

    * <span data-ttu-id="a8f0d-150">Для каждого имени пакета номер версии указывается после знака `@`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-150">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="a8f0d-151">npm устанавливает указанные версии пакета.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-151">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="a8f0d-152">Использование параметра `-E` позволяет отключить установленное по умолчанию поведение npm, предусматривающее запись операторов диапазона [семантического управления версиями](https://semver.org/) в файл *package.json*.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-152">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="a8f0d-153">Например, `"webpack": "4.29.3"` используется вместо `"webpack": "^4.29.3"`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-153">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="a8f0d-154">Этот параметр позволяет исключить непреднамеренное обновление до более новых версий пакета.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-154">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="a8f0d-155">Дополнительные сведения см. в официальной документации по [npm-install](https://docs.npmjs.com/cli/install).</span><span class="sxs-lookup"><span data-stu-id="a8f0d-155">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="a8f0d-156">Замените свойство `scripts` в файле *package.json* следующим фрагментом кода:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-156">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="a8f0d-157">Некоторые пояснения к скриптам:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-157">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="a8f0d-158">`build`: создает пакет ваших ресурсов на стороне клиента в режиме разработки и отслеживает изменения.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-158">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="a8f0d-159">Наблюдатель за файлами выполняет повторное создание пакета при каждом изменении файла проекта.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-159">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="a8f0d-160">Параметр `mode` позволяет отключить оптимизации в рабочей среде, такие как встряхивание дерева и минификация.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-160">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="a8f0d-161">В среде разработки следует использовать только `build`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-161">Only use `build` in development.</span></span>
    * <span data-ttu-id="a8f0d-162">`release`: создает пакет ресурсов на стороне клиента в рабочем режиме.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-162">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="a8f0d-163">`publish`: запускает скрипт `release` для создания пакета ресурсов на стороне клиента в рабочем режиме.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-163">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="a8f0d-164">Этот скрипт вызывает команду [publish](/dotnet/core/tools/dotnet-publish) интерфейса командной строки .NET Core для публикации приложения.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-164">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="a8f0d-165">Создайте в корневом элементе проекта файл *webpack.config.js* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-165">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/3.x/webpack.config.js)]

    <span data-ttu-id="a8f0d-166">Приведенный выше файл определяет конфигурацию компиляции Webpack.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-166">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="a8f0d-167">Сведения о конфигурации, на которые следует обратить внимание:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-167">Some configuration details to note:</span></span>

    * <span data-ttu-id="a8f0d-168">Свойство `output` переопределяет значение по умолчанию для *dist*.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-168">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="a8f0d-169">Вместо этого пакет выводится в каталог *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-169">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="a8f0d-170">Массив `resolve.extensions` содержит элемент *.js* для импорта кода JavaScript клиента SignalR.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-170">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="a8f0d-171">Создайте новый каталог *src* в корневом элементе проекта.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-171">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="a8f0d-172">В этом каталоге будут храниться ресурсы на стороне клиента для проекта.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-172">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="a8f0d-173">Создайте файл *src/index.html* со следующим содержимым.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-173">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/3.x/src/index.html)]

    <span data-ttu-id="a8f0d-174">Приведенный выше HTML-файл определяет стереотипную разметку главной страницы.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-174">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="a8f0d-175">Создайте каталог *src/css*.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-175">Create a new *src/css* directory.</span></span> <span data-ttu-id="a8f0d-176">В нем будут храниться файлы *.css* проекта.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-176">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="a8f0d-177">Создайте файл *src/css/main.css* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-177">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/3.x/src/css/main.css)]

    <span data-ttu-id="a8f0d-178">Приведенный выше файл *main.css* определяет стиль приложения.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-178">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="a8f0d-179">Создайте файл *src/tsconfig.json* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-179">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/3.x/src/tsconfig.json)]

    <span data-ttu-id="a8f0d-180">Приведенный выше код определяет конфигурацию компилятора TypeScript для получения совместимого с [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-180">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="a8f0d-181">Создайте файл *src/index.ts* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-181">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="a8f0d-182">Приведенный выше код TypeScript извлекает ссылки на элементы модели DOM и присоединяет два обработчика событий:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-182">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="a8f0d-183">`keyup`: это событие возникает в том случае, если пользователь вводит какой-либо текст в поле, идентифицированное как `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-183">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="a8f0d-184">Функция `send` вызывается, когда пользователь нажимает клавишу **ВВОД**.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-184">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="a8f0d-185">`click`: это событие возникает, когда пользователь нажимает кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-185">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="a8f0d-186">Вызывается функция `send`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-186">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="a8f0d-187">Настройка приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8f0d-187">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="a8f0d-188">В методе `Startup.Configure` добавьте вызовы к [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) и [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="a8f0d-188">In the `Startup.Configure` method, add calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseStaticDefaultFiles&highlight=2-3)]

   <span data-ttu-id="a8f0d-189">Приведенный выше код позволяет серверу обнаруживать и обрабатывать файл *index.html* независимо от того, вводит ли пользователь полный URL-адрес или URL-адрес корня веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-189">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="a8f0d-190">В конце метода `Startup.Configure` сопоставьте маршрут */hub* с концентратором `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-190">At the end of the `Startup.Configure` method, map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="a8f0d-191">Замените код, отображающий *Hello World!* ,</span><span class="sxs-lookup"><span data-stu-id="a8f0d-191">Replace the code that displays *Hello World!*</span></span> <span data-ttu-id="a8f0d-192">следующей строкой:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-192">with the following line:</span></span> 

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseSignalR&highlight=3)]

1. <span data-ttu-id="a8f0d-193">В методе `Startup.ConfigureServices` вызовите [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_).</span><span class="sxs-lookup"><span data-stu-id="a8f0d-193">In the `Startup.ConfigureServices` method, call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_).</span></span> <span data-ttu-id="a8f0d-194">Этот метод добавляет службы SignalR в проект.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-194">It adds the SignalR services to your project.</span></span>

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="a8f0d-195">Создайте новый каталог *Hubs* в корневом элементе проекта.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-195">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="a8f0d-196">Этот каталог служит для хранения концентратора SignalR, созданного на предыдущем шаге.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-196">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="a8f0d-197">Создайте концентратор *Hubs/ChatHub.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-197">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="a8f0d-198">Добавьте следующий код в самое начало файла *Startup.cs*, чтобы разрешить ссылку `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-198">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="a8f0d-199">Обеспечение взаимодействия между клиентом и сервером</span><span class="sxs-lookup"><span data-stu-id="a8f0d-199">Enable client and server communication</span></span>

<span data-ttu-id="a8f0d-200">На данный момент приложение отображает простую форму для отправки сообщений.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-200">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="a8f0d-201">При попытке сделать это ничего не происходит.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-201">Nothing happens when you try to do so.</span></span> <span data-ttu-id="a8f0d-202">Сервер прослушивает конкретный маршрут, но ничего не делает с отправленными сообщениями.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-202">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="a8f0d-203">Выполните следующую команду в корневом элементе проекта:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-203">Execute the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="a8f0d-204">Приведенная выше команда устанавливает [клиент TypeScript SignalR](https://www.npmjs.com/package/@aspnet/signalr), который позволяет клиенту отправлять сообщения на сервер.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-204">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@aspnet/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="a8f0d-205">Добавьте выделенный код в файл *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-205">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="a8f0d-206">Приведенный выше код поддерживает получение сообщений от сервера.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-206">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="a8f0d-207">Класс `HubConnectionBuilder` создает новый построитель для настройки подключения к серверу.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-207">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="a8f0d-208">Функция `withUrl` настраивает URL-адрес концентратора.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-208">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="a8f0d-209">SignalR обеспечивает обмен сообщениями между клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-209">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="a8f0d-210">Каждому сообщению присваивается определенное имя.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-210">Each message has a specific name.</span></span> <span data-ttu-id="a8f0d-211">Например, у вас могут быть сообщения с именем `messageReceived`, которые выполняют логику отображения новых сообщений в соответствующей зоне.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-211">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="a8f0d-212">Прослушивание определенных сообщений реализуется с помощью функции `on`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-212">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="a8f0d-213">Возможно прослушивание любого числа имен сообщений.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-213">You can listen to any number of message names.</span></span> <span data-ttu-id="a8f0d-214">Кроме того, можно передавать параметры сообщения, например имя его автора или содержимое полученного сообщения.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-214">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="a8f0d-215">После того как клиент получает сообщение, создается новый элемент `div`, в атрибуте `innerHTML` которого содержатся имя автора и содержимое сообщения.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-215">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="a8f0d-216">Этот элемент добавляется в основной элемент `div`, который используется для отображения сообщений.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-216">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="a8f0d-217">Теперь клиент может принимать сообщения. Далее следует настроить его для отправки сообщений.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-217">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="a8f0d-218">Добавьте выделенный код в файл *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-218">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/src/index.ts?highlight=34-35)]

    <span data-ttu-id="a8f0d-219">Для отправки сообщения через соединение WebSockets необходимо вызвать метод `send`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-219">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="a8f0d-220">Первый параметр этого метода содержит имя сообщения.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-220">The method's first parameter is the message name.</span></span> <span data-ttu-id="a8f0d-221">Другие параметры заполняются данными сообщения.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-221">The message data inhabits the other parameters.</span></span> <span data-ttu-id="a8f0d-222">В этом примере на сервер отправляется сообщение, идентифицированное как `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-222">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="a8f0d-223">Это сообщение содержит имя пользователя, а также данные, введенные этим пользователем в текстовое поле.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-223">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="a8f0d-224">Если отправка выполняется успешно, значение текстового поля очищается.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-224">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="a8f0d-225">Добавьте выделенный метод в класс `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-225">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="a8f0d-226">Приведенный выше код осуществляет широковещательную рассылку полученных сервером сообщений всем подключенным пользователям.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-226">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="a8f0d-227">Универсальный метод `on` для получения всех сообщений не требуется.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-227">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="a8f0d-228">Имя метода указывается после суффиксов имени сообщения.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-228">A method named after the message name suffices.</span></span>

    <span data-ttu-id="a8f0d-229">В этом примере клиент TypeScript отправляет сообщение, идентифицированное как `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-229">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="a8f0d-230">Метод C# `NewMessage` ожидает данные, отправленные клиентом.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-230">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="a8f0d-231">Выполняется вызов метода [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) для [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="a8f0d-231">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="a8f0d-232">Полученные сообщения отправляются всем клиентам, подключенным к концентратору.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-232">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="a8f0d-233">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="a8f0d-233">Test the app</span></span>

<span data-ttu-id="a8f0d-234">Чтобы проверить работоспособность приложения, выполните следующие шаги.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-234">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8f0d-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8f0d-235">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="a8f0d-236">Запустите средство Webpack в режиме *release*.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-236">Run Webpack in *release* mode.</span></span> <span data-ttu-id="a8f0d-237">Выполните следующие команды в окне **Консоль диспетчера пакетов** в корневом элементе проекта.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-237">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="a8f0d-238">Если вы не в корневом каталоге проекта, введите перед этой командой `cd SignalRWebPack`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-238">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="a8f0d-239">Выберите **Отладка** > **Запуск без отладки**, чтобы запустить приложение в браузере, не присоединяя отладчик.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-239">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="a8f0d-240">Файл *wwwroot/index.html* обрабатывается по адресу `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-240">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

   <span data-ttu-id="a8f0d-241">Если во время компиляции возникают ошибки, попробуйте закрыть и снова открыть решение.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-241">If you get compile errors, try closing and reopening the solution.</span></span> 

1. <span data-ttu-id="a8f0d-242">Откройте другой экземпляр любого браузера.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-242">Open another browser instance (any browser).</span></span> <span data-ttu-id="a8f0d-243">Вставьте URL-адрес в адресную строку.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-243">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="a8f0d-244">Выберите любой браузер, введите произвольный текст в поле **Сообщение** и нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-244">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="a8f0d-245">На обеих страницах мгновенно отображаются имя пользователя и сообщение.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-245">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8f0d-246">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="a8f0d-247">Запустите средство Webpack в режиме *release*, выполнив следующую команду в корневом элементе проекта:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-247">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="a8f0d-248">Выполните построение и запустите приложение, выполнив следующую команду в корневом элементе проекта:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-248">Build and run the app by executing the following command in the project root:</span></span>

    ```console
    dotnet run
    ```

    <span data-ttu-id="a8f0d-249">Веб-сервер запускает приложение и делает его доступным на узле localhost.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-249">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="a8f0d-250">Откройте браузер с адресом `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-250">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="a8f0d-251">Обрабатывается файл *wwwroot/index.html*.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-251">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="a8f0d-252">Скопируйте URL-адрес из адресной строки.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-252">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="a8f0d-253">Откройте другой экземпляр любого браузера.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-253">Open another browser instance (any browser).</span></span> <span data-ttu-id="a8f0d-254">Вставьте URL-адрес в адресную строку.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-254">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="a8f0d-255">Выберите любой браузер, введите произвольный текст в поле **Сообщение** и нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-255">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="a8f0d-256">На обеих страницах мгновенно отображаются имя пользователя и сообщение.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-256">The unique user name and message are displayed on both pages instantly.</span></span>

---

![Сообщение, отображаемое в обоих окнах браузера](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8f0d-258">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8f0d-258">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a8f0d-259">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) с рабочей нагрузкой **ASP.NET и веб-разработка**</span><span class="sxs-lookup"><span data-stu-id="a8f0d-259">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="a8f0d-260">Пакет SDK для .NET Core 2.2 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="a8f0d-260">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="a8f0d-261">[Node.js](https://nodejs.org/) с [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="a8f0d-261">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8f0d-262">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-262">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="a8f0d-263">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-263">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="a8f0d-264">Пакет SDK для .NET Core 2.2 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="a8f0d-264">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="a8f0d-265">[C# для Visual Studio Code версии 1.17.1 или более поздней](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="a8f0d-265">[C# for Visual Studio Code version 1.17.1 or later](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>
* <span data-ttu-id="a8f0d-266">[Node.js](https://nodejs.org/) с [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="a8f0d-266">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="a8f0d-267">Создание веб-приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8f0d-267">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8f0d-268">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8f0d-268">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a8f0d-269">Настройте Visual Studio на поиск npm в переменной среды *PATH*.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-269">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="a8f0d-270">По умолчанию Visual Studio использует версию npm, которая находится в его каталоге установки.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-270">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="a8f0d-271">В Visual Studio выполните следующие инструкции:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-271">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="a8f0d-272">Выберите **Сервис** > **Параметры** > **Проекты и решения** > **Управление веб-пакетами** > **Внешние веб-инструменты**.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-272">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="a8f0d-273">Выберите элемент *$(PATH)* в списке.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-273">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="a8f0d-274">Щелкните стрелку вверх, чтобы переместить этот элемент на вторую позицию в списке.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-274">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Конфигурация Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="a8f0d-276">Настройка Visual Studio завершена.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-276">Visual Studio configuration is completed.</span></span> <span data-ttu-id="a8f0d-277">Теперь следует создать проект.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-277">It's time to create the project.</span></span>

1. <span data-ttu-id="a8f0d-278">Перейдите в меню **Файл** > **Создать** > **Проект** и выберите шаблон **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-278">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="a8f0d-279">Присвойте проекту имя *SignalRWebPack* и щелкните **Создать**.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-279">Name the project *SignalRWebPack*, and select **Create**.</span></span>
1. <span data-ttu-id="a8f0d-280">Выберите *.NET Core* в раскрывающемся списке целевых платформ и выберите *ASP.NET Core 2.2* в раскрывающемся списке средства выбора платформы.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-280">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.2* from the framework selector drop-down.</span></span> <span data-ttu-id="a8f0d-281">Выберите шаблон **Пустой** и щелкните **Создать**.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-281">Select the **Empty** template, and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8f0d-282">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-282">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a8f0d-283">Во **встроенном терминале** выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-283">Run the following command in the **Integrated Terminal**:</span></span>

```console
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="a8f0d-284">В каталоге *SignalRWebPack* создается пустое веб-приложение ASP.NET Core, ориентированное на платформу .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-284">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="a8f0d-285">Настройка Webpack и TypeScript</span><span class="sxs-lookup"><span data-stu-id="a8f0d-285">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="a8f0d-286">Далее следует настроить преобразование TypeScript в JavaScript и создание пакета ресурсов на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-286">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="a8f0d-287">В корневом элементе проекта выполните следующую команду, чтобы создать файл *package.json*:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-287">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="a8f0d-288">Добавьте выделенное свойство в файл *package.json*:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-288">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/2.x/snippets/package1.json?highlight=4)]

    <span data-ttu-id="a8f0d-289">Если свойству `private` присвоено значение `true`, на следующем шаге не будут отображаться предупреждения об установке пакета.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-289">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="a8f0d-290">Установите необходимые пакеты npm.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-290">Install the required npm packages.</span></span> <span data-ttu-id="a8f0d-291">Выполните следующую команду в корневом элементе проекта:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-291">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="a8f0d-292">Сведения о команде, на которые следует обратить внимание:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-292">Some command details to note:</span></span>

    * <span data-ttu-id="a8f0d-293">Для каждого имени пакета номер версии указывается после знака `@`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-293">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="a8f0d-294">npm устанавливает указанные версии пакета.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-294">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="a8f0d-295">Использование параметра `-E` позволяет отключить установленное по умолчанию поведение npm, предусматривающее запись операторов диапазона [семантического управления версиями](https://semver.org/) в файл *package.json*.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-295">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="a8f0d-296">Например, `"webpack": "4.29.3"` используется вместо `"webpack": "^4.29.3"`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-296">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="a8f0d-297">Этот параметр позволяет исключить непреднамеренное обновление до более новых версий пакета.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-297">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="a8f0d-298">Дополнительные сведения см. в официальной документации по [npm-install](https://docs.npmjs.com/cli/install).</span><span class="sxs-lookup"><span data-stu-id="a8f0d-298">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="a8f0d-299">Замените свойство `scripts` в файле *package.json* следующим фрагментом кода:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-299">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="a8f0d-300">Некоторые пояснения к скриптам:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-300">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="a8f0d-301">`build`: создает пакет ваших ресурсов на стороне клиента в режиме разработки и отслеживает изменения.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-301">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="a8f0d-302">Наблюдатель за файлами выполняет повторное создание пакета при каждом изменении файла проекта.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-302">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="a8f0d-303">Параметр `mode` позволяет отключить оптимизации в рабочей среде, такие как встряхивание дерева и минификация.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-303">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="a8f0d-304">В среде разработки следует использовать только `build`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-304">Only use `build` in development.</span></span>
    * <span data-ttu-id="a8f0d-305">`release`: создает пакет ресурсов на стороне клиента в рабочем режиме.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-305">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="a8f0d-306">`publish`: запускает скрипт `release` для создания пакета ресурсов на стороне клиента в рабочем режиме.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-306">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="a8f0d-307">Этот скрипт вызывает команду [publish](/dotnet/core/tools/dotnet-publish) интерфейса командной строки .NET Core для публикации приложения.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-307">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="a8f0d-308">Создайте в корневом элементе проекта файл *webpack.config.js* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-308">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/2.x/webpack.config.js)]

    <span data-ttu-id="a8f0d-309">Приведенный выше файл определяет конфигурацию компиляции Webpack.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-309">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="a8f0d-310">Сведения о конфигурации, на которые следует обратить внимание:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-310">Some configuration details to note:</span></span>

    * <span data-ttu-id="a8f0d-311">Свойство `output` переопределяет значение по умолчанию для *dist*.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-311">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="a8f0d-312">Вместо этого пакет выводится в каталог *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-312">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="a8f0d-313">Массив `resolve.extensions` содержит элемент *.js* для импорта кода JavaScript клиента SignalR.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-313">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="a8f0d-314">Создайте новый каталог *src* в корневом элементе проекта.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-314">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="a8f0d-315">В этом каталоге будут храниться ресурсы на стороне клиента для проекта.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-315">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="a8f0d-316">Создайте файл *src/index.html* со следующим содержимым.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-316">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/2.x/src/index.html)]

    <span data-ttu-id="a8f0d-317">Приведенный выше HTML-файл определяет стереотипную разметку главной страницы.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-317">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="a8f0d-318">Создайте каталог *src/css*.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-318">Create a new *src/css* directory.</span></span> <span data-ttu-id="a8f0d-319">В нем будут храниться файлы *.css* проекта.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-319">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="a8f0d-320">Создайте файл *src/css/main.css* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-320">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/2.x/src/css/main.css)]

    <span data-ttu-id="a8f0d-321">Приведенный выше файл *main.css* определяет стиль приложения.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-321">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="a8f0d-322">Создайте файл *src/tsconfig.json* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-322">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/2.x/src/tsconfig.json)]

    <span data-ttu-id="a8f0d-323">Приведенный выше код определяет конфигурацию компилятора TypeScript для получения совместимого с [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-323">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="a8f0d-324">Создайте файл *src/index.ts* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-324">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="a8f0d-325">Приведенный выше код TypeScript извлекает ссылки на элементы модели DOM и присоединяет два обработчика событий:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-325">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="a8f0d-326">`keyup`: это событие возникает в том случае, если пользователь вводит какой-либо текст в поле, идентифицированное как `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-326">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="a8f0d-327">Функция `send` вызывается, когда пользователь нажимает клавишу **ВВОД**.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-327">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="a8f0d-328">`click`: это событие возникает, когда пользователь нажимает кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-328">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="a8f0d-329">Вызывается функция `send`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-329">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="a8f0d-330">Настройка приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8f0d-330">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="a8f0d-331">Код в методе `Startup.Configure` отображает текст *Hello World!* .</span><span class="sxs-lookup"><span data-stu-id="a8f0d-331">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="a8f0d-332">Замените вызов метода `app.Run` вызовами методов [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) и [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="a8f0d-332">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="a8f0d-333">Приведенный выше код позволяет серверу обнаруживать и обрабатывать файл *index.html* независимо от того, вводит ли пользователь полный URL-адрес или URL-адрес корня веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-333">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="a8f0d-334">Вызовите метод [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) в методе `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-334">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="a8f0d-335">Этот метод добавляет службы SignalR в проект.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-335">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="a8f0d-336">Сопоставьте маршрут */hub* с концентратором `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-336">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="a8f0d-337">Добавьте следующие строки в конец метода `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-337">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseSignalR)]

::: moniker-end

1. <span data-ttu-id="a8f0d-338">Создайте новый каталог *Hubs* в корневом элементе проекта.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-338">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="a8f0d-339">Этот каталог служит для хранения концентратора SignalR, созданного на предыдущем шаге.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-339">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="a8f0d-340">Создайте концентратор *Hubs/ChatHub.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-340">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="a8f0d-341">Добавьте следующий код в самое начало файла *Startup.cs*, чтобы разрешить ссылку `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-341">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="a8f0d-342">Обеспечение взаимодействия между клиентом и сервером</span><span class="sxs-lookup"><span data-stu-id="a8f0d-342">Enable client and server communication</span></span>

<span data-ttu-id="a8f0d-343">На данный момент приложение отображает простую форму для отправки сообщений.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-343">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="a8f0d-344">При попытке сделать это ничего не происходит.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-344">Nothing happens when you try to do so.</span></span> <span data-ttu-id="a8f0d-345">Сервер прослушивает конкретный маршрут, но ничего не делает с отправленными сообщениями.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-345">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="a8f0d-346">Выполните следующую команду в корневом элементе проекта:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-346">Execute the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="a8f0d-347">Приведенная выше команда устанавливает [клиент TypeScript SignalR](https://www.npmjs.com/package/@aspnet/signalr), который позволяет клиенту отправлять сообщения на сервер.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-347">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@aspnet/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="a8f0d-348">Добавьте выделенный код в файл *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-348">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="a8f0d-349">Приведенный выше код поддерживает получение сообщений от сервера.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-349">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="a8f0d-350">Класс `HubConnectionBuilder` создает новый построитель для настройки подключения к серверу.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-350">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="a8f0d-351">Функция `withUrl` настраивает URL-адрес концентратора.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-351">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="a8f0d-352">SignalR обеспечивает обмен сообщениями между клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-352">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="a8f0d-353">Каждому сообщению присваивается определенное имя.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-353">Each message has a specific name.</span></span> <span data-ttu-id="a8f0d-354">Например, у вас могут быть сообщения с именем `messageReceived`, которые выполняют логику отображения новых сообщений в соответствующей зоне.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-354">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="a8f0d-355">Прослушивание определенных сообщений реализуется с помощью функции `on`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-355">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="a8f0d-356">Возможно прослушивание любого числа имен сообщений.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-356">You can listen to any number of message names.</span></span> <span data-ttu-id="a8f0d-357">Кроме того, можно передавать параметры сообщения, например имя его автора или содержимое полученного сообщения.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-357">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="a8f0d-358">После того как клиент получает сообщение, создается новый элемент `div`, в атрибуте `innerHTML` которого содержатся имя автора и содержимое сообщения.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-358">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="a8f0d-359">Этот элемент добавляется в основной элемент `div`, который используется для отображения сообщений.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-359">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="a8f0d-360">Теперь клиент может принимать сообщения. Далее следует настроить его для отправки сообщений.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-360">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="a8f0d-361">Добавьте выделенный код в файл *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-361">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/src/index.ts?highlight=34-35)]

    <span data-ttu-id="a8f0d-362">Для отправки сообщения через соединение WebSockets необходимо вызвать метод `send`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-362">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="a8f0d-363">Первый параметр этого метода содержит имя сообщения.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-363">The method's first parameter is the message name.</span></span> <span data-ttu-id="a8f0d-364">Другие параметры заполняются данными сообщения.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-364">The message data inhabits the other parameters.</span></span> <span data-ttu-id="a8f0d-365">В этом примере на сервер отправляется сообщение, идентифицированное как `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-365">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="a8f0d-366">Это сообщение содержит имя пользователя, а также данные, введенные этим пользователем в текстовое поле.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-366">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="a8f0d-367">Если отправка выполняется успешно, значение текстового поля очищается.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-367">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="a8f0d-368">Добавьте выделенный метод в класс `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-368">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="a8f0d-369">Приведенный выше код осуществляет широковещательную рассылку полученных сервером сообщений всем подключенным пользователям.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-369">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="a8f0d-370">Универсальный метод `on` для получения всех сообщений не требуется.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-370">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="a8f0d-371">Имя метода указывается после суффиксов имени сообщения.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-371">A method named after the message name suffices.</span></span>

    <span data-ttu-id="a8f0d-372">В этом примере клиент TypeScript отправляет сообщение, идентифицированное как `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-372">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="a8f0d-373">Метод C# `NewMessage` ожидает данные, отправленные клиентом.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-373">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="a8f0d-374">Выполняется вызов метода [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) для [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="a8f0d-374">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="a8f0d-375">Полученные сообщения отправляются всем клиентам, подключенным к концентратору.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-375">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="a8f0d-376">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="a8f0d-376">Test the app</span></span>

<span data-ttu-id="a8f0d-377">Чтобы проверить работоспособность приложения, выполните следующие шаги.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-377">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8f0d-378">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8f0d-378">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="a8f0d-379">Запустите средство Webpack в режиме *release*.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-379">Run Webpack in *release* mode.</span></span> <span data-ttu-id="a8f0d-380">Выполните следующие команды в окне **Консоль диспетчера пакетов** в корневом элементе проекта.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-380">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="a8f0d-381">Если вы не в корневом каталоге проекта, введите перед этой командой `cd SignalRWebPack`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-381">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="a8f0d-382">Выберите **Отладка** > **Запуск без отладки**, чтобы запустить приложение в браузере, не присоединяя отладчик.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-382">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="a8f0d-383">Файл *wwwroot/index.html* обрабатывается по адресу `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-383">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="a8f0d-384">Откройте другой экземпляр любого браузера.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-384">Open another browser instance (any browser).</span></span> <span data-ttu-id="a8f0d-385">Вставьте URL-адрес в адресную строку.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-385">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="a8f0d-386">Выберите любой браузер, введите произвольный текст в поле **Сообщение** и нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-386">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="a8f0d-387">На обеих страницах мгновенно отображаются имя пользователя и сообщение.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-387">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8f0d-388">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-388">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="a8f0d-389">Запустите средство Webpack в режиме *release*, выполнив следующую команду в корневом элементе проекта:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-389">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="a8f0d-390">Выполните построение и запустите приложение, выполнив следующую команду в корневом элементе проекта:</span><span class="sxs-lookup"><span data-stu-id="a8f0d-390">Build and run the app by executing the following command in the project root:</span></span>

    ```console
    dotnet run
    ```

    <span data-ttu-id="a8f0d-391">Веб-сервер запускает приложение и делает его доступным на узле localhost.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-391">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="a8f0d-392">Откройте браузер с адресом `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-392">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="a8f0d-393">Обрабатывается файл *wwwroot/index.html*.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-393">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="a8f0d-394">Скопируйте URL-адрес из адресной строки.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-394">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="a8f0d-395">Откройте другой экземпляр любого браузера.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-395">Open another browser instance (any browser).</span></span> <span data-ttu-id="a8f0d-396">Вставьте URL-адрес в адресную строку.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-396">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="a8f0d-397">Выберите любой браузер, введите произвольный текст в поле **Сообщение** и нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-397">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="a8f0d-398">На обеих страницах мгновенно отображаются имя пользователя и сообщение.</span><span class="sxs-lookup"><span data-stu-id="a8f0d-398">The unique user name and message are displayed on both pages instantly.</span></span>

---

![Сообщение, отображаемое в обоих окнах браузера](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="a8f0d-400">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a8f0d-400">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
