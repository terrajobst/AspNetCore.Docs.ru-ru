---
title: Начало работы с ASP.NET Core
author: rick-anderson
description: Краткий учебник, в котором с помощью ASP.NET Core создается и запускается простое приложение Hello World.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: e814277663ff5a964171a71ebb6e0f094e0ddc60
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="798b7-103">Начало работы с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="798b7-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.0"

1. <span data-ttu-id="798b7-104">Установите [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="798b7-104">Install the [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="798b7-105">Создайте проект .NET Core.</span><span class="sxs-lookup"><span data-stu-id="798b7-105">Create a new .NET Core project.</span></span>

   <span data-ttu-id="798b7-106">В macOS или Linux откройте окно терминала.</span><span class="sxs-lookup"><span data-stu-id="798b7-106">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="798b7-107">В Windows откройте окно командной строки.</span><span class="sxs-lookup"><span data-stu-id="798b7-107">On Windows, open a command prompt.</span></span> <span data-ttu-id="798b7-108">Введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="798b7-108">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="798b7-109">Запустите приложение с помощью следующих команд:</span><span class="sxs-lookup"><span data-stu-id="798b7-109">Run the app with the following commands:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="798b7-110">Перейдите по адресу [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="798b7-110">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="798b7-111">Откройте файл *Pages/About.cshtml* и измените страницу, чтобы на ней отображалось сообщение "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="798b7-111">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="798b7-112">Время на сервере — @DateTime.Now" :</span><span class="sxs-lookup"><span data-stu-id="798b7-112">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="798b7-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="798b7-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

6. <span data-ttu-id="798b7-114">Перейдите к [http://localhost:5000/About](http://localhost:5000/About) и проверьте изменения.</span><span class="sxs-lookup"><span data-stu-id="798b7-114">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="798b7-115">Установите **установщик пакета SDK** версии 1.0.4 для .NET Core со [страницы "Все загрузки .NET Core"](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="798b7-115">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="798b7-116">Создайте папку для нового проекта .NET Core.</span><span class="sxs-lookup"><span data-stu-id="798b7-116">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="798b7-117">В macOS или Linux откройте окно терминала.</span><span class="sxs-lookup"><span data-stu-id="798b7-117">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="798b7-118">В Windows откройте окно командной строки.</span><span class="sxs-lookup"><span data-stu-id="798b7-118">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="798b7-119">Если на компьютере установлена более поздняя версия пакета SDK, создайте файл *global.json*, чтобы выбрать пакет SDK версии 1.0.4.</span><span class="sxs-lookup"><span data-stu-id="798b7-119">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="798b7-120">Создайте проект .NET Core.</span><span class="sxs-lookup"><span data-stu-id="798b7-120">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```

5. <span data-ttu-id="798b7-121">Восстановите пакеты.</span><span class="sxs-lookup"><span data-stu-id="798b7-121">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

6. <span data-ttu-id="798b7-122">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="798b7-122">Run the app.</span></span>

   ```terminal
   dotnet run
   ```

   <span data-ttu-id="798b7-123">При необходимости команда [dotnet run](/dotnet/core/tools/dotnet-run) сначала выполняет сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="798b7-123">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="798b7-124">Перейдите по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="798b7-124">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end