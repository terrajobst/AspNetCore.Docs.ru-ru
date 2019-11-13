---
title: Использование интерфейса командной строки (CLI) Либман с ASP.NET Core
author: scottaddie
description: Узнайте, как использовать интерфейс командной строки Либман (CLI) в проекте ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: client-side/libman/libman-cli
ms.openlocfilehash: 8b2b1e45ab4685482554ac439b0276e0cf381609
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962804"
---
# <a name="use-the-libman-command-line-interface-cli-with-aspnet-core"></a>Использование интерфейса командной строки (CLI) Либман с ASP.NET Core

Автор: [Скотт Адди](https://twitter.com/Scott_Addie) (Scott Addie)

[Либман](xref:client-side/libman/index) CLI — это кросс-платформенный инструмент, поддерживаемый везде, где поддерживается .NET Core.

## <a name="prerequisites"></a>Необходимые компоненты

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a>Установка

Чтобы установить интерфейс командной строки Либман, выполните следующие действия.

```dotnetcli
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

[Глобальное средство .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) устанавливается из пакета NuGet [Microsoft. Web. либрариманажер. CLI](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) .

Чтобы установить Либман CLI из указанного источника пакета NuGet, выполните следующие действия.

```dotnetcli
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

В предыдущем примере глобальный инструмент .NET Core устанавливается из файла *C:\Temp\Microsoft.Web.LibraryManager.CLI.1.0.94-g606058a278.nupkg* локального компьютера Windows.

## <a name="usage"></a>Использование

После успешной установки интерфейса командной строки можно использовать следующую команду:

```console
libman
```

Чтобы просмотреть установленную версию CLI, выполните следующие действия.

```console
libman --version
```

Чтобы просмотреть доступные команды интерфейса командной строки:

```console
libman --help
```

Приведенная выше команда отображает выходные данные, аналогичные приведенным ниже.

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

Доступные команды CLI описываются в следующих разделах.

## <a name="initialize-libman-in-the-project"></a>Инициализация Либман в проекте

Команда `libman init` создает файл *Либман. JSON* , если он не существует. Файл создается с содержимым шаблона элемента по умолчанию.

### <a name="synopsis"></a>Краткий обзор

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a>Параметры

Для команды `libman init` доступны следующие параметры:

* `-d|--default-destination <PATH>`

  Путь относительно текущей папки. Файлы библиотеки устанавливаются в этом расположении, если для библиотеки в *Либман. JSON*не определено свойство `destination`. Значение `<PATH>` записывается в свойство `defaultDestination` *Либман. JSON*.

* `-p|--default-provider <PROVIDER>`

  Поставщик, используемый, если для данной библиотеки не определен поставщик. Значение `<PROVIDER>` записывается в свойство `defaultProvider` *Либман. JSON*. Замените `<PROVIDER>` одним из следующих значений:

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Примеры

Чтобы создать файл *Либман. JSON* в проекте ASP.NET Core, выполните следующие действия.

* Перейдите к корню проекта.
* Выполните следующую команду:

  ```console
  libman init
  ```

* Введите имя поставщика по умолчанию или нажмите `Enter`, чтобы использовать поставщик CDNJS по умолчанию. Допустимы следующие значения:

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![Либман init, команда-поставщик по умолчанию](_static/libman-init-provider.png)

Файл *Либман. JSON* добавляется в корень проекта со следующим содержимым:

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a>Добавление файлов библиотеки

Команда `libman install` скачивает и устанавливает файлы библиотеки в проект. Файл *Либман. JSON* добавляется, если он не существует. Файл *Либман. JSON* изменяется для хранения сведений о конфигурации для файлов библиотеки.

### <a name="synopsis"></a>Краткий обзор

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a>Аргументы

`LIBRARY`

Имя устанавливаемой библиотеки. Это имя может включать нотацию номера версии (например, `@1.2.0`).

### <a name="options"></a>Параметры

Для команды `libman install` доступны следующие параметры:

* `-d|--destination <PATH>`

  Расположение для установки библиотеки. Если значение не указано, используется расположение по умолчанию. Если в *Либман. JSON*не указано свойство `defaultDestination`, этот параметр является обязательным.

* `--files <FILE>`

  Укажите имя файла для установки из библиотеки. Если этот параметр не указан, устанавливаются все файлы из библиотеки. Укажите один параметр `--files` для каждого файла, который необходимо установить. Относительные пути также поддерживаются. Пример: `--files dist/browser/signalr.js`.

* `-p|--provider <PROVIDER>`

  Имя поставщика, используемого для получения библиотеки. Замените `<PROVIDER>` одним из следующих значений:
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  Если значение не указано, используется свойство `defaultProvider` в *Либман. JSON* . Если в *Либман. JSON*не указано свойство `defaultProvider`, этот параметр является обязательным.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Примеры

Рассмотрим следующий файл *Либман. JSON* :

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

Чтобы установить файл jQuery версии 3.2.1 *jQuery. min. js* в папку *wwwroot/Scripts/jQuery* с помощью поставщика CDNJS:

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot/scripts/jquery --files jquery.min.js
```

Файл *Либман. JSON* выглядит следующим образом:

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

Чтобы установить файлы *Calendar. js* и *Calendar. CSS* с *диска C:\\Temp\\контосокалендар\\* с помощью поставщика файловой системы:

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

Следующий запрос появляется по двум причинам:

* Файл *Либман. JSON* не содержит свойство `defaultDestination`.
* Команда `libman install` не содержит параметр `-d|--destination`.

![Команда установки Либман](_static/libman-install-destination.png)

После принятия назначения по умолчанию файл *Либман. JSON* будет выглядеть следующим образом:

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

## <a name="restore-library-files"></a>Восстановление файлов библиотеки

Команда `libman restore` устанавливает файлы библиотеки, определенные в *Либман. JSON*. Действуют следующие правила.

* Если в корне проекта не существует файла *Либман. JSON* , возвращается ошибка.
* Если в библиотеке указан поставщик, свойство `defaultProvider` в *Либман. JSON* игнорируется.
* Если в библиотеке указано назначение, свойство `defaultDestination` в *Либман. JSON* игнорируется.

### <a name="synopsis"></a>Краткий обзор

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a>Параметры

Для команды `libman restore` доступны следующие параметры:

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Примеры

Чтобы восстановить файлы библиотеки, определенные в *Либман. JSON*, сделайте следующее:

```console
libman restore
```

## <a name="delete-library-files"></a>Удаление файлов библиотеки

Команда `libman clean` удаляет файлы библиотеки, восстановленные ранее с помощью Либман. Папки, которые становятся пустыми после удаления этой операции. Конфигурации, связанные с файлами библиотеки, в свойстве `libraries` *Либман. JSON* не удаляются.

### <a name="synopsis"></a>Краткий обзор

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a>Параметры

Для команды `libman clean` доступны следующие параметры:

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Примеры

Удаление файлов библиотеки, установленных с помощью Либман:

```console
libman clean
```

## <a name="uninstall-library-files"></a>Удаление файлов библиотеки

Команда `libman uninstall`:

* Удаляет все файлы, связанные с указанной библиотекой, из места назначения в *Либман. JSON*.
* Удаляет связанную конфигурацию библиотеки из *Либман. JSON*.

Ошибка возникает в следующих случаях:

* В корне проекта не существует файла *Либман. JSON* .
* Указанная библиотека не существует.

Если установлено несколько библиотек с одним и тем же именем, вам будет предложено выбрать одну из них.

### <a name="synopsis"></a>Краткий обзор

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a>Аргументы

`LIBRARY`

Имя удаляемой библиотеки. Это имя может включать нотацию номера версии (например, `@1.2.0`).

### <a name="options"></a>Параметры

Для команды `libman uninstall` доступны следующие параметры:

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Примеры

Рассмотрим следующий файл *Либман. JSON* :

[!code-json[](samples/LibManSample/libman.json)]

* Чтобы удалить jQuery, выполните одну из следующих команд:

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* Чтобы удалить файлы Лодаш, установленные с помощью поставщика `filesystem`, выполните следующие действия.

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a>Обновление версии библиотеки

Команда `libman update` обновляет библиотеку, установленную с помощью Либман, до указанной версии.

Ошибка возникает в следующих случаях:

* В корне проекта не существует файла *Либман. JSON* .
* Указанная библиотека не существует.

Если установлено несколько библиотек с одним и тем же именем, вам будет предложено выбрать одну из них.

### <a name="synopsis"></a>Краткий обзор

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a>Аргументы

`LIBRARY`

Имя обновляемой библиотеки.

### <a name="options"></a>Параметры

Для команды `libman update` доступны следующие параметры:

* `-pre`

  Получите последнюю предварительную версию библиотеки.

* `--to <VERSION>`

  Получение определенной версии библиотеки.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Примеры

* Чтобы обновить jQuery до последней версии, выполните следующие действия.

  ```console
  libman update jquery
  ```

* Чтобы обновить jQuery до версии 3.3.1, выполните следующие действия.

  ```console
  libman update jquery --to 3.3.1
  ```

* Чтобы обновить jQuery до последней предварительной версии, выполните следующие действия.

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a>Управление кэшем библиотеки

Команда `libman cache` управляет кэшем библиотеки Либман. Поставщик `filesystem` не использует кэш библиотеки.

### <a name="synopsis"></a>Краткий обзор

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a>Аргументы

`PROVIDER`

Используется только с командой `clean`. Указывает кэш поставщика для очистки. Допустимы следующие значения:

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a>Параметры

Для команды `libman cache` доступны следующие параметры:

* `--files`

  Список имен файлов, которые кэшируются.

* `--libraries`

  Перечислите имена библиотек, которые кэшируются.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Примеры

* Чтобы просмотреть имена кэшированных библиотек для каждого поставщика, выполните одну из следующих команд:

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

* Чтобы просмотреть имена кэшированных файлов библиотеки для каждого поставщика:

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

  Обратите внимание, что предыдущие выходные данные показывают, что jQuery версии 3.2.1 и 3.3.1 кэшируются в поставщике CDNJS.

* Чтобы очистить кэш библиотеки для поставщика CDNJS, сделайте следующее:

  ```console
  libman cache clean cdnjs
  ```

  После очистки кэша поставщика CDNJS команда `libman cache list` выводит следующее:

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

* Чтобы очистить кэш для всех поддерживаемых поставщиков, выполните следующие действия.

  ```console
  libman cache clean
  ```

  После очистки всех кэшей поставщика команда `libman cache list` выводит следующее:

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
