---
title: Разработка приложений ASP.NET Core с использованием наблюдателя файлов
author: rick-anderson
description: В этом руководстве показано, как установить и использовать наблюдатель за файлами .NET Core CLI (dotnet watch) в приложении ASP.NET Core.
ms.author: riande
ms.date: 05/31/2018
uid: tutorials/dotnet-watch
ms.openlocfilehash: 03b4f7f4ade5268915482a659890c7edc2d9a852
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64889879"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a><span data-ttu-id="5e868-103">Разработка приложений ASP.NET Core с использованием наблюдателя файлов</span><span class="sxs-lookup"><span data-stu-id="5e868-103">Develop ASP.NET Core apps using a file watcher</span></span>

<span data-ttu-id="5e868-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Виктор Хурдугачи](https://twitter.com/victorhurdugaci) (Victor Hurdugaci)</span><span class="sxs-lookup"><span data-stu-id="5e868-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="5e868-105">`dotnet watch` — это средство, которое запускает команду [.NET Core CLI](/dotnet/core/tools) при изменении исходных файлов.</span><span class="sxs-lookup"><span data-stu-id="5e868-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="5e868-106">Например, в результате изменения файла может выполняться компиляция, тестирование или развертывание.</span><span class="sxs-lookup"><span data-stu-id="5e868-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="5e868-107">В этом руководстве используется уже существующее приложение веб-API с двумя конечными точками, одна из которых возвращает сумму, а другая — произведение.</span><span class="sxs-lookup"><span data-stu-id="5e868-107">This tutorial uses an existing web API with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="5e868-108">В методе произведения есть ошибка, которая исправлена в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="5e868-108">The product method has a bug, which is fixed in this tutorial.</span></span>

