---
title: Разработка приложений ASP.NET Core с использованием наблюдателя файлов
author: rick-anderson
description: В этом руководстве показано, как установить и использовать наблюдатель за файлами .NET Core CLI (dotnet watch) в приложении ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 05/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/dotnet-watch
ms.openlocfilehash: 016ee107ae646ed43d8a98e97fd2d5b41356910e
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341851"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a><span data-ttu-id="28eaf-103">Разработка приложений ASP.NET Core с использованием наблюдателя файлов</span><span class="sxs-lookup"><span data-stu-id="28eaf-103">Develop ASP.NET Core apps using a file watcher</span></span>

<span data-ttu-id="28eaf-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Виктор Хурдугачи](https://twitter.com/victorhurdugaci) (Victor Hurdugaci)</span><span class="sxs-lookup"><span data-stu-id="28eaf-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="28eaf-105">`dotnet watch` — это средство, которое запускает команду [.NET Core CLI](/dotnet/core/tools) при изменении исходных файлов.</span><span class="sxs-lookup"><span data-stu-id="28eaf-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="28eaf-106">Например, в результате изменения файла может выполняться компиляция, тестирование или развертывание.</span><span class="sxs-lookup"><span data-stu-id="28eaf-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="28eaf-107">В этом руководстве используется уже существующее приложение веб-API с двумя конечными точками, одна из которых возвращает сумму, а другая — произведение.</span><span class="sxs-lookup"><span data-stu-id="28eaf-107">This tutorial uses an existing web API with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="28eaf-108">В методе произведения есть ошибка, которая исправлена в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="28eaf-108">The product method has a bug, which is fixed in this tutorial.</span></span>

<span data-ttu-id="28eaf-109">Скачайте [образец приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="28eaf-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="28eaf-110">Он содержит два проекта: *WebApp* (веб-API ASP.NET Core) и *WebAppTests* (модульные тесты для веб-API).</span><span class="sxs-lookup"><span data-stu-id="28eaf-110">It consists of two projects: *WebApp* (an ASP.NET Core web API) and *WebAppTests* (unit tests for the web API).</span></span>

<span data-ttu-id="28eaf-111">В командной оболочке перейдите в папку *WebApp*.</span><span class="sxs-lookup"><span data-stu-id="28eaf-111">In a command shell, navigate to the *WebApp* folder.</span></span> <span data-ttu-id="28eaf-112">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="28eaf-112">Run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="28eaf-113">В выходных данных в консоли будут показаны аналогичные следующим сообщения, которые свидетельствуют о том, что приложение выполняется и ожидает запросов:</span><span class="sxs-lookup"><span data-stu-id="28eaf-113">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="28eaf-114">В браузере перейдите на адрес `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="28eaf-114">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="28eaf-115">Должен появиться результат `9`.</span><span class="sxs-lookup"><span data-stu-id="28eaf-115">You should see the result of `9`.</span></span>

<span data-ttu-id="28eaf-116">Перейдите к API продукта (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="28eaf-116">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="28eaf-117">Он возвращает `9`, а не `20`, как ожидалось.</span><span class="sxs-lookup"><span data-stu-id="28eaf-117">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="28eaf-118">Эта проблема устраняется далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="28eaf-118">That problem is fixed later in the tutorial.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="28eaf-119">Добавление `dotnet watch` в проект</span><span class="sxs-lookup"><span data-stu-id="28eaf-119">Add `dotnet watch` to a project</span></span>

<span data-ttu-id="28eaf-120">Средство наблюдения за файлами `dotnet watch` входит в версию 2.1.300 пакета SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="28eaf-120">The `dotnet watch` file watcher tool is included with version 2.1.300 of the .NET Core SDK.</span></span> <span data-ttu-id="28eaf-121">Выполните следующие действия при использовании более ранней версии пакета SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="28eaf-121">The following steps are required when using an earlier version of the .NET Core SDK.</span></span>

1. <span data-ttu-id="28eaf-122">Добавьте ссылку на пакет `Microsoft.DotNet.Watcher.Tools` в *CSPROJ*-файл:</span><span class="sxs-lookup"><span data-stu-id="28eaf-122">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup>
    ```

1. <span data-ttu-id="28eaf-123">Установите пакет `Microsoft.DotNet.Watcher.Tools`, запустив следующую команду:</span><span class="sxs-lookup"><span data-stu-id="28eaf-123">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>

    ```console
    dotnet restore
    ```

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="28eaf-124">Запуск команд .NET Core CLI с помощью `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="28eaf-124">Run .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="28eaf-125">Любую [команду .NET Core CLI](/dotnet/core/tools#cli-commands) можно запустить с `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="28eaf-125">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="28eaf-126">Пример:</span><span class="sxs-lookup"><span data-stu-id="28eaf-126">For example:</span></span>

| <span data-ttu-id="28eaf-127">Команда</span><span class="sxs-lookup"><span data-stu-id="28eaf-127">Command</span></span> | <span data-ttu-id="28eaf-128">Команда с контрольным значением</span><span class="sxs-lookup"><span data-stu-id="28eaf-128">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="28eaf-129">dotnet run</span><span class="sxs-lookup"><span data-stu-id="28eaf-129">dotnet run</span></span> | <span data-ttu-id="28eaf-130">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="28eaf-130">dotnet watch run</span></span> |
| <span data-ttu-id="28eaf-131">dotnet run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="28eaf-131">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="28eaf-132">dotnet watch run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="28eaf-132">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="28eaf-133">dotnet run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="28eaf-133">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="28eaf-134">dotnet watch run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="28eaf-134">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="28eaf-135">dotnet test</span><span class="sxs-lookup"><span data-stu-id="28eaf-135">dotnet test</span></span> | <span data-ttu-id="28eaf-136">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="28eaf-136">dotnet watch test</span></span> |

<span data-ttu-id="28eaf-137">Запустите `dotnet watch run` в папке *WebApp*.</span><span class="sxs-lookup"><span data-stu-id="28eaf-137">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="28eaf-138">В выходных данных консоли будет указано, что запущен `watch`.</span><span class="sxs-lookup"><span data-stu-id="28eaf-138">The console output indicates `watch` has started.</span></span>

## <a name="make-changes-with-dotnet-watch"></a><span data-ttu-id="28eaf-139">Изменения с помощью `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="28eaf-139">Make changes with `dotnet watch`</span></span>

<span data-ttu-id="28eaf-140">Убедитесь, что `dotnet watch` выполняется.</span><span class="sxs-lookup"><span data-stu-id="28eaf-140">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="28eaf-141">Исправьте ошибку в методе `Product` для *MathController.cs* так, чтобы он возвращал произведение, а не сумму чисел.</span><span class="sxs-lookup"><span data-stu-id="28eaf-141">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
}
```

<span data-ttu-id="28eaf-142">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="28eaf-142">Save the file.</span></span> <span data-ttu-id="28eaf-143">В выходных данных консоли будет показано, что `dotnet watch` обнаружил изменение файла и перезапустил приложение.</span><span class="sxs-lookup"><span data-stu-id="28eaf-143">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="28eaf-144">Убедитесь, что `http://localhost:<port number>/api/math/product?a=4&b=5` возвращает правильный результат.</span><span class="sxs-lookup"><span data-stu-id="28eaf-144">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="run-tests-using-dotnet-watch"></a><span data-ttu-id="28eaf-145">Тестирование с помощью `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="28eaf-145">Run tests using `dotnet watch`</span></span>

