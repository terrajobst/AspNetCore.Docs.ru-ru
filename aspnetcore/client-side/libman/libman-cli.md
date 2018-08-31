---
title: Использование LibMan командной строки (CLI) с ASP.NET Core
author: scottaddie
description: Сведения об использовании LibMan командной строки (CLI) в проекте ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/30/2018
uid: client-side/libman/libman-cli
ms.openlocfilehash: ad81af2e789a31382f50ed37754bfc94469eb197
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336041"
---
# <a name="use-the-libman-command-line-interface-cli-with-aspnet-core"></a>Использование LibMan командной строки (CLI) с ASP.NET Core

Автор: [Скотт Адди](https://twitter.com/Scott_Addie) (Scott Addie)

[LibMan](xref:client-side/libman/index) CLI — это средство кросс платформенных, имеющий поддерживаемые everywhere .NET Core поддерживается.

## <a name="prerequisites"></a>Предварительные требования

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a>Установка

Чтобы установить LibMan CLI:

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

Объект [глобальное средство .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) устанавливается из [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) пакет NuGet.

Чтобы установить LibMan CLI из определенного источника пакета NuGet:

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

В предыдущем примере, это средство глобального .NET Core устанавливается с локального компьютера Windows *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* файла.

## <a name="usage"></a>Использование

После успешной установки интерфейса командной строки можно использовать следующую команду:

```console
libman
```

Чтобы просмотреть установленную версию интерфейса командной строки:

```console
libman --version
```

Для просмотра доступных команд интерфейса командной строки:

```console
libman --help
```

Приведенная выше команда отображает результат, аналогичный приведенному ниже:

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

В следующих разделах описываются доступные команды интерфейса командной строки.

## <a name="initialize-libman-in-the-project"></a>Инициализировать LibMan в проекте

`libman init` Команда создает *libman.json* файла, если она не существует. Файл создан с содержимым шаблона элемента по умолчанию.

### <a name="synopsis"></a>Краткий обзор

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a>Параметры

Следующие параметры доступны для `libman init` команды:

* `-d|--default-destination <PATH>`

  Путь по отношению к текущей папке. Файлы библиотек устанавливаются в этом расположении, если не `destination` свойство определено для библиотеки в *libman.json*. `<PATH>` Записывается значение `defaultDestination` свойство *libman.json*.

* `-p|--default-provider <PROVIDER>`

  Поставщик, используемый, если поставщик не определен для данной библиотеки. `<PROVIDER>` Записывается значение `defaultProvider` свойство *libman.json*. Замените `<PROVIDER>` с одним из следующих значений:

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Примеры

Чтобы создать *libman.json* файл в проекте ASP.NET Core:

* Перейдите в корневую папку проекта.
* Выполните следующую команду:

  ```console
  libman init
  ```

* Введите имя поставщика по умолчанию, или нажмите клавишу `Enter` использование CDNJS поставщика по умолчанию. Допустимы следующие значения:

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![Команда libman init — поставщик по умолчанию](_static/libman-init-provider.png)

Объект *libman.json* файл добавляется в корень проекта со следующим содержимым:

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a>Добавьте файлы библиотеки

`libman install` Команда загружает и устанавливает файлы библиотеки в проект. Объект *libman.json* файл добавляется в том случае, если она не существует. *Libman.json* файл будет изменен для хранения сведений о конфигурации для файлов библиотек.

### <a name="synopsis"></a>Краткий обзор

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a>Аргументы

`LIBRARY`

Имя библиотеки для установки. Это имя может содержать число нотации версии (например, `@1.2.0`).

### <a name="options"></a>Параметры

Следующие параметры доступны для `libman install` команды:

* `-d|--destination <PATH>`

  Расположение, чтобы установить библиотеку. Если не указан, используется расположение по умолчанию. Если не `defaultDestination` свойство, указанное в *libman.json*, этот параметр является обязательным.

* `--files <FILE>`

  Укажите имя файла для установки из библиотеки. Если не указан, устанавливаются все файлы из библиотеки. Укажите один `--files` параметр каждого файла должен быть установлен. Относительные пути поддерживаются слишком. Например, `--files dist/browser/signalr.js`.

* `-p|--provider <PROVIDER>`

  Имя поставщика, используемого для получения библиотеки. Замените `<PROVIDER>` с одним из следующих значений:
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  Если не указан, `defaultProvider` свойство в *libman.json* используется. Если не `defaultProvider` свойство, указанное в *libman.json*, этот параметр является обязательным.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Примеры

Рассмотрим следующий *libman.json* файла:

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

Для установки версии jQuery 3.2.1 *jquery.min.js* файл *wwwroot\scripts\jquery* папки с помощью поставщика CDNJS:

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot\scripts\jquery --files jquery.min.js
```

*Libman.json* файла имеет следующий вид:

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot\\scripts\\jquery",
      "files": [
        "jquery.min.js"
      ]
    }
  ]
}
```

Чтобы установить *calendar.js* и *calendar.css* файлов из *C:\\temp\\contosoCalendar\\*  в файловой системе Поставщик:

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

Появится следующий запрос по двум причинам:

* *Libman.json* файл не содержит `defaultDestination` свойство.
* `libman install` Команда не содержит `-d|--destination` параметр.

![Команда — установить libman назначения](_static/libman-install-destination.png)

После принятия назначение по умолчанию, *libman.json* файла имеет следующий вид:

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot\\scripts\\jquery",
      "files": [
        "jquery.min.js"
      ]
    },
    {
      "library": "C:\\temp\\contosoCalendar\\",
      "provider": "filesystem",
      "destination": "wwwroot\\lib\\contosoCalendar",
      "files": [
        "calendar.js",
        "calendar.css"
      ]
    }
  ]
}
```

## <a name="restore-library-files"></a>Восстановить файлы библиотеки

`libman restore` Команда устанавливает файлы библиотеки, определенные в *libman.json*. Действуют следующие правила.

* Если не *libman.json* файл находится в корневом каталоге проекта, будет возвращена ошибка.
* Если библиотеку указывает поставщика, `defaultProvider` свойство в *libman.json* учитывается.
* Если библиотека определяет назначение, `defaultDestination` свойство в *libman.json* учитывается.

### <a name="synopsis"></a>Краткий обзор

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a>Параметры

Следующие параметры доступны для `libman restore` команды:

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Примеры

Чтобы восстановить файлы библиотеки, определенные в *libman.json*:

```console
libman restore
```

## <a name="delete-library-files"></a>Удалить файлы библиотеки

`libman clean` Команда удаляет файлы библиотек, ранее восстанавливается при LibMan. Папки, которые становятся пустыми после выполнения этой операции будут удалены. Файлы библиотеки связанный конфигураций в `libraries` свойство *libman.json* не удаляются.

### <a name="synopsis"></a>Краткий обзор

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a>Параметры

Следующие параметры доступны для `libman clean` команды:

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Примеры

Чтобы удалить файлы библиотек, установленных с помощью LibMan:

```console
libman clean
```

## <a name="uninstall-library-files"></a>Удалите файлы библиотеки

`libman uninstall` Команды:

* Удаляет все файлы, связанные с указанной библиотеки из назначения в *libman.json*.
* Удаляет конфигурацию связанные библиотеки из *libman.json*.

Произошла ошибка при:

* Не *libman.json* файл существует в корневом каталоге проекта.
* Указанная библиотека не существует.

Если установлено более одной библиотеки с тем же именем, будет предложено выбрать одну.

### <a name="synopsis"></a>Краткий обзор

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a>Аргументы

`LIBRARY`

Имя библиотеки для удаления. Это имя может содержать число нотации версии (например, `@1.2.0`).

### <a name="options"></a>Параметры

Следующие параметры доступны для `libman uninstall` команды:

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Примеры

Рассмотрим следующий *libman.json* файла:

[!code-json[](samples/LibManSample/libman.json)]

* Чтобы удалить jQuery, либо из следующих команд выполняются успешно:

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* Для удаления Lodash файлы, установленные с помощью `filesystem` поставщика:

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a>Обновить версию библиотеки

`libman update` Команда обновляет библиотеку, установленных с помощью LibMan заданной версии.

Произошла ошибка при:

* Не *libman.json* файл существует в корневом каталоге проекта.
* Указанная библиотека не существует.

Если установлено более одной библиотеки с тем же именем, будет предложено выбрать одну.

### <a name="synopsis"></a>Краткий обзор

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a>Аргументы

`LIBRARY`

Имя библиотеки для обновления.

### <a name="options"></a>Параметры

Следующие параметры доступны для `libman update` команды:

* `-pre`

  Получите последнюю предварительную версию библиотеки.

* `--to <VERSION>`

  Получите определенную версию библиотеки.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Примеры

* Чтобы обновить jQuery до последней версии:

  ```console
  libman update jquery
  ```

* Чтобы обновить jQuery до версии 3.3.1:

  ```console
  libman update jquery --to 3.3.1
  ```

* Чтобы обновить jQuery до последней предварительной версии:

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a>Управление кэшем библиотеки

`libman cache` Команда управляет кэшем LibMan библиотеки. `filesystem` Поставщик не использует кэш библиотеки.

### <a name="synopsis"></a>Краткий обзор

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a>Аргументы

`PROVIDER`

Использовать только с `clean` команды. Задает кэш поставщика для очистки. Допустимы следующие значения:

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a>Параметры

Следующие параметры доступны для `libman cache` команды:

* `--files`

  Список имен файлов, которые кэшируются.

* `--libraries`

  Список имен библиотек, которые кэшируются.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Примеры

* Чтобы просмотреть имена библиотек кэшированных каждого поставщика, используйте один из следующих команд:

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  Выходные данные должны выглядеть примерно так:

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

* Чтобы увидеть имена всех кэшированных файлов библиотеки каждого поставщика:

  ```console
  libman cache list --files
  ```

  Выходные данные должны выглядеть примерно так:

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

  Обратите внимание, что предыдущих выходных данных показано jQuery, версии 3.2.1 и 3.3.1 кэшируются при использовании поставщика CDNJS.

* Чтобы очистить кэш библиотеки для поставщика CDNJS:

  ```console
  libman cache clean cdnjs
  ```

  После очистки кэша поставщика, который CDNJS, `libman cache list` команда отображает следующее:

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

* Чтобы очистить кэш для всех поддерживаемых поставщиков:

  ```console
  libman cache clean
  ```

  После очистки всех кэшей поставщика `libman cache list` команда отображает следующее:

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Установка глобального средства](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [Репозиторий LibMan на GitHub](https://github.com/aspnet/LibraryManager)