<span data-ttu-id="5e868-109">Скачайте [образец приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="5e868-109">Download the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="5e868-110">Он содержит два проекта: *WebApp* (веб-API ASP.NET Core) и *WebAppTests* (модульные тесты для веб-API).</span><span class="sxs-lookup"><span data-stu-id="5e868-110">It consists of two projects: *WebApp* (an ASP.NET Core web API) and *WebAppTests* (unit tests for the web API).</span></span>

<span data-ttu-id="5e868-111">В командной оболочке перейдите в папку *WebApp*.</span><span class="sxs-lookup"><span data-stu-id="5e868-111">In a command shell, navigate to the *WebApp* folder.</span></span> <span data-ttu-id="5e868-112">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="5e868-112">Run the following command:</span></span>

```console
dotnet run
```

> [!NOTE]
> <span data-ttu-id="5e868-113">Можно использовать `dotnet run --project <PROJECT>`, чтобы указать проект для запуска.</span><span class="sxs-lookup"><span data-stu-id="5e868-113">You can use `dotnet run --project <PROJECT>` to specify a project to run.</span></span> <span data-ttu-id="5e868-114">Например, при выполнении `dotnet run --project WebApp` из корневой папки примера приложения также будет запущен проект *WebApp*.</span><span class="sxs-lookup"><span data-stu-id="5e868-114">For example, running `dotnet run --project WebApp` from the root of the sample app will also run the *WebApp* project.</span></span>

<span data-ttu-id="5e868-115">В выходных данных в консоли будут показаны аналогичные следующим сообщения, которые свидетельствуют о том, что приложение выполняется и ожидает запросов:</span><span class="sxs-lookup"><span data-stu-id="5e868-115">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="5e868-116">В браузере перейдите на адрес `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="5e868-116">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="5e868-117">Должен появиться результат `9`.</span><span class="sxs-lookup"><span data-stu-id="5e868-117">You should see the result of `9`.</span></span>

<span data-ttu-id="5e868-118">Перейдите к API продукта (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="5e868-118">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="5e868-119">Он возвращает `9`, а не `20`, как ожидалось.</span><span class="sxs-lookup"><span data-stu-id="5e868-119">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="5e868-120">Эта проблема устраняется далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="5e868-120">That problem is fixed later in the tutorial.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="5e868-121">Добавление `dotnet watch` в проект</span><span class="sxs-lookup"><span data-stu-id="5e868-121">Add `dotnet watch` to a project</span></span>

<span data-ttu-id="5e868-122">Средство наблюдения за файлами `dotnet watch` входит в версию 2.1.300 пакета SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="5e868-122">The `dotnet watch` file watcher tool is included with version 2.1.300 of the .NET Core SDK.</span></span> <span data-ttu-id="5e868-123">Выполните следующие действия при использовании более ранней версии пакета SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="5e868-123">The following steps are required when using an earlier version of the .NET Core SDK.</span></span>

1. <span data-ttu-id="5e868-124">Добавьте ссылку на пакет `Microsoft.DotNet.Watcher.Tools` в *CSPROJ*-файл:</span><span class="sxs-lookup"><span data-stu-id="5e868-124">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup>
    ```

1. <span data-ttu-id="5e868-125">Установите пакет `Microsoft.DotNet.Watcher.Tools`, запустив следующую команду:</span><span class="sxs-lookup"><span data-stu-id="5e868-125">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>

    ```console
    dotnet restore
    ```

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="5e868-126">Запуск команд .NET Core CLI с помощью `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="5e868-126">Run .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="5e868-127">Любую [команду .NET Core CLI](/dotnet/core/tools#cli-commands) можно запустить с `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="5e868-127">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="5e868-128">Например:</span><span class="sxs-lookup"><span data-stu-id="5e868-128">For example:</span></span>

| <span data-ttu-id="5e868-129">Команда</span><span class="sxs-lookup"><span data-stu-id="5e868-129">Command</span></span> | <span data-ttu-id="5e868-130">Команда с контрольным значением</span><span class="sxs-lookup"><span data-stu-id="5e868-130">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="5e868-131">dotnet run</span><span class="sxs-lookup"><span data-stu-id="5e868-131">dotnet run</span></span> | <span data-ttu-id="5e868-132">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="5e868-132">dotnet watch run</span></span> |
| <span data-ttu-id="5e868-133">dotnet run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="5e868-133">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="5e868-134">dotnet watch run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="5e868-134">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="5e868-135">dotnet run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="5e868-135">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="5e868-136">dotnet watch run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="5e868-136">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="5e868-137">dotnet test</span><span class="sxs-lookup"><span data-stu-id="5e868-137">dotnet test</span></span> | <span data-ttu-id="5e868-138">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="5e868-138">dotnet watch test</span></span> |

<span data-ttu-id="5e868-139">Запустите `dotnet watch run` в папке *WebApp*.</span><span class="sxs-lookup"><span data-stu-id="5e868-139">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="5e868-140">В выходных данных консоли будет указано, что запущен `watch`.</span><span class="sxs-lookup"><span data-stu-id="5e868-140">The console output indicates `watch` has started.</span></span>

> [!NOTE]
> <span data-ttu-id="5e868-141">Можно использовать `dotnet watch --project <PROJECT>`, чтобы указать проект для наблюдения.</span><span class="sxs-lookup"><span data-stu-id="5e868-141">You can use `dotnet watch --project <PROJECT>` to specify a project to watch.</span></span> <span data-ttu-id="5e868-142">Например, при выполнении `dotnet watch --project WebApp run` из корневой папки примера приложения также будет запущен проект *WebApp*, в том числе для наблюдения.</span><span class="sxs-lookup"><span data-stu-id="5e868-142">For example, running `dotnet watch --project WebApp run` from the root of the sample app will also run and watch the *WebApp* project.</span></span>

## <a name="make-changes-with-dotnet-watch"></a><span data-ttu-id="5e868-143">Изменения с помощью `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="5e868-143">Make changes with `dotnet watch`</span></span>

<span data-ttu-id="5e868-144">Убедитесь, что `dotnet watch` выполняется.</span><span class="sxs-lookup"><span data-stu-id="5e868-144">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="5e868-145">Исправьте ошибку в методе `Product` для *MathController.cs* так, чтобы он возвращал произведение, а не сумму чисел.</span><span class="sxs-lookup"><span data-stu-id="5e868-145">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
    return a * b;
}
```

<span data-ttu-id="5e868-146">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="5e868-146">Save the file.</span></span> <span data-ttu-id="5e868-147">В выходных данных консоли будет показано, что `dotnet watch` обнаружил изменение файла и перезапустил приложение.</span><span class="sxs-lookup"><span data-stu-id="5e868-147">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="5e868-148">Убедитесь, что `http://localhost:<port number>/api/math/product?a=4&b=5` возвращает правильный результат.</span><span class="sxs-lookup"><span data-stu-id="5e868-148">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="run-tests-using-dotnet-watch"></a><span data-ttu-id="5e868-149">Тестирование с помощью `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="5e868-149">Run tests using `dotnet watch`</span></span>

