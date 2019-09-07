---
title: Различия между SignalR и ASP.NET Core SignalR
author: bradygaster
description: Различия между SignalR и ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/14/2018
uid: signalr/version-differences
ms.openlocfilehash: 70b09493d9b4c96c897465d60e53e93a793c42f9
ms.sourcegitcommit: 387cf29f5d5addef2cbc70670a11d612806b36b2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70746538"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="fd5f6-103">Различия между ASP.NET SignalR и SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fd5f6-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="fd5f6-104">ASP.NET Core SignalR несовместим с клиентами или серверами для ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="fd5f6-105">В этой статье подробно описаны функции, которые были удалены или изменены в ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="fd5f6-106">Как опознать версию SignalR</span><span class="sxs-lookup"><span data-stu-id="fd5f6-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="fd5f6-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="fd5f6-107">ASP.NET SignalR</span></span> | <span data-ttu-id="fd5f6-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="fd5f6-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="fd5f6-109">Пакет NuGet сервера</span><span class="sxs-lookup"><span data-stu-id="fd5f6-109">Server NuGet Package</span></span> | [<span data-ttu-id="fd5f6-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="fd5f6-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="fd5f6-111">[Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="fd5f6-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="fd5f6-112">[Microsoft. AspNetCore. SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="fd5f6-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="fd5f6-113">Клиентские пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="fd5f6-113">Client NuGet Packages</span></span> | [<span data-ttu-id="fd5f6-114">Microsoft. AspNet. SignalR. Client</span><span class="sxs-lookup"><span data-stu-id="fd5f6-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="fd5f6-115">Microsoft. AspNet. SignalR. JS</span><span class="sxs-lookup"><span data-stu-id="fd5f6-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="fd5f6-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="fd5f6-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="fd5f6-117">Клиентский пакет NPM</span><span class="sxs-lookup"><span data-stu-id="fd5f6-117">Client npm Package</span></span> | [<span data-ttu-id="fd5f6-118">SignalR</span><span class="sxs-lookup"><span data-stu-id="fd5f6-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="fd5f6-119">Клиент Java</span><span class="sxs-lookup"><span data-stu-id="fd5f6-119">Java Client</span></span> | <span data-ttu-id="fd5f6-120">[Репозиторий GitHub](https://github.com/SignalR/java-client) не рекомендуется</span><span class="sxs-lookup"><span data-stu-id="fd5f6-120">[GitHub Repository](https://github.com/SignalR/java-client) (deprecated)</span></span>  | <span data-ttu-id="fd5f6-121">Пакет Maven [com. Microsoft. SignalR](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span><span class="sxs-lookup"><span data-stu-id="fd5f6-121">Maven package [com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span></span> |
| <span data-ttu-id="fd5f6-122">Тип серверного приложения</span><span class="sxs-lookup"><span data-stu-id="fd5f6-122">Server App Type</span></span> | <span data-ttu-id="fd5f6-123">ASP.NET (System. Web) или OWIN Self-Host</span><span class="sxs-lookup"><span data-stu-id="fd5f6-123">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="fd5f6-124">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fd5f6-124">ASP.NET Core</span></span> |
| <span data-ttu-id="fd5f6-125">Поддерживаемые серверные платформы</span><span class="sxs-lookup"><span data-stu-id="fd5f6-125">Supported Server Platforms</span></span> | <span data-ttu-id="fd5f6-126">.NET Framework 4,5 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="fd5f6-126">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="fd5f6-127">.NET Framework 4.6.1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="fd5f6-127">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="fd5f6-128">.NET Core 2,1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="fd5f6-128">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="fd5f6-129">Различия в функциях</span><span class="sxs-lookup"><span data-stu-id="fd5f6-129">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="fd5f6-130">Автоматические повторное подключения</span><span class="sxs-lookup"><span data-stu-id="fd5f6-130">Automatic reconnects</span></span>

<span data-ttu-id="fd5f6-131">Автоматические повторное подключения не поддерживаются в ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-131">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="fd5f6-132">Если Клиент отключен, пользователь должен явно запустить новое подключение, если требуется повторное подключение.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-132">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="fd5f6-133">В ASP.NET SignalR SignalR пытается повторно подключиться к серверу при разрыве соединения.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-133">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="fd5f6-134">Поддержка протокола</span><span class="sxs-lookup"><span data-stu-id="fd5f6-134">Protocol support</span></span>

<span data-ttu-id="fd5f6-135">ASP.NET Core SignalR поддерживает JSON, а также новый двоичный протокол на основе [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="fd5f6-135">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="fd5f6-136">Кроме того, можно создавать пользовательские протоколы.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-136">Additionally, custom protocols can be created.</span></span>

### <a name="transports"></a><span data-ttu-id="fd5f6-137">Транспорты</span><span class="sxs-lookup"><span data-stu-id="fd5f6-137">Transports</span></span>

<span data-ttu-id="fd5f6-138">Непостоянное многокадровый транспорт не поддерживается в ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-138">The Forever Frame transport is not supported in ASP.NET Core SignalR.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="fd5f6-139">Различия на сервере</span><span class="sxs-lookup"><span data-stu-id="fd5f6-139">Differences on the server</span></span>

<span data-ttu-id="fd5f6-140">Серверные библиотеки ASP.NET Core SignalR включены в пакет [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app) , который входит в состав шаблона **ASP.NET Core веб-приложения** для проектов Razor и MVC.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-140">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="fd5f6-141">ASP.NET Core SignalR является ASP.NET Core по промежуточного слоя, поэтому его необходимо настроить, вызвав [аддсигналр](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-141">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fd5f6-142">Чтобы настроить маршрутизацию, сопоставьте маршруты с концентраторами внутри вызова метода [усиндпоинтс](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) в `Startup.Configure` методе.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-142">To configure routing, map routes to hubs inside the [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) method call in the `Startup.Configure` method.</span></span>


```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="fd5f6-143">Чтобы настроить маршрутизацию, сопоставьте маршруты с концентраторами внутри вызова метода [усесигналр](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) в `Startup.Configure` методе.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-143">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a><span data-ttu-id="fd5f6-144">Закрепленные сеансы</span><span class="sxs-lookup"><span data-stu-id="fd5f6-144">Sticky sessions</span></span>

<span data-ttu-id="fd5f6-145">Модель масштабирования для ASP.NET SignalR позволяет клиентам повторно подключаться и передавать сообщения на любой сервер в ферме.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-145">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="fd5f6-146">В ASP.NET Core SignalR клиент должен взаимодействовать с тем же сервером на время соединения.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-146">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="fd5f6-147">Для масштабирования с помощью Redis, это означает, что требуются прикрепленные сеансы.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-147">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="fd5f6-148">Для масштабирования с помощью [службы Azure SignalR](/azure/azure-signalr/)не требуется использовать закрепленные сеансы, так как служба обрабатывает соединения с клиентами.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-148">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="fd5f6-149">Один концентратор на подключение</span><span class="sxs-lookup"><span data-stu-id="fd5f6-149">Single hub per connection</span></span>

<span data-ttu-id="fd5f6-150">В ASP.NET Core SignalR модель подключения была упрощена.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-150">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="fd5f6-151">Подключения осуществляются непосредственно в одном концентраторе, а не в одном соединении, используемом для совместного доступа к нескольким концентраторам.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-151">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="fd5f6-152">Потоковые операторы</span><span class="sxs-lookup"><span data-stu-id="fd5f6-152">Streaming</span></span>

<span data-ttu-id="fd5f6-153">ASP.NET Core SignalR теперь поддерживает [потоковую передачу данных](xref:signalr/streaming) от концентратора клиенту.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-153">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="fd5f6-154">Область</span><span class="sxs-lookup"><span data-stu-id="fd5f6-154">State</span></span>

<span data-ttu-id="fd5f6-155">Возможность передачи произвольного состояния между клиентами и концентратором (часто называется Хубстате) была удалена, а также поддержкой сообщений о ходе выполнения.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-155">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="fd5f6-156">В данный момент не существует аналога прокси-серверов концентратора.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-156">There is no counterpart of hub proxies at the moment.</span></span>

### <a name="persistentconnection-removal"></a><span data-ttu-id="fd5f6-157">Удаление Персистентконнектион</span><span class="sxs-lookup"><span data-stu-id="fd5f6-157">PersistentConnection removal</span></span>

<span data-ttu-id="fd5f6-158">В ASP.NET Core SignalR был удален класс [персистентконнектион](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) .</span><span class="sxs-lookup"><span data-stu-id="fd5f6-158">In ASP.NET Core SignalR, the [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) class has been removed.</span></span>

### <a name="globalhost"></a><span data-ttu-id="fd5f6-159">GlobalHost</span><span class="sxs-lookup"><span data-stu-id="fd5f6-159">GlobalHost</span></span>

<span data-ttu-id="fd5f6-160">ASP.NET Core в платформе встроена встраивание зависимостей (DI).</span><span class="sxs-lookup"><span data-stu-id="fd5f6-160">ASP.NET Core has dependency injection (DI) built into the framework.</span></span> <span data-ttu-id="fd5f6-161">Службы могут использовать DI для доступа к [хубконтекст](xref:signalr/hubcontext).</span><span class="sxs-lookup"><span data-stu-id="fd5f6-161">Services can use DI to access the [HubContext](xref:signalr/hubcontext).</span></span> <span data-ttu-id="fd5f6-162">Объект, используемый в ASP.NET SignalR для получения, `HubContext` не существует в ASP.NET Core SignalR. `GlobalHost`</span><span class="sxs-lookup"><span data-stu-id="fd5f6-162">The `GlobalHost` object that is used in ASP.NET SignalR to get a `HubContext` doesn't exist in ASP.NET Core SignalR.</span></span>

### <a name="hubpipeline"></a><span data-ttu-id="fd5f6-163">хубпипелине</span><span class="sxs-lookup"><span data-stu-id="fd5f6-163">HubPipeline</span></span>

<span data-ttu-id="fd5f6-164">ASP.NET Core SignalR не поддерживает `HubPipeline` модули.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-164">ASP.NET Core SignalR doesn't have support for `HubPipeline` modules.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="fd5f6-165">Различия в клиенте</span><span class="sxs-lookup"><span data-stu-id="fd5f6-165">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="fd5f6-166">TypeScript</span><span class="sxs-lookup"><span data-stu-id="fd5f6-166">TypeScript</span></span>

<span data-ttu-id="fd5f6-167">Клиент SignalR ASP.NET Core записывается в [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="fd5f6-167">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="fd5f6-168">При использовании [клиента JavaScript](xref:signalr/javascript-client)можно писать на JavaScript или TypeScript.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-168">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="fd5f6-169">Клиент JavaScript размещается по адресу [NPM](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="fd5f6-169">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="fd5f6-170">В предыдущих версиях клиент JavaScript был получен с помощью пакета NuGet в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-170">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="fd5f6-171">Для основных версий [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) пакет NPM содержит библиотеки JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-171">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="fd5f6-172">Этот пакет не входит в шаблон **веб-приложения ASP.NET Core** .</span><span class="sxs-lookup"><span data-stu-id="fd5f6-172">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="fd5f6-173">Используйте NPM для получения и установки `@aspnet/signalr` пакета NPM.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-173">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="fd5f6-174">jQuery</span><span class="sxs-lookup"><span data-stu-id="fd5f6-174">jQuery</span></span>

<span data-ttu-id="fd5f6-175">Зависимость от jQuery удалена, однако проекты по-прежнему могут использовать jQuery.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-175">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="internet-explorer-support"></a><span data-ttu-id="fd5f6-176">Поддержка Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="fd5f6-176">Internet Explorer support</span></span>

<span data-ttu-id="fd5f6-177">Для ASP.NET Core SignalR требуется Microsoft Internet Explorer 11 или более поздняя версия (ASP.NET SignalR поддерживал Microsoft Internet Explorer 8 и более поздние версии).</span><span class="sxs-lookup"><span data-stu-id="fd5f6-177">ASP.NET Core SignalR requires Microsoft Internet Explorer 11 or later (ASP.NET SignalR supported Microsoft Internet Explorer 8 and later).</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="fd5f6-178">Синтаксис клиентского метода JavaScript</span><span class="sxs-lookup"><span data-stu-id="fd5f6-178">JavaScript client method syntax</span></span>

<span data-ttu-id="fd5f6-179">Синтаксис JavaScript был изменен с предыдущей версии SignalR.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-179">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="fd5f6-180">Вместо использования `$connection` объекта создайте подключение с помощью API [хубконнектионбуилдер](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) .</span><span class="sxs-lookup"><span data-stu-id="fd5f6-180">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="fd5f6-181">Используйте метод [On](/javascript/api/@aspnet/signalr/HubConnection#on) , чтобы указать клиентские методы, которые может вызывать концентратор.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-181">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="fd5f6-182">После создания клиентского метода запустите подключение концентратора.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-182">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="fd5f6-183">Привязать метод [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) для ведения журнала или обработке ошибок.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-183">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="fd5f6-184">Прокси-серверы концентратора</span><span class="sxs-lookup"><span data-stu-id="fd5f6-184">Hub proxies</span></span>

<span data-ttu-id="fd5f6-185">Прокси-серверы концентратора больше не создаются автоматически.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-185">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="fd5f6-186">Вместо этого имя метода передается в API [вызова](/javascript/api/%40aspnet/signalr/hubconnection#invoke) в виде строки.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-186">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="fd5f6-187">.NET и другие клиенты</span><span class="sxs-lookup"><span data-stu-id="fd5f6-187">.NET and other clients</span></span>

<span data-ttu-id="fd5f6-188">Пакет `Microsoft.AspNetCore.SignalR.Client` NuGet содержит клиентские библиотеки .NET для ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-188">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="fd5f6-189">Используйте [хубконнектионбуилдер](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) , чтобы создать и построить экземпляр соединения с концентратором.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-189">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="fd5f6-190">Различия масштабирования</span><span class="sxs-lookup"><span data-stu-id="fd5f6-190">Scaleout differences</span></span>

<span data-ttu-id="fd5f6-191">ASP.NET SignalR поддерживает SQL Server и Redis.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-191">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="fd5f6-192">ASP.NET Core SignalR поддерживает службу Azure SignalR и Redis.</span><span class="sxs-lookup"><span data-stu-id="fd5f6-192">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="fd5f6-193">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="fd5f6-193">ASP.NET</span></span>

* [<span data-ttu-id="fd5f6-194">Масштабирование SignalR с помощью служебной шины Azure</span><span class="sxs-lookup"><span data-stu-id="fd5f6-194">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="fd5f6-195">Масштабирование SignalR с помощью Redis</span><span class="sxs-lookup"><span data-stu-id="fd5f6-195">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="fd5f6-196">Масштабирование SignalR с SQL Server</span><span class="sxs-lookup"><span data-stu-id="fd5f6-196">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="fd5f6-197">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fd5f6-197">ASP.NET Core</span></span>

* [<span data-ttu-id="fd5f6-198">Служба Azure SignalR</span><span class="sxs-lookup"><span data-stu-id="fd5f6-198">Azure SignalR Service</span></span>](/azure/azure-signalr/)
* [<span data-ttu-id="fd5f6-199">Объединительная плата Redis</span><span class="sxs-lookup"><span data-stu-id="fd5f6-199">Redis Backplane</span></span>](xref:signalr/redis-backplane)

## <a name="additional-resources"></a><span data-ttu-id="fd5f6-200">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="fd5f6-200">Additional resources</span></span>

* [<span data-ttu-id="fd5f6-201">Центры</span><span class="sxs-lookup"><span data-stu-id="fd5f6-201">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="fd5f6-202">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="fd5f6-202">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="fd5f6-203">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="fd5f6-203">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="fd5f6-204">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="fd5f6-204">Supported platforms</span></span>](xref:signalr/supported-platforms)
