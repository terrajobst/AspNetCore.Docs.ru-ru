---
title: Использование ASP.NET Core SignalR с TypeScript и webpack
author: ssougnez
description: В рамках этого учебника вы настроите средство webpack для создания пакета и сборки веб-приложения ASP.NET Core SignalR, клиент которого написан на языке TypeScript.
ms.author: bradyg
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- SignalR
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: 9094a1d391c087a6f58aa9dd66e3697a79f4af86
ms.sourcegitcommit: ef1720cb733908f36a54825d84c3461c5280bdbe
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2020
ms.locfileid: "75737520"
---
# <a name="use-aspnet-core-opno-locsignalr-with-typescript-and-webpack"></a><span data-ttu-id="6efc5-103">Использование ASP.NET Core SignalR с TypeScript и webpack</span><span class="sxs-lookup"><span data-stu-id="6efc5-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="6efc5-104">Авторы: [Себастьен Сунье (Sébastien Sougnez)](https://twitter.com/ssougnez) и [Скотт Эдди (Scott Addie)](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="6efc5-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="6efc5-105">С помощью средства [Webpack](https://webpack.js.org/) разработчики могут создавать пакеты и выполнять сборку ресурсов на стороне клиента для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="6efc5-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="6efc5-106">В этом учебнике демонстрируется использование средства webpack для создания пакета и сборки веб-приложения ASP.NET Core SignalR, клиент которого написан на языке [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="6efc5-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="6efc5-107">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="6efc5-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6efc5-108">сформировать шаблон для начального приложения ASP.NET Core SignalR;</span><span class="sxs-lookup"><span data-stu-id="6efc5-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="6efc5-109">настроить клиент TypeScript SignalR;</span><span class="sxs-lookup"><span data-stu-id="6efc5-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="6efc5-110">Настроить конвейер сборки с использованием Webpack</span><span class="sxs-lookup"><span data-stu-id="6efc5-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="6efc5-111">настроить сервер SignalR.</span><span class="sxs-lookup"><span data-stu-id="6efc5-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="6efc5-112">Обеспечить взаимодействие между клиентом и сервером</span><span class="sxs-lookup"><span data-stu-id="6efc5-112">Enable communication between client and server</span></span>

