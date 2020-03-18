---
title: Поставщики файлов в ASP.NET Core
author: rick-anderson
description: Сведения о том, как ASP.NET Core абстрагирует доступ к файловой системе с помощью поставщиков файлов.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: fundamentals/file-providers
ms.openlocfilehash: 34a48bbcf9ffb20bb61f89c80adedc1cc4783988
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78647050"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="21ceb-103">Поставщики файлов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="21ceb-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="21ceb-104">Автор: [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="21ceb-104">By [Steve Smith](https://ardalis.com/)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="21ceb-105">ASP.NET Core абстрагирует доступ к файловой системе с помощью поставщиков файлов.</span><span class="sxs-lookup"><span data-stu-id="21ceb-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="21ceb-106">В рамках платформы ASP.NET Core используются поставщики файлов.</span><span class="sxs-lookup"><span data-stu-id="21ceb-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="21ceb-107">`IWebHostEnvironment` предоставляет [корневой каталог содержимого](xref:fundamentals/index#content-root) и [корневой каталог документов](xref:fundamentals/index#web-root) приложения в виде типов `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="21ceb-107">`IWebHostEnvironment` exposes the app's [content root](xref:fundamentals/index#content-root) and [web root](xref:fundamentals/index#web-root) as `IFileProvider` types.</span></span>
* <span data-ttu-id="21ceb-108">[ПО промежуточного слоя для статических файлов](xref:fundamentals/static-files) использует поставщики файлов для поиска статических файлов.</span><span class="sxs-lookup"><span data-stu-id="21ceb-108">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="21ceb-109">[Razor](xref:mvc/views/razor) использует поставщики файлов для поиска страниц и представлений.</span><span class="sxs-lookup"><span data-stu-id="21ceb-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="21ceb-110">Инструменты .NET Core используют поставщики файлов и стандартные маски, чтобы указать файлы для публикации.</span><span class="sxs-lookup"><span data-stu-id="21ceb-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="21ceb-111">[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="21ceb-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="21ceb-112">Интерфейсы поставщика файлов</span><span class="sxs-lookup"><span data-stu-id="21ceb-112">File Provider interfaces</span></span>

<span data-ttu-id="21ceb-113">Основной интерфейс — <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="21ceb-113">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="21ceb-114">`IFileProvider` предоставляет методы для следующих действий:</span><span class="sxs-lookup"><span data-stu-id="21ceb-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="21ceb-115">получение сведений о файле (<xref:Microsoft.Extensions.FileProviders.IFileInfo>);</span><span class="sxs-lookup"><span data-stu-id="21ceb-115">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="21ceb-116">получение сведений о каталоге (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>);</span><span class="sxs-lookup"><span data-stu-id="21ceb-116">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="21ceb-117">настройка уведомлений об изменениях (с помощью <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span><span class="sxs-lookup"><span data-stu-id="21ceb-117">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="21ceb-118">`IFileInfo` предоставляет методы и свойства для работы с файлами:</span><span class="sxs-lookup"><span data-stu-id="21ceb-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="21ceb-119"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (в байтах);</span><span class="sxs-lookup"><span data-stu-id="21ceb-119"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="21ceb-120"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> (дата).</span><span class="sxs-lookup"><span data-stu-id="21ceb-120"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="21ceb-121">Данные из файла можно считывать с помощью метода [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*).</span><span class="sxs-lookup"><span data-stu-id="21ceb-121">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="21ceb-122">Пример приложения демонстрирует, как настроить в `Startup.ConfigureServices` поставщике файлов, чтобы использовать его в приложении путем [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="21ceb-122">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="21ceb-123">Реализации поставщиков файлов</span><span class="sxs-lookup"><span data-stu-id="21ceb-123">File Provider implementations</span></span>

<span data-ttu-id="21ceb-124">Доступны три реализации `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="21ceb-124">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="21ceb-125">Реализация</span><span class="sxs-lookup"><span data-stu-id="21ceb-125">Implementation</span></span> | <span data-ttu-id="21ceb-126">Описание</span><span class="sxs-lookup"><span data-stu-id="21ceb-126">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="21ceb-127">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="21ceb-127">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="21ceb-128">Физический поставщик используется для доступа к физическим файлам системы.</span><span class="sxs-lookup"><span data-stu-id="21ceb-128">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="21ceb-129">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="21ceb-129">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="21ceb-130">Поставщик внедренных манифестов используется для доступа к файлам, внедренным в сборки.</span><span class="sxs-lookup"><span data-stu-id="21ceb-130">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="21ceb-131">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="21ceb-131">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="21ceb-132">Составной поставщик используется для предоставления комбинированного доступа к файлам и каталогам из одного или нескольких поставщиков.</span><span class="sxs-lookup"><span data-stu-id="21ceb-132">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="21ceb-133">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="21ceb-133">PhysicalFileProvider</span></span>

<span data-ttu-id="21ceb-134"><xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> предоставляет доступ к физической файловой системе.</span><span class="sxs-lookup"><span data-stu-id="21ceb-134">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="21ceb-135">`PhysicalFileProvider` использует тип <xref:System.IO.File?displayProperty=fullName> (для физического поставщика), ограничивая все пути каталогом и его дочерними элементами.</span><span class="sxs-lookup"><span data-stu-id="21ceb-135">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="21ceb-136">Такая привязка к области защищает от доступа к файловой системе за пределами указанного каталога и его дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="21ceb-136">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="21ceb-137">Самый распространенный подход к созданию и использованию `PhysicalFileProvider` — запросить `IFileProvider` в конструкторе путем [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="21ceb-137">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="21ceb-138">При непосредственном создании экземпляра этого поставщика нужно указать путь к каталогу, который станет базовым путем для всех запросов, выполненных с помощью этого поставщика.</span><span class="sxs-lookup"><span data-stu-id="21ceb-138">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="21ceb-139">Следующий пример кода демонстрирует, как создать и использовать `PhysicalFileProvider` для получения содержимого каталога и сведений о файлах:</span><span class="sxs-lookup"><span data-stu-id="21ceb-139">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="21ceb-140">В предшествующем примере используются следующие типы:</span><span class="sxs-lookup"><span data-stu-id="21ceb-140">Types in the preceding example:</span></span>

* <span data-ttu-id="21ceb-141">`provider` является `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="21ceb-141">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="21ceb-142">`contents` является `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="21ceb-142">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="21ceb-143">`fileInfo` является `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="21ceb-143">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="21ceb-144">С помощью поставщика файлов вы можете выполнить итерацию по каталогу, указанному в параметре `applicationRoot`, или вызвать `GetFileInfo` для получения сведений о файлах.</span><span class="sxs-lookup"><span data-stu-id="21ceb-144">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="21ceb-145">Поставщик файлов не имеет доступ к каталогам за пределами `applicationRoot`.</span><span class="sxs-lookup"><span data-stu-id="21ceb-145">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="21ceb-146">Этот пример приложения создает поставщик для приложения в классе `Startup.ConfigureServices`, используя [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span><span class="sxs-lookup"><span data-stu-id="21ceb-146">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="21ceb-147">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="21ceb-147">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="21ceb-148"><xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> используется для доступа к файлам, внедренным в сборки.</span><span class="sxs-lookup"><span data-stu-id="21ceb-148">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="21ceb-149">`ManifestEmbeddedFileProvider` использует манифест, скомпилированный в сборку, для воссоздания исходных путей для внедренных файлов.</span><span class="sxs-lookup"><span data-stu-id="21ceb-149">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="21ceb-150">Добавьте ссылку на проект в пакет [Microsoft.Extensions.FileProviders.Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded).</span><span class="sxs-lookup"><span data-stu-id="21ceb-150">Add a package reference to the project for the [Microsoft.Extensions.FileProviders.Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) package.</span></span>

<span data-ttu-id="21ceb-151">Чтобы создать манифест для внедренных файлов, задайте для свойства `<GenerateEmbeddedFilesManifest>` значение `true`.</span><span class="sxs-lookup"><span data-stu-id="21ceb-151">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="21ceb-152">Выберите файлы для внедрения с помощью [\<EmbeddedResource>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span><span class="sxs-lookup"><span data-stu-id="21ceb-152">Specify the files to embed with [\<EmbeddedResource>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

<span data-ttu-id="21ceb-153">Используйте [стандартные маски](#glob-patterns) для указания одного или нескольких файлов, которые вы хотите внедрить в сборку.</span><span class="sxs-lookup"><span data-stu-id="21ceb-153">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="21ceb-154">Наш пример приложения создает `ManifestEmbeddedFileProvider` и передает в соответствующий конструктор текущую выполняемую сборку.</span><span class="sxs-lookup"><span data-stu-id="21ceb-154">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="21ceb-155">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="21ceb-155">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(typeof(Program).Assembly);
```

<span data-ttu-id="21ceb-156">Дополнительные перегрузки позволяют сделать следующее:</span><span class="sxs-lookup"><span data-stu-id="21ceb-156">Additional overloads allow you to:</span></span>

* <span data-ttu-id="21ceb-157">указать относительный путь к файлу;</span><span class="sxs-lookup"><span data-stu-id="21ceb-157">Specify a relative file path.</span></span>
* <span data-ttu-id="21ceb-158">ограничить файлы по дате последнего изменения;</span><span class="sxs-lookup"><span data-stu-id="21ceb-158">Scope files to a last modified date.</span></span>
* <span data-ttu-id="21ceb-159">указать имя внедренного ресурса, содержащего внедренный файл манифеста.</span><span class="sxs-lookup"><span data-stu-id="21ceb-159">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="21ceb-160">Перегрузка</span><span class="sxs-lookup"><span data-stu-id="21ceb-160">Overload</span></span> | <span data-ttu-id="21ceb-161">Описание</span><span class="sxs-lookup"><span data-stu-id="21ceb-161">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="21ceb-162">Принимает необязательный параметр `root` со значением относительного пути.</span><span class="sxs-lookup"><span data-stu-id="21ceb-162">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="21ceb-163">Укажите `root`, чтобы ограничить вызовы к <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> только ресурсами по указанному пути.</span><span class="sxs-lookup"><span data-stu-id="21ceb-163">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="21ceb-164">Принимает необязательный параметр `root` со значением относительного пути и параметр `lastModified` со значением даты (<xref:System.DateTimeOffset>).</span><span class="sxs-lookup"><span data-stu-id="21ceb-164">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="21ceb-165">Параметр `lastModified` ограничивает дату последнего изменения для экземпляров <xref:Microsoft.Extensions.FileProviders.IFileInfo>, возвращаемых функцией <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="21ceb-165">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="21ceb-166">Принимает необязательный параметр `root` со значением относительного пути, дату `lastModified` и параметры `manifestName`.</span><span class="sxs-lookup"><span data-stu-id="21ceb-166">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="21ceb-167">`manifestName` здесь представляет имя встроенного ресурса, содержащего манифест.</span><span class="sxs-lookup"><span data-stu-id="21ceb-167">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="21ceb-168">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="21ceb-168">CompositeFileProvider</span></span>

<span data-ttu-id="21ceb-169"><xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> объединяет экземпляры `IFileProvider`, предоставляя единый интерфейс для работы с файлами от нескольких поставщиков.</span><span class="sxs-lookup"><span data-stu-id="21ceb-169">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="21ceb-170">При создании `CompositeFileProvider` передайте в соответствующий конструктор один или несколько экземпляров `IFileProvider`:</span><span class="sxs-lookup"><span data-stu-id="21ceb-170">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="21ceb-171">В нашем примере приложения `PhysicalFileProvider` и `ManifestEmbeddedFileProvider` предоставляют файлы для `CompositeFileProvider` с регистрацией в контейнере служб приложения:</span><span class="sxs-lookup"><span data-stu-id="21ceb-171">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="21ceb-172">Отслеживание изменений</span><span class="sxs-lookup"><span data-stu-id="21ceb-172">Watch for changes</span></span>

<span data-ttu-id="21ceb-173">Метод [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) позволяет контролировать изменения в одном или нескольких файлах или каталогах.</span><span class="sxs-lookup"><span data-stu-id="21ceb-173">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="21ceb-174">`Watch` принимает строку пути, которая можно использовать [стандартные маски](#glob-patterns) для указания нескольких файлов.</span><span class="sxs-lookup"><span data-stu-id="21ceb-174">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="21ceb-175">`Watch` возвращает <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span><span class="sxs-lookup"><span data-stu-id="21ceb-175">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="21ceb-176">Этот токен изменений предоставляет следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="21ceb-176">The change token exposes:</span></span>

* <span data-ttu-id="21ceb-177"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; — свойство, которое можно проверить, чтобы определить, произошло ли изменение.</span><span class="sxs-lookup"><span data-stu-id="21ceb-177"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="21ceb-178"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; — вызывается при обнаружении изменений в указанной строке пути.</span><span class="sxs-lookup"><span data-stu-id="21ceb-178"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="21ceb-179">Каждый токен изменения выполняет соответствующий обратный вызов только в ответ на отдельное изменение.</span><span class="sxs-lookup"><span data-stu-id="21ceb-179">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="21ceb-180">Чтобы реализовать непрерывный мониторинг, используйте <xref:System.Threading.Tasks.TaskCompletionSource`1>, как показано ниже, или повторно создавайте экземпляры `IChangeToken` в ответ на изменения.</span><span class="sxs-lookup"><span data-stu-id="21ceb-180">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="21ceb-181">В нашем примере консольное приложение *WatchConsole* будет отображать сообщение при изменении текстового файла:</span><span class="sxs-lookup"><span data-stu-id="21ceb-181">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/3.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="21ceb-182">Некоторые файловые системы, такие как контейнеры Docker и сетевые папки, не могут надежно отправлять уведомления об изменениях.</span><span class="sxs-lookup"><span data-stu-id="21ceb-182">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="21ceb-183">Задайте для переменной среды `DOTNET_USE_POLLING_FILE_WATCHER` значение `1` или `true`, чтобы опрашивать файловую систему на предмет изменений каждые 4 секунды (это значение нельзя изменить).</span><span class="sxs-lookup"><span data-stu-id="21ceb-183">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="21ceb-184">Стандартные маски</span><span class="sxs-lookup"><span data-stu-id="21ceb-184">Glob patterns</span></span>

<span data-ttu-id="21ceb-185">В путях файловой системы используются шаблоны подстановочных знаков, которые называются *стандартными масками*.</span><span class="sxs-lookup"><span data-stu-id="21ceb-185">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="21ceb-186">Эти маски позволяют указывать группы файлов.</span><span class="sxs-lookup"><span data-stu-id="21ceb-186">Specify groups of files with these patterns.</span></span> <span data-ttu-id="21ceb-187">Поддерживаются два подстановочных знака — `*` и `**`.</span><span class="sxs-lookup"><span data-stu-id="21ceb-187">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="21ceb-188">Совпадает с любым элементом на текущем уровне папок, любым именем или расширением файла.</span><span class="sxs-lookup"><span data-stu-id="21ceb-188">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="21ceb-189">Совпадения завершаются символами `/` и `.` в пути файла.</span><span class="sxs-lookup"><span data-stu-id="21ceb-189">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="21ceb-190">Совпадает со всем содержимым на разных уровнях каталогов.</span><span class="sxs-lookup"><span data-stu-id="21ceb-190">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="21ceb-191">Может использоваться для рекурсивного сопоставления множества файлов в иерархии каталогов.</span><span class="sxs-lookup"><span data-stu-id="21ceb-191">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="21ceb-192">**Примеры стандартных масок**</span><span class="sxs-lookup"><span data-stu-id="21ceb-192">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="21ceb-193">Соответствует конкретному файлу в заданном каталоге.</span><span class="sxs-lookup"><span data-stu-id="21ceb-193">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="21ceb-194">Соответствует всем файлам с расширением *.txt* в заданном каталоге.</span><span class="sxs-lookup"><span data-stu-id="21ceb-194">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="21ceb-195">Соответствует всем файлам `appsettings.json` в любом каталоге, расположенном ровно на один уровень ниже каталога *directory*.</span><span class="sxs-lookup"><span data-stu-id="21ceb-195">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="21ceb-196">Соответствует всем файлам с расширением *.txt*, находящимся на любом уровне ниже каталога *directory*.</span><span class="sxs-lookup"><span data-stu-id="21ceb-196">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="21ceb-197">ASP.NET Core абстрагирует доступ к файловой системе с помощью поставщиков файлов.</span><span class="sxs-lookup"><span data-stu-id="21ceb-197">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="21ceb-198">В рамках платформы ASP.NET Core используются поставщики файлов.</span><span class="sxs-lookup"><span data-stu-id="21ceb-198">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="21ceb-199"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> предоставляет [корневой каталог содержимого](xref:fundamentals/index#content-root) и [корневой каталог документов](xref:fundamentals/index#web-root) приложения в виде типов `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="21ceb-199"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> exposes the app's [content root](xref:fundamentals/index#content-root) and [web root](xref:fundamentals/index#web-root) as `IFileProvider` types.</span></span>
* <span data-ttu-id="21ceb-200">[ПО промежуточного слоя для статических файлов](xref:fundamentals/static-files) использует поставщики файлов для поиска статических файлов.</span><span class="sxs-lookup"><span data-stu-id="21ceb-200">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="21ceb-201">[Razor](xref:mvc/views/razor) использует поставщики файлов для поиска страниц и представлений.</span><span class="sxs-lookup"><span data-stu-id="21ceb-201">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="21ceb-202">Инструменты .NET Core используют поставщики файлов и стандартные маски, чтобы указать файлы для публикации.</span><span class="sxs-lookup"><span data-stu-id="21ceb-202">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="21ceb-203">[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="21ceb-203">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="21ceb-204">Интерфейсы поставщика файлов</span><span class="sxs-lookup"><span data-stu-id="21ceb-204">File Provider interfaces</span></span>

<span data-ttu-id="21ceb-205">Основной интерфейс — <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="21ceb-205">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="21ceb-206">`IFileProvider` предоставляет методы для следующих действий:</span><span class="sxs-lookup"><span data-stu-id="21ceb-206">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="21ceb-207">получение сведений о файле (<xref:Microsoft.Extensions.FileProviders.IFileInfo>);</span><span class="sxs-lookup"><span data-stu-id="21ceb-207">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="21ceb-208">получение сведений о каталоге (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>);</span><span class="sxs-lookup"><span data-stu-id="21ceb-208">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="21ceb-209">настройка уведомлений об изменениях (с помощью <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span><span class="sxs-lookup"><span data-stu-id="21ceb-209">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="21ceb-210">`IFileInfo` предоставляет методы и свойства для работы с файлами:</span><span class="sxs-lookup"><span data-stu-id="21ceb-210">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="21ceb-211"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (в байтах);</span><span class="sxs-lookup"><span data-stu-id="21ceb-211"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="21ceb-212"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> (дата).</span><span class="sxs-lookup"><span data-stu-id="21ceb-212"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="21ceb-213">Данные из файла можно считывать с помощью метода [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*).</span><span class="sxs-lookup"><span data-stu-id="21ceb-213">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="21ceb-214">Пример приложения демонстрирует, как настроить в `Startup.ConfigureServices` поставщике файлов, чтобы использовать его в приложении путем [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="21ceb-214">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="21ceb-215">Реализации поставщиков файлов</span><span class="sxs-lookup"><span data-stu-id="21ceb-215">File Provider implementations</span></span>

<span data-ttu-id="21ceb-216">Доступны три реализации `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="21ceb-216">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="21ceb-217">Реализация</span><span class="sxs-lookup"><span data-stu-id="21ceb-217">Implementation</span></span> | <span data-ttu-id="21ceb-218">Описание</span><span class="sxs-lookup"><span data-stu-id="21ceb-218">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="21ceb-219">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="21ceb-219">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="21ceb-220">Физический поставщик используется для доступа к физическим файлам системы.</span><span class="sxs-lookup"><span data-stu-id="21ceb-220">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="21ceb-221">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="21ceb-221">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="21ceb-222">Поставщик внедренных манифестов используется для доступа к файлам, внедренным в сборки.</span><span class="sxs-lookup"><span data-stu-id="21ceb-222">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="21ceb-223">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="21ceb-223">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="21ceb-224">Составной поставщик используется для предоставления комбинированного доступа к файлам и каталогам из одного или нескольких поставщиков.</span><span class="sxs-lookup"><span data-stu-id="21ceb-224">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="21ceb-225">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="21ceb-225">PhysicalFileProvider</span></span>

<span data-ttu-id="21ceb-226"><xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> предоставляет доступ к физической файловой системе.</span><span class="sxs-lookup"><span data-stu-id="21ceb-226">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="21ceb-227">`PhysicalFileProvider` использует тип <xref:System.IO.File?displayProperty=fullName> (для физического поставщика), ограничивая все пути каталогом и его дочерними элементами.</span><span class="sxs-lookup"><span data-stu-id="21ceb-227">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="21ceb-228">Такая привязка к области защищает от доступа к файловой системе за пределами указанного каталога и его дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="21ceb-228">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="21ceb-229">Самый распространенный подход к созданию и использованию `PhysicalFileProvider` — запросить `IFileProvider` в конструкторе путем [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="21ceb-229">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="21ceb-230">При непосредственном создании экземпляра этого поставщика нужно указать путь к каталогу, который станет базовым путем для всех запросов, выполненных с помощью этого поставщика.</span><span class="sxs-lookup"><span data-stu-id="21ceb-230">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="21ceb-231">Следующий пример кода демонстрирует, как создать и использовать `PhysicalFileProvider` для получения содержимого каталога и сведений о файлах:</span><span class="sxs-lookup"><span data-stu-id="21ceb-231">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="21ceb-232">В предшествующем примере используются следующие типы:</span><span class="sxs-lookup"><span data-stu-id="21ceb-232">Types in the preceding example:</span></span>

* <span data-ttu-id="21ceb-233">`provider` является `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="21ceb-233">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="21ceb-234">`contents` является `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="21ceb-234">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="21ceb-235">`fileInfo` является `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="21ceb-235">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="21ceb-236">С помощью поставщика файлов вы можете выполнить итерацию по каталогу, указанному в параметре `applicationRoot`, или вызвать `GetFileInfo` для получения сведений о файлах.</span><span class="sxs-lookup"><span data-stu-id="21ceb-236">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="21ceb-237">Поставщик файлов не имеет доступ к каталогам за пределами `applicationRoot`.</span><span class="sxs-lookup"><span data-stu-id="21ceb-237">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="21ceb-238">Этот пример приложения создает поставщик для приложения в классе `Startup.ConfigureServices`, используя [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span><span class="sxs-lookup"><span data-stu-id="21ceb-238">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="21ceb-239">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="21ceb-239">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="21ceb-240"><xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> используется для доступа к файлам, внедренным в сборки.</span><span class="sxs-lookup"><span data-stu-id="21ceb-240">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="21ceb-241">`ManifestEmbeddedFileProvider` использует манифест, скомпилированный в сборку, для воссоздания исходных путей для внедренных файлов.</span><span class="sxs-lookup"><span data-stu-id="21ceb-241">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="21ceb-242">Чтобы создать манифест для внедренных файлов, задайте для свойства `<GenerateEmbeddedFilesManifest>` значение `true`.</span><span class="sxs-lookup"><span data-stu-id="21ceb-242">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="21ceb-243">Выберите файлы для внедрения с помощью [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span><span class="sxs-lookup"><span data-stu-id="21ceb-243">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="21ceb-244">Используйте [стандартные маски](#glob-patterns) для указания одного или нескольких файлов, которые вы хотите внедрить в сборку.</span><span class="sxs-lookup"><span data-stu-id="21ceb-244">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="21ceb-245">Наш пример приложения создает `ManifestEmbeddedFileProvider` и передает в соответствующий конструктор текущую выполняемую сборку.</span><span class="sxs-lookup"><span data-stu-id="21ceb-245">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="21ceb-246">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="21ceb-246">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(typeof(Program).Assembly);
```

<span data-ttu-id="21ceb-247">Дополнительные перегрузки позволяют сделать следующее:</span><span class="sxs-lookup"><span data-stu-id="21ceb-247">Additional overloads allow you to:</span></span>

* <span data-ttu-id="21ceb-248">указать относительный путь к файлу;</span><span class="sxs-lookup"><span data-stu-id="21ceb-248">Specify a relative file path.</span></span>
* <span data-ttu-id="21ceb-249">ограничить файлы по дате последнего изменения;</span><span class="sxs-lookup"><span data-stu-id="21ceb-249">Scope files to a last modified date.</span></span>
* <span data-ttu-id="21ceb-250">указать имя внедренного ресурса, содержащего внедренный файл манифеста.</span><span class="sxs-lookup"><span data-stu-id="21ceb-250">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="21ceb-251">Перегрузка</span><span class="sxs-lookup"><span data-stu-id="21ceb-251">Overload</span></span> | <span data-ttu-id="21ceb-252">Описание</span><span class="sxs-lookup"><span data-stu-id="21ceb-252">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="21ceb-253">Принимает необязательный параметр `root` со значением относительного пути.</span><span class="sxs-lookup"><span data-stu-id="21ceb-253">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="21ceb-254">Укажите `root`, чтобы ограничить вызовы к <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> только ресурсами по указанному пути.</span><span class="sxs-lookup"><span data-stu-id="21ceb-254">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="21ceb-255">Принимает необязательный параметр `root` со значением относительного пути и параметр `lastModified` со значением даты (<xref:System.DateTimeOffset>).</span><span class="sxs-lookup"><span data-stu-id="21ceb-255">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="21ceb-256">Параметр `lastModified` ограничивает дату последнего изменения для экземпляров <xref:Microsoft.Extensions.FileProviders.IFileInfo>, возвращаемых функцией <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="21ceb-256">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="21ceb-257">Принимает необязательный параметр `root` со значением относительного пути, дату `lastModified` и параметры `manifestName`.</span><span class="sxs-lookup"><span data-stu-id="21ceb-257">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="21ceb-258">`manifestName` здесь представляет имя встроенного ресурса, содержащего манифест.</span><span class="sxs-lookup"><span data-stu-id="21ceb-258">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="21ceb-259">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="21ceb-259">CompositeFileProvider</span></span>

<span data-ttu-id="21ceb-260"><xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> объединяет экземпляры `IFileProvider`, предоставляя единый интерфейс для работы с файлами от нескольких поставщиков.</span><span class="sxs-lookup"><span data-stu-id="21ceb-260">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="21ceb-261">При создании `CompositeFileProvider` передайте в соответствующий конструктор один или несколько экземпляров `IFileProvider`:</span><span class="sxs-lookup"><span data-stu-id="21ceb-261">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="21ceb-262">В нашем примере приложения `PhysicalFileProvider` и `ManifestEmbeddedFileProvider` предоставляют файлы для `CompositeFileProvider` с регистрацией в контейнере служб приложения:</span><span class="sxs-lookup"><span data-stu-id="21ceb-262">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="21ceb-263">Отслеживание изменений</span><span class="sxs-lookup"><span data-stu-id="21ceb-263">Watch for changes</span></span>

<span data-ttu-id="21ceb-264">Метод [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) позволяет контролировать изменения в одном или нескольких файлах или каталогах.</span><span class="sxs-lookup"><span data-stu-id="21ceb-264">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="21ceb-265">`Watch` принимает строку пути, которая можно использовать [стандартные маски](#glob-patterns) для указания нескольких файлов.</span><span class="sxs-lookup"><span data-stu-id="21ceb-265">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="21ceb-266">`Watch` возвращает <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span><span class="sxs-lookup"><span data-stu-id="21ceb-266">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="21ceb-267">Этот токен изменений предоставляет следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="21ceb-267">The change token exposes:</span></span>

* <span data-ttu-id="21ceb-268"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; — свойство, которое можно проверить, чтобы определить, произошло ли изменение.</span><span class="sxs-lookup"><span data-stu-id="21ceb-268"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="21ceb-269"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; — вызывается при обнаружении изменений в указанной строке пути.</span><span class="sxs-lookup"><span data-stu-id="21ceb-269"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="21ceb-270">Каждый токен изменения выполняет соответствующий обратный вызов только в ответ на отдельное изменение.</span><span class="sxs-lookup"><span data-stu-id="21ceb-270">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="21ceb-271">Чтобы реализовать непрерывный мониторинг, используйте <xref:System.Threading.Tasks.TaskCompletionSource`1>, как показано ниже, или повторно создавайте экземпляры `IChangeToken` в ответ на изменения.</span><span class="sxs-lookup"><span data-stu-id="21ceb-271">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="21ceb-272">В нашем примере консольное приложение *WatchConsole* будет отображать сообщение при изменении текстового файла:</span><span class="sxs-lookup"><span data-stu-id="21ceb-272">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="21ceb-273">Некоторые файловые системы, такие как контейнеры Docker и сетевые папки, не могут надежно отправлять уведомления об изменениях.</span><span class="sxs-lookup"><span data-stu-id="21ceb-273">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="21ceb-274">Задайте для переменной среды `DOTNET_USE_POLLING_FILE_WATCHER` значение `1` или `true`, чтобы опрашивать файловую систему на предмет изменений каждые 4 секунды (это значение нельзя изменить).</span><span class="sxs-lookup"><span data-stu-id="21ceb-274">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="21ceb-275">Стандартные маски</span><span class="sxs-lookup"><span data-stu-id="21ceb-275">Glob patterns</span></span>

<span data-ttu-id="21ceb-276">В путях файловой системы используются шаблоны подстановочных знаков, которые называются *стандартными масками*.</span><span class="sxs-lookup"><span data-stu-id="21ceb-276">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="21ceb-277">Эти маски позволяют указывать группы файлов.</span><span class="sxs-lookup"><span data-stu-id="21ceb-277">Specify groups of files with these patterns.</span></span> <span data-ttu-id="21ceb-278">Поддерживаются два подстановочных знака — `*` и `**`.</span><span class="sxs-lookup"><span data-stu-id="21ceb-278">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="21ceb-279">Совпадает с любым элементом на текущем уровне папок, любым именем или расширением файла.</span><span class="sxs-lookup"><span data-stu-id="21ceb-279">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="21ceb-280">Совпадения завершаются символами `/` и `.` в пути файла.</span><span class="sxs-lookup"><span data-stu-id="21ceb-280">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="21ceb-281">Совпадает со всем содержимым на разных уровнях каталогов.</span><span class="sxs-lookup"><span data-stu-id="21ceb-281">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="21ceb-282">Может использоваться для рекурсивного сопоставления множества файлов в иерархии каталогов.</span><span class="sxs-lookup"><span data-stu-id="21ceb-282">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="21ceb-283">**Примеры стандартных масок**</span><span class="sxs-lookup"><span data-stu-id="21ceb-283">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="21ceb-284">Соответствует конкретному файлу в заданном каталоге.</span><span class="sxs-lookup"><span data-stu-id="21ceb-284">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="21ceb-285">Соответствует всем файлам с расширением *.txt* в заданном каталоге.</span><span class="sxs-lookup"><span data-stu-id="21ceb-285">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="21ceb-286">Соответствует всем файлам `appsettings.json` в любом каталоге, расположенном ровно на один уровень ниже каталога *directory*.</span><span class="sxs-lookup"><span data-stu-id="21ceb-286">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="21ceb-287">Соответствует всем файлам с расширением *.txt*, находящимся на любом уровне ниже каталога *directory*.</span><span class="sxs-lookup"><span data-stu-id="21ceb-287">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end
