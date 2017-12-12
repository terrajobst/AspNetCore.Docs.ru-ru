---
title: "Ведение журнала высокой производительности с LoggerMessage в ASP.NET Core"
author: guardrex
description: "Узнайте, как использовать возможности LoggerMessage Создание кэшируемый делегатов, которые требуют меньше распределение объектов, чем методы расширения, средства ведения журнала для сценариев высокой производительности ведения журнала."
ms.author: riande
manager: wpickett
ms.date: 11/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: defba75c6c9ea13d24af4cd8515d82d9e7cf9853
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a>Ведение журнала высокой производительности с LoggerMessage в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) возможности создания кэшируемый делегатов, которые требуют меньше распределение объектов и снижение вычислительных ресурсов, чем [методы расширения для средства ведения журнала](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), такие как `LogInformation`, `LogDebug`и `LogError`. В сценариях высокого уровня ведения журнала, используйте `LoggerMessage` шаблон.

`LoggerMessage`предоставляет следующие преимущества по сравнению с методами расширения средство ведения журнала производительности:

* Методы расширения для средства ведения журнала требуется типы значений «упаковкой» (преобразование), например `int`, в `object`. `LoggerMessage` Шаблон позволяет избежать упаковка-преобразование с помощью статического `Action` поля и методы расширения со строгой типизацией параметров.
* Методы расширения для средства ведения журнала необходимо выполнить синтаксический анализ шаблона сообщения (строка форматирования именованный), каждый раз записывается сообщение журнала. `LoggerMessage`требуется только синтаксическом анализе шаблона один раз при определении сообщения.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

В примере приложения показано `LoggerMessage` возможности с помощью системы отслеживания основные предложения. Добавляет и удаляет кавычки использует базу данных в памяти приложения. При возникновении этих операций, сообщения журнала создаются с помощью `LoggerMessage` шаблон.

## <a name="loggermessagedefine"></a>LoggerMessage.Define

