---
title: "Высокопроизводительное ведение журналов с помощью LoggerMessage в ASP.NET Core"
author: guardrex
description: "Сведения о том, как использовать функции LoggerMessage для создания кэшируемых делегатов, которым нужно меньше выделений объектов, чем методам расширения для средства ведения журнала в сценариях высокопроизводительного ведения журналов."
manager: wpickett
ms.author: riande
ms.date: 11/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: b155826b5047e88a79d9e339d7bca8885a79006d
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a>Высокопроизводительное ведение журналов с помощью LoggerMessage в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Функции [LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) создают кэшируемые делегаты, которым нужно меньше выделений объектов и вычислительных ресурсов, чем [методам расширения для средства ведения журнала](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), таким как `LogInformation`, `LogDebug` и `LogError`. Для сценариев высокопроизводительного ведения журналов используйте шаблон `LoggerMessage`.

`LoggerMessage` предоставляет следующие преимущества производительности по сравнению с методами расширения для средства ведения журнала:

* Методы расширения для средства ведения журнала требуют "упаковку-преобразование" типов значений, таких как `int`, в `object`. Шаблон `LoggerMessage` позволяет избежать упаковки-преобразования за счет статических полей `Action` и методов расширения со строго типизированными параметрами.
* Методы расширения для средства ведения журнала должны анализировать шаблон сообщения (именованную строку формата) при каждой записи сообщения журнала. `LoggerMessage` требует анализировать шаблон всего один раз — при определении сообщения.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

Пример приложения показывает функции `LoggerMessage` с базовой системой отслеживания цитат. Это приложение добавляет и удаляет цитаты, используя выполняющуюся в памяти базу данных. По мере выполнения этих операций создаются сообщения журнала с помощью шаблона `LoggerMessage`.

## <a name="loggermessagedefine"></a>LoggerMessage.Define

[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) создает делегат `Action` для внесения сообщения в журнал. Перегрузки `Define` позволяют передать до шести параметров типа в именованную строку формата (шаблон).

## <a name="loggermessagedefinescope"></a>LoggerMessage.DefineScope

