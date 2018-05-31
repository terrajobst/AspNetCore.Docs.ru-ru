---
title: Реализации веб-сервера Kestrel в ASP.NET Core
author: rick-anderson
description: Общие сведения о Kestrel — кроссплатформенном веб-сервере для ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/02/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 1c5d229614e6d6ca6889d19a5f3dc145da01bc04
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/27/2018
ms.locfileid: "34555330"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a>Реализации веб-сервера Kestrel в ASP.NET Core

Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra), [Крис Росс](https://github.com/Tratcher) (Chris Ross) и [Стивен Хальтер](https://twitter.com/halter73) (Stephen Halter)

Kestrel — это кроссплатформенный [веб-сервер для ASP.NET Core](xref:fundamentals/servers/index). Веб-сервер Kestrel по умолчанию включается в шаблоны проектов ASP.NET Core.

Kestrel поддерживает следующие функции:

* HTTPS
* Непрозрачное обновление для поддержки [WebSocket](https://github.com/aspnet/websockets)
* Сокеты UNIX для повышения производительности при работе за Nginx

Kestrel поддерживается на всех платформах и во всех версиях, поддерживаемых .NET Core.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Условия использования Kestrel с обратным прокси-сервером

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Kestrel можно использовать отдельно или с *обратным прокси-сервером*, таким как IIS, Nginx или Apache. Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки.

![Kestrel взаимодействует с Интернетом напрямую, без обратного прокси-сервера](kestrel/_static/kestrel-to-internet2.png)

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

Любая из этих конфигураций &mdash; с обратным прокси-сервером и без него &mdash; является допустимой и поддерживаемой для размещения основных компонентов приложений ASP.NET версии 2.0 и выше.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Если приложение принимает запросы только от внутренней сети, можно использовать Kestrel напрямую как сервер приложения.

![Kestrel взаимодействует с вашей внутренней сетью напрямую.](kestrel/_static/kestrel-to-internal.png)

Если приложение имеет доступ к Интернету, необходимо использовать IIS, Nginx или Apache в качестве *обратного прокси-сервера*. Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки.

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

По соображениям безопасности для развертываний пограничного сервера (открытого для интернет-трафика) нужен обратный прокси-сервер. В версиях Kestrel 1.x нет полного набора функций защиты от атак, например надлежащего времени ожидания, ограничений по размеру и ограничений количества одновременных подключений.

---

Обратный прокси-сервер требуется в ситуациях, когда несколько приложений совместно используют один IP-адрес и порт и выполняются на одном сервере. Kestrel не поддерживает этот сценарий, поскольку не поддерживает совместное использование одного IP-адреса и порта несколькими процессами. Когда Kestrel настроен на ожидание передачи данных от порта, Kestrel обрабатывает весь трафик для этого порта независимо от заголовка узла запросов. Поэтому обратный прокси-сервер, который может совместно использовать порты, способен пересылать запросы в Kestrel с уникальным IP-адресом и портом.

Даже если обратный прокси-сервер не требуется, его использование может оказаться удобным:

* Он может ограничить общедоступную контактную зону размещенных на нем приложений.
* Он предоставляет дополнительный уровень конфигурации и защиты.
* Он может лучше интегрироваться с существующей инфраструктурой.
* Он упрощает балансировку нагрузки и настройку SSL. SSL-сертификат требуется только обратному прокси-серверу, а сам этот сервер может обмениваться данными с серверами приложений во внутренней сети по обычному протоколу HTTP.

> [!WARNING]
> Если не используется обратный прокси-сервер с включенной фильтрацией узла, необходимо включить [фильтрацию узла](#host-filtering).

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>Условия использования Kestrel в приложениях ASP.NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Пакет [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) входит в состав [метапакета Microsoft.AspNetCore.All](xref:fundamentals/metapackage).

Шаблоны проектов ASP.NET Core используют Kestrel по умолчанию. В файле *Program.cs* код шаблона вызывает [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), который в фоновом режиме вызывает [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel).

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Установите пакет NuGet [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).

Вызовите метод расширения [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) для [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) в методе `Main`, указав все необходимые [параметры Kestrel](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1), как показано в следующем разделе.

[!code-csharp[](kestrel/samples/1.x/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Параметры Kestrel

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Веб-сервер Kestrel имеет ограничительные параметры конфигурации, которые удобно использовать в развертываниях с выходом в Интернет. Несколько важных ограничений, которые можно настроить:

* максимальное число клиентских подключений;
* максимальный размер текста запроса;
* минимальная скорость передачи данных в тексте запроса.

Задайте эти и другие ограничения в свойстве [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) класса [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions). Свойство `Limits` содержит экземпляр класса [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits).

**Максимальное число клиентских подключений**

[MaxConcurrentConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[MaxConcurrentUpgradedConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

Максимальное число одновременно открытых подключений TCP для всего приложения можно задать с помощью следующего кода:

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=3)]

Существует отдельный предел по подключениям, измененным с HTTP или HTTPS на другой протокол (например, по запросу WebSocket). После изменения подключение не учитывается в пределе `MaxConcurrentConnections`.

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=4)]

По умолчанию максимальное число подключений не ограничено (null).

**Maximum request body size** (Максимальный размер текста запроса)

[MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

По умолчанию максимальный размер текста запроса составляет 30 000 000 байт, что примерно соответствует 28,6 МБ.

Чтобы переопределить это ограничение в приложении ASP.NET Core MVC, рекомендуется использовать атрибут [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) в методе действия:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Приведенный ниже пример показывает, как настроить ограничение для приложения и каждого запроса:

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=5)]

Можно переопределить параметр для конкретного запроса в ПО промежуточного слоя:

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Limits&highlight=3-4)]

