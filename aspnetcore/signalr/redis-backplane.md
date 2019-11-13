---
title: Redis Объединительная плата для ASP.NET Core SignalR горизонтальное масштабирование
author: bradygaster
description: Узнайте, как настроить объединительную Redis, чтобы включить горизонтальное масштабирование для приложения ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/redis-backplane
ms.openlocfilehash: 379d46fcaabb8eb0d04e521a5ad698229f947b7c
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963922"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-opno-locsignalr-scale-out"></a><span data-ttu-id="85dcf-103">Настройка объединительной платы Redis для ASP.NET Core SignalR горизонтальное масштабирование</span><span class="sxs-lookup"><span data-stu-id="85dcf-103">Set up a Redis backplane for ASP.NET Core SignalR scale-out</span></span>

<span data-ttu-id="85dcf-104">[Эндрю Стантон-медперсонала](https://twitter.com/anurse), [Брейди Гастер](https://twitter.com/bradygaster)и [Tom Dykstra)](https://github.com/tdykstra),</span><span class="sxs-lookup"><span data-stu-id="85dcf-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), and [Tom Dykstra](https://github.com/tdykstra),</span></span>

<span data-ttu-id="85dcf-105">В этой статье описываются SignalRособенности настройки сервера [Redis](https://redis.io/) для масштабирования приложения ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="85dcf-105">This article explains SignalR-specific aspects of setting up a [Redis](https://redis.io/) server to use for scaling out an ASP.NET Core SignalR app.</span></span>

## <a name="set-up-a-redis-backplane"></a><span data-ttu-id="85dcf-106">Настройка объединительной платы Redis</span><span class="sxs-lookup"><span data-stu-id="85dcf-106">Set up a Redis backplane</span></span>

* <span data-ttu-id="85dcf-107">Разверните сервер Redis.</span><span class="sxs-lookup"><span data-stu-id="85dcf-107">Deploy a Redis server.</span></span>

  > [!IMPORTANT] 
  > <span data-ttu-id="85dcf-108">Для использования в рабочей среде Redisная Объединительная плата рекомендуется только в том случае, если она выполняется в том же центре обработки данных, что и SignalRое приложение.</span><span class="sxs-lookup"><span data-stu-id="85dcf-108">For production use, a Redis backplane is recommended only when it runs in the same data center as the SignalR app.</span></span> <span data-ttu-id="85dcf-109">В противном случае задержка сети снижает производительность.</span><span class="sxs-lookup"><span data-stu-id="85dcf-109">Otherwise, network latency degrades performance.</span></span> <span data-ttu-id="85dcf-110">Если приложение SignalR выполняется в облаке Azure, мы рекомендуем использовать службу SignalR Azure вместо объединительной платы Redis.</span><span class="sxs-lookup"><span data-stu-id="85dcf-110">If your SignalR app is running in the Azure cloud, we recommend Azure SignalR Service instead of a Redis backplane.</span></span> <span data-ttu-id="85dcf-111">Службу кэша Redis для Azure можно использовать для сред разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="85dcf-111">You can use the Azure Redis Cache Service for development and test environments.</span></span>

  <span data-ttu-id="85dcf-112">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="85dcf-112">For more information, see the following resources:</span></span>

  * <xref:signalr/scale>
  * [<span data-ttu-id="85dcf-113">Документация по Redis</span><span class="sxs-lookup"><span data-stu-id="85dcf-113">Redis documentation</span></span>](https://redis.io/)
  * [<span data-ttu-id="85dcf-114">Документация по кэшу Redis для Azure</span><span class="sxs-lookup"><span data-stu-id="85dcf-114">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* <span data-ttu-id="85dcf-115">В приложении SignalR установите `Microsoft.AspNetCore.SignalR.Redis` пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="85dcf-115">In the SignalR app, install the `Microsoft.AspNetCore.SignalR.Redis` NuGet package.</span></span> <span data-ttu-id="85dcf-116">(Также существует пакет `Microsoft.AspNetCore.SignalR.StackExchangeRedis`, но он предназначен для ASP.NET Core 2,2 и более поздних версий).</span><span class="sxs-lookup"><span data-stu-id="85dcf-116">(There is also a `Microsoft.AspNetCore.SignalR.StackExchangeRedis` package, but that one is for ASP.NET Core 2.2 and later.)</span></span>

* <span data-ttu-id="85dcf-117">В методе `Startup.ConfigureServices` вызовите `AddRedis` после `AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="85dcf-117">In the `Startup.ConfigureServices` method, call `AddRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="85dcf-118">При необходимости настройте параметры:</span><span class="sxs-lookup"><span data-stu-id="85dcf-118">Configure options as needed:</span></span>
 
  <span data-ttu-id="85dcf-119">Большинство параметров можно задать в строке соединения или в объекте [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) .</span><span class="sxs-lookup"><span data-stu-id="85dcf-119">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="85dcf-120">Параметры, указанные в `ConfigurationOptions` переопределяют наборы, заданные в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="85dcf-120">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="85dcf-121">В следующем примере показано, как задать параметры в объекте `ConfigurationOptions`.</span><span class="sxs-lookup"><span data-stu-id="85dcf-121">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="85dcf-122">В этом примере добавляется префикс канала, чтобы несколько приложений могли совместно использовать один и тот же экземпляр Redis, как описано в следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="85dcf-122">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="85dcf-123">В приведенном выше коде `options.Configuration` инициализируется с помощью любого, указанного в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="85dcf-123">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.1"

* <span data-ttu-id="85dcf-124">В приложении SignalR установите один из следующих пакетов NuGet:</span><span class="sxs-lookup"><span data-stu-id="85dcf-124">In the SignalR app, install one of the following NuGet packages:</span></span>

  * <span data-ttu-id="85dcf-125">`Microsoft.AspNetCore.SignalR.StackExchangeRedis` — зависит от StackExchange. Redis 2. X.X.</span><span class="sxs-lookup"><span data-stu-id="85dcf-125">`Microsoft.AspNetCore.SignalR.StackExchangeRedis` - Depends on StackExchange.Redis 2.X.X.</span></span> <span data-ttu-id="85dcf-126">Это рекомендуемый пакет для ASP.NET Core 2,2 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="85dcf-126">This is the recommended package for ASP.NET Core 2.2 and later.</span></span>
  * <span data-ttu-id="85dcf-127">`Microsoft.AspNetCore.SignalR.Redis` — зависит от StackExchange. Redis 1. X.X.</span><span class="sxs-lookup"><span data-stu-id="85dcf-127">`Microsoft.AspNetCore.SignalR.Redis` - Depends on StackExchange.Redis 1.X.X.</span></span> <span data-ttu-id="85dcf-128">Этот пакет не будет поставляться в ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="85dcf-128">This package will not be shipping in ASP.NET Core 3.0.</span></span>

* <span data-ttu-id="85dcf-129">В методе `Startup.ConfigureServices` вызовите `AddStackExchangeRedis` после `AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="85dcf-129">In the `Startup.ConfigureServices` method, call `AddStackExchangeRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="85dcf-130">При необходимости настройте параметры:</span><span class="sxs-lookup"><span data-stu-id="85dcf-130">Configure options as needed:</span></span>
 
  <span data-ttu-id="85dcf-131">Большинство параметров можно задать в строке соединения или в объекте [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) .</span><span class="sxs-lookup"><span data-stu-id="85dcf-131">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="85dcf-132">Параметры, указанные в `ConfigurationOptions` переопределяют наборы, заданные в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="85dcf-132">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="85dcf-133">В следующем примере показано, как задать параметры в объекте `ConfigurationOptions`.</span><span class="sxs-lookup"><span data-stu-id="85dcf-133">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="85dcf-134">В этом примере добавляется префикс канала, чтобы несколько приложений могли совместно использовать один и тот же экземпляр Redis, как описано в следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="85dcf-134">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="85dcf-135">В приведенном выше коде `options.Configuration` инициализируется с помощью любого, указанного в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="85dcf-135">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

  <span data-ttu-id="85dcf-136">Сведения о параметрах Redis см. в [документации по StackExchange Redis](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span><span class="sxs-lookup"><span data-stu-id="85dcf-136">For information about Redis options, see the [StackExchange Redis documentation](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span></span>

::: moniker-end

* <span data-ttu-id="85dcf-137">Если вы используете один сервер Redis для нескольких SignalR приложений, используйте разные префиксы канала для каждого приложения SignalR.</span><span class="sxs-lookup"><span data-stu-id="85dcf-137">If you're using one Redis server for multiple SignalR apps, use a different channel prefix for each SignalR app.</span></span>

  <span data-ttu-id="85dcf-138">Установка префикса канала изолирует одно SignalRное приложение от других, использующих разные префиксы каналов.</span><span class="sxs-lookup"><span data-stu-id="85dcf-138">Setting a channel prefix isolates one SignalR app from others that use different channel prefixes.</span></span> <span data-ttu-id="85dcf-139">Если не назначить разные префиксы, сообщение, отправленное из одного приложения всем своим клиентам, будет передаваться всем клиентам всех приложений, использующих сервер Redis в качестве объединительной платы.</span><span class="sxs-lookup"><span data-stu-id="85dcf-139">If you don't assign different prefixes, a message sent from one app to all of its own clients will go to all clients of all apps that use the Redis server as a backplane.</span></span>

* <span data-ttu-id="85dcf-140">Настройте программное обеспечение балансировки нагрузки фермы серверов для закрепленных сеансов.</span><span class="sxs-lookup"><span data-stu-id="85dcf-140">Configure your server farm load balancing software for sticky sessions.</span></span> <span data-ttu-id="85dcf-141">Ниже приведены некоторые примеры документации.</span><span class="sxs-lookup"><span data-stu-id="85dcf-141">Here are some examples of documentation on how to do that:</span></span>

  * [<span data-ttu-id="85dcf-142">Службы IIS</span><span class="sxs-lookup"><span data-stu-id="85dcf-142">IIS</span></span>](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [<span data-ttu-id="85dcf-143">HAProxy</span><span class="sxs-lookup"><span data-stu-id="85dcf-143">HAProxy</span></span>](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [<span data-ttu-id="85dcf-144">Nginx</span><span class="sxs-lookup"><span data-stu-id="85dcf-144">Nginx</span></span>](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [<span data-ttu-id="85dcf-145">пфсенсе</span><span class="sxs-lookup"><span data-stu-id="85dcf-145">pfSense</span></span>](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a><span data-ttu-id="85dcf-146">Ошибки сервера Redis</span><span class="sxs-lookup"><span data-stu-id="85dcf-146">Redis server errors</span></span>

<span data-ttu-id="85dcf-147">Когда сервер Redis выйдет из строя, SignalR создает исключения, указывающие, что сообщения не будут доставлены.</span><span class="sxs-lookup"><span data-stu-id="85dcf-147">When a Redis server goes down, SignalR throws exceptions that indicate messages won't be delivered.</span></span> <span data-ttu-id="85dcf-148">Некоторые типичные сообщения об исключениях:</span><span class="sxs-lookup"><span data-stu-id="85dcf-148">Some typical exception messages:</span></span>

* <span data-ttu-id="85dcf-149">*Не удалось записать сообщение*</span><span class="sxs-lookup"><span data-stu-id="85dcf-149">*Failed writing message*</span></span>
* <span data-ttu-id="85dcf-150">*Не удалось вызвать метод концентратора "имя_метода"*</span><span class="sxs-lookup"><span data-stu-id="85dcf-150">*Failed to invoke hub method 'MethodName'*</span></span>
* <span data-ttu-id="85dcf-151">*Сбой подключения к Redis*</span><span class="sxs-lookup"><span data-stu-id="85dcf-151">*Connection to Redis failed*</span></span>

SignalR<span data-ttu-id="85dcf-152"> не помещает сообщения в буфер, чтобы отправить их при резервном копировании сервера.</span><span class="sxs-lookup"><span data-stu-id="85dcf-152"> doesn't buffer messages to send them when the server comes back up.</span></span> <span data-ttu-id="85dcf-153">Все сообщения, отправленные во время работы сервера Redis, теряются.</span><span class="sxs-lookup"><span data-stu-id="85dcf-153">Any messages sent while the Redis server is down are lost.</span></span>

SignalR<span data-ttu-id="85dcf-154"> автоматически повторно подключается, когда сервер Redis снова становится доступным.</span><span class="sxs-lookup"><span data-stu-id="85dcf-154"> automatically reconnects when the Redis server is available again.</span></span>

### <a name="custom-behavior-for-connection-failures"></a><span data-ttu-id="85dcf-155">Настраиваемое поведение для сбоев подключения</span><span class="sxs-lookup"><span data-stu-id="85dcf-155">Custom behavior for connection failures</span></span>

<span data-ttu-id="85dcf-156">Ниже приведен пример, демонстрирующий обработку событий сбоя подключения Redis.</span><span class="sxs-lookup"><span data-stu-id="85dcf-156">Here's an example that shows how to handle Redis connection failure events.</span></span>

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

## <a name="redis-clustering"></a><span data-ttu-id="85dcf-157">Кластеризация Redis</span><span class="sxs-lookup"><span data-stu-id="85dcf-157">Redis Clustering</span></span>

<span data-ttu-id="85dcf-158">[Кластеризация Redis](https://redis.io/topics/cluster-spec) — это метод достижения высокого уровня доступности с помощью нескольких серверов Redis.</span><span class="sxs-lookup"><span data-stu-id="85dcf-158">[Redis Clustering](https://redis.io/topics/cluster-spec) is a method for achieving high availability by using multiple Redis servers.</span></span> <span data-ttu-id="85dcf-159">Кластеризация официально не поддерживается, но может работать.</span><span class="sxs-lookup"><span data-stu-id="85dcf-159">Clustering isn't officially supported, but it might work.</span></span>

## <a name="next-steps"></a><span data-ttu-id="85dcf-160">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="85dcf-160">Next steps</span></span>

<span data-ttu-id="85dcf-161">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="85dcf-161">For more information, see the following resources:</span></span>

* <xref:signalr/scale>
* [<span data-ttu-id="85dcf-162">Документация по Redis</span><span class="sxs-lookup"><span data-stu-id="85dcf-162">Redis documentation</span></span>](https://redis.io/documentation)
* [<span data-ttu-id="85dcf-163">Документация по StackExchange Redis</span><span class="sxs-lookup"><span data-stu-id="85dcf-163">StackExchange Redis documentation</span></span>](https://stackexchange.github.io/StackExchange.Redis/)
* [<span data-ttu-id="85dcf-164">Документация по кэшу Redis для Azure</span><span class="sxs-lookup"><span data-stu-id="85dcf-164">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/azure/redis-cache/)
