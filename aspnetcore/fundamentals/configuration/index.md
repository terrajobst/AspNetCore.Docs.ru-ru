---
title: Конфигурация в .NET Core
author: guardrex
description: Узнайте, как использовать API конфигурации для настройки приложения ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: df49286c3f050b8e90cb5427cf03e2b896a39294
ms.sourcegitcommit: c81ef12a1b6e6ac838e5e07042717cf492e6635b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2020
ms.locfileid: "76885562"
---
# <a name="configuration-in-aspnet-core"></a>Конфигурация в .NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Конфигурация приложения в ASP.NET Core основана на парах "ключ — значение", установленных *поставщиками конфигурации*. Поставщики конфигурации получают данные конфигурации в парах "ключ — значение" из различных источников:

* Хранилище ключей Azure;
* конфигурация приложения Azure;
* аргументов командной строки;
* а также пользовательские поставщики (устанавливаемые или создаваемые).
* справочных файлов;
* Переменные среды
* объектов .NET в памяти;
* файлов параметров;

::: moniker range=">= aspnetcore-3.0"

Пакеты конфигурации для распространенных вариантов провайдеров конфигурации ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) включаются платформой неявным образом.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Пакеты конфигурации для распространенных вариантов провайдеров конфигурации ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) включаются в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

::: moniker-end

В приведенных ниже и представленных в образце приложения примерах кода используется пространство имен <xref:Microsoft.Extensions.Configuration>:

```csharp
using Microsoft.Extensions.Configuration;
```

*Шаблон параметров* является расширением конфигурации основных понятий, описанных в этом разделе. Параметры используют классы для представления групп связанных настроек. Для получения дополнительной информации см. <xref:fundamentals/configuration/options>.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="host-versus-app-configuration"></a>Конфигурация узла и приложения

Перед настройкой и запуском приложения настройте и запустите *узел*. Узел отвечает за запуск приложения и управление временем существования. Как приложение, так и узел настраиваются с использованием поставщиков конфигурации, описанных в этом разделе. Пары "ключ — значение" конфигурации узлов также включаются в конфигурацию приложения. Дополнительные сведения о том, как используются поставщики конфигурации при создании узла и как влияют источники конфигурации на узел, см. в разделе <xref:fundamentals/index#host>.

## <a name="other-configuration"></a>Другая конфигурация

Этот раздел относится только к *конфигурации приложений*. Другие аспекты запуска и размещения приложений ASP.NET Core настраиваются с помощью файлов конфигурации, которые не рассматриваются в этом разделе.

