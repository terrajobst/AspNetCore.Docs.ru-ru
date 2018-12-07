---
title: Универсальный узел .NET
author: guardrex
description: Сведения об универсальном узле в .NET, который отвечает за запуск приложений и управление временем существования.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: 4d435984d8169b558ab026ef8541c90f7a2a96b9
ms.sourcegitcommit: 0fc89b80bb1952852ecbcf3c5c156459b02a6ceb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/29/2018
ms.locfileid: "52618159"
---
# <a name="net-generic-host"></a>Универсальный узел .NET

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Приложения .NET Core настраивают и запускают *узел*. Узел отвечает за запуск приложения и управление временем существования. В этом разделе рассматривается универсальный узел ASP.NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>), используемый для размещения приложений, которые не обрабатывают запросы HTTP. Сведения о веб-узле (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>) см. в статье <xref:fundamentals/host/web-host>.

Универсальный узел предназначен для отделения конвейера HTTP от API веб-узла, чтобы можно было иметь больше сценариев узла. Обмен сообщениями, фоновые задачи и другие рабочие нагрузки типа "не HTTP", использующие перекрестные возможности универсальных узлов, такие как конфигурация, внедрение зависимости (DI) и ведения журналов.

Универсальный узел является новой возможностью ASP.NET Core 2.1 и не подходит для сценариев с веб-размещением. Сведения о сценариях с веб-размещением см. в статье о [веб-узле](xref:fundamentals/host/web-host). Универсальный узел разрабатывается с целью заменить веб-узел в будущих версиях. Он будет выступать основным узлом API для сценариев HTTP и "не HTTP".

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([как скачивать](xref:index#how-to-download-a-sample))

При выполнении примера приложения в [Visual Studio Code](https://code.visualstudio.com/) используйте *внешний или встроенный терминал*. Не выполняйте пример в `internalConsole`.

Настройка консоли в Visual Studio Code:

1. Откройте файл *.vscode/launch.json*.
1. В разделе конфигурации **.NET Core Launch (console)** найдите параметр **console**. Измените значение на `externalTerminal` или `integratedTerminal`.

## <a name="introduction"></a>Вступление

Библиотека универсального узла находится в пространстве имен <xref:Microsoft.Extensions.Hosting> и включена в пакет [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/). Пакет `Microsoft.Extensions.Hosting` входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 или более поздней версии).

<xref:Microsoft.Extensions.Hosting.IHostedService> — это точка входа для выполнения кода. Каждая реализация `IHostedService` выполняется в порядке [регистрации служб в ConfigureServices](#configureservices). Метод <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> вызывается в каждом `IHostedService` при запуске узла, а метод <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> вызывается в порядке, обратном регистрации, когда узел корректно завершает работу.

## <a name="set-up-a-host"></a>Создание узла

<xref:Microsoft.Extensions.Hosting.IHostBuilder> является основным компонентом, который библиотеки и приложения используют для инициализации, построения и запуска узла:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a>Параметры

<xref:Microsoft.Extensions.Hosting.HostOptions> настраивает параметры для <xref:Microsoft.Extensions.Hosting.IHost>.

### <a name="shutdown-timeout"></a>Время ожидания завершения работы

<xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> задает время ожидания для <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>. Значение по умолчанию — пять секунд.

Следующий пример конфигурации в `Program.Main` увеличивает значение времени ожидания завершения работы по умолчанию с пяти до 20 секунд:

```csharp
var host = new HostBuilder()
    .ConfigureServices((hostContext, services) =>
    {
        services.Configure<HostOptions>(option =>
        {
            option.ShutdownTimeout = System.TimeSpan.FromSeconds(20);
        });
    })
    .Build();
```

## <a name="default-services"></a>Службы по умолчанию

Во время инициализации узла регистрируются следующие службы:

* [Окружение](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* [Конфигурация](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)
* <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)
* <xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)
* <xref:Microsoft.Extensions.Hosting.IHost>
* [Параметры](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)
* [Ведение журнала](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)

## <a name="host-configuration"></a>Конфигурация узла

Существуют следующие подходы для создания конфигурации узла:

* вызов методов расширения в <xref:Microsoft.Extensions.Hosting.IHostBuilder> для задания [корневой папки содержимого](#content-root) и [среды](#environment);
* чтение конфигурации из поставщиков конфигурации в <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.

### <a name="extension-methods"></a>Методы расширения

### <a name="application-key-name"></a>Ключ приложения (имя)

Свойство [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) задается в конфигурации узла во время создания узла. Чтобы явно задать значение, используйте [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey).

**Ключ**: applicationName  
**Тип**: *string*  
**По умолчанию**: имя сборки, содержащей точку входа приложения  
**Задается с помощью**: `HostBuilderContext.HostingEnvironment.ApplicationName`  
**Переменная среды**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` [необязательно и определяется пользователем](#configurehostconfiguration))

### <a name="content-root"></a>Корневой каталог содержимого

Этот параметр определяет, где узел начинает искать файлы содержимого.

**Ключ**: contentRoot  
**Тип**: *string*  
**Значение по умолчанию**: папка, в которой находится сборка приложения.  
**Задается с помощью**: `UseContentRoot`  
**Переменная среды**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` [необязательно и определяется пользователем](#configurehostconfiguration))

Если путь не существует, узел не запускается.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

### <a name="environment"></a>Среда

Задает [среду](xref:fundamentals/environments) приложения.

**Ключ**: environment  
**Тип**: *string*  
**Значение по умолчанию**: Production  
**Задается с помощью**: `UseEnvironment`  
**Переменная среды**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` [необязательно и определяется пользователем](#configurehostconfiguration))

В качестве среды можно указать любое значение. В платформе определены значения `Development`, `Staging` и `Production`. Регистр символов в значениях не учитывается.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a>ConfigureHostConfiguration

Метод `ConfigureHostConfiguration` использует <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>, чтобы создать экземпляр <xref:Microsoft.Extensions.Configuration.IConfiguration> для узла. Конфигурация узла применяется для инициализации <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> в целях использования в процессе сборки приложения.

Метод `ConfigureHostConfiguration` может вызываться несколько раз с накоплением результатов. Узел использует значение, заданное последним для данного ключа.

Конфигурация узла автоматически передается в конфигурацию приложения ([ConfigureAppConfiguration](#configureappconfiguration) и остальную часть приложения).

Поставщики не включены по умолчанию. Необходимо явно указать поставщики конфигурации, требуемые приложению в `ConfigureHostConfiguration`, в том числе:

* конфигурация файла (например, из файла *hostsettings.json*);
* конфигурация переменной среды;
* конфигурация аргумента командной строки;
* другие необходимые поставщики конфигурации.

Конфигурация файла узла включается путем указания основного пути приложения с помощью `SetBasePath` и последующего вызова одного из [поставщиков конфигурации файла](xref:fundamentals/configuration/index#file-configuration-provider). В примере приложения используется JSON-файл *hostsettings.json* и вызывается метод <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> для использования параметров конфигурации узла файла.

Чтобы добавить [конфигурацию переменной среды](xref:fundamentals/configuration/index#environment-variables-configuration-provider) узла, вызовите метод <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> в построителе узла. `AddEnvironmentVariables` принимает необязательный определяемый пользователем префикс. В примере приложения используется префикс `PREFIX_`. Префикс удаляется при чтении переменных среды. После настройки узла в примере приложения значение переменной среды для `PREFIX_ENVIRONMENT` становится значением конфигурации узла для ключа `environment`.

Во время разработки с использованием [Visual Studio](https://www.visualstudio.com/) или запуска приложения с помощью `dotnet run` переменные среды можно задать в файле *Properties/launchSettings.json*. В [Visual Studio Code](https://code.visualstudio.com/) переменные среды можно задавать в файле *.vscode/launch.json* во время разработки. Дополнительные сведения см. в разделе <xref:fundamentals/environments>.

[Конфигурация командной строки](xref:fundamentals/configuration/index#command-line-configuration-provider) добавляется путем вызова метода <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>. Конфигурация командной строки добавляется в последнюю очередь, чтобы разрешить аргументам командной строки переопределять конфигурацию, предоставленную предыдущими поставщиками конфигурации.

*hostsettings.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

Дополнительную конфигурацию можно указать с помощью ключей [applicationName](#application-key-name) и [contentRoot](#content-root).

Пример конфигурации `HostBuilder` с использованием `ConfigureHostConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Конфигурация приложения создается путем вызова метода <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> в реализации <xref:Microsoft.Extensions.Hosting.IHostBuilder>. Метод `ConfigureAppConfiguration` использует <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>, чтобы создать экземпляр <xref:Microsoft.Extensions.Configuration.IConfiguration> для приложения. Метод `ConfigureAppConfiguration` может вызываться несколько раз с накоплением результатов. Приложение использует значение, заданное последним для данного ключа. Конфигурация, созданная с помощью `ConfigureAppConfiguration`, доступна в свойствах [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) для последующих операций и в <xref:Microsoft.Extensions.Hosting.IHost.Services*>.

Конфигурация приложения автоматически получает конфигурацию узла, предоставленную с помощью [ConfigureHostConfiguration](#configurehostconfiguration).

Пример конфигурации приложения с использованием `ConfigureAppConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appsettings.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appsettings.Development.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appsettings.Production.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

Чтобы переместить файлы параметров в выходной каталог, укажите файлы параметров как [элементы проекта MSBuild](/visualstudio/msbuild/common-msbuild-project-items) в файле проекта. Пример приложения перемещает свои файлы параметров приложения JSON и *hostsettings.json* со следующим элементом `<Content>`:

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a>ConfigureServices

Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> добавляет службы в контейнер [внедрения зависимостей](xref:fundamentals/dependency-injection) приложения. Метод `ConfigureServices` может вызываться несколько раз с накоплением результатов.

Размещенная служба — это класс с логикой фоновой задачи, реализующий интерфейс <xref:Microsoft.Extensions.Hosting.IHostedService>. Дополнительные сведения см. в разделе <xref:fundamentals/host/hosted-services>.

[Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) использует метод расширения `AddHostedService` для добавления службы для событий времени жизни (`LifetimeEventsHostedService`) и синхронизированной фоновой задачи (`TimedHostedService`):

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> добавляет делегат для настройки указанного интерфейса <xref:Microsoft.Extensions.Logging.ILoggingBuilder>. Метод `ConfigureLogging` может вызываться несколько раз с накоплением результатов.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> прослушивает сигналы `Ctrl+C`/SIGINT или SIGTERM и вызывает метод <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> для запуска процесса завершения работы. Метод `UseConsoleLifetime` разблокирует расширения, такие как [RunAsync](#runasync) и [WaitForShutdownAsync](#waitforshutdownasync). Класс <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> предварительно зарегистрирован и является реализацией времени жизни по умолчанию. Используется то время жизни, которое зарегистрировано последним.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>Конфигурация контейнера

Для поддержки подключения к другим контейнерам узел может принимать <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory`1>. Предоставляет фабрику, не являющуюся частью регистрации контейнера внедрения зависимостей, а являющуюся встроенной функцией узла, которая используется для создания конкретных контейнеров внедрения зависимостей. [UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) переопределяет фабрику по умолчанию, используемую для создания поставщика службы приложения.

Пользовательская конфигурация контейнера управляется методом <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>. Метод `ConfigureContainer` обеспечивает возможности строго типизированной настройки контейнера на основе базового API узла. Метод `ConfigureContainer` может вызываться несколько раз с накоплением результатов.

Создание контейнера службы для приложения:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

Настройка фабрики контейнера службы:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

Использование фабрики и настройка пользовательского контейнера служб для приложения:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a>Расширение среды

Расширяемость узла выполняется с помощью методов расширения класса `IHostBuilder`. В следующем примере показано, как метод расширения расширяет реализацию класса `IHostBuilder` с помощью класса [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks), пример которого приведен в статье <xref:fundamentals/host/hosted-services>.

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

Приложение устанавливает метод расширения `UseHostedService`, чтобы зарегистрировать размещенную службу, переданную в `T`:

```csharp
using System;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

public static class Extensions
{
    public static IHostBuilder UseHostedService<T>(this IHostBuilder hostBuilder)
        where T : class, IHostedService, IDisposable
    {
        return hostBuilder.ConfigureServices(services =>
            services.AddHostedService<T>());
    }
}
```

## <a name="manage-the-host"></a>Управление узлом

Реализация <xref:Microsoft.Extensions.Hosting.IHost> отвечает за запуск и остановку реализаций `IHostedService`, которые зарегистрированы в контейнере службы.

### <a name="run"></a>Выполнить

Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> запускает приложение и блокирует вызывающий поток, пока работа узла не будет завершена:

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        host.Run();
    }
}
```

### <a name="runasync"></a>RunAsync

Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> запускает приложение и возвращает объект `Task`, который завершается при активации токена отмены или завершение работы:

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        await host.RunAsync();
    }
}
```

### <a name="runconsoleasync"></a>RunConsoleAsync

Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> включает поддержку консоли, собирает и запускает узел и ожидает сигналы `Ctrl+C`/SIGINT или SIGTERM для завершения работы.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var hostBuilder = new HostBuilder();

        await hostBuilder.RunConsoleAsync();
    }
}
```

### <a name="start-and-stopasync"></a>Start и StopAsync

Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> запускает узел синхронно.

Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> пытается остановить узел в течение указанного периода ожидания.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            await host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

### <a name="startasync-and-stopasync"></a>StartAsync и StopAsync

Метод <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> запускает приложение.

Метод <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> останавливает приложение.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.StopAsync();
        }
    }
}
```

### <a name="waitforshutdown"></a>WaitForShutdown

Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> запускается с помощью <xref:Microsoft.Extensions.Hosting.IHostLifetime>, например <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (прослушивает `Ctrl+C`/SIGINT или SIGTERM). `WaitForShutdown` вызывает <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            host.WaitForShutdown();
        }
    }
}
```

### <a name="waitforshutdownasync"></a>WaitForShutdownAsync

Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> возвращает объект `Task`, который завершается после активации завершения работы через токен, и вызывает метод <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.WaitForShutdownAsync();
        }

    }
}
```

