---
title: ASP.NET основные рекомендации по производительности
author: mjrousos
description: Советы по повышению производительности в ASP.NET приложений Core и предотвращению общих проблем с производительностью.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/06/2020
no-loc:
- SignalR
uid: performance/performance-best-practices
ms.openlocfilehash: 068a35fbe410dad24030fe68c0dfd062b402212c
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977188"
---
# <a name="aspnet-core-performance-best-practices"></a>ASP.NET основные рекомендации по производительности

Автор: [Майк Роусос (Mike Rousos)](https://github.com/mjrousos)

В этой статье содержатся рекомендации по наилучшей производительности с ASP.NET Core.

## <a name="cache-aggressively"></a>Кэш агрессивно

Кэшинг обсуждается в нескольких частях этого документа. Для получения дополнительной информации см. <xref:performance/caching/response>.

## <a name="understand-hot-code-paths"></a>Понимание путей горячего кода

В этом документе *путь горячего кода* определяется как путь кода, который часто вызывается и где происходит большая часть времени выполнения. Горячие пути кода обычно ограничивают масштаб и производительность приложения и обсуждаются в нескольких частях этого документа.

## <a name="avoid-blocking-calls"></a>Избегайте блокирования вызовов

ASP.NET основные приложения должны быть разработаны для обработки многих запросов одновременно. Асинхронные AIS позволяют небольшому пулу потоков обрабатывать тысячи одновременных запросов, не дожидаясь блокировки вызовов. Вместо того, чтобы ждать выполнения длительной синхронной задачи, поток может работать по другому запросу.

Общей проблемой производительности в приложениях ASP.NET Core является блокирование вызовов, которые могут быть асинхронными. Многие синхронные блокировки вызовов приводят к [голоду потока пула](https://blogs.msdn.microsoft.com/vancem/2018/10/16/diagnosing-net-core-threadpool-starvation-with-perfview-why-my-service-is-not-saturating-all-cores-or-seems-to-stall/) и деградированному времени отклика.

**Не:**

* Блокируйте асинхронное исполнение, позвонив [в Task.Wait](/dotnet/api/system.threading.tasks.task.wait) или [Task.Result](/dotnet/api/system.threading.tasks.task-1.result).
* Приобрести блокировки в общих путей кода. ASP.NET приложения Core являются наиболее performant, когда архитектор для запуска кода параллельно.
* Позвоните [В Task.Run](/dotnet/api/system.threading.tasks.task.run) и сразу же дождитесь его. ASP.NET Core уже запускает код приложения на обычных потоках потока потока, поэтому вызов Task.Run приводит только к дополнительному ненужному планированию потока пула. Даже если запланированный код блокирует поток, Task.Run не препятствует этому.

**У:**

* Сделайте [горячие пути кода](#understand-hot-code-paths) асинхронными.
* Доступ к данным вызова, ввод/во-пасси и длительные операции API асинхронно при наличии асинхронного API. Не **not** используйте [Task.Run](/dotnet/api/system.threading.tasks.task.run) для синхронизации API асинхронным.
* Сделать действия контроллера/страницы Razor асинхронными. Весь стек вызовов асинхронный, чтобы извлечь выгоду из шаблонов [async/await.](/dotnet/csharp/programming-guide/concepts/async/)

Профайлер, например [PerfView,](https://github.com/Microsoft/perfview)может использоваться для поиска потоков, часто добавляемых в [пул потоков.](/windows/desktop/procthread/thread-pools) Событие `Microsoft-Windows-DotNETRuntime/ThreadPoolWorkerThread/Start` указывает на поток, добавленный в пул потока. <!--  For more information, see [async guidance docs](TBD-Link_To_Davifowl_Doc)  -->

## <a name="minimize-large-object-allocations"></a>Минимизация выделения крупных объектов

[Сборщик мусора .NET Core](/dotnet/standard/garbage-collection/) управляет распределением и выпуском памяти автоматически в ASP.NET приложений Core. Автоматический сбор мусора обычно означает, что разработчикам не нужно беспокоиться о том, как и когда память освобождается. Однако очистка неотсываемых объектов занимает время процессора, поэтому разработчики должны свести к минимуму выделение объектов в [горячих путях кода.](#understand-hot-code-paths) Сбор мусора особенно дорого стоит на крупных объектах (> 85 Байт). Крупные объекты хранятся на [большой объектной кучи](/dotnet/standard/garbage-collection/large-object-heap) и требуют полного (поколения 2) сбора мусора для очистки. В отличие от коллекций поколения 0 и поколения 1, коллекция поколения 2 требует временной приостановки исполнения приложения. Частое распределение и де-распределение крупных объектов может привести к несогласованности производительности.

Рекомендации

* **Рассмотрите** возможность кэширования крупных объектов, которые часто используются. Кэширование больших объектов предотвращает дорогостоящие выделения.
* **Сделайте** буферы пула с помощью [>\<ArrayPool T](/dotnet/api/system.buffers.arraypool-1) для хранения больших массивов.
* **Не** выделяйте много, недолго крупных объектов на [пути горячего кода.](#understand-hot-code-paths)

Проблемы с памятью, такие как предыдущая, можно диагностировать, просмотрев статистику сбора мусора (GC) в [PerfView](https://github.com/Microsoft/perfview) и изучив:

* Время сбора мусора.
* Какой процент времени процессора тратится на сбор мусора.
* Сколько коллекций мусора составляют поколения 0, 1 и 2.

Для получения дополнительной информации [см.](/dotnet/standard/garbage-collection/performance)

## <a name="optimize-data-access-and-io"></a>Оптимизация доступа к данным и ввоза/o

Взаимодействия с хранилищем данных и другими удаленными службами часто являются самыми медленными частями приложения ASP.NET Core. Эффективное чтение и написание данных имеет решающее значение для хорошей производительности.

Рекомендации

* **Вызов** все данные доступа AIS асинхронно.
* **Не извлекайте** больше данных, чем это необходимо. Напишите запросы, чтобы вернуть только данные, необходимые для текущего запроса HTTP.
* **Рассмотрите** возможность кэширования часто доступных данных, извлеченных из базы данных или удаленной службы, если немного устаревшие данные являются приемлемыми. В зависимости от сценария используйте [MemoryCache](xref:performance/caching/memory) или [DistributedCache.](xref:performance/caching/distributed) Для получения дополнительной информации см. <xref:performance/caching/response>.
* **Сделайте** минимизацию сетевых поездок. Цель состоит в том, чтобы получить необходимые данные в одном вызове, а не несколько вызовов.
* **Используйте** [запросы без отслеживания](/ef/core/querying/tracking#no-tracking-queries) в Core Entity Framework При доступе к данным только для целей чтения. EF Core может более эффективно возвращать результаты запросов без отслеживания.
* **Фильтруйте** и агрегируете `.Select`запросы LIN '(например, с `.Sum` `.Where`операторами или операторами), чтобы фильтрация выполнялась базой данных.
* **Учтите,** что EF Core разрешает некоторые операторы запросов на клиенте, что может привести к неэффективному исполнению запроса. Для получения дополнительной [Client evaluation performance issues](/ef/core/querying/client-eval#client-evaluation-performance-issues)информации см.
* **Не** используйте проекционные запросы на собрания, что может привести к выполнения запросов «N и 1» S'L. Для получения дополнительной информации [см.](/ef/core/what-is-new/ef-core-2.1#optimization-of-correlated-subqueries)

[См. подходы EF High Performance,](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries) которые могут повысить производительность в высококлассных приложениях:

* [Создание пулов DbContext](/ef/core/what-is-new/ef-core-2.0#dbcontext-pooling)
* [Явным образом скомпилированные запросы](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries)

Мы рекомендуем измерить влияние предыдущих высокопроизводительных подходов перед фиксацией базы кода. Дополнительная сложность компилированных запросов может не оправдать повышение производительности.

Проблемы с запросами можно обнаружить, проанализировав время, затраченное на доступ к данным с помощью [Application Insights](/azure/application-insights/app-insights-overview) или с помощью инструментов профилирования. Большинство баз данных также предоставляют статистические данные по часто выполняемым запросам.

## <a name="pool-http-connections-with-httpclientfactory"></a>Соединение пула HTTP с httpClientfactory

Хотя [HttpClient](/dotnet/api/system.net.http.httpclient) реализует `IDisposable` интерфейс, он предназначен для повторного использования. Закрытые `HttpClient` экземпляры оставляют розетки открытыми в состоянии `TIME_WAIT` в течение короткого периода времени. Если часто используется путь кода, `HttpClient` который создает и утилизирует объекты, приложение может исчерпать доступные розетки. [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) был представлен в ASP.NET Core 2.1 в качестве решения этой проблемы. Он обрабатывает объединение соединений HTTP для оптимизации производительности и надежности.

Рекомендации

* **Не создавайте** и `HttpClient` не распоряжайтесь экземплярами напрямую.
* **Используйте** [httpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) для `HttpClient` извлечения экземпляров. Для получения дополнительной информации [см.](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)

## <a name="keep-common-code-paths-fast"></a>Держите общие пути кода быстро

Вы хотите, чтобы все ваши коды были быстрыми. Часто называемые пути кода являются наиболее важными для оптимизации. Сюда входит следующее.

* Компоненты Middleware в конвейере обработки запросов приложения, особенно промежуточное программное обеспечение, запущенное на ранних стадиях конвейера. Эти компоненты оказывают большое влияние на производительность.
* Код, выполняемый для каждого запроса или несколько раз за запрос. Например, пользовательские журналы, обработчики авторизации или инициализация временных служб.

Рекомендации

* **Не** используйте пользовательские компоненты промежуточного посуды с длительными задачами.
* **Используйте** инструменты профилирования производительности, такие как [Visual Studio Diagnostic Tools](/visualstudio/profiling/profiling-feature-tour) или [PerfView),](https://github.com/Microsoft/perfview)для определения [путей горячего кода.](#understand-hot-code-paths)

## <a name="complete-long-running-tasks-outside-of-http-requests"></a>Полные длительные задачи за пределами запросов HTTP

Большинство запросов в ASP.NET приложение Core может обрабатываться контроллером или моделью страницы, вызывающей необходимые службы и возвращающей ответ HTTP. Для некоторых запросов, которые связаны с длительными задачами, лучше сделать весь процесс ответа запроса асинхронным.

Рекомендации

* **Не** ждите длительных задач, которые будут выполнены в рамках обычной обработки запросов HTTP.
* **Рассмотрите** возможность обработки долгосрочных запросов с [помощью фоновых служб](xref:fundamentals/host/hosted-services) или вне процесса с [функцией Azure.](/azure/azure-functions/) Завершение работы вне процесса особенно полезно для процессора-интенсивных задач.
* **Используйте** варианты общения в [SignalR](xref:signalr/introduction)режиме реального времени, такие как, чтобы общаться с клиентами асинхронно.

## <a name="minify-client-assets"></a>Миниатюрные клиентские активы

ASP.NET приложения Core со сложными передними концами часто обслуживают множество файлов JavaScript, CSS или изображений. Производительность первоначальных запросов нагрузки может быть улучшена за счет:

* Комплектация, которая объединяет несколько файлов в один.
* Минулямия, которая уменьшает размер файлов, удаляя пробел и комментарии.

Рекомендации

* **Используйте** [встроенную поддержку](xref:client-side/bundling-and-minification) ASP.NET Core для объединения и минимации клиентских активов.
* **Рассмотрите** другие сторонние инструменты, такие как [Webpack,](https://webpack.js.org/)для комплексного управления активами клиентов.

## <a name="compress-responses"></a>Компресс ответы

 Уменьшение размера ответа обычно увеличивает отзывчивость приложения, часто резко. Одним из способов уменьшения размеров полезной нагрузки является сжатие ответов приложения. Для получения дополнительной [информации см.](xref:performance/response-compression)

## <a name="use-the-latest-aspnet-core-release"></a>Используйте последний релиз ASP.NET Core

Каждый новый выпуск ASP.NET Core включает в себя повышение производительности. Оптимизация в .NET Core и ASP.NET Core означает, что новые версии обычно превосходят старые версии. Например, .NET Core 2.1 добавил поддержку для компилированных регулярных выражений и получил выгоду от [Span\<T>. ](https://msdn.microsoft.com/magazine/mt814808.aspx) ASP.NET Core 2.2 добавил поддержку HTTP/2. [ASP.NET Core 3.0 добавляет множество улучшений,](xref:aspnetcore-3.0) которые снижают использование памяти и улучшают пропускную работу. Если производительность является приоритетной задачей, рассмотрите возможность обновления до текущей версии ASP.NET Core.

## <a name="minimize-exceptions"></a>Минимизация исключений

Исключения должны быть редкими. Исключение метания и ловли происходит медленно по сравнению с другими шаблонами потока кода. Из-за этого исключения не должны использоваться для управления обычным потоком программ.

Рекомендации

* **Не** используйте метание или ловлю исключений в качестве средства нормального потока программы, особенно в [горячих путей кода.](#understand-hot-code-paths)
* **Включите** логику в приложение для обнаружения и обработки условий, которые могут привести к исключению.
* **Бросьте** или поймать исключения для необычных или неожиданных условий.

Диагностические инструменты приложения, такие как Application Insights, могут помочь определить общие исключения в приложении, которые могут повлиять на производительность.

## <a name="performance-and-reliability"></a>Производительность и надежность

Следующие разделы предоставляют советы по производительности и известные проблемы с надежностью и решения.

## <a name="avoid-synchronous-read-or-write-on-httprequesthttpresponse-body"></a>Избегайте синхронного чтения или записи на httprequest/HttpResponse тела

Все ввод/вASP.NET Core является асинхронным. Серверы `Stream` реализуют интерфейс, который имеет как синхронные, так и асинхронные перегрузки. Асинхронные из них следует предпочесть, чтобы избежать блокирования потоков пула потоков. Блокирование потоков может привести к голоду пула потоков.

**Не делайте этого:** В следующем примере <xref:System.IO.StreamReader.ReadToEnd*>используется . Он блокирует текущий поток, чтобы ждать результата. Это пример [синхронизации над async](https://github.com/davidfowl/AspNetCoreDiagnosticScenarios/blob/master/AsyncGuidance.md#warning-sync-over-async
).

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MyFirstController.cs?name=snippet1)]

В предыдущем коде `Get` синхронно считывает весь тело запроса HTTP в память. Если клиент медленно загружается, приложение синхронизируется над async. Приложение синхронизируется над async, потому что Kestrel **не** поддерживает синхронные чтения.

**Сделайте это:** Следующий пример <xref:System.IO.StreamReader.ReadToEndAsync*> использует и не блокирует поток во время чтения.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MyFirstController.cs?name=snippet2)]

Предыдущий код асинхронно считывает весь тело запроса HTTP в память.

> [!WARNING]
> Если запрос большой, чтение всего тела запроса HTTP в память может привести к изврцеловане (OOM) состоянию. OOM может привести к отказу в обслуживании.  Для получения дополнительной информации в этом документе [см.](#arlb)

**Сделайте это:** Следующий пример полностью асинхронный с использованием тела не буферизированного запроса:

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MyFirstController.cs?name=snippet3)]

Предыдущий код асинхронно десериализирует тело запроса в объект C..

## <a name="prefer-readformasync-over-requestform"></a>Предпочитаю readFormAsync по сравнению с Request.Form

Используйте `HttpContext.Request.ReadFormAsync` вместо `HttpContext.Request.Form`.
`HttpContext.Request.Form`безопасно ею можно читать только при следующих условиях:

* Форма была прочитана по `ReadFormAsync`призыву к
* Значение кэшированной формы считывается с помощью`HttpContext.Request.Form`

**Не делайте этого:** В следующем `HttpContext.Request.Form`примере используется .  `HttpContext.Request.Form`синхронизирует [синхронизацию над async](https://github.com/davidfowl/AspNetCoreDiagnosticScenarios/blob/master/AsyncGuidance.md#warning-sync-over-async
) и может привести к голоду пула потоков.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MySecondController.cs?name=snippet1)]

**Сделайте это:** Следующий пример `HttpContext.Request.ReadFormAsync` использует для чтения формы тела асинхронно.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MySecondController.cs?name=snippet2)]

<a name="arlb"></a>

## <a name="avoid-reading-large-request-bodies-or-response-bodies-into-memory"></a>Избегайте чтения больших тел запросов или ответных органов в память

В .NET каждое распределение объектов, превышающее 85 кБ, попадает в большую объектную кучу[(LOH).](https://blogs.msdn.microsoft.com/maoni/2006/04/19/large-object-heap/) Крупные объекты стоят дорого двумя способами:

* Стоимость выделения высока, поскольку память для недавно выделенного крупного объекта должна быть очищена. CLR гарантирует, что память для всех вновь выделенных объектов очищается.
* LOH собирается с остальной частью кучи. LOH требует полного [сбора мусора](/dotnet/standard/garbage-collection/fundamentals) или [сбора Gen2.](/dotnet/standard/garbage-collection/fundamentals#generations)

Этот [блог](https://adamsitnik.com/Array-Pool/#the-problem) описывает проблему кратко:

> При выделении крупного объекта он помечается как объект Gen 2. Не Gen 0, как для небольших объектов. Последствия в том, что если вы бежите из памяти в LOH, GC очищает всю управляемую кучу, а не только LOH. Таким образом, он очищает Gen 0, Gen 1 и Gen 2 в том числе LOH. Это называется полным сбором мусора и является наиболее трудоемким сбором мусора. Для многих приложений это может быть приемлемым. Но определенно не для высокопроизводительных веб-серверов, где несколько больших буферов памяти необходимы для обработки среднего веб-запроса (читай из гнезда, декомпресс, расшифровка JSON & больше).

Наивное хранение большого органа запроса `byte[]` `string`или ответа в один или:

* Может привести к быстрому иссяке места в LOH.
* Может вызвать проблемы с производительностью для приложения из-за полного выполнения GCs.

## <a name="working-with-a-synchronous-data-processing-api"></a>Работа с синхронной обработкой данных API

При использовании сериализатора/де-серизатора, который поддерживает только синхронные чтения и записи (например, [JSON.NET):](https://www.newtonsoft.com/json/help/html/Introduction.htm)

* Buffer данные в память асинхронно перед передачей его в serializer / де-сериализатор.

> [!WARNING]
> Если запрос большой, это может привести к изврянке памяти (OOM) состояние. OOM может привести к отказу в обслуживании.  Для получения дополнительной информации в этом документе [см.](#arlb)

ASP.NET Core 3.0 использует <xref:System.Text.Json> по умолчанию для сериализации JSON. <xref:System.Text.Json>:

* асинхронно считывает и записывает JSON;
* оптимизирован для текста UTF-8;
* предоставляет более высокую производительность, чем `Newtonsoft.Json`.

## <a name="do-not-store-ihttpcontextaccessorhttpcontext-in-a-field"></a>Не храните IHttpContextaccessor.HttpContext в поле

[IHttpContextAccessor.httpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext) возвращает `HttpContext` активный запрос при доступе из потока запроса. Не `IHttpContextAccessor.HttpContext` **следует** хранить в поле или переменной.

**Не делайте этого:** Следующий пример `HttpContext` хранит в поле, а затем пытается использовать его позже.

[!code-csharp[](performance-best-practices/samples/3.0/MyType.cs?name=snippet1)]

Предыдущий код часто фиксирует `HttpContext` нулевую или неправильную в конструкторе.

**Сделайте это:** Следующий пример:

* Хранит <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> в поле.
* Использует `HttpContext` поле в нужное время `null`и проверяет.

[!code-csharp[](performance-best-practices/samples/3.0/MyType.cs?name=snippet2)]

## <a name="do-not-access-httpcontext-from-multiple-threads"></a>Не получите доступ к HttpContext из нескольких потоков

`HttpContext`*НЕ* является безопасным потоком. Параллельный `HttpContext` доступ из нескольких потоков может привести к неопределенным поведением, таким как зависания, сбои и повреждения данных.

**Не делайте этого:** Следующий пример делает три параллельных запроса и регистрирует входящий путь запроса до и после исходящего запроса HTTP. Путь запроса доступен из нескольких потоков, потенциально параллельно.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncFirstController.cs?name=snippet1&highlight=25,28)]

**Сделайте это:** Следующий пример копирует все данные из входящего запроса, прежде чем сделать три параллельных запроса.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncFirstController.cs?name=snippet2&highlight=6,8,22,28)]

## <a name="do-not-use-the-httpcontext-after-the-request-is-complete"></a>Не используйте HttpContext после завершения запроса

`HttpContext`действителен только при наличии активного запроса HTTP в конвейере ASP.NET Core. Весь конвейер ASP.NET Core представляет собой асинхронную цепочку делегатов, которая выполняет каждый запрос. Когда `Task` возвращенное из этой `HttpContext` цепочки завершается, перерабатывается.

**Не делайте этого:** Следующий пример `async void` использует, который делает запрос `await` HTTP завершен, когда первый достигнут:

* Что **всегда** плохая практика в ASP.NET приложений Core.
* Доступ к `HttpResponse` запросу после завершения http.
* Сбои процесса.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncBadVoidController.cs?name=snippet1)]

**Сделайте это:** Следующий пример `Task` возвращается в фреймворк, поэтому запрос HTTP не завершается до завершения действия.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncSecondController.cs?name=snippet1)]

## <a name="do-not-capture-the-httpcontext-in-background-threads"></a>Не захватывайте httpContext в фоновых потоках

**Не делайте этого:** Следующий пример показывает, что `HttpContext` замыкание захватывает из `Controller` свойства. Это плохая практика, потому что рабочий элемент может:

* Выполнить вне сферы запроса.
* Попытка прочитать `HttpContext`неправильно .

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetFirstController.cs?name=snippet1)]

