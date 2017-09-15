---
title: "Начало работы с ASP.NET Core 2.0"
author: rick-anderson
description: "Краткий учебник, в котором с помощью ASP.NET Core создается и запускается простое приложение Hello World."
keywords: "ASP.NET Core, учебник, начало работы"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: 3399df3958093da9117b013736b1cb370fd6beb2
ms.sourcegitcommit: 297ee5d2f3b3b24eb8a2c4a25195c9e2973cb91b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/14/2017
---
# <a name="getting-started-with-aspnet-core"></a><span data-ttu-id="a5844-104">Начало работы с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a5844-104">Getting Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="a5844-105">Эти инструкции предназначены для последней версии ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a5844-105">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="a5844-106">Хотите приступить к работе с более ранней версией?</span><span class="sxs-lookup"><span data-stu-id="a5844-106">Looking to get started with an earlier version?</span></span> <span data-ttu-id="a5844-107">См. [учебник для версии 1.1](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="a5844-107">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="a5844-108">Установите [.NET Core](https://microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="a5844-108">Install [.NET Core](https://microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="a5844-109">Создайте проект .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a5844-109">Create a new .NET Core project.</span></span>

   <span data-ttu-id="a5844-110">В macOS или Linux откройте окно терминала.</span><span class="sxs-lookup"><span data-stu-id="a5844-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="a5844-111">В Windows откройте окно командной строки.</span><span class="sxs-lookup"><span data-stu-id="a5844-111">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   dotnet new web
   ```
    
4. <span data-ttu-id="a5844-112">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="a5844-112">Run the app.</span></span>

   <span data-ttu-id="a5844-113">При необходимости команда `dotnet run` сначала выполняет сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="a5844-113">The `dotnet run` command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="a5844-114">Перейдите по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a5844-114">Browse to `http://localhost:5000`</span></span>

### <a name="next-steps"></a><span data-ttu-id="a5844-115">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="a5844-115">Next steps</span></span>

<span data-ttu-id="a5844-116">Учебники по началу работы см. на странице [Учебники по ASP.NET Core](tutorials/index.md).</span><span class="sxs-lookup"><span data-stu-id="a5844-116">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="a5844-117">Сведения об основных понятиях и архитектуре ASP.NET Core см. в статьях [Введение в ASP.NET Core](index.md) и [Основы ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="a5844-117">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="a5844-118">Приложение ASP.NET Core может использовать библиотеку базовых классов и среду выполнения .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a5844-118">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="a5844-119">Дополнительные сведения см. в статье [Выбор между .NET Core и .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="a5844-119">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
