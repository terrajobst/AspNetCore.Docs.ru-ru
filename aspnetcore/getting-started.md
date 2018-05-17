---
title: Начало работы с ASP.NET Core
author: rick-anderson
description: Краткий учебник, в котором с помощью ASP.NET Core создается и запускается простое приложение Hello World.
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: c2f18c69901a5a6503314d508a776e6985872681
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="4dddc-103">Начало работы с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4dddc-103">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="4dddc-104">Эти инструкции предназначены для последней версии ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4dddc-104">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="4dddc-105">Документ для версии 1.1 см. в разделе [Начало работы с ASP.NET Core 1.1](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="4dddc-105">See [Getting Started with ASP.NET Core 1.1](xref:getting-started-1.1) for the 1.1 version of this document.</span></span>

1. <span data-ttu-id="4dddc-106">Установите [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="4dddc-106">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="4dddc-107">Создайте проект .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4dddc-107">Create a new .NET Core project.</span></span>

   <span data-ttu-id="4dddc-108">В macOS или Linux откройте окно терминала.</span><span class="sxs-lookup"><span data-stu-id="4dddc-108">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="4dddc-109">В Windows откройте окно командной строки.</span><span class="sxs-lookup"><span data-stu-id="4dddc-109">On Windows, open a command prompt.</span></span> <span data-ttu-id="4dddc-110">Введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="4dddc-110">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
3. <span data-ttu-id="4dddc-111">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="4dddc-111">Run the app.</span></span>

    <span data-ttu-id="4dddc-112">Используйте следующие команды для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="4dddc-112">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="4dddc-113">Перейдите по адресу [http://localhost:5000](http://localhost:5000)</span><span class="sxs-lookup"><span data-stu-id="4dddc-113">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

5. <span data-ttu-id="4dddc-114">Откройте файл <em>Pages/About.cshtml</em> и измените страницу, чтобы на ней отображалось сообщение "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="4dddc-114">Open <em>Pages/About.cshtml</em> and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="4dddc-115">Время на сервере — @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="4dddc-115">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="4dddc-116">Перейдите к [http://localhost:5000/About](http://localhost:5000/About) и проверьте изменения.</span><span class="sxs-lookup"><span data-stu-id="4dddc-116">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="4dddc-117">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="4dddc-117">Next steps</span></span>

<span data-ttu-id="4dddc-118">Учебники по началу работы см. на странице [Учебники по ASP.NET Core](tutorials/index.md).</span><span class="sxs-lookup"><span data-stu-id="4dddc-118">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="4dddc-119">Сведения об основных понятиях и архитектуре ASP.NET Core см. в статьях [Введение в ASP.NET Core](index.md) и [Основы ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="4dddc-119">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="4dddc-120">Приложение ASP.NET Core может использовать библиотеку базовых классов и среду выполнения .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4dddc-120">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="4dddc-121">Дополнительные сведения см. в статье [Выбор между .NET Core и .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="4dddc-121">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
