---
title: "Файл поставщиков в ASP.NET Core"
author: ardalis
description: "Узнайте, как ASP.NET Core абстрагирует доступ к файловой системе при помощи поставщиков файла."
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/file-providers
ms.openlocfilehash: 10f3276d3e71e8a29b452d4c62865cbb82298513
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="faf0f-103">Файл поставщиков в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="faf0f-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="faf0f-104">По [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="faf0f-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="faf0f-105">ASP.NET Core абстрагирует доступ к файловой системе при помощи поставщиков файла.</span><span class="sxs-lookup"><span data-stu-id="faf0f-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span>

<span data-ttu-id="faf0f-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="faf0f-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-abstractions"></a><span data-ttu-id="faf0f-107">Абстрактные классы поставщика файла</span><span class="sxs-lookup"><span data-stu-id="faf0f-107">File Provider abstractions</span></span>

<span data-ttu-id="faf0f-108">Файл поставщики, это абстракция над файловых систем.</span><span class="sxs-lookup"><span data-stu-id="faf0f-108">File Providers are an abstraction over file systems.</span></span> <span data-ttu-id="faf0f-109">— Это основной интерфейс `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="faf0f-109">The main interface is `IFileProvider`.</span></span> <span data-ttu-id="faf0f-110">`IFileProvider`Предоставляет методы для получения сведений о файле (`IFileInfo`), сведения о каталоге (`IDirectoryContents`) и для настройки уведомлений об изменениях (с помощью `IChangeToken`).</span><span class="sxs-lookup"><span data-stu-id="faf0f-110">`IFileProvider` exposes methods to get file information (`IFileInfo`), directory information (`IDirectoryContents`), and to set up change notifications (using an `IChangeToken`).</span></span>

<span data-ttu-id="faf0f-111">`IFileInfo`Предоставляет методы и свойства об отдельных файлов или каталогов.</span><span class="sxs-lookup"><span data-stu-id="faf0f-111">`IFileInfo` provides methods and properties about individual files or directories.</span></span> <span data-ttu-id="faf0f-112">Он имеет два свойства типа boolean, `Exists` и `IsDirectory`, а также свойства, описывающие его `Name`, `Length` (в байтах), и `LastModified` даты.</span><span class="sxs-lookup"><span data-stu-id="faf0f-112">It has two boolean properties, `Exists` and `IsDirectory`, as well as properties describing the file's `Name`, `Length` (in bytes), and `LastModified` date.</span></span> <span data-ttu-id="faf0f-113">Можно считывать из файла с помощью его `CreateReadStream` метод.</span><span class="sxs-lookup"><span data-stu-id="faf0f-113">You can read from the file using its `CreateReadStream` method.</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="faf0f-114">Файл реализации поставщика</span><span class="sxs-lookup"><span data-stu-id="faf0f-114">File Provider implementations</span></span>

<span data-ttu-id="faf0f-115">Три способа реализации `IFileProvider` доступны: физических, внедренных и составными.</span><span class="sxs-lookup"><span data-stu-id="faf0f-115">Three implementations of `IFileProvider` are available: Physical, Embedded, and Composite.</span></span> <span data-ttu-id="faf0f-116">Физический поставщика используется для доступа к файлам текущий системный.</span><span class="sxs-lookup"><span data-stu-id="faf0f-116">The physical provider is used to access the actual system's files.</span></span> <span data-ttu-id="faf0f-117">Внедренные поставщика используется для доступа к файлам, внедренных в сборки.</span><span class="sxs-lookup"><span data-stu-id="faf0f-117">The embedded provider is used to access files embedded in assemblies.</span></span> <span data-ttu-id="faf0f-118">Составной поставщика используется для предоставления объединенный доступа к файлам и каталогам из одного или нескольких поставщиков.</span><span class="sxs-lookup"><span data-stu-id="faf0f-118">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span>

### <a name="physicalfileprovider"></a><span data-ttu-id="faf0f-119">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="faf0f-119">PhysicalFileProvider</span></span>

<span data-ttu-id="faf0f-120">`PhysicalFileProvider` Предоставляет доступ к физической файловой системы.</span><span class="sxs-lookup"><span data-stu-id="faf0f-120">The `PhysicalFileProvider` provides access to the physical file system.</span></span> <span data-ttu-id="faf0f-121">Он инкапсулирует `System.IO.File` типа (для физического поставщика), определение области все пути к каталогу и его дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="faf0f-121">It wraps the `System.IO.File` type (for the physical provider), scoping all paths to a directory and its children.</span></span> <span data-ttu-id="faf0f-122">Эту область ограничивает доступ к определенной папке и ее дочерних элементов, ограничивая его доступ к файловой системе вне этой границы.</span><span class="sxs-lookup"><span data-stu-id="faf0f-122">This scoping limits access to a certain directory and its children, preventing access to the file system outside of this boundary.</span></span> <span data-ttu-id="faf0f-123">При создании экземпляра этого поставщика, вам необходимо указать путь к каталогу, который выступает в качестве базовый путь для всех запросов, внесенные в этот поставщик (который ограничивает доступ за пределами этого пути).</span><span class="sxs-lookup"><span data-stu-id="faf0f-123">When instantiating this provider, you must provide it with a directory path, which serves as the base path for all requests made to this provider (and which restricts access outside of this path).</span></span> <span data-ttu-id="faf0f-124">В приложении ASP.NET Core, можно создать экземпляр `PhysicalFileProvider` поставщика непосредственно, или можно запросить `IFileProvider` в контроллере или конструктор службы через [внедрения зависимостей](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="faf0f-124">In an ASP.NET Core app, you can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a Controller or service's constructor through [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="faf0f-125">Второй подход обычно даст более гибкими и тестирования решения.</span><span class="sxs-lookup"><span data-stu-id="faf0f-125">The latter approach will typically yield a more flexible and testable solution.</span></span>

<span data-ttu-id="faf0f-126">В следующем примере показано, как создать `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="faf0f-126">The sample below shows how to create a `PhysicalFileProvider`.</span></span>


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

