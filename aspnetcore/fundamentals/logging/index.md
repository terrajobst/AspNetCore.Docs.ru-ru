---
title: Ведение журналов в ASP.NET Core
author: ardalis
description: Сведения о платформе ведения журналов в ASP.NET Core. Ознакомьтесь со встроенными поставщиками ведения журналов и получите подробные сведения о распространенных сторонних поставщиках.
manager: wpickett
ms.author: tdykstra
ms.date: 12/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/logging/index
ms.openlocfilehash: 8b53a19f4958e97198175d6acea4017d54f827bb
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/24/2018
---
# <a name="logging-in-aspnet-core"></a>Ведение журналов в ASP.NET Core

Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith) и [Том Дакстра](https://github.com/tdykstra) (Tom Dykstra)

ASP.NET Core поддерживает API ведения журнала, который работает с разными поставщиками. Встроенные поставщики позволяют отправлять журналы в одно или несколько мест назначений. Можно подключить стороннюю платформу ведения журналов. В этой статье содержатся сведения об использовании в коде встроенного API и поставщиков ведения журналов.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

---

## <a name="how-to-create-logs"></a>Создание журналов

Чтобы создать журналы, получите объект `ILogger` из контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection):

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Затем в этом объекте средства ведения журнала вызовите методы ведения журналов:

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