При попытке настроить ограничение для запроса после того, как приложение начало считывать запрос, возникает исключение. Доступно свойство `IsReadOnly`, указывающее, что свойство `MaxRequestBodySize` находится в состоянии только для чтения и настраивать ограничение слишком поздно.

**Minimum request body data rate** (Минимальная скорость передачи данных в тексте запроса)

[MinRequestBodyDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[MinResponseDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

Kestrel каждую секунду проверяет, поступают ли данные с указанной скоростью в байтах в секунду. Если скорость падает ниже минимума, для соединения истекает время ожидания. Льготный период — это количество времени, которое Kestrel дает клиенту на увеличение его скорости отправки до минимального уровня; в течение этого периода скорость не проверяется. Льготный период помогает избежать разрыва соединений, которые первоначально отправляют данные с небольшой скоростью из-за медленного запуска TCP.

Минимальная скорость по умолчанию составляет 240 байт/с, льготный период равен 5 секундам.

Минимальная скорость также применяется к отклику. Код для задания лимита запросов и лимита откликов различается только наличием `RequestBody` или `Response` в именах свойств и интерфейсов.

Ниже приведен пример, показывающий, как настроить минимальную скорость передачи данных в *Program.cs*:

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=6-7)]

Вы можете настроить скорости для отдельного запроса в ПО промежуточного слоя:

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Limits&highlight=5-8)]

Сведения о других параметрах и ограничениях Kestrel см. в следующих разделах:

* [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Сведения о параметрах и ограничениях Kestrel см. в следующих разделах:

* [Класс KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

---

### <a name="endpoint-configuration"></a>Конфигурация конечной точки

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

::: moniker range="= aspnetcore-2.0"
По умолчанию платформа ASP.NET Core привязана к `http://localhost:5000`. Вызовите метод [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) или [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) в классе [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions), чтобы настроить префиксы URL-адреса и порты для Kestrel. `UseUrls`, аргумент командной строки `--urls`, ключ конфигурации узла `urls` и переменная среды `ASPNETCORE_URLS` тоже работают, однако на них распространяются ограничения, указанные далее в этой статье.

Ключ конфигурации узла `urls` должен поступать из конфигурации узла, а не конфигурации приложения. Добавление ключа и значения `urls` в *appsettings.json* не влияет на конфигурацию узла, так как узел полностью инициализируется к моменту считывания конфигурации из файла конфигурации. Тем не менее ключ `urls` в *appsettings.json* можно использовать с [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) в конструкторе узла для настройки узла:

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appSettings.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseKestrel()
    .UseConfiguration(config)
    .UseContentRoot(Directory.GetCurrentDirectory())
    .UseStartup<Startup>()
    .Build();
```

::: moniker-end
::: moniker range=">= aspnetcore-2.1"

По умолчанию платформа ASP.NET Core привязана к:

* `http://localhost:5000`
* `https://localhost:5001` (если присутствует локальный сертификат разработки)

Сертификат разработки создается, когда:

* установлен [пакет SDK для .NET Core](/dotnet/core/sdk);
* используется [средство dev-certs](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-dev-certs) для создания сертификата.

В некоторых браузерах требуется явное разрешение доверять локальному сертификату разработки.

Шаблоны проектов на ASP.NET Core 2.1 и более поздних версий настраивают приложения так, чтобы они запускались на HTTPS по умолчанию и включали [поддержку перенаправления HTTPS и HSTS](xref:security/enforcing-ssl).

Вызовите метод [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) или [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) в классе [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions), чтобы настроить префиксы URL-адреса и порты для Kestrel.

`UseUrls`, аргумент командной строки `--urls`, ключ конфигурации узла `urls` и переменная среды `ASPNETCORE_URLS` тоже работают, однако на них распространяются ограничения, указанные далее в этой статье (для конфигурации конечной точки HTTPS требуется сертификат по умолчанию).

Конфигурация `KestrelServerOptions` в ASP.NET Core 2.1:

**ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)**  
Указывает конфигурацию `Action` для выполнения каждой заданной конечной точки. Если вызвать `ConfigureEndpointDefaults` несколько раз, предыдущие элементы `Action` будут заменены последним элементом `Action`.

**ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)**  
Указывает конфигурацию `Action` для выполнения каждой конечной точки HTTPS. Если вызвать `ConfigureHttpsDefaults` несколько раз, предыдущие элементы `Action` будут заменены последним элементом `Action`.

**Configure(IConfiguration)**  
Создает загрузчик конфигурации для настройки Kestrel, который принимает [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) в качестве входных данных. Для Kestrel конфигурация должна быть ограничена разделом конфигурации.

**ListenOptions.UseHttps**  
Настройте Kestrel для использования протокола HTTPS.

Расширения `ListenOptions.UseHttps`:

* `UseHttps` &ndash; Настройте Kestrel для использования протокола HTTPS с сертификатом по умолчанию. Создает исключение, если сертификат по умолчанию не настроен.
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

Параметры `ListenOptions.UseHttps`:

* `filename` — это путь и имя файла сертификата, связанного с каталогом, где находятся файлы содержимого приложения.
* `password` — это пароль для доступа к данным сертификата X.509.
* `configureOptions` — это `Action` для настройки `HttpsConnectionAdapterOptions`. Возвращает `ListenOptions`.
* `storeName` — это хранилище сертификатов, из которого выполняется загрузка сертификата.
* `subject` — это имя субъекта для сертификата.
* `allowInvalid` указывает, следует ли учитывать недопустимые сертификаты, например самозаверяющие сертификаты.
* `location` — это расположение хранилища, из которого загружается сертификат.
* `serverCertificate` — это сертификат X.509.

В рабочей среде необходимо явно настроить HTTPS. Как минимум необходимо указать сертификат по умолчанию.

Поддерживаемые конфигурации, описанные далее:

* Отсутствие конфигурации
* Замена сертификата по умолчанию из конфигурации
* Изменение значений по умолчанию в коде

*Отсутствие конфигурации*

Kestrel ожидает передачи данных через `http://localhost:5000` и `https://localhost:5001` (если доступен сертификат по умолчанию).

Укажите URL-адреса с помощью следующих параметров:

