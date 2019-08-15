---
title: Ведение журнала в .NET Core и ASP.NET Core
author: tdykstra
description: Узнайте, как использовать платформу ведения журналов, предоставляемую пакетом NuGet Microsoft.Extensions.Logging.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/11/2019
uid: fundamentals/logging/index
ms.openlocfilehash: 4e2aa1e18c3e3119e22452d5ca9b838581efbfd8
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/13/2019
ms.locfileid: "68994109"
---
# <a name="logging-in-net-core-and-aspnet-core"></a>Ведение журнала в .NET Core и ASP.NET Core

Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra) и [Стив Смит](https://ardalis.com/) (Steve Smith)

.NET Core поддерживает API ведения журналов, который работает с разными встроенными и сторонними поставщиками. В этой статье описано, как использовать в коде API ведения журналов, работающего со встроенными поставщиками.

::: moniker range=">= aspnetcore-3.0"

Большинство примеров кода, приведенных в этой статье, взяты из приложений ASP.NET Core. Части этих фрагментов кода, относящиеся к ведению журнала, применимы ко всем приложениям .NET Core, использующим [универсальный узел](xref:fundamentals/host/generic-host). Сведения об использовании универсального узла в приложениях, не выполняющихся в веб-консоли, см. в разделе [Размещенные службы](xref:fundamentals/host/hosted-services).

Код ведения журнала для приложений без универсального узла отличается тем, как [добавляются поставщики](#add-providers) и [создаются средства ведения журнала](#create-logs). Примеры кода, не связанные с размещением, приведены в соответствующих разделах статьи.

::: moniker-end

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="add-providers"></a>Добавление поставщиков

Поставщик ведения журналов отображает или сохраняет журналы. Например, поставщик Console отображает журналы на консоли, а поставщик Azure Application Insights может сохранить их в Azure Application Insights. Журналы могут отправляться в несколько назначений после добавления нескольких поставщиков.

::: moniker range=">= aspnetcore-3.0"

Чтобы добавить поставщик в приложение, использующее универсальный узел, вызовите метод расширения `Add{provider name}` поставщика в *Program.cs*:

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AddProvider&highlight=6)]

В консольном приложении, не использующем узел, вызовите метод расширения `Add{provider name}` поставщика при создании `LoggerFactory`:

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=1,7)]

`LoggerFactory` и `AddConsole` требуют оператор `using` для `Microsoft.Extensions.Logging`.

Шаблоны проектов ASP.NET Core по умолчанию вызывают метод <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A>, который добавляет следующие поставщики ведения журналов:

* Консоль
* Отладка
* EventSource
* Журнал событий (только при запуске в Windows)

Поставщики по умолчанию можно заменить собственными поставщиками. Вызовите <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> и добавьте требуемые поставщики.

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AddProvider&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0 "

Для добавления поставщика вызовите метод расширения `Add{provider name}` поставщика в файле *Program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=18-20)]

В указанном выше коде необходимо указать ссылки на `Microsoft.Extensions.Logging` и `Microsoft.Extensions.Configuration`.

Шаблон проекта по умолчанию вызывает метод <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, который добавляет следующие поставщики ведения журналов:

* Консоль
* Отладка
* EventSource (начиная с версии ASP.NET Core 2.2)

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

Если вы используете `CreateDefaultBuilder`, поставщики по умолчанию можно заменить собственными поставщиками. Вызовите <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> и добавьте требуемые поставщики.

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

См. сведения о [встроенных](#built-in-logging-providers) и [сторонних](#third-party-logging-providers) поставщиках ведения журналов.

## <a name="create-logs"></a>Создание журналов

Чтобы создать журналы, используйте объект <xref:Microsoft.Extensions.Logging.ILogger%601>. В веб-приложении или размещенной службе получите `ILogger` посредством внедрения зависимостей (DI). В консольных приложениях без размещения используйте `LoggerFactory` для создания `ILogger`.

В приведенном ниже примере для ASP.NET Core создается средство ведения журнала с категорией `TodoApiSample.Pages.AboutModel`. *Категория* ведения журналов представляет строку, которая связана с каждым журналом. Экземпляр `ILogger<T>`, предоставляемый внедрением зависимостей, создает журналы с полным именем типа `T` в качестве категории. 

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

В приведенном ниже примере консольного приложения без размещения создается средство ведения журнала с категорией `LoggingConsoleApp.Program`.

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

::: moniker-end

В приведенных ниже примерах ASP.NET Core и консольного приложения средство ведения журнала используется для создания журналов с уровнем `Information`. *Уровень* ведения журналов определяет степень серьезности или важности записанного в журнал события. 

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=11)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

