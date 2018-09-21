---
title: Ведение журналов в ASP.NET Core
author: ardalis
description: Сведения о платформе ведения журналов в ASP.NET Core. Ознакомьтесь со встроенными поставщиками ведения журналов и получите подробные сведения о распространенных сторонних поставщиках.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/19/2018
uid: fundamentals/logging/index
ms.openlocfilehash: e3bf5909edb515a2dabba206abaa160bc431d622
ms.sourcegitcommit: 8bf4dff3069e62972c1b0839a93fb444e502afe7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2018
ms.locfileid: "46483091"
---
# <a name="logging-in-aspnet-core"></a>Ведение журналов в ASP.NET Core

Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith) и [Том Дакстра](https://github.com/tdykstra) (Tom Dykstra)

ASP.NET Core поддерживает API ведения журнала, который работает с разными поставщиками. Встроенные поставщики позволяют отправлять журналы в одно или несколько мест назначений. Можно подключить стороннюю платформу ведения журналов. В этой статье содержатся сведения об использовании в коде встроенного API и поставщиков ведения журналов.

Сведения о журналах stdout при размещении в службах IIS см. здесь: <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>. Сведения о журналах stdout при размещении в Службе приложений Azure см. здесь: <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="how-to-create-logs"></a>Создание журналов

Для создания журналов реализуйте объект [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) из контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection).

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

Затем в этом объекте средства ведения журнала вызовите методы ведения журналов:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

