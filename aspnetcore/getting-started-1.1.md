---
title: "Начало работы с ASP.NET Core 1.1"
author: rick-anderson
description: "Краткий учебник, в котором с помощью ASP.NET Core 1.1 создается и запускается простое приложение Hello World."
keywords: "ASP.NET Core, учебник, начало работы"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started-1.1
ms.openlocfilehash: e8fd9ef60ebc1cff6ca0e03000ea50eebff0a9f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-aspnet-core-11"></a><span data-ttu-id="3346b-104">Начало работы с ASP.NET Core 1.1</span><span class="sxs-lookup"><span data-stu-id="3346b-104">Getting Started with ASP.NET Core 1.1</span></span>

> [!NOTE]
> <span data-ttu-id="3346b-105">Эти инструкции предназначены для ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="3346b-105">These instructions are for ASP.NET Core 1.1.</span></span> <span data-ttu-id="3346b-106">Нужны сведения о последней версии?</span><span class="sxs-lookup"><span data-stu-id="3346b-106">Looking for the latest version?</span></span> <span data-ttu-id="3346b-107">См. [текущую версию этого учебника](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="3346b-107">See [the current version of this tutorial](xref:getting-started).</span></span>

1. <span data-ttu-id="3346b-108">Запустите **установщик пакета SDK** версии 1.0.4 для .NET Core, который можно получить на [странице скачивания пакета SDK версии 1.0.4 для .NET Core 1.0.5 и 1.1.2](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span><span class="sxs-lookup"><span data-stu-id="3346b-108">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core 1.0.5 & 1.1.2 SDK 1.0.4 downloads page](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span></span>

2. <span data-ttu-id="3346b-109">Создайте папку для нового проекта .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3346b-109">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="3346b-110">В macOS или Linux откройте окно терминала.</span><span class="sxs-lookup"><span data-stu-id="3346b-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="3346b-111">В Windows откройте окно командной строки.</span><span class="sxs-lookup"><span data-stu-id="3346b-111">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. <span data-ttu-id="3346b-112">Если на компьютере установлена более поздняя версия пакета SDK, создайте файл *global.json*, чтобы выбрать пакет SDK версии 1.0.4.</span><span class="sxs-lookup"><span data-stu-id="3346b-112">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. <span data-ttu-id="3346b-113">Создайте проект .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3346b-113">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```
   
3.  <span data-ttu-id="3346b-114">Восстановите пакеты.</span><span class="sxs-lookup"><span data-stu-id="3346b-114">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

4. <span data-ttu-id="3346b-115">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="3346b-115">Run the app.</span></span>

   <span data-ttu-id="3346b-116">При необходимости команда `dotnet run` сначала выполняет сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="3346b-116">The `dotnet run` command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="3346b-117">Перейдите по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="3346b-117">Browse to `http://localhost:5000`</span></span>

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a><span data-ttu-id="3346b-118">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="3346b-118">Next steps</span></span>

<span data-ttu-id="3346b-119">Учебники по началу работы см. на странице [Учебники по ASP.NET Core](tutorials/index.md).</span><span class="sxs-lookup"><span data-stu-id="3346b-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="3346b-120">Сведения об основных понятиях и архитектуре ASP.NET Core см. в статьях [Введение в ASP.NET Core](index.md) и [Основы ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="3346b-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="3346b-121">Приложение ASP.NET Core может использовать библиотеку базовых классов и среду выполнения .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="3346b-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="3346b-122">Дополнительные сведения см. в статье [Выбор между .NET Core и .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="3346b-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
