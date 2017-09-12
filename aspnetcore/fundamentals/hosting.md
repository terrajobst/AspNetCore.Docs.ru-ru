---
title: "Размещение в ASP.NET Core"
author: guardrex
description: "Дополнительные сведения о веб-узла в ASP.NET Core, который отвечает за управление запуском и временем существования приложения."
keywords: "ASP.NET Core веб-узла, IWebHost, WebHostBuilder, IHostingEnvironment, IApplicationLifetime"
ms.author: riande
manager: wpickett
ms.date: 09/10/2017
ms.topic: article
ms.assetid: 4e45311d-8d56-46e2-b99d-6f65b648a277
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1a789ff1bc6b3e3af99419e7d74d3fb46bb2345
ms.sourcegitcommit: 368aabde4de3728a8e5a8c016a2ec61f9c0854bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="hosting-in-aspnet-core"></a>Размещение в ASP.NET Core

По [Latham Люк](https://github.com/guardrex)

Настройка приложений ASP.NET Core и запустите *узла*, который отвечает за управление запуском и временем существования приложения. Как минимум узел настраивает сервер и конвейер обработки запросов.

## <a name="setting-up-a-host"></a>Настройка узла

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Создание узла с помощью экземпляра [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Это обычно выполняется в точке входа приложения, `Main` метод. В шаблоны проектов `Main` находится в папке *Program.cs*. Типичный *Program.cs* вызовы [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) начата Настройка узла:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

`CreateDefaultBuilder`выполняет следующие задачи:

* Настраивает [Kestrel](servers/kestrel.md) и веб-сервер.
* Задает содержимое корневого [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).
* Необязательная конфигурация загружает из:
  * *appSettings.JSON*.
  * *appSettings. {Среды} .json*.
  * [Секреты пользователя](xref:security/app-secrets) при запуске приложения `Development` среде.
  * Переменные среды.
  * Аргументы командной строки.
* Настраивает [входа](xref:fundamentals/logging) для вывода на консоль и отладка с [фильтрации журнала](xref:fundamentals/logging#log-filtering) правила, указанные в разделе конфигурации ведения журнала *appsettings.json* или *appsettings. {Среды} .json* файл.
* При работе под IIS обеспечивает интеграцию служб IIS, настроив базовый путь и порт, сервер должен прослушивать при использовании [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). Модуль создает обратного прокси-сервера между Kestrel и службами IIS. Кроме того, настраивает приложение для [перехватить ошибки запуска](#capture-startup-errors).

*Содержимое корневого* определяет, где узел ищет файлы содержимого, такие как файлы представления MVC. Корень содержимого по умолчанию — [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory). Это приведет к с помощью корневую папку веб-проекта, который является корнем содержимого при запуске приложения из корневой папки (например, вызов [dotnet запуска](/dotnet/core/tools/dotnet-run) из папки проекта). Это значение по умолчанию, используемых в [Visual Studio](https://www.visualstudio.com/) и [dotnet новые шаблоны](/dotnet/core/tools/dotnet-new).

В разделе [конфигурации в ASP.NET Core](xref:fundamentals/configuration) Дополнительные сведения о настройке приложения.

> [!NOTE]
> В качестве альтернативы с помощью статического `CreateDefaultBuilder` метод, создание узла из [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) поддерживаемых подход с ASP.NET Core 2.x. См. на вкладке 1.x ASP.NET Core для получения дополнительной информации.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Создание узла с помощью экземпляра [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Это обычно выполняется в точке входа приложения, `Main` метод. В шаблоны проектов `Main` находится в папке *Program.cs*. Следующие *Program.cs* демонстрируется использование `WebHostBuilder` для создания узла:

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

`WebHostBuilder`требуется [сервера, который реализует IServer](servers/index.md). Встроенные серверы являются [Kestrel](servers/kestrel.md) и [HTTP.sys](servers/httpsys.md) (до выхода ASP.NET Core 2.0 был вызван HTTP.sys [WebListener](xref:fundamentals/servers/weblistener)). В этом примере [метод расширения UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) указывает сервер Kestrel.

*Содержимое корневого* определяет, где узел ищет файлы содержимого, такие как файлы представления MVC. Указанный корневой содержимого по умолчанию для `UseContentRoot` — [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1). Это приведет к с помощью корневую папку веб-проекта, который является корнем содержимого при запуске приложения из корневой папки (например, вызов [dotnet запуска](/dotnet/core/tools/dotnet-run) из папки проекта). Это значение по умолчанию, используемых в [Visual Studio](https://www.visualstudio.com/) и [dotnet новые шаблоны](/dotnet/core/tools/dotnet-new).

Чтобы использовать в качестве обратного прокси-сервера IIS, вызовите [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) как часть построения узла. `UseIISIntegration`неправильно настраивает *сервера*, таких как [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does. `UseIISIntegration`настраивает сервер должен прослушивать при использовании порта и пути к базовой папке [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) создание обратного прокси-сервера между Kestrel и службами IIS. Чтобы использовать IIS с ASP.NET Core, необходимо указать `UseKestrel` и `UseIISIntegration`. `UseIISIntegration`активируется, только при запуске IIS или IIS Express. Дополнительные сведения см. в разделе [введение в ASP.NET Core модуля](xref:fundamentals/servers/aspnet-core-module) и [ссылки на конфигурации ASP.NET Core модуль](xref:hosting/aspnet-core-module).

Минимальную реализацию, которая настраивает узел (и приложения ASP.NET Core) включает в себя указание сервера и настройки конвейера запросов приложения:

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

При настройке узла, чтобы обеспечить [Настройка](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) и [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) методы. При указании `Startup` класса, оно должно определять `Configure` метод. Дополнительные сведения см. в разделе [запуск приложения в ASP.NET Core](startup.md). Несколько вызовов `ConfigureServices` append друг с другом. Несколько вызовов `Configure` или `UseStartup` на `WebHostBuilder` заменить предыдущие параметры.

## <a name="host-configuration-values"></a>Значения конфигурации узла

[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) предоставляет методы для настройки большинства значений конфигурации, доступные для узла, которой можно также задать непосредственно с [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) и связанный ключ. При установке значения с `UseSetting`, значение устанавливается в виде строки (в кавычках) независимо от типа.

### <a name="capture-startup-errors"></a>Запись ошибки при загрузке

Этот параметр управляет отслеживания ошибок при запуске.

**Ключ**: captureStartupErrors  
**Тип**: *bool* (`true` или `1`)  
**По умолчанию**: по умолчанию используется значение `false` Если приложение запускается с Kestrel за IIS, где значение по умолчанию — `true`.  
**Задать с помощью**:`CaptureStartupErrors`

Когда `false`, ошибки во время запуска результат в узле выхода. Когда `true`, узел перехватывает исключения во время запуска и пытается запустить сервер.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a>Содержимое корневого

Этот параметр определяет, где ASP.NET Core начинает поиск файлов содержимого, такие как представления MVC. 

**Ключ**: contentRoot  
**Тип**: *строка*  
**По умолчанию**: по умолчанию для папки, в которой хранится сборку приложения.  
**Задать с помощью**:`UseContentRoot`

Содержимое корневого также используется как базовый путь для [параметр корневого веб-каталога](#web-root). Если путь не существует, узлу не удается запустить.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a>Подробные сведения об ошибках

Определяет ли подробные ошибки должны быть зафиксированы.

**Ключ**: detailedErrors  
**Тип**: *bool* (`true` или `1`)  
**По умолчанию**: false  
**Задать с помощью**:`UseSetting`

При включении (или когда <a href="#environment">среды</a> равно `Development`), приложение записывает подробные сведения об исключениях.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a>Среда

Задает среду приложения.

**Ключ**: среды  
**Тип**: *строка*  
**По умолчанию**: рабочей среде  
**Задать с помощью**:`UseEnvironment`

Можно задать *среды* любое значение. Значения, определяемые платформой `Development`, `Staging`, и `Production`. Значения не учитывается регистр. По умолчанию *среды* считывается из `ASPNETCORE_ENVIRONMENT` переменной среды. При использовании [Visual Studio](https://www.visualstudio.com/), переменные среды может быть задан в *launchSettings.json* файла. Дополнительные сведения см. в разделе [работа с несколькими средами](xref:fundamentals/environments).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a>Размещение загрузки сборок

Задает размещения сборок при запуске приложения.

**Ключ**: hostingStartupAssemblies  
**Тип**: *строка*  
**По умолчанию**: пустая строка  
**Задать с помощью**:`UseSetting`

Разделенный точками с запятой строка размещения запуска сборок для загрузки при запуске. Этот компонент впервые появился в ASP.NET Core 2.0.

Несмотря на то, что значения конфигурации по умолчанию устанавливается равным пустой строке, размещения сборок запуска всегда включать сборку приложения. При предоставлении услуг размещения сборок при запуске, они добавляются в сборку приложения для загрузки, когда приложение создает его общие службы во время запуска.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Эта функция недоступна в ASP.NET Core 1.x.

---

### <a name="prefer-hosting-urls"></a>Предпочтение размещение URL-адреса

Указывает ли узел должен прослушивать URL-адресов настроены `WebHostBuilder` вместо настроенных с `IServer` реализации.

**Ключ**: preferHostingUrls  
**Тип**: *bool* (`true` или `1`)  
**По умолчанию**: true  
**Задать с помощью**:`PreferHostingUrls`

Этот компонент впервые появился в ASP.NET Core 2.0.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Эта функция недоступна в ASP.NET Core 1.x.

---

### <a name="prevent-hosting-startup"></a>Запретить размещение запуска

Предотвращает автоматическая загрузка размещение загрузки сборок, включая сборку приложения.

**Ключ**: preventHostingStartup  
**Тип**: *bool* (`true` или `1`)  
**По умолчанию**: false  
**Задать с помощью**:`UseSetting`

Этот компонент впервые появился в ASP.NET Core 2.0.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Эта функция недоступна в ASP.NET Core 1.x.

---

### <a name="server-urls"></a>URL-адреса сервера

Указывает IP-адреса или адреса узлов с портами и протоколы, которые сервер должен прослушивать запросы.

**Ключ**: URL-адреса  
**Тип**: *строка*  
**По умолчанию**: http://localhost: 5000  
**Задать с помощью**:`UseUrls`

Значение, разделенных точкой с запятой (;) префиксов список URL-адрес которого сервер должен отвечать. Например, `http://localhost:123`. Используйте "\*» для указания, что сервер должен прослушивать запросы по любой IP-адрес или имя узла, используя указанный порт и протокол (например, `http://*:5000`). Протокол (`http://` или `https://`) должен быть включен, все URL-адреса. Поддерживаемые форматы различаться для разных серверов.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

Kestrel имеет собственную конфигурацию конечной точки API. Дополнительные сведения см. в разделе [Kestrel реализация веб-сервера в ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a>Время ожидания завершения работы

Указывает время ожидания для завершения работы веб-узла.

**Ключ**: shutdownTimeoutSeconds  
**Тип**: *int*  
**По умолчанию**: 5  
**Задать с помощью**:`UseShutdownTimeout`

Несмотря на то, что ключ принимает *int* с `UseSetting` (например, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), `UseShutdownTimeout` принимает метод расширения `TimeSpan`. Этот компонент впервые появился в ASP.NET Core 2.0.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Эта функция недоступна в ASP.NET Core 1.x.

---

### <a name="startup-assembly"></a>При запуске сборки

Определяет сборку для поиска `Startup` класса.

**Ключ**: startupAssembly  
**Тип**: *строка*  
**По умолчанию**: сборка приложения  
**Задать с помощью**:`UseStartup`

Сборку можно ссылаться по имени (`string`) или тип (`TStartup`). При наличии нескольких `UseStartup` методы вызываются, приоритет имеет последняя.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

### <a name="web-root"></a>Персональный компьютер

Задает относительный путь для приложения статических ресурсах.

**Ключ**: webroot  
**Тип**: *строка*  
**По умолчанию**: Если не указан, значение по умолчанию — "(Content Root)/wwwroot», если путь существует. Если путь не существует, то используется поставщик холостой файла.  
**Задать с помощью**:`UseWebRoot`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a>Переопределение конфигурации

Используйте [конфигурации](configuration.md) для настройки узла. В следующем примере конфигурации узла при необходимости указывается в *hosting.json* файла. Загрузить любую конфигурацию из *hosting.json* файл может быть переопределена аргументы командной строки. Конфигурации построения (в `config`) используется для настройки узла с `UseConfiguration`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

*Hosting.JSON*:

```json
{
    urls: "http://*:5005"
}
```

Переопределение конфигурации, обеспечиваемой `UseUrls` с *hosting.json* конфигурации первый аргументом командной строки конфигурации второй:

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

*Hosting.JSON*:

```json
{
    urls: "http://*:5005"
}
```

Переопределение конфигурации, обеспечиваемой `UseUrls` с *hosting.json* конфигурации первый аргументом командной строки конфигурации второй:

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
> `UseConfiguration` Метода расширения не может в настоящее время синтаксического анализа раздела конфигурации, возвращенных `GetSection` (например, `.UseConfiguration(Configuration.GetSection("section"))`. `GetSection` Метод фильтрует ключи в запрошенный раздел конфигурации, но оставляет имя раздела по ключам (например, `section:urls`, `section:environment`). `UseConfiguration` Метод ожидает ключи для сопоставления `WebHostBuilder` ключи (например, `urls`, `environment`). Имя раздела по ключам присутствия запрещает настраивать узла значения этого раздела. Эта проблема будет устранена в следующем выпуске. Дополнительные сведения и способы решения проблем см. в разделе [передачи раздела конфигурации в WebHostBuilder.UseConfiguration использует полное ключи](https://github.com/aspnet/Hosting/issues/839).

Для указания выполнения на определенный URL-адрес узла, можно передать в нужное значение из командной строки при выполнении `dotnet run`. Аргумент командной строки переопределяет `urls` значение из *hosting.json* файла, а сервер прослушивает порт 8080:

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a>Упорядочение важности

Некоторые из `WebHostBuilder` параметры сначала считываются из переменных среды, если задать. Эти переменные среды используйте формат `ASPNETCORE_{configurationKey}`. Чтобы задать URL-адреса, которые прослушивает сервер по умолчанию, задайте `ASPNETCORE_URLS`.

Эти значения переменной среды можно переопределить, указав конфигурацию (с помощью `UseConfiguration`) или явно задать значение (с помощью `UseSetting` или одного из методов явного расширения, такие как `UseUrls`). Узел использует какой бы параметр задает значение последнего. Если требуется программно присвоено одно значение по умолчанию URL-адрес, но позволить приложению переопределить с помощью конфигурации, можно использовать настройки командной строки после настройки URL-адреса. В разделе [переопределение конфигурации](#overriding-configuration).

## <a name="starting-the-host"></a>Запуск узла

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**Выполнить**

`Run` Метод запускает веб-приложения и блокирует вызывающий поток до завершения работы узла:

```csharp
host.Run();
```

**Start**

Узел без блокирования запускается путем вызова его `Start` метод:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Если передать список URL-адресов для `Start` метод, он прослушивает URL-адреса:

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

Можно инициализировать и начать новый узел с помощью предварительно настроенного значения по умолчанию `CreateDefaultBuilder` с помощью статических удобных метода. Эти методы требуют запуска серверу без вывода на консоль с [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) ожидания break (Ctrl-C или SIGINT или SIGTERM):

**Начала (приложение RequestDelegate)**

Начать с `RequestDelegate`:

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Сделать запрос в браузере для `http://localhost:5000` для получения ответа «Hello World!» `WaitForShutdown`блоки, пока не будет выполнена break (Ctrl-C или SIGINT или SIGTERM). Приложение отображается `Console.WriteLine` сообщение и ожидает keypress для выхода.

**Запуск (строковый URL-адрес, RequestDelegate приложения)**

Запуск с URL-адреса и `RequestDelegate`:

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Дает тот же результат, как **начала (приложение RequestDelegate)**, за исключением приложение реагирует на `http://localhost:8080`.

**Запуск (действие<IRouteBuilder> routeBuilder)**

Использовать экземпляр `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) для использования маршрутизации по промежуточного слоя:

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

В примере можно используйте следующие запросы браузера:

| Запрос                                    | Ответ                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | Привет, Мартин!                           |
| `http://localhost:5000/buenosdias/Catrina` | Буэнос dias Catrina!                    |
| `http://localhost:5000/throw/ooops!`       | Вызывает исключение со строкой «ooops!» |
| `http://localhost:5000/throw`              | Вызывает исключение со строкой «Uh ой!» |
| `http://localhost:5000/Sante/Kevin`        | Sante Kevin!                            |
| `http://localhost:5000`                    | Пример "Здравствуй,                             |

`WaitForShutdown`блоки, пока не будет выполнена break (Ctrl-C или SIGINT или SIGTERM). Приложение отображается `Console.WriteLine` сообщение и ожидает keypress для выхода.

**Запуск (URL-адрес, действие строки<IRouteBuilder> routeBuilder)**

Использовать URL-адреса и имени экземпляра `IRouteBuilder`:

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

Дает тот же результат, как **запуска (действие<IRouteBuilder> routeBuilder)**, за исключением приложение реагирует на `http://localhost:8080`.

**StartWith (действие<IApplicationBuilder> приложения)**

Укажите делегат для настройки `IApplicationBuilder`:

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

Сделать запрос в браузере для `http://localhost:5000` для получения ответа «Hello World!» `WaitForShutdown`блоки, пока не будет выполнена break (Ctrl-C или SIGINT или SIGTERM). Приложение отображается `Console.WriteLine` сообщение и ожидает keypress для выхода.

**StartWith (URL-адрес, действие строки<IApplicationBuilder> приложения)**

Укажите URL-адрес и делегат для настройки `IApplicationBuilder`:

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

Дает тот же результат, как **StartWith (действие<IApplicationBuilder> приложения)**, за исключением приложение реагирует на `http://localhost:8080`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**Выполнить**

`Run` Метод запускает веб-приложения и блокирует вызывающий поток до завершения работы узла:

```csharp
host.Run();
```

**Start**

Узел без блокирования запускается путем вызова его `Start` метод:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Если передать список URL-адресов для `Start` метод, он прослушивает URL-адреса:


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

[IHostingEnvironment интерфейс](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) предоставляет сведения о среде размещения веб-приложения. Можно использовать [внедрение конструктора](xref:fundamentals/dependency-injection) для получения `IHostingEnvironment` для использования его свойства и методы расширения:

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

Можно использовать [подход на основе соглашения о](xref:fundamentals/environments#startup-conventions) нужно настроить приложение во время запуска, в зависимости от среды. Кроме того, можно ввести `IHostingEnvironment` в `Startup` конструктора для использования в `ConfigureServices`:

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
> В дополнение к `IsDevelopment` метод расширения `IHostingEnvironment` предлагает `IsStaging`, `IsProduction`, и `IsEnvironment(string environmentName)` методы. В разделе [работа с несколькими средами](xref:fundamentals/environments) подробные сведения.

`IHostingEnvironment` Службы также могут быть добавлены непосредственно в `Configure` метода настройки конвейер обработки:

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

Можно ввести `IHostingEnvironment` в `Invoke` метод при создании пользовательских [по промежуточного слоя](xref:fundamentals/middleware#writing-middleware):

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

[IApplicationLifetime интерфейс](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) дает возможность выполнять действия, выполняемые после запуска и завершения работы. Три свойства для интерфейса являются токенов отмены, которые можно зарегистрировать `Action` методы для определения событий запуска и завершения работы. Имеется также `StopApplication` метод.

| Токен отмены    | Запустить &#8230; |
| --------------------- | --------------------- |
| `ApplicationStarted`  | Узел полностью запущен. |
| `ApplicationStopping` | Узел выполняет правильное завершение работы. По-прежнему может обрабатывать запросы. Завершение работы блокируется до завершения этого события. |
| `ApplicationStopped`  | Узел завершает нормальное завершение работы. Все запросы должны обрабатываться полностью. Завершение работы блокируется до завершения этого события. |

| Метод            | Действие                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | Завершение запросов текущего приложения. |

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

## <a name="troubleshooting-systemargumentexception"></a>Устранение неполадок System.ArgumentException

**Применяется к только базовые ASP.NET 2.0**

При создании узла, внедряя `IStartup` непосредственно в контейнер внедрения зависимостей, а не вызовом `UseStartup` или `Configure`, может возникнуть следующая ошибка: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.

Это происходит потому, что [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (текущая сборка) необходим для проверки на наличие `HostingStartupAttributes`. Если вы вручную ввести `IStartup` в контейнер внедрения зависимостей, добавьте следующий вызов для вашей `WebHostBuilder` с указанным именем сборки:

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

Кроме того, добавить фиктивное `Configure` для вашей `WebHostBuilder`, который устанавливает `applicationName`(`ApplicationKey`) автоматически:

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

**Примечание**: это необходимо только обязательные с выпуском ASP.NET Core 2.0 и только если вы не вызываете `UseStartup` или `Configure`.

Дополнительные сведения см. в разделе [объявления: Microsoft.Extensions.PlatformAbstractions был удален (комментарий)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) и [StartupInjection пример](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Публикация в Windows с помощью служб IIS](../publishing/iis.md)
* [Публикация в Linux с помощью Nginx](../publishing/linuxproduction.md)
* [Публикация в Linux с помощью Apache](../publishing/apache-proxy.md)
* [Узел в службе Windows](xref:hosting/windows-service)
