---
title: Конфигурация в .NET Core
author: rick-anderson
description: Узнайте, как использовать API конфигурации для настройки приложения ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 59ab0cd0f6975d15bd01ce7e4128521938182c24
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228628"
---
# <a name="configuration-in-aspnet-core"></a>Конфигурация в .NET Core

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson), [Марк Михаэлис](http://intellitect.com/author/mark-michaelis/) (Mark Michaelis), [Стив Смит](https://ardalis.com/) (Steve Smith), [Даниэль Рот](https://github.com/danroth27) (Daniel Roth) и [Люк Лэтхэм](https://github.com/guardrex) (Luke Latham)

API конфигурации позволяет настраивать веб-приложения ASP.NET Core на основе списка пар "имя-значение". Конфигурация считывается во время выполнения из нескольких источников. Пары "имя-значение" можно сгруппировать в многоуровневую иерархию.

Существуют следующие поставщики конфигурации:

* Форматы файлов (INI, JSON и XML).
* аргументы командной строки.
* Переменные среды.
* Объекты .NET в памяти.
* Незашифрованное хранилище [Secret Manager](xref:security/app-secrets) (Диспетчер секретов).
* Зашифрованное пользовательское хранилище, например [Azure Key Vault](xref:security/key-vault-configuration).
* Пользовательские поставщики (установленные или созданные).

Каждое значение конфигурации сопоставляется строковому ключу. Имеется встроенная поддержка привязки для десериализации параметров в пользовательский объект [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) (простой класс .NET со свойствами).

Шаблон параметров использует классы параметров для представления групп связанных настроек. Дополнительные сведения об использовании шаблона параметров см. в разделе [Параметры](xref:fundamentals/configuration/options).

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.1"

Примеры, приведенные в этом разделе, основаны на следующем.

* Указание базового пути приложения с помощью [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath). `SetBasePath` предоставляется приложению с помощью ссылки на пакет [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/).
* Разрешение разделов файлов конфигурации с помощью [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection). `GetSection` предоставляется приложению с помощью ссылки на пакет [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/).
* Настройка привязки с помощью [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind). `Bind` предоставляется приложению с помощью ссылки на пакет [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/).

Пакеты включены в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Примеры, приведенные в этом разделе, основаны на следующем.

* Указание базового пути приложения с помощью [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath). `SetBasePath` предоставляется приложению с помощью ссылки на пакет [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/).
* Разрешение разделов файлов конфигурации с помощью [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection). `GetSection` предоставляется приложению с помощью ссылки на пакет [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/).
* Настройка привязки с помощью [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind). `Bind` предоставляется приложению с помощью ссылки на пакет [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/).

Эти пакеты включены в [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage).

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

Примеры, приведенные в этом разделе, основаны на следующем.

* Указание базового пути приложения с помощью [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath). `SetBasePath` предоставляется приложению с помощью ссылки на пакет [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/).
* Разрешение разделов файлов конфигурации с помощью [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection). `GetSection` предоставляется приложению с помощью ссылки на пакет [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/).
* Настройка привязки с помощью [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind). `Bind` предоставляется приложению с помощью ссылки на пакет [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/).

::: moniker-end

## <a name="json-configuration"></a>Конфигурация JSON

В следующем консольном приложении используется поставщик конфигурации JSON:

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

Приложение считывает и отображает следующие параметры конфигурации приложения:

[!code-json[](index/sample/ConfigJson/appsettings.json)]

Конфигурация — это иерархический список пар "имя-значение", в котором узлы разделяются двоеточием (`:`). Чтобы получить значение, обратитесь к индексатору `Configuration` с ключом соответствующего элемента:

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

Для работы с массивами в источниках конфигурации в формате JSON используйте индекс массива в составе строки с разделителем-двоеточием. В следующем примере возвращается имя первого элемента в предыдущем массиве `wizards`:

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

Пары "имя-значение", записываемые во встроенные поставщики [конфигурации](/dotnet/api/microsoft.extensions.configuration), **не сохраняются**. Но вы можете создать настраиваемый поставщик, который сохраняет значения. См. раздел [Пользовательский поставщик конфигурации](xref:fundamentals/configuration/index#custom-config-providers).

В предыдущем примере для считывания значений используется индексатор конфигурации. Для доступа к конфигурации вне `Startup` воспользуйтесь *шаблоном параметров*. Дополнительные сведения см. в разделе [Параметры](xref:fundamentals/configuration/options).

## <a name="xml-configuration"></a>Конфигурации XML

Для работы с массивами в источниках конфигураций с XML-форматированием укажите индекс `name` для каждого элемента. Используйте этот индекс для доступа к значениям.

```xml
<wizards>
  <wizard name="Gandalf">
    <age>1000</age>
  </wizard>
  <wizard name="Harry">
    <age>17</age>
  </wizard>
</wizards>
```

```csharp
Console.Write($"{Configuration["wizard:Harry:age"]}");
// Output: 17
```

## <a name="configuration-by-environment"></a>Конфигурация для разных сред

Как правило, для разных сред (например, разработки, тестирования и производства) существуют разные параметры конфигурации. Метод расширения `CreateDefaultBuilder` в приложении ASP.NET Core 2.x (или при использовании `AddJsonFile` и `AddEnvironmentVariables` непосредственно в приложении ASP.NET Core 1.x) добавляет поставщики конфигурации для чтения JSON-файлов и источников системной конфигурации:

* *appsettings.json*
* *appsettings.\<имя_среды>.json*
* Переменные среды

Приложения ASP.NET Core 1.x должны вызывать `AddJsonFile` и [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).

Объяснение параметров см. в разделе [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions). `reloadOnChange` поддерживается только в ASP.NET Core 1.1 и более поздних версиях.

Источники конфигурации считываются в порядке их указания. В приведенном выше коде переменные сред считываются последними. Все значения конфигурации, заданные с помощью среды, заменяют значения, заданные в предыдущих двух поставщиках.

Рассмотрим следующий файл *appsettings.Staging.JSON*.

[!code-json[](index/sample/appsettings.Staging.json)]

В следующем коде `Configure` считывает значение `MyConfig`.

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]

Для среды обычно задаются значения `Development`, `Staging` или `Production`. Дополнительные сведения см. в статье [Использование нескольких сред](xref:fundamentals/environments).

Рекомендации по конфигурации:

* [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) может перезагрузить данные конфигурации после их изменения.
* Ключи конфигурации **не учитывают** регистр.
* **Никогда** не храните пароли и другие конфиденциальные данные в коде поставщика конфигурации или в файлах конфигурации в виде обычного текста. Не используйте секреты рабочей среды в средах разработки и тестирования. Указывайте секреты вне проекта, чтобы их нельзя было случайно зафиксировать в репозитории с исходным кодом. Узнайте больше об [использовании нескольких сред](xref:fundamentals/environments) и управлении [безопасным хранением секретов приложения во время разработки](xref:security/app-secrets).
* Для значений иерархической конфигурации, указанных в переменных среды, двоеточие (`:`) поддерживается не на всех платформах. Двойной знак подчеркивания (`__`) поддерживается на всех платформах.
* При взаимодействии с API конфигурации двоеточие (`:`) поддерживается на всех платформах.

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>Поставщик в памяти и привязка к классу POCO

В следующем примере демонстрируется использование поставщика в памяти и привязка к классу:

[!code-csharp[](index/sample/InMemory/Program.cs)]

Значения конфигурации возвращаются в виде строк, но с помощью привязки можно создавать объекты. Привязка позволяет извлекать объекты POCO и даже целые графы объектов.

### <a name="getvalue"></a>GetValue

В следующем примере показан метод расширения [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_):

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

Метод `GetValue<T>` из ConfigurationBinder позволяет задавать значение по умолчанию (в примере — 80). `GetValue<T>` используется для простых сценариев и не выполняет привязку ко всем разделам. Метод `GetValue<T>` получает из `GetSection(key).Value` скалярные значения, преобразованные в определенный тип.

## <a name="bind-to-an-object-graph"></a>Привязка к графу объектов

Для каждого объекта в классе возможна рекурсивная привязка. Рассмотрим следующий класс `AppSettings`:

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

В следующем примере выполняется привязка к классу `AppSettings`:

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

**ASP.NET Core 1.1** и более поздние версии могут использовать метод `Get<T>`, который работает с целыми разделами. Метод `Get<T>` может быть более удобным, чем `Bind`. Следующий код показывает, как использовать `Get<T>` с предыдущим примером.

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

Используется следующий файл *appsettings.json*:

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

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

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

Добавьте `ConfigurationContext` в хранилище и обратитесь к настроенным значениям:

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

Создайте класс, реализующий [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource).

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

Создайте пользовательский поставщик конфигурации путем наследования от [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider). Поставщик конфигурации инициализирует пустую базу данных:

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

При выполнении примера отображаются выделенные значения из базы данных ("value_from_ef_1" и "value_from_ef_2").

Вы можете использовать метод расширения `EFConfigSource` для добавления источника конфигурации.

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

В следующем примере кода демонстрируется использование настраиваемого `EFConfigProvider`:

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

Обратите внимание, что в этом примере пользовательский `EFConfigProvider` добавляется после поставщика JSON, поэтому все параметры из базы данных переопределяют параметры из файла *appsettings.json*.

Используется следующий файл *appsettings.json*:

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

Выводится следующий результат.

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>Поставщик конфигурации CommandLine

[Поставщик конфигурации CommandLine](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) получает пары "ключ-значение" аргумента командной строки для конфигурации во время выполнения.

[Просмотрите или скачайте пример конфигурации CommandLine](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample/CommandLine).

### <a name="setup-and-use-the-commandline-configuration-provider"></a>Настройка и использование поставщика конфигурации CommandLine

# <a name="basic-configurationtabbasicconfiguration"></a>[Базовая конфигурация](#tab/basicconfiguration/)

Чтобы активировать конфигурацию командной строки, вызовите метод расширения `AddCommandLine` в экземпляре [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

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

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Для создания узла стандартные приложения ASP.NET Core 2.x используют статический удобный метод `CreateDefaultBuilder`:

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

`CreateDefaultBuilder` загружает дополнительную конфигурацию из *appsettings.json*, *appsettings.{среда}.json*, [секреты пользователя](xref:security/app-secrets) (в среде `Development`), переменные среды и аргументы командной строки. Поставщик конфигурации CommandLine вызывается последним. В этом случае аргументы командной строки, передаваемые во время выполнения, переопределяют конфигурацию, заданную другими вызванными ранее поставщиками конфигурации.

Предположим, что в файлах *appsettings*

* Включено `reloadOnChange`.
* Аргументы командной строки и файл *appsettings* содержат одинаковые параметры.
* Файл *appsettings*, содержащий соответствующий аргумент командной строки, изменяется после запуска приложения.

Если все вышеперечисленное верно, аргументы командной строки переопределяются.

Приложение ASP.NET Core 2.x может использовать [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) вместо `CreateDefaultBuilder`. При использовании `WebHostBuilder` задайте конфигурацию вручную с помощью [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder). Дополнительные сведения см. в разделе о ASP.NET Core 1.x.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Чтобы использовать поставщик конфигурации CommandLine, создайте [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) и вызовите метод `AddCommandLine`. В этом случае аргументы командной строки, передаваемые во время выполнения, переопределяют конфигурацию, заданную другими вызванными ранее поставщиками конфигурации. Примените конфигурацию к [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) с помощью метода `UseConfiguration`:

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

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
| Один дефис (`-`) & #8224; | `-key2=value2`  |
| Два дефиса (`--`)        | `--key3=value3` |
| Прямая косая черта (`/`)      | `/key4=value4`  |

& #8224;Ключ с префиксом из одного дефиса (`-`) должен быть указан в [сопоставлениях переключений](#switch-mappings), описанных ниже.

Пример команды:

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

Примечание. Если `-key2` отсутствует в [сопоставлениях переключений](#switch-mappings), переданных поставщику конфигурации, возникает исключение `FormatException`.

**Последовательность из двух аргументов**

Значение не может быть равно NULL и должно следовать за ключом, разделенным пробелом.

Ключ должен иметь префикс.

| Префикс ключа               | Пример         |
| ------------------------ | :-------------: |
| Один дефис (`-`) & #8224; | `-key1 value1`  |
| Два дефиса (`--`)        | `--key2 value2` |
| Прямая косая черта (`/`)      | `/key3 value3`  |

& #8224;Ключ с префиксом из одного дефиса (`-`) должен быть указан в [сопоставлениях переключений](#switch-mappings), описанных ниже.

Пример команды:

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

Примечание. Если `-key1` отсутствует в [сопоставлениях переключений](#switch-mappings), переданных поставщику конфигурации, возникает исключение `FormatException`.

### <a name="duplicate-keys"></a>Повторяющиеся ключи

Если указаны повторяющиеся ключи, используется последняя пара "ключ-значение".

### <a name="switch-mappings"></a>Сопоставления переключений

При создании конфигурации вручную с помощью `ConfigurationBuilder` вы можете добавить в метод `AddCommandLine` словарь сопоставления параметров. Сопоставление параметров позволяет указать логику замены имен ключей.

В словаре сопоставлений переключений выполняется поиск ключа, который совпадает с ключом, предоставляемым аргументом командной строки. Если ключ командной строки найден, значение словаря (новый ключ) передается обратно, чтобы задать конфигурацию. Сопоставление переключений необходимо для любого ключа командной строки с префиксом из одного дефиса (`-`).

Правила ключей из словаря сопоставления переключений:

* Переключения должны начинаться с дефиса (`-`) или двойного дефиса (`--`).
* Словарь сопоставлений переключений не должен содержать повторяющиеся ключи.

В следующем примере метод `GetSwitchMappings` позволяет аргументам командной строки использовать префикс ключа с одним дефисом (`-`) вместо начальных префиксов подключа.

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

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

Созданный словарь сопоставления параметров содержит данные, показанные в следующей таблице.

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

## <a name="webconfig-file"></a>Файл web.config

Файл *web.config* необходим для размещения приложения в службах IIS или IIS Express. Параметры в файле *web.config* включают [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) для запуска приложения и настройки других параметров и модулей IIS. Если файл *web.config* отсутствует и файл проекта содержит `<Project Sdk="Microsoft.NET.Sdk.Web">`, при публикации проекта файл *web.config* создается в опубликованных выходных данных (папка *publish*). Дополнительные сведения см. в разделе [Размещение ASP.NET Core в Windows со службами IIS](xref:host-and-deploy/iis/index#webconfig-file).

## <a name="access-configuration-during-startup"></a>Доступ к конфигурации во время запуска

Чтобы получить доступ к конфигурации в `ConfigureServices` или `Configure` во время запуска, см. примеры в разделе [Запуск приложения](xref:fundamentals/startup).

## <a name="adding-configuration-from-an-external-assembly"></a>Добавление конфигурации из внешней сборки

Реализация [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) позволяет при запуске добавлять в приложение улучшения из внешней сборки вне класса `Startup` приложения. Дополнительные сведения см. в разделе [Усовершенствование приложения из внешней сборки](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="access-configuration-in-a-razor-page-or-mvc-view"></a>Настройка доступа на странице Razor или в представлении MVC

Для доступа к параметрам конфигурации на странице Razor Pages или в представлении MVC добавьте [использование директивы](xref:mvc/views/razor#using) ([Справочник по C#: использование директивы](/dotnet/csharp/language-reference/keywords/using-directive)) для [пространства имен Microsoft.Extensions.Configuration](/dotnet/api/microsoft.extensions.configuration) и вставьте [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) на страницу или в представление.

На странице Razor Pages

```cshtml
@page
@model IndexModel

@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

В представлении MVC

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

## <a name="additional-notes"></a>Дополнительные сведения

* Внедрение зависимостей (DI) настраивается после вызова `ConfigureServices`.
* Система конфигурации не поддерживает внедрение зависимостей.
* `IConfiguration` имеет две специализации:
  * `IConfigurationRoot` используется для корневого узла. Может инициировать перезагрузку.
  * `IConfigurationSection` представляет раздел значений конфигурации. Методы `GetSection` и `GetChildren` возвращают `IConfigurationSection`.
  * Используйте [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) при перезагрузке конфигурации или для доступа к каждому поставщику. Оба этих случая нетипичные.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Параметры](xref:fundamentals/configuration/options)
* [Использование нескольких сред](xref:fundamentals/environments)
* [Безопасное хранение секретов приложений во время разработки](xref:security/app-secrets)
* [Размещение в ASP.NET Core](xref:fundamentals/host/index)
* [Введение зависимостей](xref:fundamentals/dependency-injection)
* [Поставщик конфигурации Azure Key Vault](xref:security/key-vault-configuration)
