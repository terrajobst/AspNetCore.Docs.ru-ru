---
title: Redis объединительной платы для горизонтального масштабирования ASP.NET Core SignalR
author: bradygaster
description: Узнайте, как настройка Redis объединительной платы для обеспечения горизонтального масштабирования для приложения ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/redis-backplane
ms.openlocfilehash: adf9bbce1353fd811a4044e173533f76bc4193de
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/17/2019
ms.locfileid: "67152917"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-signalr-scale-out"></a>Настройка Redis объединительной платы для горизонтального масштабирования ASP.NET Core SignalR

По [Andrew Stanton медсестра](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), и [том Дайкстра](https://github.com/tdykstra),

В этой статье объясняется SignalR особенности настройки [Redis](https://redis.io/) сервер для масштабирования приложения ASP.NET Core SignalR.

## <a name="set-up-a-redis-backplane"></a>Настройка Redis объединительной платы

* Развертывание сервера Redis.

  > [!IMPORTANT] 
  > Для использования в рабочей среде объединительной Redis рекомендуется только в том случае, когда оно работает в одном центре обработки данных, что и приложение SignalR. В противном случае задержки в сети снижает производительность. Если ваше приложение SignalR работает в облаке Azure, мы рекомендуем служба Azure SignalR вместо объединительной Redis. Можно использовать службу кэша Redis Azure для разработки и тестовые среды.

  Дополнительные сведения см. в следующих ресурсах:

  * <xref:signalr/scale>
  * [Документация по redis](https://redis.io/)
  * [Документация по кэш Redis для Azure](https://docs.microsoft.com/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* В приложении SignalR, установить `Microsoft.AspNetCore.SignalR.Redis` пакет NuGet. (Также `Microsoft.AspNetCore.SignalR.StackExchangeRedis` пакета, но, чтобы одно находилось в ASP.NET Core 2.2 и более поздних версий.)

* В `Startup.ConfigureServices` мы вызываем метод `AddRedis` после `AddSignalR`:

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* При необходимости настройте параметры:
 
  Большинство параметров можно задать в строке подключения или в [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) объекта. Параметры, указанные в `ConfigurationOptions` переопределяют значения в строке подключения.

  Приведенный ниже показано, как настроить параметры в `ConfigurationOptions` объекта. Этот пример добавляет префикс канала, чтобы несколько приложений могут совместно использовать тот же экземпляр Redis, как описано в следующем шаге.

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  В приведенном выше коде `options.Configuration` инициализируется с помощью все, что было указано в строке подключения.

::: moniker-end

::: moniker range="> aspnetcore-2.1"

* В приложении SignalR установите одно из следующих пакетов NuGet.

  * `Microsoft.AspNetCore.SignalR.StackExchangeRedis` -Зависит от StackExchange.Redis 2.X.X. Это рекомендуемый пакет для ASP.NET Core 2.2 и более поздних версий.
  * `Microsoft.AspNetCore.SignalR.Redis` -Зависит от StackExchange.Redis 1.X.X. Этот пакет не будет публиковаться в ASP.NET Core 3.0.

* В `Startup.ConfigureServices` мы вызываем метод `AddStackExchangeRedis` после `AddSignalR`:

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

* При необходимости настройте параметры:
 
  Большинство параметров можно задать в строке подключения или в [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) объекта. Параметры, указанные в `ConfigurationOptions` переопределяют значения в строке подключения.

  Приведенный ниже показано, как настроить параметры в `ConfigurationOptions` объекта. Этот пример добавляет префикс канала, чтобы несколько приложений могут совместно использовать тот же экземпляр Redis, как описано в следующем шаге.

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  В приведенном выше коде `options.Configuration` инициализируется с помощью все, что было указано в строке подключения.

  Сведения о параметрах Redis см. в разделе [StackExchange Redis документации](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).

::: moniker-end

* Если вы используете один сервер Redis для нескольких приложений SignalR, используйте префикс другой канал для каждого приложения SignalR.

  Задание префикса канала изолирует одно приложение SignalR от других пользователей, использующих разных канала префиксы. Если вы не назначены разные префиксы, сообщение, отправленное из одного приложения на все свои собственные клиентские компьютеры перейдет ко всем клиентам всех приложений, использующих сервер Redis в качестве объединительной платы.

* Настройка вашего сервера фермы балансировки нагрузки программного обеспечения для прикрепленных сеансов. Ниже приведены некоторые примеры документации о том, как это сделать.

  * [Службы IIS](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [HAProxy](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [Nginx](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [pfSense](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a>Ошибки сервера redis

Если сервер Redis выходит из строя, SignalR вызывает исключения, указывающие, что не удалось доставить сообщения. Некоторые типичные исключение сообщения:

* *Сбой записи сообщения*
* *Не удалось вызвать метод концентратора «Имя_метода»*
* *Не удалось подключиться к Redis*

SignalR не буфера сообщения для отправки их, когда сервер возобновляет работу. Теряются все сообщения, отправленные хотя сервер Redis не работает.

SignalR автоматически повторно подключается когда сервер Redis снова станет доступным.

### <a name="custom-behavior-for-connection-failures"></a>Пользовательское поведение для сбоев подключения

Ниже приведен пример, в котором демонстрируется обработка события сбоев подключения Redis.

::: moniker range="= aspnetcore-2.1"

```csharp
services.AddSignalR()
        .AddRedis(o =>
        {
            o.ConnectionFactory = async writer =>
            {
                var config = new ConfigurationOptions
                {
                    AbortOnConnectFail = false
                };
                config.EndPoints.Add(IPAddress.Loopback, 0);
                config.SetDefaultPorts();
                var connection = await ConnectionMultiplexer.ConnectAsync(config, writer);
                connection.ConnectionFailed += (_, e) =>
                {
                    Console.WriteLine("Connection to Redis failed.");
                };

                if (!connection.IsConnected)
                {
                    Console.WriteLine("Did not connect to Redis.");
                }

                return connection;
            };
        });
```

::: moniker-end

::: moniker range="> aspnetcore-2.1"

```csharp
services.AddSignalR()
        .AddMessagePackProtocol()
        .AddStackExchangeRedis(o =>
        {
            o.ConnectionFactory = async writer =>
            {
                var config = new ConfigurationOptions
                {
                    AbortOnConnectFail = false
                };
                config.EndPoints.Add(IPAddress.Loopback, 0);
                config.SetDefaultPorts();
                var connection = await ConnectionMultiplexer.ConnectAsync(config, writer);
                connection.ConnectionFailed += (_, e) =>
                {
                    Console.WriteLine("Connection to Redis failed.");
                };

                if (!connection.IsConnected)
                {
                    Console.WriteLine("Did not connect to Redis.");
                }

                return connection;
            };
        });
```

::: moniker-end

## <a name="redis-clustering"></a>Кластеризацию redis

[Кластеризация redis](https://redis.io/topics/cluster-spec) — это метод, для достижения высокого уровня доступности с помощью нескольких серверов Redis. Кластеризация не поддерживается официально, но она может работать.

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения см. в следующих ресурсах:

* <xref:signalr/scale>
* [Документация по redis](https://redis.io/documentation)
* [Документация по StackExchange Redis](https://stackexchange.github.io/StackExchange.Redis/)
* [Документация по кэш Redis для Azure](https://docs.microsoft.com/azure/redis-cache/)
