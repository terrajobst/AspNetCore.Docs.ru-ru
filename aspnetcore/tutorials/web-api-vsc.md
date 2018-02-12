---
title: "Создание веб-API с помощью ASP.NET Core и VS Code"
author: rick-anderson
description: "Создание веб-API на платформах macOS, Linux или Windows с помощью ASP.NET Core MVC и Visual Studio Code"
manager: wpickett
ms.author: riande
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: 44566c4014400aa2ca3d512eeaa226637b5f0b97
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="e85c8-103">Создание веб-API с помощью ASP.NET Core MVC и Visual Studio Code на платформах macOS, Windows и Linux</span><span class="sxs-lookup"><span data-stu-id="e85c8-103">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="e85c8-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Майк Уоссон](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="e85c8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="e85c8-105">В этом руководстве создается веб-API для управления элементами списка дел.</span><span class="sxs-lookup"><span data-stu-id="e85c8-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="e85c8-106">При этом пользовательский интерфейс не создается.</span><span class="sxs-lookup"><span data-stu-id="e85c8-106">A UI isn't constructed.</span></span>

<span data-ttu-id="e85c8-107">Существует 3 версии этого учебника:</span><span class="sxs-lookup"><span data-stu-id="e85c8-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="e85c8-108">macOS, Linux, Windows: создание веб-API с помощью Visual Studio Code (этот учебник)</span><span class="sxs-lookup"><span data-stu-id="e85c8-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="e85c8-109">macOS: [создание веб-API с помощью Visual Studio для Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="e85c8-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="e85c8-110">Windows: [создание веб-API с помощью Visual Studio для Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="e85c8-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="e85c8-111">Настройка среды разработки</span><span class="sxs-lookup"><span data-stu-id="e85c8-111">Set up your development environment</span></span>

<span data-ttu-id="e85c8-112">Скачайте и установите следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="e85c8-112">Download and install:</span></span>
- <span data-ttu-id="e85c8-113">[Пакет SDK .NET Core 2.0.0](https://www.microsoft.com/net/core) или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="e85c8-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
- [<span data-ttu-id="e85c8-114">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e85c8-114">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="e85c8-115">[Расширение C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e85c8-115">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="e85c8-116">Создание проекта</span><span class="sxs-lookup"><span data-stu-id="e85c8-116">Create the project</span></span>

<span data-ttu-id="e85c8-117">Из консоли выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="e85c8-117">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="e85c8-118">Откройте папку *TodoApi* в Visual Studio Code (VS Code) и выберите файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="e85c8-118">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="e85c8-119">В **предупреждении** "Required assets to build and debug are missing from 'TodoApi'.Add them?" (В TodoApi отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?) нажмите кнопку **Да**</span><span class="sxs-lookup"><span data-stu-id="e85c8-119">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="e85c8-120">.</span><span class="sxs-lookup"><span data-stu-id="e85c8-120">Add them?"</span></span>
- <span data-ttu-id="e85c8-121">В **информационном** сообщении "There are unresolved dependencies" (Имеются неразрешенные зависимости) щелкните **Восстановить**.</span><span class="sxs-lookup"><span data-stu-id="e85c8-121">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code с предупреждением "Required assets to build and debug are missing from 'TodoApi'.Add them?" (В TodoApi отсутствуют необходимые ресурсы для сборки и отладки.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="e85c8-125">Нажмите клавишу **отладки** (F5), чтобы выполнить сборку программы и запустить ее.</span><span class="sxs-lookup"><span data-stu-id="e85c8-125">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="e85c8-126">В браузере перейдите по адресу http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="e85c8-126">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="e85c8-127">Отобразится следующее:</span><span class="sxs-lookup"><span data-stu-id="e85c8-127">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="e85c8-128">Советы по использованию VS Code см. в [справке по Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="e85c8-128">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="e85c8-129">Добавление поддержки для Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e85c8-129">Add support for Entity Framework Core</span></span>

<span data-ttu-id="e85c8-130">Создание проекта в .NET Core 2.0 добавляет поставщик "Microsoft.AspNetCore.All" в файл *TodoApi.csproj*.</span><span class="sxs-lookup"><span data-stu-id="e85c8-130">Creating a new project in .NET Core 2.0 adds the 'Microsoft.AspNetCore.All' provider in the *TodoApi.csproj* file.</span></span> <span data-ttu-id="e85c8-131">Устанавливать поставщик базы данных [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) отдельно не требуется.</span><span class="sxs-lookup"><span data-stu-id="e85c8-131">There's no need to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="e85c8-132">Этот поставщик базы данных позволяет использовать Entity Framework Core с выполняющейся в памяти базой данных.</span><span class="sxs-lookup"><span data-stu-id="e85c8-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a><span data-ttu-id="e85c8-133">Добавление класса модели</span><span class="sxs-lookup"><span data-stu-id="e85c8-133">Add a model class</span></span>

<span data-ttu-id="e85c8-134">Модель — это объект, представляющий данные в приложении.</span><span class="sxs-lookup"><span data-stu-id="e85c8-134">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="e85c8-135">В этом случае единственной моделью является задача.</span><span class="sxs-lookup"><span data-stu-id="e85c8-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="e85c8-136">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="e85c8-136">Add a folder named *Models*.</span></span> <span data-ttu-id="e85c8-137">Классы моделей можно размещать в любом месте в проекте, но по соглашению используется папка *Models*.</span><span class="sxs-lookup"><span data-stu-id="e85c8-137">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="e85c8-138">Добавьте класс `TodoItem` с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="e85c8-138">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="e85c8-139">После создания `TodoItem` база данных сформирует `Id`.</span><span class="sxs-lookup"><span data-stu-id="e85c8-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="e85c8-140">Создание контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="e85c8-140">Create the database context</span></span>

<span data-ttu-id="e85c8-141">*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для заданной модели данных.</span><span class="sxs-lookup"><span data-stu-id="e85c8-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="e85c8-142">Этот класс создается путем наследования от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="e85c8-142">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="e85c8-143">Добавьте класс `TodoContext` в папку *Models*:</span><span class="sxs-lookup"><span data-stu-id="e85c8-143">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="e85c8-144">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="e85c8-144">Add a controller</span></span>

<span data-ttu-id="e85c8-145">В папке *Controllers* создайте класс с именем `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="e85c8-145">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="e85c8-146">Добавьте следующий код:</span><span class="sxs-lookup"><span data-stu-id="e85c8-146">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="e85c8-147">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="e85c8-147">Launch the app</span></span>

<span data-ttu-id="e85c8-148">В VS Code нажмите клавишу F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="e85c8-148">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="e85c8-149">Перейдите по адресу http://localhost:5000/api/todo (только что созданный контроллер `Todo`).</span><span class="sxs-lookup"><span data-stu-id="e85c8-149">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="e85c8-150">Справка по Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e85c8-150">Visual Studio Code help</span></span>

- [<span data-ttu-id="e85c8-151">Начало работы</span><span class="sxs-lookup"><span data-stu-id="e85c8-151">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="e85c8-152">Отладка</span><span class="sxs-lookup"><span data-stu-id="e85c8-152">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="e85c8-153">Интегрированный терминал</span><span class="sxs-lookup"><span data-stu-id="e85c8-153">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="e85c8-154">Сочетания клавиш</span><span class="sxs-lookup"><span data-stu-id="e85c8-154">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="e85c8-155">Сочетания клавиш для Mac</span><span class="sxs-lookup"><span data-stu-id="e85c8-155">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="e85c8-156">Сочетания клавиш для Linux</span><span class="sxs-lookup"><span data-stu-id="e85c8-156">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="e85c8-157">Сочетания клавиш для Windows</span><span class="sxs-lookup"><span data-stu-id="e85c8-157">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


