---
title: "Размещение в ASP.NET Core | Документы Microsoft"
author: ardalis
description: "Общие сведения о веб-узлы в ASP.NET Core."
keywords: "ASP.NET Core, веб-узел, IWebHost"
ms.author: riande
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 4e45311d-8d56-46e2-b99d-6f65b648a277
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0725f3d2c130068094792e5ba9e17481155e4294
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/25/2017
---
# <a name="introduction-to-hosting-in-aspnet-core"></a>Общие сведения о размещении в ASP.NET Core

По [Стив Смит](http://ardalis.com)

Для запуска приложения ASP.NET Core, необходимо настроить и запустить узла с помощью `WebHostBuilder`.

## <a name="what-is-a-host"></a>Что такое узел?

Приложения ASP.NET Core требуется *узла* в которой выполняется. Узел должен реализовать `IWebHost` интерфейс, который предоставляет доступ к коллекции компонентов и служб, и `Start` метод. Узел обычно создается с использованием экземпляра `WebHostBuilder`, который создает и возвращает `WebHost` экземпляра. `WebHost` Ссылается на сервер, который будет обрабатывать запросы. Дополнительные сведения о [серверов](servers/index.md).

### <a name="what-is-the-difference-between-a-host-and-a-server"></a>Какова разница между узлом и сервером?

Основное приложение отвечает за управление запуском и временем существования приложения. Сервер отвечает за принятие HTTP-запросов. Часть ответственности узла включает в том, что службы приложения и сервер доступен и правильно настроен. Можно рассматривать как оболочка сервера узла. Узел настроен на использование конкретного сервера; Поэтому серверу оно неизвестно его узла.

## <a name="setting-up-a-host"></a>Настройка узла

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Создание узла с помощью экземпляра `WebHostBuilder`. Обычно это делается в точке входа приложения: `public static void Main` (который в шаблонах проектов находится в *Program.cs* файла). Типичный *Program.cs*, показанный ниже, демонстрирует использование `WebHostBuilder` для создания узла.

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=14,15,16,17,18,19,20,21)]

`WebHostBuilder` Отвечает за создание узла, который будет начальной загрузки сервера для приложения. `WebHostBuilder`требуется указать сервер, который реализует `IServer` (`UseKestrel` в приведенном выше коде). `UseKestrel`Указывает, что сервер Kestrel будет использоваться приложением.

Сервер *содержимое корневого* определяет, где он выполняет поиск содержимого файлах, таких как представление MVC. Содержимое корневого по умолчанию — это папка, с которой выполняется приложение.

> [!NOTE]
> Указание `Directory.GetCurrentDirectory` как содержимое корневого использовать корневую папку веб-проект как корневой элемент содержимого приложения при запуске приложения из этой папки (например, вызов метода `dotnet run` из веб-папки проекта). Это значение по умолчанию, используемых в Visual Studio и `dotnet new` шаблонов.

Чтобы использовать в качестве обратного прокси-сервера IIS, вызовите `UseIISIntegration` как часть построения узла. 

Обратите внимание, что `UseIISIntegration` неправильно настраивает *сервера*, таких как `UseKestrel` does. Чтобы использовать IIS с ASP.NET Core, необходимо указать `UseKestrel` и `UseIISIntegration`. `UseKestrel`создает веб-сервер и размещено приложение. `UseIISIntegration`проверяет переменные среды, используемые IIS/IISExpress и настраивает параметры, такие как порт для прослушивания и заголовки для использования.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Создание узла с помощью экземпляра `WebHostBuilder`. Обычно это делается в точке входа приложения: `public static void Main` (который в шаблонах проектов находится в *Program.cs* файла). Типичный *Program.cs*описанных ниже вызовы `CreateDefaultbuilder` для создания узла:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

`CreateDefaultbuilder`Создает экземпляр `WebHostBuilder` для построения узел, который загружает сервера для приложения. Узлу требуется [сервера, который реализует IServer](servers/index.md). Встроенные серверы являются [Kestrel](servers/kestrel.md) и [HTTP.sys](servers/httpsys.md); `CreateDefaultbuilder` используют Kestrel по умолчанию.

`CreateDefaultbuilder`выполняет задачи настройки, помимо настройки Kestrel и веб-сервер:

* Задает содержимое корневого `Directory.GetCurrentDirectory`.
* Загружает конфигурацию из:
  * *appSettings.JSON*
  * *appSettings. \<EnvironmentName > .json*.
  * секретные данные пользователя при запуске приложения в среде разработки
  * переменные среды
  * args предоставленного командной строки
* Настраивает ведение журнала для выходных данных консоли и отладки с помощью фильтрации правила, указанные в разделе конфигурации ведения журнала.
* Обеспечивает интеграцию служб IIS.
* Добавление страницы исключения разработчик, при запуске приложения в среде разработки.

