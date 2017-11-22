---
title: "Начало работы с ASP.NET Core 2.0"
author: rick-anderson
description: "Краткий учебник, в котором с помощью ASP.NET Core создается и запускается простое приложение Hello World."
keywords: "ASP.NET Core, учебник, начало работы"
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: e944f0f5a3da6d1686ca8a3036666d8dadc9a0f8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="31e6b-104">Начало работы с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="31e6b-104">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="31e6b-105">Эти инструкции предназначены для последней версии ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="31e6b-105">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="31e6b-106">Хотите приступить к работе с более ранней версией?</span><span class="sxs-lookup"><span data-stu-id="31e6b-106">Looking to get started with an earlier version?</span></span> <span data-ttu-id="31e6b-107">См. [учебник для версии 1.1](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="31e6b-107">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="31e6b-108">Установите [.NET Core](https://www.microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="31e6b-108">Install [.NET Core](https://www.microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="31e6b-109">Создайте проект .NET Core.</span><span class="sxs-lookup"><span data-stu-id="31e6b-109">Create a new .NET Core project.</span></span>

   <span data-ttu-id="31e6b-110">В macOS или Linux откройте окно терминала.</span><span class="sxs-lookup"><span data-stu-id="31e6b-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="31e6b-111">В Windows откройте окно командной строки.</span><span class="sxs-lookup"><span data-stu-id="31e6b-111">On Windows, open a command prompt.</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. <span data-ttu-id="31e6b-112">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="31e6b-112">Run the app.</span></span>

    <span data-ttu-id="31e6b-113">Используйте следующие команды для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="31e6b-113">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="31e6b-114">Перейдите по адресу [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="31e6b-114">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

6. <span data-ttu-id="31e6b-115">Откройте файл *Pages/About.cshtml* и измените страницу, чтобы на ней отображалось сообщение "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="31e6b-115">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="31e6b-116">Время на сервере — @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="31e6b-116">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. <span data-ttu-id="31e6b-117">Перейдите по адресу [http://localhost:5000/About](http://localhost:5000/About) и подтвердите изменения.</span><span class="sxs-lookup"><span data-stu-id="31e6b-117">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="31e6b-118">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="31e6b-118">Next steps</span></span>

<span data-ttu-id="31e6b-119">Учебники по началу работы см. на странице [Учебники по ASP.NET Core](tutorials/index.md).</span><span class="sxs-lookup"><span data-stu-id="31e6b-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="31e6b-120">Сведения об основных понятиях и архитектуре ASP.NET Core см. в статьях [Введение в ASP.NET Core](index.md) и [Основы ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="31e6b-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="31e6b-121">Приложение ASP.NET Core может использовать библиотеку базовых классов и среду выполнения .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="31e6b-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="31e6b-122">Дополнительные сведения см. в статье [Выбор между .NET Core и .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="31e6b-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
