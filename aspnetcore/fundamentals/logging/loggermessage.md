---
title: Высокопроизводительное ведение журналов с помощью LoggerMessage в ASP.NET Core
author: guardrex
description: Сведения о том, как использовать LoggerMessage для создания кэшируемых делегатов, которым нужно меньше выделений объектов в сценариях высокопроизводительного ведения журналов.
manager: wpickett
ms.author: riande
ms.date: 11/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: 5b5bd03b6cb5da693f046653a09ba400ee6ff585
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729198"
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a><span data-ttu-id="34523-103">Высокопроизводительное ведение журналов с помощью LoggerMessage в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="34523-103">High-performance logging with LoggerMessage in ASP.NET Core</span></span>

<span data-ttu-id="34523-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="34523-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="34523-105">Функции [LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) создают кэшируемые делегаты, которым нужно меньше выделений объектов и вычислительных ресурсов, чем [методам расширения для средства ведения журнала](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), таким как `LogInformation`, `LogDebug` и `LogError`.</span><span class="sxs-lookup"><span data-stu-id="34523-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) features create cacheable delegates that require fewer object allocations and reduced computational overhead compared to [logger extension methods](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), such as `LogInformation`, `LogDebug`, and `LogError`.</span></span> <span data-ttu-id="34523-106">Для сценариев высокопроизводительного ведения журналов используйте шаблон `LoggerMessage`.</span><span class="sxs-lookup"><span data-stu-id="34523-106">For high-performance logging scenarios, use the `LoggerMessage` pattern.</span></span>

<span data-ttu-id="34523-107">`LoggerMessage` предоставляет следующие преимущества производительности по сравнению с методами расширения для средства ведения журнала:</span><span class="sxs-lookup"><span data-stu-id="34523-107">`LoggerMessage` provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="34523-108">Методы расширения для средства ведения журнала требуют "упаковку-преобразование" типов значений, таких как `int`, в `object`.</span><span class="sxs-lookup"><span data-stu-id="34523-108">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="34523-109">Шаблон `LoggerMessage` позволяет избежать упаковки-преобразования за счет статических полей `Action` и методов расширения со строго типизированными параметрами.</span><span class="sxs-lookup"><span data-stu-id="34523-109">The `LoggerMessage` pattern avoids boxing by using static `Action` fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="34523-110">Методы расширения для средства ведения журнала должны анализировать шаблон сообщения (именованную строку формата) при каждой записи сообщения журнала.</span><span class="sxs-lookup"><span data-stu-id="34523-110">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="34523-111">`LoggerMessage` требует анализировать шаблон всего один раз — при определении сообщения.</span><span class="sxs-lookup"><span data-stu-id="34523-111">`LoggerMessage` only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="34523-112">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="34523-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="34523-113">Пример приложения показывает функции `LoggerMessage` с базовой системой отслеживания цитат.</span><span class="sxs-lookup"><span data-stu-id="34523-113">The sample app demonstrates `LoggerMessage` features with a basic quote tracking system.</span></span> <span data-ttu-id="34523-114">Это приложение добавляет и удаляет цитаты, используя выполняющуюся в памяти базу данных.</span><span class="sxs-lookup"><span data-stu-id="34523-114">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="34523-115">По мере выполнения этих операций создаются сообщения журнала с помощью шаблона `LoggerMessage`.</span><span class="sxs-lookup"><span data-stu-id="34523-115">As these operations occur, log messages are generated using the `LoggerMessage` pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="34523-116">LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="34523-116">LoggerMessage.Define</span></span>

<span data-ttu-id="34523-117">[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) создает делегат `Action` для внесения сообщения в журнал.</span><span class="sxs-lookup"><span data-stu-id="34523-117">[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) creates an `Action` delegate for logging a message.</span></span> <span data-ttu-id="34523-118">Перегрузки `Define` позволяют передать до шести параметров типа в именованную строку формата (шаблон).</span><span class="sxs-lookup"><span data-stu-id="34523-118">`Define` overloads permit passing up to six type parameters to a named format string (template).</span></span>