Сервер *содержимое корневого* определяет, где он выполняет поиск содержимого файлах, таких как представление MVC. Содержимое корневого по умолчанию — это папка, с которой выполняется приложение.

> [!NOTE]
> Указание `Directory.GetCurrentDirectory` как содержимое корневого использовать корневую папку веб-проект как корневой элемент содержимого приложения при запуске приложения из этой папки (например, вызов метода `dotnet run` из веб-папки проекта). Это значение по умолчанию, используемых в Visual Studio и `dotnet new` шаблонов.

При использовании служб IIS в качестве обратного прокси-сервера ASP.NET Core автоматически вызывает `UseIISIntegration` как часть построения узла. Дополнительные сведения см. в разделе [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

Обратите внимание, что `UseIISIntegration` неправильно настраивает *сервера*, таких как `UseKestrel` does. `UseKestrel`создает веб-сервер и размещено приложение. `UseIISIntegration`проверяет переменные среды, используемые IIS/IISExpress и настраивает параметры, такие как порт для прослушивания и заголовки для использования.

---

Минимальная реализация настройки узла (и приложения ASP.NET Core) будет включать только сервера и настройки конвейера запросов приложения:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
    })
    .Build();

host.Run();
```

> [!NOTE]
> При настройке узла, чтобы обеспечить `Configure` и `ConfigureServices` методов вместо или в дополнение к определению `Startup` класса (которого необходимо определить эти методы - в разделе [запуска приложения](startup.md)). Несколько вызовов `ConfigureServices` друг с другом, будут добавлены; вызовы `Configure` или `UseStartup` заменяет предыдущие параметры.

## <a name="configuring-a-host"></a>Настройка узла

`WebHostBuilder` Предоставляет методы для настройки большинства значений конфигурации, доступные для узла, который также можно задать непосредственно с помощью `UseSetting` и связанный ключ.

### <a name="host-configuration-values"></a>Значения конфигурации узла

**Запись ошибки при загрузке**`bool`

Ключ: `captureStartupErrors`. По умолчанию — `false`. Когда `false`, ошибки во время запуска результат в узле выхода. Когда `true`, узел соберет все исключения из `Startup` класса и попытки запуска сервера. Он будет отображаться страница ошибки (универсальный или подробные, на основе параметра подробных сообщений об ошибках, ниже) для каждого запроса. Задать с помощью `CaptureStartupErrors` метод.

Примечание: При запуске приложения с Kestrel и IIS по умолчанию выполняется для записи ошибок запуска. 

```csharp
new WebHostBuilder()
    .CaptureStartupErrors(true)
   ```

**Содержимое корневой**`string`

Ключ: `contentRoot`. Значения по умолчанию в папку, в котором находится сборка приложения (для Kestrel; Службы IIS будут использовать корневой папке веб-проекта по умолчанию). Этот параметр определяет, где ASP.NET Core начнется поиск файлов содержимого, такие как представления MVC. Также используется как базовый путь для [корневого веб-параметром](#web-root-setting). Задать с помощью `UseContentRoot` метод. Путь должен существовать, или узел не смогут запуститься.

```csharp
new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
   ```

**Подробных описаний ошибок**`bool`

Ключ: `detailedErrors`. По умолчанию — `false`. При `true` (или если среды имеет значение «Разработка»), приложение будет отображать сведения о запуска исключения, а не просто универсальная страница ошибки. Задать с помощью `UseSetting`.

```csharp
new WebHostBuilder()
    .UseSetting("detailedErrors", "true")
```

Если подробные сообщения об ошибках равно `false` и отслеживания ошибок запуска `true`, отображается страница универсальное сообщение об ошибке в ответ на каждый запрос к серверу.

![Универсальная страница ошибки](hosting/_static/generic-error-page.png)

Если подробные сообщения об ошибках равно `true` и отслеживания ошибок запуска `true`, отображается страница подробные сведения об ошибке в ответ на каждый запрос к серверу.

![Подробные сведения об ошибке страницы](hosting/_static/detailed-error-page.png)

**Среда**`string`

Ключ: `environment`. Значение по умолчанию «Production». Может быть присвоено любое значение. Определяемые платформой значения включают «Разработка», «Промежуточный» и «Production». Значения не учитывается регистр. В разделе [работа с несколькими средами](environments.md). Задать с помощью `UseEnvironment` метод.

```csharp
new WebHostBuilder()
    .UseEnvironment("Development")
```

> [!NOTE]
> По умолчанию среды считывается из `ASPNETCORE_ENVIRONMENT` переменной среды. При использовании Visual Studio, переменные среды может быть задан в *launchSettings.json* файла.

<a id="server-urls"></a>

**URL-адреса серверов**`string`

Ключ: `urls`. Задать как точку с запятой (;) запятыми список URL-адрес префиксов, для которого сервер должен отвечать. Например, `http://localhost:123`. Можно заменить имя домена и узла «\*» для указания сервер должен прослушивать запросы по любому IP-адресу или размещения, используя указанный порт и протокол (например, `http://*:5000` или `https://*:5001`). Протокол (`http://` или `https://`) должен быть включен, все URL-адреса. Префиксы интерпретируются настроенный сервер; форматы, поддерживаемые будут различаться для разных серверов.

