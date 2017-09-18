---
title: "Начало работы с ASP.NET Core 2.0"
author: rick-anderson
description: "Краткий учебник, в котором с помощью ASP.NET Core создается и запускается простое приложение Hello World."
keywords: "ASP.NET Core, учебник, начало работы"
ms.author: riande
manager: wpickett
ms.date: 08/30/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: f7852f0dddb0585089f5ccd8f4c865f5b87b049b
ms.sourcegitcommit: fb518f856f31fe53c09196a13309eacb85b37a22
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/08/2017
---
# <a name="getting-started-with-aspnet-core"></a><span data-ttu-id="9ccf5-104">Начало работы с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9ccf5-104">Getting Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="9ccf5-105">Эти инструкции предназначены для последней версии ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9ccf5-105">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="9ccf5-106">Хотите приступить к работе с более ранней версией?</span><span class="sxs-lookup"><span data-stu-id="9ccf5-106">Looking to get started with an earlier version?</span></span> <span data-ttu-id="9ccf5-107">См. [учебник для версии 1.1](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="9ccf5-107">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="9ccf5-108">Установите [.NET Core](https://microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="9ccf5-108">Install [.NET Core](https://microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="9ccf5-109">Создайте проект .NET Core.</span><span class="sxs-lookup"><span data-stu-id="9ccf5-109">Create a new .NET Core project.</span></span>

   <span data-ttu-id="9ccf5-110">В macOS или Linux откройте окно терминала.</span><span class="sxs-lookup"><span data-stu-id="9ccf5-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="9ccf5-111">В Windows откройте окно командной строки.</span><span class="sxs-lookup"><span data-stu-id="9ccf5-111">On Windows, open a command prompt.</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. <span data-ttu-id="9ccf5-112">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="9ccf5-112">Run the app.</span></span>

    <span data-ttu-id="9ccf5-113">Используйте следующие команды для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="9ccf5-113">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="9ccf5-114">Перейдите по адресу [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="9ccf5-114">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

6. <span data-ttu-id="9ccf5-115">Откройте файл *Pages/About.cshtml* и измените страницу, чтобы на ней отображалось сообщение "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="9ccf5-115">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="9ccf5-116">Время на сервере — @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="9ccf5-116">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="9ccf5-117">[!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="9ccf5-117">[!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

7. <span data-ttu-id="9ccf5-118">Перейдите по адресу [http://localhost:5000/About](http://localhost:5000/About) и подтвердите изменения.</span><span class="sxs-lookup"><span data-stu-id="9ccf5-118">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="9ccf5-119">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="9ccf5-119">Next steps</span></span>

<span data-ttu-id="9ccf5-120">Учебники по началу работы см. на странице [Учебники по ASP.NET Core](tutorials/index.md).</span><span class="sxs-lookup"><span data-stu-id="9ccf5-120">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="9ccf5-121">Сведения об основных понятиях и архитектуре ASP.NET Core см. в статьях [Введение в ASP.NET Core](index.md) и [Основы ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="9ccf5-121">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="9ccf5-122">Приложение ASP.NET Core может использовать библиотеку базовых классов и среду выполнения .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="9ccf5-122">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="9ccf5-123">Дополнительные сведения см. в статье [Выбор между .NET Core и .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="9ccf5-123">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
