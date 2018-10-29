---
title: Универсальный узел .NET
author: guardrex
description: Сведения об универсальном узле в .NET, который отвечает за запуск приложений и управление временем существования.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: e5f91ed64b7f8402dfe938f0fa8a0d94755d15c6
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207723"
---
# <a name="net-generic-host"></a>Универсальный узел .NET

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Приложения .NET Core настраивают и запускают *узел*. Узел отвечает за запуск приложения и управление временем существования. В этом разделе рассматриваются универсальный узел ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), используемый для размещения приложений, которые не умеют обрабатывать запросы HTTP. Сведения о веб-узле ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)) см. в статье <xref:fundamentals/host/web-host>.

Универсальный узел предназначен для отделения конвейера HTTP от API веб-узла, чтобы можно было иметь больше сценариев узла. Обмен сообщениями, фоновые задачи и другие рабочие нагрузки типа "не HTTP", использующие перекрестные возможности универсальных узлов, такие как конфигурация, внедрение зависимости (DI) и ведения журналов.

Универсальный узел является новой возможностью ASP.NET Core 2.1 и не подходит для сценариев с веб-размещением. Сведения о сценариях с веб-размещением см. в статье о [веб-узле](xref:fundamentals/host/web-host). Универсальный узел разрабатывается с целью заменить веб-узел в будущих версиях. Он будет выступать основным узлом API для сценариев HTTP и "не HTTP".

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([как скачивать](xref:index#how-to-download-a-sample))

При выполнении примера приложения в [Visual Studio Code](https://code.visualstudio.com/) используйте *внешний или встроенный терминал*. Не выполняйте пример в `internalConsole`.

Настройка консоли в Visual Studio Code:

1. Откройте файл *.vscode/launch.json*.
1. В разделе конфигурации **.NET Core Launch (console)** найдите параметр **console**. Измените значение на `externalTerminal` или `integratedTerminal`.

## <a name="introduction"></a>Вступление

Библиотека универсального узла находится в [пространстве имен Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) и включена в [пакет Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/). Пакет `Microsoft.Extensions.Hosting` входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 или более поздней версии).

[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) — это точка входа для выполнения кода. Каждая реализация `IHostedService` выполняется в порядке [регистрации служб в ConfigureServices](#configureservices). Метод [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) вызывается в каждом `IHostedService` при запуске узла, а метод [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) вызывается в порядке, обратном регистрации, когда узел корректно завершает работу.

## <a name="set-up-a-host"></a>Создание узла

[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) является основным компонентом, который библиотеки и приложения используют для инициализации, построения и запуска узла:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

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

Для задания значений конфигурации узла класс [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) поддерживает следующие подходы:

* Построитель конфигурации
* Настройка метода расширения

### <a name="configuration-builder"></a>Построитель конфигурации

Конфигурация построителя узла создается путем вызова метода [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) в реализации [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder). Метод `ConfigureHostConfiguration` использует [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder), чтобы создать экземпляр [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) для узла. Построитель конфигурации инициализирует [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) для использования в процессе построения приложения.

Конфигурация переменной среды не добавляется по умолчанию. Вызовите [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) в конструкторе узла, чтобы настроить узел из переменных среды. `AddEnvironmentVariables` принимает необязательный определяемый пользователем префикс. В примере приложения используется префикс `PREFIX_`. Префикс удаляется при чтении переменных среды. После настройки узла в примере приложения значение переменной среды для `PREFIX_ENVIRONMENT` становится значением конфигурации узла для ключа `environment`.

Во время разработки с использованием [Visual Studio](https://www.visualstudio.com/) или запуска приложения с помощью `dotnet run` переменные среды можно задать в файле *Properties/launchSettings.json*. В [Visual Studio Code](https://code.visualstudio.com/) переменные среды можно задавать в файле *.vscode/launch.json* во время разработки. Дополнительные сведения см. в разделе <xref:fundamentals/environments>.

Метод `ConfigureHostConfiguration` может вызываться несколько раз с накоплением результатов. Хост использует значение, заданное последним.

*hostsettings.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

Пример конфигурации `HostBuilder` с использованием `ConfigureHostConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> Метод расширения [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) в настоящее время не может анализировать раздел конфигурации, возвращаемый методом [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (например, `.AddConfiguration(Configuration.GetSection("section"))`). Метод `GetSection` фильтрует ключи конфигурации до запрошенного раздела, но оставляет имя раздела в ключах (например, `section:environment`). Метод `AddConfiguration` ожидает, что ключи соответствуют ключам `HostBuilder` (например, `environment`). Наличие имени раздела в ключах предотвращает настройку узла с помощью значений этого раздела. Эта проблема будет устранена в следующем выпуске. Дополнительные сведения и способы решения см. в описании проблемы [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (При передаче раздела конфигурации в WebHostBuilder.UseConfiguration используются полные ключи).

### <a name="extension-method-configuration"></a>Настройка метода расширения

Методы расширения вызываются в реализации `IHostBuilder`, чтобы настроить корень содержимого и среду.

#### <a name="application-key-name"></a>Ключ приложения (имя)

Свойство [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) задается в конфигурации узла во время создания узла. Чтобы явно задать значение, используйте [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey).

**Ключ**: applicationName  
**Тип**: *string*  
**По умолчанию**: имя сборки, содержащей точку входа приложения  
**Задается с помощью**: `HostBuilderContext.HostingEnvironment.ApplicationName`  
**Переменная среды**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` [необязательно и определяется пользователем](#configuration-builder))

```csharp
var host = new HostBuilder()
    .ConfigureAppConfiguration((hostContext, configApp) =>
    {
        hostContext.HostingEnvironment.ApplicationName = "CustomApplicationName";
    })
```

#### <a name="content-root"></a>Корень содержимого

Этот параметр определяет, где узел начинает искать файлы содержимого.

**Ключ**: contentRoot  
**Тип**: *string*  
**Значение по умолчанию**: папка, в которой находится сборка приложения.  
**Задается с помощью**: `UseContentRoot`  
**Переменная среды**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` [необязательно и определяется пользователем](#configuration-builder))

Если путь не существует, узел не запускается.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a>Среда

Задает [среду](xref:fundamentals/environments) приложения.

**Ключ**: environment  
**Тип**: *string*  
**Значение по умолчанию**: Production  
**Задается с помощью**: `UseEnvironment`  
**Переменная среды**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` [необязательно и определяется пользователем](#configuration-builder))

В качестве среды можно указать любое значение. В платформе определены значения `Development`, `Staging` и `Production`. Регистр символов в значениях не учитывается.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Конфигурация построителя приложения создается путем вызова метода [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) в реализации [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder). Метод `ConfigureAppConfiguration` использует [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder), чтобы создать экземпляр [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) для приложения. Метод `ConfigureAppConfiguration` может вызываться несколько раз с накоплением результатов. Приложение использует то значение, которое задано последним. Конфигурация, созданная с помощью `ConfigureAppConfiguration`, доступна в свойствах [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) для последующих операций и в [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).

Пример конфигурации приложения с использованием `ConfigureAppConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appsettings.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appsettings.Development.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appsettings.Production.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> Метод расширения [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) в настоящее время не может анализировать раздел конфигурации, возвращаемый методом [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (например, `.AddConfiguration(Configuration.GetSection("section"))`). Метод `GetSection` фильтрует ключи конфигурации до запрошенного раздела, но оставляет имя раздела в ключах (например, `section:Logging:LogLevel:Default`). Метод `AddConfiguration` ожидает точное соответствие ключей конфигурации (например, `Logging:LogLevel:Default`). Наличие имени раздела в ключах предотвращает настройку приложения с помощью значений этого раздела. Эта проблема будет устранена в следующем выпуске. Дополнительные сведения и способы решения см. в описании проблемы [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (При передаче раздела конфигурации в WebHostBuilder.UseConfiguration используются полные ключи).

Чтобы переместить файлы параметров в выходной каталог, укажите файлы параметров как [элементы проекта MSBuild](/visualstudio/msbuild/common-msbuild-project-items) в файле проекта. Пример приложения перемещает свои файлы параметров приложения JSON и *hostsettings.json* со следующим элементом **&lt;Content:&gt;**.

```xml
<ItemGroup>
  <Content Include="**\*.json" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a>ConfigureServices

Метод [ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) добавляет службы в контейнер [внедрения зависимостей](xref:fundamentals/dependency-injection) приложения. Метод `ConfigureServices` может вызываться несколько раз с накоплением результатов.

Размещенная служба — это класс с логикой фоновой задачи, реализующий интерфейс [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice). Дополнительные сведения см. в разделе <xref:fundamentals/host/hosted-services>.

[Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) использует метод расширения `AddHostedService` для добавления службы для событий времени жизни (`LifetimeEventsHostedService`) и синхронизированной фоновой задачи (`TimedHostedService`):

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

Метод [ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) добавляет делегата для настройки указанного [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder). Метод `ConfigureLogging` может вызываться несколько раз с накоплением результатов.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

Метод [UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) прослушивает сигналы `Ctrl+C`/SIGINT или SIGTERM и вызывает метод [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) для запуска процесса завершения работы. Метод `UseConsoleLifetime` разблокирует расширения, такие как [RunAsync](#runasync) и [WaitForShutdownAsync](#waitforshutdownasync). Класс [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) предварительно зарегистрирован и является реализацией времени жизни по умолчанию. Используется то время жизни, которое зарегистрировано последним.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>Конфигурация контейнера

Для поддержки подключения к другим контейнерам узел может принимать [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1). Предоставляет фабрику, не являющуюся частью регистрации контейнера внедрения зависимостей, а являющуюся встроенной функцией узла, которая используется для создания конкретных контейнеров внедрения зависимостей. [UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) переопределяет фабрику по умолчанию, используемую для создания поставщика службы приложения.

Пользовательская конфигурация контейнера управляется методом [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer). Метод `ConfigureContainer` обеспечивает возможности строго типизированной настройки контейнера на основе базового API узла. Метод `ConfigureContainer` может вызываться несколько раз с накоплением результатов.

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

Реализация [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) отвечает за запуск и остановку реализаций `IHostedService`, которые зарегистрированы в контейнере службы.

### <a name="run"></a>Выполнить

Метод [Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) запускает приложение и блокирует вызывающий поток, пока работа узла не будет завершена:

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

Метод [RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) запускает приложение и возвращает объект `Task`, который завершается при активации токена отмены или завершение работы:

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

Метод [RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) включает поддержку консоли, собирает и запускает узел и ожидает сигналы `Ctrl+C`/SIGINT или SIGTERM для завершения работы.

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

Метод [Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) запускает узел синхронно.

Метод [StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) пытается остановить узел в течение указанного периода ожидания.

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

Метод [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) запускает приложение.

Метод [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) завершает работу приложения.

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

Метод [WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) инициируется через [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), например через [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (прослушивает сигналы `Ctrl+C`/SIGINT и SIGTERM). Метод `WaitForShutdown` вызывает [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).

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

Метод [WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) возвращает объект `Task`, который завершается после активации завершения работы через токен, и вызывает метод [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).

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

[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) вызывается в начале [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), который ждет, пока он не будет завершен, прежде чем продолжить. Можно отложить запуск до получения сигнала от внешнего события.

## <a name="ihostingenvironment-interface"></a>Интерфейс IHostingEnvironment

Интерфейс [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) предоставляет сведения о среде размещения приложения. Чтобы получить интерфейс `IHostingEnvironment` для использования его свойств и методов расширения, воспользуйтесь [внедрением конструктора](xref:fundamentals/dependency-injection):

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

Интерфейс [IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) позволяет выполнять действия после запуска и завершения работы, включая запросы о нормальном завершении работы. Три свойства этого интерфейса представляют собой токены отмены, которые служат для регистрации методов `Action`, определяющих события запуска и завершения работы.

| Токен отмены | Условие инициации&#8230; |
| ------------------ | --------------------- |
| [ApplicationStarted](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | Узел полностью запущен. |
| [ApplicationStopped](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | Заканчивается нормальное завершение работы узла. Все запросы должны быть обработаны. Завершение работы блокируется до тех пор, пока это событие не завершится. |
| [ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | Происходит нормальное завершение работы узла. Запросы могут все еще обрабатываться. Завершение работы блокируется до тех пор, пока это событие не завершится. |

Конструктор внедряет службу `IApplicationLifetime` в любой класс. [Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) использует внедрение в конструкторе в класс `LifetimeEventsHostedService` (реализацию `IHostedService`) для регистрации событий.

*LifetimeEventsHostedService.cs*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) запрашивает остановку приложения. Следующий класс использует `StopApplication` для корректного завершения работы приложения при вызове метода класса `Shutdown`:

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