В этом примере создаются журналы с классом `TodoController` в качестве *категории*. Категории рассматриваются [далее в этой статье](#log-category).

ASP.NET Core не поддерживает асинхронные методы ведения журналов. Здесь требуется настолько быстрая запись, что затраты на асинхронные операции совершенно нецелесообразны. Если возникла обратная ситуация, рекомендуется рассмотреть возможность изменения способа ведения журнала. Если хранилище данных работает медленно, сначала записывайте сообщения журнала в быстродействующее хранилище, а затем перемещайте их в медленное хранилище. Например, записывайте сообщения в очередь, которую обрабатывает другой процесс для долгосрочного сохранения в медленном хранилище.

## <a name="how-to-add-providers"></a>Добавление поставщиков

::: moniker range=">= aspnetcore-2.0"

Поставщик ведения журнала принимает сообщения, созданные с помощью объекта `ILogger`, и отображает или сохраняет их. Например, поставщик Console отображает сообщения на консоли, а поставщик службы приложений Azure может сохранить их в хранилище больших двоичных объектов.

Для использования поставщика вызовите метод расширения `Add<ProviderName>` поставщика в файле *Program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

Шаблон проекта по умолчанию активирует поставщики ведения журналов Console и Debug с помощью вызова метода расширения [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) в *Program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Поставщик ведения журнала принимает сообщения, созданные с помощью объекта `ILogger`, и отображает или сохраняет их. Например, поставщик Console отображает сообщения на консоли, а поставщик службы приложений Azure может сохранить их в хранилище больших двоичных объектов.

Чтобы использовать поставщик, установите его пакет NuGet и вызовите метод расширения поставщика в экземпляре [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), как показано в следующем примере.

[!code-csharp[](index/samples/1.x/TodoApiSample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

[Внедрение зависимостей](xref:fundamentals/dependency-injection) (DI) ASP.NET Core предоставляет экземпляр `ILoggerFactory`. Методы расширения `AddConsole` и `AddDebug` определены в пакетах [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) и [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/). Каждый метод расширения вызывает метод `ILoggerFactory.AddProvider`, передавая экземпляр поставщика.

> [!NOTE]
> [Пример приложения 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) добавляет поставщиков ведения журнала в метод `Startup.Configure`. Чтобы получить выходные данные журнала из выполняемого ранее кода, добавьте поставщики ведения журналов в конструктор класса `Startup`.

::: moniker-end

Дополнительные сведения о встроенных поставщиках ведения журналов см. [здесь](#built-in-logging-providers). Кроме того, далее в статье приведены ссылки на [сторонние поставщики](#third-party-logging-providers).

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

Ключи `LogLevel` представляют имена журналов. Ключ `Default` применяется к журналам, которые не указаны явно. Значение представляет [уровень ведения журнала](#log-level), который применен к указанному журналу. Ключи журнала, которые задают `IncludeScopes` (в примере — `Console`), следует указывать, если [области ведения журнала](#log-scopes) включены для указанного журнала.

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

Используя пример кода из предыдущего раздела, вы увидите журналы в консоли после запуска из командной строки. Ниже приведен пример выходных данных консоли:

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

Эти журналы были созданы при переходе по адресу `http://localhost:5000/api/todo/0`, который запускает выполнение обоих вызовов `ILogger`, приведенных в предыдущем разделе.

Ниже приведен пример тех же журналов в том виде, как они отображаются в окне отладки при запуске примера приложения в Visual Studio:

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

Журналы, которые были созданы путем вызовов `ILogger`, приведенных в предыдущем разделе, начинаются с "TodoApi.Controllers.TodoController". Журналы, которые начинаются с категорий "Microsoft", входят в ASP.NET Core. В ASP.NET Core и коде приложения используется один и тот же API ведения журнала и одни и те же поставщики.

В оставшейся части этой статьи рассматриваются некоторые сведения и параметры для ведения журналов.

## <a name="nuget-packages"></a>Пакеты NuGet

Интерфейсы `ILogger` и `ILoggerFactory` находятся в [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), а их реализации по умолчанию — в [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).

## <a name="log-category"></a>Категория журнала

*Категория* входит в состав каждого создаваемого журнала. Категория указывается при создании объекта `ILogger`. Категория может быть любой строкой, однако действует соглашение по использованию полного имени класса, из которого записываются журналы. Например: "TodoApi.Controllers.TodoController".

Можно указать категорию в виде строки или использовать метод расширения, который наследует категорию от типа. Чтобы указать категорию как строку, вызовите `CreateLogger` в экземпляре `ILoggerFactory`, как показано ниже.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

В большинстве случаев будет проще использовать `ILogger<T>`, как показано в следующем примере.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

Это эквивалентно вызову `CreateLogger` с полным именем типа `T`.

## <a name="log-level"></a>Уровень ведения журнала

Каждый раз при создании журнала указывается его значение перечисления [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel). Уровень ведения журнала означает степень серьезности или важности. Например, можно создать журнал `Information` при нормальном завершении метода, журнал `Warning`, если метод возвращает код 404, и журнал `Error` при перехвате непредвиденного исключения.

В следующем примере кода имена методов (например, `LogWarning`) указывают уровень ведения журнала. Первый параметр — это [ идентификатор события журнала](#log-event-id). Второй параметр — это [шаблон сообщения](#log-message-template) с заполнителями для значений аргументов, предоставляемых оставшимися параметрами метода. Параметры метода рассматриваются более подробно далее в этой статье.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

Методы ведения журнала, которые содержат уровень в имени метода, являются [методами расширения для ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions). На самом деле эти методы вызывают метод `Log`, принимающий параметр `LogLevel`. Метод `Log`, в отличие от этих методов расширения, можно вызывать напрямую, однако его синтаксис довольно сложен. Дополнительные сведения см. в разделе [Интерфейс ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) и [Исходный код расширений средства ведения журналов](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).

В ASP.NET Core определяются следующие [уровни ведения журнала](/dotnet/api/microsoft.extensions.logging.loglevel), упорядоченные в этом документе от наименьшего до наибольшего уровня серьезности.

* Трассировка = 0

  Для получения сведений, которые пригодятся только разработчикам при отладке. Эти сообщения могут содержать конфиденциальные данные приложения, поэтому их не следует включать в рабочей среде. *По умолчанию отключено.* Пример: `Credentials: {"User":"someuser", "Password":"P@ssword"}`

* Отладка = 1

  Для сведений, которые полезны в краткосрочной перспективе во время разработки и отладки. Пример: `Entering method Configure with flag set to true.` как правило, в рабочей среде журналы уровня `Debug` применяются только при устранении неполадок, так как они занимают большой объем.

* Информация = 2

  Для отслеживания общего хода выполнения приложения. Эти журналы обычно полезны в долгосрочной перспективе. Пример: `Request received for path /api/todo`

* Предупреждение = 3

  Для нестандартных или непредвиденных событий, возникающих при выполнении приложения. В их число могут входить ошибки или другие условия, которые не приводят к остановке приложения, но которые может потребоваться изучить. Обработанные исключения являются распространенным условием использования уровня ведения журнала `Warning`. Пример: `FileNotFoundException for file quotes.txt.`

* Ошибка = 4

  Для ошибок и исключений, которые не могут быть обработаны. Эти сообщения указывают на сбой текущего действия или операции (например, текущего HTTP-запроса), а не ошибку уровня приложения. Пример сообщения журнала: `Cannot insert record due to duplicate key violation.`

* Критический = 5

  Для сбоев, которые требуют немедленного внимания. Примеры: потеря данных, нехватка места на диске.

Уровень ведения журнала можно использовать для управления объемом выходных данных, записываемых на определенный носитель или в окно отображения. Например, в рабочей среде может потребоваться, чтобы все журналы уровня `Information` и ниже записывались в хранилище томов, а все журналы уровня `Warning` и выше записывались в хранилище значений. Во время разработки можно отправлять журналы `Warning` или более высоких уровней серьезности на консоль. Затем, если потребуется устранить неполадки, можно добавить уровень `Debug`. В разделе [Фильтрация журналов](#log-filtering) далее в этой статье приводятся сведения о том, как контролировать уровни журнала, обрабатываемые поставщиком.

Платформа ASP.NET Core создает журналы уровня `Debug` для событий платформы. В примерах журнала ранее в этой статье не указывались журналы с уровнем ниже `Information`, поэтому журналы уровня `Debug` не отображались. Ниже приведен пример журналов консоли при запуске примера приложения, настроенного для отображения журналов уровня `Debug` и выше для поставщика Console.

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

При каждом создании журнала можно указывать *идентификатор события*. В примере приложения для этого используется локально определенный класс `LoggingEvents`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

Идентификатор события — это целочисленное значение, используемое для связи набора зарегистрированных событий друг с другом. Например, журнал для добавления позиции в корзину покупок может иметь идентификатор события 1000, а журнал для завершения покупки может иметь идентификатор события 1001.

В зависимости от поставщика идентификатор события может быть сохранен в поле или включен в текст сообщения в выходных данных журнала. Поставщик Debug не отображает идентификаторы событий, но поставщик Console показывает их в квадратных скобках после категории:

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>Шаблон сообщения журнала

Каждый раз при записи сообщения журнала предоставляется шаблон сообщения. Шаблон сообщения может быть строкой либо содержать именованные заполнители, в которые помещаются значения аргументов. Шаблон не является строкой формата, а заполнители должны быть именованными, а не нумерованными.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

Параметры, используемые для предоставления значений, определяются порядком заполнителей, а не их именами. Предположим, что имеется следующий код:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

Полученное в результате сообщение журнала выглядит следующим образом:

```
Parameter values: parm1, parm2
```

Платформа ведения журнала поддерживает такое форматирование сообщений, чтобы поставщики ведения журналов могли реализовывать [семантическое ведение журналов, также известное как структурированное ведение журнала](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging). Так как в систему ведения журналов передаются сами аргументы, а не только отформатированный шаблон сообщения, поставщики ведения журналов могут сохранять значения параметров как поля (наряду с шаблоном сообщения). Если выходные данных журналов направляются в хранилище таблиц Azure, вызов метода средства ведения журнала выглядит следующим образом:

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Каждая сущность таблицы Azure может иметь свойства `ID` и `RequestTime`, что упрощает выполнение запросов к данным журнала. Можно находить все журналы в пределах определенного диапазона `RequestTime`, не анализируя время ожидания текстового сообщения.

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

Чтобы запретить все журналы, укажите `LogLevel.None` в качестве минимального уровня ведения журнала. `LogLevel.None` равно целочисленному значению 6, то есть больше, чем `LogLevel.Critical` (5).

### <a name="create-filter-rules-in-configuration"></a>Создание правил фильтрации в конфигурации

Шаблоны проектов создают код, который вызывает `CreateDefaultBuilder` для настройки ведения журналов для поставщиков Console и Debug. Метод `CreateDefaultBuilder` также настраивает ведение журнала для просмотра конфигурации в разделе `Logging`, используя код, аналогичный следующему:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=15)]

Данные конфигурации указывают минимальные уровни ведения журнала для каждого поставщика и категории, как показано в следующем примере:

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

Этот JSON создает шесть правил фильтрации: одно для поставщика Debug, четыре для поставщика Console и одно, которое применяется ко всем поставщикам. Далее вы увидите, как при создании объекта `ILogger` для каждого поставщика выбирается лишь одно из этих правил.

### <a name="filter-rules-in-code"></a>Правила фильтрации в коде

Можно зарегистрировать правила фильтрации в коде, как показано в следующем примере:

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

При создании объекта `ILogger`, используемого для создания журналов, объект `ILoggerFactory` выбирает одно правило для каждого поставщика, которое будет применено к этому средству ведения журнала. Все сообщения, записываемые с помощью этого объекта `ILogger`, фильтруются на основе выбранных правил. Самое подходящее правило для каждой пары поставщика и категории выбирается из списка доступных правил.

При создании `ILogger` для данной категории для каждого поставщика используется приведенный далее алгоритм:

* Выберите все правила, которые соответствуют поставщику или его псевдониму. Если ничего не найдено, выберите все правила с пустым поставщиком.
* В результатах предыдущего шага выберите правила с самым длинным соответствующим префиксом категории. Если ничего не найдено, выберите все правила, которые не указывают категорию.
* Если выбрано несколько правил, примите **последнее**.
* Если правила не выбраны, используйте `MinimumLevel`.

Предположим, что у вас есть указанный выше список правил и вы создаете объект `ILogger` для категории "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":

* К поставщику Debug применяются правила 1, 6 и 8. Правило 8 является наиболее подходящим, поэтому выбрано оно.
* К поставщику отладки применяются правила 3, 4, 5 и 6. Правило 3 является наиболее подходящим.

При создании журналов с помощью `ILogger` для категории "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" журналы уровня `Trace` и выше перейдут поставщику отладки, а журналы уровня `Debug` и выше перейдут поставщику консоли.

### <a name="provider-aliases"></a>Псевдонимы поставщиков

Имя типа можно использовать, чтобы указать поставщик в конфигурации, но каждый поставщик имеет более короткий и простой в использовании *псевдоним*. Для встроенных поставщиков используйте следующие псевдонимы:

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

В функции фильтрации можно написать код для применения правил фильтрации. Функция фильтрации вызывается для всех поставщиков и категорий, которым в конфигурации и (или) коде не назначены правила. Код в функции имеет доступ к типу поставщика, категории и уровню ведения журнала, чтобы определить необходимость регистрации сообщений. Пример:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Некоторые поставщики ведения журналов позволяют указывать, когда журналы следует записывать на носитель, а когда — игнорировать. При этом учитывается уровень ведения журнала и категория.

Методы расширения `AddConsole` и `AddDebug`предоставляют перегрузки для передачи условий фильтрации. В следующем примере кода поставщик Console игнорирует журналы с уровнем ниже `Warning`, а поставщик Debug не учитывает журналы, создаваемые платформой.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

Метод `AddEventLog` имеет перегрузку, принимающую экземпляр `EventLogSettings`, который в своем свойстве `Filter` может содержать функцию фильтрации. Поставщик TraceSource не предоставляет эти перегрузки, так как уровень ведения журнала и другие параметры определяются для него на основе используемых `SourceSwitch` и `TraceListener`.

Можно задать правила фильтрации для всех поставщиков, которые зарегистрированы в экземпляре `ILoggerFactory`, с помощью метода расширения `WithFilter`. В приведенном ниже примере журналы платформы (категория начинается с "Microsoft" или "System") ограничиваются предупреждениями, однако разрешаются журналы приложений на уровне отладки.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Чтобы использовать фильтрацию для предотвращения создания всех журналов для определенной категории, укажите `LogLevel.None` в качестве минимального уровня ведения журнала для этой категории. `LogLevel.None` равно целочисленному значению 6, то есть больше, чем `LogLevel.Critical` (5).

Пакет NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) предоставляет метод расширения `WithFilter`. Метод возвращает новый экземпляр `ILoggerFactory` для фильтрации журнала сообщений, переданного всем поставщикам средства ведения журналов, которые зарегистрированы в этом экземпляре. Он не влияет на другие экземпляры `ILoggerFactory`, включая исходный экземпляр `ILoggerFactory`.

::: moniker-end

## <a name="log-scopes"></a>Области журналов

Для присоединения одних и тех же данных к каждому журналу, созданному в рамках набора, можно сгруппировать набор логических операций в *область*. Например, может потребоваться, чтобы каждый журнал, созданный в ходе обработки транзакции, содержал идентификатор транзакции.

Область — это тип `IDisposable`, возвращаемый методом [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) и действующий до удаления. Чтобы использовать область, следует заключить вызовы средства ведения журналов в блок `using`, как показано ниже:

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
* [служба приложений Azure](#azure-app-service-provider);

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

[Перегрузки AddConsole](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) позволяют передавать минимальный уровень ведения журнала, функцию фильтрации и логическое значение, указывающее, поддерживаются ли области. Другим вариантом является передача объекта `IConfiguration`, который может указать поддержку областей и уровни ведения журнала.

Если вы собираетесь использовать поставщик Console в рабочей среде, имейте в виду, что он оказывает значительное влияние на производительность.

При создании проекта в Visual Studio метод `AddConsole` выглядит следующим образом:

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Этот код ссылается на раздел `Logging` файла *appSettings.json*:

[!code-json[](index/samples/1.x/TodoApiSample//appsettings.json)]

Приведенные параметры ограничивают журналы платформы предупреждениями, но разрешают регистрацию приложений на уровне отладки, как описано в разделе [Фильтрация журналов](#log-filtering). Дополнительные сведения см. в разделе [Конфигурация](xref:fundamentals/configuration/index).

::: moniker-end

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

[Перегрузки AddDebug](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) позволяют передавать минимальный уровень ведения журнала или функцию фильтрации.

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

[Перегрузки AddEventLog](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) позволяют передавать `EventLogSettings` или минимальный уровень ведения журнала.

::: moniker-end

### <a name="tracesource-provider"></a>Поставщик TraceSource

Пакет поставщика [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) использует библиотеки и поставщики [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource).

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

[Перегрузки AddTraceSource](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) позволяют передавать исходный параметр и прослушиватель трассировки.

Чтобы использовать этот поставщик, приложение должно выполняться в .NET Framework (а не в .NET Core). Поставщик позволяет перенаправлять сообщения различным [прослушивателям](/dotnet/framework/debug-trace-profile/trace-listeners), например [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr), который используется в примере приложения.

В следующем примере показана настройка поставщика `TraceSource`, записывающего в окне консоли сообщения типа `Warning` и сообщения с более высоким уровнем серьезности.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

### <a name="azure-app-service-provider"></a>Поставщик службы приложений Azure

Пакет поставщика [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) записывает журналы в текстовые файлы в файловой системе приложения службы приложений Azure и в [хранилище больших двоичных объектов](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) в учетной записи хранения Azure. Пакет поставщика доступен для приложений, предназначенных для .NET Core 1.1 или более поздней версии.

::: moniker range=">= aspnetcore-2.0"

Если планируется использовать .NET Core, обратите внимание на следующее.

* Пакет поставщика включен в [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ASP.NET Core, но не включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).
* Не выполняйте явный вызов [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics). Поставщик становится автоматически доступным для приложения при развертывании приложения в службе приложений Azure.

Если планируется использовать .NET Framework или будет указана ссылка на метапакет `Microsoft.AspNetCore.App`, добавьте пакет поставщика в проект. Вызовите `AddAzureWebAppDiagnostics` в экземпляре [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory).

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

Перегрузка [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) позволяет передавать [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) для переопределения параметров по умолчанию, таких как шаблон выходных данных ведения журнала, имя BLOB-объекта и предельный размер файла. (*Шаблон выходных данных* — это шаблон сообщений, который применяется ко всем журналам на основе того, который вы указали при вызове метода `ILogger`.)

При развертывании в приложение службы приложений приложение учитывает параметры в разделе [Журналы диагностики](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) на странице **Служба приложений** на портале Azure. При обновлении этих параметров изменения вступают в силу немедленно без перезапуска или повторного развертывания приложения.

![Параметры ведения журнала Azure](index/_static/azure-logging-settings.png)

По умолчанию файлы журнала находятся в папке *D:\\home\\LogFiles\\Application*, а имя файла по умолчанию — *diagnostics-yyyymmdd.txt*. Максимальный размер файла по умолчанию составляет 10 МБ, а максимальное количество сохраняемых по умолчанию файлов равно 2. Имя BLOB-объекта по умолчанию — *{имя_приложения}{метка_времени}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*. Дополнительные сведения о поведении по умолчанию см. в разделе [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).

Поставщик работает только при выполнении проекта в среде Azure. Он не функционирует при запуске проекта в локальной среде &mdash;(то есть не выполняет запись в локальные файлы или в локальное хранилище разработки для больших двоичных объектов).

## <a name="third-party-logging-providers"></a>Сторонние поставщики ведения журналов

Некоторые сторонние платформы ведения журналов, которые работают с ASP.NET Core:

* [ELMAH.IO](https://elmah.io/) ([в репозитории GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging));
* [Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([в репозитории GitHub](https://github.com/mattwcole/gelf-extensions-logging))
* [JSNLog](http://jsnlog.com/) ([в репозитории GitHub](https://github.com/mperdeck/jsnlog));
* [KissLog.net](https://kisslog.net/) ([репозиторий GitHub](https://github.com/catalingavan/KissLog-net))
* [Loggr](http://loggr.net/) ([в репозитории GitHub](https://github.com/imobile3/Loggr.Extensions.Logging));
* [NLog](http://nlog-project.org/) ([в репозитории GitHub](https://github.com/NLog/NLog.Extensions.Logging));
* [Serilog](https://serilog.net/) ([в репозитории GitHub](https://github.com/serilog/serilog-extensions-logging)).

Некоторые сторонние платформы выполняют [семантическое ведение журналов, также известное как структурированное ведение журналов](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Использование сторонней платформы аналогично использованию одного из встроенных поставщиков:

1. Добавьте пакет NuGet в проект.
1. Вызовите метод расширения в `ILoggerFactory`.

Дополнительные сведения см. в документации по каждой платформе.

## <a name="azure-log-streaming"></a>Потоковая передача журналов Azure

Потоковая передача журналов Azure позволяет просматривать активность журнала в режиме реального времени из следующих источников:

* сервер приложений;
* веб-сервер;
* трассировка неудачно завершенных запросов.

Настройка потоковой передачи журналов Azure

* Со страницы портала приложения перейдите на страницу **Журналы диагностики**.
* Включите параметр **Ведение журнала приложения (файловая система)**.

![Страница журналов диагностики на портале Azure](index/_static/azure-diagnostic-logs.png)

Перейдите на страницу **Потоковая передача журналов**, чтобы просмотреть сообщения приложения. Они передаются в приложении через интерфейс `ILogger`.

![Потоковая передача журнала приложения на портале Azure](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a>Ведение журнала трассировки Azure Application Insights

Пакет SDK [Application Insights](https://azure.microsoft.com/services/application-insights/) способен собирать телеметрию трассировки из журналов, сформированных инфраструктурой ведения журналов ASP.NET Core. Дополнительные сведения см. в разделе [Application Insights для ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core) и на [вики-сайте Microsoft/ApplicationInsights-aspnetcore: ведение журнала](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/logging/loggermessage>
