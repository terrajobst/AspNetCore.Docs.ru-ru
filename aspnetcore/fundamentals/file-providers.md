---
title: "Файл поставщиков в ASP.NET Core"
author: ardalis
description: "Узнайте, как ASP.NET Core абстрагирует доступ к файловой системе при помощи поставщиков файла."
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 1e35d362-0005-4f84-a187-274ca203a787
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/file-providers
ms.openlocfilehash: fd847db992b20ab096b54378418d2b9bccff67be
ms.sourcegitcommit: 2b263e87217658caa42eedc4f9d2d21ef0ab5d59
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2018
---
# <a name="file-providers-in-aspnet-core"></a>Файл поставщиков в ASP.NET Core

По [Стив Смит](https://ardalis.com/)

ASP.NET Core абстрагирует доступ к файловой системе при помощи поставщиков файла.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="file-provider-abstractions"></a>Абстрактные классы поставщика файла

Файл поставщики, это абстракция над файловых систем. — Это основной интерфейс `IFileProvider`. `IFileProvider`Предоставляет методы для получения сведений о файле (`IFileInfo`), сведения о каталоге (`IDirectoryContents`) и для настройки уведомлений об изменениях (с помощью `IChangeToken`).

`IFileInfo`Предоставляет методы и свойства об отдельных файлов или каталогов. Он имеет два свойства типа boolean, `Exists` и `IsDirectory`, а также свойства, описывающие его `Name`, `Length` (в байтах), и `LastModified` даты. Можно считывать из файла с помощью его `CreateReadStream` метод.

## <a name="file-provider-implementations"></a>Файл реализации поставщика

Три способа реализации `IFileProvider` доступны: физических, внедренных и составными. Физический поставщика используется для доступа к файлам текущий системный. Внедренные поставщика используется для доступа к файлам, внедренных в сборки. Составной поставщика используется для предоставления объединенный доступа к файлам и каталогам из одного или нескольких поставщиков.

### <a name="physicalfileprovider"></a>PhysicalFileProvider

`PhysicalFileProvider` Предоставляет доступ к физической файловой системы. Он инкапсулирует `System.IO.File` типа (для физического поставщика), определение области все пути к каталогу и его дочерних элементов. Эту область ограничивает доступ к определенной папке и ее дочерних элементов, ограничивая его доступ к файловой системе вне этой границы. При создании экземпляра этого поставщика, вам необходимо указать путь к каталогу, который выступает в качестве базовый путь для всех запросов, внесенные в этот поставщик (который ограничивает доступ за пределами этого пути). В приложении ASP.NET Core, можно создать экземпляр `PhysicalFileProvider` поставщика непосредственно, или можно запросить `IFileProvider` в контроллере или конструктор службы через [внедрения зависимостей](dependency-injection.md). Второй подход обычно даст более гибкими и тестирования решения.

В следующем примере показано, как создать `PhysicalFileProvider`.


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

Можно прохода его содержимое каталога или получить сведения о конкретном файле, предоставляя вложенным.

Чтобы запросить поставщика из контроллера, укажите его в конструкторе контроллера и назначьте его локальное поле. Использование локального экземпляра в методах действий.

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

Затем создайте поставщика в приложении `Startup` класса:

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

В *Index.cshtml* Просмотр, итерации `IDirectoryContents` указано:

[!code-html[Main](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

Результат:

![Файл поставщика образец приложения список физических файлов и папок](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a>EmbeddedFileProvider

`EmbeddedFileProvider` Используется для доступа к файлам, внедренных в сборки. В .NET Core внедрить файлы в сборке с `<EmbeddedResource>` элемент в *.csproj* файла:

[!code-json[Main](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

Можно использовать [шаблоны глобализацию](#globbing-patterns) при указании файлов для внедрения в сборку. Эти шаблоны можно использовать для сопоставления одного или нескольких файлов.

> [!NOTE]
> Маловероятно, что когда-либо требуется фактически внедрения каждого JS-файла в проект в сборку; приведенном выше примере — только в целях демонстрации.

При создании `EmbeddedFileProvider`, передать сборку, он будет считать его конструктору.

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

Приведенном выше фрагменте показано создание `EmbeddedFileProvider` с доступом к выполняющейся сборки.

Обновление примера приложения для использования `EmbeddedFileProvider` приводит к следующие данные:

![Файл поставщика образец приложения внедренные файлы-перечень](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> Внедренные ресурсы, не следует предоставлять каталоги. Вместо этого путь к ресурсу (через пространство имен) внедряется в ее имени файла с помощью `.` разделители.

> [!TIP]
> `EmbeddedFileProvider` Конструктор принимает необязательный `baseNamespace` параметра. Задание этого будет действовать вызовы `GetDirectoryContents` к этим ресурсам в указанных имен.

### <a name="compositefileprovider"></a>CompositeFileProvider

`CompositeFileProvider` Объединяет `IFileProvider` экземпляров, предоставление доступа к единый интерфейс для работы с файлами из нескольких поставщиков. При создании `CompositeFileProvider`, передать один или несколько `IFileProvider` экземпляров в его конструктор:

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

Обновление примера приложения для использования `CompositeFileProvider` , которые входят обоих физических и внедренные поставщики настроены ранее, результаты в следующий результат:

![Файл поставщика образец приложения перечисление физических файлов и папок и внедренными файлами](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a>Контроль изменений

`IFileProvider` `Watch` Метод предоставляет способ для отслеживания файлов или каталогов для изменения. Этот метод принимает строку пути, который может использовать [глобализацию шаблоны](#globbing-patterns) для указания нескольких файлов и возвращает `IChangeToken`. Предоставляет этот токен `HasChanged` свойство, которое может быть проверен, и `RegisterChangeCallback` метод, вызываемый при обнаружении изменений для указанной строки пути. Обратите внимание, что каждый маркер изменений вызывает только его связанного обратного вызова в ответ на одно изменение. Для постоянного наблюдения, можно использовать `TaskCompletionSource` как показано ниже, или повторно создайте `IChangeToken` экземпляров в ответ на изменения.

В образце в этой статье консольное приложение настраивается для отображения сообщения при изменении текстового файла:

[!code-csharp[Main](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Результат после сохранения файла несколько раз:

![Окно командной строки после выполнения запуска dotnet показано приложение, мониторинг quotes.txt файл для изменения и измененного файла пять раз.](file-providers/_static/watch-console.png)

> [!NOTE]
> Некоторые файловые системы, такие как контейнеры Docker и общих сетевых ресурсов не могут надежно отправлять уведомления об изменениях. Задать `DOTNET_USE_POLLINGFILEWATCHER` переменную среды, чтобы `1` или `true` опроса файловой системы для изменения каждые 4 секунды.

## <a name="globbing-patterns"></a>Этот режим шаблонов

Пути файловой системы используйте шаблоны из подстановочных знаков вызывается *глобализацию шаблоны*. Эти простые шаблоны можно использовать для указания группы файлов. Два подстановочных знаков, `*` и `**`.

**`*`**

   Совпадает со всем на текущем уровне папок или любое имя файла или любое расширение файла. Прервано совпадений `/` и `.` символов в пути файла.

<strong><code>**</code></strong>

   Совпадает со всем по нескольким уровням каталога. Может использоваться для рекурсивного соответствует много файлов в иерархии каталога.

### <a name="globbing-pattern-examples"></a>Примеры шаблонов глобализации

**`directory/file.txt`**

   Соответствует конкретный файл в заданном каталоге.

**<code>directory/*.txt</code>**

   Обозначает все файлы с `.txt` расширения в заданном каталоге.

**`directory/*/bower.json`**

   Соответствует всем `bower.json` файлы в каталоги ровно один уровень ниже `directory` каталога.

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   Обозначает все файлы с `.txt` расширения вложенной в любом `directory` каталога.

## <a name="file-provider-usage-in-aspnet-core"></a>Использование поставщика файлов в ASP.NET Core

Несколько частей ASP.NET Core использовать файл поставщиков. `IHostingEnvironment`предоставляет содержимое корневого приложения и корневого веб-каталога как `IFileProvider` типов. По промежуточного слоя статических файлов использует файл поставщики для поиска статических файлов. Razor усложняет использование `IFileProvider` при поиске представления. DotNet публикация поставщиков файл использует функциональные возможности и шаблоны этот режим для указания того, какие файлы должны публиковаться.

## <a name="recommendations-for-use-in-apps"></a>Рекомендации для использования в приложениях

Если приложение ASP.NET Core требуется доступ к файловой системе, вы можете запросить экземпляр `IFileProvider` через внедрения зависимостей и использовать его методы для осуществления доступа к, как показано в этом примере. Это дает возможность настроить поставщик один раз, когда приложение запускается и уменьшает количество типов реализации создает приложение.
