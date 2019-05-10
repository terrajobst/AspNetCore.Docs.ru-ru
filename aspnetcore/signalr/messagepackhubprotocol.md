---
title: Используют протокол центра MessagePack в SignalR для ASP.NET Core
author: bradygaster
description: Добавьте протокол MessagePack концентратора SignalR ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/27/2019
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 7742f6f8bb53fb3c299ff98ae52a0da519ff396c
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896521"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="b9e10-103">Используют протокол центра MessagePack в SignalR для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b9e10-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="b9e10-104">По [Бреннан Конрой](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="b9e10-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="b9e10-105">В этой статье предполагается, читатель знаком с подразделах, рассматриваемых в [приступить к работе](xref:tutorials/signalr).</span><span class="sxs-lookup"><span data-stu-id="b9e10-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="b9e10-106">Что такое MessagePack?</span><span class="sxs-lookup"><span data-stu-id="b9e10-106">What is MessagePack?</span></span>

<span data-ttu-id="b9e10-107">[MessagePack](https://msgpack.org/index.html) — это быстрый и компактный формат двоичной сериализации.</span><span class="sxs-lookup"><span data-stu-id="b9e10-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="b9e10-108">Это удобно при производительности и пропускной способности являются проблемой, так как он создает относительно мелкие сообщения, по сравнению с [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="b9e10-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="b9e10-109">Поскольку это двоичный формат, сообщения являются нечитаемыми, когда просмотрев журналы и трассировки сети, если только байты передаются через MessagePack средство синтаксического анализа.</span><span class="sxs-lookup"><span data-stu-id="b9e10-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="b9e10-110">SignalR имеет встроенную поддержку формата MessagePack, а также предоставляет API-интерфейсы для клиента и сервера для использования.</span><span class="sxs-lookup"><span data-stu-id="b9e10-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="b9e10-111">Настройка MessagePack на сервере</span><span class="sxs-lookup"><span data-stu-id="b9e10-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="b9e10-112">Чтобы включить протокол концентратора MessagePack на сервере, установите `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` в приложении пакет.</span><span class="sxs-lookup"><span data-stu-id="b9e10-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="b9e10-113">В файле Startup.cs добавьте `AddMessagePackProtocol` для `AddSignalR` вызов, чтобы включить поддержку MessagePack на сервере.</span><span class="sxs-lookup"><span data-stu-id="b9e10-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="b9e10-114">JSON включена по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b9e10-114">JSON is enabled by default.</span></span> <span data-ttu-id="b9e10-115">Добавление MessagePack включает поддержку JSON и MessagePack клиентов.</span><span class="sxs-lookup"><span data-stu-id="b9e10-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="b9e10-116">Для настройки, как MessagePack будет форматирования данных, `AddMessagePackProtocol` принимает делегат для настройки параметров.</span><span class="sxs-lookup"><span data-stu-id="b9e10-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="b9e10-117">В этот делегат `FormatterResolvers` свойство может использоваться для настройки параметров MessagePack сериализации.</span><span class="sxs-lookup"><span data-stu-id="b9e10-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="b9e10-118">Дополнительные сведения о работе сопоставители посетить MessagePack библиотеки [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="b9e10-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="b9e10-119">Атрибуты можно использовать на объекты, которые необходимо сериализовать, чтобы определить, как они должны обрабатываться.</span><span class="sxs-lookup"><span data-stu-id="b9e10-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="b9e10-120">Настройка MessagePack на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="b9e10-120">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="b9e10-121">JSON включена по умолчанию для поддерживаемых клиентов.</span><span class="sxs-lookup"><span data-stu-id="b9e10-121">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="b9e10-122">Клиенты, поддерживают только один протокол.</span><span class="sxs-lookup"><span data-stu-id="b9e10-122">Clients can only support a single protocol.</span></span> <span data-ttu-id="b9e10-123">Добавление поддержки MessagePack заменяет любой ранее настроены протоколы.</span><span class="sxs-lookup"><span data-stu-id="b9e10-123">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="b9e10-124">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="b9e10-124">.NET client</span></span>

<span data-ttu-id="b9e10-125">Чтобы включить MessagePack в клиенте .NET, установите `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` пакета и вызов `AddMessagePackProtocol` на `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b9e10-125">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="b9e10-126">Это `AddMessagePackProtocol` вызов принимает делегат для настройки параметров так же, как сервер.</span><span class="sxs-lookup"><span data-stu-id="b9e10-126">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="b9e10-127">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="b9e10-127">JavaScript client</span></span>

<span data-ttu-id="b9e10-128">Обеспечивается поддержка MessagePack для клиента JavaScript `@aspnet/signalr-protocol-msgpack` пакета npm.</span><span class="sxs-lookup"><span data-stu-id="b9e10-128">MessagePack support for the JavaScript client is provided by the `@aspnet/signalr-protocol-msgpack` npm package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="b9e10-129">После установки пакета npm, модуль можно использовать непосредственно с помощью загрузчика модулей JavaScript или импортированы в браузере, ссылаясь на *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* файла.</span><span class="sxs-lookup"><span data-stu-id="b9e10-129">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="b9e10-130">В обозревателе `msgpack5` также должна быть указана библиотека.</span><span class="sxs-lookup"><span data-stu-id="b9e10-130">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="b9e10-131">Используйте `<script>` тег, чтобы создать ссылку.</span><span class="sxs-lookup"><span data-stu-id="b9e10-131">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="b9e10-132">Посетите библиотеку *node_modules\msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="b9e10-132">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="b9e10-133">При использовании `<script>` элемент, порядок важен.</span><span class="sxs-lookup"><span data-stu-id="b9e10-133">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="b9e10-134">Если *signalr-protocol-msgpack.js* указывается перед *msgpack5.js*, произошла ошибка при попытке подключения с MessagePack.</span><span class="sxs-lookup"><span data-stu-id="b9e10-134">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="b9e10-135">*SignalR.js* также необходима перед *signalr-protocol-msgpack.js*.</span><span class="sxs-lookup"><span data-stu-id="b9e10-135">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="b9e10-136">Добавление `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` для `HubConnectionBuilder` будет настроить клиент для использования протокола MessagePack, при подключении к серверу.</span><span class="sxs-lookup"><span data-stu-id="b9e10-136">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="b9e10-137">В настоящее время существуют не для протокола MessagePack параметры конфигурации на стороне клиента JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b9e10-137">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="messagepack-quirks"></a><span data-ttu-id="b9e10-138">MessagePack совместимости</span><span class="sxs-lookup"><span data-stu-id="b9e10-138">MessagePack quirks</span></span>

<span data-ttu-id="b9e10-139">Существует несколько проблем, которые следует учитывать при использовании протокола MessagePack концентратора.</span><span class="sxs-lookup"><span data-stu-id="b9e10-139">There are a few issues to be aware of when using the MessagePack Hub Protocol.</span></span>

### <a name="messagepack-is-case-sensitive"></a><span data-ttu-id="b9e10-140">MessagePack с учетом регистра</span><span class="sxs-lookup"><span data-stu-id="b9e10-140">MessagePack is case-sensitive</span></span>

<span data-ttu-id="b9e10-141">Протокол MessagePack — с учетом регистра.</span><span class="sxs-lookup"><span data-stu-id="b9e10-141">The MessagePack protocol is case-sensitive.</span></span> <span data-ttu-id="b9e10-142">Например, рассмотрим следующие C# класса:</span><span class="sxs-lookup"><span data-stu-id="b9e10-142">For example, consider the following C# class:</span></span>

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

<span data-ttu-id="b9e10-143">При отправке из клиентского кода JavaScript, необходимо использовать `PascalCased` имена свойств, так как регистр должен соответствовать C# точно класса.</span><span class="sxs-lookup"><span data-stu-id="b9e10-143">When sending from the JavaScript client, you must use `PascalCased` property names, since the casing must match the C# class exactly.</span></span> <span data-ttu-id="b9e10-144">Пример:</span><span class="sxs-lookup"><span data-stu-id="b9e10-144">For example:</span></span>

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

<span data-ttu-id="b9e10-145">С помощью `camelCased` имена не будут правильно привязки к C# класса.</span><span class="sxs-lookup"><span data-stu-id="b9e10-145">Using `camelCased` names won't properly bind to the C# class.</span></span> <span data-ttu-id="b9e10-146">Это можно обойти с помощью `Key` атрибут, чтобы указать другое имя для свойства MessagePack.</span><span class="sxs-lookup"><span data-stu-id="b9e10-146">You can work around this by using the `Key` attribute to specify a different name for the MessagePack property.</span></span> <span data-ttu-id="b9e10-147">Дополнительные сведения см. в разделе [документации по c# MessagePack](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span><span class="sxs-lookup"><span data-stu-id="b9e10-147">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span></span>

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a><span data-ttu-id="b9e10-148">При сериализации/десериализации DateTime.Kind не сохраняется</span><span class="sxs-lookup"><span data-stu-id="b9e10-148">DateTime.Kind is not preserved when serializing/deserializing</span></span>

<span data-ttu-id="b9e10-149">MessagePack протокол не предоставляет способ кодирования `Kind` значение `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="b9e10-149">The MessagePack protocol doesn't provide a way to encode the `Kind` value of a `DateTime`.</span></span> <span data-ttu-id="b9e10-150">Таким образом при десериализации дату, протокол концентратора MessagePack предполагается, что входящие дата находится в формате UTC.</span><span class="sxs-lookup"><span data-stu-id="b9e10-150">As a result, when deserializing a date, the MessagePack Hub Protocol assumes the incoming date is in UTC format.</span></span> <span data-ttu-id="b9e10-151">Если вы работаете с `DateTime` значения в формате местного времени, рекомендуется преобразовать в формат UTC перед их отправкой.</span><span class="sxs-lookup"><span data-stu-id="b9e10-151">If you're working with `DateTime` values in local time, we recommend converting to UTC before sending them.</span></span> <span data-ttu-id="b9e10-152">Преобразуйте их от времени UTC в местное время при их получении.</span><span class="sxs-lookup"><span data-stu-id="b9e10-152">Convert them from UTC to local time when you receive them.</span></span>

<span data-ttu-id="b9e10-153">Дополнительные сведения об этих ограничениях см. в разделе GitHub проблема [aspnet/SignalR #2632](https://github.com/aspnet/SignalR/issues/2632).</span><span class="sxs-lookup"><span data-stu-id="b9e10-153">For more information on this limitation, see GitHub issue [aspnet/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).</span></span>

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a><span data-ttu-id="b9e10-154">DateTime.MinValue MessagePack в JavaScript не поддерживается</span><span class="sxs-lookup"><span data-stu-id="b9e10-154">DateTime.MinValue is not supported by MessagePack in JavaScript</span></span>

<span data-ttu-id="b9e10-155">[Msgpack5](https://github.com/mcollina/msgpack5) библиотека, используемая клиентом SignalR JavaScript не поддерживает `timestamp96` типа в MessagePack.</span><span class="sxs-lookup"><span data-stu-id="b9e10-155">The [msgpack5](https://github.com/mcollina/msgpack5) library used by the SignalR JavaScript client doesn't support the `timestamp96` type in MessagePack.</span></span> <span data-ttu-id="b9e10-156">Этот тип используется для кодирования значений очень больших даты (или очень рано в прошлом очень далеко в будущем).</span><span class="sxs-lookup"><span data-stu-id="b9e10-156">This type is used to encode very large date values (either very early in the past or very far in the future).</span></span> <span data-ttu-id="b9e10-157">Значение `DateTime.MinValue` — `January 1, 0001` которого должны быть закодированы в `timestamp96` значение.</span><span class="sxs-lookup"><span data-stu-id="b9e10-157">The value of `DateTime.MinValue` is `January 1, 0001` which must be encoded in a `timestamp96` value.</span></span> <span data-ttu-id="b9e10-158">Из-за этого, отправка `DateTime.MinValue` для JavaScript клиента не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="b9e10-158">Because of this, sending `DateTime.MinValue` to a JavaScript client isn't supported.</span></span> <span data-ttu-id="b9e10-159">Когда `DateTime.MinValue` получении клиентом JavaScript, возникает следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="b9e10-159">When `DateTime.MinValue` is received by the JavaScript client, the following error is thrown:</span></span>

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

<span data-ttu-id="b9e10-160">Как правило `DateTime.MinValue` , используемый для кодирования «отсутствует» или `null` значение.</span><span class="sxs-lookup"><span data-stu-id="b9e10-160">Usually, `DateTime.MinValue` is used to encode a "missing" or `null` value.</span></span> <span data-ttu-id="b9e10-161">Если необходимо закодировать это значение в MessagePack, используйте значение необязательной определенности `DateTime` значение (`DateTime?`) или закодировать отдельного `bool` значение, указывающее, присутствует ли дата.</span><span class="sxs-lookup"><span data-stu-id="b9e10-161">If you need to encode that value in MessagePack, use a nullable `DateTime` value (`DateTime?`) or encode a separate `bool` value indicating if the date is present.</span></span>

<span data-ttu-id="b9e10-162">Дополнительные сведения об этих ограничениях см. в разделе GitHub проблема [aspnet/SignalR #2228](https://github.com/aspnet/SignalR/issues/2228).</span><span class="sxs-lookup"><span data-stu-id="b9e10-162">For more information on this limitation, see GitHub issue [aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span></span>

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a><span data-ttu-id="b9e10-163">Поддержка MessagePack в среде компиляции «ahead-of-time»</span><span class="sxs-lookup"><span data-stu-id="b9e10-163">MessagePack support in "ahead-of-time" compilation environment</span></span>

<span data-ttu-id="b9e10-164">[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) библиотеки, используемой клиентом .NET и сервером используется создание кода для оптимизации сериализации.</span><span class="sxs-lookup"><span data-stu-id="b9e10-164">The [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) library used by the .NET client and server uses code generation to optimize serialization.</span></span> <span data-ttu-id="b9e10-165">Таким образом он не поддерживается в средах, использующих компиляции «ahead-of-time» (например, Xamarin iOS или Unity) по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b9e10-165">As a result, it isn't supported by default on environments that use "ahead-of-time" compilation (such as Xamarin iOS or Unity).</span></span> <span data-ttu-id="b9e10-166">Это можно использовать MessagePack в этих средах, «предварительно создав» код сериализатора/десериализатора.</span><span class="sxs-lookup"><span data-stu-id="b9e10-166">It's possible to use MessagePack in these environments by "pre-generating" the serializer/deserializer code.</span></span> <span data-ttu-id="b9e10-167">Дополнительные сведения см. в разделе [документации по c# MessagePack](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports).</span><span class="sxs-lookup"><span data-stu-id="b9e10-167">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports).</span></span> <span data-ttu-id="b9e10-168">После того, как предварительно сериализаторов, вы можете зарегистрировать их с помощью конфигурации делегат, передаваемый `AddMessagePackProtocol`:</span><span class="sxs-lookup"><span data-stu-id="b9e10-168">Once you have pre-generated the serializers, you can register them using the configuration delegate passed to `AddMessagePackProtocol`:</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.GeneratedResolver.Instance,
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

### <a name="type-checks-are-more-strict-in-messagepack"></a><span data-ttu-id="b9e10-169">Тип проверки не действуют более строгие в MessagePack</span><span class="sxs-lookup"><span data-stu-id="b9e10-169">Type checks are more strict in MessagePack</span></span>

<span data-ttu-id="b9e10-170">Протокол JSON концентратора выполнит преобразования типов во время десериализации.</span><span class="sxs-lookup"><span data-stu-id="b9e10-170">The JSON Hub Protocol will perform type conversions during deserialization.</span></span> <span data-ttu-id="b9e10-171">Например, если входящий объект имеет значение свойства, является числом (`{ foo: 42 }`), но свойство класса .NET имеет тип `string`, значение будет преобразовано.</span><span class="sxs-lookup"><span data-stu-id="b9e10-171">For example, if the incoming object has a property value that is a number (`{ foo: 42 }`) but the property on the .NET class is of type `string`, the value will be converted.</span></span> <span data-ttu-id="b9e10-172">Тем не менее MessagePack не выполняет это преобразование и вызовет исключение, которое можно увидеть в журналах на стороне сервера (и в консоли):</span><span class="sxs-lookup"><span data-stu-id="b9e10-172">However, MessagePack doesn't perform this conversion and will throw an exception that can be seen in server-side logs (and in the console):</span></span>

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

<span data-ttu-id="b9e10-173">Дополнительные сведения об этих ограничениях см. в разделе GitHub проблема [aspnet/SignalR #2937](https://github.com/aspnet/SignalR/issues/2937).</span><span class="sxs-lookup"><span data-stu-id="b9e10-173">For more information on this limitation, see GitHub issue [aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span></span>

## <a name="related-resources"></a><span data-ttu-id="b9e10-174">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="b9e10-174">Related resources</span></span>

* [<span data-ttu-id="b9e10-175">Начало работы</span><span class="sxs-lookup"><span data-stu-id="b9e10-175">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="b9e10-176">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="b9e10-176">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="b9e10-177">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="b9e10-177">JavaScript client</span></span>](xref:signalr/javascript-client)
