---
title: "Состояние сеанса и приложения в ASP.NET Core"
author: rick-anderson
description: "Методы сохранения приложения и состояние пользователя (сеанс) между запросами."
keywords: "Учет ASP.NET Core, состояние приложения, состояние сеанса, строки запроса,"
ms.author: riande
manager: wpickett
ms.date: 10/08/2017
ms.topic: article
ms.assetid: 18cda488-0769-4cb9-82f6-4c6685f2045d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/app-state
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f9c1d10101d23e105c4a8af41d851f69b1b6a175
ms.sourcegitcommit: 9c27fa0f0c57ad611aa43f63afb9b9c9571d4a94
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a>Общие сведения о состоянии сеанса и приложения в ASP.NET Core

По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Стив Смит](https://ardalis.com/), и [Diana LaRose](https://github.com/DianaLaRose)

Протокол HTTP является протоколом без состояния. Веб-сервер обрабатывает каждый HTTP-запрос как независимый запрос и не сохраняет значения пользователя из предыдущих запросов. В этой статье описаны различные способы сохранить состояние приложения и сеанса между запросами. 

## <a name="session-state"></a>Состояние сеанса

Состояние сеанса — это функция в ASP.NET Core, которую можно использовать для сохранения пользовательских данных во время просмотра веб-приложения пользователем. Состоящий из словаря или хэш-таблицы на сервере, состояние сеанса сохраняет данные для запросов из браузера. Поддерживаемый кэш данных сеанса.

ASP.NET Core сохраняет состояние сеанса, предоставляя файл cookie, который содержит идентификатор сеанса, который отправляется на сервер при каждом запросе клиента. Сервер использует идентификатор сеанса для выборки данных сеанса. Так как файл cookie сеанса является специфичным для браузера, не могут совместно использовать сеансы браузерами. Файлы cookie сеанса, удаляются только в том случае, при завершении сеанса браузера. Если файл cookie поступает просроченного сеанса, создается новый сеанс, в котором используется один и тот же файл cookie сеанса. 

Сервер сохраняет сеанс в течение ограниченного времени после выполнения последнего запроса. Можно задать время ожидания сеанса или использовать значение по умолчанию 20 минут. Состояние сеанса идеально подходит для хранения пользовательских данных, связанную с определенным сеансом, но может не быть сохранен без возможности восстановления. Данные удаляются из резервного хранилища либо при вызове `Session.Clear` или после окончания сеанса в хранилище данных. Сервер не может определить при закрытии браузера или при удалении файла cookie сеанса.

> [!WARNING]
> Не храните конфиденциальные данные в сеансе. Клиент не может закрыть браузер и очистить файл cookie сеанса (и некоторые браузеры сохранения файлов cookie сеанса через windows). Кроме того сеанс не может быть ограничен до одного пользователя; Следующий пользователь может по-прежнему с того же сеанса.

Поставщик сеансов в памяти сохраняет данные сеанса на локальном сервере. Если вы планируете запускать веб-приложения на ферме серверов, необходимо использовать закрепленные сеансы для каждого сеанса к определенному серверу перегрузки. По умолчанию платформа веб-сайтов Windows Azure для прикрепленных сеансов (маршрутизации запросов приложений или ARR). Тем не менее закрепленные сеансы могут повлиять на масштабируемость и усложнить обновления веб-приложений. Лучшим вариантом является использование команды Redis или кэширует распределенных SQL Server, не требующую закрепленных сеансов. Дополнительные сведения см. в разделе [работа с распределенного кэша](xref:performance/caching/distributed). Дополнительные сведения о настройке поставщиков услуг см. в разделе [Настройка сеанса](#configuring-session) далее в этой статье.


<a name="temp"></a>
## <a name="tempdata"></a>TempData

Основные ASP.NET MVC предоставляет [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) свойство [контроллера](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0). Это свойство хранит данные до тех пор, пока они не будут прочитаны. Для проверки данных без удаления можно использовать методы `Keep` и `Peek`. `TempData`особенно полезна для перенаправления, когда данные, необходимые для более чем одного запроса. `TempData`реализуется TempData поставщиков, например, с помощью файлов cookie или с состоянием сеанса.

### <a name="tempdata-providers"></a>Поставщики TempData

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

В ASP.NET Core 2.0 и более поздних версиях поставщика на основе файлов cookie TempData используется по умолчанию для хранения TempData в файлах cookie.

Данные cookie кодируется с [Base64UrlTextEncoder](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0). Один файл cookie, так как файл cookie шифруется и фрагментирован, размером в ASP.NET Core 1.x не применяется ограничение. Данные cookie не сжимается, поскольку сжатие данных encryped может привести к проблемам безопасности таких как [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) и [нарушения](https://wikipedia.org/wiki/BREACH_(security_exploit)) атак. Дополнительные сведения на основе файлов cookie TempData поставщика см. в разделе [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

В ASP.NET Core 1.0 и 1.1 поставщик TempData состояния сеанса используется по умолчанию.

--------------

### <a name="choosing-a-tempdata-provider"></a>Выбор поставщика TempData

Выбор поставщика TempData включает в себя несколько факторов, таких как:

1. Используется ли приложение уже состояния сеанса для других целей? В этом случае использование TempData поставщика состояния сеанса имеет без дополнительной оплаты (помимо объема данных) в приложение.
2. Приложение использует TempData только ограниченно для сравнительно небольших объемов данных (до 500 байт)? Если таким образом, поставщик TempData куки-файл будет добавлен небольшой стоимость каждого запроса, который выполняет TempData. В противном случае TempData поставщика состояния сеанса может быть полезно во избежание циклической обработки большой объем данных в каждом запросе до полного исчерпания TempData.
3. Приложение выполняется на веб-ферме (несколько серверов)? В этом случае имеется дополнительная настройка не требуется для использования поставщика TempData куки-файл.

> [!NOTE]
> Большинство веб-клиентов (например, веб-браузеры) соблюдения ограничений на максимальный размер каждого файла cookie и общее количество файлов cookie. Таким образом при использовании поставщика TempData куки-файл, убедитесь, что приложение не будет превышать эти пределы. Рассмотрим общий объем данных, учитывая накладных расходов, шифрования и фрагментации.

Чтобы настроить поставщик TempData для приложения, зарегистрируйте реализации поставщика TempData в `ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services
        .AddMvc()
        .AddSessionStateTempDataProvider();

    // The Session State TempData Provider requires adding the session state service
    services.AddSession();
}
```

## <a name="query-strings"></a>Строки запросов

Можно передать ограниченный объем данных от одного запроса в другой, добавьте его в строку запроса нового запроса. Это полезно для записи состояния постоянных способом, который позволяет связи внедренные состояния для совместного использования по электронной почте или социальных сетей. Однако по этой причине следует никогда не использовать строки запроса для конфиденциальных данных. Помимо легко общедоступную, включая данные в строках запроса можно создать возможности для [подделкой межсайтовых запросов (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) атак, которые может сбить с толку пользователей в посещении вредоносных веб-узлов во время проверки подлинности. Злоумышленники могут затем похитить данные пользователя из приложения или вредоносных действий от имени пользователя. Любое сохраненное состояние application или session необходимо защититься от атак CSRF. Дополнительные сведения об атаках CSRF см. в разделе [Предотвращение межсайтовой запроса подделки XSRF-атак в ASP.NET Core](../security/anti-request-forgery.md).

## <a name="post-data-and-hidden-fields"></a>Данные POST и скрытые поля

Данные можно сохранить в скрытые поля формы и отправки обратно при следующем запросе. Обычно это происходит в многостраничных форм. Тем не менее поскольку клиент потенциально могут изменять данные, сервер должен всегда тщательно проверяйте его. 

## <a name="cookies"></a>Файлы cookie

Файлы cookie предоставляют способ хранения данных конкретного пользователя в веб-приложениях. Поскольку файлы cookie, отправленных с каждым запросом, их размера должна быть сведена к минимуму. В идеальном случае только идентификатор должны храниться в файле cookie с текущими данными, хранящимися на сервере. Большинство браузеров ограничить файлы Cookie до 4096 байт. Кроме того для каждого домена доступны только ограниченное число файлов cookie.  

Поскольку файлы cookie могут быть прочитаны, они должны быть проверены на сервере. Несмотря на то, что надежность куки-файл на компьютере клиента регулируется вмешательства пользователя и срок действия, они обычно являются более долговременной формой сохранения данных на стороне клиента.

Файлы cookie часто используются для персонализации, если содержимое настраивается для известного пользователя. Так как только после определения и не прошел проверку подлинности, в большинстве случаев пользователь, обычно можно защитить файл cookie путем сохранения имени пользователя, имя учетной записи или уникальный ID пользователя (например, GUID) в файле cookie. Затем можно использовать файл cookie для доступа к персональной инфраструктуре веб-узла.

## <a name="httpcontextitems"></a>HttpContext.Items

`Items` Коллекция имеет хорошее расположение для хранения данных, надобности только во время обработки одного конкретного запроса. Содержимое коллекции удаляются после каждого запроса. `Items` Коллекции лучше всего использовать как способ для компонентов или по промежуточного слоя для обмена данными, если они работают в разные моменты времени, во время запроса и имеют прямой способ передачи параметров. Дополнительные сведения см. в разделе [работа с HttpContext.Items](#working-with-httpcontextitems)далее в этой статье.

## <a name="cache"></a>Кэш

Кэширование — эффективный способ хранения и извлечения данных. Можно управлять временем жизни кэшированных элементов на основе времени и другие вопросы. Дополнительные сведения о [кэширование](../performance/caching/index.md).

<a name=session></a>
## <a name="working-with-session-state"></a>Работа с состоянием сеанса

### <a name="configuring-session"></a>Настройка сеанса

`Microsoft.AspNetCore.Session` Пакет предоставляет по промежуточного слоя для управления состоянием сеанса. Чтобы включить сеанс по промежуточного слоя, `Startup`должен содержать:

- Любой из [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) кэшей памяти. `IDistributedCache` Реализация используется в качестве резервного хранилища для сеанса.
- [AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) вызов, требующий пакет NuGet «Microsoft.AspNetCore.Session».
- [UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) вызова.

Ниже показано, как для настройки поставщика сеансов в памяти.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

Можно ссылаться на сеанс из `HttpContext` после его установки и настройки.

Если при попытке доступа к `Session` перед `UseSession` был вызван, исключение `InvalidOperationException: Session has not been configured for this application or request` возникает исключение.

При попытке создать новый `Session` (то есть, объекты cookie сеанса не был создан) после начала записи в `Response` потока, исключение `InvalidOperationException: The session cannot be established after the response has started` возникает исключение. Исключение, можно найти в журнале веб-сервера; он не будет отображаться в браузере.

### <a name="loading-session-asynchronously"></a>Асинхронная загрузка сеанса 

Поставщик сеанса по умолчанию в ASP.NET Core запись сеанса загружается из основного [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) хранилище асинхронно только тогда, когда [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) до явного вызова метода  `TryGetValue`, `Set`, или `Remove` методы. Если `LoadAsync` не вызывается во-первых, базовый сеанс записи загружается синхронно, который могут повлиять на возможность масштабирования приложения.

Чтобы применить этот шаблон приложения, перенос [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) и [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) реализации с версиями, если исключение `LoadAsync` метод не является Вызывается перед `TryGetValue`, `Set`, или `Remove`. Зарегистрируйте оболочку версии в контейнере службы.

### <a name="implementation-details"></a>Сведения о реализации

Сеанс использует файл cookie для отслеживания и идентификации запросов из одного браузера. По умолчанию этот файл cookie с именем «. AspNet.Session», который использует путь «/». Так как файл cookie по умолчанию домен не указан, он не становится доступным клиентский сценарий на странице (поскольку `CookieHttpOnly` по умолчанию используется значение `true`).

Чтобы переопределить значения по умолчанию сеанс, используйте `SessionOptions`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

Сервер использует `IdleTimeout` свойства, чтобы определить, как долго сеанс может быть неактивным до его содержимое оставляются. Это свойство не зависит от срок действия файла cookie. Каждый запрос, который проходит через по промежуточного слоя сеанса (чтение и запись для данных) сбрасывает время ожидания.

Поскольку `Session` — *неблокирующую*, если два запроса как попытка изменить содержимое сеанса последним переопределяет первый. `Session`реализуется в виде *согласованного сеанса*, что означает, что все содержимое хранятся вместе. Два запроса, изменяющие различные части сеанса (разные ключи) по-прежнему могут повлиять друг на друга.

### <a name="setting-and-getting-session-values"></a>Настройка и получение значения сеанса

Сеанс осуществляется с помощью `Session` свойство `HttpContext`. Это свойство является [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) реализации.

В следующем примере показано задание и получение int и строку:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet1)]

При добавлении следующие методы расширения, можно задать и получить сериализуемые объекты для сеанса:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

Следующий пример показано, как задать и получить сериализуемый объект:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a>Работа с HttpContext.Items

`HttpContext` Абстракции обеспечивает поддержку для коллекции словаря типа `IDictionary<object, object>`, который называется `Items`. Эта коллекция доступна с начала *HttpRequest* и удаляются в конце каждого запроса. Его можно открыть, путем присвоения значения в запись с ключом, или с запросом на ввод значения для определенного ключа.

В приведенном ниже примере [по промежуточного слоя](middleware.md) добавляет `isVerified` для `Items` коллекции.

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

Далее в конвейере другого по промежуточного слоя может получить доступ к его:

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

По промежуточного слоя, который будет использовать только одно приложение `string` ключи являются допустимыми. Тем не менее по промежуточного слоя, который будет совместно использоваться приложениями следует использовать уникальный объект во избежание любой вероятность возникновения конфликтов. При разработке по промежуточного слоя, который будет работать с несколькими приложениями, используйте уникальный объект ключа, определенных в классе по промежуточного слоя, как показано ниже:

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

Другой код может получить доступ к значение, хранящееся в `HttpContext.Items` с помощью ключа, предоставляемые классом по промежуточного слоя:

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

Данный подход также имеет преимущество устраняя повторение «magic строки» в нескольких местах в коде.

<a name=appstate-errors></a>

## <a name="application-state-data"></a>Данные состояния приложения

Используйте [внедрения зависимостей](xref:fundamentals/dependency-injection) чтобы сделать данные доступными для всех пользователей:

1. Определения службы, содержащий данные (например, класс с именем `MyAppData`).

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. Добавление класса службы для `ConfigureServices` (например `services.AddSingleton<MyAppData>();`).
3. Использование класса службы данных в каждом контроллере:

```csharp
public class MyController : Controller
{
    public MyController(MyAppData myService)
    {
        // Do something with the service (read some data from it, 
        // store it in a private field/property, etc.)
    }
} 
```

### <a name="common-errors-when-working-with-session"></a>Распространенные ошибки при работе с сеансом

* «Не удается разрешить службу типа «Microsoft.Extensions.Caching.Distributed.IDistributedCache» при попытке активации «Microsoft.AspNetCore.Session.DistributedSessionStore»».

  Обычно это вызвано неправильная настройка по крайней мере один `IDistributedCache` реализации. Дополнительные сведения см. в разделе [работа с распределенного кэша](xref:performance/caching/distributed) и [в кэширование памяти](xref:performance/caching/memory).

### <a name="additional-resources"></a>Дополнительные ресурсы


* [ASP.NET Core 1.x: пример кода, используемого в данном документе](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [ASP.NET Core 2.x: пример кода, используемого в данном документе](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