* Переменная среды `ASPNETCORE_URLS`.
* Аргументы командной строки `--urls`.
* Ключ конфигурации узла `urls`.
* Метод расширения `UseUrls`.

Дополнительные сведения см. в разделах [URL-адреса серверов](xref:fundamentals/host/web-host#server-urls) и [Переопределение конфигурации](xref:fundamentals/host/web-host#override-configuration).

Значение, указанное с помощью этих подходов, может быть одной или несколькими конечными точками HTTP и HTTPS (HTTPS при наличии сертификата по умолчанию). Настройте значение в виде списка с разделением точкой с запятой (например, `"Urls": "http://localhost:8000;http://localhost:8001"`).

*Замена сертификата по умолчанию из конфигурации*

[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) вызывает `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` по умолчанию, чтобы загрузить конфигурацию Kestrel. Kestrel имеет доступ к схеме конфигурации параметров приложения HTTPS по умолчанию. Настройте несколько конечных точек, включая URL-адреса и сертификаты для использования, либо из файла на диске, либо из хранилища сертификатов.

В следующем примере *appsettings.json*:

* Установите для **AllowInvalid** значение `true`, чтобы разрешить использование недопустимых сертификатов (например, самозаверяющих сертификатов).
* Любая конечная точка HTTPS, которая не указывает сертификат (**HttpsDefaultCert** в следующем примере), будет использовать сертификат, определенный в разделе **Сертификаты** > **По умолчанию** , или сертификат разработки.

```json
{
"Kestrel": {
  "EndPoints": {
    "Http": {
      "Url": "http://localhost:5000"
    },

    "HttpsInlineCertFile": {
      "Url": "https://localhost:5001",
      "Certificate": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    },

    "HttpsInlineCertStore": {
      "Url": "https://localhost:5002",
      "Certificate": {
        "Subject": "<subject; required>",
        "Store": "<certificate store; defaults to My>",
        "Location": "<location; defaults to CurrentUser>",
        "AllowInvalid": "<true or false; defaults to false>"
      }
    },

    "HttpsDefaultCert": {
      "Url": "https://localhost:5003"
    },

    "Https": {
      "Url": "https://*:5004",
      "Certificate": {
      "Path": "<path to .pfx file>",
      "Password": "<certificate password>"
      }
    }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

Вместо использования параметров **Path** и **Password** для узла сертификата можно указать сертификат с помощью полей хранилища сертификатов. Например, сертификат из раздела **Сертификаты** > **По умолчанию** можно указать следующим образом:

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

Примечания к схеме.

* Регистр букв в именах конечных точек не учитывается. Например, `HTTPS` и `Https` являются допустимыми.
* Параметр `Url` является обязательным для каждой конечной точки. Формат этого параметра такой же, как для параметра конфигурации `Urls` верхнего уровня, только он ограничен одиночным значением.
* Эти конечные точки заменяют конечные точки, определенные в конфигурации `Urls` верхнего уровня, а не дополняют их. Конечные точки, определенные в коде через `Listen`, объединяются с конечными точками, определенными в разделе конфигурации.
* Раздел `Certificate` является необязательным. Если раздел `Certificate` не указан, используются значения по умолчанию, определенные в предыдущих сценариях. Если значений по умолчанию нет, сервер выдает исключение и не запускается.
* Раздел `Certificate` поддерживает сертификаты **Path**&ndash;**Password** и **Subject**&ndash;**Store**.
* Таким образом, можно определить любое количество конечных точек, если это не приводит к конфликту портов.
* `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` возвращает `KestrelConfigurationLoader` с методом `.Endpoint(string name, options => { })`, который может использоваться в качестве дополнения для параметров настроенной конечной точки:

  ```csharp
  serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  Можно также напрямую использовать `KestrelServerOptions.ConfigurationLoader`, чтобы и далее выполнять итерацию с существующим загрузчиком, например, предоставленным `WebHost.CreatedDeafaultBuilder`.

* Раздел конфигурации для каждой конечной точки доступен в параметрах в методе `Endpoint`, чтобы можно было прочитать пользовательские параметры.
* Можно загрузить несколько конфигураций, снова вызвав `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` с другим разделом. Используется только последняя конфигурация, если явным образом не вызвать `Load` в предыдущих экземплярах. Метапакет не вызывает `Load`, чтобы можно было заменить его раздел конфигурации по умолчанию.
* `KestrelConfigurationLoader` отражает семейство API `Listen` из `KestrelServerOptions` как перегрузки `Endpoint`, чтобы можно было настроить конечные точки кода и конфигурации в одном месте. Эти перегрузки не используют имена и используют только параметры по умолчанию из конфигурации.

*Изменение значений по умолчанию в коде*

Можно использовать `ConfigureEndpointDefaults` и `ConfigureHttpsDefaults` для изменения параметров по умолчанию для `ListenOptions` и `HttpsConnectionAdapterOptions`, включая переопределение сертификата по умолчанию, указанного в предыдущем сценарии. Необходимо вызвать `ConfigureEndpointDefaults` и `ConfigureHttpsDefaults`, прежде чем настраивать конечные точки.

```csharp
options.ConfigureEndpointDefaults(opt =>
{
    opt.NoDelay = true;
});

options.ConfigureHttpsDefaults(httpsOptions =>
{
    httpsOptions.SslProtocols = SslProtocols.Tls12;
});
```

*Поддержка Kestrel для SNI*

Можно использовать [указание имени сервера (SNI)](https://tools.ietf.org/html/rfc6066#section-3) для размещения нескольких доменов в одном IP-адресе и порте. Для использования SNI клиент отправляет имя узла для безопасного сеанса серверу во время подтверждения TLS, чтобы сервер предоставил правильный сертификат. Клиент использует предоставленный сертификат для зашифрованного соединения с сервером во время безопасного сеанса, который следует после подтверждения TLS.

Kestrel поддерживает SNI через обратный вызов `ServerCertificateSelector`. Функция обратного вызова используется один раз за подключение, чтобы приложение проверило имя узла и выбрало соответствующий сертификат.

Для поддержки SNI приложение должно выполняться на целевой платформе `netcoreapp2.1`. В `netcoreapp2.0` и `net461` обратный вызов выполняется, но `name` всегда имеет значение `null`. `name` также имеет значение `null`, если клиент не предоставляет параметр имени узла при подтверждении TLS.

```csharp
WebHost.CreateDefaultBuilder()
    .UseKestrel((context, options) =>
    {
        options.ListenAnyIP(5005, listenOptions =>
        {
            listenOptions.UseHttps(httpsOptions =>
            {
                var localhostCert = CertificateLoader.LoadFromStoreCert(
                    "localhost", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var exampleCert = CertificateLoader.LoadFromStoreCert(
                    "example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var subExampleCert = CertificateLoader.LoadFromStoreCert(
                    "sub.example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var certs = new Dictionary(StringComparer.OrdinalIgnoreCase);
                certs["localhost"] = localhostCert;
                certs["example.com"] = exampleCert;
                certs["sub.example.com"] = subExampleCert;

                httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                {
                    if (name != null && certs.TryGetValue(name, out var cert))
                    {
                        return cert;
                    }

                    return exampleCert;
                };
            });
        });
    });
```

::: moniker-end

**Привязка к TCP-сокету**

Метод [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) привязан к TCP-сокету, а лямбда-выражение параметров позволяет настроить SSL-сертификат:

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

В примере настраивается SSL для конечной точки с помощью [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions). С помощью этого API можно настроить и другие параметры Kestrel для отдельных конечных точек.

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

**Привязка к сокету UNIX**

Вы можете прослушивать сокет UNIX с помощью [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) для улучшения производительности Nginx, как показано в следующем примере:

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_UnixSocket)]

