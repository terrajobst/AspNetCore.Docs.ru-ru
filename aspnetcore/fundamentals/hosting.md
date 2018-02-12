---
title: "Размещение в ASP.NET Core"
author: guardrex
description: "Сведения о веб-узле в ASP.NET Core, который отвечает за запуск приложений и управление временем существования."
manager: wpickett
ms.author: riande
ms.date: 09/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosting
ms.openlocfilehash: 78209c8d34fa1a2a164ae333d625feca1e145e89
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="hosting-in-aspnet-core"></a>Размещение в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Приложения ASP.NET Core настраивают и запускают *узел*. Узел отвечает за запуск приложения и управление временем существования. Узел настраивает как минимум сервер и конвейер обработки запросов.

## <a name="setting-up-a-host"></a>Настройка узла

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Создайте узел с помощью экземпляра [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Обычно это делается в точке входа в приложение, то есть в методе `Main`. В шаблонах проектов метод `Main` находится в файле *Program.cs*. Обычно *Program.cs* вызывает [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), чтобы начать настройку узла.

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

Метод `CreateDefaultBuilder` выполняет указанные ниже задачи.

* Настраивает [Kestrel](servers/kestrel.md) в качестве веб-сервера. Параметры Kestrel по умолчанию см. в разделе [Параметры Kestrel](xref:fundamentals/servers/kestrel#kestrel-options) статьи "Общие сведения о реализации веб-сервера Kestrel в ASP.NET Core".
* В качестве корня содержимого задает путь, возвращенный методом [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).
* Загружает дополнительные параметры конфигурации из следующих файлов и элементов:
  * *appsettings.json*;
  * *appsettings.{Environment}.json*;
  * [секреты пользователя](xref:security/app-secrets), когда приложение выполняется в среде `Development`;
  * Переменные среды.
  * аргументы командной строки.
* Настраивает [ведение журнала](xref:fundamentals/logging/index) для выходных данных консоли и отладки. Ведение журнала включает в себя правила [фильтрации журналов](xref:fundamentals/logging/index#log-filtering), заданные в разделе конфигурации ведения журнала в файле *appsettings.json* или *appsettings.{Environment}.json*.
* При выполнении за службами IIS обеспечивает [интеграцию со службами IIS](xref:host-and-deploy/iis/index). Настраивает базовый путь и порт, через который сервер ожидает передачи данных при использовании [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). Модуль создает обратный прокси-сервер между службами IIS и Kestrel. Кроме того, настраивает [перехват приложением ошибок запуска](#capture-startup-errors). Параметры служб IIS по умолчанию см. в разделе [Параметры служб IIS](xref:host-and-deploy/iis/index#iis-options) статьи "Размещение ASP.NET Core в Windows со службами IIS".

*Корень содержимого* определяет, где узел ищет файлы содержимого, например файлы представлений MVC. При запуске приложения из корневой папки проекта эта папка используется в качестве корня содержимого. Такое поведение по умолчанию принято в [Visual Studio](https://www.visualstudio.com/) и [шаблонах dotnet new](/dotnet/core/tools/dotnet-new).

Дополнительные сведения о конфигурации приложения см. в разделе [Конфигурация в ASP.NET Core](xref:fundamentals/configuration/index).

> [!NOTE]
> Помимо использования статического метода `CreateDefaultBuilder`, в ASP.NET Core 2.x поддерживается создание узла на основе [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Дополнительные сведения см. на вкладке со сведениями об ASP.NET Core 1.x.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Создайте узел с помощью экземпляра [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Узел обычно создается в точке входа в приложение, то есть в методе `Main`. В шаблонах проектов метод `Main` находится в файле *Program.cs*.

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

Для `WebHostBuilder` требуется [сервер, реализующий интерфейс IServer](servers/index.md). Встроенными серверами являются [Kestrel](servers/kestrel.md) и [HTTP.sys](servers/httpsys.md) (до выхода ASP.NET Core 2.0 сервер HTTP.sys назывался [WebListener](xref:fundamentals/servers/weblistener)). В этом примере [метод расширения UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) задает сервер Kestrel.

*Корень содержимого* определяет, где узел ищет файлы содержимого, например файлы представлений MVC. Для получения корня содержимого по умолчанию для `UseContentRoot` используется метод [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1). При запуске приложения из корневой папки проекта эта папка используется в качестве корня содержимого. Такое поведение по умолчанию принято в [Visual Studio](https://www.visualstudio.com/) и [шаблонах dotnet new](/dotnet/core/tools/dotnet-new).

Чтобы использовать службы IIS в качестве обратного прокси-сервера, вызовите метод [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) в процессе создания узла. Метод `UseIISIntegration` не настраивает *сервер*, как это делает метод [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1). `UseIISIntegration` настраивает базовый путь и порт, через который сервер ожидает передачи данных при использовании [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) для создания обратного прокси-сервера между Kestrel и службами IIS. Чтобы использовать службы IIS с ASP.NET Core, нужно указать `UseKestrel` и `UseIISIntegration`. `UseIISIntegration` активирует только при выполнении за службами IIS или IIS Express. Дополнительные сведения см. в разделах [Введение в модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) и [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

Минимальная реализация для настройки узла (и приложения ASP.NET Core) включает в себя указание сервера и настройку конвейера запросов приложения.

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

---

При настройке узла можно предоставить методы [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) и [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1). Если используется класс `Startup`, в нем должен быть определен метод `Configure`. Дополнительные сведения см. в разделе [Запуск приложения в ASP.NET Core](startup.md). Несколько вызовов `ConfigureServices` добавляются друг к другу. При нескольких вызовах `Configure` или `UseStartup` в `WebHostBuilder` предыдущие параметры заменяются.

## <a name="host-configuration-values"></a>Значения конфигурации узла

Для задания значений конфигурации узла класс [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) поддерживает следующие подходы:

* конфигурация построителя узла, которая включает в себя переменные среды в формате `ASPNETCORE_{configurationKey}`, например `ASPNETCORE_URLS`;
* явные методы, такие как `CaptureStartupErrors`;
* метод [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) и связанный ключ. Значение, задаваемое с помощью `UseSetting`, всегда является строкой независимо от типа.

Хост использует значение, заданное последним. Дополнительные сведения см. в подразделе [Переопределение конфигурации](#overriding-configuration) в следующем разделе.

### <a name="capture-startup-errors"></a>Перехват ошибок при загрузке

Этот параметр управляет перехватом ошибок при загрузке.

**Ключ**: captureStartupErrors  
**Тип**: *bool* (`true` или `1`)  
**Значение по умолчанию**: `false`, если только приложение не работает с сервером Kestrel за службами IIS; в этом случае значение по умолчанию — `true`.  
**Задается с помощью**: `CaptureStartupErrors`  
**Переменная среды**: `ASPNETCORE_CAPTURESTARTUPERRORS`

Если задано значение `false`, ошибки во время запуска приводят к завершению работы узла. Если задано значение `true`, узел перехватывает исключения во время запуска и пытается запустить сервер.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a>Корень содержимого

Этот параметр определяет то, где ASP.NET Core начинает искать файлы содержимого, например представления MVC. 

**Ключ**: contentRoot  
**Тип**: *string*  
**Значение по умолчанию**: папка, в которой находится сборка приложения.  
**Задается с помощью**: `UseContentRoot`  
**Переменная среды**: `ASPNETCORE_CONTENTROOT`

Корень содержимого также используется в качестве базового пути для [корневого каталога документов](#web-root). Если путь не существует, узел не запускается.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a>Подробные сообщения об ошибках

Определяет, следует ли перехватывать подробные сообщения об ошибках.

**Ключ**: detailedErrors  
**Тип**: *bool* (`true` или `1`)  
**Значение по умолчанию**: false  
**Задается с помощью**: `UseSetting`  
**Переменная среды**: `ASPNETCORE_DETAILEDERRORS`

Если этот параметр включен (или если параметр <a href="#environment">Среда</a> имеет значение `Development`), приложение перехватывает подробные исключения.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a>Среда

Задает среду приложения.

**Ключ**: environment  
**Тип**: *string*  
**Значение по умолчанию**: Production  
**Задается с помощью**: `UseEnvironment`  
**Переменная среды**: `ASPNETCORE_ENVIRONMENT`

В качестве среды можно указать любое значение. В платформе определены значения `Development`, `Staging` и `Production`. Регистр символов в значениях не учитывается. По умолчанию значение параметра *Среда* считывается из переменной среды `ASPNETCORE_ENVIRONMENT`. При использовании [Visual Studio](https://www.visualstudio.com/) переменные среды можно задавать в файле *launchSettings.json*. Дополнительные сведения см. в статье [Работа с несколькими средами](xref:fundamentals/environments).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a>Начальные сборки размещения

Задает начальные сборки размещения для приложения.

**Ключ**: hostingStartupAssemblies  
**Тип**: *string*  
**Значение по умолчанию**: пустая строка  
**Задается с помощью**: `UseSetting`  
**Переменная среды**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`

Разделенная точками с запятой строка начальных сборок размещения, загружаемых при запуске. Это новая возможность в ASP.NET Core 2.0.

Хотя значением по умолчанию этого параметра конфигурации является пустая строка, начальные сборки размещения всегда включают в себя сборку приложения. Если начальные сборки размещения указаны, они добавляются к сборке приложения для загрузки во время построения приложением общих служб при запуске.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Эта возможность недоступна в ASP.NET Core 1.x.

---

### <a name="prefer-hosting-urls"></a>Предпочитать URL-адреса размещения

Указывает, должен ли узел ожидать передачи данных по URL-адресам, настроенным с помощью `WebHostBuilder`, вместо настроенных с помощью реализации `IServer`.

**Ключ**: preferHostingUrls  
**Тип**: *bool* (`true` или `1`)  
**Значение по умолчанию**: true  
**Задается с помощью**: `PreferHostingUrls`  
**Переменная среды**: `ASPNETCORE_PREFERHOSTINGURLS`

Это новая возможность в ASP.NET Core 2.0.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Эта возможность недоступна в ASP.NET Core 1.x.

---

### <a name="prevent-hosting-startup"></a>Запретить запуск размещения

Запрещает автоматическую загрузку начальных сборок размещения, включая начальные сборки размещения, настроенные сборкой приложения. Дополнительные сведения см. в разделе [Добавление функций приложения из внешней сборки с помощью IHostingStartup](xref:host-and-deploy/ihostingstartup).

**Ключ**: preventHostingStartup  
**Тип**: *bool* (`true` или `1`)  
**Значение по умолчанию**: false  
**Задается с помощью**: `UseSetting`  
**Переменная среды**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`

Это новая возможность в ASP.NET Core 2.0.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Эта возможность недоступна в ASP.NET Core 1.x.

---

### <a name="server-urls"></a>URL-адреса сервера

Задает IP-адреса или адреса узлов с портами и протоколами, по которым сервер должен ожидать получения запросов.

**Ключ**: urls  
**Тип**: *string*  
**Значение по умолчанию**: http://localhost:5000  
**Задается с помощью**: `UseUrls`  
**Переменная среды**: `ASPNETCORE_URLS`

Укажите разделенный точками с запятой (;) список префиксов URL-адресов, на которые сервер должен отвечать. Например, `http://localhost:123`. Используйте символ "\*", чтобы указать, что сервер должен ожидать получения запросов через определенный порт и по определенному протоколу по любому IP-адресу или имени узла (например, `http://*:5000`). Протокол (`http://` или `https://`) должен указываться для каждого URL-адреса. Поддерживаемые форматы зависят от сервера.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

Kestrel имеет собственный интерфейс API настройки конечных точек. Дополнительные сведения см. в статье [Реализация веб-сервера Kestrel в ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a>Время ожидания завершения работы

Определяет, как долго необходимо ожидать завершения работы веб-узла.

**Ключ**: shutdownTimeoutSeconds  
**Тип**: *int*  
**Значение по умолчанию**: 5  
**Задается с помощью**: `UseShutdownTimeout`  
**Переменная среды**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`

Хотя ключ принимает значение *int* с `UseSetting` (например, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), метод расширения `UseShutdownTimeout` принимает `TimeSpan`. Это новая возможность в ASP.NET Core 2.0.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Эта возможность недоступна в ASP.NET Core 1.x.

---

### <a name="startup-assembly"></a>Стартовая сборка

Определяет сборку, в которой необходимо искать класс `Startup`.

**Ключ**: startupAssembly  
**Тип**: *string*  
**Значение по умолчанию**: сборка приложения  
**Задается с помощью**: `UseStartup`  
**Переменная среды**: `ASPNETCORE_STARTUPASSEMBLY`

На сборку можно ссылаться по имени (`string`) или типу (`TStartup`). При вызове нескольких методов `UseStartup` приоритет имеет последний.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
    ...
```

---

### <a name="web-root"></a>Корневой каталог документов

Задает относительный путь к статическим ресурсам приложения.

**Ключ**: webroot  
**Тип**: *string*  
**Значение по умолчанию**: если значение не задано, по умолчанию используется путь "(Корень содержимого)/wwwroot" при его наличии. Если этот путь не существует, используется фиктивный поставщик файлов.  
**Задается с помощью**: `UseWebRoot`  
**Переменная среды**: `ASPNETCORE_WEBROOT`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a>Переопределение конфигурации

Для настройки хоста используется [конфигурация](xref:fundamentals/configuration/index). В приведенном ниже примере необязательная конфигурация хоста задается в файле *hosting.json*. Любую конфигурацию, загружаемую из файла *hosting.json*, можно переопределить с помощью аргументов командной строки. Встроенная конфигурация (в `config`) используется для настройки узла с помощью `UseConfiguration`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

*hosting.json*:

```json
{
    urls: "http://*:5005"
}
```

Конфигурация, предоставленная методом `UseUrls`, сначала переопределяется конфигурацией из файла *hosting.json*, а затем с помощью аргументов командной строки.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();
    }
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

*hosting.json*:

```json
{
    urls: "http://*:5005"
}
```

Конфигурация, предоставленная методом `UseUrls`, сначала переопределяется конфигурацией из файла *hosting.json*, а затем с помощью аргументов командной строки.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

---

> [!NOTE]
> Метод расширения [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) в настоящее время не может анализировать раздел конфигурации, возвращаемый методом `GetSection` (например, `.UseConfiguration(Configuration.GetSection("section"))`). Метод `GetSection` фильтрует ключи конфигурации до запрошенного раздела, но оставляет имя раздела в ключах (например, `section:urls`, `section:environment`). Метод `UseConfiguration` ожидает, что ключи соответствуют ключам `WebHostBuilder` (например, `urls`, `environment`). Наличие имени раздела в ключах предотвращает настройку узла с помощью значений этого раздела. Эта проблема будет устранена в следующем выпуске. Дополнительные сведения и способы решения см. в описании проблемы [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (При передаче раздела конфигурации в WebHostBuilder.UseConfiguration используются полные ключи).

Чтобы указать узел, выполняющийся по определенному URL-адресу, можно передать нужное значение в окне командной строки при выполнении команды `dotnet run`. Аргумент командной строки переопределяет значение `urls` из файла *hosting.json*, и сервер будет ожидать передачи данных через порт 8080.

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a>Запуск узла

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**Выполнить**

Метод `Run` запускает веб-приложение и блокирует вызывающий поток до тех пор, пока работа узла не будет завершена.

```csharp
host.Run();
```

**Start**

Чтобы запустить узел без блокировки, вызовите метод `Start`.

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Если в метод `Start` передается список URL-адресов, он будет ожидать передачи данных по указанным URL-адресам.

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

Приложение может инициализировать и запустить новый узел с использованием предварительно настроенных значений по умолчанию `CreateDefaultBuilder` с помощью статического удобного метода. Эти методы запускают сервер без вывода данных в консоль и со временем ожидания прерывания, равным [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) (Ctrl-C/SIGINT или SIGTERM):

**Start(RequestDelegate app)**

Выполните запуск с помощью `RequestDelegate`:

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

В браузере выполните запрос по адресу `http://localhost:5000`, чтобы получить ответ "Hello World!" `WaitForShutdown` блокируется, пока не будет создано прерывание (Ctrl-C/SIGINT или SIGTERM). Приложение выводит сообщение `Console.WriteLine` и ожидает нажатия клавиши, после чего завершает работу.

**Start(string url, RequestDelegate app)**

Выполните запуск с помощью URL-адреса и `RequestDelegate`:

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Результат будет тем же, что и при использовании **Start(RequestDelegate app)**, но приложение отвечает по адресу `http://localhost:8080`.

**Start(Action<IRouteBuilder> routeBuilder)**

Используйте экземпляр `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) для применения ПО промежуточного слоя маршрутизации:

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

В этом примере используйте следующие запросы в браузере:

| Запрос                                    | Ответ                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | Hello, Martin!                           |
| `http://localhost:5000/buenosdias/Catrina` | Buenos dias, Catrina!                    |
| `http://localhost:5000/throw/ooops!`       | Вызывает исключение со строкой "ooops!" |
| `http://localhost:5000/throw`              | Вызывает исключение со строкой "Uh oh!" |
| `http://localhost:5000/Sante/Kevin`        | Sante, Kevin!                            |
| `http://localhost:5000`                    | Пример "Здравствуй,                             |

`WaitForShutdown` блокируется, пока не будет создано прерывание (Ctrl-C/SIGINT или SIGTERM). Приложение выводит сообщение `Console.WriteLine` и ожидает нажатия клавиши, после чего завершает работу.

**Start(string url, Action<IRouteBuilder> routeBuilder)**

Используйте URL-адрес и экземпляр `IRouteBuilder`:

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Результат будет тем же, что и при использовании **Start(Action<IRouteBuilder> routeBuilder)**, но приложение отвечает по адресу `http://localhost:8080`.

**StartWith(Action<IApplicationBuilder> app)**

Предоставьте делегат для настройки `IApplicationBuilder`:

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

В браузере выполните запрос по адресу `http://localhost:5000`, чтобы получить ответ "Hello World!" `WaitForShutdown` блокируется, пока не будет создано прерывание (Ctrl-C/SIGINT или SIGTERM). Приложение выводит сообщение `Console.WriteLine` и ожидает нажатия клавиши, после чего завершает работу.

**StartWith(string url, Action<IApplicationBuilder> app)**

Предоставьте URL-адрес и делегат для настройки `IApplicationBuilder`:

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Результат будет тем же, что и при использовании **StartWith(Action<IApplicationBuilder> app)**, но приложение отвечает по адресу `http://localhost:8080`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**Выполнить**

Метод `Run` запускает веб-приложение и блокирует вызывающий поток до тех пор, пока работа узла не будет завершена.

```csharp
host.Run();
```

**Start**

Чтобы запустить узел без блокировки, вызовите метод `Start`.

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Если в метод `Start` передается список URL-адресов, он будет ожидать передачи данных по указанным URL-адресам.


```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

---

## <a name="ihostingenvironment-interface"></a>Интерфейс IHostingEnvironment

[Интерфейс IHostingEnvironment](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) предоставляет сведения о среде веб-размещения приложения. Чтобы получить интерфейс `IHostingEnvironment` для использования его свойств и методов расширения, воспользуйтесь [внедрением конструктора](xref:fundamentals/dependency-injection):

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

Для настройки приложения при запуске в соответствии со средой можно применять [подход на основе соглашения](xref:fundamentals/environments#startup-conventions). Кроме того, можно внедрить интерфейс `IHostingEnvironment` в конструктор `Startup` для использования в `ConfigureServices`:

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> Помимо метода расширения `IsDevelopment`, интерфейс `IHostingEnvironment` предоставляет методы `IsStaging`, `IsProduction` и `IsEnvironment(string environmentName)`. Подробные сведения см. в статье [Работа с несколькими средами](xref:fundamentals/environments).

Службу `IHostingEnvironment` также можно внедрять непосредственно в метод `Configure` для настройки конвейера обработки:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

`IHostingEnvironment` можно внедрить в метод `Invoke` при создании пользовательского [ПО промежуточного слоя](xref:fundamentals/middleware#writing-middleware):

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a>Интерфейс IApplicationLifetime

[Интерфейс IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) позволяет выполнять действия после запуска и завершения работы. Три свойства этого интерфейса представляют собой токены отмены, которые служат для регистрации методов `Action`, определяющих события запуска и завершения работы. Также имеется метод `StopApplication`.

| Токен отмены    | Условие инициации&#8230; |
| --------------------- | --------------------- |
| `ApplicationStarted`  | Узел полностью запущен. |
| `ApplicationStopping` | Происходит нормальное завершение работы узла. Запросы могут все еще обрабатываться. Завершение работы блокируется до тех пор, пока это событие не завершится. |
| `ApplicationStopped`  | Заканчивается нормальное завершение работы узла. Все запросы должны быть обработаны. Завершение работы блокируется до тех пор, пока это событие не завершится. |

| Метод            | Действие                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | Запрашивает завершение работы текущего приложения. |

```csharp
public class Startup 
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime) 
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

## <a name="troubleshooting-systemargumentexception"></a>Устранение неполадок, связанных с исключениями System.ArgumentException

**Относится только к ASP.NET Core 2.0**

Узел может создаваться путем внедрения интерфейса `IStartup` непосредственно в контейнер внедрения зависимостей, а не путем вызова `UseStartup` или `Configure`:

```csharp
services.AddSingleton<IStartup, Startup>();
```

Если узел создается таким способом, может произойти следующая ошибка:

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

Причина в том, что [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (текущая сборка) требуется для проверки `HostingStartupAttributes`. Если в приложении интерфейс `IStartup` вручную внедряется в контейнер внедрения зависимостей, добавьте следующий вызов в `WebHostBuilder` с указанием имени сборки:

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

Кроме того, в `WebHostBuilder` можно добавить метод-заглушку `Configure`, который автоматически задает `applicationName`(`ApplicationKey`):

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

**Примечание**. Это необходимо только в выпуске ASP.NET Core 2.0 и только в случае, если приложение не вызывает метод `UseStartup` или `Configure`.

Дополнительные сведения см. в [комментарии к объявлению об удалении пакета Microsoft.Extensions.PlatformAbstractions](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) и в [примере использования StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Размещение в Windows с помощью IIS](xref:host-and-deploy/iis/index)
* [Размещение в Linux с использованием Nginx](xref:host-and-deploy/linux-nginx)
* [Размещение в Linux с использованием Apache](xref:host-and-deploy/linux-apache)
* [Размещение в службе Windows](xref:host-and-deploy/windows-service)
