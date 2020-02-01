---
title: Использование интерфейса командной строки Либман с ASP.NET Core
author: scottaddie
description: Узнайте, как использовать интерфейс командной строки Либман в проекте ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: client-side/libman/libman-cli
ms.openlocfilehash: 02d88d09805bd23a86ef924766373245fec7ff52
ms.sourcegitcommit: 0b0e485a8a6dfcc65a7a58b365622b3839f4d624
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2020
ms.locfileid: "76928366"
---
# <a name="use-the-libman-cli-with-aspnet-core"></a><span data-ttu-id="f2b65-103">Использование интерфейса командной строки Либман с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f2b65-103">Use the LibMan CLI with ASP.NET Core</span></span>

<span data-ttu-id="f2b65-104">Автор: [Скотт Адди](https://twitter.com/Scott_Addie) (Scott Addie)</span><span class="sxs-lookup"><span data-stu-id="f2b65-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="f2b65-105">[Либман](xref:client-side/libman/index) CLI — это кросс-платформенный инструмент, поддерживаемый везде, где поддерживается .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f2b65-105">The [LibMan](xref:client-side/libman/index) CLI is a cross-platform tool that's supported everywhere .NET Core is supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f2b65-106">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="f2b65-106">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="f2b65-107">Установка</span><span class="sxs-lookup"><span data-stu-id="f2b65-107">Installation</span></span>

<span data-ttu-id="f2b65-108">Чтобы установить интерфейс командной строки Либман, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="f2b65-108">To install the LibMan CLI:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

<span data-ttu-id="f2b65-109">[Глобальное средство .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) устанавливается из пакета NuGet [Microsoft. Web. либрариманажер. CLI](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) .</span><span class="sxs-lookup"><span data-stu-id="f2b65-109">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet package.</span></span>

<span data-ttu-id="f2b65-110">Чтобы установить Либман CLI из указанного источника пакета NuGet, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="f2b65-110">To install the LibMan CLI from a specific NuGet package source:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

<span data-ttu-id="f2b65-111">В предыдущем примере глобальный инструмент .NET Core устанавливается из файла *C:\Temp\Microsoft.Web.LibraryManager.CLI.1.0.94-g606058a278.nupkg* локального компьютера Windows.</span><span class="sxs-lookup"><span data-stu-id="f2b65-111">In the preceding example, a .NET Core Global Tool is installed from the local Windows machine's *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* file.</span></span>

## <a name="usage"></a><span data-ttu-id="f2b65-112">Метрики</span><span class="sxs-lookup"><span data-stu-id="f2b65-112">Usage</span></span>

<span data-ttu-id="f2b65-113">После успешной установки интерфейса командной строки можно использовать следующую команду:</span><span class="sxs-lookup"><span data-stu-id="f2b65-113">After successful installation of the CLI, the following command can be used:</span></span>

```console
libman
```

<span data-ttu-id="f2b65-114">Чтобы просмотреть установленную версию CLI, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="f2b65-114">To view the installed CLI version:</span></span>

```console
libman --version
```

<span data-ttu-id="f2b65-115">Чтобы просмотреть доступные команды интерфейса командной строки:</span><span class="sxs-lookup"><span data-stu-id="f2b65-115">To view the available CLI commands:</span></span>

```console
libman --help
```

<span data-ttu-id="f2b65-116">Приведенная выше команда отображает выходные данные, аналогичные приведенным ниже.</span><span class="sxs-lookup"><span data-stu-id="f2b65-116">The preceding command displays output similar to the following:</span></span>

```console
 1.0.163+g45474d37ed

Usage: libman [options] [command]

Options:
  --help|-h  Show help information
  --version  Show version information

Commands:
  cache      List or clean libman cache contents
  clean      Deletes all library files defined in libman.json from the project
  init       Create a new libman.json
  install    Add a library definition to the libman.json file, and download the 
             library to the specified location
  restore    Downloads all files from provider and saves them to specified 
             destination
  uninstall  Deletes all files for the specified library from their specified 
             destination, then removes the specified library definition from 
             libman.json
  update     Updates the specified library

Use "libman [command] --help" for more information about a command.
```

<span data-ttu-id="f2b65-117">Доступные команды CLI описываются в следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="f2b65-117">The following sections outline the available CLI commands.</span></span>

## <a name="initialize-libman-in-the-project"></a><span data-ttu-id="f2b65-118">Инициализация Либман в проекте</span><span class="sxs-lookup"><span data-stu-id="f2b65-118">Initialize LibMan in the project</span></span>

<span data-ttu-id="f2b65-119">Команда `libman init` создает файл *Либман. JSON* , если он не существует.</span><span class="sxs-lookup"><span data-stu-id="f2b65-119">The `libman init` command creates a *libman.json* file if one doesn't exist.</span></span> <span data-ttu-id="f2b65-120">Файл создается с содержимым шаблона элемента по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f2b65-120">The file is created with the default item template content.</span></span>

### <a name="synopsis"></a><span data-ttu-id="f2b65-121">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="f2b65-121">Synopsis</span></span>

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a><span data-ttu-id="f2b65-122">Options</span><span class="sxs-lookup"><span data-stu-id="f2b65-122">Options</span></span>

<span data-ttu-id="f2b65-123">Для команды `libman init` доступны следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="f2b65-123">The following options are available for the `libman init` command:</span></span>

* `-d|--default-destination <PATH>`

  <span data-ttu-id="f2b65-124">Путь относительно текущей папки.</span><span class="sxs-lookup"><span data-stu-id="f2b65-124">A path relative to the current folder.</span></span> <span data-ttu-id="f2b65-125">Файлы библиотеки устанавливаются в этом расположении, если для библиотеки в *Либман. JSON*не определено свойство `destination`.</span><span class="sxs-lookup"><span data-stu-id="f2b65-125">Library files are installed in this location if no `destination` property is defined for a library in *libman.json*.</span></span> <span data-ttu-id="f2b65-126">Значение `<PATH>` записывается в свойство `defaultDestination` *Либман. JSON*.</span><span class="sxs-lookup"><span data-stu-id="f2b65-126">The `<PATH>` value is written to the `defaultDestination` property of *libman.json*.</span></span>

* `-p|--default-provider <PROVIDER>`

  <span data-ttu-id="f2b65-127">Поставщик, используемый, если для данной библиотеки не определен поставщик.</span><span class="sxs-lookup"><span data-stu-id="f2b65-127">The provider to use if no provider is defined for a given library.</span></span> <span data-ttu-id="f2b65-128">Значение `<PROVIDER>` записывается в свойство `defaultProvider` *Либман. JSON*.</span><span class="sxs-lookup"><span data-stu-id="f2b65-128">The `<PROVIDER>` value is written to the `defaultProvider` property of *libman.json*.</span></span> <span data-ttu-id="f2b65-129">Замените `<PROVIDER>` одним из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="f2b65-129">Replace `<PROVIDER>` with one of the following values:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="f2b65-130">Примеры</span><span class="sxs-lookup"><span data-stu-id="f2b65-130">Examples</span></span>

<span data-ttu-id="f2b65-131">Чтобы создать файл *Либман. JSON* в проекте ASP.NET Core, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="f2b65-131">To create a *libman.json* file in an ASP.NET Core project:</span></span>

* <span data-ttu-id="f2b65-132">Перейдите к корню проекта.</span><span class="sxs-lookup"><span data-stu-id="f2b65-132">Navigate to the project root.</span></span>
* <span data-ttu-id="f2b65-133">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="f2b65-133">Run the following command:</span></span>

  ```console
  libman init
  ```

* <span data-ttu-id="f2b65-134">Введите имя поставщика по умолчанию или нажмите `Enter`, чтобы использовать поставщик CDNJS по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f2b65-134">Type the name of the default provider, or press `Enter` to use the default CDNJS provider.</span></span> <span data-ttu-id="f2b65-135">Допустимы следующие значения:</span><span class="sxs-lookup"><span data-stu-id="f2b65-135">Valid values include:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![Либман init, команда-поставщик по умолчанию](_static/libman-init-provider.png)

<span data-ttu-id="f2b65-137">Файл *Либман. JSON* добавляется в корень проекта со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="f2b65-137">A *libman.json* file is added to the project root with the following content:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a><span data-ttu-id="f2b65-138">Добавление файлов библиотеки</span><span class="sxs-lookup"><span data-stu-id="f2b65-138">Add library files</span></span>

<span data-ttu-id="f2b65-139">Команда `libman install` скачивает и устанавливает файлы библиотеки в проект.</span><span class="sxs-lookup"><span data-stu-id="f2b65-139">The `libman install` command downloads and installs library files into the project.</span></span> <span data-ttu-id="f2b65-140">Файл *Либман. JSON* добавляется, если он не существует.</span><span class="sxs-lookup"><span data-stu-id="f2b65-140">A *libman.json* file is added if one doesn't exist.</span></span> <span data-ttu-id="f2b65-141">Файл *Либман. JSON* изменяется для хранения сведений о конфигурации для файлов библиотеки.</span><span class="sxs-lookup"><span data-stu-id="f2b65-141">The *libman.json* file is modified to store configuration details for the library files.</span></span>

### <a name="synopsis"></a><span data-ttu-id="f2b65-142">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="f2b65-142">Synopsis</span></span>

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="f2b65-143">Arguments</span><span class="sxs-lookup"><span data-stu-id="f2b65-143">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="f2b65-144">Имя устанавливаемой библиотеки.</span><span class="sxs-lookup"><span data-stu-id="f2b65-144">The name of the library to install.</span></span> <span data-ttu-id="f2b65-145">Это имя может включать нотацию номера версии (например, `@1.2.0`).</span><span class="sxs-lookup"><span data-stu-id="f2b65-145">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="f2b65-146">Options</span><span class="sxs-lookup"><span data-stu-id="f2b65-146">Options</span></span>

<span data-ttu-id="f2b65-147">Для команды `libman install` доступны следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="f2b65-147">The following options are available for the `libman install` command:</span></span>

* `-d|--destination <PATH>`

  <span data-ttu-id="f2b65-148">Расположение для установки библиотеки.</span><span class="sxs-lookup"><span data-stu-id="f2b65-148">The location to install the library.</span></span> <span data-ttu-id="f2b65-149">Если значение не указано, используется расположение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f2b65-149">If not specified, the default location is used.</span></span> <span data-ttu-id="f2b65-150">Если в *Либман. JSON*не указано свойство `defaultDestination`, этот параметр является обязательным.</span><span class="sxs-lookup"><span data-stu-id="f2b65-150">If no `defaultDestination` property is specified in *libman.json*, this option is required.</span></span>

* `--files <FILE>`

  <span data-ttu-id="f2b65-151">Укажите имя файла для установки из библиотеки.</span><span class="sxs-lookup"><span data-stu-id="f2b65-151">Specify the name of the file to install from the library.</span></span> <span data-ttu-id="f2b65-152">Если этот параметр не указан, устанавливаются все файлы из библиотеки.</span><span class="sxs-lookup"><span data-stu-id="f2b65-152">If not specified, all files from the library are installed.</span></span> <span data-ttu-id="f2b65-153">Укажите один параметр `--files` для каждого файла, который необходимо установить.</span><span class="sxs-lookup"><span data-stu-id="f2b65-153">Provide one `--files` option per file to be installed.</span></span> <span data-ttu-id="f2b65-154">Относительные пути также поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="f2b65-154">Relative paths are supported too.</span></span> <span data-ttu-id="f2b65-155">Например, `--files dist/browser/signalr.js`.</span><span class="sxs-lookup"><span data-stu-id="f2b65-155">For example: `--files dist/browser/signalr.js`.</span></span>

* `-p|--provider <PROVIDER>`

  <span data-ttu-id="f2b65-156">Имя поставщика, используемого для получения библиотеки.</span><span class="sxs-lookup"><span data-stu-id="f2b65-156">The name of the provider to use for the library acquisition.</span></span> <span data-ttu-id="f2b65-157">Замените `<PROVIDER>` одним из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="f2b65-157">Replace `<PROVIDER>` with one of the following values:</span></span>
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  <span data-ttu-id="f2b65-158">Если значение не указано, используется свойство `defaultProvider` в *Либман. JSON* .</span><span class="sxs-lookup"><span data-stu-id="f2b65-158">If not specified, the `defaultProvider` property in *libman.json* is used.</span></span> <span data-ttu-id="f2b65-159">Если в *Либман. JSON*не указано свойство `defaultProvider`, этот параметр является обязательным.</span><span class="sxs-lookup"><span data-stu-id="f2b65-159">If no `defaultProvider` property is specified in *libman.json*, this option is required.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="f2b65-160">Примеры</span><span class="sxs-lookup"><span data-stu-id="f2b65-160">Examples</span></span>

<span data-ttu-id="f2b65-161">Рассмотрим следующий файл *Либман. JSON* :</span><span class="sxs-lookup"><span data-stu-id="f2b65-161">Consider the following *libman.json* file:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

<span data-ttu-id="f2b65-162">Чтобы установить файл jQuery версии 3.2.1 *jQuery. min. js* в папку *wwwroot/Scripts/jQuery* с помощью поставщика CDNJS:</span><span class="sxs-lookup"><span data-stu-id="f2b65-162">To install the jQuery version 3.2.1 *jquery.min.js* file to the *wwwroot/scripts/jquery* folder using the CDNJS provider:</span></span>

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot/scripts/jquery --files jquery.min.js
```

<span data-ttu-id="f2b65-163">Файл *Либман. JSON* выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f2b65-163">The *libman.json* file resembles the following:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot/scripts/jquery",
      "files": [
        "jquery.min.js"
      ]
    }
  ]
}
```

<span data-ttu-id="f2b65-164">Чтобы установить файлы *Calendar. js* и *Calendar. CSS* с *диска C:\\Temp\\контосокалендар\\* с помощью поставщика файловой системы:</span><span class="sxs-lookup"><span data-stu-id="f2b65-164">To install the *calendar.js* and *calendar.css* files from *C:\\temp\\contosoCalendar\\* using the file system provider:</span></span>

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

<span data-ttu-id="f2b65-165">Следующий запрос появляется по двум причинам:</span><span class="sxs-lookup"><span data-stu-id="f2b65-165">The following prompt appears for two reasons:</span></span>

* <span data-ttu-id="f2b65-166">Файл *Либман. JSON* не содержит свойство `defaultDestination`.</span><span class="sxs-lookup"><span data-stu-id="f2b65-166">The *libman.json* file doesn't contain a `defaultDestination` property.</span></span>
* <span data-ttu-id="f2b65-167">Команда `libman install` не содержит параметр `-d|--destination`.</span><span class="sxs-lookup"><span data-stu-id="f2b65-167">The `libman install` command doesn't contain the `-d|--destination` option.</span></span>

![Команда установки Либман](_static/libman-install-destination.png)

<span data-ttu-id="f2b65-169">После принятия назначения по умолчанию файл *Либман. JSON* будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f2b65-169">After accepting the default destination, the *libman.json* file resembles the following:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot/scripts/jquery",
      "files": [
        "jquery.min.js"
      ]
    },
    {
      "library": "C:\\temp\\contosoCalendar\\",
      "provider": "filesystem",
      "destination": "wwwroot/lib/contosoCalendar",
      "files": [
        "calendar.js",
        "calendar.css"
      ]
    }
  ]
}
```

## <a name="restore-library-files"></a><span data-ttu-id="f2b65-170">Восстановление файлов библиотеки</span><span class="sxs-lookup"><span data-stu-id="f2b65-170">Restore library files</span></span>

<span data-ttu-id="f2b65-171">Команда `libman restore` устанавливает файлы библиотеки, определенные в *Либман. JSON*.</span><span class="sxs-lookup"><span data-stu-id="f2b65-171">The `libman restore` command installs library files defined in *libman.json*.</span></span> <span data-ttu-id="f2b65-172">Действуют следующие правила.</span><span class="sxs-lookup"><span data-stu-id="f2b65-172">The following rules apply:</span></span>

* <span data-ttu-id="f2b65-173">Если в корне проекта не существует файла *Либман. JSON* , возвращается ошибка.</span><span class="sxs-lookup"><span data-stu-id="f2b65-173">If no *libman.json* file exists in the project root, an error is returned.</span></span>
* <span data-ttu-id="f2b65-174">Если в библиотеке указан поставщик, свойство `defaultProvider` в *Либман. JSON* игнорируется.</span><span class="sxs-lookup"><span data-stu-id="f2b65-174">If a library specifies a provider, the `defaultProvider` property in *libman.json* is ignored.</span></span>
* <span data-ttu-id="f2b65-175">Если в библиотеке указано назначение, свойство `defaultDestination` в *Либман. JSON* игнорируется.</span><span class="sxs-lookup"><span data-stu-id="f2b65-175">If a library specifies a destination, the `defaultDestination` property in *libman.json* is ignored.</span></span>

### <a name="synopsis"></a><span data-ttu-id="f2b65-176">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="f2b65-176">Synopsis</span></span>

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a><span data-ttu-id="f2b65-177">Options</span><span class="sxs-lookup"><span data-stu-id="f2b65-177">Options</span></span>

<span data-ttu-id="f2b65-178">Для команды `libman restore` доступны следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="f2b65-178">The following options are available for the `libman restore` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="f2b65-179">Примеры</span><span class="sxs-lookup"><span data-stu-id="f2b65-179">Examples</span></span>

<span data-ttu-id="f2b65-180">Чтобы восстановить файлы библиотеки, определенные в *Либман. JSON*, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="f2b65-180">To restore the library files defined in *libman.json*:</span></span>

```console
libman restore
```

## <a name="delete-library-files"></a><span data-ttu-id="f2b65-181">Удаление файлов библиотеки</span><span class="sxs-lookup"><span data-stu-id="f2b65-181">Delete library files</span></span>

<span data-ttu-id="f2b65-182">Команда `libman clean` удаляет файлы библиотеки, восстановленные ранее с помощью Либман.</span><span class="sxs-lookup"><span data-stu-id="f2b65-182">The `libman clean` command deletes library files previously restored via LibMan.</span></span> <span data-ttu-id="f2b65-183">Папки, которые становятся пустыми после удаления этой операции.</span><span class="sxs-lookup"><span data-stu-id="f2b65-183">Folders that become empty after this operation are deleted.</span></span> <span data-ttu-id="f2b65-184">Конфигурации, связанные с файлами библиотеки, в свойстве `libraries` *Либман. JSON* не удаляются.</span><span class="sxs-lookup"><span data-stu-id="f2b65-184">The library files' associated configurations in the `libraries` property of *libman.json* aren't removed.</span></span>

### <a name="synopsis"></a><span data-ttu-id="f2b65-185">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="f2b65-185">Synopsis</span></span>

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a><span data-ttu-id="f2b65-186">Options</span><span class="sxs-lookup"><span data-stu-id="f2b65-186">Options</span></span>

<span data-ttu-id="f2b65-187">Для команды `libman clean` доступны следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="f2b65-187">The following options are available for the `libman clean` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="f2b65-188">Примеры</span><span class="sxs-lookup"><span data-stu-id="f2b65-188">Examples</span></span>

<span data-ttu-id="f2b65-189">Удаление файлов библиотеки, установленных с помощью Либман:</span><span class="sxs-lookup"><span data-stu-id="f2b65-189">To delete library files installed via LibMan:</span></span>

```console
libman clean
```

## <a name="uninstall-library-files"></a><span data-ttu-id="f2b65-190">Удаление файлов библиотеки</span><span class="sxs-lookup"><span data-stu-id="f2b65-190">Uninstall library files</span></span>

<span data-ttu-id="f2b65-191">Команда `libman uninstall`:</span><span class="sxs-lookup"><span data-stu-id="f2b65-191">The `libman uninstall` command:</span></span>

* <span data-ttu-id="f2b65-192">Удаляет все файлы, связанные с указанной библиотекой, из места назначения в *Либман. JSON*.</span><span class="sxs-lookup"><span data-stu-id="f2b65-192">Deletes all files associated with the specified library from the destination in *libman.json*.</span></span>
* <span data-ttu-id="f2b65-193">Удаляет связанную конфигурацию библиотеки из *Либман. JSON*.</span><span class="sxs-lookup"><span data-stu-id="f2b65-193">Removes the associated library configuration from *libman.json*.</span></span>

<span data-ttu-id="f2b65-194">Ошибка возникает в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="f2b65-194">An error occurs when:</span></span>

* <span data-ttu-id="f2b65-195">В корне проекта не существует файла *Либман. JSON* .</span><span class="sxs-lookup"><span data-stu-id="f2b65-195">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="f2b65-196">Указанная библиотека не существует.</span><span class="sxs-lookup"><span data-stu-id="f2b65-196">The specified library doesn't exist.</span></span>

<span data-ttu-id="f2b65-197">Если установлено несколько библиотек с одним и тем же именем, вам будет предложено выбрать одну из них.</span><span class="sxs-lookup"><span data-stu-id="f2b65-197">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="f2b65-198">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="f2b65-198">Synopsis</span></span>

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="f2b65-199">Arguments</span><span class="sxs-lookup"><span data-stu-id="f2b65-199">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="f2b65-200">Имя удаляемой библиотеки.</span><span class="sxs-lookup"><span data-stu-id="f2b65-200">The name of the library to uninstall.</span></span> <span data-ttu-id="f2b65-201">Это имя может включать нотацию номера версии (например, `@1.2.0`).</span><span class="sxs-lookup"><span data-stu-id="f2b65-201">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="f2b65-202">Options</span><span class="sxs-lookup"><span data-stu-id="f2b65-202">Options</span></span>

<span data-ttu-id="f2b65-203">Для команды `libman uninstall` доступны следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="f2b65-203">The following options are available for the `libman uninstall` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="f2b65-204">Примеры</span><span class="sxs-lookup"><span data-stu-id="f2b65-204">Examples</span></span>

<span data-ttu-id="f2b65-205">Рассмотрим следующий файл *Либман. JSON* :</span><span class="sxs-lookup"><span data-stu-id="f2b65-205">Consider the following *libman.json* file:</span></span>

[!code-json[](samples/LibManSample/libman.json)]

* <span data-ttu-id="f2b65-206">Чтобы удалить jQuery, выполните одну из следующих команд:</span><span class="sxs-lookup"><span data-stu-id="f2b65-206">To uninstall jQuery, either of the following commands succeed:</span></span>

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* <span data-ttu-id="f2b65-207">Чтобы удалить файлы Лодаш, установленные с помощью поставщика `filesystem`, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="f2b65-207">To uninstall the Lodash files installed via the `filesystem` provider:</span></span>

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a><span data-ttu-id="f2b65-208">Обновление версии библиотеки</span><span class="sxs-lookup"><span data-stu-id="f2b65-208">Update library version</span></span>

<span data-ttu-id="f2b65-209">Команда `libman update` обновляет библиотеку, установленную с помощью Либман, до указанной версии.</span><span class="sxs-lookup"><span data-stu-id="f2b65-209">The `libman update` command updates a library installed via LibMan to the specified version.</span></span>

<span data-ttu-id="f2b65-210">Ошибка возникает в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="f2b65-210">An error occurs when:</span></span>

* <span data-ttu-id="f2b65-211">В корне проекта не существует файла *Либман. JSON* .</span><span class="sxs-lookup"><span data-stu-id="f2b65-211">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="f2b65-212">Указанная библиотека не существует.</span><span class="sxs-lookup"><span data-stu-id="f2b65-212">The specified library doesn't exist.</span></span>

<span data-ttu-id="f2b65-213">Если установлено несколько библиотек с одним и тем же именем, вам будет предложено выбрать одну из них.</span><span class="sxs-lookup"><span data-stu-id="f2b65-213">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="f2b65-214">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="f2b65-214">Synopsis</span></span>

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="f2b65-215">Arguments</span><span class="sxs-lookup"><span data-stu-id="f2b65-215">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="f2b65-216">Имя обновляемой библиотеки.</span><span class="sxs-lookup"><span data-stu-id="f2b65-216">The name of the library to update.</span></span>

### <a name="options"></a><span data-ttu-id="f2b65-217">Options</span><span class="sxs-lookup"><span data-stu-id="f2b65-217">Options</span></span>

<span data-ttu-id="f2b65-218">Для команды `libman update` доступны следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="f2b65-218">The following options are available for the `libman update` command:</span></span>

* `-pre`

  <span data-ttu-id="f2b65-219">Получите последнюю предварительную версию библиотеки.</span><span class="sxs-lookup"><span data-stu-id="f2b65-219">Obtain the latest prerelease version of the library.</span></span>

* `--to <VERSION>`

  <span data-ttu-id="f2b65-220">Получение определенной версии библиотеки.</span><span class="sxs-lookup"><span data-stu-id="f2b65-220">Obtain a specific version of the library.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="f2b65-221">Примеры</span><span class="sxs-lookup"><span data-stu-id="f2b65-221">Examples</span></span>

* <span data-ttu-id="f2b65-222">Чтобы обновить jQuery до последней версии, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="f2b65-222">To update jQuery to the latest version:</span></span>

  ```console
  libman update jquery
  ```

* <span data-ttu-id="f2b65-223">Чтобы обновить jQuery до версии 3.3.1, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="f2b65-223">To update jQuery to version 3.3.1:</span></span>

  ```console
  libman update jquery --to 3.3.1
  ```

* <span data-ttu-id="f2b65-224">Чтобы обновить jQuery до последней предварительной версии, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="f2b65-224">To update jQuery to the latest prerelease version:</span></span>

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a><span data-ttu-id="f2b65-225">Управление кэшем библиотеки</span><span class="sxs-lookup"><span data-stu-id="f2b65-225">Manage library cache</span></span>

<span data-ttu-id="f2b65-226">Команда `libman cache` управляет кэшем библиотеки Либман.</span><span class="sxs-lookup"><span data-stu-id="f2b65-226">The `libman cache` command manages the LibMan library cache.</span></span> <span data-ttu-id="f2b65-227">Поставщик `filesystem` не использует кэш библиотеки.</span><span class="sxs-lookup"><span data-stu-id="f2b65-227">The `filesystem` provider doesn't use the library cache.</span></span>

### <a name="synopsis"></a><span data-ttu-id="f2b65-228">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="f2b65-228">Synopsis</span></span>

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="f2b65-229">Arguments</span><span class="sxs-lookup"><span data-stu-id="f2b65-229">Arguments</span></span>

`PROVIDER`

<span data-ttu-id="f2b65-230">Используется только с командой `clean`.</span><span class="sxs-lookup"><span data-stu-id="f2b65-230">Only used with the `clean` command.</span></span> <span data-ttu-id="f2b65-231">Указывает кэш поставщика для очистки.</span><span class="sxs-lookup"><span data-stu-id="f2b65-231">Specifies the provider cache to clean.</span></span> <span data-ttu-id="f2b65-232">Допустимы следующие значения:</span><span class="sxs-lookup"><span data-stu-id="f2b65-232">Valid values include:</span></span>

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a><span data-ttu-id="f2b65-233">Options</span><span class="sxs-lookup"><span data-stu-id="f2b65-233">Options</span></span>

<span data-ttu-id="f2b65-234">Для команды `libman cache` доступны следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="f2b65-234">The following options are available for the `libman cache` command:</span></span>

* `--files`

  <span data-ttu-id="f2b65-235">Список имен файлов, которые кэшируются.</span><span class="sxs-lookup"><span data-stu-id="f2b65-235">List the names of files that are cached.</span></span>

* `--libraries`

  <span data-ttu-id="f2b65-236">Перечислите имена библиотек, которые кэшируются.</span><span class="sxs-lookup"><span data-stu-id="f2b65-236">List the names of libraries that are cached.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="f2b65-237">Примеры</span><span class="sxs-lookup"><span data-stu-id="f2b65-237">Examples</span></span>

* <span data-ttu-id="f2b65-238">Чтобы просмотреть имена кэшированных библиотек для каждого поставщика, выполните одну из следующих команд:</span><span class="sxs-lookup"><span data-stu-id="f2b65-238">To view the names of cached libraries per provider, use one of the following commands:</span></span>

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  <span data-ttu-id="f2b65-239">Выходные данные должны выглядеть примерно так:</span><span class="sxs-lookup"><span data-stu-id="f2b65-239">Output similar to the following is displayed:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      font-awesome
      jquery
      knockout
      lodash.js
      react
  ```

