---
title: Используют протокол центра MessagePack в SignalR для ASP.NET Core
author: tdykstra
description: Добавьте протокол MessagePack концентратора SignalR ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 0874afc5493eca5d43dfde30bb28aedc1f193744
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325585"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="4bb45-103">Используют протокол центра MessagePack в SignalR для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4bb45-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="4bb45-104">По [Бреннан Конрой](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="4bb45-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="4bb45-105">В этой статье предполагается, читатель знаком с подразделах, рассматриваемых в [приступить к работе](xref:tutorials/signalr).</span><span class="sxs-lookup"><span data-stu-id="4bb45-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="4bb45-106">Что такое MessagePack?</span><span class="sxs-lookup"><span data-stu-id="4bb45-106">What is MessagePack?</span></span>

<span data-ttu-id="4bb45-107">[MessagePack](https://msgpack.org/index.html) — это быстрый и компактный формат двоичной сериализации.</span><span class="sxs-lookup"><span data-stu-id="4bb45-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="4bb45-108">Это удобно при производительности и пропускной способности являются проблемой, так как он создает относительно мелкие сообщения, по сравнению с [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="4bb45-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="4bb45-109">Поскольку это двоичный формат, сообщения являются нечитаемыми, когда просмотрев журналы и трассировки сети, если только байты передаются через MessagePack средство синтаксического анализа.</span><span class="sxs-lookup"><span data-stu-id="4bb45-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="4bb45-110">SignalR имеет встроенную поддержку формата MessagePack, а также предоставляет API-интерфейсы для клиента и сервера для использования.</span><span class="sxs-lookup"><span data-stu-id="4bb45-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="4bb45-111">Настройка MessagePack на сервере</span><span class="sxs-lookup"><span data-stu-id="4bb45-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="4bb45-112">Чтобы включить протокол концентратора MessagePack на сервере, установите `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` в приложении пакет.</span><span class="sxs-lookup"><span data-stu-id="4bb45-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="4bb45-113">В файле Startup.cs добавьте `AddMessagePackProtocol` для `AddSignalR` вызов, чтобы включить поддержку MessagePack на сервере.</span><span class="sxs-lookup"><span data-stu-id="4bb45-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="4bb45-114">JSON включена по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="4bb45-114">JSON is enabled by default.</span></span> <span data-ttu-id="4bb45-115">Добавление MessagePack включает поддержку JSON и MessagePack клиентов.</span><span class="sxs-lookup"><span data-stu-id="4bb45-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="4bb45-116">Для настройки, как MessagePack будет форматирования данных, `AddMessagePackProtocol` принимает делегат для настройки параметров.</span><span class="sxs-lookup"><span data-stu-id="4bb45-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="4bb45-117">В этот делегат `FormatterResolvers` свойство может использоваться для настройки параметров MessagePack сериализации.</span><span class="sxs-lookup"><span data-stu-id="4bb45-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="4bb45-118">Дополнительные сведения о работе сопоставители посетить MessagePack библиотеки [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="4bb45-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="4bb45-119">Атрибуты можно использовать на объекты, которые необходимо сериализовать, чтобы определить, как они должны обрабатываться.</span><span class="sxs-lookup"><span data-stu-id="4bb45-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="4bb45-120">Настройка MessagePack на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="4bb45-120">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="4bb45-121">JSON включена по умолчанию для поддерживаемых клиентов.</span><span class="sxs-lookup"><span data-stu-id="4bb45-121">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="4bb45-122">Клиенты, поддерживают только один протокол.</span><span class="sxs-lookup"><span data-stu-id="4bb45-122">Clients can only support a single protocol.</span></span> <span data-ttu-id="4bb45-123">Добавление поддержки MessagePack заменяет любой ранее настроены протоколы.</span><span class="sxs-lookup"><span data-stu-id="4bb45-123">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="4bb45-124">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="4bb45-124">.NET client</span></span>

<span data-ttu-id="4bb45-125">Чтобы включить MessagePack в клиенте .NET, установите `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` пакета и вызов `AddMessagePackProtocol` на `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4bb45-125">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="4bb45-126">Это `AddMessagePackProtocol` вызов принимает делегат для настройки параметров так же, как сервер.</span><span class="sxs-lookup"><span data-stu-id="4bb45-126">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="4bb45-127">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="4bb45-127">JavaScript client</span></span>

<span data-ttu-id="4bb45-128">Обеспечивается поддержка MessagePack для клиента Javascript `@aspnet/signalr-protocol-msgpack` пакета NPM.</span><span class="sxs-lookup"><span data-stu-id="4bb45-128">MessagePack support for the Javascript client is provided by the `@aspnet/signalr-protocol-msgpack` NPM package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="4bb45-129">После установки пакета npm, модуль можно использовать непосредственно с помощью загрузчика модулей JavaScript или импортированы в браузере, ссылаясь на *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* файла.</span><span class="sxs-lookup"><span data-stu-id="4bb45-129">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="4bb45-130">В обозревателе `msgpack5` также должна быть указана библиотека.</span><span class="sxs-lookup"><span data-stu-id="4bb45-130">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="4bb45-131">Используйте `<script>` тег, чтобы создать ссылку.</span><span class="sxs-lookup"><span data-stu-id="4bb45-131">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="4bb45-132">Посетите библиотеку *node_modules\msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="4bb45-132">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="4bb45-133">При использовании `<script>` элемент, порядок важен.</span><span class="sxs-lookup"><span data-stu-id="4bb45-133">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="4bb45-134">Если *signalr-protocol-msgpack.js* указывается перед *msgpack5.js*, произошла ошибка при попытке подключения с MessagePack.</span><span class="sxs-lookup"><span data-stu-id="4bb45-134">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="4bb45-135">*SignalR.js* также необходима перед *signalr-protocol-msgpack.js*.</span><span class="sxs-lookup"><span data-stu-id="4bb45-135">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="4bb45-136">Добавление `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` для `HubConnectionBuilder` будет настроить клиент для использования протокола MessagePack, при подключении к серверу.</span><span class="sxs-lookup"><span data-stu-id="4bb45-136">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="4bb45-137">В настоящее время существуют не для протокола MessagePack параметры конфигурации на стороне клиента JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4bb45-137">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="4bb45-138">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="4bb45-138">Related resources</span></span>

* [<span data-ttu-id="4bb45-139">Начало работы</span><span class="sxs-lookup"><span data-stu-id="4bb45-139">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="4bb45-140">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="4bb45-140">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="4bb45-141">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="4bb45-141">JavaScript client</span></span>](xref:signalr/javascript-client)
