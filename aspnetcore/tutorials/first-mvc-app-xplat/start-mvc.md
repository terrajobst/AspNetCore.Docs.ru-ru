---
title: Введение в ASP.NET Core MVC для macOS, Linux и Windows
author: rick-anderson
description: Узнайте, как начать работу с MVC ASP.NET Core и Visual Studio Code в операционных системах macOS, Linux и Windows
ms.author: riande
ms.date: 07/07/2017
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: b25ee29541e345a3bf6700b67d992409c02b183a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275279"
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a><span data-ttu-id="9158c-103">Введение в ASP.NET Core MVC для macOS, Linux и Windows</span><span class="sxs-lookup"><span data-stu-id="9158c-103">Introduction to ASP.NET Core MVC on macOS, Linux, or Windows</span></span>

<span data-ttu-id="9158c-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="9158c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9158c-105">В этом учебнике представлены основы построения веб-приложений MVC ASP.NET Core с помощью [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span><span class="sxs-lookup"><span data-stu-id="9158c-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="9158c-106">Для работы с этим руководством требуется знание VS Code.</span><span class="sxs-lookup"><span data-stu-id="9158c-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="9158c-107">Дополнительные сведения см. в разделах [Начало работы с VS Code](https://code.visualstudio.com/docs) и [Справка по Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="9158c-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="9158c-108">Существует 3 версии этого учебника:</span><span class="sxs-lookup"><span data-stu-id="9158c-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="9158c-109">macOS: [Создание приложения ASP.NET Core MVC с помощью Visual Studio для Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="9158c-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="9158c-110">Windows: [Создание приложения ASP.NET Core MVC с помощью Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="9158c-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="9158c-111">macOS, Linux и Windows: [Создание приложения MVC ASP.NET Core с помощью Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="9158c-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="9158c-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="9158c-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="9158c-113">Создание веб-приложения с dotnet</span><span class="sxs-lookup"><span data-stu-id="9158c-113">Create a web app with dotnet</span></span>

<span data-ttu-id="9158c-114">Из терминала выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="9158c-114">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="9158c-115">Откройте папку *MvcMovie* в Visual Studio Code (VS Code) и выберите файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="9158c-115">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="9158c-116">Нажмите кнопку **Да** в **предупреждении** "Required assets to build and debug are missing from 'MvcMovie'.</span><span class="sxs-lookup"><span data-stu-id="9158c-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="9158c-117">Add them?" (В MvcMovie отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?)</span><span class="sxs-lookup"><span data-stu-id="9158c-117">Add them?"</span></span>
- <span data-ttu-id="9158c-118">Щелкните **Восстановить** в **информационном** сообщении "There are unresolved dependencies" (Имеются неразрешенные зависимости).</span><span class="sxs-lookup"><span data-stu-id="9158c-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![VS Code с предупреждением "Required assets to build and debug are missing from 'MvcMovie'.Add them?" (В MvcMovie отсутствуют необходимые ресурсы для сборки и отладки.](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="9158c-122">Нажмите клавишу **отладки** (F5), чтобы выполнить сборку программы и запустить ее.</span><span class="sxs-lookup"><span data-stu-id="9158c-122">Press **Debug** (F5) to build and run the program.</span></span>

![Запуск приложения](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="9158c-124">VS Code запускает веб-сервер [Kestrel](xref:fundamentals/servers/kestrel) и ваше приложение.</span><span class="sxs-lookup"><span data-stu-id="9158c-124">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="9158c-125">Обратите внимание на то, что в адресной строке указывается `localhost:5000`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="9158c-125">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="9158c-126">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="9158c-126">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="9158c-127">Шаблон по умолчанию включает рабочие ссылки **Домой, О программе** и **Контакты**.</span><span class="sxs-lookup"><span data-stu-id="9158c-127">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="9158c-128">На представленном выше снимке экрана эти ссылки не отображаются.</span><span class="sxs-lookup"><span data-stu-id="9158c-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="9158c-129">Если размер окна вашего браузера слишком мал, нажмите значок навигации, чтобы отобразить эти ссылки.</span><span class="sxs-lookup"><span data-stu-id="9158c-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Значок навигации в правом верхнем углу экрана](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="9158c-131">В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.</span><span class="sxs-lookup"><span data-stu-id="9158c-131">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="9158c-132">Справка по Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9158c-132">Visual Studio Code help</span></span>

- [<span data-ttu-id="9158c-133">Начало работы</span><span class="sxs-lookup"><span data-stu-id="9158c-133">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="9158c-134">Отладка</span><span class="sxs-lookup"><span data-stu-id="9158c-134">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="9158c-135">Интегрированный терминал</span><span class="sxs-lookup"><span data-stu-id="9158c-135">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="9158c-136">Сочетания клавиш</span><span class="sxs-lookup"><span data-stu-id="9158c-136">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="9158c-137">Сочетания клавиш для macOS</span><span class="sxs-lookup"><span data-stu-id="9158c-137">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="9158c-138">Сочетания клавиш для Linux</span><span class="sxs-lookup"><span data-stu-id="9158c-138">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="9158c-139">Сочетания клавиш для Windows</span><span class="sxs-lookup"><span data-stu-id="9158c-139">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

> [!div class="step-by-step"]
> [<span data-ttu-id="9158c-140">Следующая статья — "Добавление контроллера"</span><span class="sxs-lookup"><span data-stu-id="9158c-140">Next - Add a controller</span></span>](adding-controller.md)
