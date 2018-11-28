---
title: Redis объединительной платы для горизонтального масштабирования ASP.NET Core SignalR
author: tdykstra
description: Узнайте, как настройка Redis объединительной платы для обеспечения горизонтального масштабирования для приложения ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/redis-backplane
ms.openlocfilehash: c8b09c0d482da344b54d167c0c9757167eaa6186
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2018
ms.locfileid: "52452984"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-signalr-scale-out"></a><span data-ttu-id="57666-103">Настройка Redis объединительной платы для горизонтального масштабирования ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="57666-103">Set up a Redis backplane for ASP.NET Core SignalR scale-out</span></span>

<span data-ttu-id="57666-104">По [Andrew Stanton медсестра](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), и [том Дайкстра](https://github.com/tdykstra),</span><span class="sxs-lookup"><span data-stu-id="57666-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), and [Tom Dykstra](https://github.com/tdykstra),</span></span>

<span data-ttu-id="57666-105">В этой статье объясняется SignalR особенности настройки [Redis](https://redis.io/) сервер для масштабирования приложения ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="57666-105">This article explains SignalR-specific aspects of setting up a [Redis](https://redis.io/) server to use for scaling out an ASP.NET Core SignalR app.</span></span>

## <a name="set-up-a-redis-backplane"></a><span data-ttu-id="57666-106">Настройка Redis объединительной платы</span><span class="sxs-lookup"><span data-stu-id="57666-106">Set up a Redis backplane</span></span>

* <span data-ttu-id="57666-107">Развертывание сервера Redis.</span><span class="sxs-lookup"><span data-stu-id="57666-107">Deploy a Redis server.</span></span>

  <span data-ttu-id="57666-108">Для использования в рабочей среде объединительной Redis рекомендуется только в локальной инфраструктуре.</span><span class="sxs-lookup"><span data-stu-id="57666-108">For production use, a Redis backplane is recommended only for on-premises infrastructure.</span></span> <span data-ttu-id="57666-109">Чтобы свести к минимуму задержки, сервер Redis должно находиться в одном центре обработки данных, что и приложение SignalR.</span><span class="sxs-lookup"><span data-stu-id="57666-109">To minimize latency, the Redis server should be in the same data center as the SignalR app.</span></span> <span data-ttu-id="57666-110">Если ваше приложение SignalR работает в облаке Azure, мы рекомендуем служба Azure SignalR вместо объединительной Redis.</span><span class="sxs-lookup"><span data-stu-id="57666-110">If your SignalR app is running in the Azure cloud, we recommend Azure SignalR Service instead of a Redis backplane.</span></span> <span data-ttu-id="57666-111">Можно использовать службу кэша Redis Azure для разработки и тестовые среды.</span><span class="sxs-lookup"><span data-stu-id="57666-111">You can use the Azure Redis Cache Service for development and test environments.</span></span> <span data-ttu-id="57666-112">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="57666-112">For more information, see the following resources:</span></span>

  * <xref:signalr/scale>
  * [<span data-ttu-id="57666-113">Документация по redis</span><span class="sxs-lookup"><span data-stu-id="57666-113">Redis documentation</span></span>](https://redis.io/)
  * [<span data-ttu-id="57666-114">Документация по кэш Redis для Azure</span><span class="sxs-lookup"><span data-stu-id="57666-114">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/en-us/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* <span data-ttu-id="57666-115">В приложении SignalR, установить `Microsoft.AspNetCore.SignalR.Redis` пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="57666-115">In the SignalR app, install the `Microsoft.AspNetCore.SignalR.Redis` NuGet package.</span></span>

* <span data-ttu-id="57666-116">В `Startup.ConfigureServices` мы вызываем метод `AddRedis` после `AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="57666-116">In the `Startup.ConfigureServices` method, call `AddRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="57666-117">При необходимости настройте параметры:</span><span class="sxs-lookup"><span data-stu-id="57666-117">Configure options as needed:</span></span>
 
  <span data-ttu-id="57666-118">Большинство параметров можно задать в строке подключения или в [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) объекта.</span><span class="sxs-lookup"><span data-stu-id="57666-118">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="57666-119">Параметры, указанные в `ConfigurationOptions` переопределяют значения в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="57666-119">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="57666-120">Приведенный ниже показано, как настроить параметры в `ConfigurationOptions` объекта.</span><span class="sxs-lookup"><span data-stu-id="57666-120">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="57666-121">Этот пример добавляет префикс канала, чтобы несколько приложений могут совместно использовать тот же экземпляр Redis, как описано в следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="57666-121">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="57666-122">В приведенном выше коде `options.Configuration` инициализируется с помощью все, что было указано в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="57666-122">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.1"

* <span data-ttu-id="57666-123">В приложении SignalR, установить `Microsoft.AspNetCore.SignalR.StackExchangeRedis` пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="57666-123">In the SignalR app, install the `Microsoft.AspNetCore.SignalR.StackExchangeRedis` NuGet package.</span></span>

* <span data-ttu-id="57666-124">В `Startup.ConfigureServices` мы вызываем метод `AddStackExchangeRedis` после `AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="57666-124">In the `Startup.ConfigureServices` method, call `AddStackExchangeRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="57666-125">При необходимости настройте параметры:</span><span class="sxs-lookup"><span data-stu-id="57666-125">Configure options as needed:</span></span>
 
  <span data-ttu-id="57666-126">Большинство параметров можно задать в строке подключения или в [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) объекта.</span><span class="sxs-lookup"><span data-stu-id="57666-126">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="57666-127">Параметры, указанные в `ConfigurationOptions` переопределяют значения в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="57666-127">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="57666-128">Приведенный ниже показано, как настроить параметры в `ConfigurationOptions` объекта.</span><span class="sxs-lookup"><span data-stu-id="57666-128">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="57666-129">Этот пример добавляет префикс канала, чтобы несколько приложений могут совместно использовать тот же экземпляр Redis, как описано в следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="57666-129">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="57666-130">В приведенном выше коде `options.Configuration` инициализируется с помощью все, что было указано в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="57666-130">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

  <span data-ttu-id="57666-131">Сведения о параметрах Redis см. в разделе [StackExchange Redis документации](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span><span class="sxs-lookup"><span data-stu-id="57666-131">For information about Redis options, see the [StackExchange Redis documentation](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span></span>

::: moniker-end

* <span data-ttu-id="57666-132">Если вы используете один сервер Redis для нескольких приложений SignalR, используйте префикс другой канал для каждого приложения SignalR.</span><span class="sxs-lookup"><span data-stu-id="57666-132">If you're using one Redis server for multiple SignalR apps, use a different channel prefix for each SignalR app.</span></span>

  <span data-ttu-id="57666-133">Задание префикса канала изолирует одно приложение SignalR от других пользователей, использующих разных канала префиксы.</span><span class="sxs-lookup"><span data-stu-id="57666-133">Setting a channel prefix isolates one SignalR app from others that use different channel prefixes.</span></span> <span data-ttu-id="57666-134">Если вы не назначены разные префиксы, сообщение, отправленное из одного приложения на все свои собственные клиентские компьютеры перейдет ко всем клиентам всех приложений, использующих сервер Redis в качестве объединительной платы.</span><span class="sxs-lookup"><span data-stu-id="57666-134">If you don't assign different prefixes, a message sent from one app to all of its own clients will go to all clients of all apps that use the Redis server as a backplane.</span></span>

* <span data-ttu-id="57666-135">Настройка вашего сервера фермы балансировки нагрузки программного обеспечения для прикрепленных сеансов.</span><span class="sxs-lookup"><span data-stu-id="57666-135">Configure your server farm load balancing software for sticky sessions.</span></span> <span data-ttu-id="57666-136">Ниже приведены некоторые примеры документации о том, как это сделать.</span><span class="sxs-lookup"><span data-stu-id="57666-136">Here are some examples of documentation on how to do that:</span></span>

  * [<span data-ttu-id="57666-137">Службы IIS</span><span class="sxs-lookup"><span data-stu-id="57666-137">IIS</span></span>](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [<span data-ttu-id="57666-138">HAProxy</span><span class="sxs-lookup"><span data-stu-id="57666-138">HAProxy</span></span>](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [<span data-ttu-id="57666-139">Nginx</span><span class="sxs-lookup"><span data-stu-id="57666-139">Nginx</span></span>](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [<span data-ttu-id="57666-140">pfSense</span><span class="sxs-lookup"><span data-stu-id="57666-140">pfSense</span></span>](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a><span data-ttu-id="57666-141">Ошибки сервера redis</span><span class="sxs-lookup"><span data-stu-id="57666-141">Redis server errors</span></span>

<span data-ttu-id="57666-142">Если сервер Redis выходит из строя, SignalR вызывает исключения, указывающие, что не удалось доставить сообщения.</span><span class="sxs-lookup"><span data-stu-id="57666-142">When a Redis server goes down, SignalR throws exceptions that indicate messages won't be delivered.</span></span> <span data-ttu-id="57666-143">Некоторые типичные исключение сообщения:</span><span class="sxs-lookup"><span data-stu-id="57666-143">Some typical exception messages:</span></span>

* <span data-ttu-id="57666-144">*Сбой записи сообщения*</span><span class="sxs-lookup"><span data-stu-id="57666-144">*Failed writing message*</span></span>
* <span data-ttu-id="57666-145">*Не удалось вызвать метод концентратора «Имя_метода»*</span><span class="sxs-lookup"><span data-stu-id="57666-145">*Failed to invoke hub method 'MethodName'*</span></span>
* <span data-ttu-id="57666-146">*Не удалось подключиться к Redis*</span><span class="sxs-lookup"><span data-stu-id="57666-146">*Connection to Redis failed*</span></span>

<span data-ttu-id="57666-147">SignalR не буфера сообщения для отправки их, когда сервер возобновляет работу.</span><span class="sxs-lookup"><span data-stu-id="57666-147">SignalR doesn't buffer messages to send them when the server comes back up.</span></span> <span data-ttu-id="57666-148">Теряются все сообщения, отправленные хотя сервер Redis не работает.</span><span class="sxs-lookup"><span data-stu-id="57666-148">Any messages sent while the Redis server is down are lost.</span></span>

<span data-ttu-id="57666-149">SignalR автоматически повторно подключается когда сервер Redis снова станет доступным.</span><span class="sxs-lookup"><span data-stu-id="57666-149">SignalR automatically reconnects when the Redis server is available again.</span></span>

### <a name="custom-behavior-for-connection-failures"></a><span data-ttu-id="57666-150">Пользовательское поведение для сбоев подключения</span><span class="sxs-lookup"><span data-stu-id="57666-150">Custom behavior for connection failures</span></span>

<span data-ttu-id="57666-151">Ниже приведен пример, в котором демонстрируется обработка события сбоев подключения Redis.</span><span class="sxs-lookup"><span data-stu-id="57666-151">Here's an example that shows how to handle Redis connection failure events.</span></span>

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

## <a name="clustering"></a><span data-ttu-id="57666-152">Кластеризация</span><span class="sxs-lookup"><span data-stu-id="57666-152">Clustering</span></span>

<span data-ttu-id="57666-153">Кластер — это метод, для достижения высокого уровня доступности с помощью нескольких серверов Redis.</span><span class="sxs-lookup"><span data-stu-id="57666-153">Clustering is a method for achieving high availability by using multiple Redis servers.</span></span> <span data-ttu-id="57666-154">Кластеризация не поддерживается официально, но она может работать.</span><span class="sxs-lookup"><span data-stu-id="57666-154">Clustering isn't officially supported, but it might work.</span></span>

## <a name="next-steps"></a><span data-ttu-id="57666-155">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="57666-155">Next steps</span></span>

<span data-ttu-id="57666-156">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="57666-156">For more information, see the following resources:</span></span>

* <xref:signalr/scale>
* [<span data-ttu-id="57666-157">Документация по redis</span><span class="sxs-lookup"><span data-stu-id="57666-157">Redis documentation</span></span>](https://redis.io/documentation)
* [<span data-ttu-id="57666-158">Документация по StackExchange Redis</span><span class="sxs-lookup"><span data-stu-id="57666-158">StackExchange Redis documentation</span></span>](https://stackexchange.github.io/StackExchange.Redis/)
* [<span data-ttu-id="57666-159">Документация по кэш Redis для Azure</span><span class="sxs-lookup"><span data-stu-id="57666-159">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/en-us/azure/redis-cache/)