<span data-ttu-id="faf0f-127">Можно прохода его содержимое каталога или получить сведения о конкретном файле, предоставляя вложенным.</span><span class="sxs-lookup"><span data-stu-id="faf0f-127">You can iterate through its directory contents or get a specific file's information by providing a subpath.</span></span>

<span data-ttu-id="faf0f-128">Чтобы запросить поставщика из контроллера, укажите его в конструкторе контроллера и назначьте его локальное поле.</span><span class="sxs-lookup"><span data-stu-id="faf0f-128">To request a provider from a controller, specify it in the controller's constructor and assign it to a local field.</span></span> <span data-ttu-id="faf0f-129">Использование локального экземпляра в методах действий.</span><span class="sxs-lookup"><span data-stu-id="faf0f-129">Use the local instance from your action methods:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

<span data-ttu-id="faf0f-130">Затем создайте поставщика в приложении `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="faf0f-130">Then, create the provider in the app's `Startup` class:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

<span data-ttu-id="faf0f-131">В *Index.cshtml* Просмотр, итерации `IDirectoryContents` указано:</span><span class="sxs-lookup"><span data-stu-id="faf0f-131">In the *Index.cshtml* view, iterate through the `IDirectoryContents` provided:</span></span>

[!code-html[Main](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

<span data-ttu-id="faf0f-132">Результат:</span><span class="sxs-lookup"><span data-stu-id="faf0f-132">The result:</span></span>

![Файл поставщика образец приложения список физических файлов и папок](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a><span data-ttu-id="faf0f-134">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="faf0f-134">EmbeddedFileProvider</span></span>

<span data-ttu-id="faf0f-135">`EmbeddedFileProvider` Используется для доступа к файлам, внедренных в сборки.</span><span class="sxs-lookup"><span data-stu-id="faf0f-135">The `EmbeddedFileProvider` is used to access files embedded in assemblies.</span></span> <span data-ttu-id="faf0f-136">В .NET Core внедрить файлы в сборке с `<EmbeddedResource>` элемент в *.csproj* файла:</span><span class="sxs-lookup"><span data-stu-id="faf0f-136">In .NET Core, you embed files in an assembly with the `<EmbeddedResource>` element in the *.csproj* file:</span></span>

[!code-json[Main](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

<span data-ttu-id="faf0f-137">Можно использовать [шаблоны глобализацию](#globbing-patterns) при указании файлов для внедрения в сборку.</span><span class="sxs-lookup"><span data-stu-id="faf0f-137">You can use [globbing patterns](#globbing-patterns) when specifying files to embed in the assembly.</span></span> <span data-ttu-id="faf0f-138">Эти шаблоны можно использовать для сопоставления одного или нескольких файлов.</span><span class="sxs-lookup"><span data-stu-id="faf0f-138">These patterns can be used to match one or more files.</span></span>

> [!NOTE]
> <span data-ttu-id="faf0f-139">Маловероятно, что когда-либо требуется фактически внедрения каждого JS-файла в проект в сборку; приведенном выше примере — только в целях демонстрации.</span><span class="sxs-lookup"><span data-stu-id="faf0f-139">It's unlikely you would ever want to actually embed every .js file in your project in its assembly; the above sample is for demo purposes only.</span></span>

<span data-ttu-id="faf0f-140">При создании `EmbeddedFileProvider`, передать сборку, он будет считать его конструктору.</span><span class="sxs-lookup"><span data-stu-id="faf0f-140">When creating an `EmbeddedFileProvider`, pass the assembly it will read to its constructor.</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="faf0f-141">Приведенном выше фрагменте показано создание `EmbeddedFileProvider` с доступом к выполняющейся сборки.</span><span class="sxs-lookup"><span data-stu-id="faf0f-141">The snippet above demonstrates how to create an `EmbeddedFileProvider` with access to the currently executing assembly.</span></span>

<span data-ttu-id="faf0f-142">Обновление примера приложения для использования `EmbeddedFileProvider` приводит к следующие данные:</span><span class="sxs-lookup"><span data-stu-id="faf0f-142">Updating the sample app to use an `EmbeddedFileProvider` results in the following output:</span></span>

![Файл поставщика образец приложения внедренные файлы-перечень](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> <span data-ttu-id="faf0f-144">Внедренные ресурсы не предоставляют каталоги.</span><span class="sxs-lookup"><span data-stu-id="faf0f-144">Embedded resources don't expose directories.</span></span> <span data-ttu-id="faf0f-145">Вместо этого путь к ресурсу (через пространство имен) внедряется в ее имени файла с помощью `.` разделители.</span><span class="sxs-lookup"><span data-stu-id="faf0f-145">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span>

> [!TIP]
> <span data-ttu-id="faf0f-146">`EmbeddedFileProvider` Конструктор принимает необязательный `baseNamespace` параметра.</span><span class="sxs-lookup"><span data-stu-id="faf0f-146">The `EmbeddedFileProvider` constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="faf0f-147">Задание этого будет действовать вызовы `GetDirectoryContents` к этим ресурсам в указанных имен.</span><span class="sxs-lookup"><span data-stu-id="faf0f-147">Specifying this will scope calls to `GetDirectoryContents` to those resources under the provided namespace.</span></span>

### <a name="compositefileprovider"></a><span data-ttu-id="faf0f-148">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="faf0f-148">CompositeFileProvider</span></span>

<span data-ttu-id="faf0f-149">`CompositeFileProvider` Объединяет `IFileProvider` экземпляров, предоставление доступа к единый интерфейс для работы с файлами из нескольких поставщиков.</span><span class="sxs-lookup"><span data-stu-id="faf0f-149">The `CompositeFileProvider` combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="faf0f-150">При создании `CompositeFileProvider`, передать один или несколько `IFileProvider` экземпляров в его конструктор:</span><span class="sxs-lookup"><span data-stu-id="faf0f-150">When creating the `CompositeFileProvider`, you pass one or more `IFileProvider` instances to its constructor:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

<span data-ttu-id="faf0f-151">Обновление примера приложения для использования `CompositeFileProvider` , которые входят обоих физических и внедренные поставщики настроены ранее, результаты в следующий результат:</span><span class="sxs-lookup"><span data-stu-id="faf0f-151">Updating the sample app to use a `CompositeFileProvider` that includes both the physical and embedded providers configured previously, results in the following output:</span></span>

![Файл поставщика образец приложения перечисление физических файлов и папок и внедренными файлами](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a><span data-ttu-id="faf0f-153">Контроль изменений</span><span class="sxs-lookup"><span data-stu-id="faf0f-153">Watching for changes</span></span>

<span data-ttu-id="faf0f-154">`IFileProvider` `Watch` Метод предоставляет способ для отслеживания файлов или каталогов для изменения.</span><span class="sxs-lookup"><span data-stu-id="faf0f-154">The `IFileProvider` `Watch` method provides a way to watch one or more files or directories for changes.</span></span> <span data-ttu-id="faf0f-155">Этот метод принимает строку пути, который может использовать [глобализацию шаблоны](#globbing-patterns) для указания нескольких файлов и возвращает `IChangeToken`.</span><span class="sxs-lookup"><span data-stu-id="faf0f-155">This method accepts a path string, which can use [globbing patterns](#globbing-patterns) to specify multiple files, and returns an `IChangeToken`.</span></span> <span data-ttu-id="faf0f-156">Предоставляет этот токен `HasChanged` свойство, которое может быть проверен, и `RegisterChangeCallback` метод, вызываемый при обнаружении изменений для указанной строки пути.</span><span class="sxs-lookup"><span data-stu-id="faf0f-156">This token exposes a `HasChanged` property that can be inspected, and a `RegisterChangeCallback` method that's called when changes are detected to the specified path string.</span></span> <span data-ttu-id="faf0f-157">Обратите внимание, что каждый маркер изменений вызывает только его связанного обратного вызова в ответ на одно изменение.</span><span class="sxs-lookup"><span data-stu-id="faf0f-157">Note that each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="faf0f-158">Для постоянного наблюдения, можно использовать `TaskCompletionSource` как показано ниже, или повторно создайте `IChangeToken` экземпляров в ответ на изменения.</span><span class="sxs-lookup"><span data-stu-id="faf0f-158">To enable constant monitoring, you can use a `TaskCompletionSource` as shown below, or re-create `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="faf0f-159">В образце в этой статье консольное приложение настраивается для отображения сообщения при изменении текстового файла:</span><span class="sxs-lookup"><span data-stu-id="faf0f-159">In this article's sample, a console application is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[Main](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="faf0f-160">Результат после сохранения файла несколько раз:</span><span class="sxs-lookup"><span data-stu-id="faf0f-160">The result, after saving the file several times:</span></span>