[Уровни](#log-level) и [категории](#log-category) рассматриваются подробнее далее в этой статье. 

::: moniker range=">= aspnetcore-3.0"

### <a name="create-logs-in-the-program-class"></a>Создание журналов в классе Program

Для записи журналов в классе `Program` приложения ASP.NET Core получите экземпляр `ILogger` путем внедрения зависимостей после создания узла:

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

### <a name="create-logs-in-the-startup-class"></a>Создание журналов в классе Startup

Для записи журналов в методе `Startup.Configure` приложения ASP.NET Core включите параметр `ILogger` в сигнатуру метода:

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup.cs?name=snippet_Configure&highlight=1,5)]

Запись журналов до завершения настройки контейнера внедрения зависимостей в методе `Startup.ConfigureServices` не поддерживается:

* Внедрение средства ведения журнала в конструктор `Startup` не поддерживается.
* Внедрение средства ведения журнала в сигнатуру метода `Startup.ConfigureServices` не поддерживается.

Причина этого ограничения заключается в том, что ведение журнала зависит от внедрения зависимостей и от конфигурации, которая, в свою очередь, зависит от внедрения зависимостей. Контейнер внедрения зависимостей не настраивается, пока не завершится выполнение `ConfigureServices`.

Внедрение средства ведения журнала в класс `Startup` посредством конструктора работает в более ранних версиях ASP.NET Core, так как для веб-узла создается отдельный контейнер внедрения зависимостей. Сведения о том, почему для универсального узла создается только один контейнер, см. в [объявлении о критическом изменении](https://github.com/aspnet/Announcements/issues/353).

Если необходимо настроить службу, которая зависит от `ILogger<T>`, это можно сделать с помощью внедрения конструктора или путем предоставления фабричного метода. Фабричный метод рекомендуется использовать, только если нет других вариантов. Например, предположим, что необходимо заполнить свойство службой из внедрения зависимостей:

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup.cs?name=snippet_ConfigureServices&highlight=6-10)]

Выделенный выше код — это функция `Func`, которая выполняется, когда контейнеру внедрения зависимостей впервые требуется создать экземпляр `MyService`. Таким образом можно обращаться к любым зарегистрированным службам.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="create-logs-in-startup"></a>Создание журналов в классе Startup

Для записи журналов в классе `Startup` включите параметр `ILogger` в сигнатуру конструктора:

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-the-program-class"></a>Создание журналов в классе Program

Для записи журналов в классе `Program` получите экземпляр `ILogger` путем внедрения зависимостей:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a>Асинхронные методы ведения журналов не выполняются

Скорость ведения журналов не должна влиять на производительность выполнения асинхронного кода. Если хранилище данных, предназначенное для регистрации сообщений журнала, работает медленно, сначала записывайте эти сообщения в быстродействующее хранилище, а затем перемещайте их в медленное хранилище. Например, если вы входите в SQL Server, вам не нужно делать это непосредственно в методе `Log`, так как методы `Log` являются синхронными. Вместо этого синхронно добавьте сообщения журнала в очередь в памяти, и фоновый рабочий поток извлечет сообщения из очереди для выполнения асинхронных операций передачи данных в SQL Server.

## <a name="configuration"></a>Параметр Configuration

Конфигурацию поставщика ведения журналов предоставляет как минимум один поставщик конфигураций:

* Форматы файлов (INI, JSON и XML).
* аргументы командной строки.
* Переменные среды.
* Объекты .NET в памяти.
* Незашифрованное хранилище [Secret Manager](xref:security/app-secrets) (Диспетчер секретов).
* Зашифрованное пользовательское хранилище, например [Azure Key Vault](xref:security/key-vault-configuration).
* Пользовательские поставщики (установленные или созданные).

Например, конфигурацию ведения журналов обычно предоставляет раздел `Logging` в файле параметров приложения. В следующем примере показано содержимое типичного файла *appsettings.Development.json*.

::: moniker range=">= aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    },
    "Console":
    {
      "IncludeScopes": true
    }
  }
}
```

Свойство `Logging` может иметь свойство `LogLevel` и свойства поставщика журналов (здесь — Console).

Свойство `LogLevel` в разделе `Logging` указывает минимальный [уровень](#log-level) журнала для выбранных категорий. В этом примере категории `System` и `Microsoft` записываются на уровне `Information`, а остальные — на уровне `Debug`.

Другие свойства в разделе `Logging` определяют поставщиков ведения журналов. Пример приведен для поставщика Console. Если поставщик поддерживает [области журналов](#log-scopes), `IncludeScopes` определяет, включены ли они. Свойство поставщика (например, `Console` в этом примере) также определять свойство `LogLevel`. `LogLevel` под поставщиком указывает уровни журнала для этого поставщика.

Если уровни указаны в `Logging.{providername}.LogLevel`, они не переопределяют ничего из того, что задано в `Logging.LogLevel`.

::: moniker-end

Сведения о реализации поставщиков конфигураций см. в статье <xref:fundamentals/configuration/index>.

## <a name="sample-logging-output"></a>Пример выходных данных ведения журнала

В примере кода из предыдущего раздела журналы появляются в консоли после запуска приложения из командной строки. Ниже приведен пример выходных данных консоли:

::: moniker range=">= aspnetcore-3.0"

```console
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 84.26180000000001ms 307
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 GET https://localhost:5001/api/todo/0
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
info: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[3]
      Route matched with {action = "GetById", controller = "Todo", page = ""}. Executing controller action with signature Microsoft.AspNetCore.Mvc.IActionResult GetById(System.String) on controller TodoApiSample.Controllers.TodoController (TodoApiSample).
