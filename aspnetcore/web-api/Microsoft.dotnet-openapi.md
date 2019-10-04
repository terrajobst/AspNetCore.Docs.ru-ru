---
title: Разработка приложений ASP.NET Core с использованием OpenAPI
author: ryanbrandenburg
description: Здесь демонстрируется, как добавить ссылки в файлы OpenAPI с использованием средства Microsoft.dotnet-openapi.
ms.author: rybrande
ms.date: 09/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: f5eae9e871bc8efc30d500769adb845ff244a90c
ms.sourcegitcommit: e644258c95dd50a82284f107b9bf3becbc43b2b2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317778"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a><span data-ttu-id="2d3a7-103">Разработка приложений ASP.NET Core с использованием средств OpenAPI</span><span class="sxs-lookup"><span data-stu-id="2d3a7-103">Develop ASP.NET Core apps using OpenAPI tools</span></span>

<span data-ttu-id="2d3a7-104">Автор: Райан Бранденбург (Ryan Brandenburg)</span><span class="sxs-lookup"><span data-stu-id="2d3a7-104">By Ryan Brandenburg</span></span>

<span data-ttu-id="2d3a7-105">[Microsoft.dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) — это [глобальное средство .NET Core](/dotnet/core/tools/global-tools) для управления ссылками [OpenAPI](https://github.com/OAI/OpenAPI-Specification) в рамках проекта.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-105">[Microsoft.dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) is a [.NET Core Global Tool](/dotnet/core/tools/global-tools) for managing [OpenAPI](https://github.com/OAI/OpenAPI-Specification) references within a project.</span></span>

## <a name="installation"></a><span data-ttu-id="2d3a7-106">Установка</span><span class="sxs-lookup"><span data-stu-id="2d3a7-106">Installation</span></span>

<span data-ttu-id="2d3a7-107">Чтобы установить `Microsoft.dotnet-openapi`, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="2d3a7-107">To install `Microsoft.dotnet-openapi`, run the following command:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.dotnet-openapi
```

## <a name="add"></a><span data-ttu-id="2d3a7-108">Add</span><span class="sxs-lookup"><span data-stu-id="2d3a7-108">Add</span></span>

<span data-ttu-id="2d3a7-109">При добавлении ссылки OpenAPI с помощью любой из команд на этой странице в файл *CSPROJ* добавляется элемент `<OpenApiReference />`, аналогичный приведенному ниже:</span><span class="sxs-lookup"><span data-stu-id="2d3a7-109">Adding an OpenAPI reference using any of the commands on this page adds an `<OpenApiReference />` element similar to the following to the *.csproj* file:</span></span>

```xml
<OpenApiReference Include="openapi.json" />
```

<span data-ttu-id="2d3a7-110">Для вызова созданного клиентского кода приложению требуется предыдущая ссылка.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-110">The preceding reference is required for the app to call the generated client code.</span></span>

<!-- TODO: Restore after https://github.com/aspnet/AspNetCore/issues/12738
### Add Project

#### Options

| Short option | Long option | Description | Example |
|-------|------|-------|---------|
| -v|--verbose | Show verbose output. |dotnet openapi add project *-v* ../Ref/ProjRef.csproj |
| -p|--project | The project to operate on. |dotnet openapi add project *--project .\Ref.csproj* ../Ref/ProjRef.csproj |

#### Arguments

|  Argument  | Description | Example |
|-------------|-------------|---------|
| source-file | The source to create a reference from. Must be a project file. |dotnet openapi add project *../Ref/ProjRef.csproj* | -->

### <a name="add-file"></a><span data-ttu-id="2d3a7-111">Добавление файла</span><span class="sxs-lookup"><span data-stu-id="2d3a7-111">Add File</span></span>

#### <a name="options"></a><span data-ttu-id="2d3a7-112">Параметры</span><span class="sxs-lookup"><span data-stu-id="2d3a7-112">Options</span></span>

| <span data-ttu-id="2d3a7-113">Короткий параметр</span><span class="sxs-lookup"><span data-stu-id="2d3a7-113">Short option</span></span>| <span data-ttu-id="2d3a7-114">Длинный параметр</span><span class="sxs-lookup"><span data-stu-id="2d3a7-114">Long option</span></span>| <span data-ttu-id="2d3a7-115">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="2d3a7-115">Description</span></span> | <span data-ttu-id="2d3a7-116">Пример</span><span class="sxs-lookup"><span data-stu-id="2d3a7-116">Example</span></span> |
|-------|------|-------|---------|
| <span data-ttu-id="2d3a7-117">-v</span><span class="sxs-lookup"><span data-stu-id="2d3a7-117">-v</span></span>|<span data-ttu-id="2d3a7-118">--verbose</span><span class="sxs-lookup"><span data-stu-id="2d3a7-118">--verbose</span></span> | <span data-ttu-id="2d3a7-119">Отображение подробных выходных данных.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-119">Show verbose output.</span></span> |<span data-ttu-id="2d3a7-120">dotnet openapi add file *-v* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="2d3a7-120">dotnet openapi add file *-v* .\OpenAPI.json</span></span> |
| <span data-ttu-id="2d3a7-121">-p</span><span class="sxs-lookup"><span data-stu-id="2d3a7-121">-p</span></span>|<span data-ttu-id="2d3a7-122">--updateProject</span><span class="sxs-lookup"><span data-stu-id="2d3a7-122">--updateProject</span></span> | <span data-ttu-id="2d3a7-123">Проект для выполнения операции.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-123">The project to operate on.</span></span> |<span data-ttu-id="2d3a7-124">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="2d3a7-124">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="2d3a7-125">-c</span><span class="sxs-lookup"><span data-stu-id="2d3a7-125">-c</span></span>|<span data-ttu-id="2d3a7-126">--code-generator</span><span class="sxs-lookup"><span data-stu-id="2d3a7-126">--code-generator</span></span>| <span data-ttu-id="2d3a7-127">Генератор кода, применяемый к ссылке.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-127">The code generator to apply to the reference.</span></span> <span data-ttu-id="2d3a7-128">Возможные значения: `NSwagCSharp` и `NSwagTypeScript`.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-128">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> <span data-ttu-id="2d3a7-129">Если атрибут `--code-generator` не задан, по умолчанию для средств будет выбрано `NSwagCSharp`.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-129">If `--code-generator` is not specified the tooling defaults to `NSwagCSharp`.</span></span>|<span data-ttu-id="2d3a7-130">dotnet openapi add file .\OpenApi.json --code-generator</span><span class="sxs-lookup"><span data-stu-id="2d3a7-130">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="2d3a7-131">-h</span><span class="sxs-lookup"><span data-stu-id="2d3a7-131">-h</span></span>|<span data-ttu-id="2d3a7-132">--help</span><span class="sxs-lookup"><span data-stu-id="2d3a7-132">--help</span></span>|<span data-ttu-id="2d3a7-133">Отображение справочных сведений.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-133">Show help information</span></span>|<span data-ttu-id="2d3a7-134">dotnet openapi add file --help</span><span class="sxs-lookup"><span data-stu-id="2d3a7-134">dotnet openapi add file --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="2d3a7-135">Аргументы</span><span class="sxs-lookup"><span data-stu-id="2d3a7-135">Arguments</span></span>

|  <span data-ttu-id="2d3a7-136">Аргумент</span><span class="sxs-lookup"><span data-stu-id="2d3a7-136">Argument</span></span>  | <span data-ttu-id="2d3a7-137">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="2d3a7-137">Description</span></span> | <span data-ttu-id="2d3a7-138">Пример</span><span class="sxs-lookup"><span data-stu-id="2d3a7-138">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="2d3a7-139">source-file</span><span class="sxs-lookup"><span data-stu-id="2d3a7-139">source-file</span></span> | <span data-ttu-id="2d3a7-140">Источник, из которого создается ссылка.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-140">The source to create a reference from.</span></span> <span data-ttu-id="2d3a7-141">Должен быть файлом OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-141">Must be an OpenAPI file.</span></span> |<span data-ttu-id="2d3a7-142">dotnet openapi add file *.\OpenAPI.json*</span><span class="sxs-lookup"><span data-stu-id="2d3a7-142">dotnet openapi add file *.\OpenAPI.json*</span></span> |

### <a name="add-url"></a><span data-ttu-id="2d3a7-143">Добавление URL-адреса</span><span class="sxs-lookup"><span data-stu-id="2d3a7-143">Add URL</span></span>

#### <a name="options"></a><span data-ttu-id="2d3a7-144">Параметры</span><span class="sxs-lookup"><span data-stu-id="2d3a7-144">Options</span></span>

| <span data-ttu-id="2d3a7-145">Короткий параметр</span><span class="sxs-lookup"><span data-stu-id="2d3a7-145">Short option</span></span>| <span data-ttu-id="2d3a7-146">Длинный параметр</span><span class="sxs-lookup"><span data-stu-id="2d3a7-146">Long option</span></span>| <span data-ttu-id="2d3a7-147">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="2d3a7-147">Description</span></span> | <span data-ttu-id="2d3a7-148">Пример</span><span class="sxs-lookup"><span data-stu-id="2d3a7-148">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="2d3a7-149">-v</span><span class="sxs-lookup"><span data-stu-id="2d3a7-149">-v</span></span>|<span data-ttu-id="2d3a7-150">--verbose</span><span class="sxs-lookup"><span data-stu-id="2d3a7-150">--verbose</span></span> | <span data-ttu-id="2d3a7-151">Отображение подробных выходных данных.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-151">Show verbose output.</span></span> |<span data-ttu-id="2d3a7-152">dotnet openapi add url *-v* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="2d3a7-152">dotnet openapi add url *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="2d3a7-153">-p</span><span class="sxs-lookup"><span data-stu-id="2d3a7-153">-p</span></span>|<span data-ttu-id="2d3a7-154">--updateProject</span><span class="sxs-lookup"><span data-stu-id="2d3a7-154">--updateProject</span></span> | <span data-ttu-id="2d3a7-155">Проект для выполнения операции.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-155">The project to operate on.</span></span> |<span data-ttu-id="2d3a7-156">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="2d3a7-156">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="2d3a7-157">-o</span><span class="sxs-lookup"><span data-stu-id="2d3a7-157">-o</span></span>|<span data-ttu-id="2d3a7-158">--output-file</span><span class="sxs-lookup"><span data-stu-id="2d3a7-158">--output-file</span></span> | <span data-ttu-id="2d3a7-159">Место размещения локальной копии файла OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-159">Where to place the local copy of the OpenAPI file.</span></span> |<span data-ttu-id="2d3a7-160">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span><span class="sxs-lookup"><span data-stu-id="2d3a7-160">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span></span> |
| <span data-ttu-id="2d3a7-161">-c</span><span class="sxs-lookup"><span data-stu-id="2d3a7-161">-c</span></span>|<span data-ttu-id="2d3a7-162">--code-generator</span><span class="sxs-lookup"><span data-stu-id="2d3a7-162">--code-generator</span></span>| <span data-ttu-id="2d3a7-163">Генератор кода, применяемый к ссылке.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-163">The code generator to apply to the reference.</span></span> <span data-ttu-id="2d3a7-164">Возможные значения: `NSwagCSharp` и `NSwagTypeScript`.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-164">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> |<span data-ttu-id="2d3a7-165">dotnet openapi add file .\OpenApi.json --code-generator</span><span class="sxs-lookup"><span data-stu-id="2d3a7-165">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="2d3a7-166">-h</span><span class="sxs-lookup"><span data-stu-id="2d3a7-166">-h</span></span>|<span data-ttu-id="2d3a7-167">--help</span><span class="sxs-lookup"><span data-stu-id="2d3a7-167">--help</span></span>|<span data-ttu-id="2d3a7-168">Отображение справочных сведений.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-168">Show help information</span></span>|<span data-ttu-id="2d3a7-169">dotnet openapi add url --help</span><span class="sxs-lookup"><span data-stu-id="2d3a7-169">dotnet openapi add url --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="2d3a7-170">Аргументы</span><span class="sxs-lookup"><span data-stu-id="2d3a7-170">Arguments</span></span>

|  <span data-ttu-id="2d3a7-171">Аргумент</span><span class="sxs-lookup"><span data-stu-id="2d3a7-171">Argument</span></span>  | <span data-ttu-id="2d3a7-172">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="2d3a7-172">Description</span></span> | <span data-ttu-id="2d3a7-173">Пример</span><span class="sxs-lookup"><span data-stu-id="2d3a7-173">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="2d3a7-174">source-URL</span><span class="sxs-lookup"><span data-stu-id="2d3a7-174">source-URL</span></span> | <span data-ttu-id="2d3a7-175">Источник, из которого создается ссылка.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-175">The source to create a reference from.</span></span> <span data-ttu-id="2d3a7-176">Должен быть URL-адресом.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-176">Must be a URL.</span></span> |<span data-ttu-id="2d3a7-177">dotnet openapi add url `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="2d3a7-177">dotnet openapi add url `https://contoso.com/openapi.json`</span></span> |

## <a name="remove"></a><span data-ttu-id="2d3a7-178">Удалить</span><span class="sxs-lookup"><span data-stu-id="2d3a7-178">Remove</span></span>

<span data-ttu-id="2d3a7-179">Удаляет ссылку на OpenAPI, соответствующую заданному имени файла, из файла *CSPROJ*.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-179">Removes the OpenAPI reference matching the given filename from the *.csproj* file.</span></span> <span data-ttu-id="2d3a7-180">При удалении ссылки OpenAPI клиенты не будут создаваться.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-180">When the OpenAPI reference is removed, clients won't be generated.</span></span> <span data-ttu-id="2d3a7-181">Локальные файлы *JSON* и *YAML* удаляются.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-181">Local *.json* and *.yaml* files are deleted.</span></span>

### <a name="options"></a><span data-ttu-id="2d3a7-182">Параметры</span><span class="sxs-lookup"><span data-stu-id="2d3a7-182">Options</span></span>

| <span data-ttu-id="2d3a7-183">Короткий параметр</span><span class="sxs-lookup"><span data-stu-id="2d3a7-183">Short option</span></span>| <span data-ttu-id="2d3a7-184">Длинный параметр</span><span class="sxs-lookup"><span data-stu-id="2d3a7-184">Long option</span></span>| <span data-ttu-id="2d3a7-185">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="2d3a7-185">Description</span></span>| <span data-ttu-id="2d3a7-186">Пример</span><span class="sxs-lookup"><span data-stu-id="2d3a7-186">Example</span></span> |
|-------|------|------------|---------|
| <span data-ttu-id="2d3a7-187">-v</span><span class="sxs-lookup"><span data-stu-id="2d3a7-187">-v</span></span>|<span data-ttu-id="2d3a7-188">--verbose</span><span class="sxs-lookup"><span data-stu-id="2d3a7-188">--verbose</span></span> | <span data-ttu-id="2d3a7-189">Отображение подробных выходных данных.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-189">Show verbose output.</span></span> |<span data-ttu-id="2d3a7-190">dotnet openapi remove *-v*</span><span class="sxs-lookup"><span data-stu-id="2d3a7-190">dotnet openapi remove *-v*</span></span>|
| <span data-ttu-id="2d3a7-191">-p</span><span class="sxs-lookup"><span data-stu-id="2d3a7-191">-p</span></span>|<span data-ttu-id="2d3a7-192">--updateProject</span><span class="sxs-lookup"><span data-stu-id="2d3a7-192">--updateProject</span></span> | <span data-ttu-id="2d3a7-193">Проект для выполнения операции.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-193">The project to operate on.</span></span> |<span data-ttu-id="2d3a7-194">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="2d3a7-194">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="2d3a7-195">-h</span><span class="sxs-lookup"><span data-stu-id="2d3a7-195">-h</span></span>|<span data-ttu-id="2d3a7-196">--help</span><span class="sxs-lookup"><span data-stu-id="2d3a7-196">--help</span></span>|<span data-ttu-id="2d3a7-197">Отображение справочных сведений.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-197">Show help information</span></span>|<span data-ttu-id="2d3a7-198">dotnet openapi remove --help</span><span class="sxs-lookup"><span data-stu-id="2d3a7-198">dotnet openapi remove --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="2d3a7-199">Аргументы</span><span class="sxs-lookup"><span data-stu-id="2d3a7-199">Arguments</span></span>

|  <span data-ttu-id="2d3a7-200">Аргумент</span><span class="sxs-lookup"><span data-stu-id="2d3a7-200">Argument</span></span>  | <span data-ttu-id="2d3a7-201">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="2d3a7-201">Description</span></span>| <span data-ttu-id="2d3a7-202">Пример</span><span class="sxs-lookup"><span data-stu-id="2d3a7-202">Example</span></span> |
| ------------|------------|---------|
| <span data-ttu-id="2d3a7-203">source-file</span><span class="sxs-lookup"><span data-stu-id="2d3a7-203">source-file</span></span> | <span data-ttu-id="2d3a7-204">Источник, ссылку на который необходимо удалить.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-204">The source to remove the reference to.</span></span> |<span data-ttu-id="2d3a7-205">dotnet openapi remove *.\OpenAPI.json*</span><span class="sxs-lookup"><span data-stu-id="2d3a7-205">dotnet openapi remove *.\OpenAPI.json*</span></span> |

## <a name="refresh"></a><span data-ttu-id="2d3a7-206">Обновление</span><span class="sxs-lookup"><span data-stu-id="2d3a7-206">Refresh</span></span>

<span data-ttu-id="2d3a7-207">Обновляет локальную версию файла, скачанного с использованием последнего содержимого из URL-адреса для скачивания.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-207">Refreshes the local version of a file that was downloaded using the latest content from the download URL.</span></span>

### <a name="options"></a><span data-ttu-id="2d3a7-208">Параметры</span><span class="sxs-lookup"><span data-stu-id="2d3a7-208">Options</span></span>

| <span data-ttu-id="2d3a7-209">Короткий параметр</span><span class="sxs-lookup"><span data-stu-id="2d3a7-209">Short option</span></span>| <span data-ttu-id="2d3a7-210">Длинный параметр</span><span class="sxs-lookup"><span data-stu-id="2d3a7-210">Long option</span></span>| <span data-ttu-id="2d3a7-211">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="2d3a7-211">Description</span></span> | <span data-ttu-id="2d3a7-212">Пример</span><span class="sxs-lookup"><span data-stu-id="2d3a7-212">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="2d3a7-213">-v</span><span class="sxs-lookup"><span data-stu-id="2d3a7-213">-v</span></span>|<span data-ttu-id="2d3a7-214">--verbose</span><span class="sxs-lookup"><span data-stu-id="2d3a7-214">--verbose</span></span> | <span data-ttu-id="2d3a7-215">Отображение подробных выходных данных.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-215">Show verbose output.</span></span> | <span data-ttu-id="2d3a7-216">dotnet openapi refresh *-v* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="2d3a7-216">dotnet openapi refresh *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="2d3a7-217">-p</span><span class="sxs-lookup"><span data-stu-id="2d3a7-217">-p</span></span>|<span data-ttu-id="2d3a7-218">--updateProject</span><span class="sxs-lookup"><span data-stu-id="2d3a7-218">--updateProject</span></span> | <span data-ttu-id="2d3a7-219">Проект для выполнения операции.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-219">The project to operate on.</span></span> | <span data-ttu-id="2d3a7-220">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="2d3a7-220">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="2d3a7-221">-h</span><span class="sxs-lookup"><span data-stu-id="2d3a7-221">-h</span></span>|<span data-ttu-id="2d3a7-222">--help</span><span class="sxs-lookup"><span data-stu-id="2d3a7-222">--help</span></span>|<span data-ttu-id="2d3a7-223">Отображение справочных сведений.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-223">Show help information</span></span>|<span data-ttu-id="2d3a7-224">dotnet openapi refresh --help</span><span class="sxs-lookup"><span data-stu-id="2d3a7-224">dotnet openapi refresh --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="2d3a7-225">Аргументы</span><span class="sxs-lookup"><span data-stu-id="2d3a7-225">Arguments</span></span>

|  <span data-ttu-id="2d3a7-226">Аргумент</span><span class="sxs-lookup"><span data-stu-id="2d3a7-226">Argument</span></span>  | <span data-ttu-id="2d3a7-227">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="2d3a7-227">Description</span></span> | <span data-ttu-id="2d3a7-228">Пример</span><span class="sxs-lookup"><span data-stu-id="2d3a7-228">Example</span></span> |
| ------------|-------------|---------|
| <span data-ttu-id="2d3a7-229">source-URL</span><span class="sxs-lookup"><span data-stu-id="2d3a7-229">source-URL</span></span> | <span data-ttu-id="2d3a7-230">URL-адрес, ссылку из которого необходимо обновить.</span><span class="sxs-lookup"><span data-stu-id="2d3a7-230">The URL to refresh the reference from.</span></span> | <span data-ttu-id="2d3a7-231">dotnet openapi refresh `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="2d3a7-231">dotnet openapi refresh `https://contoso.com/openapi.json`</span></span> |
