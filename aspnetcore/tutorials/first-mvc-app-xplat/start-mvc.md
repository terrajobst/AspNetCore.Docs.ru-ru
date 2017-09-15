---
title: "Введение в ASP.NET Core MVC для Mac, Linux и Windows"
author: rick-anderson
description: "Начало работы с MVC ASP.NET Core и Visual Studio Code в операционных системах Mac, Linux и Windows"
keywords: ASP.NET Core, MVC, VS Code, Visual Studio Code, Mac, Linux, Windows
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: get-started-article
ms.assetid: 1d18b589-1638-4dc6-1638-fb0f41998d78
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 82ce5bbc695b190759ac2c05cdceebb5f7854eb7
ms.sourcegitcommit: e6a8f171f26fab1b2195a2d7f14e7d258a2e690e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/23/2017
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a><span data-ttu-id="f01a5-104">Начало работы с MVC ASP.NET Core в операционных системах Mac, Linux и Windows</span><span class="sxs-lookup"><span data-stu-id="f01a5-104">Getting started with ASP.NET Core MVC  on Mac, Linux, or Windows</span></span>

<span data-ttu-id="f01a5-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="f01a5-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f01a5-106">В этом учебнике представлены основы построения веб-приложений MVC ASP.NET Core с помощью [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span><span class="sxs-lookup"><span data-stu-id="f01a5-106">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="f01a5-107">Для работы с этим руководством требуется знание VS Code.</span><span class="sxs-lookup"><span data-stu-id="f01a5-107">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="f01a5-108">Дополнительные сведения см. в разделах [Начало работы с VS Code](https://code.visualstudio.com/docs) и [Справка по Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="f01a5-108">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> [!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="f01a5-109">Существует 3 версии этого учебника:</span><span class="sxs-lookup"><span data-stu-id="f01a5-109">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="f01a5-110">macOS: [Создание приложения ASP.NET Core MVC с помощью Visual Studio для Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="f01a5-110">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="f01a5-111">Windows: [Создание приложения ASP.NET Core MVC с помощью Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="f01a5-111">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="f01a5-112">macOS, Linux и Windows: [Создание приложения MVC ASP.NET Core с помощью Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="f01a5-112">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="install-vs-code-and-net-core"></a><span data-ttu-id="f01a5-113">Установка VS Code и .NET Core</span><span class="sxs-lookup"><span data-stu-id="f01a5-113">Install VS Code and .NET Core</span></span>

<span data-ttu-id="f01a5-114">Для этого учебника требуется [пакет SDK для .NET Core 2.0.0](https://dot.net/core) или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="f01a5-114">This tutorial requires the [.NET Core 2.0.0 SDK](https://dot.net/core) or later.</span></span> <span data-ttu-id="f01a5-115">См. [этот PDF-файл](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) для версии ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="f01a5-115">See [the pdf](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="f01a5-116">Установите следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="f01a5-116">Install the following:</span></span>

* <span data-ttu-id="f01a5-117">[Пакет SDK .NET Core 2.0.0](https://dot.net/core) или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="f01a5-117">[.NET Core 2.0.0 SDK](https://dot.net/core) or later.</span></span>
* [<span data-ttu-id="f01a5-118">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="f01a5-118">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="f01a5-119">Расширение VS Code [C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="f01a5-119">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="f01a5-120">Создание веб-приложения с dotnet</span><span class="sxs-lookup"><span data-stu-id="f01a5-120">Create a web app with dotnet</span></span>

<span data-ttu-id="f01a5-121">Из терминала выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="f01a5-121">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="f01a5-122">Откройте папку *MvcMovie* в Visual Studio Code (VS Code) и выберите файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="f01a5-122">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="f01a5-123">Нажмите кнопку **Да** в **предупреждении** "Required assets to build and debug are missing from 'MvcMovie'.</span><span class="sxs-lookup"><span data-stu-id="f01a5-123">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="f01a5-124">Add them?" (В MvcMovie отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?)</span><span class="sxs-lookup"><span data-stu-id="f01a5-124">Add them?"</span></span>
- <span data-ttu-id="f01a5-125">В **информационном** сообщении "There are unresolved dependencies" (Имеются неразрешенные зависимости) щелкните **Восстановить**.</span><span class="sxs-lookup"><span data-stu-id="f01a5-125">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![VS Code с предупреждением "Required assets to build and debug are missing from 'MvcMovie'.Add them?" (В MvcMovie отсутствуют необходимые ресурсы для сборки и отладки.](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="f01a5-129">Нажмите клавишу **отладки** (F5), чтобы выполнить сборку программы и запустить ее.</span><span class="sxs-lookup"><span data-stu-id="f01a5-129">Press **Debug** (F5) to build and run the program.</span></span>

![Запуск приложения](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="f01a5-131">VS Code запускает веб-сервер [Kestrel](xref:fundamentals/servers/kestrel) и ваше приложение.</span><span class="sxs-lookup"><span data-stu-id="f01a5-131">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="f01a5-132">Обратите внимание на то, что в адресной строке указывается `localhost:5000`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="f01a5-132">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="f01a5-133">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="f01a5-133">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="f01a5-134">Шаблон по умолчанию включает рабочие ссылки **Домой, О программе** и **Контакты**.</span><span class="sxs-lookup"><span data-stu-id="f01a5-134">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="f01a5-135">На представленном выше снимке экрана эти ссылки не отображаются.</span><span class="sxs-lookup"><span data-stu-id="f01a5-135">The browser image above doesn't show these links.</span></span> <span data-ttu-id="f01a5-136">Если размер окна вашего браузера слишком мал, нажмите значок навигации, чтобы отобразить эти ссылки.</span><span class="sxs-lookup"><span data-stu-id="f01a5-136">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Значок навигации в правом верхнем углу экрана](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="f01a5-138">В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.</span><span class="sxs-lookup"><span data-stu-id="f01a5-138">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="f01a5-139">Справка по Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f01a5-139">Visual Studio Code help</span></span>

- [<span data-ttu-id="f01a5-140">Начало работы</span><span class="sxs-lookup"><span data-stu-id="f01a5-140">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="f01a5-141">Отладка</span><span class="sxs-lookup"><span data-stu-id="f01a5-141">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="f01a5-142">Интегрированный терминал</span><span class="sxs-lookup"><span data-stu-id="f01a5-142">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="f01a5-143">Сочетания клавиш</span><span class="sxs-lookup"><span data-stu-id="f01a5-143">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="f01a5-144">Сочетания клавиш для Mac</span><span class="sxs-lookup"><span data-stu-id="f01a5-144">Mac keyboard shortcuts</span></span>](https://go.microsoft.com/fwlink/?linkid=832143)
  - [<span data-ttu-id="f01a5-145">Сочетания клавиш для Linux</span><span class="sxs-lookup"><span data-stu-id="f01a5-145">Linux keyboard shortcuts</span></span>](https://go.microsoft.com/fwlink/?linkid=832144)
  - [<span data-ttu-id="f01a5-146">Сочетания клавиш для Windows</span><span class="sxs-lookup"><span data-stu-id="f01a5-146">Windows keyboard shortcuts</span></span>](https://go.microsoft.com/fwlink/?linkid=832145)

>[!div class="step-by-step"]
[<span data-ttu-id="f01a5-147">Следующая статья — "Добавление контроллера"</span><span class="sxs-lookup"><span data-stu-id="f01a5-147">Next - Add a controller</span></span>](adding-controller.md)
