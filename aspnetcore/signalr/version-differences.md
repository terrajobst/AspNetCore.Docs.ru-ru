---
title: Различия между SignalR и ASP.NET Core SignalR
author: bradygaster
description: Различия между SignalR и ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/version-differences
ms.openlocfilehash: 0f644c132b0fcf9a0ecf0ab181791a6477c97f76
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963725"
---
# <a name="differences-between-aspnet-opno-locsignalr-and-aspnet-core-opno-locsignalr"></a><span data-ttu-id="e37a3-103">Различия между ASP.NET SignalR и ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="e37a3-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="e37a3-104">ASP.NET Core SignalR несовместимы с клиентами или серверами для SignalRASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e37a3-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="e37a3-105">В этой статье описываются функции, которые были удалены или изменены в ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="e37a3-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-opno-locsignalr-version"></a><span data-ttu-id="e37a3-106">Определение версии SignalR</span><span class="sxs-lookup"><span data-stu-id="e37a3-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="e37a3-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="e37a3-107">ASP.NET SignalR</span></span> | <span data-ttu-id="e37a3-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="e37a3-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="e37a3-109">Пакет NuGet сервера</span><span class="sxs-lookup"><span data-stu-id="e37a3-109">Server NuGet Package</span></span> | <span data-ttu-id="e37a3-110">[Microsoft. AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/)</span><span class="sxs-lookup"><span data-stu-id="e37a3-110">[Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/)</span></span> | <span data-ttu-id="e37a3-111">[Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="e37a3-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="e37a3-112">[Microsoft. AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)</span><span class="sxs-lookup"><span data-stu-id="e37a3-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)</span></span> <span data-ttu-id="e37a3-113">(.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="e37a3-113">(.NET Framework)</span></span> |
| <span data-ttu-id="e37a3-114">Клиентские пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="e37a3-114">Client NuGet Packages</span></span> | <span data-ttu-id="e37a3-115">[Microsoft. AspNet.SignalR. Компьютера](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)</span><span class="sxs-lookup"><span data-stu-id="e37a3-115">[Microsoft.AspNet.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)</span></span><br><span data-ttu-id="e37a3-116">[Microsoft. AspNet.SignalR. JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/)</span><span class="sxs-lookup"><span data-stu-id="e37a3-116">[Microsoft.AspNet.SignalR.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/)</span></span> | <span data-ttu-id="e37a3-117">[Microsoft. AspNetCore.SignalR. Компьютера](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/)</span><span class="sxs-lookup"><span data-stu-id="e37a3-117">[Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/)</span></span> |
| <span data-ttu-id="e37a3-118">Клиентский пакет NPM</span><span class="sxs-lookup"><span data-stu-id="e37a3-118">Client npm Package</span></span> | [<span data-ttu-id="e37a3-119">SignalR</span><span class="sxs-lookup"><span data-stu-id="e37a3-119">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="e37a3-120">Клиент Java</span><span class="sxs-lookup"><span data-stu-id="e37a3-120">Java Client</span></span> | <span data-ttu-id="e37a3-121">[Репозиторий GitHub](https://github.com/SignalR/java-client) (не рекомендуется)</span><span class="sxs-lookup"><span data-stu-id="e37a3-121">[GitHub Repository](https://github.com/SignalR/java-client) (deprecated)</span></span>  | <span data-ttu-id="e37a3-122">Пакет Maven [com. Microsoft. SignalR](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span><span class="sxs-lookup"><span data-stu-id="e37a3-122">Maven package [com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span></span> |
| <span data-ttu-id="e37a3-123">Тип серверного приложения</span><span class="sxs-lookup"><span data-stu-id="e37a3-123">Server App Type</span></span> | <span data-ttu-id="e37a3-124">ASP.NET (System. Web) или OWIN Self-Host</span><span class="sxs-lookup"><span data-stu-id="e37a3-124">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="e37a3-125">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e37a3-125">ASP.NET Core</span></span> |
| <span data-ttu-id="e37a3-126">Поддерживаемые серверные платформы</span><span class="sxs-lookup"><span data-stu-id="e37a3-126">Supported Server Platforms</span></span> | <span data-ttu-id="e37a3-127">.NET Framework 4,5 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="e37a3-127">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="e37a3-128">.NET Framework 4.6.1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="e37a3-128">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="e37a3-129">.NET Core 2,1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="e37a3-129">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="e37a3-130">Различия в функциях</span><span class="sxs-lookup"><span data-stu-id="e37a3-130">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="e37a3-131">Автоматические повторное подключения</span><span class="sxs-lookup"><span data-stu-id="e37a3-131">Automatic reconnects</span></span>

<span data-ttu-id="e37a3-132">Автоматическое повторное подключение не поддерживается в ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="e37a3-132">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="e37a3-133">Если Клиент отключен, пользователь должен явно запустить новое подключение, если требуется повторное подключение.</span><span class="sxs-lookup"><span data-stu-id="e37a3-133">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="e37a3-134">В SignalRASP.NET SignalR пытается повторно подключиться к серверу при разрыве соединения.</span><span class="sxs-lookup"><span data-stu-id="e37a3-134">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="e37a3-135">Поддержка протокола</span><span class="sxs-lookup"><span data-stu-id="e37a3-135">Protocol support</span></span>

<span data-ttu-id="e37a3-136">ASP.NET Core SignalR поддерживает JSON, а также новый двоичный протокол на основе [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="e37a3-136">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="e37a3-137">Кроме того, можно создавать пользовательские протоколы.</span><span class="sxs-lookup"><span data-stu-id="e37a3-137">Additionally, custom protocols can be created.</span></span>

### <a name="transports"></a><span data-ttu-id="e37a3-138">Транспорты</span><span class="sxs-lookup"><span data-stu-id="e37a3-138">Transports</span></span>

<span data-ttu-id="e37a3-139">Непостоянное многокадровый транспорт не поддерживается в ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="e37a3-139">The Forever Frame transport is not supported in ASP.NET Core SignalR.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="e37a3-140">Различия на сервере</span><span class="sxs-lookup"><span data-stu-id="e37a3-140">Differences on the server</span></span>

<span data-ttu-id="e37a3-141">Библиотеки ASP.NET Core SignalR на стороне сервера включены в пакет [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app) , который входит в состав шаблона **ASP.NET Core веб-приложения** для проектов Razor и MVC.</span><span class="sxs-lookup"><span data-stu-id="e37a3-141">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="e37a3-142">ASP.NET Core SignalR является ASP.NET Core по промежуточного слоя, поэтому его необходимо настроить, вызвав [аддсигналр](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e37a3-142">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e37a3-143">Чтобы настроить маршрутизацию, сопоставьте маршруты с концентраторами внутри вызова метода [усиндпоинтс](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) в методе `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="e37a3-143">To configure routing, map routes to hubs inside the [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) method call in the `Startup.Configure` method.</span></span>


```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="e37a3-144">Чтобы настроить маршрутизацию, сопоставьте маршруты с концентраторами внутри вызова метода [усесигналр](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) в методе `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="e37a3-144">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a><span data-ttu-id="e37a3-145">Закрепленные сеансы</span><span class="sxs-lookup"><span data-stu-id="e37a3-145">Sticky sessions</span></span>

<span data-ttu-id="e37a3-146">Модель масштабирования для ASP.NET SignalR позволяет клиентам повторно подключаться и передавать сообщения на любой сервер в ферме.</span><span class="sxs-lookup"><span data-stu-id="e37a3-146">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="e37a3-147">В ASP.NET Core SignalRклиент должен взаимодействовать с тем же сервером в течение всего соединения.</span><span class="sxs-lookup"><span data-stu-id="e37a3-147">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="e37a3-148">Для масштабирования с помощью Redis, это означает, что требуются прикрепленные сеансы.</span><span class="sxs-lookup"><span data-stu-id="e37a3-148">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="e37a3-149">Для масштабирования с помощью [службы SignalR Azure](/azure/azure-signalr/)прикрепленные сеансы не требуются, так как служба обрабатывает соединения с клиентами.</span><span class="sxs-lookup"><span data-stu-id="e37a3-149">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="e37a3-150">Один концентратор на подключение</span><span class="sxs-lookup"><span data-stu-id="e37a3-150">Single hub per connection</span></span>

<span data-ttu-id="e37a3-151">В ASP.NET Core SignalRмодель подключения была упрощена.</span><span class="sxs-lookup"><span data-stu-id="e37a3-151">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="e37a3-152">Подключения осуществляются непосредственно в одном концентраторе, а не в одном соединении, используемом для совместного доступа к нескольким концентраторам.</span><span class="sxs-lookup"><span data-stu-id="e37a3-152">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="e37a3-153">Потоковые операторы</span><span class="sxs-lookup"><span data-stu-id="e37a3-153">Streaming</span></span>

<span data-ttu-id="e37a3-154">ASP.NET Core SignalR теперь поддерживает [потоковую передачу данных](xref:signalr/streaming) от концентратора клиенту.</span><span class="sxs-lookup"><span data-stu-id="e37a3-154">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="e37a3-155">Область</span><span class="sxs-lookup"><span data-stu-id="e37a3-155">State</span></span>

<span data-ttu-id="e37a3-156">Возможность передачи произвольного состояния между клиентами и концентратором (часто называется Хубстате) была удалена, а также поддержкой сообщений о ходе выполнения.</span><span class="sxs-lookup"><span data-stu-id="e37a3-156">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="e37a3-157">В данный момент не существует аналога прокси-серверов концентратора.</span><span class="sxs-lookup"><span data-stu-id="e37a3-157">There is no counterpart of hub proxies at the moment.</span></span>

### <a name="persistentconnection-removal"></a><span data-ttu-id="e37a3-158">Удаление Персистентконнектион</span><span class="sxs-lookup"><span data-stu-id="e37a3-158">PersistentConnection removal</span></span>

<span data-ttu-id="e37a3-159">В ASP.NET Core SignalRкласс [персистентконнектион](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) был удален.</span><span class="sxs-lookup"><span data-stu-id="e37a3-159">In ASP.NET Core SignalR, the [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) class has been removed.</span></span>

### <a name="globalhost"></a><span data-ttu-id="e37a3-160">GlobalHost</span><span class="sxs-lookup"><span data-stu-id="e37a3-160">GlobalHost</span></span>

<span data-ttu-id="e37a3-161">ASP.NET Core в платформе встроена встраивание зависимостей (DI).</span><span class="sxs-lookup"><span data-stu-id="e37a3-161">ASP.NET Core has dependency injection (DI) built into the framework.</span></span> <span data-ttu-id="e37a3-162">Службы могут использовать DI для доступа к [хубконтекст](xref:signalr/hubcontext).</span><span class="sxs-lookup"><span data-stu-id="e37a3-162">Services can use DI to access the [HubContext](xref:signalr/hubcontext).</span></span> <span data-ttu-id="e37a3-163">Объект `GlobalHost`, используемый в ASP.NET SignalR для получения `HubContext`, не существует в ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="e37a3-163">The `GlobalHost` object that is used in ASP.NET SignalR to get a `HubContext` doesn't exist in ASP.NET Core SignalR.</span></span>

### <a name="hubpipeline"></a><span data-ttu-id="e37a3-164">хубпипелине</span><span class="sxs-lookup"><span data-stu-id="e37a3-164">HubPipeline</span></span>

<span data-ttu-id="e37a3-165">ASP.NET Core SignalR не поддерживает модули `HubPipeline`.</span><span class="sxs-lookup"><span data-stu-id="e37a3-165">ASP.NET Core SignalR doesn't have support for `HubPipeline` modules.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="e37a3-166">Различия в клиенте</span><span class="sxs-lookup"><span data-stu-id="e37a3-166">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="e37a3-167">TypeScript</span><span class="sxs-lookup"><span data-stu-id="e37a3-167">TypeScript</span></span>

<span data-ttu-id="e37a3-168">Клиент ASP.NET Core SignalR записывается в [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="e37a3-168">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="e37a3-169">При использовании [клиента JavaScript](xref:signalr/javascript-client)можно писать на JavaScript или TypeScript.</span><span class="sxs-lookup"><span data-stu-id="e37a3-169">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="e37a3-170">Клиент JavaScript размещается по адресу [NPM](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="e37a3-170">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="e37a3-171">В предыдущих версиях клиент JavaScript был получен с помощью пакета NuGet в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e37a3-171">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="e37a3-172">Для основных версий пакет [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) NPM содержит библиотеки JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e37a3-172">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="e37a3-173">Этот пакет не входит в шаблон **веб-приложения ASP.NET Core** .</span><span class="sxs-lookup"><span data-stu-id="e37a3-173">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="e37a3-174">Используйте NPM для получения и установки пакета NPM `@aspnet/signalr`.</span><span class="sxs-lookup"><span data-stu-id="e37a3-174">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="e37a3-175">jQuery</span><span class="sxs-lookup"><span data-stu-id="e37a3-175">jQuery</span></span>

<span data-ttu-id="e37a3-176">Зависимость от jQuery удалена, однако проекты по-прежнему могут использовать jQuery.</span><span class="sxs-lookup"><span data-stu-id="e37a3-176">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="internet-explorer-support"></a><span data-ttu-id="e37a3-177">Поддержка Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="e37a3-177">Internet Explorer support</span></span>

<span data-ttu-id="e37a3-178">Для ASP.NET Core SignalR требуется Microsoft Internet Explorer 11 или более поздняя версия (ASP.NET SignalR поддерживал Microsoft Internet Explorer 8 и более поздние версии).</span><span class="sxs-lookup"><span data-stu-id="e37a3-178">ASP.NET Core SignalR requires Microsoft Internet Explorer 11 or later (ASP.NET SignalR supported Microsoft Internet Explorer 8 and later).</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="e37a3-179">Синтаксис клиентского метода JavaScript</span><span class="sxs-lookup"><span data-stu-id="e37a3-179">JavaScript client method syntax</span></span>

<span data-ttu-id="e37a3-180">Синтаксис JavaScript был изменен с предыдущей версии SignalR.</span><span class="sxs-lookup"><span data-stu-id="e37a3-180">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="e37a3-181">Вместо использования объекта `$connection` создайте подключение с помощью API [хубконнектионбуилдер](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) .</span><span class="sxs-lookup"><span data-stu-id="e37a3-181">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="e37a3-182">Используйте метод [On](/javascript/api/@aspnet/signalr/HubConnection#on) , чтобы указать клиентские методы, которые может вызывать концентратор.</span><span class="sxs-lookup"><span data-stu-id="e37a3-182">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="e37a3-183">После создания клиентского метода запустите подключение концентратора.</span><span class="sxs-lookup"><span data-stu-id="e37a3-183">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="e37a3-184">Привязать метод [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) для ведения журнала или обработке ошибок.</span><span class="sxs-lookup"><span data-stu-id="e37a3-184">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="e37a3-185">Прокси-серверы концентратора</span><span class="sxs-lookup"><span data-stu-id="e37a3-185">Hub proxies</span></span>

<span data-ttu-id="e37a3-186">Прокси-серверы концентратора больше не создаются автоматически.</span><span class="sxs-lookup"><span data-stu-id="e37a3-186">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="e37a3-187">Вместо этого имя метода передается в API [вызова](/javascript/api/%40aspnet/signalr/hubconnection#invoke) в виде строки.</span><span class="sxs-lookup"><span data-stu-id="e37a3-187">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="e37a3-188">.NET и другие клиенты</span><span class="sxs-lookup"><span data-stu-id="e37a3-188">.NET and other clients</span></span>

<span data-ttu-id="e37a3-189">`Microsoft.AspNetCore.SignalR.Client` пакет NuGet содержит клиентские библиотеки .NET для ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="e37a3-189">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="e37a3-190">Используйте [хубконнектионбуилдер](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) , чтобы создать и построить экземпляр соединения с концентратором.</span><span class="sxs-lookup"><span data-stu-id="e37a3-190">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="e37a3-191">Различия масштабирования</span><span class="sxs-lookup"><span data-stu-id="e37a3-191">Scaleout differences</span></span>

<span data-ttu-id="e37a3-192">ASP.NET SignalR поддерживает SQL Server и Redis.</span><span class="sxs-lookup"><span data-stu-id="e37a3-192">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="e37a3-193">ASP.NET Core SignalR поддерживает службу SignalR Azure и Redis.</span><span class="sxs-lookup"><span data-stu-id="e37a3-193">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="e37a3-194">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e37a3-194">ASP.NET</span></span>

* <span data-ttu-id="e37a3-195">[SignalR масштабирование с помощью служебной шины Azure](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)</span><span class="sxs-lookup"><span data-stu-id="e37a3-195">[SignalR scaleout with Azure Service Bus](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)</span></span>
* <span data-ttu-id="e37a3-196">[SignalR масштабирование с помощью Redis](/aspnet/signalr/overview/performance/scaleout-with-redis)</span><span class="sxs-lookup"><span data-stu-id="e37a3-196">[SignalR scaleout with Redis](/aspnet/signalr/overview/performance/scaleout-with-redis)</span></span>
* <span data-ttu-id="e37a3-197">[SignalR масштабирование с помощью SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)</span><span class="sxs-lookup"><span data-stu-id="e37a3-197">[SignalR scaleout with SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)</span></span>

### <a name="aspnet-core"></a><span data-ttu-id="e37a3-198">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e37a3-198">ASP.NET Core</span></span>

* <span data-ttu-id="e37a3-199">[Служба SignalR Azure](/azure/azure-signalr/)</span><span class="sxs-lookup"><span data-stu-id="e37a3-199">[Azure SignalR Service](/azure/azure-signalr/)</span></span>
* [<span data-ttu-id="e37a3-200">Объединительная плата Redis</span><span class="sxs-lookup"><span data-stu-id="e37a3-200">Redis Backplane</span></span>](xref:signalr/redis-backplane)

## <a name="additional-resources"></a><span data-ttu-id="e37a3-201">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e37a3-201">Additional resources</span></span>

* [<span data-ttu-id="e37a3-202">Центры</span><span class="sxs-lookup"><span data-stu-id="e37a3-202">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="e37a3-203">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="e37a3-203">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="e37a3-204">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="e37a3-204">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="e37a3-205">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="e37a3-205">Supported platforms</span></span>](xref:signalr/supported-platforms)
