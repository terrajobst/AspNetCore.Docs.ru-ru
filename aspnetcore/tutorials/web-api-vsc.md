---
title: Создание веб-API с помощью ASP.NET Core и Visual Studio Code
author: rick-anderson
description: Создание веб-API на платформах macOS, Linux или Windows с помощью ASP.NET Core MVC и Visual Studio Code
ms.author: riande
ms.custom: mvc
ms.date: 07/30/2018
uid: tutorials/web-api-vsc
ms.openlocfilehash: 740110908358a382f20bc1e54e98056296278acf
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089668"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a><span data-ttu-id="3acb3-103">Создание веб-API с помощью ASP.NET Core и Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3acb3-103">Create a Web API with ASP.NET Core and Visual Studio Code</span></span>

<span data-ttu-id="3acb3-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Майк Уоссон](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="3acb3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="3acb3-105">В этом руководстве создается веб-API для управления элементами списка дел.</span><span class="sxs-lookup"><span data-stu-id="3acb3-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="3acb3-106">При этом пользовательский интерфейс не создается.</span><span class="sxs-lookup"><span data-stu-id="3acb3-106">A UI isn't constructed.</span></span>

<span data-ttu-id="3acb3-107">Существуют три версии этого руководства:</span><span class="sxs-lookup"><span data-stu-id="3acb3-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="3acb3-108">macOS, Linux, Windows: создание веб-API с помощью Visual Studio Code (этот учебник)</span><span class="sxs-lookup"><span data-stu-id="3acb3-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="3acb3-109">macOS: [создание веб-API с помощью Visual Studio для Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="3acb3-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="3acb3-110">Windows: [создание веб-API с помощью Visual Studio для Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="3acb3-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="3acb3-111">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="3acb3-111">Prerequisites</span></span>

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

<span data-ttu-id="3acb3-112">Советы по использованию VS Code см. в [справке по Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="3acb3-112">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="3acb3-113">Создание проекта</span><span class="sxs-lookup"><span data-stu-id="3acb3-113">Create the project</span></span>

<span data-ttu-id="3acb3-114">Из консоли выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="3acb3-114">From a console, run the following commands:</span></span>

```console
dotnet new webapi -o TodoApi
code TodoApi
```

<span data-ttu-id="3acb3-115">В Visual Studio Code (VS Code) откроется папка *TodoApi*.</span><span class="sxs-lookup"><span data-stu-id="3acb3-115">The *TodoApi* folder opens in Visual Studio Code (VS Code).</span></span> <span data-ttu-id="3acb3-116">Выберите файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="3acb3-116">Select the *Startup.cs* file.</span></span>

* <span data-ttu-id="3acb3-117">В **предупреждении** "Required assets to build and debug are missing from 'TodoApi'.Add them?" (В TodoApi отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?) нажмите кнопку **Да**</span><span class="sxs-lookup"><span data-stu-id="3acb3-117">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="3acb3-118">.</span><span class="sxs-lookup"><span data-stu-id="3acb3-118">Add them?"</span></span>
* <span data-ttu-id="3acb3-119">В **информационном** сообщении "There are unresolved dependencies" (Имеются неразрешенные зависимости) щелкните **Восстановить**.</span><span class="sxs-lookup"><span data-stu-id="3acb3-119">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code с предупреждением "Required assets to build and debug are missing from 'TodoApi'.Add them?" (В TodoApi отсутствуют необходимые ресурсы для сборки и отладки.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="3acb3-123">Нажмите клавишу **отладки** (F5), чтобы выполнить сборку программы и запустить ее.</span><span class="sxs-lookup"><span data-stu-id="3acb3-123">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="3acb3-124">В браузере перейдите на адрес http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="3acb3-124">In a browser, navigate to http://localhost:5000/api/values.</span></span> <span data-ttu-id="3acb3-125">Выводится следующий результат.</span><span class="sxs-lookup"><span data-stu-id="3acb3-125">The following output is displayed:</span></span>

```json
["value1","value2"]
```



## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="3acb3-126">Добавление поддержки для Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="3acb3-126">Add support for Entity Framework Core</span></span>

:::moniker range=">= aspnetcore-2.1"

<span data-ttu-id="3acb3-127">При создании проекта в ASP.NET Core 2.1 или более поздней версии в файл проекта добавляется метапакет [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="3acb3-127">Creating a new project in ASP.NET Core 2.1 or later adds the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) to the project file:</span></span>

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

:::moniker range="<= aspnetcore-2.0"

<span data-ttu-id="3acb3-128">При создании проекта в ASP.NET Core 2.0 в файл проекта добавляется метапакет [Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="3acb3-128">Creating a new project in ASP.NET Core 2.0 adds the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) to the project file:</span></span>

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

<span data-ttu-id="3acb3-129">Устанавливать поставщик базы данных [Entity Framework Core InMemory](/ef/core/providers/in-memory/) отдельно не требуется.</span><span class="sxs-lookup"><span data-stu-id="3acb3-129">There's no need to install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="3acb3-130">Этот поставщик базы данных позволяет использовать Entity Framework Core с выполняющейся в памяти базой данных.</span><span class="sxs-lookup"><span data-stu-id="3acb3-130">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="3acb3-131">Добавление класса модели</span><span class="sxs-lookup"><span data-stu-id="3acb3-131">Add a model class</span></span>

<span data-ttu-id="3acb3-132">Модель — это объект, представляющий данные в приложении.</span><span class="sxs-lookup"><span data-stu-id="3acb3-132">A model is an object representing the data in your app.</span></span> <span data-ttu-id="3acb3-133">В этом случае единственной моделью является задача.</span><span class="sxs-lookup"><span data-stu-id="3acb3-133">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="3acb3-134">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="3acb3-134">Add a folder named *Models*.</span></span> <span data-ttu-id="3acb3-135">Классы моделей можно размещать в любом месте в проекте, но по соглашению используется папка *Models*.</span><span class="sxs-lookup"><span data-stu-id="3acb3-135">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="3acb3-136">Добавьте класс `TodoItem` с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="3acb3-136">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="3acb3-137">После создания `TodoItem` база данных сформирует `Id`.</span><span class="sxs-lookup"><span data-stu-id="3acb3-137">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="3acb3-138">Создание контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="3acb3-138">Create the database context</span></span>

<span data-ttu-id="3acb3-139">*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для заданной модели данных.</span><span class="sxs-lookup"><span data-stu-id="3acb3-139">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="3acb3-140">Этот класс создается путем наследования от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="3acb3-140">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="3acb3-141">Добавьте класс `TodoContext` в папку *Models*:</span><span class="sxs-lookup"><span data-stu-id="3acb3-141">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="3acb3-142">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="3acb3-142">Add a controller</span></span>

<span data-ttu-id="3acb3-143">В папке *Controllers* создайте класс с именем `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="3acb3-143">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="3acb3-144">Замените его содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="3acb3-144">Replace its contents with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="3acb3-145">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="3acb3-145">Launch the app</span></span>

<span data-ttu-id="3acb3-146">В VS Code нажмите клавишу F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="3acb3-146">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="3acb3-147">Перейдите к http://localhost:5000/api/todo (созданному контроллеру `Todo`).</span><span class="sxs-lookup"><span data-stu-id="3acb3-147">Navigate to http://localhost:5000/api/todo (the `Todo` controller we created).</span></span>

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="3acb3-148">Справка по Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3acb3-148">Visual Studio Code help</span></span>

* [<span data-ttu-id="3acb3-149">Начало работы</span><span class="sxs-lookup"><span data-stu-id="3acb3-149">Getting started</span></span>](https://code.visualstudio.com/docs)
* [<span data-ttu-id="3acb3-150">Отладка</span><span class="sxs-lookup"><span data-stu-id="3acb3-150">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
* [<span data-ttu-id="3acb3-151">Интегрированный терминал</span><span class="sxs-lookup"><span data-stu-id="3acb3-151">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [<span data-ttu-id="3acb3-152">Сочетания клавиш</span><span class="sxs-lookup"><span data-stu-id="3acb3-152">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [<span data-ttu-id="3acb3-153">Сочетания клавиш для macOS</span><span class="sxs-lookup"><span data-stu-id="3acb3-153">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [<span data-ttu-id="3acb3-154">Сочетания клавиш для Linux</span><span class="sxs-lookup"><span data-stu-id="3acb3-154">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [<span data-ttu-id="3acb3-155">Сочетания клавиш для Windows</span><span class="sxs-lookup"><span data-stu-id="3acb3-155">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