![Окно командной строки после выполнения запуска dotnet показано приложение, мониторинг quotes.txt файл для изменения и измененного файла пять раз.](file-providers/_static/watch-console.png)

> [!NOTE]
> <span data-ttu-id="faf0f-162">Некоторые файловые системы, такие как контейнеры Docker и общих сетевых ресурсов не могут надежно отправлять уведомления об изменениях.</span><span class="sxs-lookup"><span data-stu-id="faf0f-162">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="faf0f-163">Задать `DOTNET_USE_POLLINGFILEWATCHER` переменную среды, чтобы `1` или `true` опроса файловой системы для изменения каждые 4 секунды.</span><span class="sxs-lookup"><span data-stu-id="faf0f-163">Set the `DOTNET_USE_POLLINGFILEWATCHER` environment variable to `1` or `true` to poll the file system for changes every 4 seconds.</span></span>

## <a name="globbing-patterns"></a><span data-ttu-id="faf0f-164">Этот режим шаблонов</span><span class="sxs-lookup"><span data-stu-id="faf0f-164">Globbing patterns</span></span>

<span data-ttu-id="faf0f-165">Пути файловой системы используйте шаблоны из подстановочных знаков вызывается *глобализацию шаблоны*.</span><span class="sxs-lookup"><span data-stu-id="faf0f-165">File system paths use wildcard patterns called *globbing patterns*.</span></span> <span data-ttu-id="faf0f-166">Эти простые шаблоны можно использовать для указания группы файлов.</span><span class="sxs-lookup"><span data-stu-id="faf0f-166">These simple patterns can be used to specify groups of files.</span></span> <span data-ttu-id="faf0f-167">Два подстановочных знаков, `*` и `**`.</span><span class="sxs-lookup"><span data-stu-id="faf0f-167">The two wildcard characters are `*` and `**`.</span></span>

