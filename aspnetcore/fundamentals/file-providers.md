---
title: Поставщики файлов в ASP.NET Core
author: guardrex
description: Сведения о том, как ASP.NET Core абстрагирует доступ к файловой системе с помощью поставщиков файлов.
ms.author: riande
ms.custom: mvc
ms.date: 08/01/2018
uid: fundamentals/file-providers
ms.openlocfilehash: a0d326f5fc995cb903380315879d39a8ce851d06
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913220"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="030bf-103">Поставщики файлов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="030bf-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="030bf-104">Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith) и [Люк Лэтем](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="030bf-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="030bf-105">ASP.NET Core абстрагирует доступ к файловой системе с помощью поставщиков файлов.</span><span class="sxs-lookup"><span data-stu-id="030bf-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="030bf-106">В рамках платформы ASP.NET Core используются поставщики файлов.</span><span class="sxs-lookup"><span data-stu-id="030bf-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="030bf-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) предоставляет корневой каталог содержимого приложения и корневой каталог веб-сайта в виде типов `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="030bf-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) exposes the app's content root and web root as `IFileProvider` types.</span></span>
* <span data-ttu-id="030bf-108">[Static Files Middleware](xref:fundamentals/static-files) использует поставщики файлов для поиска статических файлов.</span><span class="sxs-lookup"><span data-stu-id="030bf-108">[Static Files Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="030bf-109">[Razor](xref:mvc/views/razor) использует поставщики файлов для поиска страниц и представлений.</span><span class="sxs-lookup"><span data-stu-id="030bf-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="030bf-110">Инструменты .NET Core используют поставщики файлов и стандартные маски, чтобы указать файлы для публикации.</span><span class="sxs-lookup"><span data-stu-id="030bf-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="030bf-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="030bf-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="030bf-112">Интерфейсы поставщика файлов</span><span class="sxs-lookup"><span data-stu-id="030bf-112">File Provider interfaces</span></span>

<span data-ttu-id="030bf-113">Основным интерфейсом является [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span><span class="sxs-lookup"><span data-stu-id="030bf-113">The primary interface is [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> <span data-ttu-id="030bf-114">`IFileProvider` предоставляет методы для следующих действий:</span><span class="sxs-lookup"><span data-stu-id="030bf-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="030bf-115">получение сведений о файле ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo));</span><span class="sxs-lookup"><span data-stu-id="030bf-115">Obtain file information ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span></span>
* <span data-ttu-id="030bf-116">получение сведений о каталоге ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents));</span><span class="sxs-lookup"><span data-stu-id="030bf-116">Obtain directory information ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span></span>
* <span data-ttu-id="030bf-117">настройка уведомлений об изменениях (с помощью [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken));</span><span class="sxs-lookup"><span data-stu-id="030bf-117">Set up change notifications (using an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span></span>

<span data-ttu-id="030bf-118">`IFileInfo` предоставляет методы и свойства для работы с файлами:</span><span class="sxs-lookup"><span data-stu-id="030bf-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* <span data-ttu-id="030bf-119">[Exists](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.exists) (существует);</span><span class="sxs-lookup"><span data-stu-id="030bf-119">[Exists](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.exists)</span></span>
* <span data-ttu-id="030bf-120">[IsDirectory](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.isdirectory) (является каталогом);</span><span class="sxs-lookup"><span data-stu-id="030bf-120">[IsDirectory](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.isdirectory)</span></span>
* [<span data-ttu-id="030bf-121">Name</span><span class="sxs-lookup"><span data-stu-id="030bf-121">Name</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.name)
* <span data-ttu-id="030bf-122">[Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (длина в байтах);</span><span class="sxs-lookup"><span data-stu-id="030bf-122">[Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (in bytes)</span></span>
* <span data-ttu-id="030bf-123">[LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) (дата последнего изменения).</span><span class="sxs-lookup"><span data-stu-id="030bf-123">[LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) date</span></span>

<span data-ttu-id="030bf-124">Данные из файла можно считывать с помощью метода [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream).</span><span class="sxs-lookup"><span data-stu-id="030bf-124">You can read from the file using the [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) method.</span></span>

<span data-ttu-id="030bf-125">Пример приложения демонстрирует, как настроить в `Startup.ConfigureServices` поставщике файлов, чтобы использовать его в приложении путем [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="030bf-125">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="030bf-126">Реализации поставщиков файлов</span><span class="sxs-lookup"><span data-stu-id="030bf-126">File Provider implementations</span></span>

<span data-ttu-id="030bf-127">Доступны три реализации `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="030bf-127">Three implementations of `IFileProvider` are available.</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="030bf-128">Реализация</span><span class="sxs-lookup"><span data-stu-id="030bf-128">Implementation</span></span> | <span data-ttu-id="030bf-129">Описание:</span><span class="sxs-lookup"><span data-stu-id="030bf-129">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="030bf-130">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="030bf-130">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="030bf-131">Физический поставщик используется для доступа к физическим файлам системы.</span><span class="sxs-lookup"><span data-stu-id="030bf-131">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="030bf-132">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="030bf-132">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="030bf-133">Поставщик внедренных манифестов используется для доступа к файлам, внедренным в сборки.</span><span class="sxs-lookup"><span data-stu-id="030bf-133">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="030bf-134">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="030bf-134">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="030bf-135">Составной поставщик используется для предоставления комбинированного доступа к файлам и каталогам из одного или нескольких поставщиков.</span><span class="sxs-lookup"><span data-stu-id="030bf-135">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="030bf-136">Реализация</span><span class="sxs-lookup"><span data-stu-id="030bf-136">Implementation</span></span> | <span data-ttu-id="030bf-137">Описание:</span><span class="sxs-lookup"><span data-stu-id="030bf-137">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="030bf-138">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="030bf-138">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="030bf-139">Физический поставщик используется для доступа к физическим файлам системы.</span><span class="sxs-lookup"><span data-stu-id="030bf-139">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="030bf-140">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="030bf-140">EmbeddedFileProvider</span></span>](#embeddedfileprovider) | <span data-ttu-id="030bf-141">Внедренный поставщик используется для доступа к файлам, внедренным в сборки.</span><span class="sxs-lookup"><span data-stu-id="030bf-141">The embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="030bf-142">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="030bf-142">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="030bf-143">Составной поставщик используется для предоставления комбинированного доступа к файлам и каталогам из одного или нескольких поставщиков.</span><span class="sxs-lookup"><span data-stu-id="030bf-143">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

::: moniker-end

### <a name="physicalfileprovider"></a><span data-ttu-id="030bf-144">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="030bf-144">PhysicalFileProvider</span></span>

<span data-ttu-id="030bf-145">[PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) предоставляет доступ к физической файловой системе.</span><span class="sxs-lookup"><span data-stu-id="030bf-145">The [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) provides access to the physical file system.</span></span> <span data-ttu-id="030bf-146">`PhysicalFileProvider` использует тип [System.IO.File](/dotnet/api/system.io.file) (для физического поставщика), устанавливая для всех путей область каталога и его дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="030bf-146">`PhysicalFileProvider` uses the [System.IO.File](/dotnet/api/system.io.file) type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="030bf-147">Такая привязка к области защищает от доступа к файловой системе за пределами указанного каталога и его дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="030bf-147">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="030bf-148">При создании экземпляра этого поставщика нужно указать путь к каталогу, который станет базовым путем для всех запросов, выполненных с помощью этого поставщика.</span><span class="sxs-lookup"><span data-stu-id="030bf-148">When instantiating this provider, a directory path is required and serves as the base path for all requests made using the provider.</span></span> <span data-ttu-id="030bf-149">Вы можете создать экземпляр поставщика `PhysicalFileProvider` напрямую или вызвать `IFileProvider` в конструкторе путем [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="030bf-149">You can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="030bf-150">**Статические типы**</span><span class="sxs-lookup"><span data-stu-id="030bf-150">**Static types**</span></span>

<span data-ttu-id="030bf-151">Следующий пример кода демонстрирует, как создать и использовать `PhysicalFileProvider` для получения содержимого каталога и сведений о файлах:</span><span class="sxs-lookup"><span data-stu-id="030bf-151">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="030bf-152">В предшествующем примере используются следующие типы:</span><span class="sxs-lookup"><span data-stu-id="030bf-152">Types in the preceding example:</span></span>

* <span data-ttu-id="030bf-153">`provider` является `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="030bf-153">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="030bf-154">`contents` является `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="030bf-154">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="030bf-155">`fileInfo` является `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="030bf-155">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="030bf-156">С помощью поставщика файлов вы можете выполнить итерацию по каталогу, указанному в параметре `applicationRoot`, или вызвать `GetFileInfo` для получения сведений о файлах.</span><span class="sxs-lookup"><span data-stu-id="030bf-156">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="030bf-157">Поставщик файлов не имеет доступ к каталогам за пределами `applicationRoot`.</span><span class="sxs-lookup"><span data-stu-id="030bf-157">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="030bf-158">Этот пример приложения создает поставщик для приложения в классе `Startup.ConfigureServices`, используя [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span><span class="sxs-lookup"><span data-stu-id="030bf-158">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

<span data-ttu-id="030bf-159">**Получение типов поставщика файлов путем внедрения зависимостей**</span><span class="sxs-lookup"><span data-stu-id="030bf-159">**Obtain File Provider types with dependency injection**</span></span>

<span data-ttu-id="030bf-160">Вы можете внедрить провайдер в конструктор любого класса и назначить его локальному полю.</span><span class="sxs-lookup"><span data-stu-id="030bf-160">Inject the provider into any class constructor and assign it to a local field.</span></span> <span data-ttu-id="030bf-161">Используйте это поле в методах класса для доступа к файлам.</span><span class="sxs-lookup"><span data-stu-id="030bf-161">Use the field throughout the class's methods to access files.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="030bf-162">В нашем примере приложения класс `IndexModel` получает экземпляр `IFileProvider` для извлечения содержимого из основного каталога приложения.</span><span class="sxs-lookup"><span data-stu-id="030bf-162">In the sample app, the `IndexModel` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="030bf-163">*Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="030bf-163">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="030bf-164">На этой странице выполняется итерация `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="030bf-164">The `IDirectoryContents` are iterated in the page.</span></span>

<span data-ttu-id="030bf-165">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="030bf-165">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="030bf-166">В нашем примере приложения класс `HomeController` получает экземпляр `IFileProvider` для извлечения содержимого из основного каталога приложения.</span><span class="sxs-lookup"><span data-stu-id="030bf-166">In the sample app, the `HomeController` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="030bf-167">*Controllers/HomeController.cs*:</span><span class="sxs-lookup"><span data-stu-id="030bf-167">*Controllers/HomeController.cs*:</span></span>

[!code-csharp[](file-providers/samples/1.x/FileProviderSample/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="030bf-168">В этом представлении выполняется итерация `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="030bf-168">The `IDirectoryContents` are iterated in the view.</span></span>

<span data-ttu-id="030bf-169">*Views/Home/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="030bf-169">*Views/Home/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/1.x/FileProviderSample/Views/Home/Index.cshtml?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="030bf-170">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="030bf-170">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="030bf-171">[ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) используется для доступа к файлам, внедренным в сборки.</span><span class="sxs-lookup"><span data-stu-id="030bf-171">The [ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="030bf-172">`ManifestEmbeddedFileProvider` использует манифест, скомпилированный в сборку, для воссоздания исходных путей для внедренных файлов.</span><span class="sxs-lookup"><span data-stu-id="030bf-172">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

> [!NOTE]
> <span data-ttu-id="030bf-173">`ManifestEmbeddedFileProvider` доступно в ASP.NET Core 2.1 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="030bf-173">The `ManifestEmbeddedFileProvider` is available in ASP.NET Core 2.1 or later.</span></span> <span data-ttu-id="030bf-174">Чтобы получить доступ к файлам, внедренным в сборки, из ASP.NET Core 2.0 или более ранних версий см. в [версии этой статьи для ASP.NET Core 1.x](/aspnet/core/fundamentals/file-providers?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="030bf-174">To access files embedded in assemblies in ASP.NET Core 2.0 or earlier, see the [ASP.NET Core 1.x version of this topic](/aspnet/core/fundamentals/file-providers?view=aspnetcore-1.1).</span></span>

<span data-ttu-id="030bf-175">Чтобы создать манифест для внедренных файлов, задайте для свойства `<GenerateEmbeddedFilesManifest>` значение `true`.</span><span class="sxs-lookup"><span data-stu-id="030bf-175">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="030bf-176">Выберите файлы для внедрения с помощью [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span><span class="sxs-lookup"><span data-stu-id="030bf-176">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

<span data-ttu-id="030bf-177">Используйте [стандартные маски](#glob-patterns) для указания одного или нескольких файлов, которые вы хотите внедрить в сборку.</span><span class="sxs-lookup"><span data-stu-id="030bf-177">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="030bf-178">Наш пример приложения создает `ManifestEmbeddedFileProvider` и передает в соответствующий конструктор текущую выполняемую сборку.</span><span class="sxs-lookup"><span data-stu-id="030bf-178">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="030bf-179">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="030bf-179">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="030bf-180">Дополнительные перегрузки позволяют сделать следующее:</span><span class="sxs-lookup"><span data-stu-id="030bf-180">Additional overloads allow you to:</span></span>

* <span data-ttu-id="030bf-181">указать относительный путь к файлу;</span><span class="sxs-lookup"><span data-stu-id="030bf-181">Specify a relative file path.</span></span>
* <span data-ttu-id="030bf-182">ограничить файлы по дате последнего изменения;</span><span class="sxs-lookup"><span data-stu-id="030bf-182">Scope files to a last modified date.</span></span>
* <span data-ttu-id="030bf-183">указать имя внедренного ресурса, содержащего внедренный файл манифеста.</span><span class="sxs-lookup"><span data-stu-id="030bf-183">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="030bf-184">Перегрузка</span><span class="sxs-lookup"><span data-stu-id="030bf-184">Overload</span></span> | <span data-ttu-id="030bf-185">Описание:</span><span class="sxs-lookup"><span data-stu-id="030bf-185">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="030bf-186">ManifestEmbeddedFileProvider(Assembly, String)</span><span class="sxs-lookup"><span data-stu-id="030bf-186">ManifestEmbeddedFileProvider(Assembly, String)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_) | <span data-ttu-id="030bf-187">Принимает необязательный параметр `root` со значением относительного пути.</span><span class="sxs-lookup"><span data-stu-id="030bf-187">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="030bf-188">Укажите `root`, чтобы ограничить вызовы [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) только ресурсами по указанному пути.</span><span class="sxs-lookup"><span data-stu-id="030bf-188">Specify the `root` to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided path.</span></span> |
| [<span data-ttu-id="030bf-189">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="030bf-189">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_DateTimeOffset_) | <span data-ttu-id="030bf-190">Принимает необязательный параметр `root` со значением относительного пути и дату `lastModified` в параметре [DateTimeOffset](/dotnet/api/system.datetimeoffset).</span><span class="sxs-lookup"><span data-stu-id="030bf-190">Accepts an optional `root` relative path parameter and a `lastModified` date ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) parameter.</span></span> <span data-ttu-id="030bf-191">Дата `lastModified` ограничивает дату последнего изменения для экземпляров [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo), возвращаемых функцией [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span><span class="sxs-lookup"><span data-stu-id="030bf-191">The `lastModified` date scopes the last modification date for the [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) instances returned by the [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> |
| [<span data-ttu-id="030bf-192">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="030bf-192">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_String_System_DateTimeOffset_) | <span data-ttu-id="030bf-193">Принимает необязательный параметр `root` со значением относительного пути, дату `lastModified` и параметры `manifestName`.</span><span class="sxs-lookup"><span data-stu-id="030bf-193">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="030bf-194">`manifestName` здесь представляет имя встроенного ресурса, содержащего манифест.</span><span class="sxs-lookup"><span data-stu-id="030bf-194">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

### <a name="embeddedfileprovider"></a><span data-ttu-id="030bf-195">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="030bf-195">EmbeddedFileProvider</span></span>

<span data-ttu-id="030bf-196">[EmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider) используется для доступа к файлам, внедренным в сборки.</span><span class="sxs-lookup"><span data-stu-id="030bf-196">The [EmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="030bf-197">Укажите файлы для внедрения с помощью свойства [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) в файле проекта:</span><span class="sxs-lookup"><span data-stu-id="030bf-197">Specify the files to embed with the [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) property in the project file:</span></span>

```xml
<ItemGroup>
  <EmbeddedResource Include="Resource.txt" />
</ItemGroup>
```

<span data-ttu-id="030bf-198">Используйте [стандартные маски](#glob-patterns) для указания одного или нескольких файлов, которые вы хотите внедрить в сборку.</span><span class="sxs-lookup"><span data-stu-id="030bf-198">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="030bf-199">Наш пример приложения создает `EmbeddedFileProvider` и передает в соответствующий конструктор текущую выполняемую сборку.</span><span class="sxs-lookup"><span data-stu-id="030bf-199">The sample app creates an `EmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="030bf-200">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="030bf-200">*Startup.cs*:</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="030bf-201">Внедренные ресурсы не предоставляют каталоги.</span><span class="sxs-lookup"><span data-stu-id="030bf-201">Embedded resources don't expose directories.</span></span> <span data-ttu-id="030bf-202">Вместо этого путь к ресурсу (через пространство имен) внедряется в имя файла с помощью разделителей `.`.</span><span class="sxs-lookup"><span data-stu-id="030bf-202">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span> <span data-ttu-id="030bf-203">В нашем примере приложения `baseNamespace` имеет значение `FileProviderSample.`.</span><span class="sxs-lookup"><span data-stu-id="030bf-203">In the sample app, the `baseNamespace` is `FileProviderSample.`.</span></span>

<span data-ttu-id="030bf-204">Конструктор [EmbeddedFileProvider(Assembly, String)](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_EmbeddedFileProvider__ctor_System_Reflection_Assembly_) принимает необязательный параметр `baseNamespace`.</span><span class="sxs-lookup"><span data-stu-id="030bf-204">The [EmbeddedFileProvider(Assembly, String)](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_EmbeddedFileProvider__ctor_System_Reflection_Assembly_) constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="030bf-205">Укажите базовое пространство имен, чтобы ограничить вызовы [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) только ресурсами в указанном пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="030bf-205">Specify the base namespace to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided namespace.</span></span>

::: moniker-end

### <a name="compositefileprovider"></a><span data-ttu-id="030bf-206">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="030bf-206">CompositeFileProvider</span></span>

<span data-ttu-id="030bf-207">[CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) объединяет экземпляры `IFileProvider`, предоставляя единый интерфейс для работы с файлами от нескольких поставщиков.</span><span class="sxs-lookup"><span data-stu-id="030bf-207">The [CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="030bf-208">При создании `CompositeFileProvider` передайте в соответствующий конструктор один или несколько экземпляров `IFileProvider`:</span><span class="sxs-lookup"><span data-stu-id="030bf-208">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="030bf-209">В нашем примере приложения `PhysicalFileProvider` и `ManifestEmbeddedFileProvider` предоставляют файлы для `CompositeFileProvider` с регистрацией в контейнере служб приложения:</span><span class="sxs-lookup"><span data-stu-id="030bf-209">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="030bf-210">В нашем примере приложения `PhysicalFileProvider` и `EmbeddedFileProvider` предоставляют файлы для `CompositeFileProvider` с регистрацией в контейнере служб приложения:</span><span class="sxs-lookup"><span data-stu-id="030bf-210">In the sample app, a `PhysicalFileProvider` and an `EmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/1.x/FileProviderSample/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="watch-for-changes"></a><span data-ttu-id="030bf-211">Отслеживание изменений</span><span class="sxs-lookup"><span data-stu-id="030bf-211">Watch for changes</span></span>

<span data-ttu-id="030bf-212">Метод [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) позволяет контролировать изменения в одном или нескольких файлах или каталогах.</span><span class="sxs-lookup"><span data-stu-id="030bf-212">The [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="030bf-213">`Watch` принимает строку пути, которая можно использовать [стандартные маски](#glob-patterns) для указания нескольких файлов.</span><span class="sxs-lookup"><span data-stu-id="030bf-213">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="030bf-214">`Watch` возвращает [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span><span class="sxs-lookup"><span data-stu-id="030bf-214">`Watch` returns an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span></span> <span data-ttu-id="030bf-215">Этот токен изменений предоставляет следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="030bf-215">The change token exposes:</span></span>

* <span data-ttu-id="030bf-216">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) — свойство, которое можно проверить, чтобы определить, произошло ли изменение.</span><span class="sxs-lookup"><span data-stu-id="030bf-216">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="030bf-217">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback) — вызывается при обнаружении изменений по указанной строке пути.</span><span class="sxs-lookup"><span data-stu-id="030bf-217">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="030bf-218">Каждый токен изменения выполняет соответствующий обратный вызов только в ответ на отдельное изменение.</span><span class="sxs-lookup"><span data-stu-id="030bf-218">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="030bf-219">Чтобы реализовать постоянное наблюдение, используйте [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1), как показано ниже, или повторно создавайте экземпляры `IChangeToken` в ответ на изменения.</span><span class="sxs-lookup"><span data-stu-id="030bf-219">To enable constant monitoring, use a [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="030bf-220">В нашем примере консольное приложение *WatchConsole* будет отображать сообщение при изменении текстового файла:</span><span class="sxs-lookup"><span data-stu-id="030bf-220">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](file-providers/samples/1.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

::: moniker-end

<span data-ttu-id="030bf-221">Некоторые файловые системы, такие как контейнеры Docker и сетевые папки, не могут надежно отправлять уведомления об изменениях.</span><span class="sxs-lookup"><span data-stu-id="030bf-221">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="030bf-222">Задайте для переменной среды `DOTNET_USE_POLLING_FILE_WATCHER` значение `1` или `true`, чтобы опрашивать файловую систему на предмет изменений каждые 4 секунды (это значение нельзя изменить).</span><span class="sxs-lookup"><span data-stu-id="030bf-222">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="030bf-223">Стандартные маски</span><span class="sxs-lookup"><span data-stu-id="030bf-223">Glob patterns</span></span>

<span data-ttu-id="030bf-224">В путях файловой системы используются шаблоны подстановочных знаков, которые называются *стандартными масками*.</span><span class="sxs-lookup"><span data-stu-id="030bf-224">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="030bf-225">Эти маски позволяют указывать группы файлов.</span><span class="sxs-lookup"><span data-stu-id="030bf-225">Specify groups of files with these patterns.</span></span> <span data-ttu-id="030bf-226">Поддерживаются два подстановочных знака — `*` и `**`.</span><span class="sxs-lookup"><span data-stu-id="030bf-226">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="030bf-227">Совпадает с любым элементом на текущем уровне папок, любым именем или расширением файла.</span><span class="sxs-lookup"><span data-stu-id="030bf-227">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="030bf-228">Совпадения завершаются символами `/` и `.` в пути файла.</span><span class="sxs-lookup"><span data-stu-id="030bf-228">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="030bf-229">Совпадает со всем содержимым на разных уровнях каталогов.</span><span class="sxs-lookup"><span data-stu-id="030bf-229">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="030bf-230">Может использоваться для рекурсивного сопоставления множества файлов в иерархии каталогов.</span><span class="sxs-lookup"><span data-stu-id="030bf-230">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="030bf-231">**Примеры стандартных масок**</span><span class="sxs-lookup"><span data-stu-id="030bf-231">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="030bf-232">Соответствует конкретному файлу в заданном каталоге.</span><span class="sxs-lookup"><span data-stu-id="030bf-232">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="030bf-233">Соответствует всем файлам с расширением *.txt* в заданном каталоге.</span><span class="sxs-lookup"><span data-stu-id="030bf-233">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="030bf-234">Соответствует всем файлам `appsettings.json` в любом каталоге, расположенном ровно на один уровень ниже каталога *directory*.</span><span class="sxs-lookup"><span data-stu-id="030bf-234">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="030bf-235">Соответствует всем файлам с расширением *.txt*, находящимся на любом уровне ниже каталога *directory*.</span><span class="sxs-lookup"><span data-stu-id="030bf-235">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>
