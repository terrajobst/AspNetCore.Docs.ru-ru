---
title: Использование ASP.NET Core SignalR с TypeScript и Webpack
author: ssougnez
description: В рамках этого учебника вы настроите средство Webpack для создания пакета и сборки веб-приложения ASP.NET Core SignalR, клиент которого написан на языке TypeScript.
ms.author: bradyg
ms.custom: mvc
ms.date: 02/11/2019
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: aaf9aa59928ed6b17bc0586d97dbdefc9e30362c
ms.sourcegitcommit: 5e3797a02ff3c48bb8cb9ad4320bfd169ebe8aba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2019
ms.locfileid: "56102956"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a><span data-ttu-id="590cc-103">Использование ASP.NET Core SignalR с TypeScript и Webpack</span><span class="sxs-lookup"><span data-stu-id="590cc-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="590cc-104">Авторы: [Себастьен Сунье (Sébastien Sougnez)](https://twitter.com/ssougnez) и [Скотт Эдди (Scott Addie)](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="590cc-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="590cc-105">С помощью средства [Webpack](https://webpack.js.org/) разработчики могут создавать пакеты и выполнять сборку ресурсов на стороне клиента для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="590cc-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="590cc-106">В этом руководстве демонстрируется использование средства Webpack для создания пакета и сборки веб-приложения ASP.NET Core SignalR, клиент которого написан на языке [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="590cc-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="590cc-107">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="590cc-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="590cc-108">Сформировать шаблон для начального приложения ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="590cc-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="590cc-109">Настроить клиент TypeScript SignalR</span><span class="sxs-lookup"><span data-stu-id="590cc-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="590cc-110">Настроить конвейер сборки с использованием Webpack</span><span class="sxs-lookup"><span data-stu-id="590cc-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="590cc-111">Настроить сервер SignalR</span><span class="sxs-lookup"><span data-stu-id="590cc-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="590cc-112">Обеспечить взаимодействие между клиентом и сервером</span><span class="sxs-lookup"><span data-stu-id="590cc-112">Enable communication between client and server</span></span>

<span data-ttu-id="590cc-113">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="590cc-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-vs-vsc-2.2.md)]

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="590cc-114">Создание веб-приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="590cc-114">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="590cc-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="590cc-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="590cc-116">Настройте Visual Studio на поиск npm в переменной среды *PATH*.</span><span class="sxs-lookup"><span data-stu-id="590cc-116">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="590cc-117">По умолчанию Visual Studio использует версию npm, которая находится в его каталоге установки.</span><span class="sxs-lookup"><span data-stu-id="590cc-117">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="590cc-118">В Visual Studio выполните следующие инструкции:</span><span class="sxs-lookup"><span data-stu-id="590cc-118">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="590cc-119">Выберите **Сервис** > **Параметры** > **Проекты и решения** > **Управление веб-пакетами** > **Внешние веб-инструменты**.</span><span class="sxs-lookup"><span data-stu-id="590cc-119">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="590cc-120">Выберите элемент *$(PATH)* в списке.</span><span class="sxs-lookup"><span data-stu-id="590cc-120">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="590cc-121">Щелкните стрелку вверх, чтобы переместить этот элемент на вторую позицию в списке.</span><span class="sxs-lookup"><span data-stu-id="590cc-121">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Конфигурация Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="590cc-123">Настройка Visual Studio завершена.</span><span class="sxs-lookup"><span data-stu-id="590cc-123">Visual Studio configuration is completed.</span></span> <span data-ttu-id="590cc-124">Теперь следует создать проект.</span><span class="sxs-lookup"><span data-stu-id="590cc-124">It's time to create the project.</span></span>

1. <span data-ttu-id="590cc-125">Перейдите в меню **Файл** > **Создать** > **Проект** и выберите шаблон **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="590cc-125">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="590cc-126">Присвойте проекту имя *SignalRWebPack* и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="590cc-126">Name the project *SignalRWebPack*, and select **OK**.</span></span>
1. <span data-ttu-id="590cc-127">Выберите *.NET Core* в раскрывающемся списке целевых платформ и выберите *ASP.NET Core 2.2* в раскрывающемся списке средства выбора платформы.</span><span class="sxs-lookup"><span data-stu-id="590cc-127">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.2* from the framework selector drop-down.</span></span> <span data-ttu-id="590cc-128">Выберите шаблон **Пустой** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="590cc-128">Select the **Empty** template, and select **OK**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="590cc-129">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="590cc-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="590cc-130">Во **встроенном терминале** выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="590cc-130">Run the following command in the **Integrated Terminal**:</span></span>

```console
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="590cc-131">В каталоге *SignalRWebPack* создается пустое веб-приложение ASP.NET Core, ориентированное на платформу .NET Core.</span><span class="sxs-lookup"><span data-stu-id="590cc-131">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="590cc-132">Настройка Webpack и TypeScript</span><span class="sxs-lookup"><span data-stu-id="590cc-132">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="590cc-133">Далее следует настроить преобразование TypeScript в JavaScript и создание пакета ресурсов на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="590cc-133">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="590cc-134">В корневом элементе проекта выполните следующую команду, чтобы создать файл *package.json*:</span><span class="sxs-lookup"><span data-stu-id="590cc-134">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="590cc-135">Добавьте выделенное свойство в файл *package.json*:</span><span class="sxs-lookup"><span data-stu-id="590cc-135">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    <span data-ttu-id="590cc-136">Если свойству `private` присвоено значение `true`, на следующем шаге не будут отображаться предупреждения об установке пакета.</span><span class="sxs-lookup"><span data-stu-id="590cc-136">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="590cc-137">Установите необходимые пакеты npm.</span><span class="sxs-lookup"><span data-stu-id="590cc-137">Install the required npm packages.</span></span> <span data-ttu-id="590cc-138">Выполните следующую команду в корневом элементе проекта:</span><span class="sxs-lookup"><span data-stu-id="590cc-138">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="590cc-139">Сведения о команде, на которые следует обратить внимание:</span><span class="sxs-lookup"><span data-stu-id="590cc-139">Some command details to note:</span></span>

    * <span data-ttu-id="590cc-140">Для каждого имени пакета номер версии указывается после знака `@`.</span><span class="sxs-lookup"><span data-stu-id="590cc-140">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="590cc-141">npm устанавливает указанные версии пакета.</span><span class="sxs-lookup"><span data-stu-id="590cc-141">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="590cc-142">Использование параметра `-E` позволяет отключить установленное по умолчанию поведение npm, предусматривающее запись операторов диапазона [семантического управления версиями](https://semver.org/) в файл *package.json*.</span><span class="sxs-lookup"><span data-stu-id="590cc-142">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="590cc-143">Например, `"webpack": "4.29.3"` используется вместо `"webpack": "^4.29.3"`.</span><span class="sxs-lookup"><span data-stu-id="590cc-143">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="590cc-144">Этот параметр позволяет исключить непреднамеренное обновление до более новых версий пакета.</span><span class="sxs-lookup"><span data-stu-id="590cc-144">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="590cc-145">Дополнительные сведения см. в официальной документации по [npm-install](https://docs.npmjs.com/cli/install).</span><span class="sxs-lookup"><span data-stu-id="590cc-145">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="590cc-146">Замените свойство `scripts` в файле *package.json* следующим фрагментом кода:</span><span class="sxs-lookup"><span data-stu-id="590cc-146">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="590cc-147">Некоторые пояснения к скриптам:</span><span class="sxs-lookup"><span data-stu-id="590cc-147">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="590cc-148">`build`: создает пакет ваших ресурсов на стороне клиента в режиме разработки и отслеживает изменения.</span><span class="sxs-lookup"><span data-stu-id="590cc-148">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="590cc-149">Наблюдатель за файлами выполняет повторное создание пакета при каждом изменении файла проекта.</span><span class="sxs-lookup"><span data-stu-id="590cc-149">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="590cc-150">Параметр `mode` позволяет отключить оптимизации в рабочей среде, такие как встряхивание дерева и минификация.</span><span class="sxs-lookup"><span data-stu-id="590cc-150">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="590cc-151">В среде разработки следует использовать только `build`.</span><span class="sxs-lookup"><span data-stu-id="590cc-151">Only use `build` in development.</span></span>
    * <span data-ttu-id="590cc-152">`release`: создает пакет ресурсов на стороне клиента в рабочем режиме.</span><span class="sxs-lookup"><span data-stu-id="590cc-152">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="590cc-153">`publish`: запускает скрипт `release` для создания пакета ресурсов на стороне клиента в рабочем режиме.</span><span class="sxs-lookup"><span data-stu-id="590cc-153">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="590cc-154">Этот скрипт вызывает команду [publish](/dotnet/core/tools/dotnet-publish) интерфейса командной строки .NET Core для публикации приложения.</span><span class="sxs-lookup"><span data-stu-id="590cc-154">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="590cc-155">Создайте в корневом элементе проекта файл *webpack.config.js* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="590cc-155">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    <span data-ttu-id="590cc-156">Приведенный выше файл определяет конфигурацию компиляции Webpack.</span><span class="sxs-lookup"><span data-stu-id="590cc-156">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="590cc-157">Сведения о конфигурации, на которые следует обратить внимание:</span><span class="sxs-lookup"><span data-stu-id="590cc-157">Some configuration details to note:</span></span>

    * <span data-ttu-id="590cc-158">Свойство `output` переопределяет значение по умолчанию для *dist*.</span><span class="sxs-lookup"><span data-stu-id="590cc-158">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="590cc-159">Вместо этого пакет выводится в каталог *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="590cc-159">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="590cc-160">Массив `resolve.extensions` содержит элемент *.js* для импорта кода JavaScript клиента SignalR.</span><span class="sxs-lookup"><span data-stu-id="590cc-160">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="590cc-161">Создайте новый каталог *src* в корневом элементе проекта.</span><span class="sxs-lookup"><span data-stu-id="590cc-161">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="590cc-162">В этом каталоге будут храниться ресурсы на стороне клиента для проекта.</span><span class="sxs-lookup"><span data-stu-id="590cc-162">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="590cc-163">Создайте файл *src/index.html* со следующим содержимым.</span><span class="sxs-lookup"><span data-stu-id="590cc-163">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    <span data-ttu-id="590cc-164">Приведенный выше HTML-файл определяет стереотипную разметку главной страницы.</span><span class="sxs-lookup"><span data-stu-id="590cc-164">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="590cc-165">Создайте каталог *src/css*.</span><span class="sxs-lookup"><span data-stu-id="590cc-165">Create a new *src/css* directory.</span></span> <span data-ttu-id="590cc-166">В нем будут храниться файлы *.css* проекта.</span><span class="sxs-lookup"><span data-stu-id="590cc-166">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="590cc-167">Создайте файл *src/css/main.css* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="590cc-167">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    <span data-ttu-id="590cc-168">Приведенный выше файл *main.css* определяет стиль приложения.</span><span class="sxs-lookup"><span data-stu-id="590cc-168">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="590cc-169">Создайте файл *src/tsconfig.json* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="590cc-169">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    <span data-ttu-id="590cc-170">Приведенный выше код определяет конфигурацию компилятора TypeScript для получения совместимого с [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="590cc-170">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="590cc-171">Создайте файл *src/index.ts* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="590cc-171">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="590cc-172">Приведенный выше код TypeScript извлекает ссылки на элементы модели DOM и присоединяет два обработчика событий:</span><span class="sxs-lookup"><span data-stu-id="590cc-172">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="590cc-173">`keyup`: это событие возникает в том случае, если пользователь вводит какой-либо текст в поле, идентифицированное как `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="590cc-173">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="590cc-174">Функция `send` вызывается, когда пользователь нажимает клавишу **ВВОД**.</span><span class="sxs-lookup"><span data-stu-id="590cc-174">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="590cc-175">`click`: это событие возникает, когда пользователь нажимает кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="590cc-175">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="590cc-176">Вызывается функция `send`.</span><span class="sxs-lookup"><span data-stu-id="590cc-176">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="590cc-177">Настройка приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="590cc-177">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="590cc-178">Код в методе `Startup.Configure` отображает текст *Hello World!*.</span><span class="sxs-lookup"><span data-stu-id="590cc-178">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="590cc-179">Замените вызов метода `app.Run` вызовами методов [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) и [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="590cc-179">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="590cc-180">Приведенный выше код позволяет серверу обнаруживать и обрабатывать файл *index.html* независимо от того, вводит ли пользователь полный URL-адрес или URL-адрес корня веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="590cc-180">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="590cc-181">Вызовите метод [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) в методе `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="590cc-181">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="590cc-182">Этот метод добавляет службы SignalR в проект.</span><span class="sxs-lookup"><span data-stu-id="590cc-182">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="590cc-183">Сопоставьте маршрут */hub* с концентратором `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="590cc-183">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="590cc-184">Добавьте следующие строки в конец метода `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="590cc-184">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="590cc-185">Создайте новый каталог *Hubs* в корневом элементе проекта.</span><span class="sxs-lookup"><span data-stu-id="590cc-185">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="590cc-186">Этот каталог служит для хранения концентратора SignalR, созданного на предыдущем шаге.</span><span class="sxs-lookup"><span data-stu-id="590cc-186">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="590cc-187">Создайте концентратор *Hubs/ChatHub.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="590cc-187">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="590cc-188">Добавьте следующий код в самое начало файла *Startup.cs*, чтобы разрешить ссылку `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="590cc-188">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="590cc-189">Обеспечение взаимодействия между клиентом и сервером</span><span class="sxs-lookup"><span data-stu-id="590cc-189">Enable client and server communication</span></span>

<span data-ttu-id="590cc-190">На данный момент приложение отображает простую форму для отправки сообщений.</span><span class="sxs-lookup"><span data-stu-id="590cc-190">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="590cc-191">При попытке сделать это ничего не происходит.</span><span class="sxs-lookup"><span data-stu-id="590cc-191">Nothing happens when you try to do so.</span></span> <span data-ttu-id="590cc-192">Сервер прослушивает конкретный маршрут, но ничего не делает с отправленными сообщениями.</span><span class="sxs-lookup"><span data-stu-id="590cc-192">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="590cc-193">Выполните следующую команду в корневом элементе проекта:</span><span class="sxs-lookup"><span data-stu-id="590cc-193">Execute the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="590cc-194">Приведенная выше команда устанавливает [клиент TypeScript SignalR](https://www.npmjs.com/package/@aspnet/signalr), который позволяет клиенту отправлять сообщения на сервер.</span><span class="sxs-lookup"><span data-stu-id="590cc-194">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@aspnet/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="590cc-195">Добавьте выделенный код в файл *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="590cc-195">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="590cc-196">Приведенный выше код поддерживает получение сообщений от сервера.</span><span class="sxs-lookup"><span data-stu-id="590cc-196">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="590cc-197">Класс `HubConnectionBuilder` создает новый построитель для настройки подключения к серверу.</span><span class="sxs-lookup"><span data-stu-id="590cc-197">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="590cc-198">Функция `withUrl` настраивает URL-адрес концентратора.</span><span class="sxs-lookup"><span data-stu-id="590cc-198">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="590cc-199">SignalR обеспечивает обмен сообщениями между клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="590cc-199">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="590cc-200">Каждому сообщению присваивается определенное имя.</span><span class="sxs-lookup"><span data-stu-id="590cc-200">Each message has a specific name.</span></span> <span data-ttu-id="590cc-201">Например, у вас могут быть сообщения с именем `messageReceived`, которые выполняют логику отображения новых сообщений в соответствующей зоне.</span><span class="sxs-lookup"><span data-stu-id="590cc-201">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="590cc-202">Прослушивание определенных сообщений реализуется с помощью функции `on`.</span><span class="sxs-lookup"><span data-stu-id="590cc-202">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="590cc-203">Возможно прослушивание любого числа имен сообщений.</span><span class="sxs-lookup"><span data-stu-id="590cc-203">You can listen to any number of message names.</span></span> <span data-ttu-id="590cc-204">Кроме того, можно передавать параметры сообщения, например имя его автора или содержимое полученного сообщения.</span><span class="sxs-lookup"><span data-stu-id="590cc-204">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="590cc-205">После того как клиент получает сообщение, создается новый элемент `div`, в атрибуте `innerHTML` которого содержатся имя автора и содержимое сообщения.</span><span class="sxs-lookup"><span data-stu-id="590cc-205">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="590cc-206">Этот элемент добавляется в основной элемент `div`, который используется для отображения сообщений.</span><span class="sxs-lookup"><span data-stu-id="590cc-206">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="590cc-207">Теперь клиент может принимать сообщения. Далее следует настроить его для отправки сообщений.</span><span class="sxs-lookup"><span data-stu-id="590cc-207">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="590cc-208">Добавьте выделенный код в файл *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="590cc-208">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    <span data-ttu-id="590cc-209">Для отправки сообщения через соединение WebSockets необходимо вызвать метод `send`.</span><span class="sxs-lookup"><span data-stu-id="590cc-209">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="590cc-210">Первый параметр этого метода содержит имя сообщения.</span><span class="sxs-lookup"><span data-stu-id="590cc-210">The method's first parameter is the message name.</span></span> <span data-ttu-id="590cc-211">Другие параметры заполняются данными сообщения.</span><span class="sxs-lookup"><span data-stu-id="590cc-211">The message data inhabits the other parameters.</span></span> <span data-ttu-id="590cc-212">В этом примере на сервер отправляется сообщение, идентифицированное как `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="590cc-212">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="590cc-213">Это сообщение содержит имя пользователя, а также данные, введенные этим пользователем в текстовое поле.</span><span class="sxs-lookup"><span data-stu-id="590cc-213">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="590cc-214">Если отправка выполняется успешно, значение текстового поля очищается.</span><span class="sxs-lookup"><span data-stu-id="590cc-214">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="590cc-215">Добавьте выделенный метод в класс `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="590cc-215">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="590cc-216">Приведенный выше код осуществляет широковещательную рассылку полученных сервером сообщений всем подключенным пользователям.</span><span class="sxs-lookup"><span data-stu-id="590cc-216">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="590cc-217">Универсальный метод `on` для получения всех сообщений не требуется.</span><span class="sxs-lookup"><span data-stu-id="590cc-217">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="590cc-218">Имя метода указывается после суффиксов имени сообщения.</span><span class="sxs-lookup"><span data-stu-id="590cc-218">A method named after the message name suffices.</span></span>

    <span data-ttu-id="590cc-219">В этом примере клиент TypeScript отправляет сообщение, идентифицированное как `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="590cc-219">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="590cc-220">Метод C# `NewMessage` ожидает данные, отправленные клиентом.</span><span class="sxs-lookup"><span data-stu-id="590cc-220">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="590cc-221">Выполняется вызов метода [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) для [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="590cc-221">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="590cc-222">Полученные сообщения отправляются всем клиентам, подключенным к концентратору.</span><span class="sxs-lookup"><span data-stu-id="590cc-222">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="590cc-223">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="590cc-223">Test the app</span></span>

<span data-ttu-id="590cc-224">Чтобы проверить работоспособность приложения, выполните следующие шаги.</span><span class="sxs-lookup"><span data-stu-id="590cc-224">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="590cc-225">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="590cc-225">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="590cc-226">Запустите средство Webpack в режиме *release*.</span><span class="sxs-lookup"><span data-stu-id="590cc-226">Run Webpack in *release* mode.</span></span> <span data-ttu-id="590cc-227">Выполните следующие команды в окне **Консоль диспетчера пакетов** в корневом элементе проекта.</span><span class="sxs-lookup"><span data-stu-id="590cc-227">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="590cc-228">Если вы не в корневом каталоге проекта, введите перед этой командой `cd SignalRWebPack`.</span><span class="sxs-lookup"><span data-stu-id="590cc-228">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="590cc-229">Выберите **Отладка** > **Запуск без отладки**, чтобы запустить приложение в браузере, не присоединяя отладчик.</span><span class="sxs-lookup"><span data-stu-id="590cc-229">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="590cc-230">Файл *wwwroot/index.html* обрабатывается по адресу `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="590cc-230">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="590cc-231">Откройте другой экземпляр любого браузера.</span><span class="sxs-lookup"><span data-stu-id="590cc-231">Open another browser instance (any browser).</span></span> <span data-ttu-id="590cc-232">Вставьте URL-адрес в адресную строку.</span><span class="sxs-lookup"><span data-stu-id="590cc-232">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="590cc-233">Выберите любой браузер, введите произвольный текст в поле **Сообщение** и нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="590cc-233">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="590cc-234">На обеих страницах мгновенно отображаются имя пользователя и сообщение.</span><span class="sxs-lookup"><span data-stu-id="590cc-234">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="590cc-235">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="590cc-235">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="590cc-236">Запустите средство Webpack в режиме *release*, выполнив следующую команду в корневом элементе проекта:</span><span class="sxs-lookup"><span data-stu-id="590cc-236">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="590cc-237">Выполните построение и запустите приложение, выполнив следующую команду в корневом элементе проекта:</span><span class="sxs-lookup"><span data-stu-id="590cc-237">Build and run the app by executing the following command in the project root:</span></span>

    ```console
    dotnet run
    ```

    <span data-ttu-id="590cc-238">Веб-сервер запускает приложение и делает его доступным на узле localhost.</span><span class="sxs-lookup"><span data-stu-id="590cc-238">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="590cc-239">Откройте браузер с адресом `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="590cc-239">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="590cc-240">Обрабатывается файл *wwwroot/index.html*.</span><span class="sxs-lookup"><span data-stu-id="590cc-240">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="590cc-241">Скопируйте URL-адрес из адресной строки.</span><span class="sxs-lookup"><span data-stu-id="590cc-241">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="590cc-242">Откройте другой экземпляр любого браузера.</span><span class="sxs-lookup"><span data-stu-id="590cc-242">Open another browser instance (any browser).</span></span> <span data-ttu-id="590cc-243">Вставьте URL-адрес в адресную строку.</span><span class="sxs-lookup"><span data-stu-id="590cc-243">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="590cc-244">Выберите любой браузер, введите произвольный текст в поле **Сообщение** и нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="590cc-244">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="590cc-245">На обеих страницах мгновенно отображаются имя пользователя и сообщение.</span><span class="sxs-lookup"><span data-stu-id="590cc-245">The unique user name and message are displayed on both pages instantly.</span></span>

---

![Сообщение, отображаемое в обоих окнах браузера](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a><span data-ttu-id="590cc-247">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="590cc-247">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
