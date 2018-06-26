---
title: Разработка приложений ASP.NET Core с использованием наблюдателя файлов
author: rick-anderson
description: В этом руководстве показано, как установить и использовать наблюдатель за файлами .NET Core CLI (dotnet watch) в приложении ASP.NET Core.
ms.author: riande
ms.date: 05/31/2018
uid: tutorials/dotnet-watch
ms.openlocfilehash: 2a59267b36faf1e00ea2f0cc7e2b9ceb9828f791
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278857"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a><span data-ttu-id="d242c-103">Разработка приложений ASP.NET Core с использованием наблюдателя файлов</span><span class="sxs-lookup"><span data-stu-id="d242c-103">Develop ASP.NET Core apps using a file watcher</span></span>

<span data-ttu-id="d242c-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Виктор Хурдугачи](https://twitter.com/victorhurdugaci) (Victor Hurdugaci)</span><span class="sxs-lookup"><span data-stu-id="d242c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="d242c-105">`dotnet watch` — это средство, которое запускает команду [.NET Core CLI](/dotnet/core/tools) при изменении исходных файлов.</span><span class="sxs-lookup"><span data-stu-id="d242c-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="d242c-106">Например, в результате изменения файла может выполняться компиляция, тестирование или развертывание.</span><span class="sxs-lookup"><span data-stu-id="d242c-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="d242c-107">В этом руководстве используется уже существующее приложение веб-API с двумя конечными точками, одна из которых возвращает сумму, а другая — произведение.</span><span class="sxs-lookup"><span data-stu-id="d242c-107">This tutorial uses an existing web API with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="d242c-108">В методе произведения есть ошибка, которая исправлена в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="d242c-108">The product method has a bug, which is fixed in this tutorial.</span></span>

