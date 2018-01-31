---
title: "Начало работы с ASP.NET Core 2.0"
author: rick-anderson
description: "Краткий учебник, в котором с помощью ASP.NET Core создается и запускается простое приложение Hello World."
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: eb1fd748029743ca6991927cc95b612ed1975338
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="f279b-103">Начало работы с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f279b-103">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="f279b-104">Эти инструкции предназначены для последней версии ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f279b-104">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="f279b-105">Хотите приступить к работе с более ранней версией?</span><span class="sxs-lookup"><span data-stu-id="f279b-105">Looking to get started with an earlier version?</span></span> <span data-ttu-id="f279b-106">См. [учебник для версии 1.1](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="f279b-106">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="f279b-107">Установите [.NET Core](https://www.microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="f279b-107">Install [.NET Core](https://www.microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="f279b-108">Создайте проект .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f279b-108">Create a new .NET Core project.</span></span>

   <span data-ttu-id="f279b-109">В macOS или Linux откройте окно терминала.</span><span class="sxs-lookup"><span data-stu-id="f279b-109">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="f279b-110">В Windows откройте окно командной строки.</span><span class="sxs-lookup"><span data-stu-id="f279b-110">On Windows, open a command prompt.</span></span> <span data-ttu-id="f279b-111">Введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="f279b-111">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. <span data-ttu-id="f279b-112">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="f279b-112">Run the app.</span></span>

    <span data-ttu-id="f279b-113">Используйте следующие команды для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="f279b-113">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="f279b-114">Перейдите по адресу [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="f279b-114">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

6. <span data-ttu-id="f279b-115">Откройте файл *Pages/About.cshtml* и измените страницу, чтобы на ней отображалось сообщение "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="f279b-115">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="f279b-116">Время на сервере — @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="f279b-116">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. <span data-ttu-id="f279b-117">Перейдите по адресу [http://localhost:5000/About](http://localhost:5000/About) и подтвердите изменения.</span><span class="sxs-lookup"><span data-stu-id="f279b-117">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="f279b-118">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="f279b-118">Next steps</span></span>

<span data-ttu-id="f279b-119">Учебники по началу работы см. на странице [Учебники по ASP.NET Core](tutorials/index.md).</span><span class="sxs-lookup"><span data-stu-id="f279b-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="f279b-120">Сведения об основных понятиях и архитектуре ASP.NET Core см. в статьях [Введение в ASP.NET Core](index.md) и [Основы ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="f279b-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="f279b-121">Приложение ASP.NET Core может использовать библиотеку базовых классов и среду выполнения .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f279b-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="f279b-122">Дополнительные сведения см. в статье [Выбор между .NET Core и .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="f279b-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