<span data-ttu-id="34523-119">Строка, предоставляемая методу `Define`, является шаблоном, а не интерполированной строкой.</span><span class="sxs-lookup"><span data-stu-id="34523-119">The string provided to the `Define` method is a template and not an interpolated string.</span></span> <span data-ttu-id="34523-120">Заполнители заполняются в том порядке, в котором указаны типы.</span><span class="sxs-lookup"><span data-stu-id="34523-120">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="34523-121">Имена заполнителей в шаблоне должны быть описательными и согласованными между разными шаблонами.</span><span class="sxs-lookup"><span data-stu-id="34523-121">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="34523-122">Они выступают в качестве имен свойств в структурированных данных журнала.</span><span class="sxs-lookup"><span data-stu-id="34523-122">They serve as property names within structured log data.</span></span> <span data-ttu-id="34523-123">Мы рекомендуем использовать [стиль Pascal ](/dotnet/standard/design-guidelines/capitalization-conventions) для имен заполнителей.</span><span class="sxs-lookup"><span data-stu-id="34523-123">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="34523-124">Например, `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="34523-124">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="34523-125">Каждое сообщение журнала является `Action`, хранящимся в статическом поле, созданном `LoggerMessage.Define`.</span><span class="sxs-lookup"><span data-stu-id="34523-125">Each log message is an `Action` held in a static field created by `LoggerMessage.Define`.</span></span> <span data-ttu-id="34523-126">Например, пример приложения создает поле для описания сообщения журнала для запроса GET для страницы индексов (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="34523-126">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="34523-127">Для `Action` укажите:</span><span class="sxs-lookup"><span data-stu-id="34523-127">For the `Action`, specify:</span></span>

* <span data-ttu-id="34523-128">уровень ведения журнала;</span><span class="sxs-lookup"><span data-stu-id="34523-128">The log level.</span></span>
* <span data-ttu-id="34523-129">уникальный идентификатор события ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) с именем статического метода расширения;</span><span class="sxs-lookup"><span data-stu-id="34523-129">A unique event identifier ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) with the name of the static extension method.</span></span>
* <span data-ttu-id="34523-130">шаблон сообщения (именованная строка формата).</span><span class="sxs-lookup"><span data-stu-id="34523-130">The message template (named format string).</span></span> 

<span data-ttu-id="34523-131">Запрос страницы индексов в примере приложения задает:</span><span class="sxs-lookup"><span data-stu-id="34523-131">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="34523-132">уровень ведения журнала — `Information`;</span><span class="sxs-lookup"><span data-stu-id="34523-132">Log level to `Information`.</span></span>
* <span data-ttu-id="34523-133">идентификатор события — `1` с именем метода `IndexPageRequested`;</span><span class="sxs-lookup"><span data-stu-id="34523-133">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="34523-134">шаблон сообщения (именованная строка формата) — строка.</span><span class="sxs-lookup"><span data-stu-id="34523-134">Message template (named format string) to a string.</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="34523-135">Структурированные хранилища для ведения журнала могут использовать имя события, когда оно предоставляется вместе с идентификатором события, для улучшения ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="34523-135">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="34523-136">Например, [Serilog](https://github.com/serilog/serilog-extensions-logging) использует имя события.</span><span class="sxs-lookup"><span data-stu-id="34523-136">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="34523-137">`Action` вызывается с помощью строго типизированного метода расширения.</span><span class="sxs-lookup"><span data-stu-id="34523-137">The `Action` is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="34523-138">Метод `IndexPageRequested` заносит в журнал сообщение для запроса GET страницы индексов в примере приложения:</span><span class="sxs-lookup"><span data-stu-id="34523-138">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="34523-139">`IndexPageRequested` вызывается для средства ведения журнала в методе `OnGetAsync` в *Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="34523-139">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="34523-140">Изучите выходные данные консоли приложения:</span><span class="sxs-lookup"><span data-stu-id="34523-140">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="34523-141">Чтобы передать параметры в сообщение журнала, определите до шести типов при создании статического поля.</span><span class="sxs-lookup"><span data-stu-id="34523-141">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="34523-142">Пример приложения регистрирует строку при добавлении цитаты, определяя тип `string` для поля `Action`:</span><span class="sxs-lookup"><span data-stu-id="34523-142">The sample app logs a string when adding a quote by defining a `string` type for the `Action` field:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="34523-143">Шаблон сообщения журнала делегата получает его значения заполнителей из предоставленных типов.</span><span class="sxs-lookup"><span data-stu-id="34523-143">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="34523-144">Пример приложения определяет делегат для добавления цитаты, где параметр квоты имеет значение`string`:</span><span class="sxs-lookup"><span data-stu-id="34523-144">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="34523-145">Статический метод расширения для добавления цитаты `QuoteAdded` получает значение аргумента цитаты и передает его в делегат `Action`:</span><span class="sxs-lookup"><span data-stu-id="34523-145">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the `Action` delegate:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="34523-146">В страничной модели страницы индексов (*Pages/Index.cshtml.cs*) для регистрации сообщения в журнале вызывается `QuoteAdded`:</span><span class="sxs-lookup"><span data-stu-id="34523-146">In the Index page's page model (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="34523-147">Изучите выходные данные консоли приложения:</span><span class="sxs-lookup"><span data-stu-id="34523-147">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="34523-148">Пример приложения реализует шаблон `try`&ndash;`catch` для удаления цитаты.</span><span class="sxs-lookup"><span data-stu-id="34523-148">The sample app implements a `try`&ndash;`catch` pattern for quote deletion.</span></span> <span data-ttu-id="34523-149">Для успешной операции удаления регистрируется информационное сообщение.</span><span class="sxs-lookup"><span data-stu-id="34523-149">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="34523-150">Сообщение об ошибке регистрируется для операции удаления, когда возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="34523-150">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="34523-151">Сообщение журнала для неудачной операции удаления включает трассировку стека исключений (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="34523-151">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="34523-152">Обратите внимание, как исключение передается делегату в `QuoteDeleteFailed`:</span><span class="sxs-lookup"><span data-stu-id="34523-152">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="34523-153">В страничной модели для страницы индексов успешная операция удаления цитаты вызывает метод `QuoteDeleted` для средства ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="34523-153">In the page model for the Index page, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="34523-154">Если удаляемая цитата не найдена, возникает исключение `ArgumentNullException`.</span><span class="sxs-lookup"><span data-stu-id="34523-154">When a quote isn't found for deletion, an `ArgumentNullException` is thrown.</span></span> <span data-ttu-id="34523-155">Это исключение перехватывается инструкцией `try`&ndash;`catch` и регистрируется путем вызова метода `QuoteDeleteFailed` для средства ведения журнала в блоке `catch` (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="34523-155">The exception is trapped by the `try`&ndash;`catch` statement and logged by calling the `QuoteDeleteFailed` method on the logger in the `catch` block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

<span data-ttu-id="34523-156">При успешном удалении цитаты изучите выходные данные консоли приложения:</span><span class="sxs-lookup"><span data-stu-id="34523-156">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="34523-157">При неудачном удалении цитаты изучите выходные данные консоли приложения.</span><span class="sxs-lookup"><span data-stu-id="34523-157">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="34523-158">Обратите внимание, что исключение входит в состав сообщения журнала:</span><span class="sxs-lookup"><span data-stu-id="34523-158">Note that the exception is included in the log message:</span></span>

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

## <a name="loggermessagedefinescope"></a><span data-ttu-id="34523-159">LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="34523-159">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="34523-160">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) создает делегат `Func` для определения [области журнала](xref:fundamentals/logging/index#log-scopes).</span><span class="sxs-lookup"><span data-stu-id="34523-160">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) creates a `Func` delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="34523-161">Перегрузки `DefineScope` позволяют передать до трех параметров типа в именованную строку формата (шаблон).</span><span class="sxs-lookup"><span data-stu-id="34523-161">`DefineScope` overloads permit passing up to three type parameters to a named format string (template).</span></span>

<span data-ttu-id="34523-162">Как и в случае с методом `Define`, строка, предоставляемая методу `DefineScope`, является шаблоном, а не интерполированной строкой.</span><span class="sxs-lookup"><span data-stu-id="34523-162">As is the case with the `Define` method, the string provided to the `DefineScope` method is a template and not an interpolated string.</span></span> <span data-ttu-id="34523-163">Заполнители заполняются в том порядке, в котором указаны типы.</span><span class="sxs-lookup"><span data-stu-id="34523-163">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="34523-164">Имена заполнителей в шаблоне должны быть описательными и согласованными между разными шаблонами.</span><span class="sxs-lookup"><span data-stu-id="34523-164">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="34523-165">Они выступают в качестве имен свойств в структурированных данных журнала.</span><span class="sxs-lookup"><span data-stu-id="34523-165">They serve as property names within structured log data.</span></span> <span data-ttu-id="34523-166">Мы рекомендуем использовать [стиль Pascal ](/dotnet/standard/design-guidelines/capitalization-conventions) для имен заполнителей.</span><span class="sxs-lookup"><span data-stu-id="34523-166">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="34523-167">Например, `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="34523-167">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="34523-168">Определите [область журнала](xref:fundamentals/logging/index#log-scopes), чтобы применить последовательность сообщений журнала с помощью метода [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope).</span><span class="sxs-lookup"><span data-stu-id="34523-168">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) method.</span></span>

<span data-ttu-id="34523-169">Пример приложения имеет кнопку **Clear All** (Очистить все), позволяющую удалить все цитаты в базе данных.</span><span class="sxs-lookup"><span data-stu-id="34523-169">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="34523-170">Удаление цитат производится по одной за раз.</span><span class="sxs-lookup"><span data-stu-id="34523-170">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="34523-171">При каждом удалении цитаты для средства ведения журнала вызывается метод `QuoteDeleted`.</span><span class="sxs-lookup"><span data-stu-id="34523-171">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="34523-172">В эти сообщения журнала добавляется область журнала.</span><span class="sxs-lookup"><span data-stu-id="34523-172">A log scope is added to these log messages.</span></span>

<span data-ttu-id="34523-173">Включите `IncludeScopes` в раздел средства ведения журнала консоли в файле *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="34523-173">Enable `IncludeScopes` in the console logger section of *appsettings.json*:</span></span>

[!code-csharp[](loggermessage/sample/appsettings.json?highlight=3-5)]

<span data-ttu-id="34523-174">Для создания области журнала добавьте поле, содержащее делегат `Func` для области.</span><span class="sxs-lookup"><span data-stu-id="34523-174">To create a log scope, add a field to hold a `Func` delegate for the scope.</span></span> <span data-ttu-id="34523-175">Пример приложения создает поле `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="34523-175">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="34523-176">Используйте `DefineScope` для создания делегата.</span><span class="sxs-lookup"><span data-stu-id="34523-176">Use `DefineScope` to create the delegate.</span></span> <span data-ttu-id="34523-177">Можно указать до трех типов для использования в качестве аргументов шаблона при вызове делегата.</span><span class="sxs-lookup"><span data-stu-id="34523-177">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="34523-178">Пример приложения использует шаблон сообщения, включающий в себя число удаленных цитат (тип `int`):</span><span class="sxs-lookup"><span data-stu-id="34523-178">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="34523-179">Предоставьте статический метод расширения для сообщения журнала.</span><span class="sxs-lookup"><span data-stu-id="34523-179">Provide a static extension method for the log message.</span></span> <span data-ttu-id="34523-180">Включите любые параметры типа для именованных свойств, отображаемых в шаблоне сообщений.</span><span class="sxs-lookup"><span data-stu-id="34523-180">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="34523-181">Пример приложения принимает число удаляемых цитат `count` и возвращает `_allQuotesDeletedScope`:</span><span class="sxs-lookup"><span data-stu-id="34523-181">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="34523-182">Область создает оболочку для вызовов расширения ведения журнала в блоке `using`:</span><span class="sxs-lookup"><span data-stu-id="34523-182">The scope wraps the logging extension calls in a `using` block:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="34523-183">Изучите сообщения журнала в выходных данных консоли приложения:</span><span class="sxs-lookup"><span data-stu-id="34523-183">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="34523-184">Следующий результат указывает три удаленных цитаты с включенным сообщением области журнала:</span><span class="sxs-lookup"><span data-stu-id="34523-184">The following result shows three quotes deleted with the log scope message included:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="34523-185">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="34523-185">Additional resources</span></span>

* [<span data-ttu-id="34523-186">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="34523-186">Logging</span></span>](xref:fundamentals/logging/index)