**`*`**

   <span data-ttu-id="faf0f-168">Совпадает со всем на текущем уровне папок или любое имя файла или любое расширение файла.</span><span class="sxs-lookup"><span data-stu-id="faf0f-168">Matches anything at the current folder level, or any filename, or any file extension.</span></span> <span data-ttu-id="faf0f-169">Прервано совпадений `/` и `.` символов в пути файла.</span><span class="sxs-lookup"><span data-stu-id="faf0f-169">Matches are terminated by `/` and `.` characters in the file path.</span></span>

<strong><code>**</code></strong>

   <span data-ttu-id="faf0f-170">Совпадает со всем по нескольким уровням каталога.</span><span class="sxs-lookup"><span data-stu-id="faf0f-170">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="faf0f-171">Может использоваться для рекурсивного соответствует много файлов в иерархии каталога.</span><span class="sxs-lookup"><span data-stu-id="faf0f-171">Can be used to recursively match many files within a directory hierarchy.</span></span>

### <a name="globbing-pattern-examples"></a><span data-ttu-id="faf0f-172">Примеры шаблонов глобализации</span><span class="sxs-lookup"><span data-stu-id="faf0f-172">Globbing pattern examples</span></span>

**`directory/file.txt`**

   <span data-ttu-id="faf0f-173">Соответствует конкретный файл в заданном каталоге.</span><span class="sxs-lookup"><span data-stu-id="faf0f-173">Matches a specific file in a specific directory.</span></span>

