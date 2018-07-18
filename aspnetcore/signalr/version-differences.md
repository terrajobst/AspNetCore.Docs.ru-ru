---
title: Различия между SignalR и ASP.NET Core SignalR
author: tdykstra
description: Различия между SignalR и ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: 6ed7e2e1ecadef08d71c4d7a7c3469738d07bcda
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095012"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="ab93a-103">Различия между SignalR и ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="ab93a-103">Differences between SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="ab93a-104">ASP.NET Core SignalR не совместим с клиентами или серверами для ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="ab93a-104">ASP.NET Core SignalR is not compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="ab93a-105">В этой статье описаны возможности, которые были удалены или изменены в ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="ab93a-105">This article details the features which have been removed or changed in the ASP.NET Core SignalR.</span></span>

## <a name="feature-differences"></a><span data-ttu-id="ab93a-106">Различия в функциях</span><span class="sxs-lookup"><span data-stu-id="ab93a-106">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="ab93a-107">Автоматическое повторных подключений</span><span class="sxs-lookup"><span data-stu-id="ab93a-107">Automatic reconnects</span></span>

<span data-ttu-id="ab93a-108">Автоматическое повторных подключений больше не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="ab93a-108">Automatic reconnects are no longer supported.</span></span> <span data-ttu-id="ab93a-109">Ранее SignalR попытался подключиться к серверу, если соединение было разорвано.</span><span class="sxs-lookup"><span data-stu-id="ab93a-109">Previously, SignalR tried to reconnect to the server if the connection was dropped.</span></span> <span data-ttu-id="ab93a-110">Теперь, если клиент отключен, необходимо явным образом запустить новое соединение для повторного подключения.</span><span class="sxs-lookup"><span data-stu-id="ab93a-110">Now, if the client is disconnected the user must explicitly start a new connection if they want to reconnect.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="ab93a-111">Поддержка протоколов</span><span class="sxs-lookup"><span data-stu-id="ab93a-111">Protocol support</span></span>

