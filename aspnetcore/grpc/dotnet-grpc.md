---
title: Управление ссылками protobuf с помощью DotNet-GRPC
author: juntaoluo
description: Сведения о добавлении, обновлении, удалении и перечислении ссылок protobuf с помощью глобального инструмента DotNet-GRPC.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/24/2019
uid: grpc/dotnet-grpc
ms.openlocfilehash: ebd57419be24f7f4ed9765e36cf14189be8438b1
ms.sourcegitcommit: 020c3760492efed71b19e476f25392dda5dd7388
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/12/2019
ms.locfileid: "72290049"
---
# <a name="manage-protobuf-references-with-dotnet-grpc"></a><span data-ttu-id="21b6e-103">Управление ссылками protobuf с помощью DotNet-GRPC</span><span class="sxs-lookup"><span data-stu-id="21b6e-103">Manage Protobuf references with dotnet-grpc</span></span>

<span data-ttu-id="21b6e-104">Автор: [John Luo](https://github.com/juntaoluo) (Джон Луо)</span><span class="sxs-lookup"><span data-stu-id="21b6e-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="21b6e-105">`dotnet-grpc` — это глобальное средство .NET Core для управления ссылками protobuf в проекте .NET gRPC.</span><span class="sxs-lookup"><span data-stu-id="21b6e-105">`dotnet-grpc` is a .NET Core Global Tool for managing Protobuf references within a .NET gRPC project.</span></span> <span data-ttu-id="21b6e-106">Это средство можно использовать для добавления, обновления, удаления и перечисления ссылок protobuf.</span><span class="sxs-lookup"><span data-stu-id="21b6e-106">The tool can be used to add, refresh, remove, and list Protobuf references.</span></span>

## <a name="installation"></a><span data-ttu-id="21b6e-107">Установка</span><span class="sxs-lookup"><span data-stu-id="21b6e-107">Installation</span></span>

<span data-ttu-id="21b6e-108">Чтобы установить глобальное средство `dotnet-grpc` [.NET Core](/dotnet/core/tools/global-tools), выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="21b6e-108">To install the `dotnet-grpc` [.NET Core Global Tool](/dotnet/core/tools/global-tools), run the following command:</span></span>

```dotnetcli
dotnet tool install -g dotnet-grpc
```

## <a name="add-references"></a><span data-ttu-id="21b6e-109">Добавление ссылок</span><span class="sxs-lookup"><span data-stu-id="21b6e-109">Add references</span></span>

<span data-ttu-id="21b6e-110">`dotnet-grpc` можно использовать для добавления ссылок protobuf в виде `<Protobuf />` элементов в файл *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="21b6e-110">`dotnet-grpc` can be used to add Protobuf references as `<Protobuf />` items to the *.csproj* file:</span></span>

```xml
<Protobuf Include="..\Proto\count.proto" GrpcServices="Server" Link="Protos\count.proto" />
```

<span data-ttu-id="21b6e-111">Ссылки protobuf используются для создания ресурсов C# клиента и/или сервера.</span><span class="sxs-lookup"><span data-stu-id="21b6e-111">The Protobuf references are used to generate the C# client and/or server assets.</span></span> <span data-ttu-id="21b6e-112">@No__t 0tool может:</span><span class="sxs-lookup"><span data-stu-id="21b6e-112">The `dotnet-grpc`tool can:</span></span>

* <span data-ttu-id="21b6e-113">Создайте ссылку на protobuf из локальных файлов на диске.</span><span class="sxs-lookup"><span data-stu-id="21b6e-113">Create a Protobuf reference from local files on disk.</span></span>
* <span data-ttu-id="21b6e-114">Создание ссылки protobuf из удаленного файла, указанного с помощью URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="21b6e-114">Create a Protobuf reference from a remote file specified by a URL.</span></span>
* <span data-ttu-id="21b6e-115">Убедитесь, что в проект добавлены правильные зависимости пакета gRPC.</span><span class="sxs-lookup"><span data-stu-id="21b6e-115">Ensure the correct gRPC package dependencies are added to the project.</span></span>

<span data-ttu-id="21b6e-116">Например, пакет `Grpc.AspNetCore` добавляется в веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="21b6e-116">For example, the `Grpc.AspNetCore` package is added to a web app.</span></span> <span data-ttu-id="21b6e-117">`Grpc.AspNetCore` содержит серверные и клиентские библиотеки gRPC и поддержку инструментария.</span><span class="sxs-lookup"><span data-stu-id="21b6e-117">`Grpc.AspNetCore` contains gRPC server and client libraries and tooling support.</span></span> <span data-ttu-id="21b6e-118">Кроме того, в консольное приложение добавляются пакеты `Grpc.Net.Client`, `Grpc.Tools` и `Google.Protobuf`, которые содержат только клиентские библиотеки gRPC и средства поддержки средств.</span><span class="sxs-lookup"><span data-stu-id="21b6e-118">Alternatively, the `Grpc.Net.Client`, `Grpc.Tools` and `Google.Protobuf` packages, which contain only the gRPC client libraries and tooling support, are added to a Console app.</span></span>

### <a name="add-file"></a><span data-ttu-id="21b6e-119">Добавить файл</span><span class="sxs-lookup"><span data-stu-id="21b6e-119">Add file</span></span>

<span data-ttu-id="21b6e-120">Команда `add-file` используется для добавления локальных файлов на диске в качестве ссылок protobuf.</span><span class="sxs-lookup"><span data-stu-id="21b6e-120">The `add-file` command is used to add local files on disk as Protobuf references.</span></span> <span data-ttu-id="21b6e-121">Предоставленные пути к файлам:</span><span class="sxs-lookup"><span data-stu-id="21b6e-121">The file paths provided:</span></span>

* <span data-ttu-id="21b6e-122">Может быть относительным по отношению к текущему каталогу или абсолютным путям.</span><span class="sxs-lookup"><span data-stu-id="21b6e-122">Can be relative to the current directory or absolute paths.</span></span>
* <span data-ttu-id="21b6e-123">Может содержать подстановочные знаки для файла [глобализации](https://wikipedia.org/wiki/Glob_(programming))на основе шаблона.</span><span class="sxs-lookup"><span data-stu-id="21b6e-123">May contain wild cards for pattern-based file [globbing](https://wikipedia.org/wiki/Glob_(programming)).</span></span>

<span data-ttu-id="21b6e-124">Если какие-либо файлы находятся за пределами каталога проекта, добавляется элемент `Link` для отображения файла в папке `Protos` в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="21b6e-124">If any files are outside the project directory, a `Link` element is added to display the file under the folder `Protos` in Visual Studio.</span></span>

### <a name="usage"></a><span data-ttu-id="21b6e-125">Использование</span><span class="sxs-lookup"><span data-stu-id="21b6e-125">Usage</span></span>

```dotnetcli
dotnet grpc add-file [options] <files>...
```

#### <a name="arguments"></a><span data-ttu-id="21b6e-126">Аргументы</span><span class="sxs-lookup"><span data-stu-id="21b6e-126">Arguments</span></span>

| <span data-ttu-id="21b6e-127">Аргумент</span><span class="sxs-lookup"><span data-stu-id="21b6e-127">Argument</span></span> | <span data-ttu-id="21b6e-128">Описание</span><span class="sxs-lookup"><span data-stu-id="21b6e-128">Description</span></span> |
|-|-|
| <span data-ttu-id="21b6e-129">files</span><span class="sxs-lookup"><span data-stu-id="21b6e-129">files</span></span> | <span data-ttu-id="21b6e-130">Ссылка на файл protobuf.</span><span class="sxs-lookup"><span data-stu-id="21b6e-130">The protobuf file references.</span></span> <span data-ttu-id="21b6e-131">Это может быть путь к стандартная маска для локальных файлов protobuf.</span><span class="sxs-lookup"><span data-stu-id="21b6e-131">These can be a path to glob for local protobuf files.</span></span> |

#### <a name="options"></a><span data-ttu-id="21b6e-132">Параметры</span><span class="sxs-lookup"><span data-stu-id="21b6e-132">Options</span></span>

| <span data-ttu-id="21b6e-133">Короткий параметр</span><span class="sxs-lookup"><span data-stu-id="21b6e-133">Short option</span></span> | <span data-ttu-id="21b6e-134">Длинный параметр</span><span class="sxs-lookup"><span data-stu-id="21b6e-134">Long option</span></span> | <span data-ttu-id="21b6e-135">Описание</span><span class="sxs-lookup"><span data-stu-id="21b6e-135">Description</span></span> |
|-|-|-|
| <span data-ttu-id="21b6e-136">-p</span><span class="sxs-lookup"><span data-stu-id="21b6e-136">-p</span></span> | <span data-ttu-id="21b6e-137">--проект</span><span class="sxs-lookup"><span data-stu-id="21b6e-137">--project</span></span> | <span data-ttu-id="21b6e-138">Путь к файлу проекта для обработки.</span><span class="sxs-lookup"><span data-stu-id="21b6e-138">The path to the project file to operate on.</span></span> <span data-ttu-id="21b6e-139">Если файл не указан, команда ищет его в текущем каталоге.</span><span class="sxs-lookup"><span data-stu-id="21b6e-139">If a file is not specified, the command searches the current directory for one.</span></span>
| <span data-ttu-id="21b6e-140">-s</span><span class="sxs-lookup"><span data-stu-id="21b6e-140">-s</span></span> | <span data-ttu-id="21b6e-141">--службы</span><span class="sxs-lookup"><span data-stu-id="21b6e-141">--services</span></span> | <span data-ttu-id="21b6e-142">Тип gRPC служб, которые должны быть созданы.</span><span class="sxs-lookup"><span data-stu-id="21b6e-142">The type of gRPC services that should be generated.</span></span> <span data-ttu-id="21b6e-143">Если задано значение `Default`, для веб-проектов используется `Both`, а для проектов, не являющихся веб-проектами, используется `Client`.</span><span class="sxs-lookup"><span data-stu-id="21b6e-143">If `Default` is specified, `Both` is used for Web projects and `Client` is used for non-Web projects.</span></span> <span data-ttu-id="21b6e-144">Допустимые значения: `Both`, `Client`, `Default`, `None`, `Server`.</span><span class="sxs-lookup"><span data-stu-id="21b6e-144">Accepted values are `Both`, `Client`, `Default`, `None`, `Server`.</span></span>
| <span data-ttu-id="21b6e-145">-i</span><span class="sxs-lookup"><span data-stu-id="21b6e-145">-i</span></span> | <span data-ttu-id="21b6e-146">--reimports-dirs</span><span class="sxs-lookup"><span data-stu-id="21b6e-146">--additional-import-dirs</span></span> | <span data-ttu-id="21b6e-147">Дополнительные каталоги, которые будут использоваться при разрешении импортов для файлов protobuf.</span><span class="sxs-lookup"><span data-stu-id="21b6e-147">Additional directories to be used when resolving imports for the protobuf files.</span></span> <span data-ttu-id="21b6e-148">Это список путей, разделенных точкой с запятой.</span><span class="sxs-lookup"><span data-stu-id="21b6e-148">This is a semicolon separated list of paths.</span></span>
| | <span data-ttu-id="21b6e-149">--доступ</span><span class="sxs-lookup"><span data-stu-id="21b6e-149">--access</span></span> | <span data-ttu-id="21b6e-150">Модификатор доступа, используемый для создаваемых C# классов.</span><span class="sxs-lookup"><span data-stu-id="21b6e-150">The access modifier to use for the generated C# classes.</span></span> <span data-ttu-id="21b6e-151">Значение по умолчанию — `Public`.</span><span class="sxs-lookup"><span data-stu-id="21b6e-151">The default value is `Public`.</span></span> <span data-ttu-id="21b6e-152">Допустимые значения: `Internal` и `Public`.</span><span class="sxs-lookup"><span data-stu-id="21b6e-152">Accepted values are `Internal` and `Public`.</span></span>

### <a name="add-url"></a><span data-ttu-id="21b6e-153">Добавить URL-адрес</span><span class="sxs-lookup"><span data-stu-id="21b6e-153">Add URL</span></span>

<span data-ttu-id="21b6e-154">Команда `add-url` используется для добавления удаленного файла, заданного исходным URL-адресом, в качестве ссылки protobuf.</span><span class="sxs-lookup"><span data-stu-id="21b6e-154">The `add-url` command is used to add a remote file specified by an source URL as Protobuf reference.</span></span> <span data-ttu-id="21b6e-155">Для указания места загрузки удаленного файла необходимо указать путь к файлу.</span><span class="sxs-lookup"><span data-stu-id="21b6e-155">A file path must be provided to specify where to download the remote file.</span></span> <span data-ttu-id="21b6e-156">Путь к файлу может относиться к текущему каталогу или абсолютному пути.</span><span class="sxs-lookup"><span data-stu-id="21b6e-156">The file path can be relative to the current directory or an absolute path.</span></span> <span data-ttu-id="21b6e-157">Если путь к файлу находится за пределами каталога проекта, добавляется элемент `Link` для отображения файла в виртуальной папке `Protos` в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="21b6e-157">If the file path is outside the project directory, a `Link` element is added to display the file under the virtual folder `Protos` in Visual Studio.</span></span>

### <a name="usage"></a><span data-ttu-id="21b6e-158">Использование</span><span class="sxs-lookup"><span data-stu-id="21b6e-158">Usage</span></span>

```dotnetcli
dotnet-grpc add-url [options] <url>
```

#### <a name="arguments"></a><span data-ttu-id="21b6e-159">Аргументы</span><span class="sxs-lookup"><span data-stu-id="21b6e-159">Arguments</span></span>

| <span data-ttu-id="21b6e-160">Аргумент</span><span class="sxs-lookup"><span data-stu-id="21b6e-160">Argument</span></span> | <span data-ttu-id="21b6e-161">Описание</span><span class="sxs-lookup"><span data-stu-id="21b6e-161">Description</span></span> |
|-|-|
| <span data-ttu-id="21b6e-162">url</span><span class="sxs-lookup"><span data-stu-id="21b6e-162">url</span></span> | <span data-ttu-id="21b6e-163">URL-адрес удаленного файла protobuf.</span><span class="sxs-lookup"><span data-stu-id="21b6e-163">The URL to a remote protobuf file.</span></span> |

#### <a name="options"></a><span data-ttu-id="21b6e-164">Параметры</span><span class="sxs-lookup"><span data-stu-id="21b6e-164">Options</span></span>

| <span data-ttu-id="21b6e-165">Короткий параметр</span><span class="sxs-lookup"><span data-stu-id="21b6e-165">Short option</span></span> | <span data-ttu-id="21b6e-166">Длинный параметр</span><span class="sxs-lookup"><span data-stu-id="21b6e-166">Long option</span></span> | <span data-ttu-id="21b6e-167">Описание</span><span class="sxs-lookup"><span data-stu-id="21b6e-167">Description</span></span> |
|-|-|-|
| <span data-ttu-id="21b6e-168">-o</span><span class="sxs-lookup"><span data-stu-id="21b6e-168">-o</span></span> | <span data-ttu-id="21b6e-169">--выходные данные</span><span class="sxs-lookup"><span data-stu-id="21b6e-169">--output</span></span> | <span data-ttu-id="21b6e-170">Указывает путь загрузки для удаленного файла protobuf.</span><span class="sxs-lookup"><span data-stu-id="21b6e-170">Specifies the download path for the remote protobuf file.</span></span> <span data-ttu-id="21b6e-171">Это обязательный параметр.</span><span class="sxs-lookup"><span data-stu-id="21b6e-171">This is a required option.</span></span>
| <span data-ttu-id="21b6e-172">-p</span><span class="sxs-lookup"><span data-stu-id="21b6e-172">-p</span></span> | <span data-ttu-id="21b6e-173">--проект</span><span class="sxs-lookup"><span data-stu-id="21b6e-173">--project</span></span> | <span data-ttu-id="21b6e-174">Путь к файлу проекта для обработки.</span><span class="sxs-lookup"><span data-stu-id="21b6e-174">The path to the project file to operate on.</span></span> <span data-ttu-id="21b6e-175">Если файл не указан, команда ищет его в текущем каталоге.</span><span class="sxs-lookup"><span data-stu-id="21b6e-175">If a file is not specified, the command searches the current directory for one.</span></span>
| <span data-ttu-id="21b6e-176">-s</span><span class="sxs-lookup"><span data-stu-id="21b6e-176">-s</span></span> | <span data-ttu-id="21b6e-177">--службы</span><span class="sxs-lookup"><span data-stu-id="21b6e-177">--services</span></span> | <span data-ttu-id="21b6e-178">Тип gRPC служб, которые должны быть созданы.</span><span class="sxs-lookup"><span data-stu-id="21b6e-178">The type of gRPC services that should be generated.</span></span> <span data-ttu-id="21b6e-179">Если задано значение `Default`, для веб-проектов используется `Both`, а для проектов, не являющихся веб-проектами, используется `Client`.</span><span class="sxs-lookup"><span data-stu-id="21b6e-179">If `Default` is specified, `Both` is used for Web projects and `Client` is used for non-Web projects.</span></span> <span data-ttu-id="21b6e-180">Допустимые значения: `Both`, `Client`, `Default`, `None`, `Server`.</span><span class="sxs-lookup"><span data-stu-id="21b6e-180">Accepted values are `Both`, `Client`, `Default`, `None`, `Server`.</span></span>
| <span data-ttu-id="21b6e-181">-i</span><span class="sxs-lookup"><span data-stu-id="21b6e-181">-i</span></span> | <span data-ttu-id="21b6e-182">--reimports-dirs</span><span class="sxs-lookup"><span data-stu-id="21b6e-182">--additional-import-dirs</span></span> | <span data-ttu-id="21b6e-183">Дополнительные каталоги, которые будут использоваться при разрешении импортов для файлов protobuf.</span><span class="sxs-lookup"><span data-stu-id="21b6e-183">Additional directories to be used when resolving imports for the protobuf files.</span></span> <span data-ttu-id="21b6e-184">Это список путей, разделенных точкой с запятой.</span><span class="sxs-lookup"><span data-stu-id="21b6e-184">This is a semicolon separated list of paths.</span></span>
| | <span data-ttu-id="21b6e-185">--доступ</span><span class="sxs-lookup"><span data-stu-id="21b6e-185">--access</span></span> | <span data-ttu-id="21b6e-186">Модификатор доступа, используемый для создаваемых C# классов.</span><span class="sxs-lookup"><span data-stu-id="21b6e-186">The access modifier to use for the generated C# classes.</span></span> <span data-ttu-id="21b6e-187">Значение по умолчанию — `Public`.</span><span class="sxs-lookup"><span data-stu-id="21b6e-187">Default value is `Public`.</span></span> <span data-ttu-id="21b6e-188">Допустимые значения: `Internal` и `Public`.</span><span class="sxs-lookup"><span data-stu-id="21b6e-188">Accepted values are `Internal` and `Public`.</span></span>

## <a name="remove"></a><span data-ttu-id="21b6e-189">Удалить</span><span class="sxs-lookup"><span data-stu-id="21b6e-189">Remove</span></span>

<span data-ttu-id="21b6e-190">Команда `remove` используется для удаления ссылок protobuf из *CSPROJ* -файла.</span><span class="sxs-lookup"><span data-stu-id="21b6e-190">The `remove` command is used to remove Protobuf references from the *.csproj* file.</span></span> <span data-ttu-id="21b6e-191">Команда принимает аргументы пути и URL-адреса источника в качестве аргументов.</span><span class="sxs-lookup"><span data-stu-id="21b6e-191">The command accepts path arguments and source URLs as arguments.</span></span> <span data-ttu-id="21b6e-192">Средство:</span><span class="sxs-lookup"><span data-stu-id="21b6e-192">The tool:</span></span>

* <span data-ttu-id="21b6e-193">Удаляет только ссылку protobuf.</span><span class="sxs-lookup"><span data-stu-id="21b6e-193">Only removes the Protobuf reference.</span></span>
* <span data-ttu-id="21b6e-194">Не удаляет файл с *расширением.* , даже если он был первоначально скачан с удаленного URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="21b6e-194">Does not delete the *.proto* file, even if it was originally downloaded from a remote URL.</span></span>

### <a name="usage"></a><span data-ttu-id="21b6e-195">Использование</span><span class="sxs-lookup"><span data-stu-id="21b6e-195">Usage</span></span>

```dotnetcli
dotnet-grpc remove [options] <references>...
```

### <a name="arguments"></a><span data-ttu-id="21b6e-196">Аргументы</span><span class="sxs-lookup"><span data-stu-id="21b6e-196">Arguments</span></span>

| <span data-ttu-id="21b6e-197">Аргумент</span><span class="sxs-lookup"><span data-stu-id="21b6e-197">Argument</span></span> | <span data-ttu-id="21b6e-198">Описание</span><span class="sxs-lookup"><span data-stu-id="21b6e-198">Description</span></span> |
|-|-|
| <span data-ttu-id="21b6e-199">ссылки</span><span class="sxs-lookup"><span data-stu-id="21b6e-199">references</span></span> | <span data-ttu-id="21b6e-200">URL-адреса или пути к файлам удаляемых ссылок protobuf.</span><span class="sxs-lookup"><span data-stu-id="21b6e-200">The URLs or file paths of the protobuf references to remove.</span></span> |

### <a name="options"></a><span data-ttu-id="21b6e-201">Параметры</span><span class="sxs-lookup"><span data-stu-id="21b6e-201">Options</span></span>

| <span data-ttu-id="21b6e-202">Короткий параметр</span><span class="sxs-lookup"><span data-stu-id="21b6e-202">Short option</span></span> | <span data-ttu-id="21b6e-203">Длинный параметр</span><span class="sxs-lookup"><span data-stu-id="21b6e-203">Long option</span></span> | <span data-ttu-id="21b6e-204">Описание</span><span class="sxs-lookup"><span data-stu-id="21b6e-204">Description</span></span> |
|-|-|-|
| <span data-ttu-id="21b6e-205">-p</span><span class="sxs-lookup"><span data-stu-id="21b6e-205">-p</span></span> | <span data-ttu-id="21b6e-206">--проект</span><span class="sxs-lookup"><span data-stu-id="21b6e-206">--project</span></span> | <span data-ttu-id="21b6e-207">Путь к файлу проекта для обработки.</span><span class="sxs-lookup"><span data-stu-id="21b6e-207">The path to the project file to operate on.</span></span> <span data-ttu-id="21b6e-208">Если файл не указан, команда ищет его в текущем каталоге.</span><span class="sxs-lookup"><span data-stu-id="21b6e-208">If a file is not specified, the command searches the current directory for one.</span></span>

## <a name="refresh"></a><span data-ttu-id="21b6e-209">Обновление</span><span class="sxs-lookup"><span data-stu-id="21b6e-209">Refresh</span></span>

<span data-ttu-id="21b6e-210">Команда `refresh` используется для обновления удаленной ссылки последним содержимым из исходного URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="21b6e-210">The `refresh` command is used to update a remote reference with the latest content from the source URL.</span></span> <span data-ttu-id="21b6e-211">Для указания обновляемой ссылки можно использовать как путь к файлу загрузки, так и URL-адрес источника.</span><span class="sxs-lookup"><span data-stu-id="21b6e-211">Both the download file path and the source URL can be used to specify the reference to be updated.</span></span> <span data-ttu-id="21b6e-212">Примечание.</span><span class="sxs-lookup"><span data-stu-id="21b6e-212">Note:</span></span>

* <span data-ttu-id="21b6e-213">Хэши содержимого файла сравниваются, чтобы определить, следует ли обновлять локальный файл.</span><span class="sxs-lookup"><span data-stu-id="21b6e-213">The hashes of the file contents are compared to determine whether the local file should be updated.</span></span>
* <span data-ttu-id="21b6e-214">Сведения о метке времени не сравниваются.</span><span class="sxs-lookup"><span data-stu-id="21b6e-214">No timestamp information is compared.</span></span>

<span data-ttu-id="21b6e-215">Средство всегда заменяет локальный файл удаленным, если требуется обновление.</span><span class="sxs-lookup"><span data-stu-id="21b6e-215">The tool always replaces the local file with the remote file if an update is needed.</span></span>

### <a name="usage"></a><span data-ttu-id="21b6e-216">Использование</span><span class="sxs-lookup"><span data-stu-id="21b6e-216">Usage</span></span>

```dotnetcli
dotnet-grpc refresh [options] [<references>...]
```

### <a name="arguments"></a><span data-ttu-id="21b6e-217">Аргументы</span><span class="sxs-lookup"><span data-stu-id="21b6e-217">Arguments</span></span>

| <span data-ttu-id="21b6e-218">Аргумент</span><span class="sxs-lookup"><span data-stu-id="21b6e-218">Argument</span></span> | <span data-ttu-id="21b6e-219">Описание</span><span class="sxs-lookup"><span data-stu-id="21b6e-219">Description</span></span> |
|-|-|
| <span data-ttu-id="21b6e-220">ссылки</span><span class="sxs-lookup"><span data-stu-id="21b6e-220">references</span></span> | <span data-ttu-id="21b6e-221">URL-адреса или пути к файлам для удаленных ссылок protobuf, которые должны быть обновлены.</span><span class="sxs-lookup"><span data-stu-id="21b6e-221">The URLs or file paths to remote protobuf references that should be updated.</span></span> <span data-ttu-id="21b6e-222">Оставьте этот аргумент пустым, чтобы обновить все удаленные ссылки.</span><span class="sxs-lookup"><span data-stu-id="21b6e-222">Leave this argument empty to refresh all remote references.</span></span> |

### <a name="options"></a><span data-ttu-id="21b6e-223">Параметры</span><span class="sxs-lookup"><span data-stu-id="21b6e-223">Options</span></span>

| <span data-ttu-id="21b6e-224">Короткий параметр</span><span class="sxs-lookup"><span data-stu-id="21b6e-224">Short option</span></span> | <span data-ttu-id="21b6e-225">Длинный параметр</span><span class="sxs-lookup"><span data-stu-id="21b6e-225">Long option</span></span> | <span data-ttu-id="21b6e-226">Описание</span><span class="sxs-lookup"><span data-stu-id="21b6e-226">Description</span></span> |
|-|-|-|
| <span data-ttu-id="21b6e-227">-p</span><span class="sxs-lookup"><span data-stu-id="21b6e-227">-p</span></span> | <span data-ttu-id="21b6e-228">--проект</span><span class="sxs-lookup"><span data-stu-id="21b6e-228">--project</span></span> | <span data-ttu-id="21b6e-229">Путь к файлу проекта для обработки.</span><span class="sxs-lookup"><span data-stu-id="21b6e-229">The path to the project file to operate on.</span></span> <span data-ttu-id="21b6e-230">Если файл не указан, команда ищет его в текущем каталоге.</span><span class="sxs-lookup"><span data-stu-id="21b6e-230">If a file is not specified, the command searches the current directory for one.</span></span>
| | <span data-ttu-id="21b6e-231">--пробное выполнение</span><span class="sxs-lookup"><span data-stu-id="21b6e-231">--dry-run</span></span> | <span data-ttu-id="21b6e-232">Выводит список файлов, которые будут обновлены без скачивания нового содержимого.</span><span class="sxs-lookup"><span data-stu-id="21b6e-232">Outputs a list of files that would be updated without downloading any new content.</span></span>

## <a name="list"></a><span data-ttu-id="21b6e-233">List</span><span class="sxs-lookup"><span data-stu-id="21b6e-233">List</span></span>

<span data-ttu-id="21b6e-234">Команда `list` используется для вывода всех ссылок на protobuf в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="21b6e-234">The `list` command is used to display all the Protobuf references in the project file.</span></span> <span data-ttu-id="21b6e-235">Если все значения столбца являются значениями по умолчанию, столбец можно опустить.</span><span class="sxs-lookup"><span data-stu-id="21b6e-235">If all values of a column are default values, the column may be omitted.</span></span>

### <a name="usage"></a><span data-ttu-id="21b6e-236">Использование</span><span class="sxs-lookup"><span data-stu-id="21b6e-236">Usage</span></span>

```dotnetcli
dotnet-grpc list [options]
```

### <a name="options"></a><span data-ttu-id="21b6e-237">Параметры</span><span class="sxs-lookup"><span data-stu-id="21b6e-237">Options</span></span>

| <span data-ttu-id="21b6e-238">Короткий параметр</span><span class="sxs-lookup"><span data-stu-id="21b6e-238">Short option</span></span> | <span data-ttu-id="21b6e-239">Длинный параметр</span><span class="sxs-lookup"><span data-stu-id="21b6e-239">Long option</span></span> | <span data-ttu-id="21b6e-240">Описание</span><span class="sxs-lookup"><span data-stu-id="21b6e-240">Description</span></span> |
|-|-|-|
| <span data-ttu-id="21b6e-241">-p</span><span class="sxs-lookup"><span data-stu-id="21b6e-241">-p</span></span> | <span data-ttu-id="21b6e-242">--проект</span><span class="sxs-lookup"><span data-stu-id="21b6e-242">--project</span></span> | <span data-ttu-id="21b6e-243">Путь к файлу проекта для обработки.</span><span class="sxs-lookup"><span data-stu-id="21b6e-243">The path to the project file to operate on.</span></span> <span data-ttu-id="21b6e-244">Если файл не указан, команда ищет его в текущем каталоге.</span><span class="sxs-lookup"><span data-stu-id="21b6e-244">If a file is not specified, the command searches the current directory for one.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="21b6e-245">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="21b6e-245">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