<span data-ttu-id="6efc5-113">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6efc5-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="6efc5-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="6efc5-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6efc5-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6efc5-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6efc5-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) с рабочей нагрузкой **ASP.NET и веб-разработка**</span><span class="sxs-lookup"><span data-stu-id="6efc5-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="6efc5-117">Пакет SDK для .NET Core 3.0 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="6efc5-117">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="6efc5-118">[Node.js](https://nodejs.org/) с [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="6efc5-118">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6efc5-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6efc5-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="6efc5-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6efc5-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="6efc5-121">Пакет SDK для .NET Core 3.0 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="6efc5-121">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="6efc5-122">[C# для Visual Studio Code версии 1.17.1 или более поздней](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="6efc5-122">[C# for Visual Studio Code version 1.17.1 or later](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>
* <span data-ttu-id="6efc5-123">[Node.js](https://nodejs.org/) с [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="6efc5-123">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="6efc5-124">Создание веб-приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6efc5-124">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6efc5-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6efc5-125">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6efc5-126">Настройте Visual Studio на поиск npm в переменной среды *PATH*.</span><span class="sxs-lookup"><span data-stu-id="6efc5-126">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="6efc5-127">По умолчанию Visual Studio использует версию npm, которая находится в его каталоге установки.</span><span class="sxs-lookup"><span data-stu-id="6efc5-127">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="6efc5-128">В Visual Studio выполните следующие инструкции:</span><span class="sxs-lookup"><span data-stu-id="6efc5-128">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="6efc5-129">Щелкните **Сервис** > **Параметры** > **Проекты и решения** > **Управление веб-пакетами** > **Внешние веб-инструменты**.</span><span class="sxs-lookup"><span data-stu-id="6efc5-129">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="6efc5-130">Выберите элемент *$(PATH)* в списке.</span><span class="sxs-lookup"><span data-stu-id="6efc5-130">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="6efc5-131">Щелкните стрелку вверх, чтобы переместить этот элемент на вторую позицию в списке.</span><span class="sxs-lookup"><span data-stu-id="6efc5-131">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Конфигурация Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="6efc5-133">Настройка Visual Studio завершена.</span><span class="sxs-lookup"><span data-stu-id="6efc5-133">Visual Studio configuration is completed.</span></span> <span data-ttu-id="6efc5-134">Теперь следует создать проект.</span><span class="sxs-lookup"><span data-stu-id="6efc5-134">It's time to create the project.</span></span>

1. <span data-ttu-id="6efc5-135">Перейдите в меню **Файл** > **Создать** > **Проект** и выберите шаблон **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6efc5-135">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="6efc5-136">Присвойте проекту имя *SignalRWebPack* и щелкните **Создать**.</span><span class="sxs-lookup"><span data-stu-id="6efc5-136">Name the project *SignalRWebPack*, and select **Create**.</span></span>
1. <span data-ttu-id="6efc5-137">Выберите *.NET Core* в раскрывающемся списке целевых платформ и *ASP.NET Core 3.0* в раскрывающемся списке версий платформ.</span><span class="sxs-lookup"><span data-stu-id="6efc5-137">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 3.0* from the framework selector drop-down.</span></span> <span data-ttu-id="6efc5-138">Выберите шаблон **Пустой** и щелкните **Создать**.</span><span class="sxs-lookup"><span data-stu-id="6efc5-138">Select the **Empty** template, and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6efc5-139">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6efc5-139">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6efc5-140">Во **встроенном терминале** выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6efc5-140">Run the following command in the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="6efc5-141">В каталоге *SignalRWebPack* создается пустое веб-приложение ASP.NET Core, ориентированное на платформу .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6efc5-141">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="6efc5-142">Настройка Webpack и TypeScript</span><span class="sxs-lookup"><span data-stu-id="6efc5-142">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="6efc5-143">Далее следует настроить преобразование TypeScript в JavaScript и создание пакета ресурсов на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="6efc5-143">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="6efc5-144">В корневом элементе проекта выполните следующую команду, чтобы создать файл *package.json*:</span><span class="sxs-lookup"><span data-stu-id="6efc5-144">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="6efc5-145">Добавьте выделенное свойство в файл *package.json*:</span><span class="sxs-lookup"><span data-stu-id="6efc5-145">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/3.x/snippets/package1.json?highlight=4)]

    <span data-ttu-id="6efc5-146">Если свойству `private` присвоено значение `true`, на следующем шаге не будут отображаться предупреждения об установке пакета.</span><span class="sxs-lookup"><span data-stu-id="6efc5-146">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="6efc5-147">Установите необходимые пакеты npm.</span><span class="sxs-lookup"><span data-stu-id="6efc5-147">Install the required npm packages.</span></span> <span data-ttu-id="6efc5-148">Выполните следующую команду в корневом элементе проекта:</span><span class="sxs-lookup"><span data-stu-id="6efc5-148">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="6efc5-149">Сведения о команде, на которые следует обратить внимание:</span><span class="sxs-lookup"><span data-stu-id="6efc5-149">Some command details to note:</span></span>

    * <span data-ttu-id="6efc5-150">Для каждого имени пакета номер версии указывается после знака `@`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-150">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="6efc5-151">npm устанавливает указанные версии пакета.</span><span class="sxs-lookup"><span data-stu-id="6efc5-151">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="6efc5-152">Использование параметра `-E` позволяет отключить установленное по умолчанию поведение npm, предусматривающее запись операторов диапазона [семантического управления версиями](https://semver.org/) в файл *package.json*.</span><span class="sxs-lookup"><span data-stu-id="6efc5-152">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="6efc5-153">Например, `"webpack": "4.29.3"` используется вместо `"webpack": "^4.29.3"`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-153">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="6efc5-154">Этот параметр позволяет исключить непреднамеренное обновление до более новых версий пакета.</span><span class="sxs-lookup"><span data-stu-id="6efc5-154">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="6efc5-155">Дополнительные сведения см. в официальной документации по [npm-install](https://docs.npmjs.com/cli/install).</span><span class="sxs-lookup"><span data-stu-id="6efc5-155">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="6efc5-156">Замените свойство `scripts` в файле *package.json* следующим фрагментом кода:</span><span class="sxs-lookup"><span data-stu-id="6efc5-156">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="6efc5-157">Некоторые пояснения к скриптам:</span><span class="sxs-lookup"><span data-stu-id="6efc5-157">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="6efc5-158">`build`. создает пакет ваших ресурсов на стороне клиента в режиме разработки и отслеживает изменения.</span><span class="sxs-lookup"><span data-stu-id="6efc5-158">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="6efc5-159">Наблюдатель за файлами выполняет повторное создание пакета при каждом изменении файла проекта.</span><span class="sxs-lookup"><span data-stu-id="6efc5-159">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="6efc5-160">Параметр `mode` позволяет отключить оптимизации в рабочей среде, такие как встряхивание дерева и минификация.</span><span class="sxs-lookup"><span data-stu-id="6efc5-160">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="6efc5-161">В среде разработки следует использовать только `build`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-161">Only use `build` in development.</span></span>
    * <span data-ttu-id="6efc5-162">`release`. создает пакет ресурсов на стороне клиента в рабочем режиме.</span><span class="sxs-lookup"><span data-stu-id="6efc5-162">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="6efc5-163">`publish`. запускает скрипт `release` для создания пакета ресурсов на стороне клиента в рабочем режиме.</span><span class="sxs-lookup"><span data-stu-id="6efc5-163">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="6efc5-164">Этот скрипт вызывает команду [publish](/dotnet/core/tools/dotnet-publish) интерфейса командной строки .NET Core для публикации приложения.</span><span class="sxs-lookup"><span data-stu-id="6efc5-164">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="6efc5-165">Создайте в корневом элементе проекта файл *webpack.config.js* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="6efc5-165">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/3.x/webpack.config.js)]

    <span data-ttu-id="6efc5-166">Приведенный выше файл определяет конфигурацию компиляции Webpack.</span><span class="sxs-lookup"><span data-stu-id="6efc5-166">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="6efc5-167">Сведения о конфигурации, на которые следует обратить внимание:</span><span class="sxs-lookup"><span data-stu-id="6efc5-167">Some configuration details to note:</span></span>

    * <span data-ttu-id="6efc5-168">Свойство `output` переопределяет значение по умолчанию для *dist*.</span><span class="sxs-lookup"><span data-stu-id="6efc5-168">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="6efc5-169">Вместо этого пакет выводится в каталог *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="6efc5-169">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="6efc5-170">Массив `resolve.extensions` содержит элемент *.js* для импорта кода JavaScript клиента SignalR.</span><span class="sxs-lookup"><span data-stu-id="6efc5-170">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="6efc5-171">Создайте новый каталог *src* в корневом элементе проекта.</span><span class="sxs-lookup"><span data-stu-id="6efc5-171">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="6efc5-172">В этом каталоге будут храниться ресурсы на стороне клиента для проекта.</span><span class="sxs-lookup"><span data-stu-id="6efc5-172">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="6efc5-173">Создайте файл *src/index.html* со следующим содержимым.</span><span class="sxs-lookup"><span data-stu-id="6efc5-173">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/3.x/src/index.html)]

    <span data-ttu-id="6efc5-174">Приведенный выше HTML-файл определяет стереотипную разметку главной страницы.</span><span class="sxs-lookup"><span data-stu-id="6efc5-174">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="6efc5-175">Создайте каталог *src/css*.</span><span class="sxs-lookup"><span data-stu-id="6efc5-175">Create a new *src/css* directory.</span></span> <span data-ttu-id="6efc5-176">В нем будут храниться файлы *.css* проекта.</span><span class="sxs-lookup"><span data-stu-id="6efc5-176">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="6efc5-177">Создайте файл *src/css/main.css* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="6efc5-177">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/3.x/src/css/main.css)]

    <span data-ttu-id="6efc5-178">Приведенный выше файл *main.css* определяет стиль приложения.</span><span class="sxs-lookup"><span data-stu-id="6efc5-178">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="6efc5-179">Создайте файл *src/tsconfig.json* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="6efc5-179">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/3.x/src/tsconfig.json)]

    <span data-ttu-id="6efc5-180">Приведенный выше код определяет конфигурацию компилятора TypeScript для получения совместимого с [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6efc5-180">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="6efc5-181">Создайте файл *src/index.ts* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="6efc5-181">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="6efc5-182">Приведенный выше код TypeScript извлекает ссылки на элементы модели DOM и присоединяет два обработчика событий:</span><span class="sxs-lookup"><span data-stu-id="6efc5-182">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="6efc5-183">`keyup`. это событие возникает в том случае, если пользователь вводит какой-либо текст в поле, идентифицированное как `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-183">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="6efc5-184">Функция `send` вызывается, когда пользователь нажимает клавишу **ВВОД**.</span><span class="sxs-lookup"><span data-stu-id="6efc5-184">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="6efc5-185">`click`. это событие возникает, когда пользователь нажимает кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="6efc5-185">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="6efc5-186">Вызывается функция `send`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-186">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="6efc5-187">Настройка приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6efc5-187">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="6efc5-188">В методе `Startup.Configure` добавьте вызовы к [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) и [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="6efc5-188">In the `Startup.Configure` method, add calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseStaticDefaultFiles&highlight=2-3)]

   <span data-ttu-id="6efc5-189">Приведенный выше код позволяет серверу обнаруживать и обрабатывать файл *index.html* независимо от того, вводит ли пользователь полный URL-адрес или URL-адрес корня веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="6efc5-189">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="6efc5-190">В конце метода `Startup.Configure` сопоставьте маршрут */hub* с концентратором `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-190">At the end of the `Startup.Configure` method, map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="6efc5-191">Замените код, отображающий *Hello World!* ,</span><span class="sxs-lookup"><span data-stu-id="6efc5-191">Replace the code that displays *Hello World!*</span></span> <span data-ttu-id="6efc5-192">следующей строкой:</span><span class="sxs-lookup"><span data-stu-id="6efc5-192">with the following line:</span></span> 

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseSignalR&highlight=3)]

1. <span data-ttu-id="6efc5-193">В методе `Startup.ConfigureServices` вызовите [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_).</span><span class="sxs-lookup"><span data-stu-id="6efc5-193">In the `Startup.ConfigureServices` method, call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_).</span></span> <span data-ttu-id="6efc5-194">Этот метод добавляет службы SignalR в проект.</span><span class="sxs-lookup"><span data-stu-id="6efc5-194">It adds the SignalR services to your project.</span></span>

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="6efc5-195">Создайте новый каталог *Hubs* в корневом элементе проекта.</span><span class="sxs-lookup"><span data-stu-id="6efc5-195">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="6efc5-196">Этот каталог служит для хранения концентратора SignalR, созданного на предыдущем шаге.</span><span class="sxs-lookup"><span data-stu-id="6efc5-196">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="6efc5-197">Создайте концентратор *Hubs/ChatHub.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="6efc5-197">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="6efc5-198">Добавьте следующий код в самое начало файла *Startup.cs*, чтобы разрешить ссылку `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="6efc5-198">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="6efc5-199">Обеспечение взаимодействия между клиентом и сервером</span><span class="sxs-lookup"><span data-stu-id="6efc5-199">Enable client and server communication</span></span>

