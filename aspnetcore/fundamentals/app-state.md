---
title: Состояние сеанса и приложения в ASP.NET Core
author: rick-anderson
description: Методы для сохранения состояния приложения и пользователя (сеанса) между запросами.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/app-state
ms.openlocfilehash: 887aefdeaa45957f7b95bfe8df342eb34d267e3a
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/18/2018
---
# <a name="session-and-application-state-in-aspnet-core"></a>Состояние сеанса и приложения в ASP.NET Core

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson), [Стив Смит](https://ardalis.com/) (Steve Smith) и [Диана Лароуз](https://github.com/DianaLaRose) (Diana LaRose)

HTTP — это протокол без отслеживания состояния. Веб-сервер обрабатывает каждый HTTP-запрос независимо и не сохраняет пользовательские значения из предыдущих запросов. Эта статья описывает различные способы для сохранения состояния приложения и сеанса между запросами.

## <a name="session-state"></a>Состояние сеанса

Состояние сеанса — это функция в ASP.NET Core, которую можно использовать для сохранения пользовательских данных во время просмотра веб-приложения пользователем. Состояние сеанса, состоящее из словаря или хэш-таблицы на сервере, сохраняет данные между запросами из браузера. Данные сеанса хранятся в кэше.

ASP.NET Core сохраняет состояние сеанса, предоставляя клиенту файл cookie, содержащий идентификатор сеанса, который отправляется на сервер вместе с каждым запросом. Сервер использует этот идентификатор для получения данных сеанса. Так как файл cookie сеанса относится к конкретному браузеру, обеспечить общий доступ к сеансам из разных браузеров невозможно. Файлы cookie сеанса удаляются только при завершении сеанса браузера. Если получен файл cookie для просроченного сеанса, создается сеанс, использующий этот файл.

Сервер хранит сеанс некоторое время после последнего запроса. Установите время ожидания сеанса или используйте значение по умолчанию, равное 20 минутам. Состояние сеанса оптимально подходит для сохранения пользовательских данных, относящихся к определенному сеансу, которые не требуется хранить бессрочно. Данные удаляются из резервного хранилища либо при вызове `Session.Clear`, либо после окончания срока действия сеанса в хранилище данных. Серверу неизвестно, когда закрыт браузер или удален файл cookie сеанса.

> [!WARNING]
> Не храните конфиденциальные данные в сеансе. Клиент может не закрыть браузер и не удалить файл cookie сеанса (а некоторые браузеры используют файлы cookie сеанса в разных окнах). Кроме того, сеанс может быть не ограничен одним пользователем, то есть следующий пользователь продолжит работу с тем же сеансом.

Поставщик сеансов в памяти сохраняет данные сеанса на локальном сервере. Если вы планируете запустить веб-приложение на ферме серверов, нужно использовать прикрепленные сеансы, чтобы связать каждый сеанс с определенным сервером. По умолчанию платформа "Веб-сайты Microsoft Azure" использует прикрепленные сеансы (это называется маршрутизацией запросов приложений или ARR). Однако прикрепленные сеансы могут негативно повлиять на масштабируемость и усложнить обновление веб-приложений. Лучше использовать распределенные кэши SQL Server или Redis, для которых прикрепленные сеансы не требуются. Дополнительные сведения см. в разделе [Работа с распределенным кэшем](xref:performance/caching/distributed). Дополнительные сведения о настройке поставщиков услуг см. в разделе [Настройка сеанса](#configuring-session) ниже.

## <a name="tempdata"></a>TempData

ASP.NET Core MVC позволяет использовать свойство [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) в [контроллере](/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0). Это свойство хранит данные до тех пор, пока они не будут прочитаны. Для проверки данных без удаления можно использовать методы `Keep` и `Peek`. `TempData` особенно удобно использовать для перенаправления, когда данные требуются больше чем для одного запроса. `TempData` реализуется поставщиками TempData, например с помощью файлов cookie или состояния сеанса.

### <a name="tempdata-providers"></a>Поставщики TempData

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

В ASP.NET Core 2.0 и более поздних версий поставщик TempData, основанный на файлах cookie, используется по умолчанию для хранения TempData в файлах cookie.

Данные в файле cookie шифруются с помощью [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), кодируются с помощью [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), а затем фрагментируются. Так как файл cookie фрагментируется, ограничение на размер одного такого файла, заданное в ASP.NET Core 1.x, не применяется. Данные файла cookie не сжимаются, так как сжатие зашифрованных данных может привести к проблемам с безопасностью, например атакам [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) и [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)). Дополнительные сведения на поставщике TempData, основанном на файлах cookie, см. в разделе [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

В ASP.NET Core 1.0 и 1.1 для состояния сеанса по умолчанию используется поставщик TempData.

---

### <a name="choosing-a-tempdata-provider"></a>Выбор поставщика TempData

Выбор поставщика TempData включает в себя несколько аспектов:

1. Использует ли приложение состояние сеанса для других целей? Если это так, использование поставщика TempData для состояния сеанса не требует от приложения никаких дополнительных издержек (кроме объема данных).
2. Приложение использует TempData лишь ограниченно, для сравнительно небольших объемов данных (до 500 байт)? Если это так, поставщик TempData на основе файлов cookie будет добавлять небольшие издержки для каждого запроса, переносящего TempData. В противном случае поставщик TempData для состояния сеанса удобно использовать, чтобы устранить круговой обход большого объема данных в каждом запросе до полного использования TempData.
3. Приложение выполняется на веб-ферме (нескольких серверах)? Если это так, для использования поставщика TempData на основе файлов cookie дополнительная настройка не требуется.

> [!NOTE]
> Большинство веб-клиентов (например, веб-браузеры) налагают ограничение на максимальный размер каждого файла cookie и (или) общее количество таких файлов. Поэтому при использовании поставщика TempData на основе файлов cookie убедитесь, что приложение не нарушает эти ограничения. При оценке учитывайте общий объем данных, включающий в себя затраты на шифрование и фрагментацию.

### <a name="configure-the-tempdata-provider"></a>Настройка поставщика TempData

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Поставщик TempData на основе файлов cookie включен по умолчанию. Следующий код класса `Startup` настраивает поставщик TempData на основе сеансов:

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Следующий код класса `Startup` настраивает поставщик TempData на основе сеансов:

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

Для компонентов промежуточного слоя критически важен порядок. В предыдущем примере исключение типа `InvalidOperationException` возникает при вызове `UseSession` после `UseMvcWithDefaultRoute`. Дополнительные сведения см. в разделе [Порядок ПО промежуточного слоя](xref:fundamentals/middleware/index#ordering).

> [!IMPORTANT]
> Если вы ориентируетесь на .NET Framework и используете поставщик на основе сеансов, добавьте в проект пакет NuGet [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session).

## <a name="query-strings"></a>Строки запроса

Вы можете передать ограниченный объем данных из одного запроса в другой, добавив их в строку запроса нового запроса. Это удобно для захвата состояния в сохраняемом виде, что позволяет обмениваться ссылками с внедренным состоянием по электронной почте или через социальные сети. Однако по этой причине никогда не следует использовать строки запроса для конфиденциальных данных. Кроме удобного общего доступа, включение данных в строки запроса может открыть возможности для атак с [подделкой межсайтовых запросов (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)), которые могут обманом вынудить пользователей посещать вредоносные веб-сайты во время проверки подлинности. Это позволит злоумышленникам похитить данные пользователей из приложения или предпринимать вредоносные действия от их имени. Любое сохраненное состояние приложения или сеанса необходимо защитить от атак CSRF. Дополнительные сведения об атаках CSRF см. в статье [Предотвращение атак с подделкой межсайтовых запросов (XSRF/CSRF)](xref:security/anti-request-forgery).

## <a name="post-data-and-hidden-fields"></a>Публикация данных и скрытые поля

Данные можно сохранить в скрытых полях формы и опубликовать обратно при следующем запросе. Это типичное поведение для многостраничных форм. Однако так как клиент может незаконно изменить данные, сервер всегда должен перепроверять их.

## <a name="cookies"></a>Файлы cookie

Файлы cookie позволяют сохранять данные для конкретного пользователя в веб-приложениях. Так как файлы cookie отправляются с каждым запросом, их размер нужно свести к минимуму. В идеальном случае файл cookie должен содержать лишь идентификатор, а сами данные — храниться на сервере. Большинство браузеров позволяют использовать файлы cookie размером до 4096 байт. Кроме того, для каждого домена доступно лишь ограниченное число файлов cookie.

Так как файлы cookie могут быть незаконно изменены, их нужно проверять на сервере. Несмотря на то, что надежность файлов cookie на клиенте зависит от вмешательства пользователя и срока действия, обычно они являются наиболее надежной формой сохранения данных на клиенте.

Файлы cookie часто используются для персонализации, когда содержимое настраивается под известного пользователя. Так как в большинстве случаев пользователь лишь идентифицируется, но не проходит проверку подлинности, можно защитить файл cookie, сохранив в нем имя пользователя, имя учетной записи или уникальный идентификатор пользователя (например, GUID). Затем можно использовать этот файл для доступа к инфраструктуре персонализации пользователя на веб-сайте.

## <a name="httpcontextitems"></a>HttpContext.Items

Коллекция `Items` хорошо подходит для хранения данных, которые требуются только при обработке одного конкретного запроса. Ее содержимое удаляется после каждого запроса. Коллекцию `Items` лучше всего использовать для обеспечения взаимодействия компонентов или ПО промежуточного слоя, когда они выполняются в разные моменты времени во время обработки запроса и не могут передавать параметры напрямую. Дополнительные сведения см. в разделе [Работа с HttpContext.Items](#working-with-httpcontextitems) ниже.

## <a name="cache"></a>Кэш

Кэширование — это эффективный способ хранения и извлечения данных. Вы можете управлять временем существования кэшированных элементов по времени и другим факторам. Дополнительные сведения о [методах кэширования](../performance/caching/index.md).

## <a name="working-with-session-state"></a>Работа с состоянием сеанса

### <a name="configuring-session"></a>Настройка сеанса

Пакет `Microsoft.AspNetCore.Session` предоставляет ПО промежуточного слоя для управления состоянием сеанса. Чтобы включить сеанс ПО промежуточного слоя для сеансов, `Startup` должен содержать:

- Любой из кэшей памяти [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache), при этом реализация `IDistributedCache` используется в качестве резервного хранилища для сеанса.
- Вызов [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_), требующий пакет NuGet "Microsoft.AspNetCore.Session".
- Вызов [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_).

Следующий код показывает, как настроить поставщик сеансов в памяти.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

Вы можете сослаться на сеанс из `HttpContext` после его установки и настройки.

Если попытаться обратиться к `Session` до вызова `UseSession`, возникает исключение `InvalidOperationException: Session has not been configured for this application or request`.

Если попытаться создать `Session` (то есть без создания файла cookie сеанса) после начала записи в поток `Response`, возникает исключение `InvalidOperationException: The session cannot be established after the response has started`. Это исключение можно найти в журнале веб-сервера, в браузере оно не отображается.

### <a name="loading-session-asynchronously"></a>Асинхронная загрузка сеанса

Поставщик сеансов по умолчанию в ASP.NET Core загружает запись сеанса из базового хранилища [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) в асинхронном режиме только при явном вызове метода [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) перед методами `TryGetValue`, `Set` или `Remove`. Если `LoadAsync` не вызывается первым, базовая запись сеанса загружается синхронно, что может негативно повлиять на возможность масштабирования приложения.

Чтобы принудительно использовать этот режим в приложениях, используйте для реализаций [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) и [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) оболочку из версий, которые выдают исключение, когда метод `LoadAsync` не вызывается перед `TryGetValue`, `Set` или `Remove`. Зарегистрируйте версии оболочки в контейнере служб.

### <a name="implementation-details"></a>Сведения о реализации

Сеанс использует файл cookie для отслеживания и идентификации запросов в одном браузере. По умолчанию этот файл называется ".AspNet.Session" и использует путь "/". Так как по умолчанию файл cookie не указывает домен, он остается недоступным для клиентского сценария на странице (так как `CookieHttpOnly` по умолчанию имеет значение `true`).

Чтобы переопределить значения по умолчанию для сеанса, используйте `SessionOptions`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

Сервер использует свойство `IdleTimeout`, чтобы определить, как долго сеанс может оставаться неактивным до сброса его содержимого. Это свойство не зависит от срока действия файла cookie. Каждый запрос, проходящий через ПО промежуточного слоя сеанса (посредством чтения или записи), сбрасывает это время ожидания.

Так как `Session` является *неблокирующим*, когда два запроса пытаются изменить содержимое сеанса, последний из них переопределяет первый. `Session` реализован в виде *согласованного сеанса*, что означает, что все содержимое хранится вместе. Два запроса, изменяющие разные части сеанса (разные ключи), по-прежнему могут повлиять друг на друга.

### <a name="set-and-get-session-values"></a>Установка и получение значений сеанса

Доступ к сеансу можно получить со страницы или представления Razor через `Context.Session`:

[!code-cshtml[](app-state/sample/src/WebAppSessionDotNetCore2.0App/Views/Home/About.cshtml)]

Доступ к сеансу можно получить из класса `PageModel` или контроллера с `HttpContext.Session`. Оно является реализацией [ISession](/dotnet/api/microsoft.aspnetcore.http.isession).

Следующий пример показывает задание и получение значения int и строки:

[!code-csharp[](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

Добавив приведенные ниже методы расширения, можно задавать и получать сериализуемые объекты для сеанса:

[!code-csharp[](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

Следующий пример показывает, как задать и получить сериализуемый объект:

[!code-csharp[](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]

## <a name="working-with-httpcontextitems"></a>Работа с HttpContext.Items

Абстракция `HttpContext` обеспечивает поддержку для коллекции словаря типа `IDictionary<object, object>`, называемой `Items`. Эта коллекция доступна с начала *HttpRequest* и удаляется в конце каждого запроса. Вы можете обратиться к ней, присвоив значение записи с ключом или запросив значение для определенного ключа.

В следующем примере [ПО промежуточного слоя](xref:fundamentals/middleware/index) добавляет `isVerified` в коллекцию `Items`.

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

Далее в конвейере другое ПО промежуточного слоя может получить к нему доступ:

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " +
        context.Items["isVerified"]);
});
```

Для ПО промежуточного слоя, которое будет использоваться всего одним приложением, допустимы ключи `string`. Однако ПО промежуточного слоя, которое будет совместно использоваться несколькими приложениями, во избежание конфликтов должно использовать уникальные ключи объекта. Если вы разрабатываете ПО промежуточного слоя, которое будет работать с несколькими приложениями, используйте уникальный ключ объекта, определенный в классе ПО промежуточного слоя, как показано ниже:

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

Другой код может обратиться к значению, хранящемуся в `HttpContext.Items`, с помощью ключа, предоставляемого классом ПО промежуточного слоя:

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

Данный подход также позволяет устранить повторение соответствующих строк в нескольких местах в коде.

## <a name="application-state-data"></a>Данные о состоянии приложения

Используйте [внедрение зависимостей](xref:fundamentals/dependency-injection), чтобы сделать данные доступными для всех пользователей:

1. Определите службу, содержащую данные (например, класс с именем `MyAppData`).

    ```csharp
    public class MyAppData
    {
        // Declare properties/methods/etc.
    } 
    ```

2. Добавление класс службы в `ConfigureServices` (например, `services.AddSingleton<MyAppData>();`).

3. Используйте класс службы данных в каждом контроллере:

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

## <a name="common-errors-when-working-with-session"></a>Типичные ошибки при работе с сеансом

* "Unable to resolve service for type "Microsoft.Extensions.Caching.Distributed.IDistributedCache" while attempting to activate "Microsoft.AspNetCore.Session.DistributedSessionStore"" (Не удается разрешить службу для типа "Microsoft.Extensions.Caching.Distributed.IDistributedCache" при попытке активировать "Microsoft.AspNetCore.Session.DistributedSessionStore").

  Обычно она вызвана невозможностью настройки по меньшей мере одной реализации `IDistributedCache`. Дополнительные сведения см. в разделах [Работа с распределенным кэшем](xref:performance/caching/distributed) и [Кэширование в памяти](xref:performance/caching/memory).

* Если ПО промежуточного слоя для сеанса не удается сохранить сеанс (например, когда недоступна база данных), оно регистрирует исключение и поглощает его. После этого запрос продолжает выполняться как обычно, что приводит к очень непредсказуемому поведению.

Рассмотрим типичный пример:

Кто-то сохраняет в сеансе корзину для покупок. Пользователь добавляет элемент, но фиксация завершается сбоем. Приложению неизвестно о сбое, поэтому оно выводит сообщение "Элемент добавлен", хотя это не так.

Для проверки на наличие таких ошибок рекомендуется вызывать `await feature.Session.CommitAsync();` из кода приложения по окончании записи в сеанс. После этого ошибку можно обработать любым удобным вам образом. Это работает аналогично и при вызове `LoadAsync`.

### <a name="additional-resources"></a>Дополнительные ресурсы

* [ASP.NET Core 1.x: пример кода, используемый в этом документе](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [ASP.NET Core 2.x: пример кода, используемый в этом документе](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