[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) создает делегат `Func` для определения [области журнала](xref:fundamentals/logging/index#log-scopes). Перегрузки `DefineScope` позволяют передать до трех параметров типа в именованную строку формата (шаблон).

## <a name="message-template-named-format-string"></a>Шаблон сообщения (именованная строка формата)

Строка, предоставляемая методам `Define` и `DefineScope`, является шаблоном, а не интерполированной строкой. Заполнители заполняются в том порядке, в котором указаны типы. Имена заполнителей в шаблоне должны быть описательными и согласованными между разными шаблонами. Они выступают в качестве имен свойств в структурированных данных журнала. Мы рекомендуем использовать [стиль Pascal ](/dotnet/standard/design-guidelines/capitalization-conventions) для имен заполнителей. Например, `{Count}`, `{FirstName}`.

## <a name="implementing-loggermessagedefine"></a>Реализация LoggerMessage.Define

Каждое сообщение журнала является `Action`, хранящимся в статическом поле, созданном `LoggerMessage.Define`. Например, пример приложения создает поле для описания сообщения журнала для запроса GET для страницы индексов (*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

Для `Action` укажите:

* уровень ведения журнала;
* уникальный идентификатор события ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) с именем статического метода расширения;
* шаблон сообщения (именованная строка формата). 

Запрос страницы индексов в примере приложения задает:

* уровень ведения журнала — `Information`;
* идентификатор события — `1` с именем метода `IndexPageRequested`;
* шаблон сообщения (именованная строка формата) — строка.

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

Структурированные хранилища для ведения журнала могут использовать имя события, когда оно предоставляется вместе с идентификатором события, для улучшения ведения журнала. Например, [Serilog](https://github.com/serilog/serilog-extensions-logging) использует имя события.

`Action` вызывается с помощью строго типизированного метода расширения. Метод `IndexPageRequested` заносит в журнал сообщение для запроса GET страницы индексов в примере приложения:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

`IndexPageRequested` вызывается для средства ведения журнала в методе `OnGetAsync` в *Pages/Index.cshtml.cs*:

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

Изучите выходные данные консоли приложения:

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

Чтобы передать параметры в сообщение журнала, определите до шести типов при создании статического поля. Пример приложения регистрирует строку при добавлении цитаты, определяя тип `string` для поля `Action`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

Шаблон сообщения журнала делегата получает его значения заполнителей из предоставленных типов. Пример приложения определяет делегат для добавления цитаты, где параметр квоты имеет значение`string`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

Статический метод расширения для добавления цитаты `QuoteAdded` получает значение аргумента цитаты и передает его в делегат `Action`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

В страничной модели страницы индексов (*Pages/Index.cshtml.cs*) для регистрации сообщения в журнале вызывается `QuoteAdded`:

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

Изучите выходные данные консоли приложения:

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

Пример приложения реализует шаблон `try`&ndash;`catch` для удаления цитаты. Для успешной операции удаления регистрируется информационное сообщение. Сообщение об ошибке регистрируется для операции удаления, когда возникает исключение. Сообщение журнала для неудачной операции удаления включает трассировку стека исключений (*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

Обратите внимание, как исключение передается делегату в `QuoteDeleteFailed`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

В страничной модели для страницы индексов успешная операция удаления цитаты вызывает метод `QuoteDeleted` для средства ведения журнала. Если удаляемая цитата не найдена, возникает исключение `ArgumentNullException`. Это исключение перехватывается инструкцией `try`&ndash;`catch` и регистрируется путем вызова метода `QuoteDeleteFailed` для средства ведения журнала в блоке `catch` (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

При успешном удалении цитаты изучите выходные данные консоли приложения:

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

При неудачном удалении цитаты изучите выходные данные консоли приложения. Обратите внимание, что исключение входит в состав сообщения журнала:

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

Определите [область журнала](xref:fundamentals/logging/index#log-scopes), чтобы применить последовательность сообщений журнала с помощью метода [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope).

Пример приложения имеет кнопку **Clear All** (Очистить все), позволяющую удалить все цитаты в базе данных. Удаление цитат производится по одной за раз. При каждом удалении цитаты для средства ведения журнала вызывается метод `QuoteDeleted`. В эти сообщения журнала добавляется область журнала.

Включите `IncludeScopes` в параметрах средства ведения журнала консоли:

[!code-csharp[Main](loggermessage/sample/Program.cs?name=snippet1&highlight=22)]

Чтобы включить области журнала в приложениях ASP.NET Core 2.0, нужно задать `IncludeScopes`. Возможность задания `IncludeScopes` через файлы конфигурации *appsettings* запланирована для выпуска ASP.NET Core 2.1.

Пример приложения очищает другие поставщики и добавляет фильтры, чтобы сократить объем выходных данных журнала. Это упрощает просмотр сообщений журнала в примере, которые демонстрируют функции `LoggerMessage`.

Для создания области журнала добавьте поле, содержащее делегат `Func` для области. Пример приложения создает поле `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

Используйте `DefineScope` для создания делегата. Можно указать до трех типов для использования в качестве аргументов шаблона при вызове делегата. Пример приложения использует шаблон сообщения, включающий в себя число удаленных цитат (тип `int`):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

Предоставьте статический метод расширения для сообщения журнала. Включите любые параметры типа для именованных свойств, отображаемых в шаблоне сообщений. Пример приложения принимает число удаляемых цитат `count` и возвращает `_allQuotesDeletedScope`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

Область создает оболочку для вызовов расширения ведения журнала в блоке `using`:

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

Изучите сообщения журнала в выходных данных консоли приложения: Следующий результат указывает три удаленных цитаты с включенным сообщением области журнала:

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
