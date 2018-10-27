---
title: Конфигурация в .NET Core
author: guardrex
description: Узнайте, как использовать API конфигурации для настройки приложения ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/09/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: f60b0611a8fa1880273dd465f23a262c135c37db
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148906"
---
# <a name="configuration-in-aspnet-core"></a>Конфигурация в .NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Конфигурация приложения в ASP.NET Core основана на парах "ключ — значение", установленных *поставщиками конфигурации*. Поставщики конфигурации получают данные конфигурации в парах "ключ — значение" из различных источников:

::: moniker range=">= aspnetcore-2.1"

* Хранилище ключей Azure;
* аргументов командной строки;
* а также пользовательские поставщики (устанавливаемые или создаваемые).
* Справочные файлы
* Переменные среды
* объектов .NET в памяти;
* файлов параметров;

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* Хранилище ключей Azure;
* аргументов командной строки;
* а также пользовательские поставщики (устанавливаемые или создаваемые).
* Переменные среды
* объектов .NET в памяти;
* файлов параметров;

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* аргументов командной строки;
* а также пользовательские поставщики (устанавливаемые или создаваемые).
* Переменные среды
* объектов .NET в памяти;
* файлов параметров;

::: moniker-end

*Шаблон параметров* является расширением конфигурации основных понятий, описанных в этом разделе. Параметры используют классы для представления групп связанных настроек. Дополнительные сведения об использовании шаблона параметров см. в разделе <xref:fundamentals/configuration/options>.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

Примеры, приведенные в этом разделе, основаны на следующем.

* Указание базового пути приложения с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>. `SetBasePath` доступно приложению при использовании ссылки на пакет [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/).
* Разрешение разделов файлов конфигурации с помощью <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>. `GetSection` доступно приложению при использовании ссылки на пакет [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/).
* Связывание конфигурации с .NET-классами с <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> и [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*). `Bind` и `Get<T>` доступны для приложения при использовании ссылки на пакет [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/). `Get<T>` доступно в ASP.NET Core 1.1 и более поздних версий.

::: moniker range=">= aspnetcore-2.1"

Эти три пакета включены в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Эти три пакета включены в [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage).

::: moniker-end

## <a name="host-vs-app-configuration"></a>Конфигурация узла и приложения

Перед настройкой и запуском приложения настройте и запустите *узел*. Узел отвечает за запуск приложения и управление временем существования. Как приложение, так и узел настраиваются с использованием поставщиков конфигурации, описанных в этом разделе. Пары "ключ — значение" конфигурации узлов становятся частью глобальной конфигурации приложения. Дополнительные сведения о том, как используются поставщики конфигурации при создании узла и как влияют источники конфигурации на узел, см. в разделе <xref:fundamentals/host/index>.

## <a name="security"></a>Безопасность

Придерживайтесь следующих рекомендаций.

* Никогда не храните пароли или другие конфиденциальные данные в коде поставщика конфигурации или в файлах конфигурации обычного текста.
* Не используйте секреты рабочей среды в средах разработки и тестирования.
* Указывайте секреты вне проекта, чтобы их нельзя было случайно зафиксировать в репозитории с исходным кодом.

Дополнительные сведения см. в статье [Использование нескольких сред в ASP.NET Core](xref:fundamentals/environments) и руководствуйтесь статьей [Безопасное хранение секретов приложения во время разработки в ASP.NET Core](xref:security/app-secrets) (включает рекомендации по использованию переменной среды для хранения конфиденциальных данных). Менеджер секретов использует поставщик конфигурации файла для хранения конфиденциальных данных пользователя в файле JSON в локальной системе. Поставщик конфигурации файлов описан ниже в этом разделе.

[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) — один из вариантов для безопасного хранения секретов приложения. Дополнительные сведения см. в разделе <xref:security/key-vault-configuration>.

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

При запуске приложения источники конфигурации считываются в порядке, в котором были указаны их поставщики конфигурации.

Поставщики файлов конфигурации имеют возможность перезагрузить конфигурацию при изменении базового файла параметров после запуска приложения. Поставщик конфигурации файлов описан ниже в этом разделе.

<xref:Microsoft.Extensions.Configuration.IConfiguration> доступен в контейнере [Dependency Injection (DI)](xref:fundamentals/dependency-injection) приложения. Поставщики конфигурации не могут использовать контейнер DI, так как он недоступен при настройке узла.

В ключах конфигурации приняты следующие соглашения.