### <a name="external-control"></a>Внешнее управление

Внешнее управление узла может осуществляться с помощью методов, которые могут быть вызваны извне:

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

Метод <xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> вызывается в начале <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, который ждет, пока он не будет завершен, прежде чем продолжить. Можно отложить запуск до получения сигнала от внешнего события.

## <a name="ihostingenvironment-interface"></a>Интерфейс IHostingEnvironment

Интерфейс <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> предоставляет сведения о среде размещения приложения. Чтобы получить интерфейс `IHostingEnvironment` для использования его свойств и методов расширения, воспользуйтесь [внедрением конструктора](xref:fundamentals/dependency-injection):

```csharp
public class MyClass
{
    private readonly IHostingEnvironment _env;

    public MyClass(IHostingEnvironment env)
    {
        _env = env;
    }

    public void DoSomething()
    {
        var environmentName = _env.EnvironmentName;
    }
}
```

Дополнительные сведения см. в разделе <xref:fundamentals/environments>.

## <a name="iapplicationlifetime-interface"></a>Интерфейс IApplicationLifetime

Интерфейс <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> позволяет выполнять действия после запуска и завершения работы, включая запросы о нормальном завершении работы. Три свойства этого интерфейса представляют собой токены отмены, которые служат для регистрации методов `Action`, определяющих события запуска и завершения работы.

| Токен отмены | Условие инициации&#8230; |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | Узел полностью запущен. |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | Заканчивается нормальное завершение работы узла. Все запросы должны быть обработаны. Завершение работы блокируется до тех пор, пока это событие не завершится. |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | Происходит нормальное завершение работы узла. Запросы могут все еще обрабатываться. Завершение работы блокируется до тех пор, пока это событие не завершится. |

Конструктор внедряет службу `IApplicationLifetime` в любой класс. [Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) использует внедрение в конструкторе в класс `LifetimeEventsHostedService` (реализацию `IHostedService`) для регистрации событий.

*LifetimeEventsHostedService.cs*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

Метод <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> запрашивает остановку приложения. Следующий класс использует `StopApplication` для корректного завершения работы приложения при вызове метода класса `Shutdown`:

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/host/hosted-services>
* [Репозиторий примеров размещения на GitHub](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