В этом примере создаются журналы с классом `TodoController` в качестве *категории*. Категории рассматриваются [далее в этой статье](#log-category).

ASP.NET Core не поддерживает асинхронные методы ведения журналов. Здесь требуется настолько быстрая запись, что затраты на асинхронные операции совершенно нецелесообразны. Если возникла обратная ситуация, рекомендуется рассмотреть возможность изменения способа ведения журнала. Если хранилище данных работает медленно, сначала записывайте сообщения журнала в быстродействующее хранилище, а затем перемещайте их в медленное хранилище. Например, записывайте сообщения в очередь, которую обрабатывает другой процесс для долгосрочного сохранения в медленном хранилище.

## <a name="how-to-add-providers"></a>Добавление поставщиков

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Поставщик ведения журнала принимает сообщения, созданные с помощью объекта `ILogger`, и отображает или сохраняет их. Например, поставщик Console отображает сообщения на консоли, а поставщик службы приложений Azure может сохранить их в хранилище больших двоичных объектов.

Для использования поставщика вызовите метод расширения `Add<ProviderName>` поставщика в файле *Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

Шаблон проекта по умолчанию включает ведение журнала с помощью метода[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___).

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Поставщик ведения журнала принимает сообщения, созданные с помощью объекта `ILogger`, и отображает или сохраняет их. Например, поставщик Console отображает сообщения на консоли, а поставщик службы приложений Azure может сохранить их в хранилище больших двоичных объектов.

Чтобы использовать поставщик, установите его пакет NuGet и вызовите метод расширения поставщика в экземпляре `ILoggerFactory`, как показано в следующем примере.

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

[Внедрение зависимостей](xref:fundamentals/dependency-injection) (DI) ASP.NET Core предоставляет экземпляр `ILoggerFactory`. Методы расширения `AddConsole` и `AddDebug` определены в пакетах [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) и [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/). Каждый метод расширения вызывает метод `ILoggerFactory.AddProvider`, передавая экземпляр поставщика. 

> [!NOTE]
> В примере приложения в этой статье поставщики ведения журналов добавляются в метод `Configure` класса `Startup`. Чтобы получить выходные данные журнала из выполняемого ранее кода, добавьте поставщики ведения журналов в конструктор класса `Startup`. 

---

Сведения о каждом [встроенном поставщике ведения журнала](#built-in-logging-providers) и ссылки на [сторонние поставщики](#third-party-logging-providers) приведены далее в статье.

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

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

В большинстве случаев будет проще использовать `ILogger<T>`, как показано в следующем примере.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Это эквивалентно вызову `CreateLogger` с полным именем типа `T`.

## <a name="log-level"></a>Уровень ведения журнала

Каждый раз при создании журнала указывается его значение перечисления [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel). Уровень ведения журнала означает степень серьезности или важности. Например, можно создать журнал `Information` при нормальном завершении метода, журнал `Warning`, если метод возвращает код 404, и журнал `Error` при перехвате непредвиденного исключения.

В следующем примере кода имена методов (например, `LogWarning`) указывают уровень ведения журнала. Первый параметр — это [ идентификатор события журнала](#log-event-id). Второй параметр — это [шаблон сообщения](#log-message-template) с заполнителями для значений аргументов, предоставляемых оставшимися параметрами метода. Параметры метода рассматриваются более подробно далее в этой статье.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

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

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

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

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

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

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

Разные поставщики обрабатывают сведения об исключениях по-разному. Ниже приведен пример выходных данных поставщика Debug из приведенного выше кода.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Фильтрация журналов

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Можно указать минимальный уровень ведения журнала для определенного поставщика и категории или для всех поставщиков или всех категорий. Все журналы с уровнем ниже минимального не передаются этому поставщику, поэтому они не отображаются и не сохраняются. 

Чтобы запретить все журналы, укажите `LogLevel.None` в качестве минимального уровня ведения журнала. `LogLevel.None` равно целочисленному значению 6, то есть больше, чем `LogLevel.Critical` (5).

**Создание правил фильтрации в конфигурации**

Шаблоны проектов создают код, который вызывает `CreateDefaultBuilder` для настройки ведения журналов для поставщиков Console и Debug. Метод `CreateDefaultBuilder` также настраивает ведение журнала для просмотра конфигурации в разделе `Logging`, используя код, аналогичный следующему:

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

Данные конфигурации указывают минимальные уровни ведения журнала для каждого поставщика и категории, как показано в следующем примере:

[!code-json[](index/sample2/appsettings.json)]

Этот JSON создает шесть правил фильтрации: одно для поставщика Debug, четыре для поставщика Console и одно, которое применяется ко всем поставщикам. Далее вы увидите, как при создании объекта `ILogger` для каждого поставщика выбирается лишь одно из этих правил.

**Правила фильтрации в коде**

Можно зарегистрировать правила фильтрации в коде, как показано в следующем примере:

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

Второй `AddFilter` указывает поставщика, Debug, используя имя его типа. Первый `AddFilter` применяется ко всем поставщикам, так как он не определяет тип поставщика.

**Применение правил фильтрации**

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

**Псевдонимы поставщиков**

Имя типа можно использовать, чтобы указать поставщик в конфигурации, но каждый поставщик имеет более короткий и простой в использовании *псевдоним*. Для встроенных поставщиков используйте следующие псевдонимы:

- Консоль
- Отладка
- EventLog
- AzureAppServices
- TraceSource
- EventSource

**Минимальный уровень по умолчанию**

Существует параметр минимального уровня, который применяется, только если к определенному поставщику или категории не подходят правила из конфигурации или кода. В следующем примере показано, как задать минимальный уровень:

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

Если минимальный уровень не задан явным образом, используется значение по умолчанию — `Information`, означающее, что журналы `Trace` и `Debug` игнорируются.

**Функции фильтрации**

В функции фильтрации можно написать код для применения правил фильтрации. Функция фильтрации вызывается для всех поставщиков и категорий, которым в конфигурации и (или) коде не назначены правила. Код в функции имеет доступ к типу поставщика, категории и уровню ведения журнала, чтобы определить необходимость регистрации сообщений. Пример:

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Некоторые поставщики ведения журналов позволяют указывать, когда журналы следует записывать на носитель, а когда — игнорировать. При этом учитывается уровень ведения журнала и категория.

Методы расширения `AddConsole` и `AddDebug`предоставляют перегрузки для передачи условий фильтрации. В следующем примере кода поставщик Console игнорирует журналы с уровнем ниже `Warning`, а поставщик Debug не учитывает журналы, создаваемые платформой.

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

Метод `AddEventLog` имеет перегрузку, принимающую экземпляр `EventLogSettings`, который в своем свойстве `Filter` может содержать функцию фильтрации. Поставщик TraceSource не предоставляет эти перегрузки, так как уровень ведения журнала и другие параметры определяются для него на основе используемых `SourceSwitch` и `TraceListener`.

Можно задать правила фильтрации для всех поставщиков, которые зарегистрированы в экземпляре `ILoggerFactory`, с помощью метода расширения `WithFilter`. В приведенном ниже примере журналы платформы (категория начинается с "Microsoft" или "System") ограничиваются предупреждениями, однако разрешаются журналы приложений на уровне отладки.

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Чтобы использовать фильтрацию для предотвращения создания всех журналов для определенной категории, укажите `LogLevel.None` в качестве минимального уровня ведения журнала для этой категории. `LogLevel.None` равно целочисленному значению 6, то есть больше, чем `LogLevel.Critical` (5).

Пакет NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) предоставляет метод расширения `WithFilter`. Метод возвращает новый экземпляр `ILoggerFactory` для фильтрации журнала сообщений, переданного всем поставщикам средства ведения журналов, которые зарегистрированы в этом экземпляре. Он не влияет на другие экземпляры `ILoggerFactory`, включая исходный экземпляр `ILoggerFactory`.

---

## <a name="log-scopes"></a>Области журналов

Для присоединения одних и тех же данных к каждому журналу, созданному в рамках набора, можно сгруппировать набор логических операций в *область*. Например, может потребоваться, чтобы каждый журнал, созданный в ходе обработки транзакции, содержал идентификатор транзакции.

Область — это тип `IDisposable`, возвращаемый методом `ILogger.BeginScope<TState>` и действующий до удаления. Чтобы использовать область, следует заключить вызовы средства ведения журналов в блок `using`, как показано ниже:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

Следующий код предоставляет области для поставщика Console:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

В файле *Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Для включения ведения журнала на уровне области требуется параметр `IncludeScopes` средства ведения журналов. Настройка `IncludeScopes` с помощью файлов конфигурации *appsettings* будет доступна в выпуске ASP.NET Core 2.1.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

В файле *Startup.cs*:

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

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

* [Консоль](#console)
* [Отладка](#debug)
* [EventSource](#eventsource)
* [EventLog](#eventlog)
* [TraceSource](#tracesource)
* [служба приложений Azure](#appservice);

<a id="console"></a>
### <a name="the-console-provider"></a>Поставщик Console

Пакет поставщика [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) отправляет выходные данные журнала на консоль. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
loggerFactory.AddConsole()
```

[Перегрузки AddConsole](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) позволяют передавать минимальный уровень ведения журнала, функцию фильтрации и логическое значение, указывающее, поддерживаются ли области. Другим вариантом является передача объекта `IConfiguration`, который может указать поддержку областей и уровни ведения журнала. 

Если вы собираетесь использовать поставщик Console в рабочей среде, имейте в виду, что он оказывает значительное влияние на производительность.

При создании проекта в Visual Studio метод `AddConsole` выглядит следующим образом:

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Этот код ссылается на раздел `Logging` файла *appSettings.json*:

[!code-json[](index/sample//appsettings.json)]

Приведенные параметры ограничивают журналы платформы предупреждениями, но разрешают регистрацию приложений на уровне отладки, как описано в разделе [Фильтрация журналов](#log-filtering). Дополнительные сведения см. в разделе [Конфигурация](xref:fundamentals/configuration/index).

---

<a id="debug"></a>
### <a name="the-debug-provider"></a>Поставщик Debug

Пакет поставщика [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) записывает выходные данные журналов с помощью класса [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (вызовов метода `Debug.WriteLine`).

В Linux этот поставщик записывает журналы в каталог */var/log/message*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

[Перегрузки AddDebug](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) позволяют передавать минимальный уровень ведения журнала или функцию фильтрации.

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a>Поставщик EventSource

Для приложений, предназначенных для ASP.NET Core 1.1.0 или более поздних версий, реализовывать события трассировки можно с помощью пакета поставщика [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource). В Windows используется [трассировка событий Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803). Поставщик является кроссплатформенным, но для Linux и macOS инструменты сбора событий и отображений пока отсутствуют. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

Для сбора и просмотра данных журналов рекомендуется использовать [программу PerfView](https://www.microsoft.com/download/details.aspx?id=28567). Существуют и другие средства для просмотра журналов трассировки событий Windows, но PerfView обеспечивает максимальное удобство работы с событиями трассировки событий Windows, создаваемыми ASP.NET. 

Чтобы настроить PerfView для сбора событий, регистрируемых этим поставщиком, добавьте строку `*Microsoft-Extensions-Logging` в список **Дополнительные поставщики**. (Не пропустите звездочку в начале строки.)

![Дополнительные поставщики Perfview](index/_static/perfview-additional-providers.png)

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a>Поставщик Windows EventLog

Пакет поставщика [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) отправляет выходные данные журнала в журнал событий Windows.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

[Перегрузки AddEventLog](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) позволяют передавать `EventLogSettings` или минимальный уровень ведения журнала.

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a>Поставщик TraceSource

Пакет поставщика [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) использует библиотеки и поставщики [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

[Перегрузки AddTraceSource](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) позволяют передавать исходный параметр и прослушиватель трассировки.

Чтобы использовать этот поставщик, приложение должно выполняться в .NET Framework (а не в .NET Core). Поставщик позволяет перенаправлять сообщения различным [прослушивателям](/dotnet/framework/debug-trace-profile/trace-listeners), например [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr), который используется в примере приложения.

В следующем примере показана настройка поставщика `TraceSource`, записывающего в окне консоли сообщения типа `Warning` и сообщения с более высоким уровнем серьезности.

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a>Поставщик службы приложений Azure

Пакет поставщика [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) записывает журналы в текстовые файлы в файловой системе приложения службы приложений Azure и в [хранилище больших двоичных объектов](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) в учетной записи хранения Azure. Поставщик доступен только для приложений, предназначенных для ASP.NET Core 1.1.0 или более поздних версий. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Если планируется использовать .NET Core, не нужно устанавливать пакет поставщика или явным образом вызвать `AddAzureWebAppDiagnostics`. Поставщик становится автоматически доступным для приложения при развертывании приложения в службе приложений Azure.

Если планируется использовать .NET Framework, добавьте пакет поставщика в проект и вызовите `AddAzureWebAppDiagnostics`.

```csharp
logging.AddAzureWebAppDiagnostics();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

Перегрузка `AddAzureWebAppDiagnostics` позволяет передавать [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs) для переопределения настроек по умолчанию, таких как шаблон выходных данных ведения журнала, имя BLOB-объекта и максимально допустимый размер файла. (*Шаблон выходных данных* — это шаблон сообщений, который применяется ко всем журналам на основе того, который вы указали при вызове метода `ILogger`.)

---

При развертывании в приложение службы приложений ваше приложение учитывает параметры в разделе [Журналы диагностики](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) на странице **Служба приложений** на портале Azure. Изменения этих параметров вступают в силу сразу же после внесения и не требуют перезапуска приложения или повторного развертывания в нем кода. 

![Параметры ведения журнала Azure](index/_static/azure-logging-settings.png)

По умолчанию файлы журнала находятся в папке *D:\\home\\LogFiles\\Application*, а имя файла по умолчанию — *diagnostics-yyyymmdd.txt*. Максимальный размер файла по умолчанию составляет 10 МБ, а максимальное количество сохраняемых по умолчанию файлов равно 2. Имя BLOB-объекта по умолчанию — *{имя_приложения}{метка_времени}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*. Дополнительные сведения о поведении по умолчанию см. в разделе [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).

Поставщик работает только при выполнении проекта в среде Azure. Он не функционирует при запуске в локальной среде, то есть не выполняет запись в локальные файлы или в локальное хранилище разработки для больших двоичных объектов.

## <a name="third-party-logging-providers"></a>Сторонние поставщики ведения журналов

Некоторые сторонние платформы ведения журналов, которые работают с ASP.NET Core:

* [ELMAH.IO](https://elmah.io/) ([в репозитории GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging));
* [JSNLog](http://jsnlog.com/) ([в репозитории GitHub](https://github.com/mperdeck/jsnlog));
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


## <a name="see-also"></a>См. также

[Высокопроизводительное ведение журналов с помощью LoggerMessage](xref:fundamentals/logging/loggermessage)
