---
title: Использование LibMan командной строки (CLI) с ASP.NET Core
author: scottaddie
description: Сведения об использовании LibMan командной строки (CLI) в проекте ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/30/2018
uid: client-side/libman/libman-cli
ms.openlocfilehash: 5667f79648a60b8fd9496f8041ef08891ab766af
ms.sourcegitcommit: 9bdba90b2c97a4016188434657194b2d7027d6e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/27/2018
ms.locfileid: "47402089"
---
# <a name="use-the-libman-command-line-interface-cli-with-aspnet-core"></a><span data-ttu-id="4339d-103">Использование LibMan командной строки (CLI) с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4339d-103">Use the LibMan command-line interface (CLI) with ASP.NET Core</span></span>

<span data-ttu-id="4339d-104">Автор: [Скотт Адди](https://twitter.com/Scott_Addie) (Scott Addie)</span><span class="sxs-lookup"><span data-stu-id="4339d-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="4339d-105">[LibMan](xref:client-side/libman/index) CLI — это средство кросс платформенных, имеющий поддерживаемые everywhere .NET Core поддерживается.</span><span class="sxs-lookup"><span data-stu-id="4339d-105">The [LibMan](xref:client-side/libman/index) CLI is a cross-platform tool that's supported everywhere .NET Core is supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4339d-106">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="4339d-106">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="4339d-107">Установка</span><span class="sxs-lookup"><span data-stu-id="4339d-107">Installation</span></span>

<span data-ttu-id="4339d-108">Чтобы установить LibMan CLI:</span><span class="sxs-lookup"><span data-stu-id="4339d-108">To install the LibMan CLI:</span></span>

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

<span data-ttu-id="4339d-109">Объект [глобальное средство .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) устанавливается из [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="4339d-109">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet package.</span></span>

<span data-ttu-id="4339d-110">Чтобы установить LibMan CLI из определенного источника пакета NuGet:</span><span class="sxs-lookup"><span data-stu-id="4339d-110">To install the LibMan CLI from a specific NuGet package source:</span></span>

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

<span data-ttu-id="4339d-111">В предыдущем примере, это средство глобального .NET Core устанавливается с локального компьютера Windows *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* файла.</span><span class="sxs-lookup"><span data-stu-id="4339d-111">In the preceding example, a .NET Core Global Tool is installed from the local Windows machine's *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* file.</span></span>

## <a name="usage"></a><span data-ttu-id="4339d-112">Использование</span><span class="sxs-lookup"><span data-stu-id="4339d-112">Usage</span></span>

<span data-ttu-id="4339d-113">После успешной установки интерфейса командной строки можно использовать следующую команду:</span><span class="sxs-lookup"><span data-stu-id="4339d-113">After successful installation of the CLI, the following command can be used:</span></span>

```console
libman
```

<span data-ttu-id="4339d-114">Чтобы просмотреть установленную версию интерфейса командной строки:</span><span class="sxs-lookup"><span data-stu-id="4339d-114">To view the installed CLI version:</span></span>

```console
libman --version
```

<span data-ttu-id="4339d-115">Для просмотра доступных команд интерфейса командной строки:</span><span class="sxs-lookup"><span data-stu-id="4339d-115">To view the available CLI commands:</span></span>

```console
libman --help
```

<span data-ttu-id="4339d-116">Приведенная выше команда отображает результат, аналогичный приведенному ниже:</span><span class="sxs-lookup"><span data-stu-id="4339d-116">The preceding command displays output similar to the following:</span></span>

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

<span data-ttu-id="4339d-117">В следующих разделах описываются доступные команды интерфейса командной строки.</span><span class="sxs-lookup"><span data-stu-id="4339d-117">The following sections outline the available CLI commands.</span></span>

## <a name="initialize-libman-in-the-project"></a><span data-ttu-id="4339d-118">Инициализировать LibMan в проекте</span><span class="sxs-lookup"><span data-stu-id="4339d-118">Initialize LibMan in the project</span></span>

<span data-ttu-id="4339d-119">`libman init` Команда создает *libman.json* файла, если она не существует.</span><span class="sxs-lookup"><span data-stu-id="4339d-119">The `libman init` command creates a *libman.json* file if one doesn't exist.</span></span> <span data-ttu-id="4339d-120">Файл создан с содержимым шаблона элемента по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="4339d-120">The file is created with the default item template content.</span></span>

### <a name="synopsis"></a><span data-ttu-id="4339d-121">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="4339d-121">Synopsis</span></span>

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a><span data-ttu-id="4339d-122">Параметры</span><span class="sxs-lookup"><span data-stu-id="4339d-122">Options</span></span>

<span data-ttu-id="4339d-123">Следующие параметры доступны для `libman init` команды:</span><span class="sxs-lookup"><span data-stu-id="4339d-123">The following options are available for the `libman init` command:</span></span>

* `-d|--default-destination <PATH>`

  <span data-ttu-id="4339d-124">Путь по отношению к текущей папке.</span><span class="sxs-lookup"><span data-stu-id="4339d-124">A path relative to the current folder.</span></span> <span data-ttu-id="4339d-125">Файлы библиотек устанавливаются в этом расположении, если не `destination` свойство определено для библиотеки в *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="4339d-125">Library files are installed in this location if no `destination` property is defined for a library in *libman.json*.</span></span> <span data-ttu-id="4339d-126">`<PATH>` Записывается значение `defaultDestination` свойство *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="4339d-126">The `<PATH>` value is written to the `defaultDestination` property of *libman.json*.</span></span>

* `-p|--default-provider <PROVIDER>`

  <span data-ttu-id="4339d-127">Поставщик, используемый, если поставщик не определен для данной библиотеки.</span><span class="sxs-lookup"><span data-stu-id="4339d-127">The provider to use if no provider is defined for a given library.</span></span> <span data-ttu-id="4339d-128">`<PROVIDER>` Записывается значение `defaultProvider` свойство *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="4339d-128">The `<PROVIDER>` value is written to the `defaultProvider` property of *libman.json*.</span></span> <span data-ttu-id="4339d-129">Замените `<PROVIDER>` с одним из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="4339d-129">Replace `<PROVIDER>` with one of the following values:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="4339d-130">Примеры</span><span class="sxs-lookup"><span data-stu-id="4339d-130">Examples</span></span>

<span data-ttu-id="4339d-131">Чтобы создать *libman.json* файл в проекте ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="4339d-131">To create a *libman.json* file in an ASP.NET Core project:</span></span>

* <span data-ttu-id="4339d-132">Перейдите в корневую папку проекта.</span><span class="sxs-lookup"><span data-stu-id="4339d-132">Navigate to the project root.</span></span>
* <span data-ttu-id="4339d-133">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="4339d-133">Run the following command:</span></span>

  ```console
  libman init
  ```

* <span data-ttu-id="4339d-134">Введите имя поставщика по умолчанию, или нажмите клавишу `Enter` использование CDNJS поставщика по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="4339d-134">Type the name of the default provider, or press `Enter` to use the default CDNJS provider.</span></span> <span data-ttu-id="4339d-135">Допустимы следующие значения:</span><span class="sxs-lookup"><span data-stu-id="4339d-135">Valid values include:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![Команда libman init — поставщик по умолчанию](_static/libman-init-provider.png)

<span data-ttu-id="4339d-137">Объект *libman.json* файл добавляется в корень проекта со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="4339d-137">A *libman.json* file is added to the project root with the following content:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a><span data-ttu-id="4339d-138">Добавьте файлы библиотеки</span><span class="sxs-lookup"><span data-stu-id="4339d-138">Add library files</span></span>

<span data-ttu-id="4339d-139">`libman install` Команда загружает и устанавливает файлы библиотеки в проект.</span><span class="sxs-lookup"><span data-stu-id="4339d-139">The `libman install` command downloads and installs library files into the project.</span></span> <span data-ttu-id="4339d-140">Объект *libman.json* файл добавляется в том случае, если она не существует.</span><span class="sxs-lookup"><span data-stu-id="4339d-140">A *libman.json* file is added if one doesn't exist.</span></span> <span data-ttu-id="4339d-141">*Libman.json* файл будет изменен для хранения сведений о конфигурации для файлов библиотек.</span><span class="sxs-lookup"><span data-stu-id="4339d-141">The *libman.json* file is modified to store configuration details for the library files.</span></span>

### <a name="synopsis"></a><span data-ttu-id="4339d-142">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="4339d-142">Synopsis</span></span>

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="4339d-143">Аргументы</span><span class="sxs-lookup"><span data-stu-id="4339d-143">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="4339d-144">Имя библиотеки для установки.</span><span class="sxs-lookup"><span data-stu-id="4339d-144">The name of the library to install.</span></span> <span data-ttu-id="4339d-145">Это имя может содержать число нотации версии (например, `@1.2.0`).</span><span class="sxs-lookup"><span data-stu-id="4339d-145">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="4339d-146">Параметры</span><span class="sxs-lookup"><span data-stu-id="4339d-146">Options</span></span>

<span data-ttu-id="4339d-147">Следующие параметры доступны для `libman install` команды:</span><span class="sxs-lookup"><span data-stu-id="4339d-147">The following options are available for the `libman install` command:</span></span>

* `-d|--destination <PATH>`

  <span data-ttu-id="4339d-148">Расположение, чтобы установить библиотеку.</span><span class="sxs-lookup"><span data-stu-id="4339d-148">The location to install the library.</span></span> <span data-ttu-id="4339d-149">Если не указан, используется расположение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="4339d-149">If not specified, the default location is used.</span></span> <span data-ttu-id="4339d-150">Если не `defaultDestination` свойство, указанное в *libman.json*, этот параметр является обязательным.</span><span class="sxs-lookup"><span data-stu-id="4339d-150">If no `defaultDestination` property is specified in *libman.json*, this option is required.</span></span>

* `--files <FILE>`

  <span data-ttu-id="4339d-151">Укажите имя файла для установки из библиотеки.</span><span class="sxs-lookup"><span data-stu-id="4339d-151">Specify the name of the file to install from the library.</span></span> <span data-ttu-id="4339d-152">Если не указан, устанавливаются все файлы из библиотеки.</span><span class="sxs-lookup"><span data-stu-id="4339d-152">If not specified, all files from the library are installed.</span></span> <span data-ttu-id="4339d-153">Укажите один `--files` параметр каждого файла должен быть установлен.</span><span class="sxs-lookup"><span data-stu-id="4339d-153">Provide one `--files` option per file to be installed.</span></span> <span data-ttu-id="4339d-154">Относительные пути поддерживаются слишком.</span><span class="sxs-lookup"><span data-stu-id="4339d-154">Relative paths are supported too.</span></span> <span data-ttu-id="4339d-155">Например, `--files dist/browser/signalr.js`.</span><span class="sxs-lookup"><span data-stu-id="4339d-155">For example: `--files dist/browser/signalr.js`.</span></span>

* `-p|--provider <PROVIDER>`

  <span data-ttu-id="4339d-156">Имя поставщика, используемого для получения библиотеки.</span><span class="sxs-lookup"><span data-stu-id="4339d-156">The name of the provider to use for the library acquisition.</span></span> <span data-ttu-id="4339d-157">Замените `<PROVIDER>` с одним из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="4339d-157">Replace `<PROVIDER>` with one of the following values:</span></span>
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  <span data-ttu-id="4339d-158">Если не указан, `defaultProvider` свойство в *libman.json* используется.</span><span class="sxs-lookup"><span data-stu-id="4339d-158">If not specified, the `defaultProvider` property in *libman.json* is used.</span></span> <span data-ttu-id="4339d-159">Если не `defaultProvider` свойство, указанное в *libman.json*, этот параметр является обязательным.</span><span class="sxs-lookup"><span data-stu-id="4339d-159">If no `defaultProvider` property is specified in *libman.json*, this option is required.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="4339d-160">Примеры</span><span class="sxs-lookup"><span data-stu-id="4339d-160">Examples</span></span>

<span data-ttu-id="4339d-161">Рассмотрим следующий *libman.json* файла:</span><span class="sxs-lookup"><span data-stu-id="4339d-161">Consider the following *libman.json* file:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

<span data-ttu-id="4339d-162">Для установки версии jQuery 3.2.1 *jquery.min.js* файл *wwwroot/сценарии/jquery* папки с помощью поставщика CDNJS:</span><span class="sxs-lookup"><span data-stu-id="4339d-162">To install the jQuery version 3.2.1 *jquery.min.js* file to the *wwwroot/scripts/jquery* folder using the CDNJS provider:</span></span>

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot/scripts/jquery --files jquery.min.js
```

<span data-ttu-id="4339d-163">*Libman.json* файла имеет следующий вид:</span><span class="sxs-lookup"><span data-stu-id="4339d-163">The *libman.json* file resembles the following:</span></span>

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

<span data-ttu-id="4339d-164">Чтобы установить *calendar.js* и *calendar.css* файлов из *C:\\temp\\contosoCalendar\\*  в файловой системе Поставщик:</span><span class="sxs-lookup"><span data-stu-id="4339d-164">To install the *calendar.js* and *calendar.css* files from *C:\\temp\\contosoCalendar\\* using the file system provider:</span></span>

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

<span data-ttu-id="4339d-165">Появится следующий запрос по двум причинам:</span><span class="sxs-lookup"><span data-stu-id="4339d-165">The following prompt appears for two reasons:</span></span>

* <span data-ttu-id="4339d-166">*Libman.json* файл не содержит `defaultDestination` свойство.</span><span class="sxs-lookup"><span data-stu-id="4339d-166">The *libman.json* file doesn't contain a `defaultDestination` property.</span></span>
* <span data-ttu-id="4339d-167">`libman install` Команда не содержит `-d|--destination` параметр.</span><span class="sxs-lookup"><span data-stu-id="4339d-167">The `libman install` command doesn't contain the `-d|--destination` option.</span></span>

![Команда — установить libman назначения](_static/libman-install-destination.png)

<span data-ttu-id="4339d-169">После принятия назначение по умолчанию, *libman.json* файла имеет следующий вид:</span><span class="sxs-lookup"><span data-stu-id="4339d-169">After accepting the default destination, the *libman.json* file resembles the following:</span></span>

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

## <a name="restore-library-files"></a><span data-ttu-id="4339d-170">Восстановить файлы библиотеки</span><span class="sxs-lookup"><span data-stu-id="4339d-170">Restore library files</span></span>

<span data-ttu-id="4339d-171">`libman restore` Команда устанавливает файлы библиотеки, определенные в *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="4339d-171">The `libman restore` command installs library files defined in *libman.json*.</span></span> <span data-ttu-id="4339d-172">Действуют следующие правила.</span><span class="sxs-lookup"><span data-stu-id="4339d-172">The following rules apply:</span></span>

* <span data-ttu-id="4339d-173">Если не *libman.json* файл находится в корневом каталоге проекта, будет возвращена ошибка.</span><span class="sxs-lookup"><span data-stu-id="4339d-173">If no *libman.json* file exists in the project root, an error is returned.</span></span>
* <span data-ttu-id="4339d-174">Если библиотеку указывает поставщика, `defaultProvider` свойство в *libman.json* учитывается.</span><span class="sxs-lookup"><span data-stu-id="4339d-174">If a library specifies a provider, the `defaultProvider` property in *libman.json* is ignored.</span></span>
* <span data-ttu-id="4339d-175">Если библиотека определяет назначение, `defaultDestination` свойство в *libman.json* учитывается.</span><span class="sxs-lookup"><span data-stu-id="4339d-175">If a library specifies a destination, the `defaultDestination` property in *libman.json* is ignored.</span></span>

### <a name="synopsis"></a><span data-ttu-id="4339d-176">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="4339d-176">Synopsis</span></span>

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a><span data-ttu-id="4339d-177">Параметры</span><span class="sxs-lookup"><span data-stu-id="4339d-177">Options</span></span>

<span data-ttu-id="4339d-178">Следующие параметры доступны для `libman restore` команды:</span><span class="sxs-lookup"><span data-stu-id="4339d-178">The following options are available for the `libman restore` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="4339d-179">Примеры</span><span class="sxs-lookup"><span data-stu-id="4339d-179">Examples</span></span>

<span data-ttu-id="4339d-180">Чтобы восстановить файлы библиотеки, определенные в *libman.json*:</span><span class="sxs-lookup"><span data-stu-id="4339d-180">To restore the library files defined in *libman.json*:</span></span>

```console
libman restore
```

## <a name="delete-library-files"></a><span data-ttu-id="4339d-181">Удалить файлы библиотеки</span><span class="sxs-lookup"><span data-stu-id="4339d-181">Delete library files</span></span>

<span data-ttu-id="4339d-182">`libman clean` Команда удаляет файлы библиотек, ранее восстанавливается при LibMan.</span><span class="sxs-lookup"><span data-stu-id="4339d-182">The `libman clean` command deletes library files previously restored via LibMan.</span></span> <span data-ttu-id="4339d-183">Папки, которые становятся пустыми после выполнения этой операции будут удалены.</span><span class="sxs-lookup"><span data-stu-id="4339d-183">Folders that become empty after this operation are deleted.</span></span> <span data-ttu-id="4339d-184">Файлы библиотеки связанный конфигураций в `libraries` свойство *libman.json* не удаляются.</span><span class="sxs-lookup"><span data-stu-id="4339d-184">The library files' associated configurations in the `libraries` property of *libman.json* aren't removed.</span></span>

### <a name="synopsis"></a><span data-ttu-id="4339d-185">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="4339d-185">Synopsis</span></span>

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a><span data-ttu-id="4339d-186">Параметры</span><span class="sxs-lookup"><span data-stu-id="4339d-186">Options</span></span>

<span data-ttu-id="4339d-187">Следующие параметры доступны для `libman clean` команды:</span><span class="sxs-lookup"><span data-stu-id="4339d-187">The following options are available for the `libman clean` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="4339d-188">Примеры</span><span class="sxs-lookup"><span data-stu-id="4339d-188">Examples</span></span>

<span data-ttu-id="4339d-189">Чтобы удалить файлы библиотек, установленных с помощью LibMan:</span><span class="sxs-lookup"><span data-stu-id="4339d-189">To delete library files installed via LibMan:</span></span>

```console
libman clean
```

## <a name="uninstall-library-files"></a><span data-ttu-id="4339d-190">Удалите файлы библиотеки</span><span class="sxs-lookup"><span data-stu-id="4339d-190">Uninstall library files</span></span>

<span data-ttu-id="4339d-191">`libman uninstall` Команды:</span><span class="sxs-lookup"><span data-stu-id="4339d-191">The `libman uninstall` command:</span></span>

* <span data-ttu-id="4339d-192">Удаляет все файлы, связанные с указанной библиотеки из назначения в *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="4339d-192">Deletes all files associated with the specified library from the destination in *libman.json*.</span></span>
* <span data-ttu-id="4339d-193">Удаляет конфигурацию связанные библиотеки из *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="4339d-193">Removes the associated library configuration from *libman.json*.</span></span>

<span data-ttu-id="4339d-194">Произошла ошибка при:</span><span class="sxs-lookup"><span data-stu-id="4339d-194">An error occurs when:</span></span>

* <span data-ttu-id="4339d-195">Не *libman.json* файл существует в корневом каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="4339d-195">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="4339d-196">Указанная библиотека не существует.</span><span class="sxs-lookup"><span data-stu-id="4339d-196">The specified library doesn't exist.</span></span>

<span data-ttu-id="4339d-197">Если установлено более одной библиотеки с тем же именем, будет предложено выбрать одну.</span><span class="sxs-lookup"><span data-stu-id="4339d-197">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="4339d-198">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="4339d-198">Synopsis</span></span>

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="4339d-199">Аргументы</span><span class="sxs-lookup"><span data-stu-id="4339d-199">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="4339d-200">Имя библиотеки для удаления.</span><span class="sxs-lookup"><span data-stu-id="4339d-200">The name of the library to uninstall.</span></span> <span data-ttu-id="4339d-201">Это имя может содержать число нотации версии (например, `@1.2.0`).</span><span class="sxs-lookup"><span data-stu-id="4339d-201">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="4339d-202">Параметры</span><span class="sxs-lookup"><span data-stu-id="4339d-202">Options</span></span>

<span data-ttu-id="4339d-203">Следующие параметры доступны для `libman uninstall` команды:</span><span class="sxs-lookup"><span data-stu-id="4339d-203">The following options are available for the `libman uninstall` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="4339d-204">Примеры</span><span class="sxs-lookup"><span data-stu-id="4339d-204">Examples</span></span>

<span data-ttu-id="4339d-205">Рассмотрим следующий *libman.json* файла:</span><span class="sxs-lookup"><span data-stu-id="4339d-205">Consider the following *libman.json* file:</span></span>

[!code-json[](samples/LibManSample/libman.json)]

* <span data-ttu-id="4339d-206">Чтобы удалить jQuery, либо из следующих команд выполняются успешно:</span><span class="sxs-lookup"><span data-stu-id="4339d-206">To uninstall jQuery, either of the following commands succeed:</span></span>

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* <span data-ttu-id="4339d-207">Для удаления Lodash файлы, установленные с помощью `filesystem` поставщика:</span><span class="sxs-lookup"><span data-stu-id="4339d-207">To uninstall the Lodash files installed via the `filesystem` provider:</span></span>

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a><span data-ttu-id="4339d-208">Обновить версию библиотеки</span><span class="sxs-lookup"><span data-stu-id="4339d-208">Update library version</span></span>

<span data-ttu-id="4339d-209">`libman update` Команда обновляет библиотеку, установленных с помощью LibMan заданной версии.</span><span class="sxs-lookup"><span data-stu-id="4339d-209">The `libman update` command updates a library installed via LibMan to the specified version.</span></span>

<span data-ttu-id="4339d-210">Произошла ошибка при:</span><span class="sxs-lookup"><span data-stu-id="4339d-210">An error occurs when:</span></span>

* <span data-ttu-id="4339d-211">Не *libman.json* файл существует в корневом каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="4339d-211">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="4339d-212">Указанная библиотека не существует.</span><span class="sxs-lookup"><span data-stu-id="4339d-212">The specified library doesn't exist.</span></span>

<span data-ttu-id="4339d-213">Если установлено более одной библиотеки с тем же именем, будет предложено выбрать одну.</span><span class="sxs-lookup"><span data-stu-id="4339d-213">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="4339d-214">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="4339d-214">Synopsis</span></span>

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="4339d-215">Аргументы</span><span class="sxs-lookup"><span data-stu-id="4339d-215">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="4339d-216">Имя библиотеки для обновления.</span><span class="sxs-lookup"><span data-stu-id="4339d-216">The name of the library to update.</span></span>

### <a name="options"></a><span data-ttu-id="4339d-217">Параметры</span><span class="sxs-lookup"><span data-stu-id="4339d-217">Options</span></span>

<span data-ttu-id="4339d-218">Следующие параметры доступны для `libman update` команды:</span><span class="sxs-lookup"><span data-stu-id="4339d-218">The following options are available for the `libman update` command:</span></span>

* `-pre`

  <span data-ttu-id="4339d-219">Получите последнюю предварительную версию библиотеки.</span><span class="sxs-lookup"><span data-stu-id="4339d-219">Obtain the latest prerelease version of the library.</span></span>

* `--to <VERSION>`

  <span data-ttu-id="4339d-220">Получите определенную версию библиотеки.</span><span class="sxs-lookup"><span data-stu-id="4339d-220">Obtain a specific version of the library.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="4339d-221">Примеры</span><span class="sxs-lookup"><span data-stu-id="4339d-221">Examples</span></span>

* <span data-ttu-id="4339d-222">Чтобы обновить jQuery до последней версии:</span><span class="sxs-lookup"><span data-stu-id="4339d-222">To update jQuery to the latest version:</span></span>

  ```console
  libman update jquery
  ```

* <span data-ttu-id="4339d-223">Чтобы обновить jQuery до версии 3.3.1:</span><span class="sxs-lookup"><span data-stu-id="4339d-223">To update jQuery to version 3.3.1:</span></span>

  ```console
  libman update jquery --to 3.3.1
  ```

* <span data-ttu-id="4339d-224">Чтобы обновить jQuery до последней предварительной версии:</span><span class="sxs-lookup"><span data-stu-id="4339d-224">To update jQuery to the latest prerelease version:</span></span>

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a><span data-ttu-id="4339d-225">Управление кэшем библиотеки</span><span class="sxs-lookup"><span data-stu-id="4339d-225">Manage library cache</span></span>

<span data-ttu-id="4339d-226">`libman cache` Команда управляет кэшем LibMan библиотеки.</span><span class="sxs-lookup"><span data-stu-id="4339d-226">The `libman cache` command manages the LibMan library cache.</span></span> <span data-ttu-id="4339d-227">`filesystem` Поставщик не использует кэш библиотеки.</span><span class="sxs-lookup"><span data-stu-id="4339d-227">The `filesystem` provider doesn't use the library cache.</span></span>

### <a name="synopsis"></a><span data-ttu-id="4339d-228">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="4339d-228">Synopsis</span></span>

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="4339d-229">Аргументы</span><span class="sxs-lookup"><span data-stu-id="4339d-229">Arguments</span></span>

`PROVIDER`

<span data-ttu-id="4339d-230">Использовать только с `clean` команды.</span><span class="sxs-lookup"><span data-stu-id="4339d-230">Only used with the `clean` command.</span></span> <span data-ttu-id="4339d-231">Задает кэш поставщика для очистки.</span><span class="sxs-lookup"><span data-stu-id="4339d-231">Specifies the provider cache to clean.</span></span> <span data-ttu-id="4339d-232">Допустимы следующие значения:</span><span class="sxs-lookup"><span data-stu-id="4339d-232">Valid values include:</span></span>

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a><span data-ttu-id="4339d-233">Параметры</span><span class="sxs-lookup"><span data-stu-id="4339d-233">Options</span></span>

<span data-ttu-id="4339d-234">Следующие параметры доступны для `libman cache` команды:</span><span class="sxs-lookup"><span data-stu-id="4339d-234">The following options are available for the `libman cache` command:</span></span>

* `--files`

  <span data-ttu-id="4339d-235">Список имен файлов, которые кэшируются.</span><span class="sxs-lookup"><span data-stu-id="4339d-235">List the names of files that are cached.</span></span>

* `--libraries`

  <span data-ttu-id="4339d-236">Список имен библиотек, которые кэшируются.</span><span class="sxs-lookup"><span data-stu-id="4339d-236">List the names of libraries that are cached.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="4339d-237">Примеры</span><span class="sxs-lookup"><span data-stu-id="4339d-237">Examples</span></span>

* <span data-ttu-id="4339d-238">Чтобы просмотреть имена библиотек кэшированных каждого поставщика, используйте один из следующих команд:</span><span class="sxs-lookup"><span data-stu-id="4339d-238">To view the names of cached libraries per provider, use one of the following commands:</span></span>

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  <span data-ttu-id="4339d-239">Выходные данные должны выглядеть примерно так:</span><span class="sxs-lookup"><span data-stu-id="4339d-239">Output similar to the following is displayed:</span></span>

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

* <span data-ttu-id="4339d-240">Чтобы увидеть имена всех кэшированных файлов библиотеки каждого поставщика:</span><span class="sxs-lookup"><span data-stu-id="4339d-240">To view the names of cached library files per provider:</span></span>

  ```console
  libman cache list --files
  ```

  <span data-ttu-id="4339d-241">Выходные данные должны выглядеть примерно так:</span><span class="sxs-lookup"><span data-stu-id="4339d-241">Output similar to the following is displayed:</span></span>

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

  <span data-ttu-id="4339d-242">Обратите внимание, что предыдущих выходных данных показано jQuery, версии 3.2.1 и 3.3.1 кэшируются при использовании поставщика CDNJS.</span><span class="sxs-lookup"><span data-stu-id="4339d-242">Notice the preceding output shows that jQuery versions 3.2.1 and 3.3.1 are cached under the CDNJS provider.</span></span>

* <span data-ttu-id="4339d-243">Чтобы очистить кэш библиотеки для поставщика CDNJS:</span><span class="sxs-lookup"><span data-stu-id="4339d-243">To empty the library cache for the CDNJS provider:</span></span>

  ```console
  libman cache clean cdnjs
  ```

  <span data-ttu-id="4339d-244">После очистки кэша поставщика, который CDNJS, `libman cache list` команда отображает следующее:</span><span class="sxs-lookup"><span data-stu-id="4339d-244">After emptying the CDNJS provider cache, the `libman cache list` command displays the following:</span></span>

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

* <span data-ttu-id="4339d-245">Чтобы очистить кэш для всех поддерживаемых поставщиков:</span><span class="sxs-lookup"><span data-stu-id="4339d-245">To empty the cache for all supported providers:</span></span>

  ```console
  libman cache clean
  ```

  <span data-ttu-id="4339d-246">После очистки всех кэшей поставщика `libman cache list` команда отображает следующее:</span><span class="sxs-lookup"><span data-stu-id="4339d-246">After emptying all provider caches, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a><span data-ttu-id="4339d-247">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="4339d-247">Additional resources</span></span>

* [<span data-ttu-id="4339d-248">Установка глобального средства</span><span class="sxs-lookup"><span data-stu-id="4339d-248">Install a Global Tool</span></span>](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [<span data-ttu-id="4339d-249">Репозиторий LibMan на GitHub</span><span class="sxs-lookup"><span data-stu-id="4339d-249">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
