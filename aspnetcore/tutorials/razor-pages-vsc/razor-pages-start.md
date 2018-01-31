---
title: "Начало работы с Razor Pages в ASP.NET Core с Visual Studio Code"
author: rick-anderson
description: "Начало работы с Razor Pages в ASP.NET Core с использованием Visual Studio Code"
manager: wpickett
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 7c01d802e59951281c86c8eab64b7c6b9d646fbf
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a><span data-ttu-id="82a73-103">Начало работы с Razor Pages в ASP.NET Core с Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="82a73-103">Getting started with Razor Pages in ASP.NET Core with Visual Studio Code</span></span>

<span data-ttu-id="82a73-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="82a73-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="82a73-105">В этом учебнике приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="82a73-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="82a73-106">Перед работой с этим учебником рекомендуем изучить статью [Введение в Razor Pages](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="82a73-106">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="82a73-107">Razor Pages — это рекомендуемый способ создания пользовательского интерфейса для веб-приложений в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="82a73-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="82a73-108">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="82a73-108">Prerequisites</span></span>

<span data-ttu-id="82a73-109">Установите следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="82a73-109">Install the following:</span></span>

* <span data-ttu-id="82a73-110">[пакет SDK для .NET Core 2.0.0](https://www.microsoft.com/net/core) или более поздней версии;</span><span class="sxs-lookup"><span data-stu-id="82a73-110">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="82a73-111">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="82a73-111">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="82a73-112">Расширение VS Code [C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="82a73-112">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-razor-web-app"></a><span data-ttu-id="82a73-113">Создание веб-приложения Razor</span><span class="sxs-lookup"><span data-stu-id="82a73-113">Create a Razor web app</span></span>

<span data-ttu-id="82a73-114">Из терминала выполните следующие команды.</span><span class="sxs-lookup"><span data-stu-id="82a73-114">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="82a73-115">Указанные выше команды используют [интерфейс командной строки .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) для создания и запуска проекта Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="82a73-115">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="82a73-116">Откройте в браузере страницу http://localhost:5000 для просмотра приложения.</span><span class="sxs-lookup"><span data-stu-id="82a73-116">Open a browser to http://localhost:5000 to view the application.</span></span>

![Домашняя или индексная страница](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="82a73-118">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="82a73-118">Open the project</span></span>

<span data-ttu-id="82a73-119">Нажмите клавиши CTRL+C, чтобы завершить работу приложения.</span><span class="sxs-lookup"><span data-stu-id="82a73-119">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="82a73-120">В Visual Studio Code (VS Code) откройте меню **Файл > Открыть папку** и выберите папку *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="82a73-120">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="82a73-121">Нажмите кнопку **Да** в **предупреждении** "Required assets to build and debug are missing from 'TodoApi'.Add them?" (В TodoApi отсутствуют необходимые ресурсы для сборки и отладки.</span><span class="sxs-lookup"><span data-stu-id="82a73-121">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="82a73-122">Добавить их?).</span><span class="sxs-lookup"><span data-stu-id="82a73-122">Add them?"</span></span>
- <span data-ttu-id="82a73-123">Щелкните **Восстановить** в **информационном** сообщении "There are unresolved dependencies" (Имеются неразрешенные зависимости).</span><span class="sxs-lookup"><span data-stu-id="82a73-123">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="82a73-124">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="82a73-124">Launch the app</span></span>

<span data-ttu-id="82a73-125">Нажмите клавиши CTRL+F5, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="82a73-125">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="82a73-126">Либо в меню **Отладка** выберите пункт **Запуск без отладки**.</span><span class="sxs-lookup"><span data-stu-id="82a73-126">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="82a73-127">В следующем учебнике мы добавим в проект модель.</span><span class="sxs-lookup"><span data-stu-id="82a73-127">In the next tutorial, we add a model to the project.</span></span> 

>[!div class="step-by-step"]
[<span data-ttu-id="82a73-128">Далее: добавление модели</span><span class="sxs-lookup"><span data-stu-id="82a73-128">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