* <span data-ttu-id="f2b65-240">Чтобы просмотреть имена кэшированных файлов библиотеки для каждого поставщика:</span><span class="sxs-lookup"><span data-stu-id="f2b65-240">To view the names of cached library files per provider:</span></span>

  ```console
  libman cache list --files
  ```

  <span data-ttu-id="f2b65-241">Выходные данные должны выглядеть примерно так:</span><span class="sxs-lookup"><span data-stu-id="f2b65-241">Output similar to the following is displayed:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout:
          <list omitted for brevity>
      react:
          <list omitted for brevity>
      vue:
          <list omitted for brevity>
  cdnjs:
      font-awesome
          metadata.json
      jquery
          metadata.json
          3.2.1\core.js
          3.2.1\jquery.js
          3.2.1\jquery.min.js
          3.2.1\jquery.min.map
          3.2.1\jquery.slim.js
          3.2.1\jquery.slim.min.js
          3.2.1\jquery.slim.min.map
          3.3.1\core.js
          3.3.1\jquery.js
          3.3.1\jquery.min.js
          3.3.1\jquery.min.map
          3.3.1\jquery.slim.js
          3.3.1\jquery.slim.min.js
          3.3.1\jquery.slim.min.map
      knockout
          metadata.json
          3.4.2\knockout-debug.js
          3.4.2\knockout-min.js
      lodash.js
          metadata.json
          4.17.10\lodash.js
          4.17.10\lodash.min.js
      react
          metadata.json
  ```

  <span data-ttu-id="f2b65-242">Обратите внимание, что предыдущие выходные данные показывают, что jQuery версии 3.2.1 и 3.3.1 кэшируются в поставщике CDNJS.</span><span class="sxs-lookup"><span data-stu-id="f2b65-242">Notice the preceding output shows that jQuery versions 3.2.1 and 3.3.1 are cached under the CDNJS provider.</span></span>

* <span data-ttu-id="f2b65-243">Чтобы очистить кэш библиотеки для поставщика CDNJS, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="f2b65-243">To empty the library cache for the CDNJS provider:</span></span>

  ```console
  libman cache clean cdnjs
  ```

  <span data-ttu-id="f2b65-244">После очистки кэша поставщика CDNJS команда `libman cache list` выводит следующее:</span><span class="sxs-lookup"><span data-stu-id="f2b65-244">After emptying the CDNJS provider cache, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      (empty)
  ```

* <span data-ttu-id="f2b65-245">Чтобы очистить кэш для всех поддерживаемых поставщиков, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="f2b65-245">To empty the cache for all supported providers:</span></span>

  ```console
  libman cache clean
  ```

  <span data-ttu-id="f2b65-246">После очистки всех кэшей поставщика команда `libman cache list` выводит следующее:</span><span class="sxs-lookup"><span data-stu-id="f2b65-246">After emptying all provider caches, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a><span data-ttu-id="f2b65-247">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f2b65-247">Additional resources</span></span>

* [<span data-ttu-id="f2b65-248">Установка глобального средства</span><span class="sxs-lookup"><span data-stu-id="f2b65-248">Install a Global Tool</span></span>](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [<span data-ttu-id="f2b65-249">Репозиторий LibMan на GitHub</span><span class="sxs-lookup"><span data-stu-id="f2b65-249">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
