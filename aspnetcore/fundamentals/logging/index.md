---
title: Ведение журналов в ASP.NET Core
author: tdykstra
description: Сведения о платформе ведения журналов в ASP.NET Core. Ознакомьтесь со встроенными поставщиками ведения журналов и получите подробные сведения о распространенных сторонних поставщиках.
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/14/2019
uid: fundamentals/logging/index
---
# <a name="logging-in-aspnet-core"></a>Ведение журналов в ASP.NET Core

Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith) и [Том Дакстра](https://github.com/tdykstra) (Tom Dykstra)

ASP.NET Core поддерживает API ведения журналов, который работает с разными встроенными и сторонними поставщиками. В этой статье описано, как использовать в коде API ведения журналов, работающего со встроенными поставщиками.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="add-providers"></a>Добавление поставщиков

Поставщик ведения журналов отображает или сохраняет журналы. Например, поставщик Console отображает журналы на консоли, а поставщик Azure Application Insights может сохранить их в Azure Application Insights. Журналы могут отправляться в несколько назначений после добавления нескольких поставщиков.

::: moniker range=">= aspnetcore-2.0"

Для добавления поставщика вызовите метод расширения `Add{provider name}` поставщика в файле *Program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=17-19)]

Шаблон проекта по умолчанию вызывает метод расширения <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, который добавляет следующие поставщики ведения журналов:

* Консоль
* Отладка
* EventSource (начиная с версии ASP.NET Core 2.2)

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

Если вы используете `CreateDefaultBuilder`, поставщики по умолчанию можно заменить собственными поставщиками. Вызовите <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> и добавьте требуемые поставщики.

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Чтобы использовать поставщик, установите его пакет NuGet и вызовите метод расширения поставщика в экземпляре <xref:Microsoft.Extensions.Logging.ILoggerFactory>.

