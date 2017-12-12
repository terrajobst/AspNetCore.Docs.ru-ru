---
title: "Конфигурация в .NET Core"
author: rick-anderson
description: "Используйте API конфигурации для настройки приложения ASP.NET Core несколькими способами."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/index
ms.openlocfilehash: 6281d6ba254670b111964715410fc0694ae4d149
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/29/2017
---
# <a name="configure-an-aspnet-core-app"></a>Настройка приложения ASP.NET Core

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson), [Марк Михаэлис](http://intellitect.com/author/mark-michaelis/) (Mark Michaelis), [Стив Смит](https://ardalis.com/) (Steve Smith), [Даниэль Рот](https://github.com/danroth27) (Daniel Roth) и [Люк Лэтхэм](https://github.com/guardrex) (Luke Latham)

API конфигурации позволяет настраивать веб-приложения ASP.NET Core на основе списка пар "имя-значение". Конфигурация считывается во время выполнения из нескольких источников. Пары "имя-значение" можно сгруппировать в многоуровневую иерархию. 

Существуют следующие поставщики конфигурации:

* форматов файлов (INI, JSON и XML);
* аргументов командной строки;
* Переменные среды
* объектов .NET в памяти;
* зашифрованного пользовательского хранилища;
* [Azure Key Vault](xref:security/key-vault-configuration);
* а также пользовательские поставщики (устанавливаемые или создаваемые).

Каждое значение конфигурации сопоставляется строковому ключу. Имеется встроенная поддержка привязки для десериализации параметров в пользовательский объект [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) (простой класс .NET со свойствами).

Шаблон параметров использует классы параметров для представления групп связанных настроек. Дополнительные сведения об использовании шаблона параметров см. в разделе [Параметры](xref:fundamentals/configuration/options).

[Просмотреть или скачать образец кода](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="json-configuration"></a>Конфигурация JSON

В следующем консольном приложении используется поставщик конфигурации JSON:

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

Приложение считывает и отображает следующие параметры конфигурации приложения:

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

Конфигурация — это иерархический список пар "имя-значение", в котором узлы разделяются двоеточием. Чтобы получить значение, обратитесь к индексатору `Configuration` с ключом соответствующего элемента:

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

Для работы с массивами в источниках конфигурации в формате JSON используйте индекс массива в составе строки с разделителем-двоеточием. В следующем примере возвращается имя первого элемента в предыдущем массиве `wizards`:

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

Пары "имя-значение", записанные во встроенные поставщики `Configuration`, **не** сохраняются. Однако можно создать пользовательский поставщик, который сохраняет значения. См. раздел [Пользовательский поставщик конфигурации](xref:fundamentals/configuration/index#custom-config-providers).

В предыдущем примере для считывания значений используется индексатор конфигурации. Для доступа к конфигурации вне `Startup` воспользуйтесь *шаблоном параметров*. Дополнительные сведения см. в разделе [Параметры](xref:fundamentals/configuration/options).

Как правило, для разных сред (например, разработки, тестирования и производства) существуют разные параметры конфигурации. Метод расширения `CreateDefaultBuilder` в приложении ASP.NET Core 2.x (или при использовании `AddJsonFile` и `AddEnvironmentVariables` непосредственно в приложении ASP.NET Core 1.x) добавляет поставщики конфигурации для чтения JSON-файлов и источников системной конфигурации:

* *appsettings.json*
* *appsettings.\<имя_среды>.json*
* Переменные среды

Объяснение параметров см. в разделе [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions). `reloadOnChange` поддерживается только в ASP.NET Core 1.1 и более поздних версиях. 

Источники конфигурации считываются в порядке их указания. В приведенном выше коде переменные среды считываются последними. Все значения конфигурации, заданные с помощью среды, заменяют значения, заданные в предыдущих двух поставщиках.

Для среды обычно задаются значения `Development`, `Staging` или `Production`. Дополнительные сведения см. в статье [Работа с несколькими средами](xref:fundamentals/environments).

Рекомендации по конфигурации:

* `IOptionsSnapshot` может перезагрузить данные конфигурации после их изменения. Дополнительные сведения см. в разделе [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).
* Ключи конфигурации зависят от регистра.
* Указывайте переменные среды последними, чтобы локальная среда могла переопределять параметры в развернутых файлах конфигурации.
* **Никогда** не храните пароли и другие конфиденциальные данные в коде поставщика конфигурации или в файлах конфигурации в виде обычного текста. Не используйте секреты производства в средах разработки и тестирования. Указывайте секретные данные вне проекта, чтобы их нельзя было случайно зафиксировать в репозитории. Узнайте больше о [работе с несколькими средами](xref:fundamentals/environments) и управлении [безопасным хранением секретов приложения во время разработки](xref:security/app-secrets).
* Если двоеточие (`:`) не может быть используется в переменных среды в вашей системе, замените двоеточием (`:`) с двойным подчеркиванием (`__`).

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>Поставщик в памяти и привязка к классу POCO

В следующем примере демонстрируется использование поставщика в памяти и привязка к классу:

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

Значения конфигурации возвращаются в виде строк, но с помощью привязки можно создавать объекты. Привязка позволяет получать объекты POCO или даже графы объектов.

### <a name="getvalue"></a>GetValue

В следующем примере показан метод расширения [GetValue&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_):

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

Метод `GetValue<T>` ConfigurationBinder позволяет задавать значение по умолчанию (в примере — 80). Метод `GetValue<T>` предназначен для простых сценариях и не выполняет привязку ко всем разделам. Метод `GetValue<T>` возвращает скалярные значения из `GetSection(key).Value`, преобразованного в конкретный тип.

## <a name="bind-to-an-object-graph"></a>Привязка к графу объектов

Можно реализовать рекурсивную привязку к каждому объекту в классе. Рассмотрим следующий класс `AppSettings`:

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

В следующем примере выполняется привязка к классу `AppSettings`:

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

В **ASP.NET Core 1.1** и более поздних версиях можно использовать метод `Get<T>`, который работает со всеми разделами. Метод `Get<T>` может быть более удобным, чем `Bind`. В следующем коде демонстрируется применение метода `Get<T>` с примером выше.

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

Используется следующий файл *appsettings.json*:

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

Программа выводит `Height 11`.

Для модульного тестирования конфигурации можно выполнить следующий код:

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a>Создание пользовательского поставщика Entity Framework

В этом разделе создается поставщик базовой конфигурации, который считывает пары "имя-значение" из базы данных с помощью EF. 

Определите сущность `ConfigurationValue` для хранения значений конфигурации в базе данных:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

Добавьте `ConfigurationContext` в хранилище и обратитесь к настроенным значениям:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

Создайте класс, реализующий [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

Создайте пользовательский поставщик конфигурации путем наследования от [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).  Поставщик конфигурации инициализирует пустую базу данных:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

При выполнении примера отображаются выделенные значения из базы данных ("value_from_ef_1" и "value_from_ef_2").

Можно добавить метод расширения `EFConfigSource` для добавления источник конфигурации:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

В следующем примере кода демонстрируется использование настраиваемого `EFConfigProvider`:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

Обратите внимание, что в этом примере пользовательский `EFConfigProvider` добавляется после поставщика JSON, поэтому все параметры из базы данных переопределяют параметры из файла *appsettings.json*.

Используется следующий файл *appsettings.json*:

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

Отобразится следующее:

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>Поставщик конфигурации CommandLine

[Поставщик конфигурации CommandLine](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) получает пары "ключ-значение" аргумента командной строки для конфигурации во время выполнения.

[Просмотрите или скачайте пример конфигурации CommandLine](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine).

### <a name="setup-and-use-the-commandline-configuration-provider"></a>Настройка и использование поставщика конфигурации CommandLine

# <a name="basic-configurationtabbasicconfiguration"></a>[Базовая конфигурация](#tab/basicconfiguration)

Чтобы активировать конфигурацию командной строки, вызовите метод расширения `AddCommandLine` в экземпляре [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

После выполнения кода отобразятся следующие выходные данные:

```console
MachineName: MairaPC
Left: 1980
```

Передача пар "ключ-значение" аргументов в командной строке приведет к изменению значений `Profile:MachineName` и `App:MainWindow:Left`:

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

В окне консоли отображается следующее:

```console
MachineName: BartPC
Left: 1979
```

Чтобы переопределить конфигурацию, предоставленную другими поставщиками конфигурации, конфигурацией командной строки, вызовите `AddCommandLine` последним в `ConfigurationBuilder`:

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Для создания узла стандартные приложения ASP.NET Core 2.x используют статический удобный метод `CreateDefaultBuilder`:

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

`CreateDefaultBuilder` загружает дополнительную конфигурацию из *appsettings.json*, *appsettings.{среда}.json*, [секреты пользователя](xref:security/app-secrets) (в среде `Development`), переменные среды и аргументы командной строки. Поставщик конфигурации CommandLine вызывается последним. В этом случае аргументы командной строки, передаваемые во время выполнения, переопределяют конфигурацию, заданную другими вызванными ранее поставщиками конфигурации.

Обратите внимание, что `reloadOnChange` включен для файлов *appsettings*. Аргументы командной строки переопределяются в том случае, если после запуска приложения изменяется совпадающее значение конфигурации в файле *appsettings*.

> [!NOTE]
> В качестве альтернативы использования метода `CreateDefaultBuilder` в ASP.NET Core 2.x поддерживается создание узла с помощью [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) и формирование конфигурации вручную с помощью [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder). Дополнительные сведения см. в разделе о ASP.NET Core 1.x.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Чтобы использовать поставщик конфигурации CommandLine, создайте [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) и вызовите метод `AddCommandLine`. В этом случае аргументы командной строки, передаваемые во время выполнения, переопределяют конфигурацию, заданную другими вызванными ранее поставщиками конфигурации. Примените конфигурацию к [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) с помощью метода `UseConfiguration`:

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>Аргументы

Аргументы, передаваемые в командной строке, должны соответствовать одному из двух форматов, приведенных в следующей таблице.

| Формат аргумента                                                     | Пример        |
| ------------------------------------------------------------------- | :------------: |
| Один аргумент: пара "ключ-значение", разделенная знаком равенства (`=`) | `key1=value`   |
| Последовательность из двух аргументов: пара "ключ-значение", разделенная пробелом    | `/key1 value1` |

**Один аргумент**

Значение должно стоять после знака равенства (`=`). Значение может быть равно NULL (например, `mykey=`).

Ключ может иметь префикс.

| Префикс ключа               | Пример         |
| ------------------------ | :-------------: |
| Без префикса                | `key1=value1`   |
| Один дефис (`-`) &#8224; | `-key2=value2`  |
| Два дефиса (`--`)        | `--key3=value3` |
| Прямая косая черта (`/`)      | `/key4=value4`  |

&#8224;Ключ с префиксом из одного дефиса (`-`) должен быть указан в [сопоставлениях переключений](#switch-mappings), описанных ниже.

Пример команды:

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

Примечание. Если `-key1` отсутствует в [сопоставлениях переключений](#switch-mappings), переданных поставщику конфигурации, возникает исключение `FormatException`.

**Последовательность из двух аргументов**

Значение не может быть равно NULL и должно следовать за ключом, разделенным пробелом.

Ключ должен иметь префикс.

| Префикс ключа               | Пример         |
| ------------------------ | :-------------: |
| Один дефис (`-`) &#8224; | `-key1 value1`  |
| Два дефиса (`--`)        | `--key2 value2` |
| Прямая косая черта (`/`)      | `/key3 value3`  |

&#8224;Ключ с префиксом из одного дефиса (`-`) должен быть указан в [сопоставлениях переключений](#switch-mappings), описанных ниже.

Пример команды:

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

Примечание. Если `-key1` отсутствует в [сопоставлениях переключений](#switch-mappings), переданных поставщику конфигурации, возникает исключение `FormatException`.

### <a name="duplicate-keys"></a>Повторяющиеся ключи

Если указаны повторяющиеся ключи, используется последняя пара "ключ-значение".

### <a name="switch-mappings"></a>Сопоставления переключений

При построении конфигурации вручную с помощью `ConfigurationBuilder` при необходимости можно предоставить методу `AddCommandLine` словарь сопоставления переключений. Сопоставления переключений позволяют указать логику замены имени ключа.

В словаре сопоставлений переключений выполняется поиск ключа, который совпадает с ключом, предоставляемым аргументом командной строки. Если ключ командной строки найден, значение словаря (новый ключ) передается обратно, чтобы задать конфигурацию. Сопоставление переключений необходимо для любого ключа командной строки с префиксом из одного дефиса (`-`).

Правила ключей из словаря сопоставления переключений:

* Переключения должны начинаться с дефиса (`-`) или двойного дефиса (`--`).
* Словарь сопоставлений переключений не должен содержать повторяющиеся ключи.

В следующем примере метод `GetSwitchMappings` позволяет аргументам командной строки использовать префикс ключа с одним дефисом (`-`) и исключает начальные префиксы подключа.

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

Если аргументы командной строки не указаны, словарь для `AddInMemoryCollection` задает значения конфигурации. Запустите приложение с помощью следующей команды:

```console
dotnet run
```

В окне консоли отображается следующее:

```console
MachineName: RickPC
Left: 1980
```

Используйте следующее для передачи параметров конфигурации:

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

В окне консоли отображается следующее:

```console
MachineName: DahliaPC
Left: 1984
```

Созданный словарь сопоставлений переключений содержит данные, показанные в следующей таблице.

| Ключ            | Значение                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

Чтобы продемонстрировать переключение ключей с помощью словаря, выполните следующую команду:

```console
dotnet run -MachineName=ChadPC -Left=1988
```

Ключи командной строки меняются. В окне консоли отображаются значения конфигурации для `Profile:MachineName` и `App:MainWindow:Left`:

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a>Файл web.config

Файл *web.config* необходим для размещения приложения в службах IIS или IIS Express. Файл *web.confi* включает AspNetCoreModule в службах IIS для запуска приложения. Параметры в файле *web.config* включают модуль AspNetCoreModule в службах IIS для запуска приложения и настройки других параметров и модулей IIS. Если вы используете Visual Studio и удаляете файл *web.config*, Visual Studio создаст новый файл.

## <a name="additional-notes"></a>Дополнительные сведения

* Внедрение зависимостей (DI) настраивается после вызова `ConfigureServices`.
* Система конфигурации не поддерживает внедрение зависимостей.
* `IConfiguration` имеет две специализации:
  * `IConfigurationRoot` используется для корневого узла. Может инициировать перезагрузку.
  * `IConfigurationSection` представляет раздел значений конфигурации. Методы `GetSection` и `GetChildren` возвращают `IConfigurationSection`.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Параметры](xref:fundamentals/configuration/options)
* [Работа с несколькими средами](xref:fundamentals/environments)
* [Безопасное хранение секретов приложений во время разработки](xref:security/app-secrets)
* [Размещение в ASP.NET Core](xref:fundamentals/hosting)
* [Введение зависимостей](xref:fundamentals/dependency-injection)
* [Поставщик конфигурации Azure Key Vault](xref:security/key-vault-configuration)
