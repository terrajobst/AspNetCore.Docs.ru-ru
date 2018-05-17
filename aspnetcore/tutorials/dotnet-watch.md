---
title: Разработка приложений ASP.NET Core с использованием dotnet watch
author: rick-anderson
description: В этом учебнике показано, как установить и использовать наблюдатель за файлами .NET Core CLI (dotnet watch) в приложении ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/dotnet-watch
ms.openlocfilehash: c3ece3a5b936b2ea7b7772eee10e598cb557b361
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="develop-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="e4746-103">Разработка приложений ASP.NET Core с использованием dotnet watch</span><span class="sxs-lookup"><span data-stu-id="e4746-103">Develop ASP.NET Core apps using dotnet watch</span></span>

<span data-ttu-id="e4746-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Виктор Хурдугачи](https://twitter.com/victorhurdugaci) (Victor Hurdugaci)</span><span class="sxs-lookup"><span data-stu-id="e4746-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="e4746-105">`dotnet watch` — это средство, которое запускает команду [.NET Core CLI](/dotnet/core/tools) при изменении исходных файлов.</span><span class="sxs-lookup"><span data-stu-id="e4746-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="e4746-106">Например, в результате изменения файла может выполняться компиляция, тестирование или развертывание.</span><span class="sxs-lookup"><span data-stu-id="e4746-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="e4746-107">В этом руководстве используется уже существующее приложение веб-API с двумя конечными точками, одна из которых возвращает сумму, а другая — произведение.</span><span class="sxs-lookup"><span data-stu-id="e4746-107">In this tutorial, we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="e4746-108">В методе product присутствует ошибка, которая будет исправлена в рамках этого руководства.</span><span class="sxs-lookup"><span data-stu-id="e4746-108">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="e4746-109">Скачайте [образец приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="e4746-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="e4746-110">Он содержит два проекта: *WebApp* (веб-API ASP.NET Core) и *WebAppTests* (модульные тесты для веб-API).</span><span class="sxs-lookup"><span data-stu-id="e4746-110">It contains two projects: *WebApp* (an ASP.NET Core Web API) and *WebAppTests* (unit tests for the Web API).</span></span>

<span data-ttu-id="e4746-111">В командной оболочке перейдите в папку *WebApp* и запустите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="e4746-111">In a command shell, navigate to the *WebApp* folder and run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="e4746-112">В выходных данных в консоли будут показаны аналогичные следующим сообщения, которые свидетельствуют о том, что приложение выполняется и ожидает запросов:</span><span class="sxs-lookup"><span data-stu-id="e4746-112">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="e4746-113">В браузере перейдите на адрес `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="e4746-113">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="e4746-114">Должен появиться результат `9`.</span><span class="sxs-lookup"><span data-stu-id="e4746-114">You should see the result of `9`.</span></span>

<span data-ttu-id="e4746-115">Перейдите к API продукта (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="e4746-115">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="e4746-116">Он возвращает `9`, а не `20`, как ожидалось.</span><span class="sxs-lookup"><span data-stu-id="e4746-116">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="e4746-117">Эта ошибка будет исправлена далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="e4746-117">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="e4746-118">Добавление `dotnet watch` в проект</span><span class="sxs-lookup"><span data-stu-id="e4746-118">Add `dotnet watch` to a project</span></span>

1. <span data-ttu-id="e4746-119">Добавьте ссылку на пакет `Microsoft.DotNet.Watcher.Tools` в *CSPROJ*-файл:</span><span class="sxs-lookup"><span data-stu-id="e4746-119">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. <span data-ttu-id="e4746-120">Установите пакет `Microsoft.DotNet.Watcher.Tools`, запустив следующую команду:</span><span class="sxs-lookup"><span data-stu-id="e4746-120">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="e4746-121">Запуск команд .NET Core CLI с помощью `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="e4746-121">Running .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="e4746-122">Любую [команду .NET Core CLI](/dotnet/core/tools#cli-commands) можно запустить с `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="e4746-122">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="e4746-123">Пример:</span><span class="sxs-lookup"><span data-stu-id="e4746-123">For example:</span></span>

| <span data-ttu-id="e4746-124">Команда</span><span class="sxs-lookup"><span data-stu-id="e4746-124">Command</span></span> | <span data-ttu-id="e4746-125">Команда с контрольным значением</span><span class="sxs-lookup"><span data-stu-id="e4746-125">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="e4746-126">dotnet run</span><span class="sxs-lookup"><span data-stu-id="e4746-126">dotnet run</span></span> | <span data-ttu-id="e4746-127">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="e4746-127">dotnet watch run</span></span> |
| <span data-ttu-id="e4746-128">dotnet run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="e4746-128">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="e4746-129">dotnet watch run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="e4746-129">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="e4746-130">dotnet run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="e4746-130">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="e4746-131">dotnet watch run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="e4746-131">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="e4746-132">dotnet test</span><span class="sxs-lookup"><span data-stu-id="e4746-132">dotnet test</span></span> | <span data-ttu-id="e4746-133">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="e4746-133">dotnet watch test</span></span> |

<span data-ttu-id="e4746-134">Запустите `dotnet watch run` в папке *WebApp*.</span><span class="sxs-lookup"><span data-stu-id="e4746-134">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="e4746-135">В выходных данных консоли будет указано, что запущен `watch`.</span><span class="sxs-lookup"><span data-stu-id="e4746-135">The console output indicates `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="e4746-136">Внесение изменений с использованием `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="e4746-136">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="e4746-137">Убедитесь, что `dotnet watch` выполняется.</span><span class="sxs-lookup"><span data-stu-id="e4746-137">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="e4746-138">Исправьте ошибку в методе `Product` для *MathController.cs* так, чтобы он возвращал произведение, а не сумму чисел.</span><span class="sxs-lookup"><span data-stu-id="e4746-138">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="e4746-139">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="e4746-139">Save the file.</span></span> <span data-ttu-id="e4746-140">В выходных данных консоли будет показано, что `dotnet watch` обнаружил изменение файла и перезапустил приложение.</span><span class="sxs-lookup"><span data-stu-id="e4746-140">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="e4746-141">Убедитесь, что `http://localhost:<port number>/api/math/product?a=4&b=5` возвращает правильный результат.</span><span class="sxs-lookup"><span data-stu-id="e4746-141">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="e4746-142">Выполнение тестов с использованием `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="e4746-142">Running tests using `dotnet watch`</span></span>

1. <span data-ttu-id="e4746-143">Снова измените метод `Product` для *MathController.cs* так, чтобы он возвращал сумму чисел, после чего сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="e4746-143">Change the `Product` method of *MathController.cs* back to returning the sum and save the file.</span></span>
1. <span data-ttu-id="e4746-144">В командной оболочке перейдите в папку *WebAppTests*.</span><span class="sxs-lookup"><span data-stu-id="e4746-144">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="e4746-145">Запустите [dotnet restore](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="e4746-145">Run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>
1. <span data-ttu-id="e4746-146">Запустите `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="e4746-146">Run `dotnet watch test`.</span></span> <span data-ttu-id="e4746-147">В выходных данных будет указано, что проверка не пройдена и наблюдатель ожидает изменений в файле:</span><span class="sxs-lookup"><span data-stu-id="e4746-147">Its output indicates that a test failed and that watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="e4746-148">Исправьте код метода `Product` так, чтобы он возвращал произведение.</span><span class="sxs-lookup"><span data-stu-id="e4746-148">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="e4746-149">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="e4746-149">Save the file.</span></span>

<span data-ttu-id="e4746-150">`dotnet watch` обнаруживает изменения в файле и повторно запускает тесты.</span><span class="sxs-lookup"><span data-stu-id="e4746-150">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="e4746-151">В выходных данных консоли будет указано, что проверка пройдена.</span><span class="sxs-lookup"><span data-stu-id="e4746-151">The console output indicates the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="e4746-152">dotnet-watch на сайте GitHub</span><span class="sxs-lookup"><span data-stu-id="e4746-152">dotnet-watch in GitHub</span></span>

<span data-ttu-id="e4746-153">dotnet-watch входит в состав [репозитория DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch) на сайте GitHub.</span><span class="sxs-lookup"><span data-stu-id="e4746-153">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span></span>

<span data-ttu-id="e4746-154">В [разделе MSBuild](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) файла [ReadMe для dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) поясняется, как настроить dotnet-watch из файла проекта MSBuild, для которого осуществляется контроль.</span><span class="sxs-lookup"><span data-stu-id="e4746-154">The [MSBuild section](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="e4746-155">Файл [ReadMe для dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) содержит сведения о dotnet-watch, которые отсутствуют в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="e4746-155">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
