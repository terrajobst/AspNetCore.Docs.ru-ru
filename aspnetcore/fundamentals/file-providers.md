---
title: Поставщики файлов в ASP.NET Core
author: ardalis
description: Сведения о том, как ASP.NET Core абстрагирует доступ к файловой системе с помощью поставщиков файлов.
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/file-providers
ms.openlocfilehash: cdbffdadd9616fe941809d67dc2c0bbd52149561
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2018
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="a6e98-103">Поставщики файлов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a6e98-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="a6e98-104">Автор: [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="a6e98-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a6e98-105">ASP.NET Core абстрагирует доступ к файловой системе с помощью поставщиков файлов.</span><span class="sxs-lookup"><span data-stu-id="a6e98-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span>

<span data-ttu-id="a6e98-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a6e98-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-abstractions"></a><span data-ttu-id="a6e98-107">Абстракции поставщиков файлов</span><span class="sxs-lookup"><span data-stu-id="a6e98-107">File Provider abstractions</span></span>

<span data-ttu-id="a6e98-108">Поставщики файлов представляют собой абстракцию над файловыми системами.</span><span class="sxs-lookup"><span data-stu-id="a6e98-108">File Providers are an abstraction over file systems.</span></span> <span data-ttu-id="a6e98-109">Главным интерфейсом является `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="a6e98-109">The main interface is `IFileProvider`.</span></span> <span data-ttu-id="a6e98-110">`IFileProvider` предоставляет методы для получения сведений о файле (`IFileInfo`), о каталоге (`IDirectoryContents`) и для настройки уведомлений об изменениях (с помощью `IChangeToken`).</span><span class="sxs-lookup"><span data-stu-id="a6e98-110">`IFileProvider` exposes methods to get file information (`IFileInfo`), directory information (`IDirectoryContents`), and to set up change notifications (using an `IChangeToken`).</span></span>

<span data-ttu-id="a6e98-111">`IFileInfo` предоставляет методы и свойства для отдельных файлов или каталогов.</span><span class="sxs-lookup"><span data-stu-id="a6e98-111">`IFileInfo` provides methods and properties about individual files or directories.</span></span> <span data-ttu-id="a6e98-112">Он имеет два логических свойства `Exists` и `IsDirectory`, а также свойства, описывающие `Name`, `Length` (в байтах) и дату `LastModified` файла.</span><span class="sxs-lookup"><span data-stu-id="a6e98-112">It has two boolean properties, `Exists` and `IsDirectory`, as well as properties describing the file's `Name`, `Length` (in bytes), and `LastModified` date.</span></span> <span data-ttu-id="a6e98-113">Считывать данные из файла можно с помощью метода `CreateReadStream`.</span><span class="sxs-lookup"><span data-stu-id="a6e98-113">You can read from the file using its `CreateReadStream` method.</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="a6e98-114">Реализации поставщиков файлов</span><span class="sxs-lookup"><span data-stu-id="a6e98-114">File Provider implementations</span></span>

<span data-ttu-id="a6e98-115">Доступны три реализации `IFileProvider`: физическая, внедренная и составная.</span><span class="sxs-lookup"><span data-stu-id="a6e98-115">Three implementations of `IFileProvider` are available: Physical, Embedded, and Composite.</span></span> <span data-ttu-id="a6e98-116">Физический поставщик используется для доступа к фактическим файлам системы.</span><span class="sxs-lookup"><span data-stu-id="a6e98-116">The physical provider is used to access the actual system's files.</span></span> <span data-ttu-id="a6e98-117">Внедренный поставщик используется для доступа к файлам, внедренным в сборки.</span><span class="sxs-lookup"><span data-stu-id="a6e98-117">The embedded provider is used to access files embedded in assemblies.</span></span> <span data-ttu-id="a6e98-118">Составной поставщик используется для предоставления комбинированного доступа к файлам и каталогам из одного или нескольких поставщиков.</span><span class="sxs-lookup"><span data-stu-id="a6e98-118">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span>

### <a name="physicalfileprovider"></a><span data-ttu-id="a6e98-119">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="a6e98-119">PhysicalFileProvider</span></span>

<span data-ttu-id="a6e98-120">`PhysicalFileProvider` предоставляет доступ к физической файловой системе.</span><span class="sxs-lookup"><span data-stu-id="a6e98-120">The `PhysicalFileProvider` provides access to the physical file system.</span></span> <span data-ttu-id="a6e98-121">Он создает оболочку для типа `System.IO.File` (для физического поставщика), определяя областью всех путей каталог и его дочерние элементы.</span><span class="sxs-lookup"><span data-stu-id="a6e98-121">It wraps the `System.IO.File` type (for the physical provider), scoping all paths to a directory and its children.</span></span> <span data-ttu-id="a6e98-122">Эта область ограничивает доступ к определенному каталогу и его дочерним элементам, предотвращая доступ к файловой системе за своей границей.</span><span class="sxs-lookup"><span data-stu-id="a6e98-122">This scoping limits access to a certain directory and its children, preventing access to the file system outside of this boundary.</span></span> <span data-ttu-id="a6e98-123">При создании экземпляра этого поставщика нужно указать путь к каталогу, который выступает в качестве базового пути для всех запросов, выполняемых для этого поставщика (а также ограничивает доступ за своими пределами).</span><span class="sxs-lookup"><span data-stu-id="a6e98-123">When instantiating this provider, you must provide it with a directory path, which serves as the base path for all requests made to this provider (and which restricts access outside of this path).</span></span> <span data-ttu-id="a6e98-124">В приложении ASP.NET Core можно создать экземпляр поставщика `PhysicalFileProvider` напрямую или запросить `IFileProvider` в контроллере или конструкторе службы через [внедрение зависимостей](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="a6e98-124">In an ASP.NET Core app, you can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a Controller or service's constructor through [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="a6e98-125">Второй подход обычно дает более гибкое и пригодное для тестирования решение.</span><span class="sxs-lookup"><span data-stu-id="a6e98-125">The latter approach will typically yield a more flexible and testable solution.</span></span>

<span data-ttu-id="a6e98-126">Приведенный ниже пример показывает, как создать `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="a6e98-126">The sample below shows how to create a `PhysicalFileProvider`.</span></span>


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

<span data-ttu-id="a6e98-127">Вы можете выполнять итерации по содержимому каталога или получить сведения о конкретном файле, предоставив вложенный путь.</span><span class="sxs-lookup"><span data-stu-id="a6e98-127">You can iterate through its directory contents or get a specific file's information by providing a subpath.</span></span>

<span data-ttu-id="a6e98-128">Чтобы запросить поставщик из контроллера, укажите его в конструкторе контроллера и назначьте его локальному полю.</span><span class="sxs-lookup"><span data-stu-id="a6e98-128">To request a provider from a controller, specify it in the controller's constructor and assign it to a local field.</span></span> <span data-ttu-id="a6e98-129">Используйте локальный экземпляр из своих методов действий:</span><span class="sxs-lookup"><span data-stu-id="a6e98-129">Use the local instance from your action methods:</span></span>

[!code-csharp[](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

<span data-ttu-id="a6e98-130">После этого создайте поставщик в классе `Startup` приложения:</span><span class="sxs-lookup"><span data-stu-id="a6e98-130">Then, create the provider in the app's `Startup` class:</span></span>

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

<span data-ttu-id="a6e98-131">В представлении *Index.cshtml* выполните итерации по предоставленному `IDirectoryContents`:</span><span class="sxs-lookup"><span data-stu-id="a6e98-131">In the *Index.cshtml* view, iterate through the `IDirectoryContents` provided:</span></span>

[!code-html[](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

<span data-ttu-id="a6e98-132">Результат:</span><span class="sxs-lookup"><span data-stu-id="a6e98-132">The result:</span></span>

![Пример приложения поставщика файлов, перечисляющий физические файлы и папки](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a><span data-ttu-id="a6e98-134">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="a6e98-134">EmbeddedFileProvider</span></span>

<span data-ttu-id="a6e98-135">`EmbeddedFileProvider` используется для доступа к файлам, внедренным в сборки.</span><span class="sxs-lookup"><span data-stu-id="a6e98-135">The `EmbeddedFileProvider` is used to access files embedded in assemblies.</span></span> <span data-ttu-id="a6e98-136">В .NET Core для внедрения файлов в сборку используется элемент `<EmbeddedResource>` в файле *CSPROJ*:</span><span class="sxs-lookup"><span data-stu-id="a6e98-136">In .NET Core, you embed files in an assembly with the `<EmbeddedResource>` element in the *.csproj* file:</span></span>

[!code-json[](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

<span data-ttu-id="a6e98-137">Вы можете использовать [стандартные маски](#globbing-patterns) при указании файлов для внедрения в сборку.</span><span class="sxs-lookup"><span data-stu-id="a6e98-137">You can use [globbing patterns](#globbing-patterns) when specifying files to embed in the assembly.</span></span> <span data-ttu-id="a6e98-138">Эти шаблоны можно использовать для сопоставления одного или нескольких файлов.</span><span class="sxs-lookup"><span data-stu-id="a6e98-138">These patterns can be used to match one or more files.</span></span>

> [!NOTE]
> <span data-ttu-id="a6e98-139">Маловероятно, что вам когда-либо потребуется фактически внедрять каждый JS-файл в проект в сборке; приведенный выше пример служит лишь для демонстрации.</span><span class="sxs-lookup"><span data-stu-id="a6e98-139">It's unlikely you would ever want to actually embed every .js file in your project in its assembly; the above sample is for demo purposes only.</span></span>

<span data-ttu-id="a6e98-140">При создании `EmbeddedFileProvider` передайте сборку, которую он считает своему конструктору.</span><span class="sxs-lookup"><span data-stu-id="a6e98-140">When creating an `EmbeddedFileProvider`, pass the assembly it will read to its constructor.</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="a6e98-141">Приведенный выше фрагмент показывает создание `EmbeddedFileProvider` с доступом к выполняющейся сборке.</span><span class="sxs-lookup"><span data-stu-id="a6e98-141">The snippet above demonstrates how to create an `EmbeddedFileProvider` with access to the currently executing assembly.</span></span>

<span data-ttu-id="a6e98-142">Изменение примера приложения для использования `EmbeddedFileProvider` приводит к следующему результату:</span><span class="sxs-lookup"><span data-stu-id="a6e98-142">Updating the sample app to use an `EmbeddedFileProvider` results in the following output:</span></span>

![Пример приложения поставщика файлов, перечисляющий внедренные файлы](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> <span data-ttu-id="a6e98-144">Внедренные ресурсы не предоставляют каталоги.</span><span class="sxs-lookup"><span data-stu-id="a6e98-144">Embedded resources don't expose directories.</span></span> <span data-ttu-id="a6e98-145">Вместо этого путь к ресурсу (через пространство имен) внедряется в имя файла с помощью разделителей `.`.</span><span class="sxs-lookup"><span data-stu-id="a6e98-145">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span>

> [!TIP]
> <span data-ttu-id="a6e98-146">Конструктор `EmbeddedFileProvider` принимает необязательный параметр `baseNamespace`.</span><span class="sxs-lookup"><span data-stu-id="a6e98-146">The `EmbeddedFileProvider` constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="a6e98-147">Если задать его, область вызовов `GetDirectoryContents` будет ограничена ресурсами в указанном пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="a6e98-147">Specifying this will scope calls to `GetDirectoryContents` to those resources under the provided namespace.</span></span>

### <a name="compositefileprovider"></a><span data-ttu-id="a6e98-148">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="a6e98-148">CompositeFileProvider</span></span>

<span data-ttu-id="a6e98-149">`CompositeFileProvider` объединяет экземпляры `IFileProvider`, предоставляя единый интерфейс для работы с файлами от нескольких поставщиков.</span><span class="sxs-lookup"><span data-stu-id="a6e98-149">The `CompositeFileProvider` combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="a6e98-150">При создании `CompositeFileProvider` вы передаете один или несколько экземпляров `IFileProvider` в его конструктор:</span><span class="sxs-lookup"><span data-stu-id="a6e98-150">When creating the `CompositeFileProvider`, you pass one or more `IFileProvider` instances to its constructor:</span></span>

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

<span data-ttu-id="a6e98-151">Изменение примера приложения для использования `CompositeFileProvider`, который включает в себя настроенные ранее физический и внедренный поставщики, дает следующий результат:</span><span class="sxs-lookup"><span data-stu-id="a6e98-151">Updating the sample app to use a `CompositeFileProvider` that includes both the physical and embedded providers configured previously, results in the following output:</span></span>

![Пример приложения поставщика файлов, перечисляющий как физические файлы и папки, так и внедренные файлы](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a><span data-ttu-id="a6e98-153">Просмотр изменений</span><span class="sxs-lookup"><span data-stu-id="a6e98-153">Watching for changes</span></span>

<span data-ttu-id="a6e98-154">Метод `IFileProvider` `Watch` позволяет просматривать один или несколько файлов или каталогов на предмет изменений.</span><span class="sxs-lookup"><span data-stu-id="a6e98-154">The `IFileProvider` `Watch` method provides a way to watch one or more files or directories for changes.</span></span> <span data-ttu-id="a6e98-155">Этот метод принимает строку пути, которая может использовать [стандартные маски](#globbing-patterns) для указания нескольких файлов и возвращает `IChangeToken`.</span><span class="sxs-lookup"><span data-stu-id="a6e98-155">This method accepts a path string, which can use [globbing patterns](#globbing-patterns) to specify multiple files, and returns an `IChangeToken`.</span></span> <span data-ttu-id="a6e98-156">Этот токен предоставляет свойство `HasChanged`, которое можно проверить, и метод `RegisterChangeCallback`, вызываемый при обнаружении изменений для указанной строки пути.</span><span class="sxs-lookup"><span data-stu-id="a6e98-156">This token exposes a `HasChanged` property that can be inspected, and a `RegisterChangeCallback` method that's called when changes are detected to the specified path string.</span></span> <span data-ttu-id="a6e98-157">Обратите внимание, что каждый токен изменения выполняет соответствующий обратный вызов только в ответ на отдельное изменение.</span><span class="sxs-lookup"><span data-stu-id="a6e98-157">Note that each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="a6e98-158">Чтобы реализовать постоянное наблюдение, можно использовать `TaskCompletionSource`, как показано ниже, или повторно создавать экземпляры `IChangeToken` в ответ на изменения.</span><span class="sxs-lookup"><span data-stu-id="a6e98-158">To enable constant monitoring, you can use a `TaskCompletionSource` as shown below, or re-create `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="a6e98-159">В рассматриваемом здесь примере консольное приложение настраивается для отображения сообщения при изменении текстового файла:</span><span class="sxs-lookup"><span data-stu-id="a6e98-159">In this article's sample, a console application is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="a6e98-160">Результат после нескольких сохранений файла:</span><span class="sxs-lookup"><span data-stu-id="a6e98-160">The result, after saving the file several times:</span></span>

![После запуска dotnet командное окно показывает мониторинг приложения на предмет изменений в файле quotes.txt и то, что этот файл был изменен пять раз.](file-providers/_static/watch-console.png)

> [!NOTE]
> <span data-ttu-id="a6e98-162">Некоторые файловые системы, такие как контейнеры Docker и сетевые папки, не могут надежно отправлять уведомления об изменениях.</span><span class="sxs-lookup"><span data-stu-id="a6e98-162">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="a6e98-163">Задайте для переменной среды `DOTNET_USE_POLLINGFILEWATCHER` значение `1` или `true`, чтобы опрашивать файловую систему на предмет изменений каждые 4 секунды.</span><span class="sxs-lookup"><span data-stu-id="a6e98-163">Set the `DOTNET_USE_POLLINGFILEWATCHER` environment variable to `1` or `true` to poll the file system for changes every 4 seconds.</span></span>

## <a name="globbing-patterns"></a><span data-ttu-id="a6e98-164">Стандартные маски</span><span class="sxs-lookup"><span data-stu-id="a6e98-164">Globbing patterns</span></span>

<span data-ttu-id="a6e98-165">Пути файловой системы используют шаблоны подстановочных знаков, которые называются *стандартными масками*.</span><span class="sxs-lookup"><span data-stu-id="a6e98-165">File system paths use wildcard patterns called *globbing patterns*.</span></span> <span data-ttu-id="a6e98-166">Эти простые шаблоны можно использовать для указания групп файлов.</span><span class="sxs-lookup"><span data-stu-id="a6e98-166">These simple patterns can be used to specify groups of files.</span></span> <span data-ttu-id="a6e98-167">Поддерживаются два подстановочных знака — `*` и `**`.</span><span class="sxs-lookup"><span data-stu-id="a6e98-167">The two wildcard characters are `*` and `**`.</span></span>

**`*`**

   <span data-ttu-id="a6e98-168">Совпадает со всем содержимым на текущем уровне папок, любым именем или расширением файла.</span><span class="sxs-lookup"><span data-stu-id="a6e98-168">Matches anything at the current folder level, or any filename, or any file extension.</span></span> <span data-ttu-id="a6e98-169">Совпадения завершаются символами `/` и `.` в пути файла.</span><span class="sxs-lookup"><span data-stu-id="a6e98-169">Matches are terminated by `/` and `.` characters in the file path.</span></span>

<strong><code>**</code></strong>

   <span data-ttu-id="a6e98-170">Совпадает со всем содержимым на разных уровнях каталогов.</span><span class="sxs-lookup"><span data-stu-id="a6e98-170">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="a6e98-171">Может использоваться для рекурсивного сопоставления множества файлов в иерархии каталогов.</span><span class="sxs-lookup"><span data-stu-id="a6e98-171">Can be used to recursively match many files within a directory hierarchy.</span></span>

### <a name="globbing-pattern-examples"></a><span data-ttu-id="a6e98-172">Примеры стандартных масок</span><span class="sxs-lookup"><span data-stu-id="a6e98-172">Globbing pattern examples</span></span>

**`directory/file.txt`**

   <span data-ttu-id="a6e98-173">Соответствует конкретному файлу в заданном каталоге.</span><span class="sxs-lookup"><span data-stu-id="a6e98-173">Matches a specific file in a specific directory.</span></span>

**<code>directory/*.txt</code>**

   <span data-ttu-id="a6e98-174">Соответствует всем файлам с расширением `.txt` в заданном каталоге.</span><span class="sxs-lookup"><span data-stu-id="a6e98-174">Matches all files with `.txt` extension in a specific directory.</span></span>

**`directory/*/bower.json`**

   <span data-ttu-id="a6e98-175">Соответствует всем файлам `bower.json` в каталоге ровно на один уровень ниже каталога `directory`.</span><span class="sxs-lookup"><span data-stu-id="a6e98-175">Matches all `bower.json` files in directories exactly one level below the `directory` directory.</span></span>

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   <span data-ttu-id="a6e98-176">Соответствует всем файлам с расширением `.txt`, находящимся ниже каталога `directory`.</span><span class="sxs-lookup"><span data-stu-id="a6e98-176">Matches all files with `.txt` extension found anywhere under the `directory` directory.</span></span>

## <a name="file-provider-usage-in-aspnet-core"></a><span data-ttu-id="a6e98-177">Использование поставщиков файлов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a6e98-177">File Provider usage in ASP.NET Core</span></span>

<span data-ttu-id="a6e98-178">Несколько частей ASP.NET Core используют поставщики файлов.</span><span class="sxs-lookup"><span data-stu-id="a6e98-178">Several parts of ASP.NET Core utilize file providers.</span></span> <span data-ttu-id="a6e98-179">`IHostingEnvironment` предоставляет корень содержимого приложения и корневой каталог веб-сайта в виде типов `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="a6e98-179">`IHostingEnvironment` exposes the app's content root and web root as `IFileProvider` types.</span></span> <span data-ttu-id="a6e98-180">ПО промежуточного слоя для статических файлов использует поставщики файлов для поиска статических файлов.</span><span class="sxs-lookup"><span data-stu-id="a6e98-180">The static files middleware uses file providers to locate static files.</span></span> <span data-ttu-id="a6e98-181">Razor усложняет использование `IFileProvider` при поиске представлений.</span><span class="sxs-lookup"><span data-stu-id="a6e98-181">Razor makes heavy use of `IFileProvider` in locating views.</span></span> <span data-ttu-id="a6e98-182">Функции публикации DotNet используют поставщики файлов и стандартные маски, чтобы указать, какие файлы нужно публиковать.</span><span class="sxs-lookup"><span data-stu-id="a6e98-182">Dotnet's publish functionality uses file providers and globbing patterns to specify which files should be published.</span></span>

## <a name="recommendations-for-use-in-apps"></a><span data-ttu-id="a6e98-183">Рекомендации по использованию в приложениях</span><span class="sxs-lookup"><span data-stu-id="a6e98-183">Recommendations for use in apps</span></span>

<span data-ttu-id="a6e98-184">Если приложению ASP.NET Core требуется доступ к файловой системе, вы можете запросить экземпляр `IFileProvider` через внедрение зависимостей, а затем воспользоваться его методами для реализации доступа, как показано в этом примере.</span><span class="sxs-lookup"><span data-stu-id="a6e98-184">If your ASP.NET Core app requires file system access, you can request an instance of `IFileProvider` through dependency injection, and then use its methods to perform the access, as shown in this sample.</span></span> <span data-ttu-id="a6e98-185">Это позволяет настроить поставщик один раз при запуске приложения и сокращает число типов реализации, для которых приложение создает экземпляры.</span><span class="sxs-lookup"><span data-stu-id="a6e98-185">This allows you to configure the provider once, when the app starts up, and reduces the number of implementation types your app instantiates.</span></span>
