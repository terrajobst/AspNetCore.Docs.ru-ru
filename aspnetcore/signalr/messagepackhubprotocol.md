---
title: Использовать протокол MessagePack концентратора SignalR для ASP.NET Core
author: rachelappel
description: Добавление протокола MessagePack концентратора SignalR ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 702c77502868d6666cb2634b6959f029e036d14e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274993"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="bd903-103">Использовать протокол MessagePack концентратора SignalR для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bd903-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="bd903-104">По [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="bd903-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="bd903-105">В этой статье предполагается, средство чтения знакомы с темы, рассматриваемые в [начать](xref:tutorials/signalr).</span><span class="sxs-lookup"><span data-stu-id="bd903-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="bd903-106">Что такое MessagePack?</span><span class="sxs-lookup"><span data-stu-id="bd903-106">What is MessagePack?</span></span>

<span data-ttu-id="bd903-107">[MessagePack](https://msgpack.org/index.html) -формат двоичной сериализации, быстрый и compact.</span><span class="sxs-lookup"><span data-stu-id="bd903-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="bd903-108">Это полезно, когда производительность и пропускную способность являются проблемой, так как он создает сообщения меньшего размера, по сравнению с [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="bd903-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="bd903-109">Поскольку это двоичный формат, сообщения не читаются, при просмотре трассировок сети и журналы, если байты передаются через средство синтаксического анализа MessagePack.</span><span class="sxs-lookup"><span data-stu-id="bd903-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="bd903-110">SignalR имеет встроенную поддержку формата MessagePack и предоставляет API для клиента и сервера для использования.</span><span class="sxs-lookup"><span data-stu-id="bd903-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="bd903-111">Настройка MessagePack на сервере</span><span class="sxs-lookup"><span data-stu-id="bd903-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="bd903-112">Чтобы включить протокол концентратора MessagePack на сервере, установите `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` пакета в вашем приложении.</span><span class="sxs-lookup"><span data-stu-id="bd903-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="bd903-113">Добавьте в файл файла Startup.cs `AddMessagePackProtocol` для `AddSignalR` вызов для включения поддержки MessagePack на сервере.</span><span class="sxs-lookup"><span data-stu-id="bd903-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="bd903-114">JSON включена по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="bd903-114">JSON is enabled by default.</span></span> <span data-ttu-id="bd903-115">Добавление MessagePack включает поддержку JSON и MessagePack клиентов.</span><span class="sxs-lookup"><span data-stu-id="bd903-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="bd903-116">Чтобы настроить как MessagePack будет форматирования данных, `AddMessagePackProtocol` принимает делегат для настройки параметров.</span><span class="sxs-lookup"><span data-stu-id="bd903-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="bd903-117">В этом делегате `FormatterResolvers` свойство можно использовать для настройки параметров MessagePack сериализации.</span><span class="sxs-lookup"><span data-stu-id="bd903-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="bd903-118">Дополнительные сведения о работе сопоставители посетите библиотеке MessagePack по [MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="bd903-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="bd903-119">Можно использовать атрибуты для объектов, которые необходимо сериализовать, чтобы определить, как они должны обрабатываться.</span><span class="sxs-lookup"><span data-stu-id="bd903-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="bd903-120">Настройка MessagePack на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="bd903-120">Configure MessagePack on the client</span></span>

### <a name="net-client"></a><span data-ttu-id="bd903-121">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="bd903-121">.NET client</span></span>

<span data-ttu-id="bd903-122">Чтобы включить MessagePack в клиент .NET, установить `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` пакета и вызова `AddMessagePackProtocol` на `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="bd903-122">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="bd903-123">Это `AddMessagePackProtocol` вызова принимает делегат для настройки параметров так же, как сервер.</span><span class="sxs-lookup"><span data-stu-id="bd903-123">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="bd903-124">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="bd903-124">JavaScript client</span></span>

<span data-ttu-id="bd903-125">Обеспечивается поддержка MessagePack клиента Javascript `@aspnet/signalr-protocol-msgpack` пакета NPM.</span><span class="sxs-lookup"><span data-stu-id="bd903-125">MessagePack support for the Javascript client is provided by the `@aspnet/signalr-protocol-msgpack` NPM package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="bd903-126">После установки пакета npm, можно использовать непосредственно через загрузчик модуля JavaScript или импортируются в браузере с помощью ссылки на модуль *node_modules\\ @aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*  файла.</span><span class="sxs-lookup"><span data-stu-id="bd903-126">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="bd903-127">В браузере `msgpack5` также должна быть указана библиотека.</span><span class="sxs-lookup"><span data-stu-id="bd903-127">In a browser the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="bd903-128">Используйте `<script>` тег, чтобы создать ссылку.</span><span class="sxs-lookup"><span data-stu-id="bd903-128">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="bd903-129">Можно найти в библиотеке *node_modules\msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="bd903-129">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="bd903-130">При использовании `<script>` элемент, порядок важен.</span><span class="sxs-lookup"><span data-stu-id="bd903-130">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="bd903-131">Если *signalr протокола msgpack.js* указывается перед *msgpack5.js*, произошла ошибка при попытке подключения с MessagePack.</span><span class="sxs-lookup"><span data-stu-id="bd903-131">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="bd903-132">*SignalR.js* перед также требуется *signalr протокола msgpack.js*.</span><span class="sxs-lookup"><span data-stu-id="bd903-132">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="bd903-133">Добавление `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` для `HubConnectionBuilder` будет настроить клиент для использования протокола MessagePack при соединении с сервером.</span><span class="sxs-lookup"><span data-stu-id="bd903-133">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="bd903-134">В настоящее время существует каких-либо параметров протокола MessagePack на стороне клиента JavaScript.</span><span class="sxs-lookup"><span data-stu-id="bd903-134">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="bd903-135">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="bd903-135">Related resources</span></span>

* [<span data-ttu-id="bd903-136">Начало работы</span><span class="sxs-lookup"><span data-stu-id="bd903-136">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="bd903-137">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="bd903-137">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="bd903-138">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="bd903-138">JavaScript client</span></span>](xref:signalr/javascript-client)