1. <span data-ttu-id="5e868-150">Снова измените метод `Product` для *MathController.cs* так, чтобы он возвращал сумму чисел.</span><span class="sxs-lookup"><span data-stu-id="5e868-150">Change the `Product` method of *MathController.cs* back to returning the sum.</span></span> <span data-ttu-id="5e868-151">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="5e868-151">Save the file.</span></span>
1. <span data-ttu-id="5e868-152">В командной оболочке перейдите в папку *WebAppTests*.</span><span class="sxs-lookup"><span data-stu-id="5e868-152">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="5e868-153">Запустите [dotnet restore](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="5e868-153">Run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>
1. <span data-ttu-id="5e868-154">Запустите `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="5e868-154">Run `dotnet watch test`.</span></span> <span data-ttu-id="5e868-155">В выходных данных будет указано, что проверка не пройдена и наблюдатель ожидает изменений в файле:</span><span class="sxs-lookup"><span data-stu-id="5e868-155">Its output indicates that a test failed and that the watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="5e868-156">Исправьте код метода `Product` так, чтобы он возвращал произведение.</span><span class="sxs-lookup"><span data-stu-id="5e868-156">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="5e868-157">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="5e868-157">Save the file.</span></span>

<span data-ttu-id="5e868-158">`dotnet watch` обнаруживает изменения в файле и повторно запускает тесты.</span><span class="sxs-lookup"><span data-stu-id="5e868-158">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="5e868-159">В выходных данных консоли будет указано, что проверка пройдена.</span><span class="sxs-lookup"><span data-stu-id="5e868-159">The console output indicates the tests passed.</span></span>

## <a name="customize-files-list-to-watch"></a><span data-ttu-id="5e868-160">Настройка списка файлов для наблюдения</span><span class="sxs-lookup"><span data-stu-id="5e868-160">Customize files list to watch</span></span>

<span data-ttu-id="5e868-161">По умолчанию `dotnet-watch` отслеживает все файлы, соответствующие следующим стандартным маскам:</span><span class="sxs-lookup"><span data-stu-id="5e868-161">By default, `dotnet-watch` tracks all files matching the following glob patterns:</span></span>

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

<span data-ttu-id="5e868-162">Можно добавить в список наблюдения еще элементы, отредактировав файл *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="5e868-162">More items can be added to the watch list by editing the *.csproj* file.</span></span> <span data-ttu-id="5e868-163">Элементы можно указывать по отдельности или с помощью стандартной маски.</span><span class="sxs-lookup"><span data-stu-id="5e868-163">Items can be specified individually or by using glob patterns.</span></span>

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a><span data-ttu-id="5e868-164">Удаление файлов из списка наблюдения</span><span class="sxs-lookup"><span data-stu-id="5e868-164">Opt-out of files to be watched</span></span>

<span data-ttu-id="5e868-165">Можно настроить `dotnet-watch` так, чтобы игнорировать параметры по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5e868-165">`dotnet-watch` can be configured to ignore its default settings.</span></span> <span data-ttu-id="5e868-166">Чтобы пропускать определенные файлы, добавьте атрибут `Watch="false"` в определение элемента в файле *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="5e868-166">To ignore specific files, add the `Watch="false"` attribute to an item's definition in the *.csproj* file:</span></span>

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

## <a name="custom-watch-projects"></a><span data-ttu-id="5e868-167">Пользовательское наблюдение за проектами</span><span class="sxs-lookup"><span data-stu-id="5e868-167">Custom watch projects</span></span>

<span data-ttu-id="5e868-168">`dotnet-watch` используется не только с проектами C#.</span><span class="sxs-lookup"><span data-stu-id="5e868-168">`dotnet-watch` isn't restricted to C# projects.</span></span> <span data-ttu-id="5e868-169">Создайте пользовательское наблюдение за проектами для различных ситуаций.</span><span class="sxs-lookup"><span data-stu-id="5e868-169">Custom watch projects can be created to handle different scenarios.</span></span> <span data-ttu-id="5e868-170">Вы можете использовать следующий макет проекта:</span><span class="sxs-lookup"><span data-stu-id="5e868-170">Consider the following project layout:</span></span>

* <span data-ttu-id="5e868-171">**test/**</span><span class="sxs-lookup"><span data-stu-id="5e868-171">**test/**</span></span>
  * <span data-ttu-id="5e868-172">*UnitTests/UnitTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="5e868-172">*UnitTests/UnitTests.csproj*</span></span>
  * <span data-ttu-id="5e868-173">*IntegrationTests/IntegrationTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="5e868-173">*IntegrationTests/IntegrationTests.csproj*</span></span>

<span data-ttu-id="5e868-174">Если вы хотите наблюдать за обоими проектами, создайте пользовательский файл проекта, настроенный для наблюдения за обоими проектами:</span><span class="sxs-lookup"><span data-stu-id="5e868-174">If the goal is to watch both projects, create a custom project file configured to watch both projects:</span></span>

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

<span data-ttu-id="5e868-175">Чтобы запустить наблюдение за файлами для обоих проектов, внесите изменения в папку *test*.</span><span class="sxs-lookup"><span data-stu-id="5e868-175">To start file watching on both projects, change to the *test* folder.</span></span> <span data-ttu-id="5e868-176">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="5e868-176">Execute the following command:</span></span>

```console
dotnet watch msbuild /t:Test
```

<span data-ttu-id="5e868-177">VSTest выполняется при изменении любого файла в любом из тестовых проектов.</span><span class="sxs-lookup"><span data-stu-id="5e868-177">VSTest executes when any file changes in either test project.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="5e868-178">`dotnet-watch` на GitHub</span><span class="sxs-lookup"><span data-stu-id="5e868-178">`dotnet-watch` in GitHub</span></span>

<span data-ttu-id="5e868-179">`dotnet-watch` предоставляется в [репозитории aspnet/AspNetCore](https://github.com/aspnet/AspNetCore/tree/master/src/Tools/dotnet-watch) на GitHub.</span><span class="sxs-lookup"><span data-stu-id="5e868-179">`dotnet-watch` is part of the GitHub [aspnet/AspNetCore repository](https://github.com/aspnet/AspNetCore/tree/master/src/Tools/dotnet-watch).</span></span>
