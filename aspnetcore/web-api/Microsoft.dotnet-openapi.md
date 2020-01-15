---
title: Разработка приложений ASP.NET Core с использованием OpenAPI
author: ryanbrandenburg
description: Здесь демонстрируется, как добавить ссылки в файлы OpenAPI с использованием средства Microsoft.dotnet-openapi.
ms.author: rybrande
ms.date: 09/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: 4be2846f0348788102672978a0487e646da434a0
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75354751"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a><span data-ttu-id="1db20-103">Разработка приложений ASP.NET Core с использованием средств OpenAPI</span><span class="sxs-lookup"><span data-stu-id="1db20-103">Develop ASP.NET Core apps using OpenAPI tools</span></span>

<span data-ttu-id="1db20-104">Автор: Райан Бранденбург (Ryan Brandenburg)</span><span class="sxs-lookup"><span data-stu-id="1db20-104">By Ryan Brandenburg</span></span>

<span data-ttu-id="1db20-105">[Microsoft.dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) — это [глобальное средство .NET Core](/dotnet/core/tools/global-tools) для управления ссылками [OpenAPI](https://github.com/OAI/OpenAPI-Specification) в рамках проекта.</span><span class="sxs-lookup"><span data-stu-id="1db20-105">[Microsoft.dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) is a [.NET Core Global Tool](/dotnet/core/tools/global-tools) for managing [OpenAPI](https://github.com/OAI/OpenAPI-Specification) references within a project.</span></span>

## <a name="installation"></a><span data-ttu-id="1db20-106">Установка</span><span class="sxs-lookup"><span data-stu-id="1db20-106">Installation</span></span>

<span data-ttu-id="1db20-107">Чтобы установить `Microsoft.dotnet-openapi`, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="1db20-107">To install `Microsoft.dotnet-openapi`, run the following command:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.dotnet-openapi
```

## <a name="add"></a><span data-ttu-id="1db20-108">Add</span><span class="sxs-lookup"><span data-stu-id="1db20-108">Add</span></span>

<span data-ttu-id="1db20-109">При добавлении ссылки OpenAPI с помощью любой из команд на этой странице в файл *CSPROJ* добавляется элемент `<OpenApiReference />`, аналогичный приведенному ниже:</span><span class="sxs-lookup"><span data-stu-id="1db20-109">Adding an OpenAPI reference using any of the commands on this page adds an `<OpenApiReference />` element similar to the following to the *.csproj* file:</span></span>

```xml
<OpenApiReference Include="openapi.json" />
```

<span data-ttu-id="1db20-110">Для вызова созданного клиентского кода приложению требуется предыдущая ссылка.</span><span class="sxs-lookup"><span data-stu-id="1db20-110">The preceding reference is required for the app to call the generated client code.</span></span>

<!-- TODO: Restore after https://github.com/aspnet/AspNetCore/issues/12738
### Add Project

#### Options

| Short option | Long option | Description | Example |
|-------|------|-------|---------|
| -p|--project | The project to operate on. |dotnet openapi add project *--project .\Ref.csproj* ../Ref/ProjRef.csproj |

#### Arguments

|  Argument  | Description | Example |
|-------------|-------------|---------|
| source-file | The source to create a reference from. Must be a project file. |dotnet openapi add project *../Ref/ProjRef.csproj* | -->

### <a name="add-file"></a><span data-ttu-id="1db20-111">Добавление файла</span><span class="sxs-lookup"><span data-stu-id="1db20-111">Add File</span></span>

#### <a name="options"></a><span data-ttu-id="1db20-112">Параметры</span><span class="sxs-lookup"><span data-stu-id="1db20-112">Options</span></span>

| <span data-ttu-id="1db20-113">Короткий параметр</span><span class="sxs-lookup"><span data-stu-id="1db20-113">Short option</span></span>| <span data-ttu-id="1db20-114">Длинный параметр</span><span class="sxs-lookup"><span data-stu-id="1db20-114">Long option</span></span>| <span data-ttu-id="1db20-115">Описание</span><span class="sxs-lookup"><span data-stu-id="1db20-115">Description</span></span> | <span data-ttu-id="1db20-116">Пример</span><span class="sxs-lookup"><span data-stu-id="1db20-116">Example</span></span> |
|-------|------|-------|---------|
| <span data-ttu-id="1db20-117">-p</span><span class="sxs-lookup"><span data-stu-id="1db20-117">-p</span></span>|<span data-ttu-id="1db20-118">--updateProject</span><span class="sxs-lookup"><span data-stu-id="1db20-118">--updateProject</span></span> | <span data-ttu-id="1db20-119">Проект для выполнения операции.</span><span class="sxs-lookup"><span data-stu-id="1db20-119">The project to operate on.</span></span> |<span data-ttu-id="1db20-120">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="1db20-120">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="1db20-121">-c</span><span class="sxs-lookup"><span data-stu-id="1db20-121">-c</span></span>|<span data-ttu-id="1db20-122">--code-generator</span><span class="sxs-lookup"><span data-stu-id="1db20-122">--code-generator</span></span>| <span data-ttu-id="1db20-123">Генератор кода, применяемый к ссылке.</span><span class="sxs-lookup"><span data-stu-id="1db20-123">The code generator to apply to the reference.</span></span> <span data-ttu-id="1db20-124">Возможные значения: `NSwagCSharp` и `NSwagTypeScript`.</span><span class="sxs-lookup"><span data-stu-id="1db20-124">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> <span data-ttu-id="1db20-125">Если атрибут `--code-generator` не задан, по умолчанию для средств будет выбрано `NSwagCSharp`.</span><span class="sxs-lookup"><span data-stu-id="1db20-125">If `--code-generator` is not specified the tooling defaults to `NSwagCSharp`.</span></span>|<span data-ttu-id="1db20-126">dotnet openapi add file .\OpenApi.json --code-generator</span><span class="sxs-lookup"><span data-stu-id="1db20-126">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="1db20-127">-h</span><span class="sxs-lookup"><span data-stu-id="1db20-127">-h</span></span>|<span data-ttu-id="1db20-128">--help</span><span class="sxs-lookup"><span data-stu-id="1db20-128">--help</span></span>|<span data-ttu-id="1db20-129">Отображение справочных сведений.</span><span class="sxs-lookup"><span data-stu-id="1db20-129">Show help information</span></span>|<span data-ttu-id="1db20-130">dotnet openapi add file --help</span><span class="sxs-lookup"><span data-stu-id="1db20-130">dotnet openapi add file --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="1db20-131">Аргументы</span><span class="sxs-lookup"><span data-stu-id="1db20-131">Arguments</span></span>

|  <span data-ttu-id="1db20-132">Аргумент</span><span class="sxs-lookup"><span data-stu-id="1db20-132">Argument</span></span>  | <span data-ttu-id="1db20-133">Описание</span><span class="sxs-lookup"><span data-stu-id="1db20-133">Description</span></span> | <span data-ttu-id="1db20-134">Пример</span><span class="sxs-lookup"><span data-stu-id="1db20-134">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="1db20-135">source-file</span><span class="sxs-lookup"><span data-stu-id="1db20-135">source-file</span></span> | <span data-ttu-id="1db20-136">Источник, из которого создается ссылка.</span><span class="sxs-lookup"><span data-stu-id="1db20-136">The source to create a reference from.</span></span> <span data-ttu-id="1db20-137">Должен быть файлом OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="1db20-137">Must be an OpenAPI file.</span></span> |<span data-ttu-id="1db20-138">dotnet openapi add file *.\OpenAPI.json*</span><span class="sxs-lookup"><span data-stu-id="1db20-138">dotnet openapi add file *.\OpenAPI.json*</span></span> |

### <a name="add-url"></a><span data-ttu-id="1db20-139">Добавление URL-адреса</span><span class="sxs-lookup"><span data-stu-id="1db20-139">Add URL</span></span>

#### <a name="options"></a><span data-ttu-id="1db20-140">Параметры</span><span class="sxs-lookup"><span data-stu-id="1db20-140">Options</span></span>

| <span data-ttu-id="1db20-141">Короткий параметр</span><span class="sxs-lookup"><span data-stu-id="1db20-141">Short option</span></span>| <span data-ttu-id="1db20-142">Длинный параметр</span><span class="sxs-lookup"><span data-stu-id="1db20-142">Long option</span></span>| <span data-ttu-id="1db20-143">Описание</span><span class="sxs-lookup"><span data-stu-id="1db20-143">Description</span></span> | <span data-ttu-id="1db20-144">Пример</span><span class="sxs-lookup"><span data-stu-id="1db20-144">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="1db20-145">-p</span><span class="sxs-lookup"><span data-stu-id="1db20-145">-p</span></span>|<span data-ttu-id="1db20-146">--updateProject</span><span class="sxs-lookup"><span data-stu-id="1db20-146">--updateProject</span></span> | <span data-ttu-id="1db20-147">Проект для выполнения операции.</span><span class="sxs-lookup"><span data-stu-id="1db20-147">The project to operate on.</span></span> |<span data-ttu-id="1db20-148">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="1db20-148">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="1db20-149">-o</span><span class="sxs-lookup"><span data-stu-id="1db20-149">-o</span></span>|<span data-ttu-id="1db20-150">--output-file</span><span class="sxs-lookup"><span data-stu-id="1db20-150">--output-file</span></span> | <span data-ttu-id="1db20-151">Место размещения локальной копии файла OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="1db20-151">Where to place the local copy of the OpenAPI file.</span></span> |<span data-ttu-id="1db20-152">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span><span class="sxs-lookup"><span data-stu-id="1db20-152">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span></span> |
| <span data-ttu-id="1db20-153">-c</span><span class="sxs-lookup"><span data-stu-id="1db20-153">-c</span></span>|<span data-ttu-id="1db20-154">--code-generator</span><span class="sxs-lookup"><span data-stu-id="1db20-154">--code-generator</span></span>| <span data-ttu-id="1db20-155">Генератор кода, применяемый к ссылке.</span><span class="sxs-lookup"><span data-stu-id="1db20-155">The code generator to apply to the reference.</span></span> <span data-ttu-id="1db20-156">Возможные значения: `NSwagCSharp` и `NSwagTypeScript`.</span><span class="sxs-lookup"><span data-stu-id="1db20-156">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> |<span data-ttu-id="1db20-157">dotnet openapi add file .\OpenApi.json --code-generator</span><span class="sxs-lookup"><span data-stu-id="1db20-157">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="1db20-158">-h</span><span class="sxs-lookup"><span data-stu-id="1db20-158">-h</span></span>|<span data-ttu-id="1db20-159">--help</span><span class="sxs-lookup"><span data-stu-id="1db20-159">--help</span></span>|<span data-ttu-id="1db20-160">Отображение справочных сведений.</span><span class="sxs-lookup"><span data-stu-id="1db20-160">Show help information</span></span>|<span data-ttu-id="1db20-161">dotnet openapi add url --help</span><span class="sxs-lookup"><span data-stu-id="1db20-161">dotnet openapi add url --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="1db20-162">Аргументы</span><span class="sxs-lookup"><span data-stu-id="1db20-162">Arguments</span></span>

|  <span data-ttu-id="1db20-163">Аргумент</span><span class="sxs-lookup"><span data-stu-id="1db20-163">Argument</span></span>  | <span data-ttu-id="1db20-164">Описание</span><span class="sxs-lookup"><span data-stu-id="1db20-164">Description</span></span> | <span data-ttu-id="1db20-165">Пример</span><span class="sxs-lookup"><span data-stu-id="1db20-165">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="1db20-166">source-URL</span><span class="sxs-lookup"><span data-stu-id="1db20-166">source-URL</span></span> | <span data-ttu-id="1db20-167">Источник, из которого создается ссылка.</span><span class="sxs-lookup"><span data-stu-id="1db20-167">The source to create a reference from.</span></span> <span data-ttu-id="1db20-168">Должен быть URL-адресом.</span><span class="sxs-lookup"><span data-stu-id="1db20-168">Must be a URL.</span></span> |<span data-ttu-id="1db20-169">dotnet openapi add url `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="1db20-169">dotnet openapi add url `https://contoso.com/openapi.json`</span></span> |

## <a name="remove"></a><span data-ttu-id="1db20-170">Удалить</span><span class="sxs-lookup"><span data-stu-id="1db20-170">Remove</span></span>

<span data-ttu-id="1db20-171">Удаляет ссылку на OpenAPI, соответствующую заданному имени файла, из файла *CSPROJ*.</span><span class="sxs-lookup"><span data-stu-id="1db20-171">Removes the OpenAPI reference matching the given filename from the *.csproj* file.</span></span> <span data-ttu-id="1db20-172">При удалении ссылки OpenAPI клиенты не будут создаваться.</span><span class="sxs-lookup"><span data-stu-id="1db20-172">When the OpenAPI reference is removed, clients won't be generated.</span></span> <span data-ttu-id="1db20-173">Локальные файлы *JSON* и *YAML* удаляются.</span><span class="sxs-lookup"><span data-stu-id="1db20-173">Local *.json* and *.yaml* files are deleted.</span></span>

### <a name="options"></a><span data-ttu-id="1db20-174">Параметры</span><span class="sxs-lookup"><span data-stu-id="1db20-174">Options</span></span>

| <span data-ttu-id="1db20-175">Короткий параметр</span><span class="sxs-lookup"><span data-stu-id="1db20-175">Short option</span></span>| <span data-ttu-id="1db20-176">Длинный параметр</span><span class="sxs-lookup"><span data-stu-id="1db20-176">Long option</span></span>| <span data-ttu-id="1db20-177">Описание</span><span class="sxs-lookup"><span data-stu-id="1db20-177">Description</span></span>| <span data-ttu-id="1db20-178">Пример</span><span class="sxs-lookup"><span data-stu-id="1db20-178">Example</span></span> |
|-------|------|------------|---------|
| <span data-ttu-id="1db20-179">-p</span><span class="sxs-lookup"><span data-stu-id="1db20-179">-p</span></span>|<span data-ttu-id="1db20-180">--updateProject</span><span class="sxs-lookup"><span data-stu-id="1db20-180">--updateProject</span></span> | <span data-ttu-id="1db20-181">Проект для выполнения операции.</span><span class="sxs-lookup"><span data-stu-id="1db20-181">The project to operate on.</span></span> |<span data-ttu-id="1db20-182">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="1db20-182">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="1db20-183">-h</span><span class="sxs-lookup"><span data-stu-id="1db20-183">-h</span></span>|<span data-ttu-id="1db20-184">--help</span><span class="sxs-lookup"><span data-stu-id="1db20-184">--help</span></span>|<span data-ttu-id="1db20-185">Отображение справочных сведений.</span><span class="sxs-lookup"><span data-stu-id="1db20-185">Show help information</span></span>|<span data-ttu-id="1db20-186">dotnet openapi remove --help</span><span class="sxs-lookup"><span data-stu-id="1db20-186">dotnet openapi remove --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="1db20-187">Аргументы</span><span class="sxs-lookup"><span data-stu-id="1db20-187">Arguments</span></span>

|  <span data-ttu-id="1db20-188">Аргумент</span><span class="sxs-lookup"><span data-stu-id="1db20-188">Argument</span></span>  | <span data-ttu-id="1db20-189">Описание</span><span class="sxs-lookup"><span data-stu-id="1db20-189">Description</span></span>| <span data-ttu-id="1db20-190">Пример</span><span class="sxs-lookup"><span data-stu-id="1db20-190">Example</span></span> |
| ------------|------------|---------|
| <span data-ttu-id="1db20-191">source-file</span><span class="sxs-lookup"><span data-stu-id="1db20-191">source-file</span></span> | <span data-ttu-id="1db20-192">Источник, ссылку на который необходимо удалить.</span><span class="sxs-lookup"><span data-stu-id="1db20-192">The source to remove the reference to.</span></span> |<span data-ttu-id="1db20-193">dotnet openapi remove *.\OpenAPI.json*</span><span class="sxs-lookup"><span data-stu-id="1db20-193">dotnet openapi remove *.\OpenAPI.json*</span></span> |

## <a name="refresh"></a><span data-ttu-id="1db20-194">Обновление</span><span class="sxs-lookup"><span data-stu-id="1db20-194">Refresh</span></span>

<span data-ttu-id="1db20-195">Обновляет локальную версию файла, скачанного с использованием последнего содержимого из URL-адреса для скачивания.</span><span class="sxs-lookup"><span data-stu-id="1db20-195">Refreshes the local version of a file that was downloaded using the latest content from the download URL.</span></span>

### <a name="options"></a><span data-ttu-id="1db20-196">Параметры</span><span class="sxs-lookup"><span data-stu-id="1db20-196">Options</span></span>

| <span data-ttu-id="1db20-197">Короткий параметр</span><span class="sxs-lookup"><span data-stu-id="1db20-197">Short option</span></span>| <span data-ttu-id="1db20-198">Длинный параметр</span><span class="sxs-lookup"><span data-stu-id="1db20-198">Long option</span></span>| <span data-ttu-id="1db20-199">Описание</span><span class="sxs-lookup"><span data-stu-id="1db20-199">Description</span></span> | <span data-ttu-id="1db20-200">Пример</span><span class="sxs-lookup"><span data-stu-id="1db20-200">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="1db20-201">-p</span><span class="sxs-lookup"><span data-stu-id="1db20-201">-p</span></span>|<span data-ttu-id="1db20-202">--updateProject</span><span class="sxs-lookup"><span data-stu-id="1db20-202">--updateProject</span></span> | <span data-ttu-id="1db20-203">Проект для выполнения операции.</span><span class="sxs-lookup"><span data-stu-id="1db20-203">The project to operate on.</span></span> | <span data-ttu-id="1db20-204">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="1db20-204">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="1db20-205">-h</span><span class="sxs-lookup"><span data-stu-id="1db20-205">-h</span></span>|<span data-ttu-id="1db20-206">--help</span><span class="sxs-lookup"><span data-stu-id="1db20-206">--help</span></span>|<span data-ttu-id="1db20-207">Отображение справочных сведений.</span><span class="sxs-lookup"><span data-stu-id="1db20-207">Show help information</span></span>|<span data-ttu-id="1db20-208">dotnet openapi refresh --help</span><span class="sxs-lookup"><span data-stu-id="1db20-208">dotnet openapi refresh --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="1db20-209">Аргументы</span><span class="sxs-lookup"><span data-stu-id="1db20-209">Arguments</span></span>

|  <span data-ttu-id="1db20-210">Аргумент</span><span class="sxs-lookup"><span data-stu-id="1db20-210">Argument</span></span>  | <span data-ttu-id="1db20-211">Описание</span><span class="sxs-lookup"><span data-stu-id="1db20-211">Description</span></span> | <span data-ttu-id="1db20-212">Пример</span><span class="sxs-lookup"><span data-stu-id="1db20-212">Example</span></span> |
| ------------|-------------|---------|
| <span data-ttu-id="1db20-213">source-URL</span><span class="sxs-lookup"><span data-stu-id="1db20-213">source-URL</span></span> | <span data-ttu-id="1db20-214">URL-адрес, ссылку из которого необходимо обновить.</span><span class="sxs-lookup"><span data-stu-id="1db20-214">The URL to refresh the reference from.</span></span> | <span data-ttu-id="1db20-215">dotnet openapi refresh `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="1db20-215">dotnet openapi refresh `https://contoso.com/openapi.json`</span></span> |