**Порт 0**

Если указать номер порта `0`, Kestrel динамически привязывается к доступному порту. Следующий пример показывает, как определить, к какому порту фактически привязан Kestrel во время выполнения:

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Port0&highlight=3)]

Когда приложение выполняется, в выходных данных в окне консоли указывается динамический порт, по которому можно связаться с приложением:

```console
Now listening on: http://127.0.0.1:48508
```

**Ограничения UseUrls, аргумента командной строки --urls, ключа конфигурации узла urls и переменной среды ASPNETCORE_URLS**

Настройте конечные точки с помощью следующих подходов:

* [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls).
* Аргументы командной строки `--urls`.
* Ключ конфигурации узла `urls`.
* Переменная среды `ASPNETCORE_URLS`.

Эти методы удобны, если нужно, чтобы код работал с серверами, отличными от Kestrel. Однако следует учитывать следующие ограничения:

* С этими подходами нельзя использовать SSL, если в конфигурации конечной точки HTTPS не предоставлен сертификат по умолчанию (например, с помощью конфигурации `KestrelServerOptions` или файла конфигурации, как показано выше в этом разделе).
* Если подходы `Listen` и `UseUrls` используются одновременно, конечные точки `Listen` переопределяют конечные точки `UseUrls`.

**Конфигурация конечной точки IIS**

