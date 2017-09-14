---
title: "Начало работы с Razor Pages в ASP.NET Core с Visual Studio Code"
author: rick-anderson
description: "Начало работы с Razor Pages в ASP.NET Core с использованием Visual Studio Code"
keywords: "ASP.NET Core,Razor Pages,формирование шаблонов,Entity Framework Core,EF,EF Core,база данных,mac,macOS,Visual Studio Code,Code"
ms.author: riande
manager: wpickett
ms.date: 8/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: aa39de71addb2499af6d322db6da0ec635c54970
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a><span data-ttu-id="f176d-104">Начало работы с Razor Pages в ASP.NET Core с Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f176d-104">Getting started with Razor Pages in ASP.NET Core with Visual Studio Code</span></span>

<span data-ttu-id="f176d-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f176d-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f176d-106">В этом учебнике приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f176d-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="f176d-107">Перед работой с этим учебником рекомендуем изучить статью [Введение в Razor Pages](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="f176d-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="f176d-108">Razor Pages — это рекомендуемый способ создания пользовательского интерфейса для веб-приложений в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f176d-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f176d-109">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="f176d-109">Prerequisites</span></span>

<span data-ttu-id="f176d-110">Установите следующее.</span><span class="sxs-lookup"><span data-stu-id="f176d-110">Install the following:</span></span>

* <span data-ttu-id="f176d-111">[.NET Core 2.0.0 SDK](https://dot.net/core) или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="f176d-111">[.NET Core 2.0.0 SDK](https://dot.net/core) or later</span></span>
* [<span data-ttu-id="f176d-112">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="f176d-112">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="f176d-113">Расширение VS Code [C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="f176d-113">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-razor-web-app"></a><span data-ttu-id="f176d-114">Создание веб-приложения Razor</span><span class="sxs-lookup"><span data-stu-id="f176d-114">Create a Razor web app</span></span>

<span data-ttu-id="f176d-115">Из терминала выполните следующие команды.</span><span class="sxs-lookup"><span data-stu-id="f176d-115">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="f176d-116">Указанные выше команды используют [интерфейс командной строки .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) для создания и запуска проекта Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="f176d-116">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="f176d-117">Откройте в браузере страницу http://localhost:5000 для просмотра приложения.</span><span class="sxs-lookup"><span data-stu-id="f176d-117">Open a browser to http://localhost:5000 to view the application.</span></span>

![Домашняя или индексная страница](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="f176d-119">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="f176d-119">Open the project</span></span>

<span data-ttu-id="f176d-120">Нажмите клавиши CTRL+C, чтобы завершить работу приложения.</span><span class="sxs-lookup"><span data-stu-id="f176d-120">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="f176d-121">В Visual Studio Code (VS Code) откройте меню **Файл > Открыть папку** и выберите папку *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="f176d-121">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="f176d-122">Нажмите кнопку **Да** в **предупреждении** "Required assets to build and debug are missing from 'TodoApi'.Add them?" (В TodoApi отсутствуют необходимые ресурсы для сборки и отладки.</span><span class="sxs-lookup"><span data-stu-id="f176d-122">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="f176d-123">Добавить их?).</span><span class="sxs-lookup"><span data-stu-id="f176d-123">Add them?"</span></span>
- <span data-ttu-id="f176d-124">Щелкните **Восстановить** в **информационном** сообщении "There are unresolved dependencies" (Имеются неразрешенные зависимости).</span><span class="sxs-lookup"><span data-stu-id="f176d-124">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="f176d-125">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="f176d-125">Launch the app</span></span>

<span data-ttu-id="f176d-126">Нажмите клавиши CTRL+F5, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="f176d-126">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="f176d-127">Либо в меню **Отладка** выберите пункт **Запуск без отладки**.</span><span class="sxs-lookup"><span data-stu-id="f176d-127">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="f176d-128">В следующем учебнике мы добавим в проект модель.</span><span class="sxs-lookup"><span data-stu-id="f176d-128">In the next tutorial, we add a model to the project.</span></span> 

>[!div class="step-by-step"]
[<span data-ttu-id="f176d-129">Далее: добавление модели</span><span class="sxs-lookup"><span data-stu-id="f176d-129">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
