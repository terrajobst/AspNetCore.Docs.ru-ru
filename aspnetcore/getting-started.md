---
title: "Начало работы с ASP.NET Core 2.0"
author: rick-anderson
description: "Краткий учебник, в котором с помощью ASP.NET Core создается и запускается простое приложение Hello World."
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: b5f1fb0de2776177374b8b4d5ea6041b0fc653a9
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="ea5a4-103">Начало работы с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ea5a4-103">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="ea5a4-104">Эти инструкции предназначены для последней версии ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea5a4-104">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="ea5a4-105">Хотите приступить к работе с более ранней версией?</span><span class="sxs-lookup"><span data-stu-id="ea5a4-105">Looking to get started with an earlier version?</span></span> <span data-ttu-id="ea5a4-106">См. [учебник для версии 1.1](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="ea5a4-106">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="ea5a4-107">Установите [.NET Core](https://www.microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="ea5a4-107">Install [.NET Core](https://www.microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="ea5a4-108">Создайте проект .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea5a4-108">Create a new .NET Core project.</span></span>

   <span data-ttu-id="ea5a4-109">В macOS или Linux откройте окно терминала.</span><span class="sxs-lookup"><span data-stu-id="ea5a4-109">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="ea5a4-110">В Windows откройте окно командной строки.</span><span class="sxs-lookup"><span data-stu-id="ea5a4-110">On Windows, open a command prompt.</span></span> <span data-ttu-id="ea5a4-111">Введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="ea5a4-111">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. <span data-ttu-id="ea5a4-112">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="ea5a4-112">Run the app.</span></span>

    <span data-ttu-id="ea5a4-113">Используйте следующие команды для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="ea5a4-113">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="ea5a4-114">Перейдите по адресу [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="ea5a4-114">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

6. <span data-ttu-id="ea5a4-115">Откройте файл *Pages/About.cshtml* и измените страницу, чтобы на ней отображалось сообщение "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="ea5a4-115">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="ea5a4-116">Время на сервере — @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="ea5a4-116">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. <span data-ttu-id="ea5a4-117">Перейдите по адресу [http://localhost:5000/About](http://localhost:5000/About) и подтвердите изменения.</span><span class="sxs-lookup"><span data-stu-id="ea5a4-117">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="ea5a4-118">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="ea5a4-118">Next steps</span></span>

<span data-ttu-id="ea5a4-119">Учебники по началу работы см. на странице [Учебники по ASP.NET Core](tutorials/index.md).</span><span class="sxs-lookup"><span data-stu-id="ea5a4-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="ea5a4-120">Сведения об основных понятиях и архитектуре ASP.NET Core см. в статьях [Введение в ASP.NET Core](index.md) и [Основы ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="ea5a4-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="ea5a4-121">Приложение ASP.NET Core может использовать библиотеку базовых классов и среду выполнения .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="ea5a4-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="ea5a4-122">Дополнительные сведения см. в статье [Выбор между .NET Core и .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="ea5a4-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
