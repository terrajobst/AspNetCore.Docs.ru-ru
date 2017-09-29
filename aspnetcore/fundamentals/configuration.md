---
title: "Конфигурация в .NET Core"
author: rick-anderson
description: "Узнайте, как использовать API конфигурации для настройки приложения ASP.NET Core из нескольких источников."
keywords: "ASP.NET Core, конфигурации JSON, конфигурации"
ms.author: riande
manager: wpickett
ms.date: 6/24/2017
ms.topic: article
ms.assetid: b3a5984d-e172-42eb-8a48-547e4acb6806
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration
ms.openlocfilehash: 379030df4ca91a38fce251aeaab9c5dfaf11e915
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2017
---
# <a name="configuration-in-aspnet-core"></a>Конфигурация в .NET Core

[Рик Андерсон](https://twitter.com/RickAndMSFT), [Марка Михаэлиса](http://intellitect.com/author/mark-michaelis/), [Стив Смит](https://ardalis.com/), [рот Daniel](https://github.com/danroth27), и [Latham Люк](https://github.com/guardrex)

API конфигурации предоставляет способ настройки приложения на основе списка пар «имя значение». Конфигурация для чтения во время выполнения из нескольких источников. Пары «имя значение» могут быть сгруппированы в многоуровневой иерархии. Нет поставщиков конфигурации для:

* Форматы файлов (ini-ФАЙЛЕ, JSON и XML)
* Аргументы командной строки
* Переменные среды
* Объекты в памяти .NET
* Зашифрованные пользовательского хранилища
* [Хранилище ключей Azure](xref:security/key-vault-configuration)
* Пользовательские поставщики, которые можно установить или создать

Каждое значение конфигурации сопоставляет строковый ключ. Поддержка встроенных привязок для десериализации параметров в пользовательский [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) объекта (простой класс .NET со свойствами).

[Просмотреть или скачать образец кода](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample)

## <a name="simple-configuration"></a>Простая настройка

Следующее приложение консоли использует поставщик конфигурации JSON:

[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]

Считывает и отображает следующие параметры конфигурации приложения:

[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]

Конфигурация состоит из иерархический список пар "имя значение", в которых узлы разделяются точкой с запятой. Для получения значения, доступ к `Configuration` индексатора с ключом соответствующего элемента:

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

Для работы с массивами в источниках конфигурации в формате JSON, используйте индекс массива как часть строки двоеточием. В следующем примере возвращается имя первого элемента в предыдущем `wizards` массива:

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

Пары имя значение, записанных во встроенную в `Configuration` поставщики **не** сохраняются, однако можно создать пользовательский поставщик, который сохраняет значения. В разделе [настраиваемый поставщик конфигурации](xref:fundamentals/configuration#custom-config-providers).

Предыдущий пример использует конфигурации индексатора для считывания значений. Для настройки доступа извне `Startup`, используйте [параметры шаблона](xref:fundamentals/configuration#options-config-objects). *Параметры шаблона* показано далее в этой статье.

Это обычно имеют разные параметры конфигурации для разных сред, например, разработки, тестирования и производства. `CreateDefaultBuilder` Метод расширения в приложении ASP.NET Core 2.x (или с помощью `AddJsonFile` и `AddEnvironmentVariables` непосредственно в приложении ASP.NET Core 1.x) добавляет поставщиков конфигурации для чтения файлы JSON и системные настройки источников:

* *appsettings.json*
* *appSettings. \<EnvironmentName > .json*
* переменные среды

В разделе [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) объяснение параметров. `reloadOnChange`поддерживается только в ASP.NET Core 1.1 и более поздних версий. 

Источники конфигурации считываются в порядке, в котором они указаны. В приведенном выше коде считываются последнего переменные среды. Заменить все значения конфигурации задать с помощью среды были заданы в предыдущих двух поставщиков.

Среды обычно имеет одно из `Development`, `Staging`, или `Production`. В разделе [работа с несколькими средами](environments.md) для получения дополнительной информации.

Рекомендации по настройке:

* `IOptionsSnapshot`можно перезагрузить данные конфигурации, при его изменении. Используйте `IOptionsSnapshot` Если необходимо перезагрузить данные конфигурации.  В разделе [IOptionsSnapshot](#ioptionssnapshot) для получения дополнительной информации.
* Ключи конфигурации учитывается регистр.
* Рекомендуется указывать последним, переменные среды, чтобы локальной среды можно переопределить действий в файлах конфигурации, развернутые.
* **Никогда не** хранить пароли и другие конфиденциальные данные в коде поставщика конфигурации или в файлах конфигурации в виде обычного текста. Не используйте секреты производства в разработке и тестовую среду. Вместо этого укажите секретные данные за пределами дереве проекта, чтобы их может быть сохранена в репозиторий случайно. Дополнительные сведения о [работа с несколькими средами](environments.md) и управление ими [безопасного хранения секрета приложения во время разработки](../security/app-secrets.md).
* Если `:` не может быть использование переменных среды в системе, замените `:` с `__` (двойной символ подчеркивания).

<a name=options-config-objects></a>

## <a name="using-options-and-configuration-objects"></a>С помощью параметров и объектов конфигурации

Параметры шаблона используются классы пользовательские параметры для представления группу связанных параметров. Рекомендуется создать несвязанной классы для каждого компонента, в приложении. Выполните несвязанной классы.

* [Принцип разделения интерфейс (ISP)](http://deviq.com/interface-segregation-principle/) : классы зависеть только от параметров конфигурации, они используют.
* [Разделение областей ответственности](http://deviq.com/separation-of-concerns/) : параметры для разных частей приложения, не зависящие от них или связанные друг с другом.

Класс параметров должен быть абстрактным открытый конструктор без параметров. Пример:

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]

<a name=options-example></a>

В следующем коде включен поставщик конфигурации JSON. `MyOptions` Класса добавляется в контейнер службы и привязан к конфигурации.

[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]

Следующие [контроллера](../mvc/controllers/index.md) использует [конструктор внедрения зависимостей](xref:fundamentals/dependency-injection#what-is-dependency-injection) на [ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) для доступа к параметрам:

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]

Со следующими *appsettings.json* файла:

[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]

`HomeController.Index` Возвращает `option1 = value1_from_json, option2 = 2`.

Типичные приложения не привязки к файлу параметры единого вся конфигурация. Позднее будет показано как использовать `GetSection` для привязки к разделу.

В следующем коде секунды `IConfigureOptions<TOptions>` служба добавляется в контейнер службы. Она использует делегат для настройки привязки с `MyOptions`.

[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]

Можно добавить несколько поставщиков конфигурации. Поставщики конфигурации, доступные в пакетах NuGet. Они применяются в порядке, в котором они зарегистрированы.

Каждый вызов `Configure<TOptions>` добавляет `IConfigureOptions<TOptions>` службу в контейнер службы. В предыдущем примере значения `Option1` и `Option2` указываются в *appsettings.json* --, но значение `Option1` переопределяется настроенных делегата. 

При включении более одной конфигурации службы последнего источник конфигурации указано «побеждает» (задает значение конфигурации). В приведенном выше коде `HomeController.Index` возвращает `option1 = value1_from_action, option2 = 2`.

При привязке параметров конфигурации, каждое свойство в типе параметры привязан ключу конфигурации формы `property[:sub-property:]`. Например `MyOptions.Option1` свойства, связанного с ключом `Option1`, который считывается из `option1` свойство в *appsettings.json*. Далее в этой статье показан пример вложенного свойства.

В следующем коде третий `IConfigureOptions<TOptions>` служба добавляется в контейнер службы. Привязывает `MySubOptions` к разделу `subsection` из *appsettings.json* файла:

[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]

Примечание: Этот метод расширения требуется `Microsoft.Extensions.Options.ConfigurationExtensions` пакет NuGet.

С помощью следующих *appsettings.json* файла:

[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]

`MySubOptions` Класса:

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]

Со следующими `Controller`:

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]

`subOption1 = subvalue1_from_json, subOption2 = 200`возвращается.

Можно также предлагаемых в модель представления или вставить `IOptions<TOptions>` непосредственно в представлении:

[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]

<a name=in-memory-provider></a>

## <a name="ioptionssnapshot"></a>IOptionsSnapshot

*Требуется ASP.NET Core 1.1 или более поздней версии.*

`IOptionsSnapshot`поддерживает повторной загрузки данных конфигурации при изменении файла конфигурации. Он имеет минимальные издержки. С помощью `IOptionsSnapshot` с `reloadOnChange: true`, параметры привязываются к `IConfiguration` и загружаются в память при изменении.

В следующем образце показано, как новый `IOptionsSnapshot` создается после *config.json* изменений. Запросы к серверу будут возвращает такой же время, когда *config.json* имеет **не** изменен. После первого запроса *config.json* изменения будут отображаться новое время.

[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]

На следующем рисунке показана вывод server:

![Отображение изображений браузера «последнее обновление: 11/22/2016 4:43:00»](configuration/_static/first.png)

Обновить браузер не изменяет значение сообщения или время, отображаемое (когда *config.json* не изменились).

Изменить и сохранить *config.json* , а затем обновить браузер:

![Отображение изображений браузера «последнее обновление для e: 11/22/2016 4:53:00»](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>Поставщик в памяти и привязки для класса POCO

Следующий пример демонстрирует использование поставщика в памяти и связать с классом:

[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]

Значения конфигурации возвращается в виде строк, но привязка позволяет для создания объектов. Привязка позволяет получить POCO объекты или графы даже весь объект. Следующий пример показано, как выполнить привязку к `MyWindow` и использовать шаблон, параметры с приложением ASP.NET MVC основных компонентов:

[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]

[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]

Привязка пользовательского класса в `ConfigureServices` при создании узла:

[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]

Параметры отображения из `HomeController`:

[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]

### <a name="getvalue"></a>GetValue

В следующем образце показано [GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) метода расширения:

[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

ConfigurationBinder `GetValue<T>` метод позволяет задать значение по умолчанию (80 в образце). `GetValue<T>`для простых сценариях и не привязывается к весь раздел. `GetValue<T>`Возвращает скалярные значения из `GetSection(key).Value` преобразован к определенному типу.

## <a name="binding-to-an-object-graph"></a>Привязка к графа объекта

Можно выполнить привязку рекурсивно для каждого объекта в классе. Рассмотрим следующий `AppOptions` класса:

[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]

Следующий пример привязывает к `AppOptions` класса:

[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]

**ASP.NET Core 1.1** и более поздней версии можно использовать `Get<T>`, которая работает с весь раздел. `Get<T>`может быть более convienent, чем при использовании `Bind`. Следующий код показывает, как использовать `Get<T>` с примером выше:

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

С помощью следующих *appsettings.json* файла:

[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]

Программа выводит `Height 11`.

Следующий код может использоваться для модульного тестирования конфигурации:

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

    var options = new AppOptions();
    config.GetSection("App").Bind(options);

    Assert.Equal("Rick", options.Profile.Machine);
    Assert.Equal(11, options.Window.Height);
    Assert.Equal(11, options.Window.Width);
    Assert.Equal("connectionstring", options.Connection.Value);
}
```

<a name=custom-config-providers></a>

## <a name="basic-sample-of-entity-framework-custom-provider"></a>Базовый образец пользовательского поставщика Entity Framework

В этом разделе создается основная конфигурация поставщика, который считывает пары "имя значение" из базы данных с помощью EF. 

Определение `ConfigurationValue` сущности для хранения значения конфигурации базы данных:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

Добавить `ConfigurationContext` хранение и доступ к настроенные значения:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

Создайте класс, реализующий [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

Создание настраиваемого поставщика конфигурации путем наследования от [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).  Поставщик конфигурации инициализирует базы данных при пустом:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

При запуске образца отображаются выделенные значения из базы данных («value_from_ef_1» и «value_from_ef_2»).

Можно добавить `EFConfigSource` метод расширения для добавления источник конфигурации:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

Ниже показано, как использовать собственную `EFConfigProvider`:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

Обратите внимание, в этом примере добавляется пользовательский `EFConfigProvider` после поставщика JSON, поэтому все параметры из базы данных переопределяют параметры из *appsettings.json* файла.

С помощью следующих *appsettings.json* файла:

[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]

Отобразится следующее:

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>Поставщик конфигурации Командная строка

[Командная строка конфигурации поставщика](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) получает пар ключ значение аргумента командной строки для конфигурации во время выполнения.

[Просмотреть или загрузить образец конфигурации Командная строка](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample/CommandLine)

### <a name="setting-up-the-provider"></a>Настройка поставщика

# <a name="basic-configurationtabbasicconfiguration"></a>[Основная конфигурация](#tab/basicconfiguration)

Чтобы активировать настройки командной строки, вызовите `AddCommandLine` метод расширения в экземпляре [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program.cs?highlight=18,21)]

Выполнение кода, выводятся следующие данные:

```console
MachineName: MairaPC
Left: 1980
```

Передача аргумента пары "ключ значение" в командной строке приводит к изменению значений `Profile:MachineName` и `App:MainWindow:Left`:

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

В окне консоли отображается:

```console
MachineName: BartPC
Left: 1979
```

Чтобы переопределить конфигурацию, сформированную другими поставщиками конфигурации с конфигурацией командной строки, вызовите `AddCommandLine` последней в `ConfigurationBuilder`:

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Типичные приложения ASP.NET Core 2.x использовать метод статических удобных `CreateDefaultBuilder` для создания узла:

[!code-csharp[Main](configuration/sample_snapshot/Program.cs?highlight=12)]

`CreateDefaultBuilder`Загружает дополнительной конфигурации из *appsettings.json*, *appsettings. {} Среда} .json*, [секретов пользователя](xref:security/app-secrets) (в `Development` среды), переменные и аргументы командной строки. Поставщик конфигурации CommandLine вызывается. Последнего вызова поставщика позволяет ранее вызов аргументы командной строки, которые передаются во время выполнения для переопределения набора конфигурации с других поставщиков конфигурации.

Обратите внимание, что для *appsettings* файлов, в которой `reloadOnChange` включен. Аргументы командной строки переопределяются в том случае, если совпадающее значение конфигурации в *appsettings* был изменен после запуска приложения.

> [!NOTE]
> В качестве альтернативы с помощью `CreateDefaultBuilder` создание узла с помощью метода [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) и вручную создания конфигурации с [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) поддерживается в ASP.NET Core 2.x. См. на вкладке 1.x ASP.NET Core для получения дополнительной информации.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Создание [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) и вызвать `AddCommandLine` метод, чтобы использовать поставщик конфигурации Командная строка. Последнего вызова поставщика позволяет ранее вызов аргументы командной строки, которые передаются во время выполнения для переопределения набора конфигурации с других поставщиков конфигурации. Применить конфигурацию [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) с `UseConfiguration` метод:

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>Аргументы

Аргументы, передаваемые в командной строке должен соответствовать одному из двух форматов, показано в следующей таблице.

| Аргумент формата                                                     | Пример        |
| ------------------------------------------------------------------- | :------------: |
| Один аргумент: пара ключ значение, разделенные знаком равенства (`=`) | `key1=value`   |
| Последовательность из двух аргументов: пара ключ значение, разделенные пробелом    | `/key1 value1` |

**Один аргумент**

Значение должно следовать знак равенства (`=`). Может иметь значение null (например, `mykey=`).

Ключ может иметь префикс.

| Префикс ключа               | Пример         |
| ------------------------ | :-------------: |
| Без префикса                | `key1=value1`   |
| Единый тире (`-`) &#8224; | `-key2=value2`  |
| Двух дефисов (`--`)        | `--key3=value3` |
| Косая черта (`/`)      | `/key4=value4`  |

&#8224; Ключ с префиксом один тире (`-`) должны быть предоставлены в [переключения сопоставления](#switch-mappings), описаны ниже.

Пример команды:

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

Примечание: Если `-key1` не существуют в [переключения сопоставления](#switch-mappings) заданное поставщиком конфигурации `FormatException` возникает исключение.

**Последовательность из двух аргументов**

Значение не может иметь значение null и должны соответствовать ключ, разделенные пробелом.

Ключ должен иметь префикс.

| Префикс ключа               | Пример         |
| ------------------------ | :-------------: |
| Единый тире (`-`) &#8224; | `-key1 value1`  |
| Двух дефисов (`--`)        | `--key2 value2` |
| Косая черта (`/`)      | `/key3 value3`  |

&#8224; Ключ с префиксом один тире (`-`) должны быть предоставлены в [переключения сопоставления](#switch-mappings), описаны ниже.

Пример команды:

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

Примечание: Если `-key1` не существуют в [переключения сопоставления](#switch-mappings) заданное поставщиком конфигурации `FormatException` возникает исключение.

### <a name="duplicate-keys"></a>Повторяющиеся ключи

Если указаны повторяющиеся ключи, используется последний пару ключ значение.

### <a name="switch-mappings"></a>Переключение сопоставления

При построении конфигурации с вручную `ConfigurationBuilder`, при необходимости можно предоставить ключ словаря сопоставления для `AddCommandLine` метод. Коммутатор сопоставления позволяют предоставить логика замены имя ключа.

При использовании словарь сопоставлений коммутатора словаре проверяется на ключ, который совпадает с ключом, предоставляемые аргумента командной строки. Если ключ командной строки, найден в словаре, значение словаря (замены ключей) передается обратно, чтобы задать конфигурацию. Сопоставление коммутатора не требуется для командной строки раздела префиксом с одного дефиса (`-`).

Переключение ключа правила сопоставления словаря:

* Параметры должны начинаться с дефиса (`-`) или двойной дефис (`--`).
* Словарь сопоставлений коммутатора не должен содержать повторяющиеся ключи.

В следующем примере `GetSwitchMappings` метод позволяет аргументы командной строки для использования одного тире (`-`) ключа префикс и избежать начальные подраздела префиксы.

[!code-csharp[Main](configuration/sample/CommandLine/Program.cs?highlight=10-19,32)]

Без указания аргументов командной строки, передаваемые словаре `AddInMemoryCollection` задает значения конфигурации. Запуск приложения с помощью следующей команды:

```console
dotnet run
```

В окне консоли отображается:

```console
MachineName: RickPC
Left: 1980
```

Используйте следующие параметры для передачи в параметрах конфигурации.

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

В окне консоли отображается:

```console
MachineName: DahliaPC
Left: 1984
```

Вновь созданный словаре сопоставлений коммутатора он содержит данные, показанные в следующей таблице.

| Ключ            | Значение                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

Чтобы продемонстрировать ключа переключения, используя словарь, выполните следующую команду:

```console
dotnet run -MachineName=ChadPC -Left=1988
```

Ключи командной строки, меняются местами. В окне консоли отображаются значения конфигурации для `Profile:MachineName` и `App:MainWindow:Left`:

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a>В файле web.config

Объект *web.config* файл будет необходим для размещения приложения в IIS или IIS Express. *Web.config* включает AspNetCoreModule в службах IIS для запуска приложения. Параметры в *web.config* включить AspNetCoreModule в службах IIS для запуска приложения и настройки других параметров IIS и модулей. Если вы используете Visual Studio и удалить *web.config*, Visual Studio создаст новое.

## <a name="additional-notes"></a>Дополнительные сведения

* После внедрения зависимости (DI) не задано до `ConfigureServices` вызывается.
* Система конфигурации не учитывать DI.
* `IConfiguration`имеет две специализации.
  * `IConfigurationRoot`Используется для корневого узла. Можно инициировать перезагрузку.
  * `IConfigurationSection`Представляет раздел конфигурации значения. `GetSection` И `GetChildren` методы возвращают `IConfigurationSection`.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Работа с несколькими средами](environments.md)
* [Безопасное хранение секретов приложений во время разработки](../security/app-secrets.md)
* [Размещение в ASP.NET Core](xref:fundamentals/hosting)
* [Введение зависимостей](dependency-injection.md)
* [Поставщик конфигурации Azure Key Vault](xref:security/key-vault-configuration)