<span data-ttu-id="d242c-109">Скачайте [образец приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="d242c-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="d242c-110">Он содержит два проекта: *WebApp* (веб-API ASP.NET Core) и *WebAppTests* (модульные тесты для веб-API).</span><span class="sxs-lookup"><span data-stu-id="d242c-110">It consists of two projects: *WebApp* (an ASP.NET Core web API) and *WebAppTests* (unit tests for the web API).</span></span>

<span data-ttu-id="d242c-111">В командной оболочке перейдите в папку *WebApp*.</span><span class="sxs-lookup"><span data-stu-id="d242c-111">In a command shell, navigate to the *WebApp* folder.</span></span> <span data-ttu-id="d242c-112">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="d242c-112">Run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="d242c-113">В выходных данных в консоли будут показаны аналогичные следующим сообщения, которые свидетельствуют о том, что приложение выполняется и ожидает запросов:</span><span class="sxs-lookup"><span data-stu-id="d242c-113">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="d242c-114">В браузере перейдите на адрес `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="d242c-114">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="d242c-115">Должен появиться результат `9`.</span><span class="sxs-lookup"><span data-stu-id="d242c-115">You should see the result of `9`.</span></span>

<span data-ttu-id="d242c-116">Перейдите к API продукта (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="d242c-116">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="d242c-117">Он возвращает `9`, а не `20`, как ожидалось.</span><span class="sxs-lookup"><span data-stu-id="d242c-117">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="d242c-118">Эта проблема устраняется далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="d242c-118">That problem is fixed later in the tutorial.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="d242c-119">Добавление `dotnet watch` в проект</span><span class="sxs-lookup"><span data-stu-id="d242c-119">Add `dotnet watch` to a project</span></span>

<span data-ttu-id="d242c-120">Средство наблюдения за файлами `dotnet watch` входит в версию 2.1.300 пакета SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="d242c-120">The `dotnet watch` file watcher tool is included with version 2.1.300 of the .NET Core SDK.</span></span> <span data-ttu-id="d242c-121">Выполните следующие действия при использовании более ранней версии пакета SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="d242c-121">The following steps are required when using an earlier version of the .NET Core SDK.</span></span>

1. <span data-ttu-id="d242c-122">Добавьте ссылку на пакет `Microsoft.DotNet.Watcher.Tools` в *CSPROJ*-файл:</span><span class="sxs-lookup"><span data-stu-id="d242c-122">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup>
    ```

1. <span data-ttu-id="d242c-123">Установите пакет `Microsoft.DotNet.Watcher.Tools`, запустив следующую команду:</span><span class="sxs-lookup"><span data-stu-id="d242c-123">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>

    ```console
    dotnet restore
    ```

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="d242c-124">Запуск команд .NET Core CLI с помощью `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="d242c-124">Run .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="d242c-125">Любую [команду .NET Core CLI](/dotnet/core/tools#cli-commands) можно запустить с `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="d242c-125">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="d242c-126">Пример:</span><span class="sxs-lookup"><span data-stu-id="d242c-126">For example:</span></span>

| <span data-ttu-id="d242c-127">Команда</span><span class="sxs-lookup"><span data-stu-id="d242c-127">Command</span></span> | <span data-ttu-id="d242c-128">Команда с контрольным значением</span><span class="sxs-lookup"><span data-stu-id="d242c-128">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="d242c-129">dotnet run</span><span class="sxs-lookup"><span data-stu-id="d242c-129">dotnet run</span></span> | <span data-ttu-id="d242c-130">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="d242c-130">dotnet watch run</span></span> |
| <span data-ttu-id="d242c-131">dotnet run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="d242c-131">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="d242c-132">dotnet watch run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="d242c-132">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="d242c-133">dotnet run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="d242c-133">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="d242c-134">dotnet watch run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="d242c-134">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="d242c-135">dotnet test</span><span class="sxs-lookup"><span data-stu-id="d242c-135">dotnet test</span></span> | <span data-ttu-id="d242c-136">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="d242c-136">dotnet watch test</span></span> |

<span data-ttu-id="d242c-137">Запустите `dotnet watch run` в папке *WebApp*.</span><span class="sxs-lookup"><span data-stu-id="d242c-137">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="d242c-138">В выходных данных консоли будет указано, что запущен `watch`.</span><span class="sxs-lookup"><span data-stu-id="d242c-138">The console output indicates `watch` has started.</span></span>

## <a name="make-changes-with-dotnet-watch"></a><span data-ttu-id="d242c-139">Изменения с помощью `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="d242c-139">Make changes with `dotnet watch`</span></span>

<span data-ttu-id="d242c-140">Убедитесь, что `dotnet watch` выполняется.</span><span class="sxs-lookup"><span data-stu-id="d242c-140">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="d242c-141">Исправьте ошибку в методе `Product` для *MathController.cs* так, чтобы он возвращал произведение, а не сумму чисел.</span><span class="sxs-lookup"><span data-stu-id="d242c-141">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
}
```

<span data-ttu-id="d242c-142">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="d242c-142">Save the file.</span></span> <span data-ttu-id="d242c-143">В выходных данных консоли будет показано, что `dotnet watch` обнаружил изменение файла и перезапустил приложение.</span><span class="sxs-lookup"><span data-stu-id="d242c-143">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="d242c-144">Убедитесь, что `http://localhost:<port number>/api/math/product?a=4&b=5` возвращает правильный результат.</span><span class="sxs-lookup"><span data-stu-id="d242c-144">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="run-tests-using-dotnet-watch"></a><span data-ttu-id="d242c-145">Тестирование с помощью `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="d242c-145">Run tests using `dotnet watch`</span></span>

