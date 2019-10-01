---
title: Разработка приложений ASP.NET Core с использованием OpenAPI
author: ryanbrandenburg
description: Здесь демонстрируется, как добавить ссылки в файлы OpenAPI с использованием средства Microsoft.dotnet-openapi.
ms.author: rybrande
ms.date: 08/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: a9b38bb7e69744d72867bf69cecf1fa92d7c15b3
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187368"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a><span data-ttu-id="d7fdc-103">Разработка приложений ASP.NET Core с использованием средств OpenAPI</span><span class="sxs-lookup"><span data-stu-id="d7fdc-103">Develop ASP.NET Core apps using OpenAPI tools</span></span>

<span data-ttu-id="d7fdc-104">Автор: Райан Бранденбург (Ryan Brandenburg)</span><span class="sxs-lookup"><span data-stu-id="d7fdc-104">By Ryan Brandenburg</span></span>

<span data-ttu-id="d7fdc-105">`Microsoft.dotnet-openapi` — это глобальное средство .NET Core для управления ссылками [OpenAPI](https://github.com/OAI/OpenAPI-Specification) в рамках проекта.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-105">`Microsoft.dotnet-openapi` is a .NET Core global tool for managing [OpenAPI](https://github.com/OAI/OpenAPI-Specification) references within a project.</span></span>

## <a name="installation"></a><span data-ttu-id="d7fdc-106">Установка</span><span class="sxs-lookup"><span data-stu-id="d7fdc-106">Installation</span></span>

<span data-ttu-id="d7fdc-107">Чтобы установить `Microsoft.dotnet-openapi`, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="d7fdc-107">To install `Microsoft.dotnet-openapi`, run the following command:</span></span>

```console
dotnet tool install -g Microsoft.dotnet-openapi
```

<span data-ttu-id="d7fdc-108">`Microsoft.dotnet-openapi` — это [глобальное средство .NET Core](/dotnet/core/tools/global-tools).</span><span class="sxs-lookup"><span data-stu-id="d7fdc-108">`Microsoft.dotnet-openapi` is a [.NET Core Global Tool](/dotnet/core/tools/global-tools).</span></span>

## <a name="add"></a><span data-ttu-id="d7fdc-109">Add</span><span class="sxs-lookup"><span data-stu-id="d7fdc-109">Add</span></span>

<span data-ttu-id="d7fdc-110">При добавлении ссылки OpenAPI с помощью любой из команд на этой странице в файл *CSPROJ* добавляется элемент `<OpenApiReference />`, аналогичный приведенному ниже:</span><span class="sxs-lookup"><span data-stu-id="d7fdc-110">Adding an OpenAPI reference using any of the commands on this page adds an `<OpenApiReference />`  element similar to the following to the *.csproj* file:</span></span>

```xml
<OpenApiReference Include="openapi.json" />
```

<span data-ttu-id="d7fdc-111">Для вызова созданного клиентского кода приложению требуется предыдущая ссылка.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-111">The preceding reference is required for the app to call the generated client code.</span></span>

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

### <a name="add-file"></a><span data-ttu-id="d7fdc-112">Добавление файла</span><span class="sxs-lookup"><span data-stu-id="d7fdc-112">Add File</span></span>

#### <a name="options"></a><span data-ttu-id="d7fdc-113">Параметры</span><span class="sxs-lookup"><span data-stu-id="d7fdc-113">Options</span></span>

| <span data-ttu-id="d7fdc-114">Короткий параметр</span><span class="sxs-lookup"><span data-stu-id="d7fdc-114">Short option</span></span>| <span data-ttu-id="d7fdc-115">Длинный параметр</span><span class="sxs-lookup"><span data-stu-id="d7fdc-115">Long option</span></span>| <span data-ttu-id="d7fdc-116">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="d7fdc-116">Description</span></span> | <span data-ttu-id="d7fdc-117">Пример</span><span class="sxs-lookup"><span data-stu-id="d7fdc-117">Example</span></span> |
|-------|------|-------|---------|
| <span data-ttu-id="d7fdc-118">-v</span><span class="sxs-lookup"><span data-stu-id="d7fdc-118">-v</span></span>|<span data-ttu-id="d7fdc-119">--verbose</span><span class="sxs-lookup"><span data-stu-id="d7fdc-119">--verbose</span></span> | <span data-ttu-id="d7fdc-120">Отображение подробных выходных данных.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-120">Show verbose output.</span></span> |<span data-ttu-id="d7fdc-121">dotnet openapi add file *-v* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="d7fdc-121">dotnet openapi add file *-v* .\OpenAPI.json</span></span> |
| <span data-ttu-id="d7fdc-122">-p</span><span class="sxs-lookup"><span data-stu-id="d7fdc-122">-p</span></span>|<span data-ttu-id="d7fdc-123">--updateProject</span><span class="sxs-lookup"><span data-stu-id="d7fdc-123">--updateProject</span></span> | <span data-ttu-id="d7fdc-124">Проект для выполнения операции.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-124">The project to operate on.</span></span> |<span data-ttu-id="d7fdc-125">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="d7fdc-125">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="d7fdc-126">-c</span><span class="sxs-lookup"><span data-stu-id="d7fdc-126">-c</span></span>|<span data-ttu-id="d7fdc-127">--code-generator</span><span class="sxs-lookup"><span data-stu-id="d7fdc-127">--code-generator</span></span>| <span data-ttu-id="d7fdc-128">Генератор кода, применяемый к ссылке.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-128">The code generator to apply to the reference.</span></span> <span data-ttu-id="d7fdc-129">Возможные значения: `NSwagCSharp` и `NSwagTypeScript`.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-129">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> <span data-ttu-id="d7fdc-130">Если атрибут `--code-generator` не задан, по умолчанию для средств будет выбрано `NSwagCSharp`.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-130">If `--code-generator` is not specified the tooling defaults to `NSwagCSharp`.</span></span>|<span data-ttu-id="d7fdc-131">dotnet openapi add file .\OpenApi.json --code-generator</span><span class="sxs-lookup"><span data-stu-id="d7fdc-131">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="d7fdc-132">-h</span><span class="sxs-lookup"><span data-stu-id="d7fdc-132">-h</span></span>|<span data-ttu-id="d7fdc-133">--help</span><span class="sxs-lookup"><span data-stu-id="d7fdc-133">--help</span></span>|<span data-ttu-id="d7fdc-134">Отображение справочных сведений.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-134">Show help information</span></span>|<span data-ttu-id="d7fdc-135">dotnet openapi add file --help</span><span class="sxs-lookup"><span data-stu-id="d7fdc-135">dotnet openapi add file --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="d7fdc-136">Аргументы</span><span class="sxs-lookup"><span data-stu-id="d7fdc-136">Arguments</span></span>

|  <span data-ttu-id="d7fdc-137">Аргумент</span><span class="sxs-lookup"><span data-stu-id="d7fdc-137">Argument</span></span>  | <span data-ttu-id="d7fdc-138">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="d7fdc-138">Description</span></span> | <span data-ttu-id="d7fdc-139">Пример</span><span class="sxs-lookup"><span data-stu-id="d7fdc-139">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="d7fdc-140">source-file</span><span class="sxs-lookup"><span data-stu-id="d7fdc-140">source-file</span></span> | <span data-ttu-id="d7fdc-141">Источник, из которого создается ссылка.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-141">The source to create a reference from.</span></span> <span data-ttu-id="d7fdc-142">Должен быть файлом OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-142">Must be an OpenAPI file.</span></span> |<span data-ttu-id="d7fdc-143">dotnet openapi add file *.\OpenAPI.json*</span><span class="sxs-lookup"><span data-stu-id="d7fdc-143">dotnet openapi add file *.\OpenAPI.json*</span></span> |

### <a name="add-url"></a><span data-ttu-id="d7fdc-144">Добавление URL-адреса</span><span class="sxs-lookup"><span data-stu-id="d7fdc-144">Add URL</span></span>

#### <a name="options"></a><span data-ttu-id="d7fdc-145">Параметры</span><span class="sxs-lookup"><span data-stu-id="d7fdc-145">Options</span></span>

| <span data-ttu-id="d7fdc-146">Короткий параметр</span><span class="sxs-lookup"><span data-stu-id="d7fdc-146">Short option</span></span>| <span data-ttu-id="d7fdc-147">Длинный параметр</span><span class="sxs-lookup"><span data-stu-id="d7fdc-147">Long option</span></span>| <span data-ttu-id="d7fdc-148">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="d7fdc-148">Description</span></span> | <span data-ttu-id="d7fdc-149">Пример</span><span class="sxs-lookup"><span data-stu-id="d7fdc-149">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="d7fdc-150">-v</span><span class="sxs-lookup"><span data-stu-id="d7fdc-150">-v</span></span>|<span data-ttu-id="d7fdc-151">--verbose</span><span class="sxs-lookup"><span data-stu-id="d7fdc-151">--verbose</span></span> | <span data-ttu-id="d7fdc-152">Отображение подробных выходных данных.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-152">Show verbose output.</span></span> |<span data-ttu-id="d7fdc-153">dotnet openapi add url *-v* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="d7fdc-153">dotnet openapi add url *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="d7fdc-154">-p</span><span class="sxs-lookup"><span data-stu-id="d7fdc-154">-p</span></span>|<span data-ttu-id="d7fdc-155">--updateProject</span><span class="sxs-lookup"><span data-stu-id="d7fdc-155">--updateProject</span></span> | <span data-ttu-id="d7fdc-156">Проект для выполнения операции.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-156">The project to operate on.</span></span> |<span data-ttu-id="d7fdc-157">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="d7fdc-157">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="d7fdc-158">-o</span><span class="sxs-lookup"><span data-stu-id="d7fdc-158">-o</span></span>|<span data-ttu-id="d7fdc-159">--output-file</span><span class="sxs-lookup"><span data-stu-id="d7fdc-159">--output-file</span></span> | <span data-ttu-id="d7fdc-160">Место размещения локальной копии файла OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-160">Where to place the local copy of the OpenAPI file.</span></span> |<span data-ttu-id="d7fdc-161">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span><span class="sxs-lookup"><span data-stu-id="d7fdc-161">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span></span> |
| <span data-ttu-id="d7fdc-162">-c</span><span class="sxs-lookup"><span data-stu-id="d7fdc-162">-c</span></span>|<span data-ttu-id="d7fdc-163">--code-generator</span><span class="sxs-lookup"><span data-stu-id="d7fdc-163">--code-generator</span></span>| <span data-ttu-id="d7fdc-164">Генератор кода, применяемый к ссылке.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-164">The code generator to apply to the reference.</span></span> <span data-ttu-id="d7fdc-165">Возможные значения: `NSwagCSharp` и `NSwagTypeScript`.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-165">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> |<span data-ttu-id="d7fdc-166">dotnet openapi add file .\OpenApi.json --code-generator</span><span class="sxs-lookup"><span data-stu-id="d7fdc-166">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="d7fdc-167">-h</span><span class="sxs-lookup"><span data-stu-id="d7fdc-167">-h</span></span>|<span data-ttu-id="d7fdc-168">--help</span><span class="sxs-lookup"><span data-stu-id="d7fdc-168">--help</span></span>|<span data-ttu-id="d7fdc-169">Отображение справочных сведений.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-169">Show help information</span></span>|<span data-ttu-id="d7fdc-170">dotnet openapi add url --help</span><span class="sxs-lookup"><span data-stu-id="d7fdc-170">dotnet openapi add url --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="d7fdc-171">Аргументы</span><span class="sxs-lookup"><span data-stu-id="d7fdc-171">Arguments</span></span>

|  <span data-ttu-id="d7fdc-172">Аргумент</span><span class="sxs-lookup"><span data-stu-id="d7fdc-172">Argument</span></span>  | <span data-ttu-id="d7fdc-173">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="d7fdc-173">Description</span></span> | <span data-ttu-id="d7fdc-174">Пример</span><span class="sxs-lookup"><span data-stu-id="d7fdc-174">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="d7fdc-175">source-URL</span><span class="sxs-lookup"><span data-stu-id="d7fdc-175">source-URL</span></span> | <span data-ttu-id="d7fdc-176">Источник, из которого создается ссылка.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-176">The source to create a reference from.</span></span> <span data-ttu-id="d7fdc-177">Должен быть URL-адресом.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-177">Must be a URL.</span></span> |<span data-ttu-id="d7fdc-178">dotnet openapi add url `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="d7fdc-178">dotnet openapi add url `https://contoso.com/openapi.json`</span></span> |

## <a name="remove"></a><span data-ttu-id="d7fdc-179">Удалить</span><span class="sxs-lookup"><span data-stu-id="d7fdc-179">Remove</span></span>

<span data-ttu-id="d7fdc-180">Удаляет ссылку на OpenAPI, соответствующую заданному имени файла, из файла *CSPROJ*.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-180">Removes the OpenAPI reference matching the given filename from the *.csproj* file.</span></span> <span data-ttu-id="d7fdc-181">При удалении ссылки OpenAPI клиенты не будут создаваться.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-181">When the OpenAPI reference is removed, clients won't be generated.</span></span> <span data-ttu-id="d7fdc-182">Локальные файлы *JSON* и *YAML* удаляются.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-182">Local *.json* and *.yaml* files are deleted.</span></span>

### <a name="options"></a><span data-ttu-id="d7fdc-183">Параметры</span><span class="sxs-lookup"><span data-stu-id="d7fdc-183">Options</span></span>

| <span data-ttu-id="d7fdc-184">Короткий параметр</span><span class="sxs-lookup"><span data-stu-id="d7fdc-184">Short option</span></span>| <span data-ttu-id="d7fdc-185">Длинный параметр</span><span class="sxs-lookup"><span data-stu-id="d7fdc-185">Long option</span></span>| <span data-ttu-id="d7fdc-186">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="d7fdc-186">Description</span></span>| <span data-ttu-id="d7fdc-187">Пример</span><span class="sxs-lookup"><span data-stu-id="d7fdc-187">Example</span></span> |
|-------|------|------------|---------|
| <span data-ttu-id="d7fdc-188">-v</span><span class="sxs-lookup"><span data-stu-id="d7fdc-188">-v</span></span>|<span data-ttu-id="d7fdc-189">--verbose</span><span class="sxs-lookup"><span data-stu-id="d7fdc-189">--verbose</span></span> | <span data-ttu-id="d7fdc-190">Отображение подробных выходных данных.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-190">Show verbose output.</span></span> |<span data-ttu-id="d7fdc-191">dotnet openapi remove *-v*</span><span class="sxs-lookup"><span data-stu-id="d7fdc-191">dotnet openapi remove *-v*</span></span>|
| <span data-ttu-id="d7fdc-192">-p</span><span class="sxs-lookup"><span data-stu-id="d7fdc-192">-p</span></span>|<span data-ttu-id="d7fdc-193">--updateProject</span><span class="sxs-lookup"><span data-stu-id="d7fdc-193">--updateProject</span></span> | <span data-ttu-id="d7fdc-194">Проект для выполнения операции.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-194">The project to operate on.</span></span> |<span data-ttu-id="d7fdc-195">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="d7fdc-195">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="d7fdc-196">-h</span><span class="sxs-lookup"><span data-stu-id="d7fdc-196">-h</span></span>|<span data-ttu-id="d7fdc-197">--help</span><span class="sxs-lookup"><span data-stu-id="d7fdc-197">--help</span></span>|<span data-ttu-id="d7fdc-198">Отображение справочных сведений.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-198">Show help information</span></span>|<span data-ttu-id="d7fdc-199">dotnet openapi remove --help</span><span class="sxs-lookup"><span data-stu-id="d7fdc-199">dotnet openapi remove --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="d7fdc-200">Аргументы</span><span class="sxs-lookup"><span data-stu-id="d7fdc-200">Arguments</span></span>

|  <span data-ttu-id="d7fdc-201">Аргумент</span><span class="sxs-lookup"><span data-stu-id="d7fdc-201">Argument</span></span>  | <span data-ttu-id="d7fdc-202">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="d7fdc-202">Description</span></span>| <span data-ttu-id="d7fdc-203">Пример</span><span class="sxs-lookup"><span data-stu-id="d7fdc-203">Example</span></span> |
| ------------|------------|---------|
| <span data-ttu-id="d7fdc-204">source-file</span><span class="sxs-lookup"><span data-stu-id="d7fdc-204">source-file</span></span> | <span data-ttu-id="d7fdc-205">Источник, ссылку на который необходимо удалить.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-205">The source to remove the reference to.</span></span> |<span data-ttu-id="d7fdc-206">dotnet openapi remove *.\OpenAPI.json*</span><span class="sxs-lookup"><span data-stu-id="d7fdc-206">dotnet openapi remove *.\OpenAPI.json*</span></span> |

## <a name="refresh"></a><span data-ttu-id="d7fdc-207">Обновление</span><span class="sxs-lookup"><span data-stu-id="d7fdc-207">Refresh</span></span>

<span data-ttu-id="d7fdc-208">Обновляет локальную версию файла, скачанного с использованием последнего содержимого из URL-адреса для скачивания.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-208">Refreshes the local version of a file that was downloaded using the latest content from the download URL.</span></span>

### <a name="options"></a><span data-ttu-id="d7fdc-209">Параметры</span><span class="sxs-lookup"><span data-stu-id="d7fdc-209">Options</span></span>

| <span data-ttu-id="d7fdc-210">Короткий параметр</span><span class="sxs-lookup"><span data-stu-id="d7fdc-210">Short option</span></span>| <span data-ttu-id="d7fdc-211">Длинный параметр</span><span class="sxs-lookup"><span data-stu-id="d7fdc-211">Long option</span></span>| <span data-ttu-id="d7fdc-212">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="d7fdc-212">Description</span></span> | <span data-ttu-id="d7fdc-213">Пример</span><span class="sxs-lookup"><span data-stu-id="d7fdc-213">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="d7fdc-214">-v</span><span class="sxs-lookup"><span data-stu-id="d7fdc-214">-v</span></span>|<span data-ttu-id="d7fdc-215">--verbose</span><span class="sxs-lookup"><span data-stu-id="d7fdc-215">--verbose</span></span> | <span data-ttu-id="d7fdc-216">Отображение подробных выходных данных.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-216">Show verbose output.</span></span> | <span data-ttu-id="d7fdc-217">dotnet openapi refresh *-v* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="d7fdc-217">dotnet openapi refresh *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="d7fdc-218">-p</span><span class="sxs-lookup"><span data-stu-id="d7fdc-218">-p</span></span>|<span data-ttu-id="d7fdc-219">--updateProject</span><span class="sxs-lookup"><span data-stu-id="d7fdc-219">--updateProject</span></span> | <span data-ttu-id="d7fdc-220">Проект для выполнения операции.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-220">The project to operate on.</span></span> | <span data-ttu-id="d7fdc-221">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="d7fdc-221">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="d7fdc-222">-h</span><span class="sxs-lookup"><span data-stu-id="d7fdc-222">-h</span></span>|<span data-ttu-id="d7fdc-223">--help</span><span class="sxs-lookup"><span data-stu-id="d7fdc-223">--help</span></span>|<span data-ttu-id="d7fdc-224">Отображение справочных сведений.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-224">Show help information</span></span>|<span data-ttu-id="d7fdc-225">dotnet openapi refresh --help</span><span class="sxs-lookup"><span data-stu-id="d7fdc-225">dotnet openapi refresh --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="d7fdc-226">Аргументы</span><span class="sxs-lookup"><span data-stu-id="d7fdc-226">Arguments</span></span>

|  <span data-ttu-id="d7fdc-227">Аргумент</span><span class="sxs-lookup"><span data-stu-id="d7fdc-227">Argument</span></span>  | <span data-ttu-id="d7fdc-228">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="d7fdc-228">Description</span></span> | <span data-ttu-id="d7fdc-229">Пример</span><span class="sxs-lookup"><span data-stu-id="d7fdc-229">Example</span></span> |
| ------------|-------------|---------|
| <span data-ttu-id="d7fdc-230">source-URL</span><span class="sxs-lookup"><span data-stu-id="d7fdc-230">source-URL</span></span> | <span data-ttu-id="d7fdc-231">URL-адрес, ссылку из которого необходимо обновить.</span><span class="sxs-lookup"><span data-stu-id="d7fdc-231">The URL to refresh the reference from.</span></span> | <span data-ttu-id="d7fdc-232">dotnet openapi refresh `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="d7fdc-232">dotnet openapi refresh `https://contoso.com/openapi.json`</span></span> |