<span data-ttu-id="ab93a-112">ASP.NET Core SignalR поддерживает JSON, а также новый двоичный протокол, основанный на [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="ab93a-112">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="ab93a-113">Кроме того можно создать пользовательские протоколы.</span><span class="sxs-lookup"><span data-stu-id="ab93a-113">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="ab93a-114">Различия на сервере</span><span class="sxs-lookup"><span data-stu-id="ab93a-114">Differences on the server</span></span>

<span data-ttu-id="ab93a-115">Серверные библиотеки SignalR, включаются в `Microsoft.AspNetCore.App` пакет, который является частью **веб-приложение ASP.NET Core** шаблон для проектов MVC и Razor.</span><span class="sxs-lookup"><span data-stu-id="ab93a-115">The SignalR server-side libraries are included in the `Microsoft.AspNetCore.App` package that is part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="ab93a-116">SignalR представляет по промежуточного слоя ASP.NET Core, поэтому он должен быть настроен путем вызова `AddSignalR` в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ab93a-116">SignalR is an ASP.NET Core middleware, so it must be configured by calling `AddSignalR` in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR();
```

<span data-ttu-id="ab93a-117">Чтобы настроить маршрутизацию, сопоставляются с маршруты концентраторов внутри `UseSignalR` вызов метода `Startup.Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="ab93a-117">To configure routing, map routes to hubs inside the `UseSignalR` method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="ab93a-118">Теперь требуется прикрепленные сеансы</span><span class="sxs-lookup"><span data-stu-id="ab93a-118">Sticky sessions now required</span></span>

<span data-ttu-id="ab93a-119">Из-за как горизонтальное масштабирование работали в предыдущих версиях SignalR клиенты сможет повторно подключиться и отправлять сообщения на любой сервер в ферме.</span><span class="sxs-lookup"><span data-stu-id="ab93a-119">Because of how scale-out worked in the previous versions of SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="ab93a-120">Из-за изменения модели горизонтального масштабирования, а также не поддерживает повторных подключений это больше не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="ab93a-120">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="ab93a-121">Теперь когда клиент подключается к серверу, ей нужно взаимодействовать с тем же сервером в течение всего соединения.</span><span class="sxs-lookup"><span data-stu-id="ab93a-121">Now, once the client connects to the server it needs to interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="ab93a-122">Единый центр на соединение</span><span class="sxs-lookup"><span data-stu-id="ab93a-122">Single hub per connection</span></span>

<span data-ttu-id="ab93a-123">В ASP.NET Core SignalR упрощена модель соединения.</span><span class="sxs-lookup"><span data-stu-id="ab93a-123">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="ab93a-124">Соединения устанавливаются непосредственно в одном центре, а не одного соединения, используемого для общий доступ для нескольких центров.</span><span class="sxs-lookup"><span data-stu-id="ab93a-124">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="ab93a-125">Потоковые операторы</span><span class="sxs-lookup"><span data-stu-id="ab93a-125">Streaming</span></span>

<span data-ttu-id="ab93a-126">Теперь поддерживает SignalR [потоковой передачи данных](xref:signalr/streaming) от концентратора клиенту.</span><span class="sxs-lookup"><span data-stu-id="ab93a-126">SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="ab93a-127">Регион</span><span class="sxs-lookup"><span data-stu-id="ab93a-127">State</span></span>

<span data-ttu-id="ab93a-128">Возможность передавать произвольное состояние между клиентами и концентратор (часто называемые HubState) был удален, а также поддержка сообщения о ходе выполнения.</span><span class="sxs-lookup"><span data-stu-id="ab93a-128">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="ab93a-129">На данный момент нет не существовал центра учетных записей-посредников.</span><span class="sxs-lookup"><span data-stu-id="ab93a-129">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="ab93a-130">Различия на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="ab93a-130">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="ab93a-131">TypeScript</span><span class="sxs-lookup"><span data-stu-id="ab93a-131">TypeScript</span></span>

<span data-ttu-id="ab93a-132">Создается на языке версии ASP.NET Core SignalR [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="ab93a-132">The ASP.NET Core version of SignalR is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="ab93a-133">Можно написать на языке JavaScript или TypeScript при использовании [клиента JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="ab93a-133">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="ab93a-134">Клиент JavaScript размещается в [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="ab93a-134">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="ab93a-135">В предыдущих версиях клиента JavaScript был получен через пакет NuGet в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ab93a-135">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="ab93a-136">Для версий ядра [ @aspnet/signalr пакета npm](https://www.npmjs.com/package/@aspnet/signalr) содержит библиотеки JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ab93a-136">For the Core versions, the [@aspnet/signalr npm package](https://www.npmjs.com/package/@aspnet/signalr) contains the JavaScript libraries.</span></span> <span data-ttu-id="ab93a-137">Этот пакет не входит в **веб-приложение ASP.NET Core** шаблона.</span><span class="sxs-lookup"><span data-stu-id="ab93a-137">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="ab93a-138">Получить и установить с помощью npm `@aspnet/signalr` пакета npm.</span><span class="sxs-lookup"><span data-stu-id="ab93a-138">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="ab93a-139">jQuery</span><span class="sxs-lookup"><span data-stu-id="ab93a-139">jQuery</span></span>

<span data-ttu-id="ab93a-140">Зависимость от jQuery был удален, однако проекты по-прежнему можете использовать jQuery.</span><span class="sxs-lookup"><span data-stu-id="ab93a-140">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="ab93a-141">Синтаксис метода клиента JavaScript</span><span class="sxs-lookup"><span data-stu-id="ab93a-141">JavaScript client method syntax</span></span>

<span data-ttu-id="ab93a-142">Синтаксис JavaScript был изменен с предыдущей версии SignalR.</span><span class="sxs-lookup"><span data-stu-id="ab93a-142">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="ab93a-143">Вместо использования `$connection` следует создать соединения с помощью `HubConnectionBuilder` API.</span><span class="sxs-lookup"><span data-stu-id="ab93a-143">Rather than using the `$connection` object, create a connection using the `HubConnectionBuilder` API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="ab93a-144">Используйте `connection.on` для указания клиентских методов, которые могут вызывать концентратора.</span><span class="sxs-lookup"><span data-stu-id="ab93a-144">Use `connection.on` to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="ab93a-145">После создания метода клиента, запустите подключение концентратора.</span><span class="sxs-lookup"><span data-stu-id="ab93a-145">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="ab93a-146">Цепочка `catch` метод входа или обработки ошибок.</span><span class="sxs-lookup"><span data-stu-id="ab93a-146">Chain a `catch` method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="ab93a-147">Центр учетных записей-посредников</span><span class="sxs-lookup"><span data-stu-id="ab93a-147">Hub proxies</span></span>

<span data-ttu-id="ab93a-148">Больше не будет автоматически создаются прокси концентратора.</span><span class="sxs-lookup"><span data-stu-id="ab93a-148">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="ab93a-149">Вместо этого передается имя метода `invoke` API как строка.</span><span class="sxs-lookup"><span data-stu-id="ab93a-149">Instead, the method name is passed into the `invoke` API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="ab93a-150">.NET и других клиентов</span><span class="sxs-lookup"><span data-stu-id="ab93a-150">.NET and other clients</span></span>

<span data-ttu-id="ab93a-151">`Microsoft.AspNetCore.SignalR.Client` Пакет NuGet содержит клиентские библиотеки .NET для ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="ab93a-151">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="ab93a-152">Используйте `HubConnectionBuilder` Создание и построение экземпляр подключения к концентратору.</span><span class="sxs-lookup"><span data-stu-id="ab93a-152">Use the `HubConnectionBuilder` to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="ab93a-153">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ab93a-153">Additional resources</span></span>

* [<span data-ttu-id="ab93a-154">Центры</span><span class="sxs-lookup"><span data-stu-id="ab93a-154">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="ab93a-155">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="ab93a-155">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="ab93a-156">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="ab93a-156">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="ab93a-157">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="ab93a-157">Supported platforms</span></span>](xref:signalr/supported-platforms)