[Определение (LogLevel EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) создает `Action` делегат для ведения журнала сообщений. `Define`перегрузки позволяют передача до шести параметров типа именованного формата строки (шаблона).

## <a name="loggermessagedefinescope"></a>LoggerMessage.DefineScope

[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) создает `Func` делегата для определения [входа области](xref:fundamentals/logging/index#log-scopes). `DefineScope`перегрузки позволяют передача до трех параметров типа именованного формата строки (шаблона).

## <a name="message-template-named-format-string"></a>Шаблон сообщения (с именем строка форматирования)

Строка, предоставленная для `Define` и `DefineScope` методов, шаблон и не интерполированную строку. Заполнители заполняются в том порядке, в котором были указаны типы. Имена заполнителей в шаблоне должны быть описательным и согласованное на шаблоны. Они служат в качестве имен свойств в структурированных данных журнала. Мы рекомендуем [Pascal регистр](/dotnet/standard/design-guidelines/capitalization-conventions) имен заполнителя. Например `{Count}`, `{FirstName}`.

## <a name="implementing-loggermessagedefine"></a>Реализация LoggerMessage.Define

Каждое сообщение журнала является `Action` , которые содержатся в статического поля, созданные `LoggerMessage.Define`. Например, в пример приложения создает поле для описания сообщения журнала для запроса GET для страницы индекса (*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

Для `Action`, укажите:

* Уровень ведения журнала.
* Уникальный идентификатор события ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) с именем расширения статического метода.
* Шаблон сообщения (с именем строка форматирования). 

Запрос страницы индекса наборов выборки приложения:

* Уровень для ведения журнала `Information`.
* Идентификатор события, `1` именем `IndexPageRequested` метод.
* Шаблон сообщения (с именем строка форматирования) в строку.

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

Структурированный ведения журнала хранилища воспользоваться имя события прилагается идентификатор события для обогащения ведения журнала. Например [Serilog](https://github.com/serilog/serilog-extensions-logging) использует имя события.

`Action` Вызывается с помощью метода расширения на строго типизированными. `IndexPageRequested` Метод заносит в журнал сообщение для запроса на получение страницы индекса в пример приложения:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

`IndexPageRequested`вызывается для средства ведения журнала в `OnGetAsync` метод в *Pages/Index.cshtml.cs*:

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

Проверьте приложение вывод на консоль:

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

Чтобы передать параметры в сообщение журнала, определите до шести типов при создании статического поля. Пример приложения регистрирует строка при добавлении предложения, определив `string` тип для `Action` поля:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

Шаблон сообщения журнала делегата получает его значения заполнителей из типов, предоставляемых. Пример приложения определяет делегат для добавления предложения параметр квоты которого `string`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

Метод статического расширения для добавления предложения, `QuoteAdded`, получает значения аргумента квоты и передает их в `Action` делегировать:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

В файле кода страницы индекса (*Pages/Index.cshtml.cs*), `QuoteAdded` вызывается в журнал сообщение:

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

Проверьте приложение вывод на консоль:

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

В образце приложения реализуется `try` &ndash; `catch` шаблон для удаления квоты. Для операции удаления успешно записывается информационное сообщение. Сообщение об ошибке регистрируется для операции удаления, когда возникает исключение. Сообщение журнала для неудачной операции удаления включает трассировку стека исключения (*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

Обратите внимание на то, как исключение передается в делегат `QuoteDeleteFailed`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

В индекс страницы кода, вызывает удаление успешно Квота `QuoteDeleted` метод для средства ведения журнала. Если не найдены знак кавычек для удаления, `ArgumentNullException` возникает исключение. Перехватить исключение в приложении `try` &ndash; `catch` инструкции и регистрируется путем вызова `QuoteDeleteFailed` метод средство ведения журнала в `catch` блока (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

При успешном удалении знак кавычек, проверьте приложение вывод на консоль:

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

При сбое удаления квоты, проверьте приложение вывод на консоль. Обратите внимание, что исключения будут включены в сообщение журнала.

```console
fail: LoggerMessageSample.Pages.IndexModel[5]
      => RequestId:0HL90M6E7PHK5:00000010 RequestPath:/ => /Index
      Quote delete failed (Id = 999)
System.ArgumentNullException: Value cannot be null.
Parameter name: entity
   at Microsoft.EntityFrameworkCore.Utilities.Check.NotNull[T](T value, String parameterName)
   at Microsoft.EntityFrameworkCore.DbContext.Remove[TEntity](TEntity entity)
   at Microsoft.EntityFrameworkCore.Internal.InternalDbSet`1.Remove(TEntity entity)
   at LoggerMessageSample.Pages.IndexModel.<OnPostDeleteQuoteAsync>d__14.MoveNext() in 
      <PATH>\sample\Pages\Index.cshtml.cs:line 87
```

## <a name="implementing-loggermessagedefinescope"></a>Реализация LoggerMessage.DefineScope

Определение [входа области](xref:fundamentals/logging/index#log-scopes) для применения в серии сообщений журнала с помощью [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) метод.

Пример приложения имеет **очистить все** кнопку для удаления всех квот в базе данных. Кавычки будут удалены, удалив один одновременно. Каждый раз, удаляется знак кавычек, `QuoteDeleted` метод будет вызван на средство ведения журнала. Область журнала добавляется в эти сообщения журнала.

Включить `IncludeScopes` в параметры средства ведения журнала консоли:

[!code-csharp[Main](loggermessage/sample/Program.cs?name=snippet1&highlight=22)]

Параметр `IncludeScopes` требуется для включения областей журнала в приложениях ASP.NET Core 2.0. Установка `IncludeScopes` через *appsettings* файлы конфигурации — это компонент, который запланировал для версии ASP.NET Core 2.1.

Пример приложения очищает других поставщиков и добавляет фильтры, чтобы уменьшить выходные данные журнала. Это упрощает см. пример сообщений журнала, которые демонстрируют `LoggerMessage` функции.

Для создания области журнала, добавьте поле для хранения `Func` делегата для области. Пример приложения создает поле с именем `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

Используйте `DefineScope` для создания делегата. Три типа можно указать до для использования как аргументы шаблонов при вызове делегата. Пример приложения используется шаблон сообщений, который включает в себя количество удаленных квот ( `int` типа):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

Предусмотрите статических расширения сообщение журнала. Включать любые параметры типа для именованных свойств, отображаемых в шаблоне сообщения. Пример приложения принимает `count` кавычки для удаления и возвращает `_allQuotesDeletedScope`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

Вызывает модуль ведения журнала в качестве оболочки для области `using` блока:

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

Проверьте сообщения журнала в приложение вывод на консоль. Следующий результат показывает три кавычки удалена с сообщением области журнала включено:

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 1' Id = 2)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 2' Id = 3)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 3' Id = 4)
```

## <a name="see-also"></a>См. также

* [Ведение журнала](xref:fundamentals/logging/index)
