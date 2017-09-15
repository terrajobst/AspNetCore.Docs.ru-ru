---
title: "Разработка приложений ASP.NET Core с использованием dotnet watch"
author: rick-anderson
description: "Показывает способы использования dotnet watch."
keywords: "ASP.NET Core, использование dotnet watch"
ms.author: riande
manager: wpickett
ms.date: 03/09/2017
ms.topic: article
ms.assetid: 563ffb3f-d369-4aa5-bf0a-7300b4e7832c
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: 30e0d07bdfbd16a475e03c1a21cdd10220bd1630
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="4e499-104">Разработка приложений ASP.NET Core с использованием dotnet watch</span><span class="sxs-lookup"><span data-stu-id="4e499-104">Developing ASP.NET Core apps using dotnet watch</span></span>


<span data-ttu-id="4e499-105">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Виктор Хурдугачи](https://twitter.com/victorhurdugaci) (Victor Hurdugaci)</span><span class="sxs-lookup"><span data-stu-id="4e499-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="4e499-106">Средство `dotnet watch` выполняет команду `dotnet` при изменении файлов исходного кода.</span><span class="sxs-lookup"><span data-stu-id="4e499-106">`dotnet watch` is a tool that runs a `dotnet` command when source files change.</span></span> <span data-ttu-id="4e499-107">Например, в результате изменения файла может выполняться компиляция, тестирование или развертывание.</span><span class="sxs-lookup"><span data-stu-id="4e499-107">For example, a file change can trigger compilation, tests, or deployment.</span></span>

<span data-ttu-id="4e499-108">В этом руководстве мы используем существующее приложение веб-API с двумя контрольными точками, одна из которых возвращает сумму, а другая произведение.</span><span class="sxs-lookup"><span data-stu-id="4e499-108">In this tutorial we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="4e499-109">В методе product присутствует ошибка, которая будет исправлена в рамках этого руководства.</span><span class="sxs-lookup"><span data-stu-id="4e499-109">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="4e499-110">Скачайте [образец приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="4e499-110">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="4e499-111">В нем содержатся два проекта: `WebApp` (веб-приложение) и `WebAppTests` (модульные тесты для веб-приложения).</span><span class="sxs-lookup"><span data-stu-id="4e499-111">It contains two projects, `WebApp` (a web app) and `WebAppTests` (unit tests for the web app).</span></span>

<span data-ttu-id="4e499-112">В консоли перейдите в папку WebApp и выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="4e499-112">In a console, navigate to the WebApp folder and run the following commands:</span></span>

- `dotnet restore`
- `dotnet run`

<span data-ttu-id="4e499-113">В выходных данных в консоли будут показаны аналогичные следующим сообщения, которые свидетельствуют о том, что приложение выполняется и ожидает запросов:</span><span class="sxs-lookup"><span data-stu-id="4e499-113">The console output will show messages similar to the following (indicating that the app is running and waiting for requests):</span></span>

```console
$ dotnet run
Hosting environment: Production
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="4e499-114">В веб-браузере перейдите по адресу `http://localhost:5000/api/math/sum?a=4&b=5`. Должен появиться результат `9`.</span><span class="sxs-lookup"><span data-stu-id="4e499-114">In a web browser, navigate to `http://localhost:5000/api/math/sum?a=4&b=5`, you should see the result `9`.</span></span>

<span data-ttu-id="4e499-115">Перейдите к API product (`http://localhost:5000/api/math/product?a=4&b=5`), который возвращает значение `9`, а не ожидаемое `20`.</span><span class="sxs-lookup"><span data-stu-id="4e499-115">Navigate to the product API (`http://localhost:5000/api/math/product?a=4&b=5`), it returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="4e499-116">Эта ошибка будет исправлена далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="4e499-116">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="4e499-117">Добавление `dotnet watch` в проект</span><span class="sxs-lookup"><span data-stu-id="4e499-117">Add `dotnet watch` to a project</span></span>

- <span data-ttu-id="4e499-118">Добавьте `Microsoft.DotNet.Watcher.Tools` в файл *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="4e499-118">Add `Microsoft.DotNet.Watcher.Tools` to the *.csproj* file:</span></span>
 ```xml
 <ItemGroup>
   <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="1.0.0" />
 </ItemGroup> 
 ```

- <span data-ttu-id="4e499-119">Запустите `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="4e499-119">Run `dotnet restore`.</span></span>

## <a name="running-dotnet-commands-using-dotnet-watch"></a><span data-ttu-id="4e499-120">Выполнение команд `dotnet` с помощью `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="4e499-120">Running `dotnet` commands using `dotnet watch`</span></span>

<span data-ttu-id="4e499-121">Любые команды `dotnet` можно запускать с помощью `dotnet watch`, например:</span><span class="sxs-lookup"><span data-stu-id="4e499-121">Any `dotnet` command can be run with `dotnet watch`, for example:</span></span>

| <span data-ttu-id="4e499-122">Команда</span><span class="sxs-lookup"><span data-stu-id="4e499-122">Command</span></span> | <span data-ttu-id="4e499-123">Команда с контрольным значением</span><span class="sxs-lookup"><span data-stu-id="4e499-123">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="4e499-124">dotnet run</span><span class="sxs-lookup"><span data-stu-id="4e499-124">dotnet run</span></span> | <span data-ttu-id="4e499-125">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="4e499-125">dotnet watch run</span></span> |
| <span data-ttu-id="4e499-126">dotnet run -f net451</span><span class="sxs-lookup"><span data-stu-id="4e499-126">dotnet run -f net451</span></span> | <span data-ttu-id="4e499-127">dotnet watch run -f net451</span><span class="sxs-lookup"><span data-stu-id="4e499-127">dotnet watch run -f net451</span></span> |
| <span data-ttu-id="4e499-128">dotnet run -f net451 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="4e499-128">dotnet run -f net451 -- --arg1</span></span> | <span data-ttu-id="4e499-129">dotnet watch run -f net451 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="4e499-129">dotnet watch run -f net451 -- --arg1</span></span> |
| <span data-ttu-id="4e499-130">dotnet test</span><span class="sxs-lookup"><span data-stu-id="4e499-130">dotnet test</span></span> | <span data-ttu-id="4e499-131">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="4e499-131">dotnet watch test</span></span> |

<span data-ttu-id="4e499-132">Выполните `dotnet watch run` в папке `WebApp`.</span><span class="sxs-lookup"><span data-stu-id="4e499-132">Run `dotnet watch run` in the `WebApp` folder.</span></span> <span data-ttu-id="4e499-133">В выходных данных консоли будет указано, что выполнен запуск `watch`.</span><span class="sxs-lookup"><span data-stu-id="4e499-133">The console output will indicate `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="4e499-134">Внесение изменений с использованием `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="4e499-134">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="4e499-135">Убедитесь, что `dotnet watch` выполняется.</span><span class="sxs-lookup"><span data-stu-id="4e499-135">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="4e499-136">Исправьте ошибку в методе `Product` для `MathController` так, чтобы он возвращал произведение, а не сумму чисел.</span><span class="sxs-lookup"><span data-stu-id="4e499-136">Fix the bug in the `Product` method of the `MathController` so it returns the product and not the sum.</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="4e499-137">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="4e499-137">Save the file.</span></span> <span data-ttu-id="4e499-138">В выходных данных консоли будут показаны сообщения, указывающие, что `dotnet watch` обнаружил изменение файла и перезапустил приложение.</span><span class="sxs-lookup"><span data-stu-id="4e499-138">The console output will show messages indicating that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="4e499-139">Убедитесь, что `http://localhost:5000/api/math/product?a=4&b=5` возвращает правильный результат.</span><span class="sxs-lookup"><span data-stu-id="4e499-139">Verify `http://localhost:5000/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="4e499-140">Выполнение тестов с использованием `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="4e499-140">Running tests using `dotnet watch`</span></span>

- <span data-ttu-id="4e499-141">Снова измените метод `Product` для `MathController` так, чтобы он возвращал сумму чисел, после чего сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="4e499-141">Change the `Product` method of the `MathController` back to returning the sum and save the file.</span></span>
- <span data-ttu-id="4e499-142">В окне командной строки перейдите в папку `WebAppTests`.</span><span class="sxs-lookup"><span data-stu-id="4e499-142">In a command window, naviagate to the `WebAppTests` folder.</span></span>
- <span data-ttu-id="4e499-143">Запуск `dotnet restore`</span><span class="sxs-lookup"><span data-stu-id="4e499-143">Run `dotnet restore`</span></span>
- <span data-ttu-id="4e499-144">Запустите `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="4e499-144">Run `dotnet watch test`.</span></span> <span data-ttu-id="4e499-145">В выходных данных появятся сообщения о том, что проверка завершилась неудачно и модуль наблюдения ожидает изменений в файле:</span><span class="sxs-lookup"><span data-stu-id="4e499-145">You see output indicating that a test failed and that watcher is waiting for file changes:</span></span>

 ```console
 Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
 Test Run Failed.
  ```
- <span data-ttu-id="4e499-146">Исправьте код метода `Product` так, чтобы он возвращал произведение.</span><span class="sxs-lookup"><span data-stu-id="4e499-146">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="4e499-147">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="4e499-147">Save the file.</span></span>

<span data-ttu-id="4e499-148">`dotnet watch` обнаруживает изменения в файле и повторно запускает тесты.</span><span class="sxs-lookup"><span data-stu-id="4e499-148">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="4e499-149">В выходных данных консоли появляются сообщения об успешном прохождении тестов.</span><span class="sxs-lookup"><span data-stu-id="4e499-149">The console output will show the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="4e499-150">dotnet-watch на сайте GitHub</span><span class="sxs-lookup"><span data-stu-id="4e499-150">dotnet-watch in GitHub</span></span>

<span data-ttu-id="4e499-151">dotnet-watch входит в состав [репозитория DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools) на сайте GitHub.</span><span class="sxs-lookup"><span data-stu-id="4e499-151">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools).</span></span>

<span data-ttu-id="4e499-152">В [разделе MSBuild](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) файла [ReadMe для dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) поясняется, как настроить dotnet-watch из файла проекта MSBuild, для которого осуществляется контроль.</span><span class="sxs-lookup"><span data-stu-id="4e499-152">The [MSBuild section](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="4e499-153">Файл [ReadMe для dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) содержит сведения о dotnet-watch, которые отсутствуют в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="4e499-153">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