При использовании служб IIS привязки URL-адресов для IIS переопределяют привязки, заданные `Listen` или `UseUrls`. Дополнительные сведения см. в статье [Модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

По умолчанию платформа ASP.NET Core привязана к `http://localhost:5000`. Настройте префиксы URL-адресов и порты для Kestrel, используя:

* Метод расширения [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1)
* Аргументы командной строки `--urls`.
* Ключ конфигурации узла `urls`.
* Система конфигурации ASP.NET Core, включая переменную среды `ASPNETCORE_URLS`.

Дополнительные сведения об этих методах см. в разделе [Размещение](xref:fundamentals/host/index).

**Конфигурация конечной точки IIS**

При использовании служб IIS привязки URL-адресов для IIS переопределяют привязки, заданные `UseUrls`. Дополнительные сведения см. в статье [Модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

---

::: moniker range=">= aspnetcore-2.1"

## <a name="transport-configuration"></a>Конфигурация транспорта

После выпуска ASP.NET Core 2.1 транспорт Kestrel по умолчанию основан не на Libuv, а на управляемых сокетах. Это критическое изменение для приложений ASP.NET Core 2.0, обновляющихся до версии 2.1, которые вызывают [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) и зависят от одного из следующих пакетов:

* [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (прямая ссылка на пакет)
* [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

Для ASP.NET Core 2.1 или более поздней версии проекты, которые используют метапакет `Microsoft.AspNetCore.App` и требуют использования Libuv:

* Добавляют зависимость для пакета [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) к файлу проекта приложения:

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                    Version="2.1.0" />
    ```

* Вызывают [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):

    ```csharp
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseLibuv()
                .UseStartup<Startup>();
    }
    ```

::: moniker-end

### <a name="url-prefixes"></a>Префиксы URL-адресов

Если вы используете `UseUrls`, аргумент командной строки `--urls`, ключ конфигурации узла `urls` или переменную среды `ASPNETCORE_URLS`, префиксы URL-адресов могут иметь любой из указанных ниже форматов.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Допустимы только префиксы URL-адресов HTTP. Kestrel не поддерживает SSL при настройке привязки URL-адресов с помощью `UseUrls`.

* IPv4-адрес с номером порта

  ```
  http://65.55.39.10:80/
  ```

  `0.0.0.0` является особым случаем, соответствующим привязке ко всем IPv4-адресам.

* IPv6-адрес с номером порта

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  `[::]` является IPv6-аналогом IPv4-адреса `0.0.0.0`.

* Имя узла с номером порта

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Имена узлов, `*` и `+`, не являются особыми. Все, что не распознается как допустимый IP-адрес или `localhost`, привязывается ко всем IP-адресам IPv4 и IPv6. Чтобы привязать разные имена узлов к разным приложениям ASP.NET Core по одному порту, используйте [HTTP.sys](xref:fundamentals/servers/httpsys) или обратный прокси-сервер, такой как IIS, Nginx или Apache.

  > [!WARNING]
  > Если не используется обратный прокси-сервер с включенной фильтрацией узла, необходимо включить [фильтрацию узла](#host-filtering).

* Имя узла `localhost` с номером порта или IP-адрес замыкания на себя с номером порта

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Когда указан `localhost`, Kestrel пытается привязаться к обоим интерфейсам замыкания на себя IPv4 и IPv6. Если запрошенный порт уже используется другой службой в одном из интерфейсов замыкания на себя, Kestrel не запускается. Если один из интерфейсов замыкания на себя недоступен по любой другой причине (чаще всего, это отсутствие поддержки IPv6), Kestrel заносит в журнал предупреждение.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* IPv4-адрес с номером порта

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  `0.0.0.0` является особым случаем, соответствующим привязке ко всем IPv4-адресам.

* IPv6-адрес с номером порта

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  `[::]` является IPv6-аналогом IPv4-адреса `0.0.0.0`.

* Имя узла с номером порта

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  Имена узлов, `*` и `+`, не являются особыми. Все, что не распознается как IP-адрес или `localhost`, привязывается ко всем IP-адресам IPv4 и IPv6. Чтобы привязать разные имена узлов к разным приложениям ASP.NET Core по одному порту, используйте [WebListener](xref:fundamentals/servers/weblistener) или обратный прокси-сервер, такой как IIS, Nginx или Apache.

* Имя узла `localhost` с номером порта или IP-адрес замыкания на себя с номером порта

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Когда указан `localhost`, Kestrel пытается привязаться к обоим интерфейсам замыкания на себя IPv4 и IPv6. Если запрошенный порт уже используется другой службой в одном из интерфейсов замыкания на себя, Kestrel не запускается. Если один из интерфейсов замыкания на себя недоступен по любой другой причине (чаще всего, это отсутствие поддержки IPv6), Kestrel заносит в журнал предупреждение.

* Сокет UNIX

  ```
  http://unix:/run/dan-live.sock
  ```

**Порт 0**

Если указать номер порта `0`, Kestrel динамически привязывается к доступному порту. Привязка к порту `0` разрешена для любого имени узла или IP-адреса, кроме `localhost`.

Когда приложение выполняется, в выходных данных в окне консоли указывается динамический порт, по которому можно связаться с приложением:

```console
Now listening on: http://127.0.0.1:48508
```

**Префиксы URL-адресов для SSL**

При вызове метода расширения `UseHttps` укажите префиксы URL-адресов с `https:`:

```csharp
var host = new WebHostBuilder()
    .UseKestrel(options =>
    {
        options.UseHttps("testCert.pfx", "testPassword");
    })
   .UseUrls("http://localhost:5000", "https://localhost:5001")
   .UseContentRoot(Directory.GetCurrentDirectory())
   .UseStartup<Startup>()
   .Build();
```

> [!NOTE]
> HTTPS и HTTP не могут размещаться на одном порте.

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

---

## <a name="host-filtering"></a>Фильтрация узлов

Хотя Kestrel поддерживает конфигурации с использованием префиксов, такие как `http://example.com:5000`, Kestrel не учитывает имя узла. Узел `localhost` является особым случаем, используемым для привязки к адресам замыкания на себя. Любой узел, отличный от явного IP-адреса, привязывается ко всем общедоступным IP-адресам. Эта информация не используется для проверки заголовков запроса `Host`.

У вас есть два варианта обойти эту проблему.

* Узел за обратным прокси-сервером с фильтрацией заголовков узла. Это был единственный поддерживаемый сценарий для Kestrel в ASP.NET Core 1.x.
* Используйте ПО промежуточного слоя, чтобы фильтровать запросы по заголовку `Host`. Пример ПО промежуточного слоя:

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Primitives;
using Microsoft.Net.Http.Headers;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

// A normal middleware would provide an options type, config binding, extension methods, etc..
// This intentionally does all of the work inside of the middleware so it can be
// easily copy-pasted into docs and other projects.
public class HostFilteringMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IList<string> _hosts;
    private readonly ILogger<HostFilteringMiddleware> _logger;

    public HostFilteringMiddleware(RequestDelegate next, IConfiguration config, ILogger<HostFilteringMiddleware> logger)
    {
        if (config == null)
        {
            throw new ArgumentNullException(nameof(config));
        }

        _next = next ?? throw new ArgumentNullException(nameof(next));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));

        // A semicolon separated list of host names without the port numbers.
        // IPv6 addresses must use the bounding brackets and be in their normalized form.
        _hosts = config["AllowedHosts"]?.Split(new[] { ';' }, StringSplitOptions.RemoveEmptyEntries);
        if (_hosts == null || _hosts.Count == 0)
        {
            throw new InvalidOperationException("No configuration entry found for AllowedHosts.");
        }
    }

    public Task Invoke(HttpContext context)
    {
        if (!ValidateHost(context))
        {
            context.Response.StatusCode = 400;
            _logger.LogDebug("Request rejected due to incorrect Host header.");
            return Task.CompletedTask;
        }

        return _next(context);
    }

    // This does not duplicate format validations that are expected to be performed by the host.
    private bool ValidateHost(HttpContext context)
    {
        StringSegment host = context.Request.Headers[HeaderNames.Host].ToString().Trim();

        if (StringSegment.IsNullOrEmpty(host))
        {
            // Http/1.0 does not require the Host header.
            // Http/1.1 requires the header but the value may be empty.
            return true;
        }

        // Drop the port

        var colonIndex = host.LastIndexOf(':');

        // IPv6 special case
        if (host.StartsWith("[", StringComparison.Ordinal))
        {
            var endBracketIndex = host.IndexOf(']');
            if (endBracketIndex < 0)
            {
                // Invalid format
                return false;
            }
            if (colonIndex < endBracketIndex)
            {
                // No port, just the IPv6 Host
                colonIndex = -1;
            }
        }

        if (colonIndex > 0)
        {
            host = host.Subsegment(0, colonIndex);
        }

        foreach (var allowedHost in _hosts)
        {
            if (StringSegment.Equals(allowedHost, host, StringComparison.OrdinalIgnoreCase))
            {
                return true;
            }

            // Sub-domain wildcards: *.example.com
            if (allowedHost.StartsWith("*.", StringComparison.Ordinal) && host.Length >= allowedHost.Length)
            {
                // .example.com
                var allowedRoot = new StringSegment(allowedHost, 1, allowedHost.Length - 1);

                var hostRoot = host.Subsegment(host.Length - allowedRoot.Length, allowedRoot.Length);
                if (hostRoot.Equals(allowedRoot, StringComparison.OrdinalIgnoreCase))
                {
                    return true;
                }
            }
        }

        return false;
    }
}
```

Зарегистрируйте предыдущий `HostFilteringMiddleware` в `Startup.Configure`. Обратите внимание, что [порядок регистрации ПО промежуточного слоя](xref:fundamentals/middleware/index#ordering) важен. Регистрация должна выполняться сразу после регистрации диагностического ПО промежуточного слоя (например, `app.UseExceptionHandler`).

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseBrowserLink();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseMiddleware<HostFilteringMiddleware>();

    app.UseMvcWithDefaultRoute();
}
```

Предыдущее ПО промежуточного слоя ожидает ключ `AllowedHosts` в *appsettings.\<EnvironmentName >.json*. Значение этого ключа — разделенный точками с запятой список имен узлов без номеров портов. Включите пару "ключ-значение" `AllowedHosts` в *appsettings. Production.JSON*:

```json
{
  "AllowedHosts": "example.com"
}
```

*appsettings.Development.json* (файл конфигурации localhost):

```json
{
  "AllowedHosts": "localhost"
}
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Принудительное использование HTTPS](xref:security/enforcing-ssl)
* [Исходный код Kestrel](https://github.com/aspnet/KestrelHttpServer)
