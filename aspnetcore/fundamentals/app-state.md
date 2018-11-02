---
title: Состояние сеанса и приложения в ASP.NET Core
author: rick-anderson
description: Методы обнаружения для сохранения состояния сеанса и приложения между запросами.
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2018
uid: fundamentals/app-state
ms.openlocfilehash: da20538a0dc6e13caedaf6a1130e66981dcb7af2
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207294"
---
# <a name="session-and-app-state-in-aspnet-core"></a>Состояние сеанса и приложения в ASP.NET Core

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson), [Стив Смит](https://ardalis.com/) (Steve Smith), [Диана Лароуз](https://github.com/DianaLaRose) (Diana LaRose) и [Люк Лэтэм](https://github.com/guardrex) (Luke Latham)

HTTP — это протокол без отслеживания состояния. Без дополнительных действий HTTP-запросы являются независимыми сообщениями, которые не сохраняют значения пользователя или состояние приложения. В этой статье описывается несколько подходов для сохранения данных пользователя и состояния приложения между запросами.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="state-management"></a>Управление состоянием

Состояние можно сохранить несколькими способами. Эти способы обсуждается ниже в данном разделе.

| Способ хранения данных | Механизм хранения |
| ---------------- | ----------------- |
| [Файлы "cookie"](#cookies) | Файлы cookie HTTP (могут содержать данные, сохраненные с помощью кода приложения на стороне сервера) |
| [Состояние сеанса](#session-state) | Файлы cookie HTTP и код приложения на стороне сервера |
| [TempData](#tempdata) | Файлы cookie HTTP или состояние сеанса |
| [Строки запросов](#query-strings) | Строки запросов HTTP |
| [Скрытые поля](#hidden-fields) | Поля формы HTTP |
| [HttpContext.Items](#httpcontextitems) | Код приложения на стороне сервера |
| [Кэш](#cache) | Код приложения на стороне сервера |
| [Введение зависимостей](#dependency-injection) | Код приложения на стороне сервера |

## <a name="cookies"></a>Файлы cookie

Файлы cookie хранят данные между запросами. Так как файлы cookie отправляются с каждым запросом, их размер нужно свести к минимуму. В идеальном случае файл cookie должен содержать лишь идентификатор, а сами данные хранятся в приложении. Большинство браузеров позволяют использовать файлы cookie размером до 4096 байт. Для каждого домена доступно лишь ограниченное число файлов cookie.

Так как файлы cookie могут быть незаконно изменены, их нужно проверять в приложении. Файлы cookie могут удаляться пользователем и имеют ограниченный срок хранения на клиентах. Тем не менее файлы cookie обычно являются самым надежным способом хранения данных на стороне клиента.

Файлы cookie часто используются для персонализации, когда содержимое настраивается под известного пользователя. В большинстве случаев пользователь проходит только идентификацию, а не проверку подлинности. Файл cookie может хранить имя пользователя, имя учетной записи или уникальный идентификатор пользователя (например, GUID). Затем можно использовать файл cookie для доступа к личным настройкам пользователя, таким как цвет фона на веб-сайте.

Помните об [Общем регламенте по защите данных в ЕС (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection) при создании файлов cookie и решении вопросов с конфиденциальностью. Дополнительные сведения см. в разделе [Общий регламент по защите данных (GDPR), принятый в ЕС, в ASP.NET Core](xref:security/gdpr).

## <a name="session-state"></a>Состояние сеанса

Состояние сеанса — это сценарий ASP.NET Core для хранения пользовательских данных, когда пользователь находится в веб-приложении. Состояние сеанса использует хранилище, обслуживаемое приложением, для сохранения данных между запросами от клиента. Данные сеанса поддерживаются кэшем и считаются временными, &mdash; сайт должен работать и без данных сеанса.

> [!NOTE]
> Сеанс не поддерживается в приложениях [SignalR](xref:signalr/index), так как [хаб SignalR](xref:signalr/hubs) может выполняться независимо от контекста HTTP. Например, это может произойти, если хаб поддерживает длительный запрос на опрос открытым, когда время существования контекста HTTP для запроса уже истекло.

ASP.NET Core сохраняет состояние сеанса, предоставляя клиенту файл cookie, содержащий идентификатор сеанса, который отправляется в приложение вместе с каждым запросом. Приложение использует этот идентификатор для получения данных сеанса.

Состояние сеанса реагирует на события следующим образом:

* Так как файл cookie сеанса относится к конкретному браузеру, обеспечить общий доступ к сеансам из разных браузеров невозможно.
* Файлы cookie сеанса удаляются при завершении сеанса браузера.
* Если получен файл cookie для просроченного сеанса, создается сеанс, использующий этот же файл.
* Пустые сеансы не сохраняются &mdash; в сеансе должно быть хотя бы одно значение, чтобы его можно быть сохранить между запросами. Если сеанс не сохраняется, для каждого нового запроса создается новый идентификатор сеанса.
* Приложение хранит сеанс некоторое время после последнего запроса. Приложение устанавливает время ожидания сеанса или использует значение по умолчанию, равное 20 минутам. Состояние сеанса оптимально подходит для сохранения пользовательских данных, относящихся к определенному сеансу, которые не требуется хранить бессрочно для нескольких сеансов.
* Данные сеанса удаляются при вызове реализации [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) или когда истекает срок действия сеанса.
* Не существует механизма по умолчанию, который бы информировал код приложения о том, что браузер клиента закрыт или файл cookie сеанса удален или просрочен на клиенте.

> [!WARNING]
> Не храните конфиденциальные данные в состоянии сеанса. Пользователь может не закрывать браузер и очистить файл cookie сеанса. Некоторые браузеры сохраняют файлы cookie сеанса в нескольких окнах браузера. Сеанс может быть не ограничен одним пользователем &mdash; следующий пользователь продолжит использовать приложение с тем же файлом cookie сеанса.

Поставщик кэша в памяти хранит данные сеанса в памяти сервера, содержащего приложение. В сценарии с фермой серверов:

* Используйте *прикрепленные сеансы* для привязки сеанса к конкретному экземпляру приложения на отдельном сервере. [Служба приложений Azure](https://azure.microsoft.com/services/app-service/) использует [маршрутизацию запросов приложений (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module), чтобы прикрепленные сеансы создавались по умолчанию. Однако прикрепленные сеансы могут негативно повлиять на масштабируемость и усложнить обновление веб-приложений. Лучше использовать распределенные кэши SQL Server или Redis, для которых прикрепленные сеансы не требуются. Дополнительные сведения см. в разделе <xref:performance/caching/distributed>.
* Файл cookie сеанса шифруется через [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector). Необходимо правильно настроить защиту данных, чтобы читать файлы cookie сеанса на каждом компьютере. Дополнительные сведения см. в разделе [Защита данных в ASP.NET Core](xref:security/data-protection/index) и [Поставщики хранилища ключей](xref:security/data-protection/implementation/key-storage-providers).

### <a name="configure-session-state"></a>Настройка состояния сеанса

::: moniker range=">= aspnetcore-2.0"

Пакет [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/), который входит в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), предоставляет ПО промежуточного слоя для управления состоянием сеанса. Чтобы включить сеанс ПО промежуточного слоя для сеансов, `Startup` должен содержать:

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Пакет [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) предоставляет ПО промежуточного слоя для управления состоянием сеанса. Чтобы включить сеанс ПО промежуточного слоя для сеансов, `Startup` должен содержать:

::: moniker-end

* Любой из кэшей памяти [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache), при этом реализация `IDistributedCache` используется в качестве резервного хранилища для сеанса. Дополнительные сведения см. в разделе <xref:performance/caching/distributed>.
* Вызов [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) в `ConfigureServices`.
* Вызов [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) в `Configure`.

Следующий код показывает, как настроить поставщик сеансов в памяти с реализацией в памяти `IDistributedCache` по умолчанию:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13-18,39)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5,7-12,19)]

::: moniker-end

Порядок ПО промежуточного слоя важен. В предыдущем примере исключение `InvalidOperationException` возникает при вызове `UseSession` после `UseMvc`. Дополнительные сведения см. в разделе [Порядок расположения ПО промежуточного слоя](xref:fundamentals/middleware/index#order).

После настройки состояния сеанса доступно свойство [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session).

Невозможно получить доступ к `HttpContext.Session` до вызова `UseSession`.

Невозможно создать новый сеанс с новым файлом cookie сеанса после того, как приложение начинает запись в поток ответа. Исключение записывается в журнал веб-сервера и не отображается в браузере.

### <a name="load-session-state-asynchronously"></a>Асинхронная загрузка состояния сеанса

Поставщик сеансов по умолчанию в ASP.NET Core загружает запись сеанса из резервного хранилища [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) в асинхронном режиме только при явном вызове метода [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) перед методами [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [Set](/dotnet/api/microsoft.aspnetcore.http.isession.set) или [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.remove). Если `LoadAsync` не вызывается первым, базовая запись сеанса загружается синхронно, что может негативно повлиять на производительность.

Чтобы принудительно использовать этот режим в приложениях, используйте для реализаций [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) и [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) оболочку из версий, которые выдают исключение, когда метод `LoadAsync` не вызывается перед `TryGetValue`, `Set` или `Remove`. Зарегистрируйте версии оболочки в контейнере служб.

### <a name="session-options"></a>Параметры сеанса

Чтобы переопределить значения по умолчанию для сеанса, используйте [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).

::: moniker range=">= aspnetcore-2.0"

| Параметр | Описание: |
| ------ | ----------- |
| [Файл cookie](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookie) | Определяет параметры, используемые для создания файлов cookie. Параметр [Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) по умолчанию имеет значение [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`). Параметр [Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) по умолчанию имеет значение [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`). Параметр [SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) по умолчанию имеет значение [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`). Параметр [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) по умолчанию имеет значение `true`. Параметр [IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) по умолчанию имеет значение `false`. |
| [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | `IdleTimeout` указывает, как долго сеанс может быть неактивным, прежде чем его содержимое отбрасывается. Каждый доступ к сеансу сбрасывает время ожидания. Этот параметр применяется только к содержимому сеанса, а не к файлу cookie. Значение по умолчанию — 20 минут. |
| [IOTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.iotimeout) | Максимальное количество времени для загрузки сеанса из хранилища или его сохранения обратно в хранилище. Этот параметр может применяться только к асинхронным операциям. Время ожидания можно отключить с помощью [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan). Значение по умолчанию — 1 минута. |

Сеанс использует файл cookie для отслеживания и идентификации запросов в одном браузере. По умолчанию этот файл cookie называется `.AspNetCore.Session` и использует путь `/`. Так как по умолчанию файл cookie не указывает домен, он остается недоступным для клиентского сценария на странице (так как [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) по умолчанию имеет значение `true`).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| Параметр | Описание: |
| ------ | ----------- |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiedomain) | Определяет домен, используемый для создания файла cookie. `CookieDomain` не устанавливается по умолчанию. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiehttponly) | Определяет, будет ли браузер предоставлять доступ к файлу cookie для JavaScript на стороне клиента. Значение по умолчанию — `true`, то есть файл cookie передается только HTTP-запросам и не доступен для скрипта на странице. |
| [CookieName](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiename) | Определяет имя файла cookie, используемое для хранения идентификатора сеанса. Значение по умолчанию — [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`). |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiepath) | Определяет путь, используемый для создания файла cookie. Значение по умолчанию — [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`). |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiesecure) | Определяет, должен ли файл cookie передаваться только по HTTPS-запросу. Значение по умолчанию — [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`). |
| [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | `IdleTimeout` указывает, как долго сеанс может быть неактивным, прежде чем его содержимое отбрасывается. Каждый доступ к сеансу сбрасывает время ожидания. Это относится только к содержимому сеанса, а не к файлу cookie. Значение по умолчанию — 20 минут. |

Сеанс использует файл cookie для отслеживания и идентификации запросов в одном браузере. По умолчанию этот файл cookie называется `.AspNet.Session` и использует путь `/`.

::: moniker-end

Чтобы переопределить файл cookie по умолчанию для сеанса, используйте `SessionOptions`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=13-18)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5-9)]

::: moniker-end

Приложение использует свойство [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout), чтобы определить, как долго сеанс может оставаться неактивным до сброса его содержимого в кэше сервера. Это свойство не зависит от срока действия файла cookie. Каждый запрос, проходящий через [ПО промежуточного слоя сеанса](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware), сбрасывает это время ожидания.

Состояние сеанса является *неблокирующим*. Когда два запроса пытаются изменить содержимое сеанса, последний из них переопределяет первый. `Session` реализован в виде *согласованного сеанса*, что означает, что все содержимое хранится вместе. Когда два запроса пытаются изменить разные значения сеанса, последний запрос может переопределить изменения, внесенные первым.

### <a name="set-and-get-session-values"></a>Установка и получение значений сеанса

Получить доступ к состоянию сеанса можно через класс Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) или класс MVC [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) со свойством [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session). Оно является реализацией [ISession](/dotnet/api/microsoft.aspnetcore.http.isession).

::: moniker range=">= aspnetcore-2.0"

Реализация `ISession` предоставляет несколько методов расширения для задания и извлечения значений integer и string. Методы расширения находятся в пространстве имен [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) (добавьте оператор `using Microsoft.AspNetCore.Http;`, чтобы получить доступ к методам расширения), когда проект ссылается на пакет [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/). Оба пакета входят в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Реализация `ISession` предоставляет несколько методов расширения для задания и извлечения значений integer и string. Методы расширения находятся в пространстве имен [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) (добавьте оператор `using Microsoft.AspNetCore.Http;`, чтобы получить доступ к методам расширения), когда проект ссылается на пакет [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/).

::: moniker-end

Методы расширения `ISession`:

* [Get(ISession, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.get)
* [GetInt32(ISession, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getint32)
* [GetString(ISession, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getstring)
* [SetInt32(ISession, String, Int32)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setint32)
* [SetString(ISession, String, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setstring)

В следующем примере извлекается значение сеанса для ключа `IndexModel.SessionKeyName` (`_Name` в примере приложения) на странице Razor Pages:

```csharp
@page
@using Microsoft.AspNetCore.Http
@model IndexModel

...

Name: @HttpContext.Session.GetString(IndexModel.SessionKeyName)
```

В следующем примере показано, как задать, а затем получить значения integer и string:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet1&highlight=10-11,18-19)]

::: moniker-end

Все данные сеанса должны быть сериализованы, чтобы использовать сценарий-распределенного кэша даже при использовании кэша в памяти. Предоставляются минимальные сериализаторы строки и числа (см. методы и методы расширения [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)). Сложные типы должны быть сериализованы пользователем с помощью другого механизма, например JSON.

Добавьте приведенные ниже методы расширения, чтобы задавать и получать сериализуемые объекты:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

Следующий пример показывает, как задать и получить сериализуемый объект с помощью методов расширения:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet2&highlight=4,12)]

::: moniker-end

## <a name="tempdata"></a>TempData

ASP.NET Core предоставляет [свойство TempData модели страницы Razor Pages](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) или [TempData контроллера MVC](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata). Это свойство хранит данные до тех пор, пока они не будут прочитаны. Для проверки данных без удаления можно использовать методы [Keep](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) и [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek). TempData особенно удобно использовать для перенаправления, когда данные требуются больше чем для одного запроса. TempData реализуется поставщиками TempData, например с помощью файлов cookie или состояния сеанса.

### <a name="tempdata-providers"></a>Поставщики TempData

::: moniker range=">= aspnetcore-2.0"

В ASP.NET Core 2.0 и более поздних версий поставщик TempData, основанный на файлах cookie, используется по умолчанию для хранения TempData в файлах cookie.

Данные в файле cookie шифруются с помощью [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), кодируются с помощью [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), а затем фрагментируются. Так как файл cookie фрагментируется, ограничение на размер одного такого файла, заданное в ASP.NET Core 1.x, не применяется. Данные файла cookie не сжимаются, так как сжатие зашифрованных данных может привести к проблемам с безопасностью, например атакам [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) и [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)). Дополнительные сведения на поставщике TempData, основанном на файлах cookie, см. в разделе [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

В ASP.NET Core 1.0 и 1.1 для состояния сеанса по умолчанию используется поставщик TempData.

::: moniker-end

### <a name="choose-a-tempdata-provider"></a>Выбор поставщика TempData

Выбор поставщика TempData включает в себя несколько аспектов:

1. Приложение уже использует состояние сеанса? Если это так, использование поставщика TempData для состояния сеанса не требует от приложения никаких дополнительных издержек (кроме объема данных).
2. Приложение использует TempData лишь ограниченно, для сравнительно небольших объемов данных (до 500 байт)? Если это так, поставщик TempData на основе файлов cookie незначительно увеличивает издержки для каждого запроса, переносящего TempData. В противном случае поставщик TempData для состояния сеанса удобно использовать, чтобы устранить круговой обход большого объема данных в каждом запросе до полного использования TempData.
3. Приложение выполняется на ферме серверов на нескольких серверах? Если это так, не требуется дополнительная настройка для использования поставщика TempData для файлов cookie, за исключением защиты данных (см. разделы [Защита данных](xref:security/data-protection/index) и [Поставщики хранилища ключей](xref:security/data-protection/implementation/key-storage-providers)).

> [!NOTE]
> Большинство веб-клиентов (например, веб-браузеры) налагают ограничение на максимальный размер каждого файла cookie и (или) общее количество таких файлов. При использовании поставщика TempData на основе файлов cookie убедитесь, что приложение не нарушает эти ограничения. Учитывайте общий объем данных. Учитывайте увеличение размеров файла cookie в результате шифрования и фрагментирования.

### <a name="configure-the-tempdata-provider"></a>Настройка поставщика TempData

::: moniker range=">= aspnetcore-2.0"

Поставщик TempData на основе файлов cookie включен по умолчанию.

Чтобы включить поставщик TempData на основе сеанса, используйте метод расширения [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider):

[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Следующий код класса `Startup` настраивает поставщик TempData на основе сеансов:

[!code-csharp[](app-state/samples_snapshot_2/1.x/SessionSample/Startup.cs?name=snippet1&highlight=4,9)]

::: moniker-end

Порядок ПО промежуточного слоя важен. В предыдущем примере исключение `InvalidOperationException` возникает при вызове `UseSession` после `UseMvc`. Дополнительные сведения см. в разделе [Порядок расположения ПО промежуточного слоя](xref:fundamentals/middleware/index#order).

> [!IMPORTANT]
> Если вы ориентируетесь на .NET Framework и используете поставщик TempData на основе сеансов, добавьте в проект пакет [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/).

## <a name="query-strings"></a>Строки запроса

Вы можете передать ограниченный объем данных из одного запроса в другой, добавив их в строку запроса нового запроса. Это удобно для захвата состояния в сохраняемом виде, что позволяет обмениваться ссылками с внедренным состоянием по электронной почте или через социальные сети. Поскольку строки запроса URL-адреса являются открытыми, никогда не используйте их для конфиденциальных данных.

Вдобавок к случайному раскрытию информации включение данных в строки запроса может открыть возможности для атак с [подделкой межсайтовых запросов (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)), которые могут обманом вынудить пользователей посещать вредоносные веб-сайты во время проверки подлинности. Это позволит злоумышленникам похитить данные пользователей из приложения или предпринимать вредоносные действия от их имени. Любое сохраненное состояние приложения или сеанса необходимо защитить от атак CSRF. Дополнительные сведения см. в статье [Предотвращение атак с подделкой межсайтовых запросов (XSRF/CSRF)](xref:security/anti-request-forgery).

## <a name="hidden-fields"></a>Скрытые поля

Данные можно сохранить в скрытых полях формы и опубликовать обратно при следующем запросе. Это типичное поведение для многостраничных форм. Так как клиент может незаконно изменить данные, приложение всегда должно перепроверять данные в скрытых полях.

## <a name="httpcontextitems"></a>HttpContext.Items

Коллекция [HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) используется для хранения данных при обработке одного запроса. Ее содержимое удаляется после обработки каждого запроса. Коллекцию `Items` часто используют для обеспечения взаимодействия компонентов или ПО промежуточного слоя, когда они выполняются в разные моменты времени во время обработки запроса и не могут передавать параметры напрямую.

В следующем примере [ПО промежуточного слоя](xref:fundamentals/middleware/index) добавляет `isVerified` в коллекцию `Items`.

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

Далее в конвейере другое ПО промежуточного слоя может получить доступ к значению `isVerified`:

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync($"Verified: {context.Items["isVerified"]}");
});
```

Для ПО промежуточного слоя, которое используется всего одним приложением, допустимы ключи `string`. ПО промежуточного слоя, используемое несколькими экземплярами приложения, должно использовать уникальные ключи объекта во избежание конфликтов. В следующем примере показано, как использовать уникальный ключ объекта, определенный в классе ПО промежуточного слоя:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=5,14)]

::: moniker-end

Другой код может обратиться к значению, хранящемуся в `HttpContext.Items`, с помощью ключа, предоставляемого классом ПО промежуточного слоя:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet3)]

::: moniker-end

Данный подход также позволяет не использовать строки ключей в коде.

## <a name="cache"></a>Кэш

Кэширование — это эффективный способ хранения и извлечения данных. Приложение может управлять временем существования элементов кэша.

Кэшированные данные не связаны с конкретным запросом, пользователем или сеансом. **Будьте внимательны и не кэшируйте данные пользователя, которые можно извлечь по запросу другого пользователя.**

Дополнительные сведения см. в разделе [Ответы кэша](xref:performance/caching/index).

## <a name="dependency-injection"></a>Внедрение зависимостей

Используйте [внедрение зависимостей](xref:fundamentals/dependency-injection), чтобы сделать данные доступными для всех пользователей:

1. Определите службу, содержащую данные. Например, определяется класс с именем `MyAppData`:

    ```csharp
    public class MyAppData
    {
        // Declare properties and methods
    }
    ```

2. Добавьте класс службы в `Startup.ConfigureServices`:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<MyAppData>();
    }
    ```

3. Используйте класс службы данных:

    ::: moniker range=">= aspnetcore-2.0"

    ```csharp
    public class IndexModel : PageModel
    {
        public IndexModel(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

    ::: moniker range="< aspnetcore-2.0"

    ```csharp
    public class HomeController : Controller
    {
        public HomeController(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

## <a name="common-errors"></a>Распространенные ошибки

* "Unable to resolve service for type "Microsoft.Extensions.Caching.Distributed.IDistributedCache" while attempting to activate "Microsoft.AspNetCore.Session.DistributedSessionStore"" (Не удается разрешить службу для типа "Microsoft.Extensions.Caching.Distributed.IDistributedCache" при попытке активировать "Microsoft.AspNetCore.Session.DistributedSessionStore").

  Обычно она вызвана невозможностью настройки по меньшей мере одной реализации `IDistributedCache`. Дополнительные сведения см. в разделах <xref:performance/caching/distributed> и <xref:performance/caching/memory>.

* Если ПО промежуточного слоя для сеанса не удается сохранить сеанс (например, резервное хранилище недоступно), оно регистрирует исключение и запрос выполняется обычным образом. Это приводит к непредсказуемому поведению.

  Например, пользователь сохраняет корзину для покупок в сеансе. Он добавляет товар в корзину, но фиксация завершается сбоем. Приложению неизвестно о сбое, поэтому оно сообщает пользователю, что товар добавлен в корзину, хотя это не так.

  Для проверки на наличие таких ошибок рекомендуется вызывать `await feature.Session.CommitAsync();` из кода приложения по окончании записи в сеанс. `CommitAsync` создает исключение, если резервное хранилище недоступно. Если `CommitAsync` завершается ошибкой, приложение может обработать исключение. Исключение `LoadAsync` возникает в таких же условиях, когда хранилище данных недоступно.

## <a name="additional-resources"></a>Дополнительные ресурсы

<xref:host-and-deploy/web-farm>