**Сделайте это:** Следующий пример:

* Копирует данные, необходимые в фоновой задаче во время запроса.
* Не ссылается на что-либо от контроллера.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetFirstController.cs?name=snippet2)]

Фоновые задачи должны выполняться как хосусуемые службы. Дополнительные сведения см. в статье [Background tasks with hosted services in ASP.NET Core](xref:fundamentals/host/hosted-services) (Фоновые задачи с размещенными службами в ASP.NET Core).

## <a name="do-not-capture-services-injected-into-the-controllers-on-background-threads"></a>Не захватывайте службы, вводимые в контроллеры на фоновых потоках

**Не делайте этого:** Следующий пример показывает, что `DbContext` замыкание захватывает параметр `Controller` действия. Это плохая практика.  Элемент работы может выполниться за пределами сферы запроса. Область `ContosoDbContext` к запросу, в результате `ObjectDisposedException`чего .

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetSecondController.cs?name=snippet1)]

**Сделайте это:** Следующий пример:

* Вводит для <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> создания области в элементе фоновой работы. `IServiceScopeFactory`является синглтон.
* Создает новую область впрыска зависимости в фоновом потоке.
* Не ссылается на что-либо от контроллера.
* Не захватывает `ContosoDbContext` от входящего запроса.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetSecondController.cs?name=snippet2)]