```csharp
new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

В ASP.NET Core 2.0 Kestrel имеет собственную конфигурацию конечной точки API и не поддерживает `https://` в `urls` строку. Дополнительные сведения см. в разделе [введение в Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).

**При запуске сборки**`string`

Ключ: `startupAssembly`. Определяет сборку для поиска `Startup` класса. Задать с помощью `UseStartup` метод. Вместо этого ссылаться с помощью определенного типа `WebHostBuilder.UseStartup<StartupType>`. При наличии нескольких `UseStartup` методы вызываются, приоритет имеет последняя.

```csharp
new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

<a name=web-root-setting></a>

**Корневой веб-**`string`

Ключ: `webroot`. Если не указано значение по умолчанию — `(Content Root Path)\wwwroot`, если он существует. Если этот путь не существует, то используется поставщик холостой файла. Задать с помощью `UseWebRoot`.

```csharp
new WebHostBuilder()
    .UseWebRoot("public")
```

### <a name="overriding-configuration"></a>Переопределение конфигурации

Используйте [конфигурации](configuration.md) для задания значений конфигурации, используемые узлом. Эти значения могут переопределяться впоследствии. Этот параметр указан с помощью `UseConfiguration`.

```csharp
public static void Main(string[] args)
{
    var config = new ConfigurationBuilder()
        .AddJsonFile("hosting.json", optional: true)
        .AddCommandLine(args)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .Configure(app =>
        {
            app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
        })
        .Build();

    host.Run();
}
```

В приведенном выше примере аргументы командной строки может быть передан в настроить узел или параметры конфигурации при необходимости могут быть указаны в *hosting.json* файла. Для указания выполнения на определенный URL-адрес узла, можно передавать в нужное значение из командной строки:

```console
dotnet run --urls "http://*:5000"
```

`Run` Метод запускает веб-приложения и блокирует вызывающий поток до завершения работы узла.

```csharp
host.Run();
```

Узел без блокирования запускается путем вызова его `Start` метод:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Передайте список URL-адресов для `Start` метод и он будет прослушивать URL-адреса:

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

Форматы URL-адресов, которые являются допустимыми здесь зависят от сервера, которую вы используете. Дополнительные сведения см. в разделе [URL-адреса серверов](#server-urls) ранее в этой статье.

> [!NOTE]
> `UseConfiguration` Метода расширения не может в настоящее время синтаксического анализа раздела конфигурации, возвращенных `GetSection` (например, `.UseConfiguration(Configuration.GetSection("section"))`. `GetSection` Метод фильтрует ключи в запрошенный раздел конфигурации, но оставляет имя раздела по ключам (например, `section:urls`, `section:environment`). `UseConfiguration` Метод ожидает ключи для сопоставления `WebHostBuilder` ключи (например, `urls`, `environment`). Имя раздела по ключам присутствия запрещает настраивать узла значения этого раздела. Эта проблема будет устранена в следующем выпуске. Дополнительные сведения и способы решения проблем см. в разделе [передачи раздела конфигурации в WebHostBuilder.UseConfiguration использует полное ключи](https://github.com/aspnet/Hosting/issues/839).

### <a name="ordering-importance"></a>Упорядочение важности

`WebHostBuilder`Параметры сначала считываются из определенные переменные среды, если задать. Эти переменные среды следует использовать формат `ASPNETCORE_{configurationKey}`, поэтому, например задать URL-адреса, сервер будет прослушивать по умолчанию, задайте `ASPNETCORE_URLS`.

Эти значения переменной среды можно переопределить, указав конфигурацию (с помощью `UseConfiguration`) или явно задать значение (с помощью `UseUrls` для экземпляра). Узел использует какой бы параметр задает значение последнего. По этой причине `UseIISIntegration` должны располагаться после `UseUrls`, так как он заменяет URL-адрес с одним динамически, реализуемую IIS. Если требуется программно присвоено одно значение по умолчанию URL-адрес, однако возможность их переопределения с конфигурацией, можно настроить узел следующим образом:

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseUrls("http://*:1000") // default URL
    .UseConfiguration(config) // override from command line
    .UseKestrel()
    .Build();
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Публикация в Windows с помощью служб IIS](../publishing/iis.md)
* [Публикация в Linux с помощью Nginx](../publishing/linuxproduction.md)
* [Публикация в Linux с помощью Apache](../publishing/apache-proxy.md)
* [Узел в службе Windows](xref:hosting/windows-service)