1. <span data-ttu-id="d242c-146">Снова измените метод `Product` для *MathController.cs* так, чтобы он возвращал сумму чисел.</span><span class="sxs-lookup"><span data-stu-id="d242c-146">Change the `Product` method of *MathController.cs* back to returning the sum.</span></span> <span data-ttu-id="d242c-147">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="d242c-147">Save the file.</span></span>
1. <span data-ttu-id="d242c-148">В командной оболочке перейдите в папку *WebAppTests*.</span><span class="sxs-lookup"><span data-stu-id="d242c-148">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="d242c-149">Запустите [dotnet restore](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="d242c-149">Run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>
1. <span data-ttu-id="d242c-150">Запустите `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="d242c-150">Run `dotnet watch test`.</span></span> <span data-ttu-id="d242c-151">В выходных данных будет указано, что проверка не пройдена и наблюдатель ожидает изменений в файле:</span><span class="sxs-lookup"><span data-stu-id="d242c-151">Its output indicates that a test failed and that the watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="d242c-152">Исправьте код метода `Product` так, чтобы он возвращал произведение.</span><span class="sxs-lookup"><span data-stu-id="d242c-152">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="d242c-153">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="d242c-153">Save the file.</span></span>

<span data-ttu-id="d242c-154">`dotnet watch` обнаруживает изменения в файле и повторно запускает тесты.</span><span class="sxs-lookup"><span data-stu-id="d242c-154">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="d242c-155">В выходных данных консоли будет указано, что проверка пройдена.</span><span class="sxs-lookup"><span data-stu-id="d242c-155">The console output indicates the tests passed.</span></span>

## <a name="customize-files-list-to-watch"></a><span data-ttu-id="d242c-156">Настройка списка файлов для наблюдения</span><span class="sxs-lookup"><span data-stu-id="d242c-156">Customize files list to watch</span></span>

<span data-ttu-id="d242c-157">По умолчанию `dotnet-watch` отслеживает все файлы, соответствующие следующим стандартным маскам:</span><span class="sxs-lookup"><span data-stu-id="d242c-157">By default, `dotnet-watch` tracks all files matching the following glob patterns:</span></span>

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

<span data-ttu-id="d242c-158">Можно добавить в список наблюдения еще элементы, отредактировав файл *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="d242c-158">More items can be added to the watch list by editing the *.csproj* file.</span></span> <span data-ttu-id="d242c-159">Элементы можно указывать по отдельности или с помощью стандартной маски.</span><span class="sxs-lookup"><span data-stu-id="d242c-159">Items can be specified individually or by using glob patterns.</span></span>

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a><span data-ttu-id="d242c-160">Удаление файлов из списка наблюдения</span><span class="sxs-lookup"><span data-stu-id="d242c-160">Opt-out of files to be watched</span></span>

<span data-ttu-id="d242c-161">Можно настроить `dotnet-watch` так, чтобы игнорировать параметры по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d242c-161">`dotnet-watch` can be configured to ignore its default settings.</span></span> <span data-ttu-id="d242c-162">Чтобы пропускать определенные файлы, добавьте атрибут `Watch="false"` в определение элемента в файле *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="d242c-162">To ignore specific files, add the `Watch="false"` attribute to an item's definition in the *.csproj* file:</span></span>

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

## <a name="custom-watch-projects"></a><span data-ttu-id="d242c-163">Пользовательское наблюдение за проектами</span><span class="sxs-lookup"><span data-stu-id="d242c-163">Custom watch projects</span></span>

<span data-ttu-id="d242c-164">`dotnet-watch` используется не только с проектами C#.</span><span class="sxs-lookup"><span data-stu-id="d242c-164">`dotnet-watch` isn't restricted to C# projects.</span></span> <span data-ttu-id="d242c-165">Создайте пользовательское наблюдение за проектами для различных ситуаций.</span><span class="sxs-lookup"><span data-stu-id="d242c-165">Custom watch projects can be created to handle different scenarios.</span></span> <span data-ttu-id="d242c-166">Вы можете использовать следующий макет проекта:</span><span class="sxs-lookup"><span data-stu-id="d242c-166">Consider the following project layout:</span></span>

* <span data-ttu-id="d242c-167">**test/**</span><span class="sxs-lookup"><span data-stu-id="d242c-167">**test/**</span></span>
  * <span data-ttu-id="d242c-168">*UnitTests/UnitTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="d242c-168">*UnitTests/UnitTests.csproj*</span></span>
  * <span data-ttu-id="d242c-169">*IntegrationTests/IntegrationTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="d242c-169">*IntegrationTests/IntegrationTests.csproj*</span></span>

<span data-ttu-id="d242c-170">Если вы хотите наблюдать за обоими проектами, создайте пользовательский файл проекта, настроенный для наблюдения за обоими проектами:</span><span class="sxs-lookup"><span data-stu-id="d242c-170">If the goal is to watch both projects, create a custom project file configured to watch both projects:</span></span>

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

<span data-ttu-id="d242c-171">Чтобы запустить наблюдение за файлами для обоих проектов, внесите изменения в папку *test*.</span><span class="sxs-lookup"><span data-stu-id="d242c-171">To start file watching on both projects, change to the *test* folder.</span></span> <span data-ttu-id="d242c-172">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="d242c-172">Execute the following command:</span></span>

```console
dotnet watch msbuild /t:Test
```

<span data-ttu-id="d242c-173">VSTest выполняется при изменении любого файла в любом из тестовых проектов.</span><span class="sxs-lookup"><span data-stu-id="d242c-173">VSTest executes when any file changes in either test project.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="d242c-174">`dotnet-watch` на GitHub</span><span class="sxs-lookup"><span data-stu-id="d242c-174">`dotnet-watch` in GitHub</span></span>

<span data-ttu-id="d242c-175">`dotnet-watch` входит в [репозиторий DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch) на GitHub.</span><span class="sxs-lookup"><span data-stu-id="d242c-175">`dotnet-watch` is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span></span>