Следующий выделенный код:

* Создает область для продолжительности службы фоновой операции и разрешает службы из нее.
* Использует `ContosoDbContext` из правильной области.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetSecondController.cs?name=snippet2&highlight=9-16)]

## <a name="do-not-modify-the-status-code-or-headers-after-the-response-body-has-started"></a>Не изменяйте код состояния или заголовки после запуска тела ответа

ASP.NET Core не буфер амплина ЦИММ. Первый раз, когда ответ написан:

* Заголовки отправляются вместе с этим куском тела клиенту.
* Изменить заголовки ответов уже невозможно.

**Не делайте этого:** Следующий код пытается добавить заголовки ответов после того, как ответ уже начался:

[!code-csharp[](performance-best-practices/samples/3.0/Startup22.cs?name=snippet1)]

В предыдущем коде будет сделано исключение, `context.Response.Headers["test"] = "test value";` если `next()` он написал ответ.

**Сделайте это:** Следующий пример проверяет, запущен ли ответ HTTP перед изменением заголовков.

[!code-csharp[](performance-best-practices/samples/3.0/Startup22.cs?name=snippet2)]

**Сделайте это:** Следующий пример `HttpResponse.OnStarting` используется для установки заголовков до того, как заголовки ответов будут смыты к клиенту.

Проверка того, не запущен ли ответ, позволяет зарегистрировать обратный вызов, который будет вызываться непосредственно перед написанием заголовков ответов. Проверка, не начался ли ответ:

* Предоставляет возможность приложения или переопределения заголовков как раз вовремя.
* Не требует знания следующего промежуточного посуды в конвейере.

[!code-csharp[](performance-best-practices/samples/3.0/Startup22.cs?name=snippet3)]

## <a name="do-not-call-next-if-you-have-already-started-writing-to-the-response-body"></a>Не звоните в следующий(), если вы уже начали писать в орган реагирования

Компоненты будут вызываться только в том случае, если они могут обрабатывать и манипулировать ответом.