info: TodoApiSample.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```

::: moniker-end

Предыдущие журналы созданы путем отправки HTTP-запроса Get к примеру приложения по адресу `http://localhost:5000/api/todo/0`.

Ниже приведен пример этих журналов в том виде, в каком они отображаются в окне отладки при запуске примера приложения в Visual Studio.

::: moniker range=">= aspnetcore-3.0"

```console
Microsoft.AspNetCore.Hosting.Diagnostics: Information: Request starting HTTP/2.0 GET https://localhost:44328/api/todo/0  
Microsoft.AspNetCore.Routing.EndpointMiddleware: Information: Executing endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker: Information: Route matched with {action = "GetById", controller = "Todo", page = ""}. Executing controller action with signature Microsoft.AspNetCore.Mvc.IActionResult GetById(System.String) on controller TodoApiSample.Controllers.TodoController (TodoApiSample).
TodoApiSample.Controllers.TodoController: Information: Getting item 0
TodoApiSample.Controllers.TodoController: Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult: Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker: Information: Executed action TodoApiSample.Controllers.TodoController.GetById (TodoApiSample) in 34.167ms
Microsoft.AspNetCore.Routing.EndpointMiddleware: Information: Executed endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
Microsoft.AspNetCore.Hosting.Diagnostics: Information: Request finished in 98.41300000000001ms 404
```

Журналы, которые созданы путем вызовов `ILogger`, как показано в предыдущем разделе, начинаются с TodoApiSample. Журналы, которые начинаются с категорий Microsoft, входят в код платформы ASP.NET Core. В ASP.NET Core и коде приложения используется один и тот же API ведения журналов и одни и те же поставщики.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

Журналы, которые созданы путем вызовов `ILogger`, как показано в предыдущем разделе, начинаются с TodoApi. Журналы, которые начинаются с категорий Microsoft, входят в код платформы ASP.NET Core. В ASP.NET Core и коде приложения используется один и тот же API ведения журналов и одни и те же поставщики.

::: moniker-end

В оставшейся части этой статьи рассматриваются некоторые сведения и параметры для ведения журналов.

## <a name="nuget-packages"></a>Пакеты NuGet

Интерфейсы `ILogger` и `ILoggerFactory` находятся в [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), а их реализации по умолчанию — в [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).

## <a name="log-category"></a>Категория журнала

При создании объекта `ILogger` для него указывается *категория*. Эта категория входит в состав каждого сообщения журнала, создаваемого этим экземпляром `ILogger`. Категория может быть любой строкой, обычно используется имя класса, например TodoApi.Controllers.TodoController.

Используйте `ILogger<T>` для получения экземпляра `ILogger`, который использует полное имя типа `T` в качестве категории:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

Чтобы явно указать категорию, вызовите `ILoggerFactory.CreateLogger`:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

Использование `ILogger<T>` эквивалентно вызову `CreateLogger` с полным именем типа `T`.

## <a name="log-level"></a>Уровень ведения журнала

Каждый журнал задает значение <xref:Microsoft.Extensions.Logging.LogLevel>. Уровень ведения журналов означает степень серьезности или важности. Например, можно создать журнал `Information` при нормальном завершении метода и журнал `Warning`, если метод возвращает код состояния *404 — не найдено*.

Следующий код создает журналы `Information`и `Warning`:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