1. <span data-ttu-id="28eaf-146">Снова измените метод `Product` для *MathController.cs* так, чтобы он возвращал сумму чисел.</span><span class="sxs-lookup"><span data-stu-id="28eaf-146">Change the `Product` method of *MathController.cs* back to returning the sum.</span></span> <span data-ttu-id="28eaf-147">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="28eaf-147">Save the file.</span></span>
1. <span data-ttu-id="28eaf-148">В командной оболочке перейдите в папку *WebAppTests*.</span><span class="sxs-lookup"><span data-stu-id="28eaf-148">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="28eaf-149">Запустите [dotnet restore](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="28eaf-149">Run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>
1. <span data-ttu-id="28eaf-150">Запустите `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="28eaf-150">Run `dotnet watch test`.</span></span> <span data-ttu-id="28eaf-151">В выходных данных будет указано, что проверка не пройдена и наблюдатель ожидает изменений в файле:</span><span class="sxs-lookup"><span data-stu-id="28eaf-151">Its output indicates that a test failed and that the watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="28eaf-152">Исправьте код метода `Product` так, чтобы он возвращал произведение.</span><span class="sxs-lookup"><span data-stu-id="28eaf-152">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="28eaf-153">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="28eaf-153">Save the file.</span></span>

<span data-ttu-id="28eaf-154">`dotnet watch` обнаруживает изменения в файле и повторно запускает тесты.</span><span class="sxs-lookup"><span data-stu-id="28eaf-154">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="28eaf-155">В выходных данных консоли будет указано, что проверка пройдена.</span><span class="sxs-lookup"><span data-stu-id="28eaf-155">The console output indicates the tests passed.</span></span>

## <a name="customize-files-list-to-watch"></a><span data-ttu-id="28eaf-156">Настройка списка файлов для наблюдения</span><span class="sxs-lookup"><span data-stu-id="28eaf-156">Customize files list to watch</span></span>

<span data-ttu-id="28eaf-157">По умолчанию `dotnet-watch` отслеживает все файлы, соответствующие следующим стандартным маскам:</span><span class="sxs-lookup"><span data-stu-id="28eaf-157">By default, `dotnet-watch` tracks all files matching the following glob patterns:</span></span>

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

<span data-ttu-id="28eaf-158">Можно добавить в список наблюдения еще элементы, отредактировав файл *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="28eaf-158">More items can be added to the watch list by editing the *.csproj* file.</span></span> <span data-ttu-id="28eaf-159">Элементы можно указывать по отдельности или с помощью стандартной маски.</span><span class="sxs-lookup"><span data-stu-id="28eaf-159">Items can be specified individually or by using glob patterns.</span></span>

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a><span data-ttu-id="28eaf-160">Удаление файлов из списка наблюдения</span><span class="sxs-lookup"><span data-stu-id="28eaf-160">Opt-out of files to be watched</span></span>

<span data-ttu-id="28eaf-161">Можно настроить `dotnet-watch` так, чтобы игнорировать параметры по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="28eaf-161">`dotnet-watch` can be configured to ignore its default settings.</span></span> <span data-ttu-id="28eaf-162">Чтобы пропускать определенные файлы, добавьте атрибут `Watch="false"` в определение элемента в файле *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="28eaf-162">To ignore specific files, add the `Watch="false"` attribute to an item's definition in the *.csproj* file:</span></span>

```xml
<ItemGroup>
    <!-- exclude Generated.cs from dotnet-watch -->
    <Compile Include="Generated.cs" Watch="false" />

    <!-- exclude Strings.resx from dotnet-watch -->
    <EmbeddedResource Include="Strings.resx" Watch="false" />

    <!-- exclude changes in this referenced project -->
    <ProjectReference Include="..\ClassLibrary1\ClassLibrary1.csproj" Watch="false" />
</ItemGroup>
```

## <a name="custom-watch-projects"></a><span data-ttu-id="28eaf-163">Пользовательское наблюдение за проектами</span><span class="sxs-lookup"><span data-stu-id="28eaf-163">Custom watch projects</span></span>

<span data-ttu-id="28eaf-164">`dotnet-watch` используется не только с проектами C#.</span><span class="sxs-lookup"><span data-stu-id="28eaf-164">`dotnet-watch` isn't restricted to C# projects.</span></span> <span data-ttu-id="28eaf-165">Создайте пользовательское наблюдение за проектами для различных ситуаций.</span><span class="sxs-lookup"><span data-stu-id="28eaf-165">Custom watch projects can be created to handle different scenarios.</span></span> <span data-ttu-id="28eaf-166">Вы можете использовать следующий макет проекта:</span><span class="sxs-lookup"><span data-stu-id="28eaf-166">Consider the following project layout:</span></span>

* <span data-ttu-id="28eaf-167">**test/**</span><span class="sxs-lookup"><span data-stu-id="28eaf-167">**test/**</span></span>
  * <span data-ttu-id="28eaf-168">*UnitTests/UnitTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="28eaf-168">*UnitTests/UnitTests.csproj*</span></span>
  * <span data-ttu-id="28eaf-169">*IntegrationTests/IntegrationTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="28eaf-169">*IntegrationTests/IntegrationTests.csproj*</span></span>

<span data-ttu-id="28eaf-170">Если вы хотите наблюдать за обоими проектами, создайте пользовательский файл проекта, настроенный для наблюдения за обоими проектами:</span><span class="sxs-lookup"><span data-stu-id="28eaf-170">If the goal is to watch both projects, create a custom project file configured to watch both projects:</span></span>

```xml
<Project>
    <ItemGroup>
        <TestProjects Include="**\*.csproj" />
        <Watch Include="**\*.cs" />
    </ItemGroup>

    <Target Name="Test">
        <MSBuild Targets="VSTest" Projects="@(TestProjects)" />
    </Target>

    <Import Project="$(MSBuildExtensionsPath)\Microsoft.Common.targets" />
</Project>
```

<span data-ttu-id="28eaf-171">Чтобы запустить наблюдение за файлами для обоих проектов, внесите изменения в папку *test*.</span><span class="sxs-lookup"><span data-stu-id="28eaf-171">To start file watching on both projects, change to the *test* folder.</span></span> <span data-ttu-id="28eaf-172">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="28eaf-172">Execute the following command:</span></span>

```console
dotnet watch msbuild /t:Test
```

<span data-ttu-id="28eaf-173">VSTest выполняется при изменении любого файла в любом из тестовых проектов.</span><span class="sxs-lookup"><span data-stu-id="28eaf-173">VSTest executes when any file changes in either test project.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="28eaf-174">`dotnet-watch` на GitHub</span><span class="sxs-lookup"><span data-stu-id="28eaf-174">`dotnet-watch` in GitHub</span></span>

<span data-ttu-id="28eaf-175">`dotnet-watch` входит в [репозиторий DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch) на GitHub.</span><span class="sxs-lookup"><span data-stu-id="28eaf-175">`dotnet-watch` is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span></span>
