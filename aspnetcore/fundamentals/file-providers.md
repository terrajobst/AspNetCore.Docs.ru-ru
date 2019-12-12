---
title: Поставщики файлов в ASP.NET Core
author: guardrex
description: Сведения о том, как ASP.NET Core абстрагирует доступ к файловой системе с помощью поставщиков файлов.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: fundamentals/file-providers
ms.openlocfilehash: a454ca394546184968222ca2ca44d7159b19a12a
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944312"
---
# <a name="file-providers-in-aspnet-core"></a>Поставщики файлов в ASP.NET Core

Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith) и [Люк Лэтем](https://github.com/guardrex) (Luke Latham)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core абстрагирует доступ к файловой системе с помощью поставщиков файлов. В рамках платформы ASP.NET Core используются поставщики файлов.

* `IWebHostEnvironment` предоставляет [корневой каталог содержимого](xref:fundamentals/index#content-root) и [корневой каталог документов](xref:fundamentals/index#web-root) приложения в виде типов `IFileProvider`.
* [ПО промежуточного слоя для статических файлов](xref:fundamentals/static-files) использует поставщики файлов для поиска статических файлов.
* [Razor](xref:mvc/views/razor) использует поставщики файлов для поиска страниц и представлений.
* Инструменты .NET Core используют поставщики файлов и стандартные маски, чтобы указать файлы для публикации.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="file-provider-interfaces"></a>Интерфейсы поставщика файлов

Основной интерфейс — <xref:Microsoft.Extensions.FileProviders.IFileProvider>. `IFileProvider` предоставляет методы для следующих действий:

* получение сведений о файле (<xref:Microsoft.Extensions.FileProviders.IFileInfo>);
* получение сведений о каталоге (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>);
* настройка уведомлений об изменениях (с помощью <xref:Microsoft.Extensions.Primitives.IChangeToken>).

`IFileInfo` предоставляет методы и свойства для работы с файлами:

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (в байтах);
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> (дата).

Данные из файла можно считывать с помощью метода [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*).

Пример приложения демонстрирует, как настроить в `Startup.ConfigureServices` поставщике файлов, чтобы использовать его в приложении путем [внедрения зависимостей](xref:fundamentals/dependency-injection).

## <a name="file-provider-implementations"></a>Реализации поставщиков файлов

Доступны три реализации `IFileProvider`.

| Реализация | ОПИСАНИЕ |
| -------------- | ----------- |
| [PhysicalFileProvider](#physicalfileprovider) | Физический поставщик используется для доступа к физическим файлам системы. |
| [ManifestEmbeddedFileProvider](#manifestembeddedfileprovider) | Поставщик внедренных манифестов используется для доступа к файлам, внедренным в сборки. |
| [CompositeFileProvider](#compositefileprovider) | Составной поставщик используется для предоставления комбинированного доступа к файлам и каталогам из одного или нескольких поставщиков. |

### <a name="physicalfileprovider"></a>PhysicalFileProvider

<xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> предоставляет доступ к физической файловой системе. `PhysicalFileProvider` использует тип <xref:System.IO.File?displayProperty=fullName> (для физического поставщика), ограничивая все пути каталогом и его дочерними элементами. Такая привязка к области защищает от доступа к файловой системе за пределами указанного каталога и его дочерних элементов. Самый распространенный подход к созданию и использованию `PhysicalFileProvider` — запросить `IFileProvider` в конструкторе путем [внедрения зависимостей](xref:fundamentals/dependency-injection).

При непосредственном создании экземпляра этого поставщика нужно указать путь к каталогу, который станет базовым путем для всех запросов, выполненных с помощью этого поставщика.

Следующий пример кода демонстрирует, как создать и использовать `PhysicalFileProvider` для получения содержимого каталога и сведений о файлах:

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

В предшествующем примере используются следующие типы:

* `provider` является `IFileProvider`.
* `contents` является `IDirectoryContents`.
* `fileInfo` является `IFileInfo`.

С помощью поставщика файлов вы можете выполнить итерацию по каталогу, указанному в параметре `applicationRoot`, или вызвать `GetFileInfo` для получения сведений о файлах. Поставщик файлов не имеет доступ к каталогам за пределами `applicationRoot`.

Этот пример приложения создает поставщик для приложения в классе `Startup.ConfigureServices`, используя [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a>ManifestEmbeddedFileProvider

<xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> используется для доступа к файлам, внедренным в сборки. `ManifestEmbeddedFileProvider` использует манифест, скомпилированный в сборку, для воссоздания исходных путей для внедренных файлов.

Добавьте ссылку на проект в пакет [Microsoft.Extensions.FileProviders.Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded).

Чтобы создать манифест для внедренных файлов, задайте для свойства `<GenerateEmbeddedFilesManifest>` значение `true`. Выберите файлы для внедрения с помощью [\<EmbeddedResource>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

Используйте [стандартные маски](#glob-patterns) для указания одного или нескольких файлов, которые вы хотите внедрить в сборку.

Наш пример приложения создает `ManifestEmbeddedFileProvider` и передает в соответствующий конструктор текущую выполняемую сборку.

*Startup.cs*:

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(typeof(Program).Assembly);
```

Дополнительные перегрузки позволяют сделать следующее:

* указать относительный путь к файлу;
* ограничить файлы по дате последнего изменения;
* указать имя внедренного ресурса, содержащего внедренный файл манифеста.

| Перегрузка | ОПИСАНИЕ |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | Принимает необязательный параметр `root` со значением относительного пути. Укажите `root`, чтобы ограничить вызовы к <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> только ресурсами по указанному пути. |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | Принимает необязательный параметр `root` со значением относительного пути и параметр `lastModified` со значением даты (<xref:System.DateTimeOffset>). Параметр `lastModified` ограничивает дату последнего изменения для экземпляров <xref:Microsoft.Extensions.FileProviders.IFileInfo>, возвращаемых функцией <xref:Microsoft.Extensions.FileProviders.IFileProvider>. |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | Принимает необязательный параметр `root` со значением относительного пути, дату `lastModified` и параметры `manifestName`. `manifestName` здесь представляет имя встроенного ресурса, содержащего манифест. |

### <a name="compositefileprovider"></a>CompositeFileProvider

<xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> объединяет экземпляры `IFileProvider`, предоставляя единый интерфейс для работы с файлами от нескольких поставщиков. При создании `CompositeFileProvider` передайте в соответствующий конструктор один или несколько экземпляров `IFileProvider`:

В нашем примере приложения `PhysicalFileProvider` и `ManifestEmbeddedFileProvider` предоставляют файлы для `CompositeFileProvider` с регистрацией в контейнере служб приложения:

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a>Отслеживание изменений

Метод [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) позволяет контролировать изменения в одном или нескольких файлах или каталогах. `Watch` принимает строку пути, которая можно использовать [стандартные маски](#glob-patterns) для указания нескольких файлов. `Watch` возвращает <xref:Microsoft.Extensions.Primitives.IChangeToken>. Этот токен изменений предоставляет следующие свойства:

* <xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> — свойство, которое можно проверить, чтобы определить, произошло ли изменение.
* <xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> — вызывается при обнаружении изменений в указанной строке пути. Каждый токен изменения выполняет соответствующий обратный вызов только в ответ на отдельное изменение. Чтобы реализовать непрерывный мониторинг, используйте <xref:System.Threading.Tasks.TaskCompletionSource`1>, как показано ниже, или повторно создавайте экземпляры `IChangeToken` в ответ на изменения.

В нашем примере консольное приложение *WatchConsole* будет отображать сообщение при изменении текстового файла:

[!code-csharp[](file-providers/samples/3.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Некоторые файловые системы, такие как контейнеры Docker и сетевые папки, не могут надежно отправлять уведомления об изменениях. Задайте для переменной среды `DOTNET_USE_POLLING_FILE_WATCHER` значение `1` или `true`, чтобы опрашивать файловую систему на предмет изменений каждые 4 секунды (это значение нельзя изменить).

## <a name="glob-patterns"></a>Стандартные маски

В путях файловой системы используются шаблоны подстановочных знаков, которые называются *стандартными масками*. Эти маски позволяют указывать группы файлов. Поддерживаются два подстановочных знака — `*` и `**`.

**`*`**  
Совпадает с любым элементом на текущем уровне папок, любым именем или расширением файла. Совпадения завершаются символами `/` и `.` в пути файла.

**`**`**  
Совпадает со всем содержимым на разных уровнях каталогов. Может использоваться для рекурсивного сопоставления множества файлов в иерархии каталогов.

**Примеры стандартных масок**

**`directory/file.txt`**  
Соответствует конкретному файлу в заданном каталоге.

**`directory/*.txt`**  
Соответствует всем файлам с расширением *.txt* в заданном каталоге.

**`directory/*/appsettings.json`**  
Соответствует всем файлам `appsettings.json` в любом каталоге, расположенном ровно на один уровень ниже каталога *directory*.

**`directory/**/*.txt`**  
Соответствует всем файлам с расширением *.txt*, находящимся на любом уровне ниже каталога *directory*.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core абстрагирует доступ к файловой системе с помощью поставщиков файлов. В рамках платформы ASP.NET Core используются поставщики файлов.

* <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> предоставляет [корневой каталог содержимого](xref:fundamentals/index#content-root) и [корневой каталог документов](xref:fundamentals/index#web-root) приложения в виде типов `IFileProvider`.
* [ПО промежуточного слоя для статических файлов](xref:fundamentals/static-files) использует поставщики файлов для поиска статических файлов.
* [Razor](xref:mvc/views/razor) использует поставщики файлов для поиска страниц и представлений.
* Инструменты .NET Core используют поставщики файлов и стандартные маски, чтобы указать файлы для публикации.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="file-provider-interfaces"></a>Интерфейсы поставщика файлов

Основной интерфейс — <xref:Microsoft.Extensions.FileProviders.IFileProvider>. `IFileProvider` предоставляет методы для следующих действий:

* получение сведений о файле (<xref:Microsoft.Extensions.FileProviders.IFileInfo>);
* получение сведений о каталоге (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>);
* настройка уведомлений об изменениях (с помощью <xref:Microsoft.Extensions.Primitives.IChangeToken>).

`IFileInfo` предоставляет методы и свойства для работы с файлами:

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (в байтах);
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> (дата).

Данные из файла можно считывать с помощью метода [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*).

Пример приложения демонстрирует, как настроить в `Startup.ConfigureServices` поставщике файлов, чтобы использовать его в приложении путем [внедрения зависимостей](xref:fundamentals/dependency-injection).

## <a name="file-provider-implementations"></a>Реализации поставщиков файлов

Доступны три реализации `IFileProvider`.

| Реализация | ОПИСАНИЕ |
| -------------- | ----------- |
| [PhysicalFileProvider](#physicalfileprovider) | Физический поставщик используется для доступа к физическим файлам системы. |
| [ManifestEmbeddedFileProvider](#manifestembeddedfileprovider) | Поставщик внедренных манифестов используется для доступа к файлам, внедренным в сборки. |
| [CompositeFileProvider](#compositefileprovider) | Составной поставщик используется для предоставления комбинированного доступа к файлам и каталогам из одного или нескольких поставщиков. |

### <a name="physicalfileprovider"></a>PhysicalFileProvider

<xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> предоставляет доступ к физической файловой системе. `PhysicalFileProvider` использует тип <xref:System.IO.File?displayProperty=fullName> (для физического поставщика), ограничивая все пути каталогом и его дочерними элементами. Такая привязка к области защищает от доступа к файловой системе за пределами указанного каталога и его дочерних элементов. Самый распространенный подход к созданию и использованию `PhysicalFileProvider` — запросить `IFileProvider` в конструкторе путем [внедрения зависимостей](xref:fundamentals/dependency-injection).

При непосредственном создании экземпляра этого поставщика нужно указать путь к каталогу, который станет базовым путем для всех запросов, выполненных с помощью этого поставщика.

Следующий пример кода демонстрирует, как создать и использовать `PhysicalFileProvider` для получения содержимого каталога и сведений о файлах:

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

В предшествующем примере используются следующие типы:

* `provider` является `IFileProvider`.
* `contents` является `IDirectoryContents`.
* `fileInfo` является `IFileInfo`.

С помощью поставщика файлов вы можете выполнить итерацию по каталогу, указанному в параметре `applicationRoot`, или вызвать `GetFileInfo` для получения сведений о файлах. Поставщик файлов не имеет доступ к каталогам за пределами `applicationRoot`.

Этот пример приложения создает поставщик для приложения в классе `Startup.ConfigureServices`, используя [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a>ManifestEmbeddedFileProvider

<xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> используется для доступа к файлам, внедренным в сборки. `ManifestEmbeddedFileProvider` использует манифест, скомпилированный в сборку, для воссоздания исходных путей для внедренных файлов.

Чтобы создать манифест для внедренных файлов, задайте для свойства `<GenerateEmbeddedFilesManifest>` значение `true`. Выберите файлы для внедрения с помощью [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

Используйте [стандартные маски](#glob-patterns) для указания одного или нескольких файлов, которые вы хотите внедрить в сборку.

Наш пример приложения создает `ManifestEmbeddedFileProvider` и передает в соответствующий конструктор текущую выполняемую сборку.

*Startup.cs*:

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(typeof(Program).Assembly);
```

Дополнительные перегрузки позволяют сделать следующее:

* указать относительный путь к файлу;
* ограничить файлы по дате последнего изменения;
* указать имя внедренного ресурса, содержащего внедренный файл манифеста.

| Перегрузка | ОПИСАНИЕ |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | Принимает необязательный параметр `root` со значением относительного пути. Укажите `root`, чтобы ограничить вызовы к <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> только ресурсами по указанному пути. |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | Принимает необязательный параметр `root` со значением относительного пути и параметр `lastModified` со значением даты (<xref:System.DateTimeOffset>). Параметр `lastModified` ограничивает дату последнего изменения для экземпляров <xref:Microsoft.Extensions.FileProviders.IFileInfo>, возвращаемых функцией <xref:Microsoft.Extensions.FileProviders.IFileProvider>. |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | Принимает необязательный параметр `root` со значением относительного пути, дату `lastModified` и параметры `manifestName`. `manifestName` здесь представляет имя встроенного ресурса, содержащего манифест. |

### <a name="compositefileprovider"></a>CompositeFileProvider

<xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> объединяет экземпляры `IFileProvider`, предоставляя единый интерфейс для работы с файлами от нескольких поставщиков. При создании `CompositeFileProvider` передайте в соответствующий конструктор один или несколько экземпляров `IFileProvider`:

В нашем примере приложения `PhysicalFileProvider` и `ManifestEmbeddedFileProvider` предоставляют файлы для `CompositeFileProvider` с регистрацией в контейнере служб приложения:

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a>Отслеживание изменений

Метод [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) позволяет контролировать изменения в одном или нескольких файлах или каталогах. `Watch` принимает строку пути, которая можно использовать [стандартные маски](#glob-patterns) для указания нескольких файлов. `Watch` возвращает <xref:Microsoft.Extensions.Primitives.IChangeToken>. Этот токен изменений предоставляет следующие свойства:

* <xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> — свойство, которое можно проверить, чтобы определить, произошло ли изменение.
* <xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> — вызывается при обнаружении изменений в указанной строке пути. Каждый токен изменения выполняет соответствующий обратный вызов только в ответ на отдельное изменение. Чтобы реализовать непрерывный мониторинг, используйте <xref:System.Threading.Tasks.TaskCompletionSource`1>, как показано ниже, или повторно создавайте экземпляры `IChangeToken` в ответ на изменения.

В нашем примере консольное приложение *WatchConsole* будет отображать сообщение при изменении текстового файла:

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Некоторые файловые системы, такие как контейнеры Docker и сетевые папки, не могут надежно отправлять уведомления об изменениях. Задайте для переменной среды `DOTNET_USE_POLLING_FILE_WATCHER` значение `1` или `true`, чтобы опрашивать файловую систему на предмет изменений каждые 4 секунды (это значение нельзя изменить).

## <a name="glob-patterns"></a>Стандартные маски

В путях файловой системы используются шаблоны подстановочных знаков, которые называются *стандартными масками*. Эти маски позволяют указывать группы файлов. Поддерживаются два подстановочных знака — `*` и `**`.

**`*`**  
Совпадает с любым элементом на текущем уровне папок, любым именем или расширением файла. Совпадения завершаются символами `/` и `.` в пути файла.

**`**`**  
Совпадает со всем содержимым на разных уровнях каталогов. Может использоваться для рекурсивного сопоставления множества файлов в иерархии каталогов.

**Примеры стандартных масок**

**`directory/file.txt`**  
Соответствует конкретному файлу в заданном каталоге.

**`directory/*.txt`**  
Соответствует всем файлам с расширением *.txt* в заданном каталоге.

**`directory/*/appsettings.json`**  
Соответствует всем файлам `appsettings.json` в любом каталоге, расположенном ровно на один уровень ниже каталога *directory*.

**`directory/**/*.txt`**  
Соответствует всем файлам с расширением *.txt*, находящимся на любом уровне ниже каталога *directory*.

::: moniker-end
