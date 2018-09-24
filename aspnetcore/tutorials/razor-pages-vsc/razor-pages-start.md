---
title: Начало работы с Razor Pages ASP.NET Core в Visual Studio Code
author: rick-anderson
description: Основные сведения о создании веб-приложении Razor Pages в ASP.NET Core с помощью Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: b7f6ca377a892fce912dc0ee9d4b7378f40fbf24
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/21/2018
ms.locfileid: "46522931"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a><span data-ttu-id="ba978-103">Начало работы с Razor Pages ASP.NET Core в Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ba978-103">Get started with ASP.NET Core Razor Pages in Visual Studio Code</span></span>

<span data-ttu-id="ba978-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="ba978-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ba978-105">В этом учебнике приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ba978-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="ba978-106">Перед работой с этим учебником рекомендуем изучить статью [Введение в Razor Pages](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="ba978-106">We recommend you complete [Introduction to Razor Pages](xref:razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="ba978-107">Razor Pages — это рекомендуемый способ создания пользовательского интерфейса для веб-приложений в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ba978-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ba978-108">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="ba978-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="ba978-109">Создание веб-приложения Razor</span><span class="sxs-lookup"><span data-stu-id="ba978-109">Create a Razor web app</span></span>

<span data-ttu-id="ba978-110">Из терминала выполните следующие команды.</span><span class="sxs-lookup"><span data-stu-id="ba978-110">From a terminal, run the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

<span data-ttu-id="ba978-111">Указанные выше команды используют [интерфейс командной строки .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) для создания и запуска проекта Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="ba978-111">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="ba978-112">Откройте в браузере страницу http://localhost:5000 для просмотра приложения.</span><span class="sxs-lookup"><span data-stu-id="ba978-112">Open a browser to http://localhost:5000 to view the application.</span></span>

![Домашняя или индексная страница](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="ba978-114">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="ba978-114">Open the project</span></span>

<span data-ttu-id="ba978-115">Нажмите клавиши CTRL+C, чтобы завершить работу приложения.</span><span class="sxs-lookup"><span data-stu-id="ba978-115">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="ba978-116">В Visual Studio Code (VS Code) откройте меню **Файл > Открыть папку** и выберите папку *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="ba978-116">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="ba978-117">Нажмите кнопку **Да** в **предупреждении** "Required assets to build and debug are missing from 'TodoApi'.Add them?" (В TodoApi отсутствуют необходимые ресурсы для сборки и отладки.</span><span class="sxs-lookup"><span data-stu-id="ba978-117">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="ba978-118">Добавить их?).</span><span class="sxs-lookup"><span data-stu-id="ba978-118">Add them?"</span></span>
- <span data-ttu-id="ba978-119">Щелкните **Восстановить** в **информационном** сообщении "There are unresolved dependencies" (Имеются неразрешенные зависимости).</span><span class="sxs-lookup"><span data-stu-id="ba978-119">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="ba978-120">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="ba978-120">Launch the app</span></span>

<span data-ttu-id="ba978-121">Нажмите клавиши CTRL+F5, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="ba978-121">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="ba978-122">Либо в меню **Отладка** выберите пункт **Запуск без отладки**.</span><span class="sxs-lookup"><span data-stu-id="ba978-122">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="ba978-123">В следующем учебнике мы добавим в проект модель.</span><span class="sxs-lookup"><span data-stu-id="ba978-123">In the next tutorial, we add a model to the project.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="ba978-124">Далее: добавление модели</span><span class="sxs-lookup"><span data-stu-id="ba978-124">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
