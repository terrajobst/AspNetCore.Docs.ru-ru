---
title: Различия между SignalR и ASP.NET Core SignalR
author: tdykstra
description: Различия между SignalR и ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 09/10/2018
uid: signalr/version-differences
ms.openlocfilehash: ea2de2606a99de70fa645c0c42303525fea0a44e
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325540"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="a88cf-103">Различия между ASP.NET SignalR и ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="a88cf-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="a88cf-104">ASP.NET Core SignalR несовместим с клиентами или серверами для ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="a88cf-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="a88cf-105">В этой статье описаны возможности, которые были удалены или изменены в ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="a88cf-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="a88cf-106">Как определить версию SignalR</span><span class="sxs-lookup"><span data-stu-id="a88cf-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="a88cf-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="a88cf-107">ASP.NET SignalR</span></span> | <span data-ttu-id="a88cf-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="a88cf-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="a88cf-109">Пакет NuGet Server</span><span class="sxs-lookup"><span data-stu-id="a88cf-109">Server NuGet Package</span></span> | [<span data-ttu-id="a88cf-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="a88cf-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="a88cf-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="a88cf-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="a88cf-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="a88cf-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="a88cf-113">Клиентские пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="a88cf-113">Client NuGet Packages</span></span> | [<span data-ttu-id="a88cf-114">Microsoft.AspNet.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="a88cf-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="a88cf-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="a88cf-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="a88cf-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="a88cf-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="a88cf-117">Клиент npm пакета</span><span class="sxs-lookup"><span data-stu-id="a88cf-117">Client npm Package</span></span> | [<span data-ttu-id="a88cf-118">SignalR</span><span class="sxs-lookup"><span data-stu-id="a88cf-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="a88cf-119">Тип сервера приложений</span><span class="sxs-lookup"><span data-stu-id="a88cf-119">Server App Type</span></span> | <span data-ttu-id="a88cf-120">ASP.NET (System.Web) или тестированием OWIN</span><span class="sxs-lookup"><span data-stu-id="a88cf-120">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="a88cf-121">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a88cf-121">ASP.NET Core</span></span> |
| <span data-ttu-id="a88cf-122">Поддерживаемые серверные платформы</span><span class="sxs-lookup"><span data-stu-id="a88cf-122">Supported Server Platforms</span></span> | <span data-ttu-id="a88cf-123">.NET framework 4.5 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="a88cf-123">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="a88cf-124">.NET Framework 4.6.1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="a88cf-124">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="a88cf-125">.NET core 2.1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="a88cf-125">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="a88cf-126">Различия в функциях</span><span class="sxs-lookup"><span data-stu-id="a88cf-126">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="a88cf-127">Автоматическое повторных подключений</span><span class="sxs-lookup"><span data-stu-id="a88cf-127">Automatic reconnects</span></span>

<span data-ttu-id="a88cf-128">Автоматическое повторных подключений не поддерживаются в ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="a88cf-128">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="a88cf-129">При отключении клиента, необходимо явным образом запустить новое соединение для повторного подключения.</span><span class="sxs-lookup"><span data-stu-id="a88cf-129">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="a88cf-130">В ASP.NET SignalR SignalR пытается подключиться к серверу, если соединение разорвано.</span><span class="sxs-lookup"><span data-stu-id="a88cf-130">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span> 

### <a name="protocol-support"></a><span data-ttu-id="a88cf-131">Поддержка протоколов</span><span class="sxs-lookup"><span data-stu-id="a88cf-131">Protocol support</span></span>

<span data-ttu-id="a88cf-132">ASP.NET Core SignalR поддерживает JSON, а также новый двоичный протокол, основанный на [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="a88cf-132">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="a88cf-133">Кроме того можно создать пользовательские протоколы.</span><span class="sxs-lookup"><span data-stu-id="a88cf-133">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="a88cf-134">Различия на сервере</span><span class="sxs-lookup"><span data-stu-id="a88cf-134">Differences on the server</span></span>

<span data-ttu-id="a88cf-135">Серверные библиотеки ASP.NET Core SignalR, включаются в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) пакет, который является частью **веб-приложение ASP.NET Core** шаблона MVC и Razor проекты.</span><span class="sxs-lookup"><span data-stu-id="a88cf-135">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="a88cf-136">ASP.NET Core SignalR — по промежуточного слоя ASP.NET Core, поэтому он должен быть настроен путем вызова [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a88cf-136">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

<span data-ttu-id="a88cf-137">Чтобы настроить маршрутизацию, сопоставляются с маршруты концентраторов внутри [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) вызов метода `Startup.Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="a88cf-137">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="a88cf-138">Теперь требуется прикрепленные сеансы</span><span class="sxs-lookup"><span data-stu-id="a88cf-138">Sticky sessions now required</span></span>

<span data-ttu-id="a88cf-139">Из-за как горизонтального масштабирования работает в ASP.NET SignalR клиенты сможет повторно подключиться и отправлять сообщения на любой сервер в ферме.</span><span class="sxs-lookup"><span data-stu-id="a88cf-139">Because of how scale-out worked in ASP.NET SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="a88cf-140">Из-за изменения модели горизонтального масштабирования, а также не поддерживает повторных подключений это больше не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="a88cf-140">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="a88cf-141">Когда клиент подключается к серверу, оно должно взаимодействовать с тот же сервер в течение всего соединения.</span><span class="sxs-lookup"><span data-stu-id="a88cf-141">Once the client connects to the server, it must interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="a88cf-142">Единый центр на соединение</span><span class="sxs-lookup"><span data-stu-id="a88cf-142">Single hub per connection</span></span>

<span data-ttu-id="a88cf-143">В ASP.NET Core SignalR упрощена модель соединения.</span><span class="sxs-lookup"><span data-stu-id="a88cf-143">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="a88cf-144">Соединения устанавливаются непосредственно в одном центре, а не одного соединения, используемого для общий доступ для нескольких центров.</span><span class="sxs-lookup"><span data-stu-id="a88cf-144">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="a88cf-145">Потоковые операторы</span><span class="sxs-lookup"><span data-stu-id="a88cf-145">Streaming</span></span>

<span data-ttu-id="a88cf-146">ASP.NET Core SignalR теперь поддерживает [потоковой передачи данных](xref:signalr/streaming) от концентратора клиенту.</span><span class="sxs-lookup"><span data-stu-id="a88cf-146">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="a88cf-147">Регион</span><span class="sxs-lookup"><span data-stu-id="a88cf-147">State</span></span>

<span data-ttu-id="a88cf-148">Возможность передавать произвольное состояние между клиентами и концентратор (часто называемые HubState) был удален, а также поддержка сообщения о ходе выполнения.</span><span class="sxs-lookup"><span data-stu-id="a88cf-148">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="a88cf-149">На данный момент нет не существовал центра учетных записей-посредников.</span><span class="sxs-lookup"><span data-stu-id="a88cf-149">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="a88cf-150">Различия на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="a88cf-150">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="a88cf-151">TypeScript</span><span class="sxs-lookup"><span data-stu-id="a88cf-151">TypeScript</span></span>

<span data-ttu-id="a88cf-152">ASP.NET Core SignalR клиента создается на языке [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="a88cf-152">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="a88cf-153">Можно написать на языке JavaScript или TypeScript при использовании [клиента JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="a88cf-153">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="a88cf-154">Клиент JavaScript размещается в [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="a88cf-154">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="a88cf-155">В предыдущих версиях клиента JavaScript был получен через пакет NuGet в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a88cf-155">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="a88cf-156">Для версий ядра [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) пакет npm содержит библиотеки JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a88cf-156">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="a88cf-157">Этот пакет не входит в **веб-приложение ASP.NET Core** шаблона.</span><span class="sxs-lookup"><span data-stu-id="a88cf-157">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="a88cf-158">Получить и установить с помощью npm `@aspnet/signalr` пакета npm.</span><span class="sxs-lookup"><span data-stu-id="a88cf-158">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="a88cf-159">jQuery</span><span class="sxs-lookup"><span data-stu-id="a88cf-159">jQuery</span></span>

<span data-ttu-id="a88cf-160">Зависимость от jQuery был удален, однако проекты по-прежнему можете использовать jQuery.</span><span class="sxs-lookup"><span data-stu-id="a88cf-160">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="a88cf-161">Синтаксис метода клиента JavaScript</span><span class="sxs-lookup"><span data-stu-id="a88cf-161">JavaScript client method syntax</span></span>

<span data-ttu-id="a88cf-162">Синтаксис JavaScript был изменен с предыдущей версии SignalR.</span><span class="sxs-lookup"><span data-stu-id="a88cf-162">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="a88cf-163">Вместо использования `$connection` следует создать соединения с помощью [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span><span class="sxs-lookup"><span data-stu-id="a88cf-163">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="a88cf-164">Используйте [на](/javascript/api/@aspnet/signalr/HubConnection#on) метод для указания клиентских методов, которые могут вызывать концентратора.</span><span class="sxs-lookup"><span data-stu-id="a88cf-164">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="a88cf-165">После создания метода клиента, запустите подключение концентратора.</span><span class="sxs-lookup"><span data-stu-id="a88cf-165">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="a88cf-166">Цепочка [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) метод входа или обработки ошибок.</span><span class="sxs-lookup"><span data-stu-id="a88cf-166">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="a88cf-167">Центр учетных записей-посредников</span><span class="sxs-lookup"><span data-stu-id="a88cf-167">Hub proxies</span></span>

<span data-ttu-id="a88cf-168">Больше не будет автоматически создаются прокси концентратора.</span><span class="sxs-lookup"><span data-stu-id="a88cf-168">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="a88cf-169">Вместо этого передается имя метода [вызова](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API как строка.</span><span class="sxs-lookup"><span data-stu-id="a88cf-169">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="a88cf-170">.NET и других клиентов</span><span class="sxs-lookup"><span data-stu-id="a88cf-170">.NET and other clients</span></span>

<span data-ttu-id="a88cf-171">`Microsoft.AspNetCore.SignalR.Client` Пакет NuGet содержит клиентские библиотеки .NET для ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="a88cf-171">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="a88cf-172">Используйте [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) Создание и построение экземпляр подключения к концентратору.</span><span class="sxs-lookup"><span data-stu-id="a88cf-172">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="a88cf-173">Различия горизонтального масштабирования</span><span class="sxs-lookup"><span data-stu-id="a88cf-173">Scaleout differences</span></span>

<span data-ttu-id="a88cf-174">ASP.NET SignalR поддерживает SQL Server и Redis.</span><span class="sxs-lookup"><span data-stu-id="a88cf-174">ASP.NET SignalR supports both SQL Server and Redis.</span></span> <span data-ttu-id="a88cf-175">ASP.NET Core SignalR поддерживает служба Azure SignalR и Redis.</span><span class="sxs-lookup"><span data-stu-id="a88cf-175">ASP.NET Core SignalR supports both Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="a88cf-176">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a88cf-176">ASP.NET</span></span>

* [<span data-ttu-id="a88cf-177">Масштабирование SignalR с помощью служебной шины Azure</span><span class="sxs-lookup"><span data-stu-id="a88cf-177">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="a88cf-178">Масштабирование SignalR с помощью Redis</span><span class="sxs-lookup"><span data-stu-id="a88cf-178">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="a88cf-179">Масштабирование SignalR с помощью SQL Server</span><span class="sxs-lookup"><span data-stu-id="a88cf-179">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="a88cf-180">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a88cf-180">ASP.NET Core</span></span>

* [<span data-ttu-id="a88cf-181">Служба Azure SignalR</span><span class="sxs-lookup"><span data-stu-id="a88cf-181">Azure SignalR Service</span></span>](/azure/azure-signalr/)

## <a name="additional-resources"></a><span data-ttu-id="a88cf-182">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a88cf-182">Additional resources</span></span>

* [<span data-ttu-id="a88cf-183">Центры</span><span class="sxs-lookup"><span data-stu-id="a88cf-183">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="a88cf-184">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="a88cf-184">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="a88cf-185">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="a88cf-185">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="a88cf-186">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="a88cf-186">Supported platforms</span></span>](xref:signalr/supported-platforms)