[!code-csharp[](index/samples/1.x/TodoApiSample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

[Внедрение зависимостей](xref:fundamentals/dependency-injection) (DI) ASP.NET Core предоставляет экземпляр `ILoggerFactory`. Методы расширения `AddConsole` и `AddDebug` определены в пакетах [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) и [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/). Каждый метод расширения вызывает метод `ILoggerFactory.AddProvider`, передавая экземпляр поставщика.

> [!NOTE]
> [Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) добавляет поставщиков ведения журнала в метод `Startup.Configure`. Чтобы получить выходные данные журнала из выполняемого ранее кода, добавьте поставщики ведения журналов в конструктор класса `Startup`.

::: moniker-end

См. сведения о [встроенных](#built-in-logging-providers) и [сторонних](#third-party-logging-providers) поставщиках ведения журналов.

## <a name="create-logs"></a>Создание журналов

Получите <xref:Microsoft.Extensions.Logging.ILogger`1> объект путем внедрения зависимостей.

::: moniker range=">= aspnetcore-2.0"

В следующем примере контроллера создаются журналы `Information` и `Warning`. *Категория* — это `TodoApiSample.Controllers.TodoController` (полное имя класса `TodoController` в примере приложения):

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=4,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

В следующем примере Razor Pages создаются журналы с `Information` в качестве *категории* и `TodoApiSample.Pages.AboutModel` в качестве *уровня*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3, 7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

В предыдущем примере создаются журналы с `Information` и `Warning` в качестве *уровня* и классом `TodoController` в качестве *категории*. 

::: moniker-end

*Уровень* ведения журналов определяет степень серьезности или важности записанного в журнал события. *Категория* ведения журналов представляет строку, которая связана с каждым журналом. Экземпляр `ILogger<T>` создает журналы с полным именем типа `T` в качестве категории. [Уровни](#log-level) и [категории](#log-category) рассматриваются подробнее далее в этой статье. 

::: moniker range=">= aspnetcore-2.0"

### <a name="create-logs-in-startup"></a>Создание журналов в классе Startup

Для записи журналов в классе `Startup` включите параметр `ILogger` в сигнатуру конструктора:

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,19,26)]

### <a name="create-logs-in-program"></a>Создание журналов в классе Program

Для записи журналов в классе `Program` получите экземпляр `ILogger` путем внедрения зависимостей:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a>Асинхронные методы ведения журналов не выполняются

Скорость ведения журналов не должна влиять на производительность выполнения асинхронного кода. Если хранилище данных, предназначенное для регистрации сообщений журнала, работает медленно, сначала записывайте эти сообщения в быстродействующее хранилище, а затем перемещайте их в медленное хранилище. Например, записывайте сообщения в очередь, которую обрабатывает другой процесс для долгосрочного сохранения в медленном хранилище.

## <a name="configuration"></a>Конфигурация

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

::: moniker range="< aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    }
  }
}
```

Ключи `LogLevel` представляют имена журналов. Ключ `Default` применяется к журналам, которые не указаны явно. Значение представляет [уровень ведения журнала](#log-level), который применен к указанному журналу.

::: moniker-end

Сведения о реализации поставщиков конфигураций см. в статье <xref:fundamentals/configuration/index>.

## <a name="sample-logging-output"></a>Пример выходных данных ведения журнала

В примере кода из предыдущего раздела журналы появляются в консоли после запуска приложения из командной строки. Ниже приведен пример выходных данных консоли:

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

Предыдущие журналы созданы путем отправки HTTP-запроса Get к примеру приложения по адресу `http://localhost:5000/api/todo/0`.

Ниже приведен пример этих журналов в том виде, в каком они отображаются в окне отладки при запуске примера приложения в Visual Studio.


```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

Журналы, которые созданы путем вызовов `ILogger`, как показано в предыдущем разделе, начинаются с TodoApi.Controllers.TodoController. Журналы, которые начинаются с категорий Microsoft, входят в код платформы ASP.NET Core. В ASP.NET Core и коде приложения используется один и тот же API ведения журналов и одни и те же поставщики.

В оставшейся части этой статьи рассматриваются некоторые сведения и параметры для ведения журналов.

## <a name="nuget-packages"></a>Пакеты NuGet

Интерфейсы `ILogger` и `ILoggerFactory` находятся в [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), а их реализации по умолчанию — в [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).

## <a name="log-category"></a>Категория журнала

При создании объекта `ILogger` для него указывается *категория*. Эта категория входит в состав каждого сообщения журнала, создаваемого этим экземпляром `Ilogger`. Категория может быть любой строкой, обычно используется имя класса, например TodoApi.Controllers.TodoController.

Используйте `ILogger<T>` для получения экземпляра `ILogger`, который использует полное имя типа `T` в качестве категории:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

Чтобы явно указать категорию, вызовите `ILoggerFactory.CreateLogger`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

Использование `ILogger<T>` эквивалентно вызову `CreateLogger` с полным именем типа `T`.

## <a name="log-level"></a>Уровень ведения журнала

Каждый журнал задает значение <xref:Microsoft.Extensions.Logging.LogLevel>. Уровень ведения журналов означает степень серьезности или важности. Например, можно создать журнал `Information` при нормальном завершении метода и журнал `Warning`, если метод возвращает код состояния *404 — не найдено*.

Следующий код создает журналы `Information`и `Warning`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

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

## <a name="log-event-id"></a>Идентификатор события журнала

Каждый журнал может указывать *идентификатор события*. В примере приложения для этого используется локально определенный класс `LoggingEvents`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

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

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

Параметры, используемые для предоставления значений, определяются порядком заполнителей, а не их именами. В приведенном ниже коде обратите внимание на то, что имена параметров идут не по порядку в шаблоне сообщения:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

Этот код создает сообщение журнала со значениями параметров в определенном порядке:

```
Parameter values: parm1, parm2
```

Платформа ведения журналов поддерживает такое поведение, чтобы поставщики ведения журнала могли реализовывать [семантическое ведение журналов, также известное как структурированное ведение журналов](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging). Сами аргументы передаются в систему ведения журналов, а не только в отформатированный шаблон сообщения. Эта информация позволяет поставщикам ведения журналов хранить значения параметров как поля. Предположим, например, что вызовы метода средства ведения журналов выглядят так:

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Если вы отправляете журналы в Хранилище таблиц Azure, каждая сущность таблицы Azure может иметь свойства `ID` и `RequestTime`, что упрощает выполнение запросов к данным журналов. Запрос может находить все журналы в пределах определенного диапазона `RequestTime`, не анализируя время ожидания текстового сообщения.

## <a name="logging-exceptions"></a>Ведение журнала исключений

Методы средства ведения журнала имеют перегрузки, которые позволяют передавать исключение, как показано в следующем примере:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

Разные поставщики обрабатывают сведения об исключениях по-разному. Ниже приведен пример выходных данных поставщика Debug из приведенного выше кода.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Фильтрация журналов

::: moniker range=">= aspnetcore-2.0"

Можно указать минимальный уровень ведения журнала для определенного поставщика и категории или для всех поставщиков или всех категорий. Все журналы с уровнем ниже минимального не передаются этому поставщику, поэтому они не отображаются и не сохраняются.

Чтобы проигнорировать все журналы, укажите `LogLevel.None` в качестве минимального уровня ведения журналов. `LogLevel.None` равно целочисленному значению 6, то есть больше, чем `LogLevel.Critical` (5).

### <a name="create-filter-rules-in-configuration"></a>Создание правил фильтрации в конфигурации

Код шаблона проектов вызывает `CreateDefaultBuilder` для настройки ведения журналов для поставщиков Console и Debug. Метод `CreateDefaultBuilder` также настраивает ведение журнала для просмотра конфигурации в разделе `Logging`, используя код, аналогичный следующему:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=16)]

Данные конфигурации указывают минимальные уровни ведения журнала для каждого поставщика и категории, как показано в следующем примере:

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

Этот JSON создает шесть правил фильтрации: одно для поставщика Debug, четыре для поставщика Console и одно для всех поставщиков. При создании объекта `ILogger` для каждого поставщика выбирается только одно из этих правил.

### <a name="filter-rules-in-code"></a>Правила фильтрации в коде

В следующем примере показано, как зарегистрировать в коде правила фильтрации:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

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
* EventLog
* AzureAppServices
* TraceSource
* EventSource

### <a name="default-minimum-level"></a>Минимальный уровень по умолчанию

Существует параметр минимального уровня, который применяется, только если к определенному поставщику или категории не подходят правила из конфигурации или кода. В следующем примере показано, как задать минимальный уровень:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

Если минимальный уровень не задан явным образом, используется значение по умолчанию — `Information`, означающее, что журналы `Trace` и `Debug` игнорируются.

### <a name="filter-functions"></a>Функции фильтрации

Функция фильтрации вызывается для всех поставщиков и категорий, которым в конфигурации и (или) коде не назначены правила. Код в функции имеет доступ к типу поставщика, категории и уровню ведения журналов. Например:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Некоторые поставщики ведения журналов позволяют указывать, когда журналы следует записывать на носитель, а когда — игнорировать. При этом учитывается уровень ведения журнала и категория.

Методы расширения `AddConsole` и `AddDebug`предоставляют перегрузки для принятия условий фильтрации. В следующем примере кода поставщик Console игнорирует журналы с уровнем ниже `Warning`, а поставщик Debug не учитывает журналы, создаваемые платформой.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

Метод `AddEventLog` имеет перегрузку, принимающую экземпляр `EventLogSettings`, который в своем свойстве `Filter` может содержать функцию фильтрации. Поставщик TraceSource не предоставляет эти перегрузки, так как уровень ведения журнала и другие параметры определяются для него на основе используемых `SourceSwitch` и `TraceListener`.

Чтобы задать правила фильтрации для всех поставщиков, которые зарегистрированы в экземпляре `ILoggerFactory`, используйте метод расширения `WithFilter`. В приведенном ниже примере журналы платформы (категория начинается с Microsoft или System) ограничиваются предупреждениями. При этом разрешается ведение журналов на уровне отладки для журналов, созданных кодом приложения.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Чтобы запретить запись журналов, укажите `LogLevel.None` как минимальный уровень ведения журналов. `LogLevel.None` равно целочисленному значению 6, то есть больше, чем `LogLevel.Critical` (5).

Пакет NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) предоставляет метод расширения `WithFilter`. Метод возвращает новый экземпляр `ILoggerFactory` для фильтрации журнала сообщений, переданного всем поставщикам средства ведения журналов, которые зарегистрированы в этом экземпляре. Он не влияет на другие экземпляры `ILoggerFactory`, включая исходный экземпляр `ILoggerFactory`.

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

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

Следующий код предоставляет области для поставщика Console:

::: moniker range="> aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Для включения ведения журнала на уровне области требуется параметр `IncludeScopes` средства ведения журналов.
>
> Сведения о настройках см. в разделе [Конфигурация](#configuration).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Для включения ведения журнала на уровне области требуется параметр `IncludeScopes` средства ведения журналов.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

*Startup.cs*:

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

Каждое сообщение журнала содержит ограниченную информацию:

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>Встроенные поставщики ведения журналов

В состав ASP.NET Core входят следующие поставщики:

* [Консоль](#console-provider)
* [Отладка](#debug-provider)
* [EventSource](#eventsource-provider)
* [EventLog](#windows-eventlog-provider)
* [TraceSource](#tracesource-provider)

Параметры для [ведения журналов в Azure](#logging-in-azure) рассматриваются далее в этой статье.

См. сведения о <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>ведении журналов stdout<xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.

### <a name="console-provider"></a>Поставщик Console

Пакет поставщика [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) отправляет выходные данные журнала на консоль. 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

[Перегрузки AddConsole](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) позволяют передавать минимальный уровень ведения журналов, функцию фильтрации и логическое значение, определяющее поддержку областей. Другим вариантом является передача объекта `IConfiguration`, который может указать поддержку областей и уровни ведения журнала.

Поставщик Console оказывает значительное влияние на производительность и потому, как правило, не подходит для использования в рабочей среде.

При создании проекта в Visual Studio метод `AddConsole` выглядит следующим образом:

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Этот код ссылается на раздел `Logging` файла *appSettings.json*:

[!code-json[](index/samples/1.x/TodoApiSample//appsettings.json)]

Приведенные параметры ограничивают журналы платформы предупреждениями, но разрешают регистрацию приложений на уровне отладки, как описано в разделе [Фильтрация журналов](#log-filtering). Дополнительные сведения см. в разделе [Конфигурация](xref:fundamentals/configuration/index).

::: moniker-end

Чтобы просмотреть выходные данные ведения журналов в консоли, откройте командную строку, перейдите в папку проекта и запустите приведенную ниже команду:

```console
dotnet run
```

### <a name="debug-provider"></a>Поставщик Debug

Пакет поставщика [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) записывает выходные данные журналов с помощью класса [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (вызовов метода `Debug.WriteLine`).

В Linux этот поставщик записывает журналы в каталог */var/log/message*.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

[Перегрузки AddDebug](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) позволяют передавать минимальный уровень ведения журнала или функцию фильтрации.

::: moniker-end

### <a name="eventsource-provider"></a>Поставщик EventSource

Для приложений, предназначенных для ASP.NET Core 1.1.0 или более поздней версии, реализовывать события трассировки можно с помощью пакета поставщика [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource). В Windows используется [трассировка событий Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803). Поставщик является кроссплатформенным, но для Linux и macOS инструменты сбора событий и отображений пока отсутствуют.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger();
```

::: moniker-end

Для сбора и просмотра данных журналов рекомендуется использовать [программу PerfView](https://github.com/Microsoft/perfview). Существуют и другие средства для просмотра журналов трассировки событий Windows, но PerfView обеспечивает максимальное удобство работы с событиями трассировки событий Windows, создаваемыми ASP.NET.

Чтобы настроить PerfView для сбора событий, регистрируемых этим поставщиком, добавьте строку `*Microsoft-Extensions-Logging` в список **Дополнительные поставщики**. (Не пропустите звездочку в начале строки.)

![Дополнительные поставщики Perfview](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a>Поставщик Windows EventLog

Пакет поставщика [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) отправляет выходные данные журнала в журнал событий Windows.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

[Перегрузки AddEventLog](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) позволяют передавать `EventLogSettings` или минимальный уровень ведения журнала.

::: moniker-end

### <a name="tracesource-provider"></a>Поставщик TraceSource

Пакет поставщика [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) использует библиотеки и поставщики <xref:System.Diagnostics.TraceSource>.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddTraceSource(sourceSwitchName);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

::: moniker-end

[Перегрузки AddTraceSource](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) позволяют передавать исходный параметр и прослушиватель трассировки.

Чтобы использовать этот поставщик, приложение должно выполняться в .NET Framework (а не в .NET Core). Поставщик позволяет перенаправлять сообщения различным [прослушивателям](/dotnet/framework/debug-trace-profile/trace-listeners), например <xref:System.Diagnostics.TextWriterTraceListener>, который используется в примере приложения.

::: moniker range="< aspnetcore-2.0"

В следующем примере показана настройка поставщика `TraceSource`, записывающего в окне консоли сообщения типа `Warning` и сообщения с более высоким уровнем серьезности.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

## <a name="logging-in-azure"></a>Ведение журналов в Azure

См. сведения о ведении журналов в Azure:

* [Поставщик Службы приложений Azure](#azure-app-service-provider)
* [Потоковая передача журналов Azure](#azure-log-streaming)

::: moniker range=">= aspnetcore-1.1"

* [Ведение журнала трассировки Azure Application Insights](#azure-application-insights-trace-logging)

::: moniker-end

### <a name="azure-app-service-provider"></a>Поставщик службы приложений Azure

Пакет поставщика [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) записывает журналы в текстовые файлы в файловой системе приложения службы приложений Azure и в [хранилище больших двоичных объектов](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) в учетной записи хранения Azure. Пакет поставщика доступен для приложений, предназначенных для .NET Core 1.1 или более поздней версии.

::: moniker range=">= aspnetcore-2.0"

Если планируется использовать .NET Core, обратите внимание на следующее.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* Пакет поставщиков включен в [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage) для ASP.NET Core.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* Этот пакет не входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Чтобы использовать поставщик, установите пакет.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

* Не вызывайте <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> явным образом. Поставщик становится автоматически доступным для приложения при развертывании приложения в службе приложений Azure.

Если планируется использовать .NET Framework или будет указана ссылка на метапакет `Microsoft.AspNetCore.App`, добавьте пакет поставщика в проект. Вызовите `AddAzureWebAppDiagnostics` в экземпляре <xref:Microsoft.Extensions.Logging.ILoggerFactory>:

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

Перегрузка <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> позволяет передавать <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>. Объект параметров может переопределять параметры по умолчанию, например шаблон выходных данных ведения журналов, имя BLOB-объекта и максимально допустимый размер файла. (*Шаблон выходных данных* — это шаблон сообщений, который применяется ко всем журналам наряду с тем, что предоставляется при вызове метода `ILogger`.)

При развертывании в приложение службы приложений ваше приложение учитывает параметры в разделе [Журналы диагностики](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) на странице **Служба приложений** на портале Azure. При обновлении этих параметров изменения вступают в силу немедленно без перезапуска или повторного развертывания приложения.

![Параметры ведения журнала Azure](index/_static/azure-logging-settings.png)

По умолчанию файлы журнала находятся в папке *D:\\home\\LogFiles\\Application*, а имя файла по умолчанию — *diagnostics-yyyymmdd.txt*. Максимальный размер файла по умолчанию составляет 10 МБ, а максимальное количество сохраняемых по умолчанию файлов равно 2. Имя BLOB-объекта по умолчанию — *{имя_приложения}{метка_времени}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*. См. сведения о поведении по умолчанию здесь: <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.

Поставщик работает только при выполнении проекта в среде Azure. Он не функционирует при запуске проекта в локальной среде &mdash;(то есть не выполняет запись в локальные файлы или в локальное хранилище разработки для больших двоичных объектов).

::: moniker-end

### <a name="azure-log-streaming"></a>Потоковая передача журналов Azure

Потоковая передача журналов Azure позволяет просматривать активность журнала в реальном времени из следующих источников:

* сервер приложений;
* веб-сервер;
* трассировка неудачно завершенных запросов.

Настройка потоковой передачи журналов Azure

* Со страницы портала приложения перейдите на страницу **Журналы диагностики**.
* **Включите** параметр **Ведение журнала приложения (файловая система)**.

![Страница журналов диагностики на портале Azure](index/_static/azure-diagnostic-logs.png)

Перейдите на страницу **Потоковая передача журналов**, чтобы просмотреть сообщения приложения. Они записываются в журнал приложением через интерфейс `ILogger`.

![Потоковая передача журнала приложения на портале Azure](index/_static/azure-log-streaming.png)

::: moniker range=">= aspnetcore-1.1"

### <a name="azure-application-insights-trace-logging"></a>Ведение журнала трассировки Azure Application Insights

Пакет SDK Application Insights может собирать и отправлять журналы, созданные инфраструктурой ведения журналов ASP.NET Core. Дополнительные сведения см. в следующих ресурсах:

* [Общие сведения об Application Insights](/azure/application-insights/app-insights-overview)
* [Application Insights для ASP.NET Core](/azure/application-insights/app-insights-asp-net-core)
* [Адаптеры ведения журналов в Application Insights](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md).

::: moniker-end

## <a name="third-party-logging-providers"></a>Сторонние поставщики ведения журналов

Некоторые сторонние платформы ведения журналов, которые работают с ASP.NET Core:

* [ELMAH.IO](https://elmah.io/) ([в репозитории GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging));
* [Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([в репозитории GitHub](https://github.com/mattwcole/gelf-extensions-logging))
* [JSNLog](http://jsnlog.com/) ([в репозитории GitHub](https://github.com/mperdeck/jsnlog));
* [KissLog.net](https://kisslog.net/) ([репозиторий GitHub](https://github.com/catalingavan/KissLog-net))
* [Loggr](http://loggr.net/) ([в репозитории GitHub](https://github.com/imobile3/Loggr.Extensions.Logging));
* [NLog](http://nlog-project.org/) ([в репозитории GitHub](https://github.com/NLog/NLog.Extensions.Logging));
* [Sentry](https://sentry.io/welcome/) ([репозиторий GitHub](https://github.com/getsentry/sentry-dotnet))
* [Serilog](https://serilog.net/) ([в репозитории GitHub](https://github.com/serilog/serilog-extensions-logging)).
* [Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([репозиторий Github](https://github.com/googleapis/google-cloud-dotnet))

Некоторые сторонние платформы выполняют [семантическое ведение журналов, также известное как структурированное ведение журналов](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Использование сторонней платформы аналогично использованию одного из встроенных поставщиков:

1. Добавьте пакет NuGet в проект.
1. Вызовите `ILoggerFactory`.

Дополнительные сведения см. в документации по каждому поставщику. Сторонние поставщики ведения журналов не поддерживаются корпорацией Майкрософт.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/logging/loggermessage>