* В ключах не учитывается регистр символов. Например `ConnectionString` и `connectionstring` обрабатываются как эквивалентные ключи.
* Если значение для одного и того же ключа установлено одним и тем же или разными поставщиками конфигурации, последним значением, установленным на ключе, является используемое значение.
* Иерархические ключи
  * При взаимодействии с API конфигурации разделитель-двоеточие (`:`) поддерживается на всех платформах.
  * В переменных среды разделитель-двоеточие может не работать на всех платформах. Двойной знак подчеркивания (`__`) поддерживается на всех платформах.
  * В хранилище ключей Azure иерархические ключи используют `--` (два дефиса) в качестве разделителя. Чтобы заменить дефисы двоеточием, при загрузке секретов в конфигурацию приложения необходимо указать код.
* <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации. Привязка массива описана в разделе [Привязка массива к классу](#bind-an-array-to-a-class).

В значениях конфигурации учитываются следующие соглашения.

* Значения являются строками.
* Значение NULL не может храниться в конфигурации или быть привязанным к объектам.

## <a name="providers"></a>Поставщики

В следующей таблице показаны поставщики конфигурации, доступные для приложений ASP.NET Core.

::: moniker range=">= aspnetcore-2.1"

| Поставщик | Предоставляет конфигурацию из &hellip; |
| -------- | ----------------------------------- |
| [Поставщик конфигурации хранилища ключей Azure](xref:security/key-vault-configuration) (раздел *Безопасность*) | Хранилище ключей Azure; |
| [Поставщик конфигурации командной строки](#command-line-configuration-provider) | Параметры командной строки |
| [Поставщик пользовательской конфигурации](#custom-configuration-provider) | Источник пользователя |
| [Поставщик конфигурации переменных среды](#environment-variables-configuration-provider) | Переменные среды |
| [Поставщик пользовательской конфигурации](#file-configuration-provider) | Файлы (INI, JSON, XML) |
| [Поставщик конфигурации ключа для каждого файла](#key-per-file-configuration-provider) | Справочные файлы |
| [Поставщик конфигурации памяти](#memory-configuration-provider) | Коллекции оперативной памяти |
| [Секреты пользователей (Менеджер секретов)](xref:security/app-secrets) (раздел *Безопасность*) | Файл в каталоге профиля пользователя |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| Поставщик | Предоставляет конфигурацию из &hellip; |
| -------- | ----------------------------------- |
| [Поставщик конфигурации хранилища ключей Azure](xref:security/key-vault-configuration) (раздел *Безопасность*) | Хранилище ключей Azure; |
| [Поставщик конфигурации командной строки](#command-line-configuration-provider) | Параметры командной строки |
| [Поставщик пользовательской конфигурации](#custom-configuration-provider) | Источник пользователя |
| [Поставщик конфигурации переменных среды](#environment-variables-configuration-provider) | Переменные среды |
| [Поставщик пользовательской конфигурации](#file-configuration-provider) | Файлы (INI, JSON, XML) |
| [Поставщик конфигурации памяти](#memory-configuration-provider) | Коллекции оперативной памяти |
| [Секреты пользователей (Менеджер секретов)](xref:security/app-secrets) (раздел *Безопасность*) | Файл в каталоге профиля пользователя |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| Поставщик | Предоставляет конфигурацию из &hellip; |
| -------- | ----------------------------------- |
| [Поставщик конфигурации командной строки](#command-line-configuration-provider) | Параметры командной строки |
| [Поставщик пользовательской конфигурации](#custom-configuration-provider) | Источник пользователя |
| [Поставщик конфигурации переменных среды](#environment-variables-configuration-provider) | Переменные среды |
| [Поставщик пользовательской конфигурации](#file-configuration-provider) | Файлы (INI, JSON, XML) |
| [Поставщик конфигурации памяти](#memory-configuration-provider) | Коллекции оперативной памяти |
| [Секреты пользователей (Менеджер секретов)](xref:security/app-secrets) (раздел *Безопасность*) | Файл в каталоге профиля пользователя |

::: moniker-end

Источники конфигурации считываются в том порядке, в котором при запуске указываются их поставщики конфигурации. Поставщики конфигурации в этом разделе описаны в алфавитном порядке, а не в необходимом вам порядке в коде. Порядок поставщиков конфигурации в коде соответствует приоритетам ваших основных источников конфигурации.

Типичная последовательность поставщиков конфигурации.

1. Файлы (*appsettings.json*, *appsettings.{Environment}.json*, где `{Environment}` — это текущая среда размещения приложения)
1. [Azure Key Vault](xref:security/key-vault-configuration);
1. [Секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки)
1. Переменные среды
1. аргументов командной строки;

Общепринятой практикой является размещение поставщика конфигурации командной строки последним в ряду поставщиков, чтобы аргументы командной строки могли переопределять конфигурацию, установленную другими поставщиками.

::: moniker range=">= aspnetcore-2.0"

Эта последовательность поставщиков помещается в месте, где происходит инициализация нового класса <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>. Дополнительные сведения см. в статье [Веб-узел ASP.NET Core. Создание узла](xref:fundamentals/host/web-host#set-up-a-host).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Эта последовательность поставщиков может быть создана для приложения (а не узла) с помощью <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> и вызова метода <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> в `Startup`.

```csharp
public Startup(IHostingEnvironment env)
{
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
        .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
            reloadOnChange: true);

    var appAssembly = Assembly.Load(new AssemblyName(env.ApplicationName));

    if (appAssembly != null)
    {
        builder.AddUserSecrets(appAssembly, optional: true);
    }

    builder.AddEnvironmentVariables();

    Configuration = builder.Build();
}

public IConfiguration Configuration { get; }

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IConfiguration>(Configuration);
}
```

В приведенном выше примере имя среды (`env.EnvironmentName`) и имя сборки приложения (`env.ApplicationName`) предоставляются <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>. Дополнительные сведения см. в разделе <xref:fundamentals/environments>.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> при сборке веб-узла, чтобы указать поставщиков конфигурации приложения в дополнение к тем, которые автоматически добавлены <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

::: moniker-end

## <a name="command-line-configuration-provider"></a>Поставщик конфигурации командной строки

<xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> загружает конфигурацию из пары "ключ — значение" аргумента командной строки в среде выполнения.

Чтобы активировать конфигурацию командной строки, вызывается метод расширения <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> для экземпляра <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

::: moniker range=">= aspnetcore-2.0"

`AddCommandLine` автоматически вызывается при инициализации нового <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>. Дополнительные сведения см. в статье [Веб-узел ASP.NET Core. Создание узла](xref:fundamentals/host/web-host#set-up-a-host).

`CreateDefaultBuilder` также загружает следующее:

* дополнительную конфигурацию из *appsettings.json* и *appsettings.{Environment}.json*;
* [секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки);
* Переменные среды.

`CreateDefaultBuilder` добавляет последним поставщика конфигурации командной строки. Аргументы командной строки передаются в набор конфигурации переопределения среды выполнения, установленной другими поставщиками.

`CreateDefaultBuilder` действует, когда создается узел. Поэтому конфигурация командной строки, активированная с помощью `CreateDefaultBuilder`, может повлиять на настройку узла.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Чтобы указать конфигурацию приложения, при сборке узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.

Объект `AddCommandLine` уже был вызван методом `CreateDefaultBuilder`. Если нужно предоставить конфигурацию приложения, сохранив возможность переопределить ее через аргументы командной строки, вызовите в <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>дополнительные поставщики приложения, в завершение вызвав `AddCommandLine`.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call other providers here and call AddCommandLine last.
                config.AddCommandLine(args);
            })
            .UseStartup<Startup>();
}
```

При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>.

Объект `AddCommandLine` уже был вызван методом `CreateDefaultBuilder` при вызове `UseConfiguration`. Если нужно предоставить конфигурацию приложения, сохранив возможность переопределить ее через аргументы командной строки, вызовите в `ConfigurationBuilder`дополнительные поставщики приложения, в завершение вызвав `AddCommandLine`.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            // Call other providers here and call AddCommandLine last.
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Чтобы активировать конфигурацию командной строки, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

В этом случае аргументы командной строки, передаваемые во время выполнения, переопределяют конфигурацию, заданную другими вызванными ранее поставщиками конфигурации.

Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    // Call additional providers here as needed.
    // Call AddCommandLine last to allow arguments to override other configuration.
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

**Пример**

::: moniker range=">= aspnetcore-2.0"

Пример приложения 2.x использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Пример приложения 1.x вызывает образец <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> в <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

::: moniker-end

1. Откройте командную строку в каталоге проекта.
1. Поставьте аргумент командной строки в команду `dotnet run`: `dotnet run CommandLineKey=CommandLineValue`.
1. После запуска приложения откройте браузер в приложении по адресу `http://localhost:5000`.
1. Обратите внимание, что вывод содержит пару "ключ — значение" для аргумента командной строки конфигурации, предоставленного для `dotnet run`.

### <a name="arguments"></a>Аргументы

Значение должно соответствовать знаку равенства (`=`), или ключ должен иметь префикс (`--` или `/`), когда значение следует за пробелом. Значение может соответствовать NULL, если используется знак равенства (например, `CommandLineKey=`).

| Префикс ключа               | Пример                                                |
| ------------------------ | ------------------------------------------------------ |
| Без префикса                | `CommandLineKey1=value1`                               |
| Два дефиса (`--`)        | `--CommandLineKey2=value2`, `--CommandLineKey2 value2` |
| Прямая косая черта (`/`)      | `/CommandLineKey3=value3`, `/CommandLineKey3 value3`   |

В рамках одной и той же команды не смешивайте пары "ключ — значение" аргумента командной строки, которые используют знак равенства, с парами "ключ — значение", которые используют пробел.

Примеры команд.

```console
dotnet run CommandLineKey1=value --CommandLineKey2=value /CommandLineKey2=value
dotnet run --CommandLineKey1 value /CommandLineKey2 value
dotnet run CommandLineKey1= CommandLineKey2=value
```

### <a name="switch-mappings"></a>Сопоставления переключений

Сопоставление параметров позволяет указать логику замены имен ключей. Когда вручную создается конфигурация с помощью <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, методу <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> можно предоставить словарь сопоставления переключений.

В словаре сопоставлений переключений выполняется поиск ключа, который совпадает с ключом, предоставляемым аргументом командной строки. Если ключ в командной строке находится в словаре, значение словаря (замена ключа) передается обратно, чтобы установить пару "ключ — значение" в конфигурацию приложения. Сопоставление переключений необходимо для любого ключа командной строки с префиксом из одного дефиса (`-`).

Правила ключей из словаря сопоставления переключений:

* Переключения должны начинаться с дефиса (`-`) или двойного дефиса (`--`).
* Словарь сопоставлений переключений не должен содержать повторяющиеся ключи.

::: moniker range=">= aspnetcore-2.1"

Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _switchMappings = 
        new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    // Do not pass the args to CreateDefaultBuilder
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder()
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddCommandLine(args, _switchMappings);
            })
            .UseStartup<Startup>();
}
```

Как показано в предыдущем примере, вызов `CreateDefaultBuilder` не должен передавать аргументы при использовании сопоставления переключений. Вызов команды `AddCommandLine` метода `CreateDefaultBuilder` не включает сопоставленные коммутаторы, и нет возможности передать словарь сопоставления переключений в `CreateDefaultBuilder`. Если аргументы включают сопоставления переключений и передаются в `CreateDefaultBuilder`, его поставщик `AddCommandLine` не может инициализироваться с помощью <xref:System.FormatException>. Чтобы решить эту проблему, нужно не передавать аргументы команде `CreateDefaultBuilder`, а позволить методу `AddCommandLine` метода `ConfigurationBuilder` обрабатывать как аргументы, так и словарь сопоставления параметров.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var switchMappings = new Dictionary<string, string>
            {
                { "-CLKey1", "CommandLineKey1" },
                { "-CLKey2", "CommandLineKey2" }
            };

        var config = new ConfigurationBuilder()
            .AddCommandLine(args, switchMappings)
            .Build();

        // Do not pass the args to CreateDefaultBuilder
        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

Как показано в предыдущем примере, вызов `CreateDefaultBuilder` не должен передавать аргументы при использовании сопоставления переключений. Вызов команды `AddCommandLine` метода `CreateDefaultBuilder` не включает сопоставленные коммутаторы, и нет возможности передать словарь сопоставления переключений в `CreateDefaultBuilder`. Если аргументы включают сопоставления переключений и передаются в `CreateDefaultBuilder`, его поставщик `AddCommandLine` не может инициализироваться с помощью <xref:System.FormatException>. Чтобы решить эту проблему, нужно не передавать аргументы команде `CreateDefaultBuilder`, а позволить методу `AddCommandLine` метода `ConfigurationBuilder` обрабатывать как аргументы, так и словарь сопоставления параметров.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public static void Main(string[] args)
{
    var switchMappings = new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    var config = new ConfigurationBuilder()
        .AddCommandLine(args, switchMappings)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .UseStartup<Startup>()
        .Start();

    using (host)
    {
        Console.ReadLine();
    }
}
```

::: moniker-end

Созданный словарь сопоставлений переключений содержит данные, показанные в следующей таблице.

| Ключ       | Значение             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

Если ключи сопоставления переключений используются при запуске приложения, конфигурация принимает значение конфигурации в ключе, предоставленном в словаре.

```console
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

В переменных средах разделитель-двоеточие (`:`) может не работать на всех платформах. Двойной знак подчеркивания (`__`) поддерживается на всех платформах и его можно заменить двоеточием.

[Служба приложений Azure](https://azure.microsoft.com/services/app-service/) позволяет задать переменные среды на портале Azure, который может переопределить конфигурацию приложения, используя поставщик конфигурации переменных среды. Дополнительные сведения см. в разделе [Приложения Azure. Переопределение конфигурации приложения с помощью портала Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).

::: moniker range=">= aspnetcore-2.0"

`AddEnvironmentVariables` автоматически вызывается при инициализации нового <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>. Дополнительные сведения см. в статье [Веб-узел ASP.NET Core. Создание узла](xref:fundamentals/host/web-host#set-up-a-host).

`CreateDefaultBuilder` также загружает следующее:

* дополнительную конфигурацию из *appsettings.json* и *appsettings.{Environment}.json*;
* [секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки);
* аргументы командной строки.

Поставщик конфигурации переменных среды вызывается после того, как настройка была создана из секретов пользователя и файлов *appsettings*. Вызов поставщика в этой позиции разрешает чтение переменных среды выполнения, чтобы переопределить конфигурацию, заданную секретом пользователя и файлом *appsettings*.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Чтобы указать конфигурацию приложения, при сборке узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.

Метод `AddEnvironmentVariables` для переменных среды с префиксом `ASPNETCORE_` уже был вызван из `CreateDefaultBuilder`. Если вам нужно добавить конфигурацию приложения из дополнительных переменных среды, вызовите дополнительные поставщики приложения в <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, а затем вызовите `AddEnvironmentVariables` с префиксом.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call additional providers here as needed.
                // Call AddEnvironmentVariables last if you need to allow environment
                // variables to override values from other providers.
                config.AddEnvironmentVariables(prefix: "PREFIX_");
            })
            .UseStartup<Startup>();
}
```

При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Вызовите метод расширения `AddEnvironmentVariables` для экземпляра <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>. Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>.

Метод `AddEnvironmentVariables` для переменных среды с префиксом `ASPNETCORE_` уже был вызван из `CreateDefaultBuilder`. Если вам нужно добавить конфигурацию приложения из дополнительных переменных среды, вызовите дополнительные поставщики приложения в <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, а затем вызовите `AddEnvironmentVariables` с префиксом.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            // Call additional providers here as needed.
            // Call AddEnvironmentVariables last if you need to allow environment
            // variables to override values from other providers.
            .AddEnvironmentVariables(prefix: "PREFIX_")
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

**Пример**

::: moniker range=">= aspnetcore-2.0"

Пример приложения 2.x использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов `AddEnvironmentVariables`.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Пример приложения 1.x вызывает образец `AddEnvironmentVariables` в `ConfigurationBuilder`.

::: moniker-end

1. Выполните пример приложения. Откройте в приложении браузер с адресом `http://localhost:5000`.
1. Обратите внимание, что выходные данные содержат пару "ключ — значение" для переменной среды `ENVIRONMENT`. Значение отражает среду, в которой выполняется приложение, обычно при локальном запуске это `Development`.

Чтобы список переменных сред, отображаемый приложением, был коротким, приложение фильтрует переменные среды по следующим пунктам:

* ASPNETCORE_
* urls
* Ведение журнала
* ENVIRONMENT
* contentRoot
* AllowedHosts
* applicationName
* CommandLine

::: moniker range=">= aspnetcore-2.0"

Если хотите просмотреть все переменные среды, доступные для приложения, измените значение `FilteredConfiguration` в *Pages/Index.cshtml.cs* на следующее:

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Если хотите просмотреть все переменные среды, доступные для приложения, измените значение `FilteredConfiguration` в *Controllers/HomeController.cs* на следующее:

::: moniker-end

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

::: moniker range=">= aspnetcore-2.0"

Статический удобный метод `CreateDefaultBuilder` создает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> для установления размещения приложения. Когда значение `WebHostBuilder` будет создано, оно найдет конфигурацию узла в переменных среды, которые начинаются с префикса `ASPNETCORE_`.

::: moniker-end

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

::: moniker range=">= aspnetcore-2.1"

Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddIniFile("config.ini", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Вызывая `CreateDefaultBuilder`, вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddIniFile("config.ini", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddIniFile("config.ini", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
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

::: moniker range=">= aspnetcore-2.0"

`AddJsonFile` автоматически вызывается дважды при инициализации нового <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>. Метод вызывается для загрузки конфигурации из:

* *appsettings.json* &ndash; первым читается этот файл. Версия файла среды может переопределить значения, предоставленные *appsettings.json*.
* *appsettings.{Environment}.json* — версия среды файла загружается на основе [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).

Дополнительные сведения см. в статье [Веб-узел ASP.NET Core. Создание узла](xref:fundamentals/host/web-host#set-up-a-host).

`CreateDefaultBuilder` также загружает следующее:

* Переменные среды.
* [секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки);
* аргументы командной строки.

Сначала устанавливается поставщик конфигурации JSON-файлов. Таким образом, секреты пользователя, переменные среды и аргументы командной строки переопределят конфигурацию, заданную файлами *appsettings*.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> при сборке узла, чтобы указать конфигурацию приложения для файлов, отличных от *appsettings.json* и *appsettings.{Environment}.json*.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddJsonFile("config.json", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Вы можете также напрямую вызвать метод расширения `AddJsonFile` в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Вызывая `CreateDefaultBuilder`, вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("config.json", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("config.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

**Пример**

::: moniker range=">= aspnetcore-2.0"

В примере приложения 2.x используется преимущество статически удобного метода `CreateDefaultBuilder` для создания узла, который включает два вызова в `AddJsonFile`. Конфигурация загружена из *appsettings.json* и *appsettings.{Environment}.json*.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Пример приложения 1.x вызывает образец `AddJsonFile` дважды в `ConfigurationBuilder`. Конфигурация загружена из *appsettings.json* и *appsettings.{Environment}.json*.

::: moniker-end

1. Выполните пример приложения. Откройте в приложении браузер с адресом `http://localhost:5000`.
1. Обратите внимание, что выходные данные содержат пары "ключ — значение" для конфигурации, представленной в таблице в зависимости от среды. Ключи конфигурации ведения журнала используют двоеточие (`:`) как иерархический разделитель.

| Ключ                        | Значение разработки | Рабочее значение |
| -------------------------- | :---------------: | :--------------: |
| Logging:LogLevel:System    | Сведения       | Сведения      |
| Logging:LogLevel:Microsoft | Сведения       | Сведения      |
| Logging:LogLevel:Default   | Отладка             | Error            |
| AllowedHosts               | *                 | *                |

### <a name="xml-configuration-provider"></a>Поставщик конфигурации XML

<xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> загружает конфигурацию из пары "ключ — значение" XML-файла в среде выполнения.

Чтобы активировать конфигурацию XML-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Перегрузки позволяют указать следующее.

* Файл является обязательным или нет.
* Будет ли перезагружена конфигурация, если файл изменится.
* <xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.

Корневой узел файла конфигурации не учитывается при создании пар "ключ — значение" конфигурации. Не указывайте в файле Document Type Definition (определение типа документа, DTD) или пространство имен.

::: moniker range=">= aspnetcore-2.1"

Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddXmlFile("config.xml", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Вызывая `CreateDefaultBuilder`, вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
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

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a>Поставщик конфигурации ключа для каждого файла

<xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> использует файлы каталога как пары "ключ — значение" конфигурации. Ключ является именем файла. Значение содержит содержимое файла. Поставщик конфигурации ключа для каждого файла используется в сценариях размещения Docker.

Чтобы активировать конфигурацию ключа для каждого файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>. Значение параметра `directoryPath` должно быть абсолютным путем к файлам.

Перегрузки позволяют указать следующее.

* `Action<KeyPerFileConfigurationSource>` — делегат, который настраивает источник.
* Обязательно ли указывать каталог и путь к каталогу.

Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddKeyPerFile(directoryPath: path, optional: true);
            })
            .UseStartup<Startup>();
}
```

При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.

```csharp
var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
var config = new ConfigurationBuilder()
    .AddKeyPerFile(directoryPath: path, optional: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

::: moniker-end

## <a name="memory-configuration-provider"></a>Поставщик конфигурации памяти

<xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> использует коллекцию памяти в качестве пар "ключ — значение" конфигурации.

Чтобы активировать конфигурацию коллекции в памяти, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Поставщик конфигурации может инициализироваться значением `IEnumerable<KeyValuePair<String,String>>`.

::: moniker range=">= aspnetcore-2.1"

Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _dict = 
        new Dictionary<string, string>
        {
            {"MemoryCollectionKey1", "value1"},
            {"MemoryCollectionKey2", "value2"}
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddInMemoryCollection(_dict);
            })
            .UseStartup<Startup>();
}
```

При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Вызывая `CreateDefaultBuilder`, вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

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
}
```

При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:

::: moniker-end

```csharp
var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

var config = new ConfigurationBuilder()
    .AddInMemoryCollection(dict)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

## <a name="getvalue"></a>GetValue

[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) извлекает значение из конфигурации с указанным ключом и преобразует его в указанный тип. Если ключ не найден, перегрузка позволяет предоставлять значение по умолчанию.

В следующем примере извлекается строковое значение из конфигурации с ключом `NumberKey`, вводится значение `int` и сохраняется значение в переменной `intValue`. Если `NumberKey` не обнаруживается в ключах конфигурации, `intValue` получает значение `99` по умолчанию.

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
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

Аналогично, чтобы получить значения для ключей в `section2:subsection0`, вызовите `GetSection` и укажите путь к разделу.

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

Значение `GetSection` никогда не возвращает значение `null`. Если соответствующий раздел не найден, возвращается пустой параметр `IConfigurationSection`.

### <a name="getchildren"></a>GetChildren

Вызов [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) в `section2` получает значение `IEnumerable<IConfigurationSection>`, которое включает:

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a>Exists

Используйте [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*), чтобы определить, существует ли раздел конфигурации.

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

Учитывая данные примера, `sectionExists` является `false`, потому что в данных конфигурации нет `section2:subsection2`.

::: moniker-end

## <a name="bind-to-a-class"></a>Привязка к классу

Конфигурация может быть привязана к классам, которые представляют группы связанных параметров, используя шаблон *вариантов*. Дополнительные сведения см. в разделе <xref:fundamentals/configuration/options>.

Значения конфигурации возвращаются как строки, но вызов <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> позволяет построить объекты [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).

Пример приложения содержит модель `Starship` (*Models/Starship.cs*):

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

Раздел `starship` файла *starship.json* создает конфигурацию, когда образец приложения использует поставщик конфигурации JSON для загрузки конфигурации.

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

Создаются следующие пары "ключ — значение" конфигурации.

| Ключ                   | Значение                                             |
| --------------------- | ------------------------------------------------- |
| starship:name         | USS Enterprise                                    |
| starship:registry     | NCC-1701                                          |
| starship:class        | Constitution                                      |
| starship:length       | 304.8                                             |
| starship:commissioned | False                                             |
| trademark             | Paramount Pictures Corp. http://www.paramount.com |

Пример приложения вызывает `GetSection` с помощью ключа `starship`. Пары "ключ — значение" `starship` изолированы. Метод `Bind` вызывается в подразделе при прохождении в экземпляр класса `Starship`. После привязки значения экземпляра экземпляру присваивается свойство для преобразования для просмотра.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a>Привязка к графу объектов

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> способна связывать весь граф объекта POCO.

Образец содержит модель `TvShow`, в графе объектов которого находятся классы `Metadata` и `Actors` (*Модели/TvShow.cs*).

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

В примере приложения есть файл *tvshow.xml*, содержащий данные конфигурации.

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

Конфигурация привязана ко всему ​​графу объектов `TvShow` с помощью метода `Bind`. Привязанный экземпляр присваивается свойству для подготовки к просмотру.

::: moniker range=">= aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
viewModel.TvShow = tvShow;
```

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) привязывает и возвращает указанный тип. Метод `Get<T>` может быть более удобным, чем использование `Bind`. В следующем коде показано, как использовать `Get<T>` с предыдущим примером, который позволяет привязать связанный экземпляр непосредственно к свойству, используемому для подготовки к просмотру.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a>Привязка массива к классу

*Пример приложения демонстрирует концепции, описанные в этом разделе.*

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации. Любой формат массива, который предоставляет сегмент числового ключа (`:0:`, `:1:`, &hellip; `:{n}:`), способен привязать массив к массиву класса POCO.

> [!NOTE]
> Привязка предоставляется соглашением. Пользовательские поставщики конфигурации не обязаны реализовывать привязку массива.

**Обработка массива в оперативной памяти**

Рассмотрите ключи и значения конфигурации, приведенные в следующей таблице.

| Ключ     | Значение  |
| :-----: | :----: |
| array:0 | value0 |
| array:1 | value1 |
| array:2 | value2 |
| array:4 | value4 |
| array:5 | value5 |

Эти ключи и значения загружаются в пример приложения с помощью поставщика конфигурации памяти.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

Массив пропускает значения для индекса &num;3. Связующее свойство конфигурации не способно связывать нулевые значения или создавать нулевые записи в связанных объектах, что становится ясным в тот момент, когда демонстрируется результат привязки этого массива к объекту.

В примере приложения класс POCO доступен для хранения привязанных данных конфигурации.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

Данные конфигурации, связанные с объектом.

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

Синтаксис [ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) также может использоваться для получения более компактного кода.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

Связанный объект, экземпляр `ArrayExample`, получает данные массива из конфигурации.

| Индекс `ArrayExamples.Entries` | Значение `ArrayExamples.Entries` |
| :---------------------------: | :---------------------------: |
| 0                             | value0                        |
| 1                             | value1                        |
| 2                             | value2                        |
| 3                             | value4                        |
| 4                             | value5                        |

Индекс &num;3 в связанном объекте содержит данные конфигурации для конфигурационного ключа `array:4` и его значения `value4`. При привязке данных конфигурации, содержащих массив индексов, в ключах конфигурации эти индексы просто используются для выполнения итерации данных конфигурации при создании объекта. Когда массив ключей конфигурации пропускает один или несколько индексов, в данных конфигурации не может быть сохранено нулевое значение и в связанном объекте не создается нулевая запись.

Отсутствующий элемент конфигурации для индекса &num;3 может быть предоставлен перед привязкой к экземпляру `ArrayExamples` любым поставщиком конфигурации, который создает правильную пару "ключ — значение" в конфигурации. Если в образец включен дополнительный поставщик конфигурации JSON с отсутствующей парой "ключ — значение", то `ArrayExamples.Entries` подойдет полному массиву конфигурации.

*missing_value.json*:

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

В <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

В конструкторе `Startup`:

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

Пары "ключ — значение", показанные в таблице, загружаются в конфигурации.

| Ключ             | Значение  |
| :-------------: | :----: |
| array:entries:3 | value3 |

Если экземпляр класса `ArrayExamples` связан после того, как поставщик конфигурации JSON включает запись для индекса &num;3, то массив `ArrayExamples.Entries` включит значение.

| Индекс `ArrayExamples.Entries` | Значение `ArrayExamples.Entries` |
| :---------------------------: | :---------------------------: |
| 0                             | value0                        |
| 1                             | value1                        |
| 2                             | value2                        |
| 3                             | value3                        |
| 4                             | value4                        |
| 5                             | value5                        |

**Обработка массива JSON**

Если JSON-файл содержит массив, ключи конфигурации будут созданы для элементов массива с индексом раздела, начиная с нуля. В следующем файле конфигурации `subsection` — это массив.

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

Поставщик конфигурации JSON считывает данные конфигурации в следующие пары "ключ — значение".

| Ключ                     | Значение  |
| ----------------------- | :----: |
| json_array:key          | valueA |
| json_array:subsection:0 | valueB |
| json_array:subsection:1 | valueC |
| json_array:subsection:2 | valueD |

В примере приложения для привязки пар "ключ — значение" конфигурации доступен следующий класс POCO.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

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

*Models/EFConfigurationValue.cs*:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

Добавьте `EFConfigurationContext` в хранилище и обратитесь к настроенным значениям.

*EFConfigurationProvider/EFConfigurationContext.cs*:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

Создайте класс, реализующий <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.

*EFConfigurationProvider/EFConfigurationSource.cs*:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

Создайте пользовательский поставщик конфигурации путем наследования от <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>. Поставщик конфигурации инициализирует пустую базу данных.

*EFConfigurationProvider/EFConfigurationProvider.cs*:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

Метод расширения `AddEFConfiguration` позволяет добавить источник конфигурации к `ConfigurationBuilder`.

*Extensions/EntityFrameworkExtensions.cs*:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

В следующем коде показано, как использовать пользовательский `EFConfigurationProvider` в *Program.cs*.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

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

Пример доступа к конфигурации с использованием удобных методов запуска см. в разделе [Запуск приложения. Удобные методы](xref:fundamentals/startup#convenience-methods).

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

Реализация <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> позволяет при запуске добавлять в приложение улучшения из внешней сборки вне приложения класса `Startup`. Дополнительные сведения см. в разделе <xref:fundamentals/configuration/platform-specific-configuration>.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/configuration/options>
* [Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/) (Подробные сведения о конфигурации Microsoft)