* *launch.json*/*launchSettings.json* — это файлы конфигурации инструментов для среды разработки, описанные в следующих источниках:
  * В <xref:fundamentals/environments#development>.
  * В документации, где показано, как файлы используются для настройки приложений ASP.NET Core для сценариев разработки.
* *web.config* — это файл конфигурации сервера, описанный в следующих источниках:
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

См. сведения о переносе конфигурации приложения из более ранних версий ASP.NET: <xref:migration/proper-to-2x/index#store-configurations>.

## <a name="default-configuration"></a>Конфигурация по умолчанию

::: moniker range=">= aspnetcore-3.0"

Веб-приложения на основе шаблонов [dotnet new](/dotnet/core/tools/dotnet-new) ASP.NET Core вызывают <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> при создании узла. `CreateDefaultBuilder` предоставляет конфигурацию по умолчанию для приложения в следующем порядке:

Приведенные ниже сведения относятся к приложениям, использующим [универсальный узел](xref:fundamentals/host/generic-host). Подробные сведения о конфигурации по умолчанию при использовании [веб-узла](xref:fundamentals/host/web-host) см. в [разделе о версии ASP.NET Core 2.2 в этой статье](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).

* Существуют следующие способы предоставления конфигурации узла.
  * Переменные среды с префиксом `DOTNET_` (например, `DOTNET_ENVIRONMENT`), использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider). Префикс (`DOTNET_`) исключается при загрузке пар "ключ — значение" конфигурации.
  * Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).
* Устанавливается конфигурация веб-узла по умолчанию (`ConfigureWebHostDefaults`):
  * Kestrel используется в качестве веб-сервера и настраивается с помощью поставщиков конфигурации приложения.
  * Добавьте ПО промежуточного слоя фильтрации узлов.
  * Если переменной среды `ASPNETCORE_FORWARDEDHEADERS_ENABLED` присвоено значение `true`, добавьте ПО промежуточного слоя перенаправления заголовков.
  * Включите интеграцию служб IIS.
* Конфигурация приложения предоставляется из следующих ресурсов:
  * Файл *appsettings.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).
  * Файл *appsettings.{Environment}.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).
  * [Менеджера секретов](xref:security/app-secrets), когда приложение выполняется в среде `Development` с использованием начальных сборок.
  * Переменные среды, использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).
  * Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Веб-приложения на основе шаблонов [dotnet new](/dotnet/core/tools/dotnet-new) ASP.NET Core вызывают <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> при создании узла. `CreateDefaultBuilder` предоставляет конфигурацию по умолчанию для приложения в следующем порядке:

Приведенные ниже сведения относятся к приложениям, использующим [веб-узел](xref:fundamentals/host/web-host). Подробные сведения о конфигурации по умолчанию при использовании [универсального узла](xref:fundamentals/host/generic-host) см. в [последней версии этой статьи](xref:fundamentals/configuration/index).

* Существуют следующие способы предоставления конфигурации узла.
  * Переменные среды с префиксом `ASPNETCORE_` (например, `ASPNETCORE_ENVIRONMENT`), использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider). Префикс (`ASPNETCORE_`) исключается при загрузке пар "ключ — значение" конфигурации.
  * Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).
* Конфигурация приложения предоставляется из следующих ресурсов:
  * Файл *appsettings.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).
  * Файл *appsettings.{Environment}.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).
  * [Менеджера секретов](xref:security/app-secrets), когда приложение выполняется в среде `Development` с использованием начальных сборок.
  * Переменные среды, использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).
  * Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).

::: moniker-end

## <a name="security"></a>Безопасность

Применяйте описанные ниже методики для защиты конфиденциальных данных конфигурации.

* Никогда не храните пароли или другие конфиденциальные данные в коде поставщика конфигурации или в файлах конфигурации обычного текста.
* Не используйте секреты рабочей среды в средах разработки и тестирования.
* Указывайте секреты вне проекта, чтобы их нельзя было случайно зафиксировать в репозитории с исходным кодом.

Дополнительные сведения см. в следующих разделах:

* <xref:fundamentals/environments>
* <xref:security/app-secrets> &ndash; содержит рекомендации по использованию переменных среды для хранения конфиденциальных данных. Менеджер секретов использует поставщик конфигурации файла для хранения конфиденциальных данных пользователя в файле JSON в локальной системе. Поставщик конфигурации файлов описан ниже в этом разделе.

В [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) безопасно хранятся секреты приложений ASP.NET Core. Для получения дополнительной информации см. <xref:security/key-vault-configuration>.

## <a name="hierarchical-configuration-data"></a>Иерархическая модель конфигурации

API конфигурации способен поддерживать иерархические данные конфигурации, выполняя преобразование в плоскую структуру иерархических данных с использованием разделителя в ключах конфигурации.

В следующем файле JSON существуют четыре ключа в структурированной иерархии двух разделов.

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

При считывании файла в конфигурацию для сохранения исходной иерархической структуры данных источника конфигурации создаются уникальные ключи. Разделы и ключи преобразовываются в плоскую структуру с использованием двоеточия (`:`) для сохранения исходной структуры.

* section0:key0
* section0:key1
* section1:key0
* section1:key1

Методы <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> и <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> доступны для изолирования разделов и дочерних элементов раздела в данных конфигурации. Эти методы описаны далее в разделе [GetSection, GetChildren и Exists](#getsection-getchildren-and-exists).

## <a name="conventions"></a>Соглашения

### <a name="sources-and-providers"></a>Источники и поставщики

При запуске приложения источники конфигурации считываются в порядке, в котором были указаны их поставщики конфигурации.

Поставщики конфигурации, которые реализуют обнаружение изменений, имеют возможность перезагрузить конфигурацию при изменении базовых параметров. Так, поставщик файла конфигурации (подробнее о нем далее в этой статье) и [поставщик конфигурации Azure Key Vault](xref:security/key-vault-configuration) реализуют обнаружение изменений.

Объект <xref:Microsoft.Extensions.Configuration.IConfiguration> доступен в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) приложения. <xref:Microsoft.Extensions.Configuration.IConfiguration> можно внедрить в <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> Razor Pages или <xref:Microsoft.AspNetCore.Mvc.Controller> MVC, чтобы получить конфигурацию для класса.

В следующих примерах поле `_config` используется для доступа к значениям конфигурации:

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }
}
```

```csharp
public class HomeController : Controller
{
    private readonly IConfiguration _config;

    public HomeController(IConfiguration config)
    {
        _config = config;
    }
}
```

Поставщики конфигурации не могут использовать контейнер DI, так как он недоступен при настройке узла.

### <a name="keys"></a>Клавиши

В ключах конфигурации приняты следующие соглашения.

* В ключах не учитывается регистр символов. Например `ConnectionString` и `connectionstring` обрабатываются как эквивалентные ключи.
* Если значение для одного и того же ключа установлено одним и тем же или разными поставщиками конфигурации, последним значением, установленным на ключе, является используемое значение.
* Иерархические ключи
  * При взаимодействии с API конфигурации разделитель-двоеточие (`:`) поддерживается на всех платформах.
  * В переменных среды разделитель-двоеточие может не работать на всех платформах. Двойной знак подчеркивания (`__`) поддерживается на всех платформах и автоматически преобразовывается в двоеточие.
  * В хранилище ключей Azure иерархические ключи используют `--` (два дефиса) в качестве разделителя. Чтобы заменить дефисы двоеточием, при загрузке секретов в конфигурацию приложения напишите код.
* <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации. Привязка массива описана в разделе [Привязка массива к классу](#bind-an-array-to-a-class).

### <a name="values"></a>Значения

В значениях конфигурации учитываются следующие соглашения.

* Значения являются строками.
* Значение NULL не может храниться в конфигурации или быть привязанным к объектам.

## <a name="providers"></a>Поставщики

В следующей таблице показаны поставщики конфигурации, доступные для приложений ASP.NET Core.

| Поставщик | Предоставляет конфигурацию из &hellip; |
| -------- | ----------------------------------- |
| [Поставщик конфигурации хранилища ключей Azure](xref:security/key-vault-configuration) (раздел *Безопасность*) | Хранилище ключей Azure; |
| [Поставщик конфигурации приложений Azure](/azure/azure-app-configuration/quickstart-aspnet-core-app) (документация Azure) | конфигурация приложения Azure; |
| [Поставщик конфигурации командной строки](#command-line-configuration-provider) | Параметры командной строки |
| [Поставщик пользовательской конфигурации](#custom-configuration-provider) | Источник пользователя |
| [Поставщик конфигурации переменных среды](#environment-variables-configuration-provider) | Переменные среды |
| [Поставщик пользовательской конфигурации](#file-configuration-provider) | Файлы (INI, JSON, XML) |
| [Поставщик конфигурации ключа для каждого файла](#key-per-file-configuration-provider) | Справочные файлы |
| [Поставщик конфигурации памяти](#memory-configuration-provider) | Коллекции оперативной памяти |
| [Секреты пользователей (Менеджер секретов)](xref:security/app-secrets) (раздел *Безопасность*) | Файл в каталоге профиля пользователя |

Источники конфигурации считываются в том порядке, в котором при запуске указываются их поставщики конфигурации. Поставщики конфигурации в этом разделе описаны в алфавитном порядке, а не в необходимом вам порядке в коде. Порядок поставщиков конфигурации в коде соответствует приоритетам ваших основных источников конфигурации, требуемых приложением.

Типичная последовательность поставщиков конфигурации.

1. Файлы (*appsettings.json*, *appsettings.{Environment}.json*, где `{Environment}` — это текущая среда размещения приложения)
1. [Azure Key Vault](xref:security/key-vault-configuration);
1. [Секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки)
1. Переменные среды
1. аргументов командной строки;

Общепринятой практикой является размещение поставщика конфигурации командной строки последним в ряду поставщиков, чтобы аргументы командной строки могли переопределять конфигурацию, установленную другими поставщиками.

Предыдущая последовательность поставщиков используется при инициализации нового построителя узла с помощью `CreateDefaultBuilder`. Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).

::: moniker range=">= aspnetcore-3.0"

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a>Настройка построителя узла с помощью ConfigureHostConfiguration

Чтобы настроить построитель узла, вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> и укажите конфигурацию. `ConfigureHostConfiguration` используется для инициализации <xref:Microsoft.Extensions.Hosting.IHostEnvironment> для дальнейшего использования в процессе сборки. Метод `ConfigureHostConfiguration` может вызываться несколько раз с накоплением результатов.

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureHostConfiguration(config =>
        {
            var dict = new Dictionary<string, string>
            {
                {"MemoryCollectionKey1", "value1"},
                {"MemoryCollectionKey2", "value2"}
            };

            config.AddInMemoryCollection(dict);
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="configure-the-host-builder-with-useconfiguration"></a>Настройка построителя узла с помощью UseConfiguration

Чтобы настроить построитель узла, вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> в построителе узла с конфигурацией.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

    var config = new ConfigurationBuilder()
        .AddInMemoryCollection(dict)
        .Build();

    return WebHost.CreateDefaultBuilder(args)
        .UseConfiguration(config)
        .UseStartup<Startup>();
}
```

::: moniker-end

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Вызовите `ConfigureAppConfiguration` при сборке узла, чтобы указать поставщики конфигурации приложения в дополнение к тем, которые автоматически добавлены `CreateDefaultBuilder`.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

### <a name="override-previous-configuration-with-command-line-arguments"></a>Переопределение предыдущей конфигурации с помощью аргументов командной строки

Чтобы предоставить конфигурацию приложения, которую можно переопределить с помощью аргументов командной строки, вызовите метод `AddCommandLine` последним:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a>Удаление поставщиков, добавленных CreateDefaultBuilder

Чтобы удалить поставщиков, добавленных `CreateDefaultBuilder`, сначала вызовите [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) в [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources):

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a>Использование конфигурации во время запуска приложения

Конфигурация, предоставленная приложению в `ConfigureAppConfiguration`, доступна во время запуска приложения, включая `Startup.ConfigureServices`. Дополнительные сведения см. в разделе [Доступ к конфигурации во время запуска](#access-configuration-during-startup).

## <a name="command-line-configuration-provider"></a>Поставщик конфигурации командной строки

<xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> загружает конфигурацию из пары "ключ — значение" аргумента командной строки в среде выполнения.

Чтобы активировать конфигурацию командной строки, вызывается метод расширения <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> для экземпляра <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

`AddCommandLine` вызывается автоматически при вызове `CreateDefaultBuilder(string [])`. Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).

`CreateDefaultBuilder` также загружает следующее:

* дополнительную конфигурацию из файлов *appsettings.json* и *appsettings.{Environment}.json*;
* [секреты пользователя (Менеджер секретов)](xref:security/app-secrets) в среде разработки.
* Переменные среды.

`CreateDefaultBuilder` добавляет последним поставщика конфигурации командной строки. Аргументы командной строки передаются в набор конфигурации переопределения среды выполнения, установленной другими поставщиками.

`CreateDefaultBuilder` действует, когда создается узел. Поэтому конфигурация командной строки, активированная с помощью `CreateDefaultBuilder`, может повлиять на настройку узла.

Для приложений на основе шаблонов ASP.NET Core метод `AddCommandLine` уже был вызван методом `CreateDefaultBuilder`. Чтобы добавить дополнительные поставщики конфигурации и обеспечить возможность переопределения конфигурации от этих поставщиков с помощью аргументов командной строки, вызовите дополнительные поставщики приложения в `ConfigureAppConfiguration` и вызовите `AddCommandLine` в последнюю очередь.

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

**Пример**

Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.

1. Откройте командную строку в каталоге проекта.
1. Поставьте аргумент командной строки в команду `dotnet run`: `dotnet run CommandLineKey=CommandLineValue`.
1. После запуска приложения откройте браузер в приложении по адресу `http://localhost:5000`.
1. Обратите внимание, что вывод содержит пару "ключ — значение" для аргумента командной строки конфигурации, предоставленного для `dotnet run`.

### <a name="arguments"></a>Аргументы

Значение должно соответствовать знаку равенства (`=`), или ключ должен иметь префикс (`--` или `/`), когда значение следует за пробелом. Значение не требуется, если используется знак равенства (например, `CommandLineKey=`).

| Префикс ключа               | Пример                                                |
| ------------------------ | ------------------------------------------------------ |
| Без префикса                | `CommandLineKey1=value1`                               |
| Два дефиса (`--`)        | `--CommandLineKey2=value2`, `--CommandLineKey2 value2` |
| Прямая косая черта (`/`)      | `/CommandLineKey3=value3`, `/CommandLineKey3 value3`   |

В рамках одной и той же команды не смешивайте пары "ключ — значение" аргумента командной строки, которые используют знак равенства, с парами "ключ — значение", которые используют пробел.

Примеры команд.

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a>Сопоставления переключений

Сопоставление параметров позволяет указать логику замены имен ключей. Когда вручную создается конфигурация с помощью <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, методу <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> можно предоставить словарь сопоставления переключений.

В словаре сопоставлений переключений выполняется поиск ключа, который совпадает с ключом, предоставляемым аргументом командной строки. Если ключ в командной строке находится в словаре, значение словаря (замена ключа) передается обратно, чтобы установить пару "ключ — значение" в конфигурацию приложения. Сопоставление переключений необходимо для любого ключа командной строки с префиксом из одного дефиса (`-`).

Правила ключей из словаря сопоставления переключений:

* Переключения должны начинаться с дефиса (`-`) или двойного дефиса (`--`).
* Словарь сопоставлений переключений не должен содержать повторяющиеся ключи.

Создайте словарь сопоставления переключений. В следующем примере создаются два сопоставления переключений:

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

При сборке узла вызовите `AddCommandLine` со словарем сопоставлений переключений:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

Для приложений, использующих сопоставления переключений, в вызове `CreateDefaultBuilder` аргументы передаваться не должны. Вызов команды `AddCommandLine` метода `CreateDefaultBuilder` не включает сопоставленные переключения, и нет возможности передать словарь сопоставления переключений в `CreateDefaultBuilder`. Чтобы решить эту проблему, нужно не передавать аргументы команде `CreateDefaultBuilder`, а позволить методу `AddCommandLine` метода `ConfigurationBuilder` обрабатывать как аргументы, так и словарь сопоставления параметров.

Созданный словарь сопоставлений переключений содержит данные, показанные в следующей таблице.

| Ключ       | Значение             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

Если ключи сопоставления переключений используются при запуске приложения, конфигурация принимает значение конфигурации в ключе, предоставленном в словаре.

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

После выполнения предыдущей команды конфигурация содержит значения, показанные в следующей таблице.

| Ключ               | Значение    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a>Поставщик конфигурации переменных среды

<xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> загружает конфигурацию из пары "ключ — значение" переменной среды выполнения.

Чтобы активировать конфигурацию переменных среды, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

[Служба приложений Azure](https://azure.microsoft.com/services/app-service/) позволяет задать переменные среды на портале Azure, который может переопределить конфигурацию приложения, используя поставщик конфигурации переменных среды. Дополнительные сведения см. в руководстве по [переопределению конфигурации приложения Azure с помощью портала Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).

::: moniker range=">= aspnetcore-3.0"

`AddEnvironmentVariables` используется для загрузки переменных среды, имеющих префикс `DOTNET_`, для [конфигурации узла](#host-versus-app-configuration) при инициализации нового построителя узла с [универсальным узлом](xref:fundamentals/host/generic-host) и вызове `CreateDefaultBuilder`. Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).

::: moniker-end

::: moniker range="< aspnetcore-3.0"

`AddEnvironmentVariables` используется для загрузки переменных среды, имеющих префикс `ASPNETCORE_`, для [конфигурации узла](#host-versus-app-configuration) при инициализации нового построителя узла с [веб-узлом](xref:fundamentals/host/web-host) и вызове `CreateDefaultBuilder`. Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).

::: moniker-end

`CreateDefaultBuilder` также загружает следующее:

* конфигурация приложения на основе переменных среды без префикса путем вызова `AddEnvironmentVariables` без префикса;
* дополнительную конфигурацию из файлов *appsettings.json* и *appsettings.{Environment}.json*;
* [секреты пользователя (Менеджер секретов)](xref:security/app-secrets) в среде разработки.
* аргументы командной строки.

Поставщик конфигурации переменных среды вызывается после выполнения настройки с помощью секретов пользователя и файлов *appsettings*. Вызов поставщика в этой позиции разрешает чтение переменных среды выполнения, чтобы переопределить конфигурацию, заданную секретом пользователя и файлом *appsettings*.

Чтобы добавить конфигурацию приложения из дополнительных переменных среды, вызовите дополнительные поставщики приложения в `ConfigureAppConfiguration`, а затем вызовите `AddEnvironmentVariables` с префиксом:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

Вызовите `AddEnvironmentVariables` последним, чтобы разрешить переменным среды с заданным префиксом переопределять значения от других поставщиков.

**Пример**

Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов `AddEnvironmentVariables`.

1. Выполните пример приложения. Откройте в приложении браузер с адресом `http://localhost:5000`.
1. Обратите внимание, что выходные данные содержат пару "ключ — значение" для переменной среды `ENVIRONMENT`. Значение отражает среду, в которой выполняется приложение, обычно при локальном запуске это `Development`.

Чтобы список переменных среды, отображаемый приложением, был коротким, приложение фильтрует переменные среды. См. пример файла *Pages/Index.cshtml.cs* приложения.

Чтобы просмотреть все переменные среды, доступные для приложения, измените значение `FilteredConfiguration` в *Pages/Index.cshtml.cs* на следующее:

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a>Префиксы

Переменные среды, которые загружаются в конфигурацию приложения, фильтруются при добавлении префикса к методу `AddEnvironmentVariables`. Например, чтобы отфильтровать переменные среды по префиксу `CUSTOM_`, введите префикс поставщику конфигурации:

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

Префикс отделяется при создании пары конфигурации "ключ — значение".

При создании построителя узла конфигурация узла предоставляется переменными среды. Дополнительные сведения о префиксе, используемом для этих переменных среды, см. в разделе [Конфигурация по умолчанию](#default-configuration).

**Префиксы строк подключения**

API конфигурации имеет специальные правила обработки для четырех строк подключения переменных среды, связанных с настройкой строк подключения Azure для среды приложения. Если префикс не указан в `AddEnvironmentVariables`, переменные среды с префиксами, указанными в таблице, загружаются в приложение.

| Префикс строки подключения | Поставщик |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | Поставщик пользователя |
| `MYSQLCONNSTR_` | [MySQL](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [База данных SQL Azure](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [SQL Server](https://www.microsoft.com/sql-server/) |

Когда переменная среды обнаруживается и загружается в конфигурацию с одним из четырех префиксов, приведенных в таблице, происходит следующее.

* Ключ конфигурации создается путем удаления префикса переменных среды и добавления ключа раздела конфигурации (`ConnectionStrings`).
* Создается новая пара "ключ — значение" конфигурации, которая представляет поставщика подключения базы данных (за исключением `CUSTOMCONNSTR_`, который не имеет указанного поставщика).

| Ключ переменной среды | Преобразованный ключ конфигурации | Запись конфигурации поставщика                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | Запись конфигурации не создана.                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | Ключ: `ConnectionStrings:<KEY>_ProviderName`:<br>Значение: `MySql.Data.MySqlClient` |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | Ключ: `ConnectionStrings:<KEY>_ProviderName`:<br>Значение: `System.Data.SqlClient`  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | Ключ: `ConnectionStrings:<KEY>_ProviderName`:<br>Значение: `System.Data.SqlClient`  |

## <a name="file-configuration-provider"></a>Поставщик пользовательской конфигурации

<xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> является базовым классом для загрузки конфигурации из файловой системы. Следующие поставщики конфигурации предназначены для определенных типов файлов:

* [Поставщик конфигурации INI](#ini-configuration-provider)
* [Поставщик конфигурации JSON](#json-configuration-provider)
* [Поставщик конфигурации XML](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a>Поставщик конфигурации INI

<xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> загружает конфигурацию из пары "ключ — значение" INI-файла во время выполнения.

Чтобы активировать конфигурацию INI-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Двоеточие можно использовать как разделитель раздела в конфигурации файла INI.

Перегрузки позволяют указать следующее.

* Файл является обязательным или нет.
* Будет ли перезагружена конфигурация, если файл изменится.
* <xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.

Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите `ConfigureAppConfiguration`.

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

Общий пример конфигурации INI-файла.

```ini
[section0]
key0=value
key1=value

[section1]
subsection:key=value

[section2:subsection0]
key=value

[section2:subsection1]
key=value
```

Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.

* section0:key0
* section0:key1
* section1:subsection:key
* section2:subsection0:key
* section2:subsection1:key

### <a name="json-configuration-provider"></a>Поставщик конфигурации JSON

<xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> загружает конфигурацию из пары "ключ — значение" JSON-файла в среде выполнения.

Чтобы активировать конфигурацию JSON-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Перегрузки позволяют указать следующее.

* Файл является обязательным или нет.
* Будет ли перезагружена конфигурация, если файл изменится.
* <xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.

`AddJsonFile` автоматически вызывается дважды при инициализации нового построителя узла с `CreateDefaultBuilder`. Метод вызывается для загрузки конфигурации из:

* *appsettings.json* &ndash; первым читается этот файл. Версия файла среды может переопределить значения, предоставленные *appsettings.json*.
* *appsettings.{Environment}.json* &ndash; версия среды файла загружается на основе [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).

Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).

`CreateDefaultBuilder` также загружает следующее:

* Переменные среды.
* [секреты пользователя (Менеджер секретов)](xref:security/app-secrets) в среде разработки.
* аргументы командной строки.

Сначала устанавливается поставщик конфигурации JSON-файлов. Таким образом, секреты пользователя, переменные среды и аргументы командной строки переопределят конфигурацию, заданную файлами *appsettings*.

Вызовите `ConfigureAppConfiguration` при сборке узла, чтобы указать конфигурацию приложения для файлов, отличных от *appsettings.json* и *appsettings.{Environment}.json*.

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

**Пример**

Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает два вызова `AddJsonFile`:

::: moniker range=">= aspnetcore-3.0"

* Первый вызов `AddJsonFile` загружает конфигурацию из *appsettings.json*:

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.json)]

* Второй вызов `AddJsonFile` загружает конфигурацию из *appsettings.{Environment}.json*. Для *appsettings.Development.json* в примере приложения загружается следующий файл:

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.Development.json)]

1. Выполните пример приложения. Откройте в приложении браузер с адресом `http://localhost:5000`.
1. Выходные данные содержат пары "ключ-значение" для конфигурации в зависимости от среды приложения. Уровень ведения журнала для ключа `Logging:LogLevel:Default` имеет значение `Debug` при запуске приложения в среде разработки.
1. Запустите пример приложения еще раз в рабочей среде:
   1. Откройте файл *Properties/launchSettings.json*.
   1. В профиле `ConfigurationSample` измените значение переменной среды `ASPNETCORE_ENVIRONMENT` на `Production`.
   1. Сохраните файл и запустите приложение с помощью `dotnet run` в командной оболочке.
1. Параметры в *appsettings.Development.json* больше не переопределяют параметры в *appsettings.json*. Уровень ведения журнала для ключа `Logging:LogLevel:Default` имеет значение `Information`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* Первый вызов `AddJsonFile` загружает конфигурацию из *appsettings.json*:

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* Второй вызов `AddJsonFile` загружает конфигурацию из *appsettings.{Environment}.json*. Для *appsettings.Development.json* в примере приложения загружается следующий файл:

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. Выполните пример приложения. Откройте в приложении браузер с адресом `http://localhost:5000`.
1. Выходные данные содержат пары "ключ-значение" для конфигурации в зависимости от среды приложения. Уровень ведения журнала для ключа `Logging:LogLevel:Default` имеет значение `Debug` при запуске приложения в среде разработки.
1. Запустите пример приложения еще раз в рабочей среде:
   1. Откройте файл *Properties/launchSettings.json*.
   1. В профиле `ConfigurationSample` измените значение переменной среды `ASPNETCORE_ENVIRONMENT` на `Production`.
   1. Сохраните файл и запустите приложение с помощью `dotnet run` в командной оболочке.
1. Параметры в *appsettings.Development.json* больше не переопределяют параметры в *appsettings.json*. Уровень ведения журнала для ключа `Logging:LogLevel:Default` имеет значение `Warning`.

::: moniker-end

### <a name="xml-configuration-provider"></a>Поставщик конфигурации XML

<xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> загружает конфигурацию из пары "ключ — значение" XML-файла в среде выполнения.

Чтобы активировать конфигурацию XML-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Перегрузки позволяют указать следующее.

* Файл является обязательным или нет.
* Будет ли перезагружена конфигурация, если файл изменится.
* <xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.

Корневой узел файла конфигурации не учитывается при создании пар "ключ — значение" конфигурации. Не указывайте в файле Document Type Definition (определение типа документа, DTD) или пространство имен.

Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите `ConfigureAppConfiguration`.

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

XML-файлы конфигурации могут использовать имена отдельных элементов для повторяющихся разделов.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section0>
    <key0>value</key0>
    <key1>value</key1>
  </section0>
  <section1>
    <key0>value</key0>
    <key1>value</key1>
  </section1>
</configuration>
```

Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.

* section0:key0
* section0:key1
* section1:key0
* section1:key1

Повторяющиеся элементы, использующие то же имя элемента, работают, если атрибут `name` используется для различения элементов.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section name="section0">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
  <section name="section1">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
</configuration>
```

Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.

* section:section0:key:key0
* section:section0:key:key1
* section:section1:key:key0
* section:section1:key:key1

Атрибуты можно использовать для предоставления значений.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.

* key:attribute
* section:key:attribute

## <a name="key-per-file-configuration-provider"></a>Поставщик конфигурации ключа для каждого файла

<xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> использует файлы каталога как пары "ключ — значение" конфигурации. Ключ является именем файла. Значение содержит содержимое файла. Поставщик конфигурации ключа для каждого файла используется в сценариях размещения Docker.

Чтобы активировать конфигурацию ключа для каждого файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>. Значение параметра `directoryPath` должно быть абсолютным путем к файлам.

Перегрузки позволяют указать следующее.

* `Action<KeyPerFileConfigurationSource>` — делегат, который настраивает источник.
* Обязательно ли указывать каталог и путь к каталогу.

Двойное подчеркивание (`__`) используется в качестве разделителя ключа конфигурации в именах файлов. Например, в имени файла `Logging__LogLevel__System` создается ключ конфигурации `Logging:LogLevel:System`.

Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите `ConfigureAppConfiguration`.

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a>Поставщик конфигурации памяти

<xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> использует коллекцию памяти в качестве пар "ключ — значение" конфигурации.

Чтобы активировать конфигурацию коллекции в памяти, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Поставщик конфигурации может инициализироваться значением `IEnumerable<KeyValuePair<String,String>>`.

Чтобы указать конфигурацию приложения, при сборке узла вызовите `ConfigureAppConfiguration`.

В следующем примере создается словарь конфигурации:

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

Словарь используется с вызовом `AddInMemoryCollection` для предоставления конфигурации:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a>GetValue

[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) извлекает одно значение из конфигурации с указанным ключом и преобразует его в указанный тип, не являющийся коллекцией. Перегрузка принимает значение по умолчанию.

В следующем примере происходит следующее:

* Из конфигурации извлекается строковое значение с ключом `NumberKey`. Если `NumberKey` отсутствует в ключах конфигурации, используется значение по умолчанию `99`.
* Значение получает тип `int`.
* Значение сохраняется в свойстве `NumberConfig` для использования на странице.

```csharp
public class IndexModel : PageModel
{
    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    public int NumberConfig { get; private set; }

    public void OnGet()
    {
        NumberConfig = _config.GetValue<int>("NumberKey", 99);
    }
}
```

## <a name="getsection-getchildren-and-exists"></a>GetSection, GetChildren и Exists

В следующих примерах рассмотрим файл JSON. Четыре ключа находятся в двух разделах, один из которых содержит пару из подразделов.

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  },
  "section2": {
    "subsection0" : {
      "key0": "value",
      "key1": "value"
    },
    "subsection1" : {
      "key0": "value",
      "key1": "value"
    }
  }
}
```

Когда файл считывается в конфигурацию, для сохранения значений конфигурации создаются следующие уникальные иерархические ключи.

* section0:key0
* section0:key1
* section1:key0
* section1:key1
* section2:subsection0:key0
* section2:subsection0:key1
* section2:subsection1:key0
* section2:subsection1:key1

### <a name="getsection"></a>GetSection

[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) извлекает подраздел конфигурации с указанным ключом подраздела.

Чтобы вернуть <xref:Microsoft.Extensions.Configuration.IConfigurationSection>, содержащий только пары "ключ — значение" в `section1`, вызовите `GetSection` и укажите имя раздела.

```csharp
var configSection = _config.GetSection("section1");
```

`configSection` не содержит значение, только ключ и путь.

Аналогично, чтобы получить значения для ключей в `section2:subsection0`, вызовите `GetSection` и укажите путь к разделу.

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

Значение `GetSection` никогда не возвращает значение `null`. Если соответствующий раздел не найден, возвращается пустой параметр `IConfigurationSection`.

Когда `GetSection` возвращает соответствующий раздел, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> не заполняется. <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> и <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> возвращаются, если раздел существует.

### <a name="getchildren"></a>GetChildren

Вызов [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) в `section2` получает значение `IEnumerable<IConfigurationSection>`, которое включает:

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a>Exists

Используйте [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*), чтобы определить, существует ли раздел конфигурации.

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

Учитывая данные примера, `sectionExists` является `false`, потому что в данных конфигурации нет `section2:subsection2`.

## <a name="bind-to-a-class"></a>Привязка к классу

Конфигурация может быть привязана к классам, которые представляют группы связанных параметров, используя шаблон *вариантов*. Для получения дополнительной информации см. <xref:fundamentals/configuration/options>.

Значения конфигурации возвращаются как строки, но вызов <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> позволяет построить объекты [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object). Модуль привязки привязывает значения ко всем открытым свойствам чтения и записи предоставленного типа. Поля **не** привязаны.

Пример приложения содержит модель `Starship` (*Models/Starship.cs*):

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

Раздел `starship` файла *starship.json* создает конфигурацию, когда образец приложения использует поставщик конфигурации JSON для загрузки конфигурации.

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

Создаются следующие пары "ключ — значение" конфигурации.

| Ключ                   | Значение                                             |
| --------------------- | ------------------------------------------------- |
| starship:name         | USS Enterprise                                    |
| starship:registry     | NCC-1701                                          |
| starship:class        | Constitution                                      |
| starship:length       | 304.8                                             |
| starship:commissioned | False                                             |
| trademark             | Paramount Pictures Corp. https://www.paramount.com |

Пример приложения вызывает `GetSection` с помощью ключа `starship`. Пары "ключ — значение" `starship` изолированы. Метод `Bind` вызывается в подразделе при прохождении в экземпляр класса `Starship`. После привязки значения экземпляра экземпляру присваивается свойство для преобразования для просмотра.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a>Привязка к графу объектов

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> способна связывать весь граф объекта POCO. Как и при привязке простого объекта, привязываются только открытые свойства чтения и записи.

Образец содержит модель `TvShow`, в графе объектов которого находятся классы `Metadata` и `Actors` (*Модели/TvShow.cs*).

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

В примере приложения есть файл *tvshow.xml*, содержащий данные конфигурации.

::: moniker range=">= aspnetcore-3.0"

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

Конфигурация привязана ко всему ​​графу объектов `TvShow` с помощью метода `Bind`. Привязанный экземпляр присваивается свойству для подготовки к просмотру.

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) привязывает и возвращает указанный тип. Метод `Get<T>` может быть более удобным, чем использование `Bind`. В следующем коде показано, как использовать `Get<T>` с предыдущим примером, который позволяет привязать связанный экземпляр непосредственно к свойству, используемому для подготовки к просмотру.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a>Привязка массива к классу

*Пример приложения демонстрирует концепции, описанные в этом разделе.*

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации. Любой формат массива, который предоставляет сегмент числового ключа (`:0:`, `:1:`, &hellip; `:{n}:`), способен привязать массив к массиву класса POCO.

> [!NOTE]
> Привязка предоставляется соглашением. Пользовательские поставщики конфигурации не обязаны реализовывать привязку массива.

**Обработка массива в оперативной памяти**

Рассмотрите ключи и значения конфигурации, приведенные в следующей таблице.

| Ключ             | Значение  |
| :-------------: | :----: |
| array:entries:0 | value0 |
| array:entries:1 | value1 |
| array:entries:2 | value2 |
| array:entries:4 | value4 |
| array:entries:5 | value5 |

Эти ключи и значения загружаются в пример приложения с помощью поставщика конфигурации памяти.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

Массив пропускает значения для индекса &num;3. Связующее свойство конфигурации не способно связывать нулевые значения или создавать нулевые записи в связанных объектах, что становится ясным в тот момент, когда демонстрируется результат привязки этого массива к объекту.

В примере приложения класс POCO доступен для хранения привязанных данных конфигурации.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

Данные конфигурации, связанные с объектом.

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

Синтаксис [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) также может использоваться для получения более компактного кода.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

Связанный объект, экземпляр `ArrayExample`, получает данные массива из конфигурации.

| Индекс `ArrayExample.Entries` | Значение `ArrayExample.Entries` |
| :--------------------------: | :--------------------------: |
| 0                            | value0                       |
| 1                            | value1                       |
| 2                            | value2                       |
| 3                            | value4                       |
| 4                            | value5                       |

Индекс &num;3 в связанном объекте содержит данные конфигурации для конфигурационного ключа `array:4` и его значения `value4`. При привязке данных конфигурации, содержащих массив индексов, в ключах конфигурации эти индексы просто используются для выполнения итерации данных конфигурации при создании объекта. Когда массив ключей конфигурации пропускает один или несколько индексов, в данных конфигурации не может быть сохранено нулевое значение и в связанном объекте не создается нулевая запись.

Отсутствующий элемент конфигурации для индекса &num;3 может быть предоставлен перед привязкой к экземпляру `ArrayExample` любым поставщиком конфигурации, который создает правильную пару "ключ — значение" в конфигурации. Если в образец включен дополнительный поставщик конфигурации JSON с отсутствующей парой "ключ — значение", то `ArrayExample.Entries` подойдет полному массиву конфигурации.

*missing_value.json*:

```json
{
  "array:entries:3": "value3"
}
```

В `ConfigureAppConfiguration`:

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

Пары "ключ — значение", показанные в таблице, загружаются в конфигурации.

| Ключ             | Значение  |
| :-------------: | :----: |
| array:entries:3 | value3 |

Если экземпляр класса `ArrayExample` связан после того, как поставщик конфигурации JSON включает запись для индекса &num;3, то массив `ArrayExample.Entries` включит значение.

| Индекс `ArrayExample.Entries` | Значение `ArrayExample.Entries` |
| :--------------------------: | :--------------------------: |
| 0                            | value0                       |
| 1                            | value1                       |
| 2                            | value2                       |
| 3                            | value3                       |
| 4                            | value4                       |
| 5                            | value5                       |

**Обработка массива JSON**

Если JSON-файл содержит массив, ключи конфигурации будут созданы для элементов массива с индексом раздела, начиная с нуля. В следующем файле конфигурации `subsection` — это массив.

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

Поставщик конфигурации JSON считывает данные конфигурации в следующие пары "ключ — значение".

| Ключ                     | Значение  |
| ----------------------- | :----: |
| json_array:key          | valueA |
| json_array:subsection:0 | valueB |
| json_array:subsection:1 | valueC |
| json_array:subsection:2 | valueD |

В примере приложения для привязки пар "ключ — значение" конфигурации доступен следующий класс POCO.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

После выполнения привязки `JsonArrayExample.Key` содержит значение `valueA`. Подраздел значения хранится в свойстве массива POCO `Subsection`.

| Индекс `JsonArrayExample.Subsection` | Значение `JsonArrayExample.Subsection` |
| :---------------------------------: | :---------------------------------: |
| 0                                   | valueB                              |
| 1                                   | valueC                              |
| 2                                   | valueD                              |

## <a name="custom-configuration-provider"></a>Поставщик пользовательской конфигурации

Пример приложения демонстрирует, как создать базовый поставщик конфигурации, который считывает пары "ключ — значение" конфигурации из базы данных, используя [Entity Framework (EF)](/ef/core/).

Поставщик имеет следующие характеристики.

* База данных в памяти EF используется для демонстрационных целей. Чтобы использовать базу данных, для которой требуется строка подключения, выполните вторичный `ConfigurationBuilder`, чтобы предоставить строку подключения от другого поставщика конфигурации.
* Поставщик считывает таблицу базы данных в конфигурации при запуске. Поставщик не запрашивает базу данных для каждого ключа.
* Функция перезагрузки на изменение не реализована, поэтому обновление базы данных после запуска приложения не влияет на конфигурацию приложения.

Определите сущность `EFConfigurationValue` для хранения значений конфигурации в базе данных.

::: moniker range=">= aspnetcore-3.0"

*Models/EFConfigurationValue.cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

Добавьте `EFConfigurationContext` в хранилище и обратитесь к настроенным значениям.

*EFConfigurationProvider/EFConfigurationContext.cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

Создайте класс, реализующий <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.

*EFConfigurationProvider/EFConfigurationSource.cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

Создайте пользовательский поставщик конфигурации путем наследования от <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>. Поставщик конфигурации инициализирует пустую базу данных. Поскольку [конфигурационные ключи не учитывают регистр](#keys), словарь, используемый для инициализации базы данных, создается с помощью функции сравнения без учета регистра ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).

*EFConfigurationProvider/EFConfigurationProvider.cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

Метод расширения `AddEFConfiguration` позволяет добавить источник конфигурации к `ConfigurationBuilder`.

*Extensions/EntityFrameworkExtensions.cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

В следующем коде показано, как использовать пользовательский `EFConfigurationProvider` в *Program.cs*.

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

*Models/EFConfigurationValue.cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

Добавьте `EFConfigurationContext` в хранилище и обратитесь к настроенным значениям.

*EFConfigurationProvider/EFConfigurationContext.cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

Создайте класс, реализующий <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.

*EFConfigurationProvider/EFConfigurationSource.cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

Создайте пользовательский поставщик конфигурации путем наследования от <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>. Поставщик конфигурации инициализирует пустую базу данных.

*EFConfigurationProvider/EFConfigurationProvider.cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

Метод расширения `AddEFConfiguration` позволяет добавить источник конфигурации к `ConfigurationBuilder`.

*Extensions/EntityFrameworkExtensions.cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

В следующем коде показано, как использовать пользовательский `EFConfigurationProvider` в *Program.cs*.

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

## <a name="access-configuration-during-startup"></a>Доступ к конфигурации во время запуска

Внесите значение `IConfiguration` в конструктор `Startup` для доступа к значениям конфигурации в `Startup.ConfigureServices`. Чтобы получить доступ к конфигурации в `Startup.Configure`, либо добавьте значение `IConfiguration` непосредственно в метод, либо используйте экземпляр из конструктора.

```csharp
public class Startup
{
    private readonly IConfiguration _config;

    public Startup(IConfiguration config)
    {
        _config = config;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        var value = _config["key"];
    }

    public void Configure(IApplicationBuilder app, IConfiguration config)
    {
        var value = config["key"];
    }
}
```

Дополнительные сведения см. в руководстве по [доступу к конфигурации с использованием удобных методов](xref:fundamentals/startup#convenience-methods).

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a>Настройте доступ на странице Razor Pages или в представлении MVC

Для доступа к параметрам конфигурации на странице Razor Pages или в представлении MVC добавьте [использование директивы](xref:mvc/views/razor#using) ([Справочник по C#: использование директивы](/dotnet/csharp/language-reference/keywords/using-directive)) для [пространства имен Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) и вставьте <xref:Microsoft.Extensions.Configuration.IConfiguration> на страницу или в представление.

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
    <p>Configuration value for 'key': @Configuration["key"]</p>
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
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

## <a name="add-configuration-from-an-external-assembly"></a>Добавление конфигурации из внешней сборки

Реализация <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> позволяет при запуске добавлять в приложение улучшения из внешней сборки вне приложения класса `Startup`. Для получения дополнительной информации см. <xref:fundamentals/configuration/platform-specific-configuration>.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/configuration/options>