**<code>directory/*.txt</code>**

   <span data-ttu-id="faf0f-174">Обозначает все файлы с `.txt` расширения в заданном каталоге.</span><span class="sxs-lookup"><span data-stu-id="faf0f-174">Matches all files with `.txt` extension in a specific directory.</span></span>

**`directory/*/bower.json`**

   <span data-ttu-id="faf0f-175">Соответствует всем `bower.json` файлы в каталоги ровно один уровень ниже `directory` каталога.</span><span class="sxs-lookup"><span data-stu-id="faf0f-175">Matches all `bower.json` files in directories exactly one level below the `directory` directory.</span></span>

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   <span data-ttu-id="faf0f-176">Обозначает все файлы с `.txt` расширения вложенной в любом `directory` каталога.</span><span class="sxs-lookup"><span data-stu-id="faf0f-176">Matches all files with `.txt` extension found anywhere under the `directory` directory.</span></span>

## <a name="file-provider-usage-in-aspnet-core"></a><span data-ttu-id="faf0f-177">Использование поставщика файлов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="faf0f-177">File Provider usage in ASP.NET Core</span></span>

<span data-ttu-id="faf0f-178">Несколько частей ASP.NET Core использовать файл поставщиков.</span><span class="sxs-lookup"><span data-stu-id="faf0f-178">Several parts of ASP.NET Core utilize file providers.</span></span> <span data-ttu-id="faf0f-179">`IHostingEnvironment`предоставляет содержимое корневого приложения и корневого веб-каталога как `IFileProvider` типов.</span><span class="sxs-lookup"><span data-stu-id="faf0f-179">`IHostingEnvironment` exposes the app's content root and web root as `IFileProvider` types.</span></span> <span data-ttu-id="faf0f-180">По промежуточного слоя статических файлов использует файл поставщики для поиска статических файлов.</span><span class="sxs-lookup"><span data-stu-id="faf0f-180">The static files middleware uses file providers to locate static files.</span></span> <span data-ttu-id="faf0f-181">Razor усложняет использование `IFileProvider` при поиске представления.</span><span class="sxs-lookup"><span data-stu-id="faf0f-181">Razor makes heavy use of `IFileProvider` in locating views.</span></span> <span data-ttu-id="faf0f-182">DotNet публикация поставщиков файл использует функциональные возможности и шаблоны этот режим для указания того, какие файлы должны публиковаться.</span><span class="sxs-lookup"><span data-stu-id="faf0f-182">Dotnet's publish functionality uses file providers and globbing patterns to specify which files should be published.</span></span>

## <a name="recommendations-for-use-in-apps"></a><span data-ttu-id="faf0f-183">Рекомендации для использования в приложениях</span><span class="sxs-lookup"><span data-stu-id="faf0f-183">Recommendations for use in apps</span></span>

<span data-ttu-id="faf0f-184">Если приложение ASP.NET Core требуется доступ к файловой системе, вы можете запросить экземпляр `IFileProvider` через внедрения зависимостей и использовать его методы для осуществления доступа к, как показано в этом примере.</span><span class="sxs-lookup"><span data-stu-id="faf0f-184">If your ASP.NET Core app requires file system access, you can request an instance of `IFileProvider` through dependency injection, and then use its methods to perform the access, as shown in this sample.</span></span> <span data-ttu-id="faf0f-185">Это дает возможность настроить поставщик один раз, когда приложение запускается и уменьшает количество типов реализации создает приложение.</span><span class="sxs-lookup"><span data-stu-id="faf0f-185">This allows you to configure the provider once, when the app starts up, and reduces the number of implementation types your app instantiates.</span></span>
