---
title: Различия между SignalR и ASP.NET Core SignalR
author: bradygaster
description: Различия между SignalR и ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/14/2018
uid: signalr/version-differences
ms.openlocfilehash: d140becaed5b4fc765b7a72571c7f5dce7f4e3d4
ms.sourcegitcommit: 28646e8ca62fb094db1557b5c0c02d5b45531824
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/23/2019
ms.locfileid: "67333448"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="577cc-103">Различия между ASP.NET SignalR и ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="577cc-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="577cc-104">ASP.NET Core SignalR несовместим с клиентами или серверами для ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="577cc-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="577cc-105">В этой статье описаны возможности, которые были удалены или изменены в ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="577cc-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="577cc-106">Как определить версию SignalR</span><span class="sxs-lookup"><span data-stu-id="577cc-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="577cc-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="577cc-107">ASP.NET SignalR</span></span> | <span data-ttu-id="577cc-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="577cc-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="577cc-109">Пакет NuGet Server</span><span class="sxs-lookup"><span data-stu-id="577cc-109">Server NuGet Package</span></span> | [<span data-ttu-id="577cc-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="577cc-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="577cc-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="577cc-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="577cc-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="577cc-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="577cc-113">Клиентские пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="577cc-113">Client NuGet Packages</span></span> | [<span data-ttu-id="577cc-114">Microsoft.AspNet.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="577cc-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="577cc-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="577cc-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="577cc-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="577cc-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="577cc-117">Клиент npm пакета</span><span class="sxs-lookup"><span data-stu-id="577cc-117">Client npm Package</span></span> | [<span data-ttu-id="577cc-118">SignalR</span><span class="sxs-lookup"><span data-stu-id="577cc-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="577cc-119">Клиент Java</span><span class="sxs-lookup"><span data-stu-id="577cc-119">Java Client</span></span> | <span data-ttu-id="577cc-120">[Репозиторий GitHub](https://github.com/SignalR/java-client) (устаревшая версия)</span><span class="sxs-lookup"><span data-stu-id="577cc-120">[GitHub Repository](https://github.com/SignalR/java-client) (deprecated)</span></span>  | <span data-ttu-id="577cc-121">Пакет maven [com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span><span class="sxs-lookup"><span data-stu-id="577cc-121">Maven package [com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span></span> |
| <span data-ttu-id="577cc-122">Тип сервера приложений</span><span class="sxs-lookup"><span data-stu-id="577cc-122">Server App Type</span></span> | <span data-ttu-id="577cc-123">ASP.NET (System.Web) или тестированием OWIN</span><span class="sxs-lookup"><span data-stu-id="577cc-123">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="577cc-124">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="577cc-124">ASP.NET Core</span></span> |
| <span data-ttu-id="577cc-125">Поддерживаемые серверные платформы</span><span class="sxs-lookup"><span data-stu-id="577cc-125">Supported Server Platforms</span></span> | <span data-ttu-id="577cc-126">.NET framework 4.5 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="577cc-126">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="577cc-127">.NET Framework 4.6.1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="577cc-127">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="577cc-128">.NET core 2.1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="577cc-128">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="577cc-129">Различия в функциях</span><span class="sxs-lookup"><span data-stu-id="577cc-129">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="577cc-130">Автоматическое повторных подключений</span><span class="sxs-lookup"><span data-stu-id="577cc-130">Automatic reconnects</span></span>

<span data-ttu-id="577cc-131">Автоматическое повторных подключений не поддерживаются в ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="577cc-131">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="577cc-132">При отключении клиента, необходимо явным образом запустить новое соединение для повторного подключения.</span><span class="sxs-lookup"><span data-stu-id="577cc-132">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="577cc-133">В ASP.NET SignalR SignalR пытается подключиться к серверу, если соединение разорвано.</span><span class="sxs-lookup"><span data-stu-id="577cc-133">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span> 

### <a name="protocol-support"></a><span data-ttu-id="577cc-134">Поддержка протоколов</span><span class="sxs-lookup"><span data-stu-id="577cc-134">Protocol support</span></span>

<span data-ttu-id="577cc-135">ASP.NET Core SignalR поддерживает JSON, а также новый двоичный протокол, основанный на [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="577cc-135">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="577cc-136">Кроме того можно создать пользовательские протоколы.</span><span class="sxs-lookup"><span data-stu-id="577cc-136">Additionally, custom protocols can be created.</span></span>

### <a name="transports"></a><span data-ttu-id="577cc-137">Транспорты</span><span class="sxs-lookup"><span data-stu-id="577cc-137">Transports</span></span>

<span data-ttu-id="577cc-138">Транспорт навсегда кадров не поддерживается в ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="577cc-138">The Forever Frame transport is not supported in ASP.NET Core SignalR.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="577cc-139">Различия на сервере</span><span class="sxs-lookup"><span data-stu-id="577cc-139">Differences on the server</span></span>

<span data-ttu-id="577cc-140">Серверные библиотеки ASP.NET Core SignalR, включаются в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) пакет, который является частью **веб-приложение ASP.NET Core** шаблона MVC и Razor проекты.</span><span class="sxs-lookup"><span data-stu-id="577cc-140">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="577cc-141">ASP.NET Core SignalR — по промежуточного слоя ASP.NET Core, поэтому он должен быть настроен путем вызова [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="577cc-141">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

<span data-ttu-id="577cc-142">Чтобы настроить маршрутизацию, сопоставляются с маршруты концентраторов внутри [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) вызов метода `Startup.Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="577cc-142">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions"></a><span data-ttu-id="577cc-143">Прикрепленные сеансы</span><span class="sxs-lookup"><span data-stu-id="577cc-143">Sticky sessions</span></span>

<span data-ttu-id="577cc-144">Модель горизонтального масштабирования для ASP.NET SignalR позволяет клиентам подключение и отправку сообщений на любой сервер в ферме.</span><span class="sxs-lookup"><span data-stu-id="577cc-144">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="577cc-145">В ASP.NET Core SignalR клиент должен взаимодействовать с тот же сервер в течение всего соединения.</span><span class="sxs-lookup"><span data-stu-id="577cc-145">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="577cc-146">Для горизонтального масштабирования с помощью Redis, это означает, что прикрепленные сеансы являются обязательными.</span><span class="sxs-lookup"><span data-stu-id="577cc-146">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="577cc-147">Для горизонтального масштабирования с помощью [служба Azure SignalR](/azure/azure-signalr/), прикрепленные сеансы не требуются, так как служба обрабатывает подключения к клиентам.</span><span class="sxs-lookup"><span data-stu-id="577cc-147">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span> 

### <a name="single-hub-per-connection"></a><span data-ttu-id="577cc-148">Единый центр на соединение</span><span class="sxs-lookup"><span data-stu-id="577cc-148">Single hub per connection</span></span>

<span data-ttu-id="577cc-149">В ASP.NET Core SignalR упрощена модель соединения.</span><span class="sxs-lookup"><span data-stu-id="577cc-149">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="577cc-150">Соединения устанавливаются непосредственно в одном центре, а не одного соединения, используемого для общий доступ для нескольких центров.</span><span class="sxs-lookup"><span data-stu-id="577cc-150">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="577cc-151">Потоковые операторы</span><span class="sxs-lookup"><span data-stu-id="577cc-151">Streaming</span></span>

<span data-ttu-id="577cc-152">ASP.NET Core SignalR теперь поддерживает [потоковой передачи данных](xref:signalr/streaming) от концентратора клиенту.</span><span class="sxs-lookup"><span data-stu-id="577cc-152">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="577cc-153">Регион</span><span class="sxs-lookup"><span data-stu-id="577cc-153">State</span></span>

<span data-ttu-id="577cc-154">Возможность передавать произвольное состояние между клиентами и концентратор (часто называемые HubState) был удален, а также поддержка сообщения о ходе выполнения.</span><span class="sxs-lookup"><span data-stu-id="577cc-154">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="577cc-155">На данный момент нет не существовал центра учетных записей-посредников.</span><span class="sxs-lookup"><span data-stu-id="577cc-155">There is no counterpart of hub proxies at the moment.</span></span>

### <a name="persistentconnection-removal"></a><span data-ttu-id="577cc-156">Удаление PersistentConnection</span><span class="sxs-lookup"><span data-stu-id="577cc-156">PersistentConnection removal</span></span>

<span data-ttu-id="577cc-157">В ASP.NET Core SignalR [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) класс был удален.</span><span class="sxs-lookup"><span data-stu-id="577cc-157">In ASP.NET Core SignalR, the [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) class has been removed.</span></span> 

### <a name="globalhost"></a><span data-ttu-id="577cc-158">GlobalHost</span><span class="sxs-lookup"><span data-stu-id="577cc-158">GlobalHost</span></span>

<span data-ttu-id="577cc-159">ASP.NET Core имеет внедрение зависимостей (DI), встроенные в платформу.</span><span class="sxs-lookup"><span data-stu-id="577cc-159">ASP.NET Core has dependency injection (DI) built into the framework.</span></span> <span data-ttu-id="577cc-160">Службы могут использовать внедрение Зависимостей для доступа к [HubContext](xref:signalr/hubcontext).</span><span class="sxs-lookup"><span data-stu-id="577cc-160">Services can use DI to access the [HubContext](xref:signalr/hubcontext).</span></span> <span data-ttu-id="577cc-161">`GlobalHost` Объект, который используется в ASP.NET SignalR для получения `HubContext` не существует в ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="577cc-161">The `GlobalHost` object that is used in ASP.NET SignalR to get a `HubContext` doesn't exist in ASP.NET Core SignalR.</span></span>

### <a name="hubpipeline"></a><span data-ttu-id="577cc-162">HubPipeline</span><span class="sxs-lookup"><span data-stu-id="577cc-162">HubPipeline</span></span>

<span data-ttu-id="577cc-163">ASP.NET Core SignalR не поддерживает `HubPipeline` модулей.</span><span class="sxs-lookup"><span data-stu-id="577cc-163">ASP.NET Core SignalR doesn't have support for `HubPipeline` modules.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="577cc-164">Различия на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="577cc-164">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="577cc-165">TypeScript</span><span class="sxs-lookup"><span data-stu-id="577cc-165">TypeScript</span></span>

<span data-ttu-id="577cc-166">ASP.NET Core SignalR клиента создается на языке [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="577cc-166">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="577cc-167">Можно написать на языке JavaScript или TypeScript при использовании [клиента JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="577cc-167">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="577cc-168">Клиент JavaScript размещается в [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="577cc-168">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="577cc-169">В предыдущих версиях клиента JavaScript был получен через пакет NuGet в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="577cc-169">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="577cc-170">Для версий ядра [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) пакет npm содержит библиотеки JavaScript.</span><span class="sxs-lookup"><span data-stu-id="577cc-170">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="577cc-171">Этот пакет не входит в **веб-приложение ASP.NET Core** шаблона.</span><span class="sxs-lookup"><span data-stu-id="577cc-171">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="577cc-172">Получить и установить с помощью npm `@aspnet/signalr` пакета npm.</span><span class="sxs-lookup"><span data-stu-id="577cc-172">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="577cc-173">jQuery</span><span class="sxs-lookup"><span data-stu-id="577cc-173">jQuery</span></span>

<span data-ttu-id="577cc-174">Зависимость от jQuery был удален, однако проекты по-прежнему можете использовать jQuery.</span><span class="sxs-lookup"><span data-stu-id="577cc-174">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="internet-explorer-support"></a><span data-ttu-id="577cc-175">Поддержка Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="577cc-175">Internet Explorer support</span></span>

<span data-ttu-id="577cc-176">ASP.NET Core SignalR требуется Microsoft Internet Explorer 11 или более поздней версии (ASP.NET SignalR поддерживается Microsoft Internet Explorer 8 и более поздние версии).</span><span class="sxs-lookup"><span data-stu-id="577cc-176">ASP.NET Core SignalR requires Microsoft Internet Explorer 11 or later (ASP.NET SignalR supported Microsoft Internet Explorer 8 and later).</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="577cc-177">Синтаксис метода клиента JavaScript</span><span class="sxs-lookup"><span data-stu-id="577cc-177">JavaScript client method syntax</span></span>

<span data-ttu-id="577cc-178">Синтаксис JavaScript был изменен с предыдущей версии SignalR.</span><span class="sxs-lookup"><span data-stu-id="577cc-178">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="577cc-179">Вместо использования `$connection` следует создать соединения с помощью [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span><span class="sxs-lookup"><span data-stu-id="577cc-179">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="577cc-180">Используйте [на](/javascript/api/@aspnet/signalr/HubConnection#on) метод для указания клиентских методов, которые могут вызывать концентратора.</span><span class="sxs-lookup"><span data-stu-id="577cc-180">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="577cc-181">После создания метода клиента, запустите подключение концентратора.</span><span class="sxs-lookup"><span data-stu-id="577cc-181">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="577cc-182">Цепочка [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) метод входа или обработки ошибок.</span><span class="sxs-lookup"><span data-stu-id="577cc-182">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="577cc-183">Центр учетных записей-посредников</span><span class="sxs-lookup"><span data-stu-id="577cc-183">Hub proxies</span></span>

<span data-ttu-id="577cc-184">Больше не будет автоматически создаются прокси концентратора.</span><span class="sxs-lookup"><span data-stu-id="577cc-184">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="577cc-185">Вместо этого передается имя метода [вызова](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API как строка.</span><span class="sxs-lookup"><span data-stu-id="577cc-185">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="577cc-186">.NET и других клиентов</span><span class="sxs-lookup"><span data-stu-id="577cc-186">.NET and other clients</span></span>

<span data-ttu-id="577cc-187">`Microsoft.AspNetCore.SignalR.Client` Пакет NuGet содержит клиентские библиотеки .NET для ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="577cc-187">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="577cc-188">Используйте [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) Создание и построение экземпляр подключения к концентратору.</span><span class="sxs-lookup"><span data-stu-id="577cc-188">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="577cc-189">Различия горизонтального масштабирования</span><span class="sxs-lookup"><span data-stu-id="577cc-189">Scaleout differences</span></span>

<span data-ttu-id="577cc-190">ASP.NET SignalR поддерживает SQL Server и Redis.</span><span class="sxs-lookup"><span data-stu-id="577cc-190">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="577cc-191">ASP.NET Core SignalR поддерживает служба Azure SignalR и Redis.</span><span class="sxs-lookup"><span data-stu-id="577cc-191">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="577cc-192">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="577cc-192">ASP.NET</span></span>

* [<span data-ttu-id="577cc-193">Масштабирование SignalR с помощью служебной шины Azure</span><span class="sxs-lookup"><span data-stu-id="577cc-193">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="577cc-194">Масштабирование SignalR с помощью Redis</span><span class="sxs-lookup"><span data-stu-id="577cc-194">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="577cc-195">Масштабирование SignalR с помощью SQL Server</span><span class="sxs-lookup"><span data-stu-id="577cc-195">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="577cc-196">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="577cc-196">ASP.NET Core</span></span>

* [<span data-ttu-id="577cc-197">Служба Azure SignalR</span><span class="sxs-lookup"><span data-stu-id="577cc-197">Azure SignalR Service</span></span>](/azure/azure-signalr/)
* [<span data-ttu-id="577cc-198">Redis объединительной платы</span><span class="sxs-lookup"><span data-stu-id="577cc-198">Redis Backplane</span></span>](xref:signalr/redis-backplane)

## <a name="additional-resources"></a><span data-ttu-id="577cc-199">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="577cc-199">Additional resources</span></span>

* [<span data-ttu-id="577cc-200">Центры</span><span class="sxs-lookup"><span data-stu-id="577cc-200">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="577cc-201">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="577cc-201">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="577cc-202">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="577cc-202">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="577cc-203">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="577cc-203">Supported platforms</span></span>](xref:signalr/supported-platforms)
