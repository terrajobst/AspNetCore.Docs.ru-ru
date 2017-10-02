---
title: "Реализация kestrel веб-сервера в ASP.NET Core"
author: tdykstra
description: "Вводит Kestrel кросс платформенных веб-сервер для ASP.NET Core в соответствии с libuv."
keywords: "ASP.NET Core, Kestrel, libuv, префиксы URL-адрес"
ms.author: tdykstra
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 560bd66f-7dd0-4e68-b5fb-f31477e4b785
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/kestrel
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 451c36fc9095b6e076e5287c992b6163205c523b
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/01/2017
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a>Общие сведения о Kestrel реализация веб-сервера в ASP.NET Core

По [Tom Dykstra](https://github.com/tdykstra), [Ross Крис](https://github.com/Tratcher), и [Стивен Хальтер](https://twitter.com/halter73)

Kestrel является кросс платформенных [веб-сервер для ASP.NET Core](index.md) на основе [libuv](https://github.com/libuv/libuv), кросс платформенной библиотекой асинхронных операций ввода-вывода. Kestrel является веб-сервером, который включен по умолчанию в шаблонах проектов ASP.NET Core. 

Kestrel поддерживает следующие функции:

  * HTTPS
  * Непрозрачный обновления используется для включения [WebSockets](https://github.com/aspnet/websockets)
  * Сокеты UNIX для повышения производительности за Nginx 

Kestrel поддерживается на всех платформах и версии, поддерживаемые .NET Core.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Просмотреть или загрузить образец кода для 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([загрузке](xref:tutorials/index#how-to-download-a-sample))

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[Просмотреть или загрузить образец кода для 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([загрузке](xref:tutorials/index#how-to-download-a-sample))

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Когда следует использовать Kestrel с обратного прокси-сервера

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Kestrel можно использовать отдельно или с *обратным прокси-сервером*, таким как IIS, Nginx или Apache. Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки.

![Kestrel взаимодействует с Интернетом напрямую, без обратного прокси-сервера](kestrel/_static/kestrel-to-internet2.png)

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

Любая конфигурация &mdash; с обратным прокси-сервером или без &mdash; также может использоваться, если Kestrel предоставляется только для внутренней сети.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Если приложение принимает запросы только из внутренней сети, можно использовать Kestrel отдельно.

![Kestrel взаимодействует с вашей внутренней сетью напрямую.](kestrel/_static/kestrel-to-internal.png)

Если приложение имеет доступ к Интернету, необходимо использовать IIS, Nginx или Apache как *обратный прокси-сервер*. Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки.

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

Обратный прокси-сервер необходим для развертываний edge (открытый для трафика из Интернета), по соображениям безопасности. Версии Kestrel 1.x не оснащены всем комплектом функций для защиты от атак. Это включает в себя, но не ограничиваются соответствующие значения времени ожидания, ограничения на размер и ограничение числа одновременных подключений.

---

Сценария, требующего обратного прокси-сервера при наличии нескольких приложений, которые совместно используют того же IP-адрес и порт, работающих на одном сервере. Это не поможет с Kestrel непосредственно Kestrel поддерживает совместное использование того же IP-адрес и порт между несколькими процессами. При настройке Kestrel для прослушивания порта, он обрабатывает весь трафик для этого порта, независимо от того, заголовок узла. Обратный прокси-сервер, который может совместно использоваться порты необходимо направить Kestrel на уникальный IP-адрес и порт.

Даже если обратного прокси-сервера не требуется, с помощью одного может быть хорошим выбором по другим причинам.

* Он может ограничивать предоставляемых область.
* Он предоставляет необязательный дополнительный уровень защиты и настройка.
* Он может лучше интегрироваться с существующей инфраструктурой.
* Оно упрощает балансировку нагрузки и настройке протокола SSL. Только обратного прокси-сервера требуется сертификат SSL, и этот сервер может обмениваться данными с серверами приложений во внутренней сети, с помощью обычного протокола HTTP.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>Как использовать в приложениях ASP.NET Core Kestrel

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) пакет включен в [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).

Шаблоны проектов ASP.NET Core Kestrel используется по умолчанию. В *Program.cs*, этот код вызывает шаблон `CreateDefaultBuilder`, который вызывает [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) незаметно.

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

Если необходимо настроить параметры Kestrel, вызовите `UseKestrel` в *Program.cs* как показано в следующем примере:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Установка [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) пакет NuGet.

Вызовите [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) метод расширения в `WebHostBuilder` в ваш `Main` метод, указывая любой [Kestrel параметры](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) , требуется, как показано в следующем разделе.

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Параметры kestrel

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Веб-сервер Kestrel имеет ограничение параметры конфигурации, которые особенно полезны в развертываниях с выходом в Интернет. Ниже приведены некоторые ограничения, которые можно задать.

- максимальное число клиентских подключений;
- максимальный размер текста запроса;
- минимальная скорость передачи данных в тексте запроса.

Задайте эти ограничения, а также других на `Limits` свойство [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) класса. `Limits` Свойство содержит экземпляр [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) класса. 

**Максимальное клиентских подключений**

Максимальное число одновременных открытых TCP-подключений можно задать для всего приложения с помощью следующего кода:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

Отдельный предел для подключений, которые были обновлены с HTTP или HTTPS для другой протокол (например, в запросе WebSockets) отсутствует. После обновления подключение, он не считается относительно `MaxConcurrentConnections` ограничение. 

Максимальное число подключений — неограниченное (null) по умолчанию.

**Размер максимального запроса**

Размер максимального запроса текст по умолчанию — 30,000,000 байт которого составляет приблизительно 28.6 МБ. 

Чтобы переопределить ограничение в приложении ASP.NET Core MVC рекомендуется использовать [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) атрибута в методе действия:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Ниже приведен пример, в котором показано, как настроить ограничения для всего приложения, каждый запрос:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

Можно переопределить параметр на конкретный запрос в по промежуточного слоя.

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
Исключение при попытке настроить ограничение на запрос после начала чтение запроса приложением. Отсутствует `IsReadOnly` свойство, определяющее, если `MaxRequestBodySize` свойства находится в состоянии только для чтения, то есть ограничение уже слишком поздно.

**Скорость передачи данных запроса минимальное текста**

Если данные поступают со скоростью, указанной в байтах в секунду, kestrel проверяет каждую секунду. Если скорость падает ниже минимума, соединения истекло время ожидания. Льготный период — количество времени, что Kestrel дает клиента, чтобы увеличить его скорость отправки до минимально; скорость в течение этого времени не проверяется. Льготный период помогает избежать потери подключения, которые первоначально отправляют данные малый размер из-за медленного пуска TCP.

Минимальная скорость по умолчанию — 240 байт/сек, с 5-секундный льготный период.

Минимальная скорость также применяется в ответ. Код, чтобы задать ограничение запроса и ответа ограничение зависит от того, за исключением того `RequestBody` или `Response` в именах свойств и интерфейс. 

Ниже приведен пример, в котором показано, как настроить курсы минимальный набор данных в *Program.cs*:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

Тарифы на запрос можно настроить в по промежуточного слоя.

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

Сведения о других параметрах Kestrel см. следующие классы:

* [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Сведения о параметрах Kestrel см. в разделе [KestrelServerOptions класса](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).

---

### <a name="endpoint-configuration"></a>Конфигурация конечной точки

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

По умолчанию ASP.NET Core привязывается к `http://localhost:5000`. Настройте префиксы URL-адреса и порты для Kestrel на прослушивание путем вызова `Listen` или `ListenUnixSocket` методы `KestrelServerOptions`. (`UseUrls`, `urls` аргумент командной строки, а также работают переменная среды ASPNETCORE_URLS, но имеют ограничения, которые указаны [далее в этой статье](#useurls-limitations).)

**Привязка к TCP-сокет**

`Listen` Метод привязан к TCP-сокет и параметров лямбда-выражения можно настроить SSL-сертификата:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

Обратите внимание на то, как в этом примере настраивается SSL для конкретной конечной точки с помощью [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs). Такой же API можно использовать для настройки других параметров Kestrel для отдельных конечных точек.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

**Привязка к сокету Unix**

Могут прослушивать сокета Unix для улучшения производительности Nginx, как показано в следующем примере:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

**Порт 0**

Если указать номер порта 0 Kestrel динамически связывает доступный порт. В следующем примере показано, как определить, какой порт Kestrel фактически привязан к во время выполнения:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

**Ограничения UseUrls**

Можно настроить конечные точки, вызвав `UseUrls` метода или с помощью `urls` аргумент командной строки или переменной среды ASPNETCORE_URLS. Эти методы полезны, если требуется, чтобы код работал с серверы, отличные от Kestrel. Однако следует учитывать следующие ограничения:

* SSL нельзя использовать с помощью этих методов.
* Если используются и `Listen` метод и `UseUrls`, `Listen` переопределить конечные точки `UseUrls` конечных точек.

**Конфигурация конечной точки для служб IIS**

При использовании IIS привязки URL-адрес для IIS переопределить все привязки, которые задаются с помощью вызова `Listen` или `UseUrls`. Дополнительные сведения см. в разделе [введение в ASP.NET Core модуля](aspnet-core-module.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

По умолчанию ASP.NET Core привязывается к `http://localhost:5000`. Можно настроить префиксы URL-адреса и порты для Kestrel на прослушивание с помощью `UseUrls` метод расширения `urls` аргумент командной строки или система конфигурации ASP.NET Core. Дополнительные сведения об этих методах см. в разделе [размещения](../../fundamentals/hosting.md). Сведения о работе привязку URL-адресов при использовании служб IIS в качестве обратного прокси-сервера см. в разделе [модуль ASP.NET Core](aspnet-core-module.md). 

---

### <a name="url-prefixes"></a>Префиксы URL-адрес

При вызове метода `UseUrls` или использовать `urls` аргумент командной строки или переменной среды ASPNETCORE_URLS, префиксы URL-адрес может быть в любом из следующих форматов. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Допустимы только префиксы URL-адрес HTTP; Kestrel не поддерживает SSL, при настройке URL-адрес привязки с помощью `UseUrls`.

* IPv4-адрес с номером порта

  ```
  http://65.55.39.10:80/
  ```

  0.0.0.0 является особым случаем, который привязывает всех адресов IPv4.


* IPv6-адрес с номером порта

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  [::] эквивалентно IPv6 IPv4 0.0.0.0.


* Имя узла с номером порта

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Имена, узлов, *, и +, не являются особыми. Все компоненты, не является распознаваемым IP-адрес или «localhost» будет привязано ко всем IPv4 и IPv6 IP-адресов. Если необходимо привязать другие имена узлов для разных приложений ASP.NET Core, к тому же порту, используйте [HTTP.sys](httpsys.md) или обратного прокси-сервера, таких как IIS, Nginx или Apache.

* Имя «Localhost» с IP-номер или замыкания на себя порт с номером порта

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Когда `localhost` указан, Kestrel пытается связаться с интерфейсы замыкания на себя IPv4 и IPv6. Если запрошенный порт уже используется другой службой или интерфейс замыкания на себя, Kestrel не запускается. Если один из этих интерфейсов замыкания на себя недоступна по другой причине (большинство часто, поскольку IPv6 не поддерживается), Kestrel заносит в журнал предупреждение. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* IPv4-адрес с номером порта

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  0.0.0.0 является особым случаем, который привязывает всех адресов IPv4.


* IPv6-адрес с номером порта

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  [::] эквивалентно IPv6 IPv4 0.0.0.0.


* Имя узла с номером порта

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  Имена, узлов \*, а + не специальные. Все, что не является распознаваемым IP-адрес или «localhost» привязывается ко всем IPv4 и IPv6 IP-адресов. Если необходимо привязать другие имена узлов для разных приложений ASP.NET Core, к тому же порту, используйте [WebListener](weblistener.md) или обратного прокси-сервера, таких как IIS, Nginx или Apache.

* Имя «Localhost» с IP-номер или замыкания на себя порт с номером порта

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Когда `localhost` указан, Kestrel пытается связаться с интерфейсы замыкания на себя IPv4 и IPv6. Если запрошенный порт уже используется другой службой или интерфейс замыкания на себя, Kestrel не запускается. Если один из этих интерфейсов замыкания на себя недоступна по другой причине (большинство часто, поскольку IPv6 не поддерживается), Kestrel заносит в журнал предупреждение. 

* Сокет UNIX

  ```
  http://unix:/run/dan-live.sock  
  ```

**Порт 0**

Если указать номер порта 0 Kestrel динамически связывает доступный порт. Привязка к порту 0 допускается для любого имени узла или IP-адрес за исключением `localhost` имя.

В следующем примере показано, как определить, какой порт Kestrel фактически привязан к во время выполнения:

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

**Префиксы URL-адреса для SSL**

Убедитесь, что префиксы URL-адрес с `https:` при вызове метода `UseHttps` метод расширения, как показано ниже.

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
> HTTPS и HTTP не может размещаться на тот же порт.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения см. в следующих ресурсах:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [Образец приложения для 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [Kestrel исходного кода](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [Образец приложения для 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [Kestrel исходного кода](https://github.com/aspnet/KestrelHttpServer)

---
