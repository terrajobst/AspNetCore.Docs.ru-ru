---
title: "Реализации веб-сервера Kestrel в ASP.NET Core"
author: tdykstra
description: "Общие сведения о Kestrel — кроссплатформенном веб-сервере для ASP.NET Core, основанном на libuv."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: bfe7644891296c7c3485c9a870d90951ba87e9e8
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a>Общие сведения о реализации веб-сервера Kestrel в ASP.NET Core

Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra), [Крис Росс](https://github.com/Tratcher) (Chris Ross) и [Стивен Хальтер](https://twitter.com/halter73) (Stephen Halter)

Kestrel — это кроссплатформенный [веб-сервер для ASP.NET Core](index.md) на основе [libuv](https://github.com/libuv/libuv), кроссплатформенный асинхронной библиотеки ввода-вывода. Веб-сервер Kestrel по умолчанию включается в шаблоны проектов ASP.NET Core. 

Kestrel поддерживает следующие функции:

  * HTTPS
  * Непрозрачное обновление для поддержки [WebSocket](https://github.com/aspnet/websockets)
  * Сокеты UNIX для повышения производительности при работе за Nginx 

Kestrel поддерживается на всех платформах и во всех версиях, поддерживаемых .NET Core.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Просмотреть или скачать пример кода для версии 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([описание скачивания](xref:tutorials/index#how-to-download-a-sample))

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[Просмотреть или скачать пример кода для версии 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([описание скачивания](xref:tutorials/index#how-to-download-a-sample))

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Условия использования Kestrel с обратным прокси-сервером

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

По соображениям безопасности для развертываний пограничного сервера (открытого для интернет-трафика) нужен обратный прокси-сервер. Версии Kestrel 1.x не оснащены всем комплектом функций для защиты от атак. Сюда входят, например, соответствующее время ожидания, предельные размеры и максимальное количество одновременных подключений.

---

Сценарием, в котором требуется обратный прокси-сервер, является выполнение нескольких приложений, которые совместно используют один IP-адрес и порт, на одном сервере. В Kestrel это невозможно реализовать напрямую, так как Kestrel не поддерживает совместное использование одного IP-адреса и порта несколькими процессами. Если Kestrel настроен для прослушивания порта, он обрабатывает весь трафик через этот порт, независимо от заголовка узла. Поэтому обратный прокси-сервер, который может совместно использовать порты, должен пересылать данные в Kestrel на уникальный IP-адрес и порт.

Даже если обратный прокси-сервер не требуется, его использование может оказаться удобным по другим причинам:

* Он позволяет ограничить открытую контактную зону.
* Он предоставляет необязательный дополнительный уровень конфигурации и защиты.
* Он может лучше интегрироваться с существующей инфраструктурой.
* Он упрощает балансировку нагрузки и настройку SSL. SSL-сертификат требуется только обратному прокси-серверу, а сам этот сервер может обмениваться данными с серверами приложений во внутренней сети по обычному протоколу HTTP.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>Условия использования Kestrel в приложениях ASP.NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Пакет [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) входит в состав [метапакета Microsoft.AspNetCore.All](xref:fundamentals/metapackage).

Шаблоны проектов ASP.NET Core используют Kestrel по умолчанию. В файле *Program.cs* код шаблона вызывает `CreateDefaultBuilder`, который в фоновом режиме вызывает [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_).

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

Если нужно настроить параметры Kestrel, вызовите `UseKestrel` в *Program.cs*, как показано в следующем примере:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Установите пакет NuGet [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).

Вызовите метод расширения [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) для `WebHostBuilder` в вашем методе `Main`, указав все нужные [параметры Kestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions), как показано в следующем разделе.

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Параметры Kestrel

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Веб-сервер Kestrel имеет ограничительные параметры конфигурации, которые удобно использовать в развертываниях с выходом в Интернет. Ниже приведены некоторые ограничения, которые можно задать:

- максимальное число клиентских подключений;
- максимальный размер текста запроса;
- минимальная скорость передачи данных в тексте запроса.

Для задания этих и других ограничений используется свойство `Limits` класса [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs). Свойство `Limits` содержит экземпляр класса [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs). 

**Максимальное число клиентских подключений**

Максимальное число одновременно открытых подключений TCP для всего приложения можно задать с помощью следующего кода:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

Существует отдельный предел по подключениям, измененным с HTTP или HTTPS на другой протокол (например, по запросу WebSocket). После изменения подключение не учитывается в пределе `MaxConcurrentConnections`. 

По умолчанию максимальное число подключений не ограничено (null).

**Maximum request body size** (Максимальный размер текста запроса)

По умолчанию максимальный размер текста запроса составляет 30 000 000 байт, что примерно соответствует 28,6 МБ. 

Чтобы переопределить это ограничение в приложении ASP.NET Core MVC, рекомендуется использовать атрибут [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) в методе действия:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Приведенный ниже пример показывает, как настроить ограничение для всего приложения и каждого запроса:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

Можно переопределить параметр для конкретного запроса в ПО промежуточного слоя:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
При попытке настроить ограничение для запроса после того, как приложение начало считывать запрос, возникает исключение. Доступно свойство `IsReadOnly`, указывающее, что свойство `MaxRequestBodySize` находится в состоянии только для чтения и настраивать ограничение слишком поздно.

**Minimum request body data rate** (Минимальная скорость передачи данных в тексте запроса)

Kestrel каждую секунду проверяет, поступают ли данные с указанной скоростью в байтах в секунду. Если скорость падает ниже минимума, для соединения истекает время ожидания. Льготный период — это количество времени, которое Kestrel дает клиенту на увеличение его скорости отправки до минимального уровня; в течение этого периода скорость не проверяется. Льготный период помогает избежать разрыва соединений, которые первоначально отправляют данные с небольшой скоростью из-за медленного запуска TCP.

Минимальная скорость по умолчанию составляет 240 байт/с, льготный период равен 5 секундам.

Минимальная скорость также применяется к отклику. Код для задания лимита запросов и лимита откликов различается только наличием `RequestBody` или `Response` в именах свойств и интерфейсов. 

Ниже приведен пример, показывающий, как настроить минимальную скорость передачи данных в *Program.cs*:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

Вы можете настроить скорости для отдельного запроса в ПО промежуточного слоя:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

Сведения о других параметрах Kestrel см. в описании следующих классов:

* [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Сведения о параметрах Kestrel см. в разделе [Класс KestrelServerOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).

---

### <a name="endpoint-configuration"></a>Конфигурация конечной точки

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

По умолчанию ASP.NET Core привязан к `http://localhost:5000`. Вы можете настроить префиксы URL-адресов и порты, которые должен прослушивать Kestrel, вызывая методы `Listen` или `ListenUnixSocket` для `KestrelServerOptions`. (`UseUrls`, аргумент командной строки `urls` и переменная среды ASPNETCORE_URLS тоже работают, однако на них распространяются ограничения, указанные [далее в этой статье](#useurls-limitations).)

**Привязка к TCP-сокету**

Метод `Listen` привязан к TCP-сокету, а лямбда-выражение параметров позволяет настроить SSL-сертификат:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

Обратите внимание, как в этом примере настраивается SSL для конечной точки с помощью [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs). С помощью этого API можно настроить и другие параметры Kestrel для отдельных конечных точек.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

**Привязка к сокету UNIX**

Вы можете прослушивать сокет UNIX для улучшения производительности Nginx, как показано в следующем примере:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

**Порт 0**

Если указать номер порта 0, Kestrel динамически привязывается к доступному порту. Следующий пример показывает, как определить, к какому порту фактически привязан Kestrel во время выполнения:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

**Ограничения UseUrls**

Вы можете настроить конечные точки, вызвав метод `UseUrls` либо используя аргумент командной строки `urls` или переменную среды ASPNETCORE_URLS. Эти методы удобны, если нужно, чтобы код работал с серверами, отличными от Kestrel. Однако следует учитывать следующие ограничения:

* С этими методами невозможно использовать SSL.
* Если вы используете как метод `Listen`, так и `UseUrls`, конечные точки `Listen` переопределяют конечные точки `UseUrls`.

**Конфигурация конечной точки для IIS**

Если вы используете IIS, привязки URL-адресов для IIS переопределяют любые привязки, заданные с помощью вызова `Listen` или `UseUrls`. Дополнительные сведения см. в статье [Введение в модуль ASP.NET Core](aspnet-core-module.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

По умолчанию ASP.NET Core привязан к `http://localhost:5000`. Настроить префиксы URL-адресов и порты, прослушиваемые Kestrel, можно с помощью метода расширения `UseUrls`, аргумента командной строки `urls` или системы конфигурации ASP.NET Core. Дополнительные сведения об этих методах см. в разделе [Размещение](../../fundamentals/hosting.md). Сведения о работе привязки URL-адресов при использовании IIS в качестве обратного прокси-сервера см. в разделе [Модуль ASP.NET Core](aspnet-core-module.md). 

---

### <a name="url-prefixes"></a>Префиксы URL-адресов

Когда вы вызываете метод `UseUrls` либо используете аргумент командной строки `urls` или переменную среды ASPNETCORE_URLS, префиксы URL-адресов могут иметь любой из указанных ниже форматов. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Допустимы только префиксы URL-адресов HTTP; Kestrel не поддерживает SSL при настройке привязок URL-адресов с помощью `UseUrls`.

* IPv4-адрес с номером порта

  ```
  http://65.55.39.10:80/
  ```

  0.0.0.0 является особым случаем, соответствующим привязке ко всем IPv4-адресам.


* IPv6-адрес с номером порта

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  [::] является IPv6-аналогом IPv4-адреса 0.0.0.0.


* Имя узла с номером порта

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Имена узлов, * и + не являются особыми. Все, что не является распознаваемым IP-адресом или "localhost", привязывается ко всем IPv4- и IPv6-адресам. Если нужно привязать разные имена узлов к разным приложениям ASP.NET Core по одному порту, используйте [HTTP.sys](httpsys.md) или обратный прокси-сервер, такой как IIS, Nginx или Apache.

* Имя "Localhost" с номером порта или IP-адрес замыкания на себя с номером порта

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Когда указан `localhost`, Kestrel пытается привязаться к обоим интерфейсам замыкания на себя IPv4 и IPv6. Если запрошенный порт уже используется другой службой в одном из интерфейсов замыкания на себя, Kestrel не запускается. Если один из интерфейсов замыкания на себя недоступен по любой другой причине (чаще всего, это отсутствие поддержки IPv6), Kestrel заносит в журнал предупреждение. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* IPv4-адрес с номером порта

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  0.0.0.0 является особым случаем, соответствующим привязке ко всем IPv4-адресам.


* IPv6-адрес с номером порта

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  [::] является IPv6-аналогом IPv4-адреса 0.0.0.0.


* Имя узла с номером порта

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  Имена узлов, \* и + не являются особыми. Все, что не является распознаваемым IP-адресом или "localhost", привязывается ко всем IPv4- и IPv6-адресам. Если нужно привязать разные имена узлов к разным приложениям ASP.NET Core по одному порту, используйте [WebListener](weblistener.md) или обратный прокси-сервер, такой как IIS, Nginx или Apache.

* Имя "Localhost" с номером порта или IP-адрес замыкания на себя с номером порта

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

Если указать номер порта 0, Kestrel динамически привязывается к доступному порту. Привязка к порту 0 разрешена для любого имени узла или IP-адреса, кроме имени `localhost`.

Следующий пример показывает, как определить, к какому порту фактически привязан Kestrel во время выполнения:

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

**Префиксы URL-адресов для SSL**

Обязательно включите префиксы URL-адресов с `https:`, если вызываете метод расширения `UseHttps`, как показано ниже.

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
> HTTPS и HTTP не могут размещаться на одном порту.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения см. в следующих ресурсах:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [Пример приложения для 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [Исходный код Kestrel](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [Пример приложения для 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [Исходный код Kestrel](https://github.com/aspnet/KestrelHttpServer)

---