<span data-ttu-id="6efc5-200">На данный момент приложение отображает простую форму для отправки сообщений.</span><span class="sxs-lookup"><span data-stu-id="6efc5-200">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="6efc5-201">При попытке сделать это ничего не происходит.</span><span class="sxs-lookup"><span data-stu-id="6efc5-201">Nothing happens when you try to do so.</span></span> <span data-ttu-id="6efc5-202">Сервер прослушивает конкретный маршрут, но ничего не делает с отправленными сообщениями.</span><span class="sxs-lookup"><span data-stu-id="6efc5-202">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="6efc5-203">Выполните следующую команду в корневом элементе проекта:</span><span class="sxs-lookup"><span data-stu-id="6efc5-203">Execute the following command at the project root:</span></span>

    ```console
    npm install @microsoft/signalr
    ```

    <span data-ttu-id="6efc5-204">Команда выше устанавливает [клиент TypeScript SignalR](https://www.npmjs.com/package/@microsoft/signalr), который позволяет клиенту отправлять сообщения на сервер.</span><span class="sxs-lookup"><span data-stu-id="6efc5-204">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@microsoft/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="6efc5-205">Добавьте выделенный код в файл *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="6efc5-205">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="6efc5-206">Приведенный выше код поддерживает получение сообщений от сервера.</span><span class="sxs-lookup"><span data-stu-id="6efc5-206">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="6efc5-207">Класс `HubConnectionBuilder` создает новый построитель для настройки подключения к серверу.</span><span class="sxs-lookup"><span data-stu-id="6efc5-207">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="6efc5-208">Функция `withUrl` настраивает URL-адрес концентратора.</span><span class="sxs-lookup"><span data-stu-id="6efc5-208">The `withUrl` function configures the hub URL.</span></span>

    SignalR<span data-ttu-id="6efc5-209"> обеспечивает обмен сообщениями между клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="6efc5-209"> enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="6efc5-210">Каждому сообщению присваивается определенное имя.</span><span class="sxs-lookup"><span data-stu-id="6efc5-210">Each message has a specific name.</span></span> <span data-ttu-id="6efc5-211">Например, у вас могут быть сообщения с именем `messageReceived`, которые выполняют логику отображения новых сообщений в соответствующей зоне.</span><span class="sxs-lookup"><span data-stu-id="6efc5-211">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="6efc5-212">Прослушивание определенных сообщений реализуется с помощью функции `on`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-212">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="6efc5-213">Возможно прослушивание любого числа имен сообщений.</span><span class="sxs-lookup"><span data-stu-id="6efc5-213">You can listen to any number of message names.</span></span> <span data-ttu-id="6efc5-214">Кроме того, можно передавать параметры сообщения, например имя его автора или содержимое полученного сообщения.</span><span class="sxs-lookup"><span data-stu-id="6efc5-214">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="6efc5-215">После того как клиент получает сообщение, создается новый элемент `div`, в атрибуте `innerHTML` которого содержатся имя автора и содержимое сообщения.</span><span class="sxs-lookup"><span data-stu-id="6efc5-215">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="6efc5-216">Этот элемент добавляется в основной элемент `div`, который используется для отображения сообщений.</span><span class="sxs-lookup"><span data-stu-id="6efc5-216">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="6efc5-217">Теперь клиент может принимать сообщения. Далее следует настроить его для отправки сообщений.</span><span class="sxs-lookup"><span data-stu-id="6efc5-217">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="6efc5-218">Добавьте выделенный код в файл *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="6efc5-218">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/src/index.ts?highlight=34-35)]

    <span data-ttu-id="6efc5-219">Для отправки сообщения через соединение WebSockets необходимо вызвать метод `send`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-219">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="6efc5-220">Первый параметр этого метода содержит имя сообщения.</span><span class="sxs-lookup"><span data-stu-id="6efc5-220">The method's first parameter is the message name.</span></span> <span data-ttu-id="6efc5-221">Другие параметры заполняются данными сообщения.</span><span class="sxs-lookup"><span data-stu-id="6efc5-221">The message data inhabits the other parameters.</span></span> <span data-ttu-id="6efc5-222">В этом примере на сервер отправляется сообщение, идентифицированное как `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-222">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="6efc5-223">Это сообщение содержит имя пользователя, а также данные, введенные этим пользователем в текстовое поле.</span><span class="sxs-lookup"><span data-stu-id="6efc5-223">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="6efc5-224">Если отправка выполняется успешно, значение текстового поля очищается.</span><span class="sxs-lookup"><span data-stu-id="6efc5-224">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="6efc5-225">Добавьте выделенный метод в класс `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="6efc5-225">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="6efc5-226">Приведенный выше код осуществляет широковещательную рассылку полученных сервером сообщений всем подключенным пользователям.</span><span class="sxs-lookup"><span data-stu-id="6efc5-226">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="6efc5-227">Универсальный метод `on` для получения всех сообщений не требуется.</span><span class="sxs-lookup"><span data-stu-id="6efc5-227">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="6efc5-228">Имя метода указывается после суффиксов имени сообщения.</span><span class="sxs-lookup"><span data-stu-id="6efc5-228">A method named after the message name suffices.</span></span>

    <span data-ttu-id="6efc5-229">В этом примере клиент TypeScript отправляет сообщение, идентифицированное как `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-229">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="6efc5-230">Метод C# `NewMessage` ожидает данные, отправленные клиентом.</span><span class="sxs-lookup"><span data-stu-id="6efc5-230">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="6efc5-231">Выполняется вызов метода [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) для [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="6efc5-231">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="6efc5-232">Полученные сообщения отправляются всем клиентам, подключенным к концентратору.</span><span class="sxs-lookup"><span data-stu-id="6efc5-232">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="6efc5-233">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="6efc5-233">Test the app</span></span>

<span data-ttu-id="6efc5-234">Чтобы проверить работоспособность приложения, выполните следующие шаги.</span><span class="sxs-lookup"><span data-stu-id="6efc5-234">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6efc5-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6efc5-235">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="6efc5-236">Запустите средство Webpack в режиме *release*.</span><span class="sxs-lookup"><span data-stu-id="6efc5-236">Run Webpack in *release* mode.</span></span> <span data-ttu-id="6efc5-237">Выполните следующие команды в окне **Консоль диспетчера пакетов** в корневом элементе проекта.</span><span class="sxs-lookup"><span data-stu-id="6efc5-237">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="6efc5-238">Если вы не в корневом каталоге проекта, введите перед этой командой `cd SignalRWebPack`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-238">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="6efc5-239">Выберите **Отладка** > **Запуск без отладки**, чтобы запустить приложение в браузере, не присоединяя отладчик.</span><span class="sxs-lookup"><span data-stu-id="6efc5-239">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="6efc5-240">Файл *wwwroot/index.html* обрабатывается по адресу `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-240">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

   <span data-ttu-id="6efc5-241">Если во время компиляции возникают ошибки, попробуйте закрыть и снова открыть решение.</span><span class="sxs-lookup"><span data-stu-id="6efc5-241">If you get compile errors, try closing and reopening the solution.</span></span> 

1. <span data-ttu-id="6efc5-242">Откройте другой экземпляр любого браузера.</span><span class="sxs-lookup"><span data-stu-id="6efc5-242">Open another browser instance (any browser).</span></span> <span data-ttu-id="6efc5-243">Вставьте URL-адрес в адресную строку.</span><span class="sxs-lookup"><span data-stu-id="6efc5-243">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="6efc5-244">Выберите любой браузер, введите произвольный текст в поле **Сообщение** и нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="6efc5-244">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="6efc5-245">На обеих страницах мгновенно отображаются имя пользователя и сообщение.</span><span class="sxs-lookup"><span data-stu-id="6efc5-245">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6efc5-246">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6efc5-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="6efc5-247">Запустите средство Webpack в режиме *release*, выполнив следующую команду в корневом элементе проекта:</span><span class="sxs-lookup"><span data-stu-id="6efc5-247">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="6efc5-248">Выполните построение и запустите приложение, выполнив следующую команду в корневом элементе проекта:</span><span class="sxs-lookup"><span data-stu-id="6efc5-248">Build and run the app by executing the following command in the project root:</span></span>

    ```dotnetcli
    dotnet run
    ```

    <span data-ttu-id="6efc5-249">Веб-сервер запускает приложение и делает его доступным на узле localhost.</span><span class="sxs-lookup"><span data-stu-id="6efc5-249">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="6efc5-250">Откройте браузер с адресом `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-250">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="6efc5-251">Обрабатывается файл *wwwroot/index.html*.</span><span class="sxs-lookup"><span data-stu-id="6efc5-251">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="6efc5-252">Скопируйте URL-адрес из адресной строки.</span><span class="sxs-lookup"><span data-stu-id="6efc5-252">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="6efc5-253">Откройте другой экземпляр любого браузера.</span><span class="sxs-lookup"><span data-stu-id="6efc5-253">Open another browser instance (any browser).</span></span> <span data-ttu-id="6efc5-254">Вставьте URL-адрес в адресную строку.</span><span class="sxs-lookup"><span data-stu-id="6efc5-254">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="6efc5-255">Выберите любой браузер, введите произвольный текст в поле **Сообщение** и нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="6efc5-255">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="6efc5-256">На обеих страницах мгновенно отображаются имя пользователя и сообщение.</span><span class="sxs-lookup"><span data-stu-id="6efc5-256">The unique user name and message are displayed on both pages instantly.</span></span>

---

![Сообщение, отображаемое в обоих окнах браузера](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="6efc5-258">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="6efc5-258">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6efc5-259">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6efc5-259">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6efc5-260">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) с рабочей нагрузкой **ASP.NET и веб-разработка**</span><span class="sxs-lookup"><span data-stu-id="6efc5-260">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="6efc5-261">Пакет SDK для .NET Core 2.2 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="6efc5-261">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="6efc5-262">[Node.js](https://nodejs.org/) с [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="6efc5-262">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6efc5-263">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6efc5-263">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="6efc5-264">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6efc5-264">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="6efc5-265">Пакет SDK для .NET Core 2.2 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="6efc5-265">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="6efc5-266">[C# для Visual Studio Code версии 1.17.1 или более поздней](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="6efc5-266">[C# for Visual Studio Code version 1.17.1 or later](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>
* <span data-ttu-id="6efc5-267">[Node.js](https://nodejs.org/) с [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="6efc5-267">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="6efc5-268">Создание веб-приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6efc5-268">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6efc5-269">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6efc5-269">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6efc5-270">Настройте Visual Studio на поиск npm в переменной среды *PATH*.</span><span class="sxs-lookup"><span data-stu-id="6efc5-270">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="6efc5-271">По умолчанию Visual Studio использует версию npm, которая находится в его каталоге установки.</span><span class="sxs-lookup"><span data-stu-id="6efc5-271">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="6efc5-272">В Visual Studio выполните следующие инструкции:</span><span class="sxs-lookup"><span data-stu-id="6efc5-272">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="6efc5-273">Щелкните **Сервис** > **Параметры** > **Проекты и решения** > **Управление веб-пакетами** > **Внешние веб-инструменты**.</span><span class="sxs-lookup"><span data-stu-id="6efc5-273">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="6efc5-274">Выберите элемент *$(PATH)* в списке.</span><span class="sxs-lookup"><span data-stu-id="6efc5-274">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="6efc5-275">Щелкните стрелку вверх, чтобы переместить этот элемент на вторую позицию в списке.</span><span class="sxs-lookup"><span data-stu-id="6efc5-275">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Конфигурация Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="6efc5-277">Настройка Visual Studio завершена.</span><span class="sxs-lookup"><span data-stu-id="6efc5-277">Visual Studio configuration is completed.</span></span> <span data-ttu-id="6efc5-278">Теперь следует создать проект.</span><span class="sxs-lookup"><span data-stu-id="6efc5-278">It's time to create the project.</span></span>

1. <span data-ttu-id="6efc5-279">Перейдите в меню **Файл** > **Создать** > **Проект** и выберите шаблон **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6efc5-279">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="6efc5-280">Присвойте проекту имя *SignalRWebPack* и щелкните **Создать**.</span><span class="sxs-lookup"><span data-stu-id="6efc5-280">Name the project *SignalRWebPack*, and select **Create**.</span></span>
1. <span data-ttu-id="6efc5-281">Выберите *.NET Core* в раскрывающемся списке целевых платформ и выберите *ASP.NET Core 2.2* в раскрывающемся списке средства выбора платформы.</span><span class="sxs-lookup"><span data-stu-id="6efc5-281">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.2* from the framework selector drop-down.</span></span> <span data-ttu-id="6efc5-282">Выберите шаблон **Пустой** и щелкните **Создать**.</span><span class="sxs-lookup"><span data-stu-id="6efc5-282">Select the **Empty** template, and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6efc5-283">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6efc5-283">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6efc5-284">Во **встроенном терминале** выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6efc5-284">Run the following command in the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="6efc5-285">В каталоге *SignalRWebPack* создается пустое веб-приложение ASP.NET Core, ориентированное на платформу .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6efc5-285">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="6efc5-286">Настройка Webpack и TypeScript</span><span class="sxs-lookup"><span data-stu-id="6efc5-286">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="6efc5-287">Далее следует настроить преобразование TypeScript в JavaScript и создание пакета ресурсов на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="6efc5-287">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="6efc5-288">В корневом элементе проекта выполните следующую команду, чтобы создать файл *package.json*:</span><span class="sxs-lookup"><span data-stu-id="6efc5-288">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="6efc5-289">Добавьте выделенное свойство в файл *package.json*:</span><span class="sxs-lookup"><span data-stu-id="6efc5-289">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/2.x/snippets/package1.json?highlight=4)]

    <span data-ttu-id="6efc5-290">Если свойству `private` присвоено значение `true`, на следующем шаге не будут отображаться предупреждения об установке пакета.</span><span class="sxs-lookup"><span data-stu-id="6efc5-290">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="6efc5-291">Установите необходимые пакеты npm.</span><span class="sxs-lookup"><span data-stu-id="6efc5-291">Install the required npm packages.</span></span> <span data-ttu-id="6efc5-292">Выполните следующую команду в корневом элементе проекта:</span><span class="sxs-lookup"><span data-stu-id="6efc5-292">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="6efc5-293">Сведения о команде, на которые следует обратить внимание:</span><span class="sxs-lookup"><span data-stu-id="6efc5-293">Some command details to note:</span></span>

    * <span data-ttu-id="6efc5-294">Для каждого имени пакета номер версии указывается после знака `@`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-294">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="6efc5-295">npm устанавливает указанные версии пакета.</span><span class="sxs-lookup"><span data-stu-id="6efc5-295">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="6efc5-296">Использование параметра `-E` позволяет отключить установленное по умолчанию поведение npm, предусматривающее запись операторов диапазона [семантического управления версиями](https://semver.org/) в файл *package.json*.</span><span class="sxs-lookup"><span data-stu-id="6efc5-296">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="6efc5-297">Например, `"webpack": "4.29.3"` используется вместо `"webpack": "^4.29.3"`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-297">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="6efc5-298">Этот параметр позволяет исключить непреднамеренное обновление до более новых версий пакета.</span><span class="sxs-lookup"><span data-stu-id="6efc5-298">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="6efc5-299">Дополнительные сведения см. в официальной документации по [npm-install](https://docs.npmjs.com/cli/install).</span><span class="sxs-lookup"><span data-stu-id="6efc5-299">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="6efc5-300">Замените свойство `scripts` в файле *package.json* следующим фрагментом кода:</span><span class="sxs-lookup"><span data-stu-id="6efc5-300">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="6efc5-301">Некоторые пояснения к скриптам:</span><span class="sxs-lookup"><span data-stu-id="6efc5-301">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="6efc5-302">`build`. создает пакет ваших ресурсов на стороне клиента в режиме разработки и отслеживает изменения.</span><span class="sxs-lookup"><span data-stu-id="6efc5-302">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="6efc5-303">Наблюдатель за файлами выполняет повторное создание пакета при каждом изменении файла проекта.</span><span class="sxs-lookup"><span data-stu-id="6efc5-303">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="6efc5-304">Параметр `mode` позволяет отключить оптимизации в рабочей среде, такие как встряхивание дерева и минификация.</span><span class="sxs-lookup"><span data-stu-id="6efc5-304">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="6efc5-305">В среде разработки следует использовать только `build`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-305">Only use `build` in development.</span></span>
    * <span data-ttu-id="6efc5-306">`release`. создает пакет ресурсов на стороне клиента в рабочем режиме.</span><span class="sxs-lookup"><span data-stu-id="6efc5-306">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="6efc5-307">`publish`. запускает скрипт `release` для создания пакета ресурсов на стороне клиента в рабочем режиме.</span><span class="sxs-lookup"><span data-stu-id="6efc5-307">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="6efc5-308">Этот скрипт вызывает команду [publish](/dotnet/core/tools/dotnet-publish) интерфейса командной строки .NET Core для публикации приложения.</span><span class="sxs-lookup"><span data-stu-id="6efc5-308">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="6efc5-309">Создайте в корневом элементе проекта файл *webpack.config.js* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="6efc5-309">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/2.x/webpack.config.js)]

    <span data-ttu-id="6efc5-310">Приведенный выше файл определяет конфигурацию компиляции Webpack.</span><span class="sxs-lookup"><span data-stu-id="6efc5-310">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="6efc5-311">Сведения о конфигурации, на которые следует обратить внимание:</span><span class="sxs-lookup"><span data-stu-id="6efc5-311">Some configuration details to note:</span></span>

    * <span data-ttu-id="6efc5-312">Свойство `output` переопределяет значение по умолчанию для *dist*.</span><span class="sxs-lookup"><span data-stu-id="6efc5-312">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="6efc5-313">Вместо этого пакет выводится в каталог *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="6efc5-313">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="6efc5-314">Массив `resolve.extensions` содержит элемент *.js* для импорта кода JavaScript клиента SignalR.</span><span class="sxs-lookup"><span data-stu-id="6efc5-314">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="6efc5-315">Создайте новый каталог *src* в корневом элементе проекта.</span><span class="sxs-lookup"><span data-stu-id="6efc5-315">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="6efc5-316">В этом каталоге будут храниться ресурсы на стороне клиента для проекта.</span><span class="sxs-lookup"><span data-stu-id="6efc5-316">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="6efc5-317">Создайте файл *src/index.html* со следующим содержимым.</span><span class="sxs-lookup"><span data-stu-id="6efc5-317">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/2.x/src/index.html)]

    <span data-ttu-id="6efc5-318">Приведенный выше HTML-файл определяет стереотипную разметку главной страницы.</span><span class="sxs-lookup"><span data-stu-id="6efc5-318">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="6efc5-319">Создайте каталог *src/css*.</span><span class="sxs-lookup"><span data-stu-id="6efc5-319">Create a new *src/css* directory.</span></span> <span data-ttu-id="6efc5-320">В нем будут храниться файлы *.css* проекта.</span><span class="sxs-lookup"><span data-stu-id="6efc5-320">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="6efc5-321">Создайте файл *src/css/main.css* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="6efc5-321">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/2.x/src/css/main.css)]

    <span data-ttu-id="6efc5-322">Приведенный выше файл *main.css* определяет стиль приложения.</span><span class="sxs-lookup"><span data-stu-id="6efc5-322">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="6efc5-323">Создайте файл *src/tsconfig.json* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="6efc5-323">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/2.x/src/tsconfig.json)]

    <span data-ttu-id="6efc5-324">Приведенный выше код определяет конфигурацию компилятора TypeScript для получения совместимого с [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6efc5-324">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="6efc5-325">Создайте файл *src/index.ts* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="6efc5-325">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="6efc5-326">Приведенный выше код TypeScript извлекает ссылки на элементы модели DOM и присоединяет два обработчика событий:</span><span class="sxs-lookup"><span data-stu-id="6efc5-326">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="6efc5-327">`keyup`. это событие возникает в том случае, если пользователь вводит какой-либо текст в поле, идентифицированное как `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-327">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="6efc5-328">Функция `send` вызывается, когда пользователь нажимает клавишу **ВВОД**.</span><span class="sxs-lookup"><span data-stu-id="6efc5-328">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="6efc5-329">`click`. это событие возникает, когда пользователь нажимает кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="6efc5-329">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="6efc5-330">Вызывается функция `send`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-330">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="6efc5-331">Настройка приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6efc5-331">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="6efc5-332">Код в методе `Startup.Configure` отображает текст *Hello World!* .</span><span class="sxs-lookup"><span data-stu-id="6efc5-332">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="6efc5-333">Замените вызов метода `app.Run` вызовами методов [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) и [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="6efc5-333">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="6efc5-334">Приведенный выше код позволяет серверу обнаруживать и обрабатывать файл *index.html* независимо от того, вводит ли пользователь полный URL-адрес или URL-адрес корня веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="6efc5-334">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="6efc5-335">Вызовите метод [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) в методе `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-335">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="6efc5-336">Этот метод добавляет службы SignalR в проект.</span><span class="sxs-lookup"><span data-stu-id="6efc5-336">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="6efc5-337">Сопоставьте маршрут */hub* с концентратором `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-337">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="6efc5-338">Добавьте следующие строки в конец метода `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="6efc5-338">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="6efc5-339">Создайте новый каталог *Hubs* в корневом элементе проекта.</span><span class="sxs-lookup"><span data-stu-id="6efc5-339">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="6efc5-340">Этот каталог служит для хранения концентратора SignalR, созданного на предыдущем шаге.</span><span class="sxs-lookup"><span data-stu-id="6efc5-340">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="6efc5-341">Создайте концентратор *Hubs/ChatHub.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="6efc5-341">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="6efc5-342">Добавьте следующий код в самое начало файла *Startup.cs*, чтобы разрешить ссылку `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="6efc5-342">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="6efc5-343">Обеспечение взаимодействия между клиентом и сервером</span><span class="sxs-lookup"><span data-stu-id="6efc5-343">Enable client and server communication</span></span>

<span data-ttu-id="6efc5-344">На данный момент приложение отображает простую форму для отправки сообщений.</span><span class="sxs-lookup"><span data-stu-id="6efc5-344">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="6efc5-345">При попытке сделать это ничего не происходит.</span><span class="sxs-lookup"><span data-stu-id="6efc5-345">Nothing happens when you try to do so.</span></span> <span data-ttu-id="6efc5-346">Сервер прослушивает конкретный маршрут, но ничего не делает с отправленными сообщениями.</span><span class="sxs-lookup"><span data-stu-id="6efc5-346">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="6efc5-347">Выполните следующую команду в корневом элементе проекта:</span><span class="sxs-lookup"><span data-stu-id="6efc5-347">Execute the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="6efc5-348">Команда выше устанавливает [клиент TypeScript SignalR](https://www.npmjs.com/package/@microsoft/signalr), который позволяет клиенту отправлять сообщения на сервер.</span><span class="sxs-lookup"><span data-stu-id="6efc5-348">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@microsoft/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="6efc5-349">Добавьте выделенный код в файл *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="6efc5-349">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="6efc5-350">Приведенный выше код поддерживает получение сообщений от сервера.</span><span class="sxs-lookup"><span data-stu-id="6efc5-350">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="6efc5-351">Класс `HubConnectionBuilder` создает новый построитель для настройки подключения к серверу.</span><span class="sxs-lookup"><span data-stu-id="6efc5-351">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="6efc5-352">Функция `withUrl` настраивает URL-адрес концентратора.</span><span class="sxs-lookup"><span data-stu-id="6efc5-352">The `withUrl` function configures the hub URL.</span></span>

    SignalR<span data-ttu-id="6efc5-353"> обеспечивает обмен сообщениями между клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="6efc5-353"> enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="6efc5-354">Каждому сообщению присваивается определенное имя.</span><span class="sxs-lookup"><span data-stu-id="6efc5-354">Each message has a specific name.</span></span> <span data-ttu-id="6efc5-355">Например, у вас могут быть сообщения с именем `messageReceived`, которые выполняют логику отображения новых сообщений в соответствующей зоне.</span><span class="sxs-lookup"><span data-stu-id="6efc5-355">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="6efc5-356">Прослушивание определенных сообщений реализуется с помощью функции `on`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-356">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="6efc5-357">Возможно прослушивание любого числа имен сообщений.</span><span class="sxs-lookup"><span data-stu-id="6efc5-357">You can listen to any number of message names.</span></span> <span data-ttu-id="6efc5-358">Кроме того, можно передавать параметры сообщения, например имя его автора или содержимое полученного сообщения.</span><span class="sxs-lookup"><span data-stu-id="6efc5-358">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="6efc5-359">После того как клиент получает сообщение, создается новый элемент `div`, в атрибуте `innerHTML` которого содержатся имя автора и содержимое сообщения.</span><span class="sxs-lookup"><span data-stu-id="6efc5-359">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="6efc5-360">Этот элемент добавляется в основной элемент `div`, который используется для отображения сообщений.</span><span class="sxs-lookup"><span data-stu-id="6efc5-360">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="6efc5-361">Теперь клиент может принимать сообщения. Далее следует настроить его для отправки сообщений.</span><span class="sxs-lookup"><span data-stu-id="6efc5-361">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="6efc5-362">Добавьте выделенный код в файл *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="6efc5-362">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/src/index.ts?highlight=34-35)]

    <span data-ttu-id="6efc5-363">Для отправки сообщения через соединение WebSockets необходимо вызвать метод `send`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-363">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="6efc5-364">Первый параметр этого метода содержит имя сообщения.</span><span class="sxs-lookup"><span data-stu-id="6efc5-364">The method's first parameter is the message name.</span></span> <span data-ttu-id="6efc5-365">Другие параметры заполняются данными сообщения.</span><span class="sxs-lookup"><span data-stu-id="6efc5-365">The message data inhabits the other parameters.</span></span> <span data-ttu-id="6efc5-366">В этом примере на сервер отправляется сообщение, идентифицированное как `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-366">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="6efc5-367">Это сообщение содержит имя пользователя, а также данные, введенные этим пользователем в текстовое поле.</span><span class="sxs-lookup"><span data-stu-id="6efc5-367">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="6efc5-368">Если отправка выполняется успешно, значение текстового поля очищается.</span><span class="sxs-lookup"><span data-stu-id="6efc5-368">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="6efc5-369">Добавьте выделенный метод в класс `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="6efc5-369">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="6efc5-370">Приведенный выше код осуществляет широковещательную рассылку полученных сервером сообщений всем подключенным пользователям.</span><span class="sxs-lookup"><span data-stu-id="6efc5-370">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="6efc5-371">Универсальный метод `on` для получения всех сообщений не требуется.</span><span class="sxs-lookup"><span data-stu-id="6efc5-371">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="6efc5-372">Имя метода указывается после суффиксов имени сообщения.</span><span class="sxs-lookup"><span data-stu-id="6efc5-372">A method named after the message name suffices.</span></span>

    <span data-ttu-id="6efc5-373">В этом примере клиент TypeScript отправляет сообщение, идентифицированное как `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-373">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="6efc5-374">Метод C# `NewMessage` ожидает данные, отправленные клиентом.</span><span class="sxs-lookup"><span data-stu-id="6efc5-374">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="6efc5-375">Выполняется вызов метода [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) для [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="6efc5-375">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="6efc5-376">Полученные сообщения отправляются всем клиентам, подключенным к концентратору.</span><span class="sxs-lookup"><span data-stu-id="6efc5-376">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="6efc5-377">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="6efc5-377">Test the app</span></span>

<span data-ttu-id="6efc5-378">Чтобы проверить работоспособность приложения, выполните следующие шаги.</span><span class="sxs-lookup"><span data-stu-id="6efc5-378">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6efc5-379">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6efc5-379">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="6efc5-380">Запустите средство Webpack в режиме *release*.</span><span class="sxs-lookup"><span data-stu-id="6efc5-380">Run Webpack in *release* mode.</span></span> <span data-ttu-id="6efc5-381">Выполните следующие команды в окне **Консоль диспетчера пакетов** в корневом элементе проекта.</span><span class="sxs-lookup"><span data-stu-id="6efc5-381">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="6efc5-382">Если вы не в корневом каталоге проекта, введите перед этой командой `cd SignalRWebPack`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-382">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="6efc5-383">Выберите **Отладка** > **Запуск без отладки**, чтобы запустить приложение в браузере, не присоединяя отладчик.</span><span class="sxs-lookup"><span data-stu-id="6efc5-383">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="6efc5-384">Файл *wwwroot/index.html* обрабатывается по адресу `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-384">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="6efc5-385">Откройте другой экземпляр любого браузера.</span><span class="sxs-lookup"><span data-stu-id="6efc5-385">Open another browser instance (any browser).</span></span> <span data-ttu-id="6efc5-386">Вставьте URL-адрес в адресную строку.</span><span class="sxs-lookup"><span data-stu-id="6efc5-386">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="6efc5-387">Выберите любой браузер, введите произвольный текст в поле **Сообщение** и нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="6efc5-387">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="6efc5-388">На обеих страницах мгновенно отображаются имя пользователя и сообщение.</span><span class="sxs-lookup"><span data-stu-id="6efc5-388">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6efc5-389">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6efc5-389">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="6efc5-390">Запустите средство Webpack в режиме *release*, выполнив следующую команду в корневом элементе проекта:</span><span class="sxs-lookup"><span data-stu-id="6efc5-390">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="6efc5-391">Выполните построение и запустите приложение, выполнив следующую команду в корневом элементе проекта:</span><span class="sxs-lookup"><span data-stu-id="6efc5-391">Build and run the app by executing the following command in the project root:</span></span>

    ```dotnetcli
    dotnet run
    ```

    <span data-ttu-id="6efc5-392">Веб-сервер запускает приложение и делает его доступным на узле localhost.</span><span class="sxs-lookup"><span data-stu-id="6efc5-392">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="6efc5-393">Откройте браузер с адресом `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="6efc5-393">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="6efc5-394">Обрабатывается файл *wwwroot/index.html*.</span><span class="sxs-lookup"><span data-stu-id="6efc5-394">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="6efc5-395">Скопируйте URL-адрес из адресной строки.</span><span class="sxs-lookup"><span data-stu-id="6efc5-395">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="6efc5-396">Откройте другой экземпляр любого браузера.</span><span class="sxs-lookup"><span data-stu-id="6efc5-396">Open another browser instance (any browser).</span></span> <span data-ttu-id="6efc5-397">Вставьте URL-адрес в адресную строку.</span><span class="sxs-lookup"><span data-stu-id="6efc5-397">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="6efc5-398">Выберите любой браузер, введите произвольный текст в поле **Сообщение** и нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="6efc5-398">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="6efc5-399">На обеих страницах мгновенно отображаются имя пользователя и сообщение.</span><span class="sxs-lookup"><span data-stu-id="6efc5-399">The unique user name and message are displayed on both pages instantly.</span></span>

---

![Сообщение, отображаемое в обоих окнах браузера](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="6efc5-401">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6efc5-401">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