В коде выше первый параметр — это [идентификатор события журнала](#log-event-id). Второй параметр — это шаблон сообщения с заполнителями для значений аргументов, предоставляемых оставшимися параметрами метода. Параметры метода рассматриваются более подробно в разделе, посвященном [шаблону сообщений](#log-message-template) далее в этой статье.

Методы журналов, которые содержат уровень в имени метода (например, `LogInformation` и `LogWarning`), являются [методами расширения для ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions). Эти методы вызывают метод `Log`, принимающий параметр `LogLevel`. Метод `Log`, в отличие от этих методов расширения, можно вызывать напрямую, однако его синтаксис довольно сложен. См. сведения о <xref:Microsoft.Extensions.Logging.ILogger> и [исходном коде расширений средства ведения журналов](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).

В ASP.NET Core определяются следующие уровни ведения журналов, упорядоченные по возрастанию уровней серьезности.

* Трассировка = 0

  Для получения сведений, которые полезны только при отладке. Эти сообщения могут содержать конфиденциальные данные приложения, поэтому их не следует включать в рабочей среде. *По умолчанию отключено.*

* Отладка = 1

  Для получения сведений, которые полезны при разработке и отладке. Пример `Entering method Configure with flag set to true.` Включайте уровни ведения журналов `Debug` в рабочей среде только при устранении неполадок, так как такие журналы занимают много места.

* Информация = 2

  Для отслеживания общего потока работы приложения. Эти журналы обычно полезны в долгосрочной перспективе. Пример: `Request received for path /api/todo`

* Предупреждение = 3

  Для нестандартных или непредвиденных событий, возникающих в потоке работы приложения. Это могут быть ошибки или другие условия, которые не приводят к остановке приложения, но которые нужно изучить. Обработанные исключения являются распространенным условием использования уровня ведения журнала `Warning`. Пример: `FileNotFoundException for file quotes.txt.`

* Ошибка = 4

  Для ошибок и исключений, которые не могут быть обработаны. Эти сообщения указывают на сбой текущего действия или операции (например, текущего HTTP-запроса), а не на ошибку уровня приложения. Пример сообщения журнала: `Cannot insert record due to duplicate key violation.`

* Критический = 5

  Для сбоев, которые требуют немедленного внимания. Примеры: потеря данных, нехватка места на диске.

Уровень ведения журналов можно использовать для управления объемом выходных данных, записываемых на определенный носитель или в окно отображения. Например:

* В рабочей среде отправьте `Trace` в контексте уровня `Information` в хранилище томов. Отправьте `Warning` в контексте уровня `Critical` в хранилище томов.
* При разработке отправьте `Warning` в контексте `Critical` в консоль и добавьте `Trace` в контексте `Information` при устранении неполадок.

В разделе [Фильтрация журналов](#log-filtering) далее в этой статье приводятся сведения о том, как контролировать уровни журнала, обрабатываемые поставщиком.

ASP.NET Core создает журналы для событий платформы. В примерах журнала ранее в этой статье не указывались журналы с уровнем ниже `Information`, поэтому журналы уровня `Debug` или `Trace` не создавались. Ниже приведен пример журналов консоли, которые созданы при запуске примера приложения, настроенного на отображение журналов `Debug`:

::: moniker range=">= aspnetcore-3.0"

```console
info: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[3]
      Route matched with {action = "GetById", controller = "Todo", page = ""}. Executing controller action with signature Microsoft.AspNetCore.Mvc.IActionResult GetById(System.String) on controller TodoApiSample.Controllers.TodoController (TodoApiSample).
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of authorization filters (in the following order): None
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of resource filters (in the following order): Microsoft.AspNetCore.Mvc.ViewFeatures.Filters.SaveTempDataFilter
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of action filters (in the following order): Microsoft.AspNetCore.Mvc.Filters.ControllerActionFilter (Order: -2147483648), Microsoft.AspNetCore.Mvc.ModelBinding.UnsupportedContentTypeFilter (Order: -3000)
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of exception filters (in the following order): None
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of result filters (in the following order): Microsoft.AspNetCore.Mvc.ViewFeatures.Filters.SaveTempDataFilter
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[22]
      Attempting to bind parameter 'id' of type 'System.String' ...
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.Binders.SimpleTypeModelBinder[44]
      Attempting to bind parameter 'id' of type 'System.String' using the name 'id' in request data ...
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.Binders.SimpleTypeModelBinder[45]
      Done attempting to bind parameter 'id' of type 'System.String'.
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[23]
      Done attempting to bind parameter 'id' of type 'System.String'.
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[26]
      Attempting to validate the bound parameter 'id' of type 'System.String' ...
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[27]
      Done attempting to validate the bound parameter 'id' of type 'System.String'.
info: TodoApiSample.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[2]
      Executed action TodoApiSample.Controllers.TodoController.GetById (TodoApiSample) in 32.690400000000004ms
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 176.9103ms 404
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

::: moniker-end

## <a name="log-event-id"></a>Идентификатор события журнала

Каждый журнал может указывать *идентификатор события*. В примере приложения для этого используется локально определенный класс `LoggingEvents`:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/3.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

Идентификатор события связывает набор событий. Например, все журналы, связанные с отображением списка элементов на странице, могут иметь значение 1001.

Поставщик ведения журналов может хранить идентификатор события в поле идентификатора, в сообщении журнала, или вообще не хранить. Поставщик Debug не отображает идентификаторы событий. Поставщик Console отображает идентификаторы событий в квадратных скобках после категории:

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>Шаблон сообщения журнала

Каждый журнал указывает шаблон сообщения. Шаблон сообщения может содержать заполнители, для которых предоставляются аргументы. Используйте для заполнителей имена, а не числа.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

Параметры, используемые для предоставления значений, определяются порядком заполнителей, а не их именами. В приведенном ниже коде обратите внимание на то, что имена параметров идут не по порядку в шаблоне сообщения:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

Этот код создает сообщение журнала со значениями параметров в определенном порядке:

```text
Parameter values: parm1, parm2
```

Платформа ведения журналов поддерживает такое поведение, чтобы поставщики ведения журнала могли реализовывать [семантическое ведение журналов, также известное как структурированное ведение журналов](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging). Сами аргументы передаются в систему ведения журналов, а не только в отформатированный шаблон сообщения. Эта информация позволяет поставщикам ведения журналов хранить значения параметров как поля. Предположим, например, что вызовы метода средства ведения журналов выглядят так:

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Если вы отправляете журналы в Хранилище таблиц Azure, каждая сущность таблицы Azure может иметь свойства `ID` и `RequestTime`, что упрощает выполнение запросов к данным журналов. Запрос может находить все журналы в пределах определенного диапазона `RequestTime`, не анализируя время ожидания текстового сообщения.

## <a name="logging-exceptions"></a>Ведение журнала исключений

Методы средства ведения журнала имеют перегрузки, которые позволяют передавать исключение, как показано в следующем примере:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

Разные поставщики обрабатывают сведения об исключениях по-разному. Ниже приведен пример выходных данных поставщика Debug из приведенного выше кода.

```text
TodoApiSample.Controllers.TodoController: Warning: GetById(55) NOT FOUND

System.Exception: Item not found exception.
   at TodoApiSample.Controllers.TodoController.GetById(String id) in C:\TodoApiSample\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Фильтрация журналов

Можно указать минимальный уровень ведения журнала для определенного поставщика и категории или для всех поставщиков или всех категорий. Все журналы с уровнем ниже минимального не передаются этому поставщику, поэтому они не отображаются и не сохраняются.

Чтобы проигнорировать все журналы, укажите `LogLevel.None` в качестве минимального уровня ведения журналов. `LogLevel.None` равно целочисленному значению 6, то есть больше, чем `LogLevel.Critical` (5).

### <a name="create-filter-rules-in-configuration"></a>Создание правил фильтрации в конфигурации

Код шаблона проектов вызывает `CreateDefaultBuilder` для настройки ведения журналов для поставщиков Console и Debug. Метод `CreateDefaultBuilder` настраивает ведение журнала для просмотра конфигурации в разделе `Logging`, как было описано [ранее в этой статье](#configuration).

Данные конфигурации указывают минимальные уровни ведения журнала для каждого поставщика и категории, как показано в следующем примере:

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/TodoApiSample/appsettings.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

::: moniker-end

Этот JSON создает шесть правил фильтрации: одно для поставщика Debug, четыре для поставщика Console и одно для всех поставщиков. При создании объекта `ILogger` для каждого поставщика выбирается только одно из этих правил.

### <a name="filter-rules-in-code"></a>Правила фильтрации в коде

В следующем примере показано, как зарегистрировать в коде правила фильтрации:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

::: moniker-end

Второй `AddFilter` указывает поставщика, Debug, используя имя его типа. Первый `AddFilter` применяется ко всем поставщикам, так как он не определяет тип поставщика.

### <a name="how-filtering-rules-are-applied"></a>Применение правил фильтрации

Данные конфигурации и код `AddFilter`, приведенный в предыдущих примерах, создают правила, показанные в следующей таблице. Первые шесть взяты из примера конфигурации, а последние два — из примера кода.

| Число | Поставщик      | Категории, которые начинаются с...          | Минимальный уровень ведения журнала |
| :----: | ------------- | --------------------------------------- | ----------------- |
| 1      | Отладка         | Все категории                          | Сведения       |
| 2      | Консоль       | Microsoft.AspNetCore.Mvc.Razor.Internal | Предупреждение           |
| 3      | Консоль       | Microsoft.AspNetCore.Mvc.Razor.Razor    | Отладка             |
| 4      | Консоль       | Microsoft.AspNetCore.Mvc.Razor          | Error             |
| 5      | Консоль       | Все категории                          | Сведения       |
| 6      | Все поставщики | Все категории                          | Отладка             |
| 7      | Все поставщики | Система                                  | Отладка             |
| 8      | Отладка         | Майкрософт                               | Трассировка             |

При создании объекта `ILogger` объект `ILoggerFactory` выбирает одно правило для каждого поставщика, которое будет применено к этому средству ведения журналов. Все сообщения, записываемые с помощью экземпляра `ILogger`, фильтруются на основе выбранных правил. Самое подходящее правило для каждой пары поставщика и категории выбирается из списка доступных правил.

При создании `ILogger` для данной категории для каждого поставщика используется приведенный далее алгоритм:

* Выберите все правила, которые соответствуют поставщику или его псевдониму. Если ничего не найдено, выберите все правила с пустым поставщиком.
* В результатах предыдущего шага выберите правила с самым длинным соответствующим префиксом категории. Если ничего не найдено, выберите все правила, которые не указывают категорию.
* Если выбрано несколько правил, примите **последнее**.
* Если правила не выбраны, используйте `MinimumLevel`.

Предположим, у вас есть указанный выше список правил и вы создаете объект `ILogger` для категории Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine:

* К поставщику Debug применяются правила 1, 6 и 8. Правило 8 является наиболее подходящим, поэтому выбрано оно.
* К поставщику отладки применяются правила 3, 4, 5 и 6. Правило 3 является наиболее подходящим.

Полученный в результате экземпляр `ILogger` отправляет журналы уровня `Trace` и выше в поставщик Debug. Журналы уровня `Debug` и выше отправляются в поставщик Console.

### <a name="provider-aliases"></a>Псевдонимы поставщиков

Каждый поставщик определяет *псевдоним*, используемый в конфигурации вместо полного имени типа.  Для встроенных поставщиков используйте следующие псевдонимы:

* Консоль
* Отладка
* EventSource
* EventLog
* TraceSource
* AzureAppServicesFile
* AzureAppServicesBlob
* ApplicationInsights

### <a name="default-minimum-level"></a>Минимальный уровень по умолчанию

Существует параметр минимального уровня, который применяется, только если к определенному поставщику или категории не подходят правила из конфигурации или кода. В следующем примере показано, как задать минимальный уровень:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

::: moniker-end

Если минимальный уровень не задан явным образом, используется значение по умолчанию — `Information`, означающее, что журналы `Trace` и `Debug` игнорируются.

### <a name="filter-functions"></a>Функции фильтрации

Функция фильтрации вызывается для всех поставщиков и категорий, которым в конфигурации и (или) коде не назначены правила. Код в функции имеет доступ к типу поставщика, категории и уровню ведения журналов. Например:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=3-11)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

## <a name="system-categories-and-levels"></a>Системные категории и уровни

Ниже приведены некоторые категории, используемые ASP.NET Core и Entity Framework Core с заметками о журналах:

| Категория                            | Примечания |
| ----------------------------------- | ----- |
| Microsoft.AspNetCore                | Общая диагностика ASP.NET Core. |
| Microsoft.AspNetCore.DataProtection | Распознанные, найденные и использованные ключи. |
| Microsoft.AspNetCore.HostFiltering  | Разрешенные узлы. |
| Microsoft.AspNetCore.Hosting        | Время начала и длительность выполнения HTTP-запросов. Определение загруженных начальных сборок размещения. |
| Microsoft.AspNetCore.Mvc            | Диагностика MVC и Razor. Привязка моделей, выполнение фильтра, компиляция представлений, выбор действия. |
| Microsoft.AspNetCore.Routing        | Перенаправление соответствующих сведений. |
| Microsoft.AspNetCore.Server         | Запуск, остановка и сохранение ответов. Сведения о сертификате HTTPS. |
| Microsoft.AspNetCore.StaticFiles    | Обработанные файлы. |
| Microsoft.EntityFrameworkCore       | Общая диагностика Entity Framework Core. Действия и конфигурация базы данных, обнаружение изменений, переносы. |

## <a name="log-scopes"></a>Области журналов

 *Область* может группировать набор логических операций. Эту возможность можно использовать для присоединения одних и тех же данных к каждому журналу, созданному как часть набора. Например, каждый журнал, созданный в ходе обработки транзакции, может включать идентификатор транзакции.

Область — это тип `IDisposable`, возвращаемый методом <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> и действующий до удаления. Используйте область, заключив вызовы средства ведения журналов в блок `using`:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

::: moniker-end

Следующий код предоставляет области для поставщика Console:

*Program.cs*:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

::: moniker-end

> [!NOTE]
> Для включения ведения журнала на уровне области требуется параметр `IncludeScopes` средства ведения журналов.
>
> Сведения о настройках см. в разделе [Конфигурация](#configuration).

Каждое сообщение журнала содержит ограниченную информацию:

```
info: TodoApiSample.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>Встроенные поставщики ведения журналов

В состав ASP.NET Core входят следующие поставщики:

* [Консоль](#console-provider)
* [Отладка](#debug-provider)
* [EventSource](#eventsource-provider)
* [EventLog](#windows-eventlog-provider)
* [TraceSource](#tracesource-provider)
* [AzureAppServicesFile](#azure-app-service-provider)
* [AzureAppServicesBlob](#azure-app-service-provider)
* [ApplicationInsights](#azure-application-insights-trace-logging)

Сведения о stdout и ведении журнала отладки с помощью модуля ASP.NET Core см. в статьях <xref:test/troubleshoot-azure-iis> и <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.

### <a name="console-provider"></a>Поставщик Console

Пакет поставщика [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) отправляет выходные данные журнала на консоль. 

```csharp
logging.AddConsole();
```

Чтобы просмотреть выходные данные ведения журналов в консоли, откройте командную строку, перейдите в папку проекта и запустите приведенную ниже команду:

```console
dotnet run
```

### <a name="debug-provider"></a>Поставщик Debug

Пакет поставщика [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) записывает выходные данные журналов с помощью класса [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (вызовов метода `Debug.WriteLine`).

В Linux этот поставщик записывает журналы в каталог */var/log/message*.

```csharp
logging.AddDebug();
```

### <a name="eventsource-provider"></a>Поставщик EventSource

Для приложений, предназначенных для ASP.NET Core 1.1.0 или более поздней версии, реализовывать события трассировки можно с помощью пакета поставщика [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource). В Windows используется [трассировка событий Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803). Поставщик является кроссплатформенным, но для Linux и macOS инструменты сбора событий и отображений пока отсутствуют.

```csharp
logging.AddEventSourceLogger();
```

Для сбора и просмотра данных журналов рекомендуется использовать [программу PerfView](https://github.com/Microsoft/perfview). Существуют и другие средства для просмотра журналов трассировки событий Windows, но PerfView обеспечивает максимальное удобство работы с событиями трассировки событий Windows, создаваемыми ASP.NET Core.

Чтобы настроить PerfView для сбора событий, регистрируемых этим поставщиком, добавьте строку `*Microsoft-Extensions-Logging` в список **Дополнительные поставщики**. (Не пропустите звездочку в начале строки.)

![Дополнительные поставщики Perfview](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a>Поставщик Windows EventLog

Пакет поставщика [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) отправляет выходные данные журнала в журнал событий Windows.

```csharp
logging.AddEventLog();
```

[Перегрузки AddEventLog](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) позволяют передавать <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>.

### <a name="tracesource-provider"></a>Поставщик TraceSource

Пакет поставщика [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) использует библиотеки и поставщики <xref:System.Diagnostics.TraceSource>.

```csharp
logging.AddTraceSource(sourceSwitchName);
```

[Перегрузки AddTraceSource](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) позволяют передавать исходный параметр и прослушиватель трассировки.

Чтобы использовать этот поставщик, приложение должно выполняться в .NET Framework (а не в .NET Core). Поставщик позволяет перенаправлять сообщения различным [прослушивателям](/dotnet/framework/debug-trace-profile/trace-listeners), например <xref:System.Diagnostics.TextWriterTraceListener>, который используется в примере приложения.

### <a name="azure-app-service-provider"></a>Поставщик службы приложений Azure

Пакет поставщика [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) записывает журналы в текстовые файлы в файловой системе приложения службы приложений Azure и в [хранилище больших двоичных объектов](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) в учетной записи хранения Azure.

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker range=">= aspnetcore-3.0"

Пакет поставщика не включен в общую платформу. Чтобы использовать поставщик, добавьте пакет поставщика в проект.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Этот пакет не входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Если планируется использовать .NET Framework или будет указана ссылка на метапакет `Microsoft.AspNetCore.App`, добавьте пакет поставщика в проект. 

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Для настройки параметров поставщика используйте <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> и <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, как показано в следующем примере:

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=17-28)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Для настройки параметров поставщика используйте <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> и <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, как показано в следующем примере:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

Перегрузка <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> позволяет передавать <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>. Объект параметров может переопределять параметры по умолчанию, например шаблон выходных данных ведения журналов, имя BLOB-объекта и максимально допустимый размер файла. (*Шаблон выходных данных* — это шаблон сообщений, который применяется ко всем журналам наряду с тем, что предоставляется при вызове метода `ILogger`.)

::: moniker-end

При развертывании в приложение Службы приложений ваше приложение использует параметры в разделе [Журналы Службы приложений](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) на странице **Служба приложений** на портале Azure. При обновлении следующих параметров изменения вступают в силу немедленно без перезапуска или повторного развертывания приложения:

* **Ведение журнала приложения (файловая система)** ;
* **Ведение журнала приложения (BLOB-объект)** .

По умолчанию файлы журнала находятся в папке *D:\\home\\LogFiles\\Application*, а имя файла по умолчанию — *diagnostics-yyyymmdd.txt*. Максимальный размер файла по умолчанию составляет 10 МБ, а максимальное количество сохраняемых по умолчанию файлов равно 2. Имя BLOB-объекта по умолчанию — *{имя_приложения}{метка_времени}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.

Поставщик работает только при выполнении проекта в среде Azure. Он не функционирует при запуске проекта в локальной среде &mdash;(то есть не выполняет запись в локальные файлы или в локальное хранилище разработки для больших двоичных объектов).

#### <a name="azure-log-streaming"></a>Потоковая передача журналов Azure

Потоковая передача журналов Azure позволяет просматривать активность журнала в реальном времени из следующих источников:

* сервер приложений;
* веб-сервер;
* трассировка неудачно завершенных запросов.

Настройка потоковой передачи журналов Azure

* Со страницы портала приложения перейдите на страницу **Журналы Службы приложений**.
* **Включите** параметр **Ведение журнала приложения (файловая система)** .
* Выберите **уровень** ведения журнала.

Перейдите на страницу **Поток журналов**, чтобы просмотреть сообщения приложения. Они записываются в журнал приложением через интерфейс `ILogger`.

### <a name="azure-application-insights-trace-logging"></a>Ведение журнала трассировки Azure Application Insights

Пакет поставщика [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) записывает журналы в Azure Application Insights. Служба Application Insights отслеживает веб-приложения и предоставляет средства для создания запросов и анализа данных телеметрии. Используя этого поставщика, вы сможете выполнять запросы к журналам и их анализ с помощью средств Application Insights.

Поставщик ведения журнала включается как зависимость [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore). Этот пакет предоставляет всю доступную телеметрию для ASP.NET Core. Если вы используете этот пакет, пакет поставщика устанавливать не нужно.

Не используйте пакет [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web), который предназначен для ASP.NET 4.x.

Дополнительные сведения см. в следующих ресурсах:

* [Общие сведения об Application Insights](/azure/application-insights/app-insights-overview)
* [Application Insights для приложений ASP.NET Core](/azure/azure-monitor/app/asp-net-core) — начните изучение с этой статьи, если вы хотите полностью реализовать возможности Application Insights для телеметрии и ведения журналов.
* [ApplicationInsightsLoggerProvider for .NET Core ILogger logs](/azure/azure-monitor/app/ilogger) (Поставщик ведения журналов Application Insights для журналов .NET Core ILogger) — начните изучение с этой статьи, если вы хотите внедрить поставщика ведения журналов, не используя остальные возможности Application Insights для телеметрии.
* [Адаптеры ведения журналов в Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).
* [Инструментирование серверного кода веб-приложения с помощью Application Insights](/learn/modules/instrument-web-app-code-with-application-insights) — интерактивный учебник на сайте Microsoft Learn.

## <a name="third-party-logging-providers"></a>Сторонние поставщики ведения журналов

Некоторые сторонние платформы ведения журналов, которые работают с ASP.NET Core:

* [ELMAH.IO](https://elmah.io/) ([в репозитории GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging));
* [Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([в репозитории GitHub](https://github.com/mattwcole/gelf-extensions-logging))
* [JSNLog](https://jsnlog.com/) ([в репозитории GitHub](https://github.com/mperdeck/jsnlog));
* [KissLog.net](https://kisslog.net/) ([репозиторий GitHub](https://github.com/catalingavan/KissLog-net))
* [Loggr](https://loggr.net/) ([в репозитории GitHub](https://github.com/imobile3/Loggr.Extensions.Logging));
* [NLog](https://nlog-project.org/) ([в репозитории GitHub](https://github.com/NLog/NLog.Extensions.Logging));
* [Sentry](https://sentry.io/welcome/) ([репозиторий GitHub](https://github.com/getsentry/sentry-dotnet))
* [Serilog](https://serilog.net/) ([в репозитории GitHub](https://github.com/serilog/serilog-aspnetcore)).
* [Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([репозиторий Github](https://github.com/googleapis/google-cloud-dotnet))

Некоторые сторонние платформы выполняют [семантическое ведение журналов, также известное как структурированное ведение журналов](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Использование сторонней платформы аналогично использованию одного из встроенных поставщиков:

1. Добавьте пакет NuGet в проект.
1. Вызовите `ILoggerFactory`.

Дополнительные сведения см. в документации по каждому поставщику. Сторонние поставщики ведения журналов не поддерживаются корпорацией Майкрософт.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/logging/loggermessage>
