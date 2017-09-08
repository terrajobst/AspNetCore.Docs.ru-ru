---
title: "Ведение журнала в ASP.NET Core"
author: ardalis
description: "Представляет платформу ведения журналов в ASP.NET Core. Содержит раздел для каждого поставщика встроенного ведения журнала, а также ссылки на некоторые популярные сторонних поставщиков."
keywords: "ASP.NET Core, ведение журнала, регистраторов, Microsoft.Extensions.Logging, ILogger, ILoggerFactory, уровень журнала, WithFilter, TraceSource, журнал событий, EventSource, область действия"
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ac27ac68-d76a-4f8e-b8ab-ea045803e5f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 15abe93d881aed3b6950a859dc9445ec50ee9bb5
ms.sourcegitcommit: 5355c96a1768e5a1d5698a98c190e7addcc4ded5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/05/2017
---
# <a name="introduction-to-logging-in-aspnet-core"></a>Общие сведения о входе ASP.NET Core

По [Стив Смит](http://ardalis.com) и [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core поддерживает API ведения журнала, который работает с множеством регистраторов. Встроенные поставщики позволяют отправлять журналы одному или нескольким назначениям, а можно подключить платформа ведения журналов сторонних разработчиков. В этой статье показано, как использовать API встроенного ведения журнала и поставщики в коде.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample2)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample)

---

## <a name="how-to-create-logs"></a>Создание журналов

Чтобы создать журналы, получить `ILogger` объекта из [внедрения зависимостей](dependency-injection.md) контейнера:

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Затем вызовите методы ведения журнала на этот объект средства ведения журнала:

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

В этом примере создается журналы с `TodoController` классов как *категории*.  Описываются категории [далее в этой статье](#log-category).

ASP.NET Core не поддерживает асинхронные методы ведения журнала так как ведение журнала должна быть настолько быстро, что это не стоит затрат на использование асинхронного. Если вы в ситуации, когда, не имеет значение true, рассмотрите возможность изменения способа входа.  Если хранилище данных выполняется медленно, сначала запись сообщений журнала быстрого хранилища, а затем переместите их в медленных хранилище позже. Например войдите в очередь сообщений, считываемых и сохраняются в медленных хранилище другим процессом.

## <a name="how-to-add-providers"></a>Добавление поставщиков

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Регистратор принимает сообщения, созданные с помощью `ILogger` и отображает или сохраняет их. Например поставщик консоли отображает сообщения на консоль и поставщика службы приложений Azure можно хранить их в хранилище больших двоичных объектов.

Для использования поставщика, вызывающих поставщик `Add<ProviderName>` метод расширения в *Program.cs*:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

Шаблон проекта по умолчанию настраивает ведение журнала способом, вы видите в приведенном выше коде, но `ConfigureLogging` осуществляется вызов `CreateDefaultBuilder` метод. Ниже приведен код *Program.cs* , создаваемый с помощью шаблонов проекта:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Регистратор принимает сообщения, созданные с помощью `ILogger` и отображает или сохраняет их. Например поставщик консоли отображает сообщения на консоль и поставщика службы приложений Azure можно хранить их в хранилище больших двоичных объектов.

Для использования поставщика, установите его пакет NuGet и вызов метода расширения поставщика экземпляра `ILoggerFactory`, как показано в следующем примере.

[!code-csharp[](logging/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

ASP.NET Core [внедрения зависимостей](dependency-injection.md) (DI) предоставляет `ILoggerFactory` экземпляра. `AddConsole` И `AddDebug` методы расширения определяются в [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) и [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) пакетов. Вызывает каждый метод расширения `ILoggerFactory.AddProvider` метод, передавая экземпляр поставщика. 

> [!NOTE]
> Образец приложения для данной статьи добавляет регистраторов в `Configure` метод `Startup` класса. Если вы хотите получить выходные данные журнала из кода, который выполняется в более ранних версий, добавьте регистраторов в `Startup` конструктора класса. 

---

Вы найдете сведения о каждом [встроенные регистратора](#built-in-logging-providers) и ссылки на [сторонних регистраторов](#third-party-logging-providers) далее в статье.

## <a name="sample-logging-output"></a>Пример выходных данных ведения журнала

Пример кода, показанный в предыдущем разделе вы увидите журналы в консоли при запуске из командной строки. Ниже приведен пример выходных данных консоли:

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
 
Были созданы эти журналы, последовательно выбрав пункты `http://localhost:5000/api/todo/0`, которое запускает выполнение обоих `ILogger` вызовы, показанный в предыдущем разделе.

Ниже приведен пример того же журналов, как они отображаются в окне отладки при запуске примера приложения в Visual Studio:

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

Журналы, которые были созданы путем `ILogger` вызовы, показанный в предыдущем разделе начинаются с «TodoApi.Controllers.TodoController». Журналы, которые начинаются с «Microsoft» категорий: от ASP.NET Core. ASP.NET Core сам и код приложения используют один API ведения журнала и те же поставщики ведения журнала.

В оставшейся части этой статьи объясняется некоторые сведения и параметры для ведения журнала.

## <a name="nuget-packages"></a>Пакеты NuGet

`ILogger` И `ILoggerFactory` интерфейсы являются в [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), и в реализации по умолчанию для них [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).

## <a name="log-category"></a>Категории журналов

Объект *категории* входит в состав каждого журнала, вы создаете.  Указать категорию при создании `ILogger` объекта. Категория может быть любой строкой, но соглашение заключается в использовании полное имя класса, из которого записываются журналы.  Например: «TodoApi.Controllers.TodoController».

Можно указать категорию, в виде строки или использовать метод расширения, который наследуется от типа категории. Чтобы указать категории, как строку, вызовите `CreateLogger` на `ILoggerFactory` экземпляра, как показано ниже.

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

В большинстве случаев, он будет проще использовать `ILogger<T>`, как показано в следующем примере.

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Это эквивалентно вызову `CreateLogger` именем полное имя типа `T`.

## <a name="log-level"></a>Уровень ведения журнала

Каждый раз при создании журнала, указать его [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel). Уровень ведения журнала указывает степень серьезность или важности.  Например, можно написать `Information` входа при завершении метода, как правило, `Warning` журнала, когда метод возвращает код возврата 404 и `Error` запись в журнал при перехватывать непредвиденное исключение.

В следующем примере кода, имена методов (например, `LogWarning`) укажите уровень ведения журнала.  Первый параметр — [входа событие с кодом](#log-event-id) (описано ниже в этой статье).  Остальные параметры построения строки сообщения журнала.

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Методы журнала, которые включают уровень из имени метода, [методы расширения для ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions). На самом деле эти методы вызывают `Log` метода, принимающего `LogLevel` параметра. Можно вызвать `Log` непосредственно, а не один из этих методов расширения, но синтаксис относительно сложен. Дополнительные сведения см. в разделе [ILogger интерфейс](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) и [расширения регистратора исходный код](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).

ASP.NET Core определяет следующие [уровня ведения журнала](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), упорядоченные здесь из как минимум с наибольшим уровнем серьезности.

* Трассировка = 0

  Сведения, особенно полезны, если только для отладки проблемы разработчика. Эти сообщения может содержать приложение конфиденциальные данные и поэтому не следует включать в рабочей среде. *По умолчанию отключено.* Пример: `Credentials: {"User":"someuser", "Password":"P@ssword"}`

* Отладка = 1

  Для получения сведений с краткосрочной полезность во время разработки и отладки. Пример: `Entering method Configure with flag set to true.` вы обычно не позволит `Debug` уровень журналы в рабочей среде, если нужно устранить, из-за большого объема журналов.

* Сведения = 2

  Для отслеживания общего потока приложения. Эти журналы обычно имеют некоторые долговечность. Пример: `Request received for path /api/todo`

* Предупреждение = 3

  Для нестандартных или непредвиденные события в потоке приложения. Они могут включать ошибок или других условий, не вызывают остановку приложения, но может, который необходимо исследовать. Обработанные исключения — это единый механизм для использования `Warning` уровень ведения журнала. Пример: `FileNotFoundException for file quotes.txt.`

* Ошибка = 4

  Для ошибок и исключений, не может быть обработан. Эти сообщения указывают сбоя текущего действия или операции (например, текущего HTTP-запроса), не произошла ошибка уровня приложения. Пример сообщения журнала:`Cannot insert record due to duplicate key violation.`

* Критические = 5

  Для сбоев, которые требуют немедленного внимания. Примеры: потери данных, недостаточно места на диске.

Уровень ведения журнала можно использовать для отображения окна или управления, сколько выходные данные журнала записываются на определенный носитель. Например, в рабочей среде может потребоваться все журналы `Information` уровня и сокращения, чтобы перейти на томе хранилища данных и все журналы `Warning` уровня и более поздних версий, чтобы перейти к хранилищу данных значение. Во время разработки, обычно может отправить журналы `Warning` или более поздней версии серьезность на консоль. Затем при необходимости для устранения неполадок, можно добавить `Debug` уровне. [Фильтрации журнала](#log-filtering) далее в этой статье объясняется, как управлять какие уровни журнала, поставщик обрабатывает.

Платформа ASP.NET Core записывает `Debug` уровня журналы для событий framework. Примеры журнала ранее в этой статье исключен журналы ниже `Information` уровне, без `Debug` были выявлены уровня журналы. Ниже приведен пример журнала консоли при запуске примера приложения, настроить отображение `Debug` и выше журналах поставщика консоли.

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

## <a name="log-event-id"></a>Идентификатор журнала событий

Каждый раз при создании журнала, можно указать *событие с кодом*. Пример приложения делает это с помощью локально определенные `LoggingEvents` класса:

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](logging/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

Код события — целочисленное значение, которое можно использовать, чтобы связать набор событий, зарегистрированных друг с другом. Для экземпляра журнала для добавления в список покупок элемента может быть событие с кодом 1000 и журнала для завершения покупки может быть событие с кодом 1001.

В выходных данных ведения журнала идентификатор события может хранится в поле или сообщение содержит текст, в зависимости от поставщика.  Поставщик отладки не показывает коды событий, однако поставщик консоли отображается их в квадратные скобки после категории:

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-format-string"></a>Строка формата сообщений журнала

Каждый раз, когда записи журнала, укажите текст сообщения. Строка сообщения может содержать именованные заполнители в аргумент, в котором размещаются значения, как показано в следующем примере:

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Порядок заполнители, а не их имена, определяет, какие параметры будут использоваться для них. Например, предположим, что имеется следующий код:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

Полученное в результате сообщение журнала будет выглядеть следующим образом:

```
Parameter values: parm1, parm2
```

Платформа ведения журнала сообщений форматирование таким образом, чтобы сделать возможным для поставщиков ведения журнала для реализации [семантической ведения журналов, также известные как структурированный ведения журнала](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging). Так как сами аргументы передаются в систему ведения журналов не только строковое сообщение, отформатированное, регистраторов можно хранить значения параметров, как поля, кроме строку сообщения. Например если Направляемая выходные данные журналов для табличного хранилища Azure и вызова метода средство ведения журнала выглядит следующим образом:

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Каждая сущность таблицы Azure может иметь `ID` и `RequestTime` свойства, упрощающими запросы на данные журнала. Может найти все журналы в пределах определенного `RequestTime` диапазона, без необходимости анализировать времени ожидания текстового сообщения.

## <a name="logging-exceptions"></a>Ведение журнала исключений

Методы ведения журнала имеют перегрузки, которые позволяют передавать исключение, как показано в следующем примере:

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

Различные поставщики обрабатывать сведения об исключении по-разному. Ниже приведен пример выходных данных отладки поставщика из приведенного выше кода.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Фильтрацию журнала

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Можно указать минимальный уровень для определенного поставщика и категории, или для всех поставщиков или всех категориях.  Все журналы с минимальным размером не передачей с этим поставщиком, поэтому они не получить отображаются или записываются. 

Если вы хотите запретить все журналы, можно указать `LogLevel.None` как уровень минимального ведения журнала. Целочисленное значение `LogLevel.None` — 6, то есть выше, чем `LogLevel.Critical` (5).

**Создание правила фильтрации в конфигурации**

Шаблоны проектов создают код, который вызывает `CreateDefaultBuilder` настройке ведения журнала для поставщиков консоли и отладки. `CreateDefaultBuilder` Метод также устанавливает ведения журнала для поиска конфигурации в `Logging` статьи, используя код, аналогичный следующему:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

Данные конфигурации указывает уровни минимальное журнала поставщиком и категории, как показано в следующем примере:

[!code-json[](logging/sample2/appsettings.json)]

Этот JSON создает шесть правила фильтрации, один для отладки поставщика, четырех для поставщика консоли и которая применяется ко всем поставщикам. Вы увидите, как одну из этих правил, выбранного для каждого поставщика при `ILogger` создан объект.

**Правила фильтрации в коде**

Правила фильтрации можно зарегистрировать в коде, как показано в следующем примере:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

Второй `AddFilter` указывает поставщика, отладки, используя имя его типа. Первый `AddFilter` применяется для всех поставщиков, так как он не определяет тип поставщика.

**Как правило фильтрации применяются**

Данные конфигурации и `AddFilter` код, приведенный в предыдущих примерах создавать правила, показаны в следующей таблице. Первые шесть извлекаются из примера конфигурации и последние две берутся из примера кода.

Число|Поставщик|Категории, которые начинаются с|Уровень минимального ведения журнала|
------|--------|--------------------------|-----------------|
1|Отладка|Все категории|Сведения|
2|Консоль|Microsoft.AspNetCore.Mvc.Razor.Internal|Предупреждение|
3|Консоль|Microsoft.AspNetCore.Mvc.Razor.Razor|Отладка|
4|Консоль|Microsoft.AspNetCore.Mvc.Razor|Ошибка|
5|Консоль|Все категории|Сведения|
6|Все поставщики|Все категории|Отладка
7|Все поставщики|Система|Отладка
8|Отладка|Майкрософт|Трассировка

При создании `ILogger` объект для записи журналов, `ILoggerFactory` выбирает одно правило каждого поставщика, чтобы применить для этого средства ведения журнала. Все сообщения записываются, `ILogger` объект фильтруются на основе выбранных правил. Наиболее определенное правило, возможных для каждого поставщика и категории пары выбираются из доступных правил.

Следующий запрошенный алгоритм используется для каждого поставщика при `ILogger` создается для данной категории:

* Выберите все правила, которые соответствуют поставщика или его псевдоним.  Если ничего не найдено, выберите все правила с поставщиком пустым.
* В результатах предыдущего шага выберите правила, которые с наиболее длительным временем соответствующий префикс категории. Если ничего не найдено, выберите все правила, которые не указать категорию.
* Если выбрано несколько правил занять **последний** один.
* Если правила не выбраны, используйте `MinimumLevel`.
 
Предположим, у вас есть вышеуказанных правил и создании `ILogger` объекта для категории «Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine»:

* Для отладки поставщика применяются правила 1, 6 и 8. Правило 8 является наиболее подходящим, то есть выбранный.
* Для поставщика консоли применяются следующие правила, 3, 4, 5 и 6. Правило 3 не является наиболее подходящим.

При создании журналов с помощью `ILogger` для категории «Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine» журналы из `Trace` уровне и более поздних версий будет отправлена отладки поставщика и журналы `Debug` уровня и выше перейдет к поставщику консоли.

**Поставщик псевдонимов**

Имя типа можно использовать, чтобы указать поставщика в конфигурации, но каждый поставщик определяет более коротким *псевдоним* проще в использовании. Для встроенных поставщиков используйте следующие псевдонимы:

- Консоль
- Отладка
- Журнал событий
- AzureAppServices
- TraceSource
- EventSource

**Минимальный уровень по умолчанию**

Нет минимальный уровень, вступает в силу только в том случае, если правила из конфигурации или кода не применяются для данного поставщика и категории. В следующем примере показано, как задать минимальный уровень:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

Если минимальный уровень не задано явно, значение по умолчанию — `Information`, означающее, что `Trace` и `Debug` журналы учитываются.

**Функции фильтров**

Можно написать код в функцию фильтрации для применения правил фильтрации. Функция фильтрации вызывается для всех поставщиков и категорий, которые не имел правил, назначенные им по конфигурации или кода. Код в функции имеет доступ к тип поставщика, категорию и уровень ведения журнала, чтобы определить, будут ли занесены сообщения. Пример:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Некоторые поставщики ведения журнала позволяют указать при журналы должны записываются на носителе или обрабатывается на основе уровень ведения журнала и категории.

`AddConsole` И `AddDebug` методы расширения предоставляют перегрузки, которые позволяют передавать в критериям фильтрации. В следующем примере кода вызывает службу консоли, чтобы игнорировать журналы ниже `Warning` уровня, во время отладки поставщик не учитывает журналы, которые платформа создает.

[!code-csharp[](logging/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

`AddEventLog` Имеет перегрузку, которая принимает `EventLogSettings` экземпляр, который может содержать функции фильтрации в его `Filter` свойство. TraceSource поставщик не предоставляет никаких из этих перегрузок с момента его уровень ведения журнала, а также другие параметры основаны на `SourceSwitch` и `TraceListener` его использует.

Можно задать правила фильтрации для всех поставщиков, которые зарегистрированы с `ILoggerFactory` экземпляра с помощью `WithFilter` метода расширения. В приведенном ниже примере ограничивает журналы framework (категория начинается с «Microsoft» или «Система»), чтобы предупреждения при этом разрешив журнала отладки на уровне приложения.

[!code-csharp[](logging/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Если вы хотите использовать фильтрацию, чтобы предотвратить запись для определенной категории все журналы, можно указать `LogLevel.None` как уровень минимального ведения журнала для этой категории. Целочисленное значение `LogLevel.None` — 6, то есть выше, чем `LogLevel.Critical` (5).

`WithFilter` Предоставляется метод расширения [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) пакет NuGet. Метод возвращает новую `ILoggerFactory` экземпляра, чтобы отфильтровать журнал сообщения, передаваемые для всех поставщиков средства ведения журнала, зарегистрированных в нем. Он не влияет на любой другой `ILoggerFactory` экземплярам, включая исходные `ILoggerFactory` экземпляра.

---

## <a name="log-scopes"></a>Областей журнала

Можно сгруппировать набор логических операций в *область* для присоединения те же данные для каждого журнала, созданные в рамках этого набора.  Например может потребоваться каждого журнала, созданного в ходе обработки транзакций включать идентификатор транзакции.

Область — `IDisposable` тип, возвращаемый `ILogger.BeginScope<TState>` метод и остается включенным до их освобождения. Использовать область, если ее заключить в средство ведения журнала вызовов в `using` блока, как показано ниже:

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

Следующий пример кода позволяет областей для поставщика консоли:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

В *Program.cs*:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

В *файла Startup.cs*:

[!code-csharp[](logging/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

Каждое сообщение журнала включает информации:

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>Встроенные регистраторов.

ASP.NET Core поставляется следующих поставщиков:

* [Консоль](#console)
* [Отладка](#debug)
* [EventSource](#eventsource)
* [EventLog](#eventlog)
* [TraceSource](#tracesource)
* [служба приложений Azure](#appservice);

<a id="console"></a>
### <a name="the-console-provider"></a>Поставщик консоли

[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) пакета поставщик отправляет выходные данные журнала на консоль. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

[Перегрузки AddConsole](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) позволяют передать минимальный уровень, функция фильтрации и логическое значение, указывающее, поддерживаются ли области.  Другим вариантом является передача `IConfiguration` объект, который можно указать поддержку областей и уровней ведения журнала. 

Если вы собираетесь консоль поставщик для использования в рабочей среде, имейте в виду, что он имеет значительное влияние на производительность.

При создании нового проекта в Visual Studio `AddConsole` метод выглядит следующим образом:

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Этот код ссылается на `Logging` раздел *appSettings.json* файла:

[!code-json[](logging/sample//appsettings.json)]

Параметры, отображаемые предел framework журналы для предупреждений, позволяет приложению для входа на уровне отладки, как описано в статье [фильтрации журнала](#log-filtering) раздела. Дополнительные сведения см. в разделе [конфигурации](configuration.md).

---

<a id="debug"></a>
### <a name="the-debug-provider"></a>Отладка поставщика

[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) пакет поставщика записывает выходные данные журналов с помощью [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) класса (`Debug.WriteLine` вызовы методов).

В Linux, этот поставщик записывает журналы на */var/log/message*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

[Перегрузки AddDebug](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) позволяют передать минимальный уровень или функцию фильтрации.

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a>Поставщик EventSource

Для приложений, предназначенных для ASP.NET Core 1.1.0 или более поздней версии, [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) пакет поставщика могут реализовывать события трассировки. В Windows, он использует [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803). Поставщик является кросс платформенных, но есть событие не сбора и отображения программ еще для Linux и macOS. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

Для сбора и просмотра журналов рекомендуется использовать [PerfView программа](https://www.microsoft.com/download/details.aspx?id=28567). Существуют другие средства просмотра журналов трассировки событий Windows, но PerfView обеспечивает максимальное удобство работы с событиями трассировки событий Windows, генерируемой ASP.NET. 

Чтобы настроить PerfView для сбора событий, регистрируемых этим поставщиком, добавьте строку `*Microsoft-Extensions-Logging` для **дополнительные поставщики** списка. (Не пропустите звездочки в начале строки.)

![Дополнительные поставщики Perfview](logging/_static/perfview-additional-providers.png)

Захват событий на сервере Nano Server требует дополнительной настройки:

* Подключение удаленного взаимодействия PowerShell в Nano Server:

  ```powershell
  Enter-PSSession [name]
  ```

* Создание сеанса трассировки событий Windows:

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* Добавьте поставщиков трассировки событий Windows для [CLR](https://msdn.microsoft.com/library/ff357718), ASP.NET Core и другим пользователям при необходимости. Идентификатор GUID поставщика ASP.NET Core `3ac73b97-af73-50e9-0822-5da4367920d0`. 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* Запустить узел и выполните нужные действия требуется получить сведения трассировки.

* Остановите сеанс трассировки после окончания:

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

Итоговый *C:\trace.etl* файла могут быть проанализированы с PerfView, как и в других выпусках Windows.

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a>Поставщик журнала событий Windows

[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) пакета поставщик отправляет выходные данные журнала в журнал событий Windows.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

[Перегрузки AddEventLog](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) позволяют передать `EventLogSettings` или минимальный уровень.

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a>Поставщик TraceSource

[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) пакет поставщик использует [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) библиотеки и поставщики.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

[Перегрузки AddTraceSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) позволяют передать переключатель источника и прослушиватель трассировки.

Чтобы использовать этот поставщик, приложения должен выполняться на .NET Framework (а не .NET Core). Поставщик позволяет, маршрутизации сообщений на различные [прослушиватели](https://msdn.microsoft.com/library/4y5y10s7), такие как [TextWriterTraceListener](https://msdn.microsoft.com/library/system.diagnostics.textwritertracelistener) используется в примере приложения.

В следующем примере настраивается `TraceSource` поставщик, записывающий в журнал `Warning` и более поздней версии сообщения в окно консоли.

[!code-csharp[](logging/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a>Поставщик службы приложений Azure

[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) пакет поставщика записи журналов в текстовые файлы в файловой системе приложения службы приложений Azure и [хранилище больших двоичных объектов](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) в учетной записи хранилища Azure. Поставщик является доступной только для приложений, предназначенных для ASP.NET Core 1.1.0 или более поздней версии. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

> [!NOTE]
> ASP.NET 2.0 лежит в режиме предварительного просмотра.  Приложениям, созданным с помощью последнего предварительного выпуска может не работать при развертывании в службе приложений Azure. При выпуске ASP.NET 2.0 основной службе приложений Azure будет выполняться 2.0 приложения и службы приложений Azure, поставщик будет работать, как указано ниже.

Не нужно устанавливать пакет поставщика или вызов `AddAzureWebAppDiagnostics` метода расширения.  Поставщик является автоматически доступны для приложения при развертывании приложения в службе приложений Azure.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

`AddAzureWebAppDiagnostics` Перегрузка позволяет передавать в [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs), с помощью которого можно переопределить параметры по умолчанию, такие как ведение журнала выходных данных шаблона, имя BLOB-объекта и максимального размера файла. (*Выходные данные шаблона* является строка формата сообщений, применяется для всех журналов на основе того, который вы указали при вызове `ILogger` метода.)  

---

При развертывании приложения службы приложений, приложение учитывает параметры в [журналы диагностики](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) раздел **службы приложений** на портале Azure. При изменении этих параметров, изменения установки вступили в силу немедленно, без необходимости перезапустить приложение или повторно развернуть код, чтобы он. 

![Параметры ведения журнала Azure](logging/_static/azure-logging-settings.png)

По умолчанию для файлов журнала находится в *D:\\домашней\\LogFiles\\приложения* папку и имя файла по умолчанию — *yyyymmdd.txt диагностики*. Максимальный размер файла по умолчанию составляет 10 МБ, а по умолчанию файлы сохраняются не более 2. Имя BLOB-объектов по умолчанию *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*. Дополнительные сведения о поведении по умолчанию см. в разделе [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).

Поставщик работает только при запуске проекта в среде Azure.  Не оказывает никакого эффекта при локальном запуске &mdash; не записываются в локальных файлах или в хранилище локального развертывания для больших двоичных объектов.

## <a name="third-party-logging-providers"></a>Ведение журнала сторонних поставщиков

Ниже приведены некоторые платформ ведения журнала сторонних разработчиков, которые работают с ASP.NET Core.

* [ELMAH.IO](https://github.com/elmahio/Elmah.Io.Extensions.Logging) -поставщика для службы Elmah.Io

* [JSNLog](http://jsnlog.com) -регистрирует в журнале серверные исключения JavaScript и других событий на стороне клиента.

* [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) -поставщика для службы Loggr

* [NLog](https://github.com/NLog/NLog.Extensions.Logging) -поставщик для библиотеки NLog

* [Serilog](https://github.com/serilog/serilog-framework-logging) -поставщик для библиотеки Serilog

Некоторые сторонние платформы можно сделать [семантической ведения журналов, также известные как структурированный ведения журнала](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

С использованием сторонней платформы похоже на использование одного из встроенных поставщиков: добавьте пакет NuGet в проект и вызов метода расширения на `ILoggerFactory`. Дополнительные сведения см. в документации каждого framework.

Собственных поставщиков также можно создать для поддержки других платформ ведения журналов или требований ведения журнала.
