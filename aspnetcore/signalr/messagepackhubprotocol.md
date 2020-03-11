---
title: Использование протокола концентратора MessagePack в SignalR для ASP.NET Core
author: bradygaster
description: Добавьте протокол концентратора MessagePack в SignalRASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 3c2a4285945d3fdc6bba195e3160da8b9dcbba44
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78654568"
---
# <a name="use-messagepack-hub-protocol-in-opno-locsignalr-for-aspnet-core"></a><span data-ttu-id="efd82-103">Использование протокола концентратора MessagePack в SignalR для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="efd82-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="efd82-104">По [Бреннан Конрой](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="efd82-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="efd82-105">В этой статье предполагается, что читатель знаком с разделами, изложенными в разделе [Приступая к работе](xref:tutorials/signalr).</span><span class="sxs-lookup"><span data-stu-id="efd82-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="efd82-106">Что такое MessagePack?</span><span class="sxs-lookup"><span data-stu-id="efd82-106">What is MessagePack?</span></span>

<span data-ttu-id="efd82-107">[MessagePack](https://msgpack.org/index.html) — это формат двоичной сериализации, который является быстрым и компактным.</span><span class="sxs-lookup"><span data-stu-id="efd82-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="efd82-108">Это полезно, когда производительность и пропускная способность являются важным фактором, поскольку они создают меньшие сообщения по сравнению с [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="efd82-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="efd82-109">Так как это двоичный формат, сообщения не читаются при просмотре трассировки сети и журналов, если только эти байты не передаются через средство синтаксического анализа MessagePack.</span><span class="sxs-lookup"><span data-stu-id="efd82-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> SignalR<span data-ttu-id="efd82-110"> имеет встроенную поддержку формата MessagePack и предоставляет API-интерфейсы для использования клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="efd82-110"> has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="efd82-111">Настройка MessagePack на сервере</span><span class="sxs-lookup"><span data-stu-id="efd82-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="efd82-112">Чтобы включить протокол концентратора MessagePack на сервере, установите пакет `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` в приложении.</span><span class="sxs-lookup"><span data-stu-id="efd82-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="efd82-113">В файле Startup.cs добавьте `AddMessagePackProtocol` в вызов `AddSignalR`, чтобы включить поддержку MessagePack на сервере.</span><span class="sxs-lookup"><span data-stu-id="efd82-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="efd82-114">JSON включен по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="efd82-114">JSON is enabled by default.</span></span> <span data-ttu-id="efd82-115">Добавление MessagePack обеспечивает поддержку для клиентов JSON и MessagePack.</span><span class="sxs-lookup"><span data-stu-id="efd82-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="efd82-116">Чтобы настроить, как MessagePack будет форматировать данные, `AddMessagePackProtocol` принимает делегат для настройки параметров.</span><span class="sxs-lookup"><span data-stu-id="efd82-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="efd82-117">В этом делегате свойство `FormatterResolvers` можно использовать для настройки параметров сериализации MessagePack.</span><span class="sxs-lookup"><span data-stu-id="efd82-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="efd82-118">Дополнительные сведения о том, как работают арбитры конфликтов, см. в библиотеке MessagePack по адресу [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="efd82-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="efd82-119">Атрибуты можно использовать для объектов, которые необходимо сериализовать, чтобы определить, как они должны обрабатываться.</span><span class="sxs-lookup"><span data-stu-id="efd82-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

> [!WARNING]
> <span data-ttu-id="efd82-120">Настоятельно рекомендуем ознакомиться с [CVE-2020-5234](https://github.com/neuecc/MessagePack-CSharp/security/advisories/GHSA-7q36-4xx7-xcxf) и применить Рекомендуемые исправления.</span><span class="sxs-lookup"><span data-stu-id="efd82-120">We strongly recommend reviewing [CVE-2020-5234](https://github.com/neuecc/MessagePack-CSharp/security/advisories/GHSA-7q36-4xx7-xcxf) and applying the recommended patches.</span></span> <span data-ttu-id="efd82-121">Например, задание статического свойства `MessagePackSecurity.Active` `MessagePackSecurity.UntrustedData`.</span><span class="sxs-lookup"><span data-stu-id="efd82-121">For example, setting the `MessagePackSecurity.Active` static property to `MessagePackSecurity.UntrustedData`.</span></span> <span data-ttu-id="efd82-122">Для настройки `MessagePackSecurity.Active` требуется ручная установка [версии 1,9. x MessagePack](https://www.nuget.org/packages/MessagePack/1.9.3).</span><span class="sxs-lookup"><span data-stu-id="efd82-122">Setting the `MessagePackSecurity.Active` requires manually installing a [1.9.x version of MessagePack](https://www.nuget.org/packages/MessagePack/1.9.3).</span></span> <span data-ttu-id="efd82-123">При установке `MessagePack` 1,9. x обновляются версии, используемые SignalR.</span><span class="sxs-lookup"><span data-stu-id="efd82-123">Installing `MessagePack` 1.9.x upgrades the version SignalR uses.</span></span> <span data-ttu-id="efd82-124">Если для `MessagePackSecurity.Active` не задано значение `MessagePackSecurity.UntrustedData`, вредоносный клиент может вызвать отказ в обслуживании.</span><span class="sxs-lookup"><span data-stu-id="efd82-124">When `MessagePackSecurity.Active` is not set to `MessagePackSecurity.UntrustedData`, a malicious client could cause a denial of service.</span></span> <span data-ttu-id="efd82-125">Задайте `MessagePackSecurity.Active` в `Program.Main`, как показано в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="efd82-125">Set `MessagePackSecurity.Active` in `Program.Main`, as shown in the following code:</span></span>

```csharp
public static void Main(string[] args)
{
  MessagePackSecurity.Active = MessagePackSecurity.UntrustedData;

  CreateHostBuilder(args).Build().Run();
}
```

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="efd82-126">Настройка MessagePack на клиенте</span><span class="sxs-lookup"><span data-stu-id="efd82-126">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="efd82-127">JSON включен по умолчанию для поддерживаемых клиентов.</span><span class="sxs-lookup"><span data-stu-id="efd82-127">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="efd82-128">Клиенты могут поддерживать только один протокол.</span><span class="sxs-lookup"><span data-stu-id="efd82-128">Clients can only support a single protocol.</span></span> <span data-ttu-id="efd82-129">Добавление поддержки MessagePack заменит все ранее настроенные протоколы.</span><span class="sxs-lookup"><span data-stu-id="efd82-129">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="efd82-130">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="efd82-130">.NET client</span></span>

<span data-ttu-id="efd82-131">Чтобы включить MessagePack в клиенте .NET, установите пакет `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` и вызовите `AddMessagePackProtocol` на `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="efd82-131">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="efd82-132">Этот `AddMessagePackProtocol` вызов принимает делегат для настройки параметров так же, как и сервер.</span><span class="sxs-lookup"><span data-stu-id="efd82-132">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="efd82-133">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="efd82-133">JavaScript client</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="efd82-134">Поддержка MessagePack для клиента JavaScript предоставляется пакетом `@microsoft/signalr-protocol-msgpack` NPM.</span><span class="sxs-lookup"><span data-stu-id="efd82-134">MessagePack support for the JavaScript client is provided by the `@microsoft/signalr-protocol-msgpack` npm package.</span></span> <span data-ttu-id="efd82-135">Установите пакет, выполнив следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="efd82-135">Install the package by executing the following command in a command shell:</span></span>

```console
npm install @microsoft/signalr-protocol-msgpack
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="efd82-136">Поддержка MessagePack для клиента JavaScript предоставляется пакетом `@aspnet/signalr-protocol-msgpack` NPM.</span><span class="sxs-lookup"><span data-stu-id="efd82-136">MessagePack support for the JavaScript client is provided by the `@aspnet/signalr-protocol-msgpack` npm package.</span></span> <span data-ttu-id="efd82-137">Установите пакет, выполнив следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="efd82-137">Install the package by executing the following command in a command shell:</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

::: moniker-end

<span data-ttu-id="efd82-138">После установки пакета NPM модуль можно использовать непосредственно через загрузчик модуля JavaScript или импортировать в браузер, обратившись к следующему файлу:</span><span class="sxs-lookup"><span data-stu-id="efd82-138">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the following file:</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="efd82-139">*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span><span class="sxs-lookup"><span data-stu-id="efd82-139">*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span></span> 

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="efd82-140">*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span><span class="sxs-lookup"><span data-stu-id="efd82-140">*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span></span> 

::: moniker-end

<span data-ttu-id="efd82-141">В браузере также необходимо ссылаться на библиотеку `msgpack5`.</span><span class="sxs-lookup"><span data-stu-id="efd82-141">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="efd82-142">Чтобы создать ссылку, используйте тег `<script>`.</span><span class="sxs-lookup"><span data-stu-id="efd82-142">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="efd82-143">Библиотеку можно найти по адресу *node_modules \msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="efd82-143">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="efd82-144">При использовании элемента `<script>` важен порядок.</span><span class="sxs-lookup"><span data-stu-id="efd82-144">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="efd82-145">Если перед *msgpack5. js*указана ссылка на *сигналр-протокол-мсгпакк. js* , возникает ошибка при попытке подключения с помощью MessagePack.</span><span class="sxs-lookup"><span data-stu-id="efd82-145">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="efd82-146">*SignalR. js* также требуется перед *сигналр-протокол-мсгпакк. js*.</span><span class="sxs-lookup"><span data-stu-id="efd82-146">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="efd82-147">Добавление `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` в `HubConnectionBuilder` настраивает клиент для использования протокола MessagePack при соединении с сервером.</span><span class="sxs-lookup"><span data-stu-id="efd82-147">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="efd82-148">В настоящее время параметры конфигурации для протокола MessagePack на клиенте JavaScript отсутствуют.</span><span class="sxs-lookup"><span data-stu-id="efd82-148">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="messagepack-quirks"></a><span data-ttu-id="efd82-149">Особенности MessagePack</span><span class="sxs-lookup"><span data-stu-id="efd82-149">MessagePack quirks</span></span>

<span data-ttu-id="efd82-150">Существует несколько вопросов, которые следует учитывать при использовании протокола концентратора MessagePack.</span><span class="sxs-lookup"><span data-stu-id="efd82-150">There are a few issues to be aware of when using the MessagePack Hub Protocol.</span></span>

### <a name="messagepack-is-case-sensitive"></a><span data-ttu-id="efd82-151">MessagePack учитывает регистр</span><span class="sxs-lookup"><span data-stu-id="efd82-151">MessagePack is case-sensitive</span></span>

<span data-ttu-id="efd82-152">В протоколе MessagePack учитывается регистр.</span><span class="sxs-lookup"><span data-stu-id="efd82-152">The MessagePack protocol is case-sensitive.</span></span> <span data-ttu-id="efd82-153">Например, рассмотрим следующий C# класс:</span><span class="sxs-lookup"><span data-stu-id="efd82-153">For example, consider the following C# class:</span></span>

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

<span data-ttu-id="efd82-154">При отправке из клиента JavaScript необходимо использовать `PascalCased` имена свойств, поскольку регистр должен точно совпадать с C# классом.</span><span class="sxs-lookup"><span data-stu-id="efd82-154">When sending from the JavaScript client, you must use `PascalCased` property names, since the casing must match the C# class exactly.</span></span> <span data-ttu-id="efd82-155">Пример:</span><span class="sxs-lookup"><span data-stu-id="efd82-155">For example:</span></span>

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

<span data-ttu-id="efd82-156">Использование `camelCased` имен не будет правильно привязано C# к классу.</span><span class="sxs-lookup"><span data-stu-id="efd82-156">Using `camelCased` names won't properly bind to the C# class.</span></span> <span data-ttu-id="efd82-157">Это можно обойти, используя атрибут `Key`, чтобы указать другое имя для свойства MessagePack.</span><span class="sxs-lookup"><span data-stu-id="efd82-157">You can work around this by using the `Key` attribute to specify a different name for the MessagePack property.</span></span> <span data-ttu-id="efd82-158">Дополнительные сведения см. [в документации по MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span><span class="sxs-lookup"><span data-stu-id="efd82-158">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span></span>

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a><span data-ttu-id="efd82-159">DateTime. Kind не сохраняется при сериализации или десериализации</span><span class="sxs-lookup"><span data-stu-id="efd82-159">DateTime.Kind is not preserved when serializing/deserializing</span></span>

<span data-ttu-id="efd82-160">Протокол MessagePack не предоставляет способ кодирования `Kind` значения `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="efd82-160">The MessagePack protocol doesn't provide a way to encode the `Kind` value of a `DateTime`.</span></span> <span data-ttu-id="efd82-161">В результате при десериализации даты протокол концентратора MessagePack предполагает, что входящая Дата находится в формате UTC.</span><span class="sxs-lookup"><span data-stu-id="efd82-161">As a result, when deserializing a date, the MessagePack Hub Protocol assumes the incoming date is in UTC format.</span></span> <span data-ttu-id="efd82-162">Если вы работаете со `DateTime` значениями в местном времени, перед их отправкой рекомендуется преобразовать в формат UTC.</span><span class="sxs-lookup"><span data-stu-id="efd82-162">If you're working with `DateTime` values in local time, we recommend converting to UTC before sending them.</span></span> <span data-ttu-id="efd82-163">Преобразуйте их из времени в формате UTC в местное время при их получении.</span><span class="sxs-lookup"><span data-stu-id="efd82-163">Convert them from UTC to local time when you receive them.</span></span>

<span data-ttu-id="efd82-164">Дополнительные сведения об этом ограничении см. в разделе Проблема с GitHub [ASPNET/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).</span><span class="sxs-lookup"><span data-stu-id="efd82-164">For more information on this limitation, see GitHub issue [aspnet/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).</span></span>

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a><span data-ttu-id="efd82-165">DateTime. MinValue не поддерживается в MessagePack в JavaScript</span><span class="sxs-lookup"><span data-stu-id="efd82-165">DateTime.MinValue is not supported by MessagePack in JavaScript</span></span>

<span data-ttu-id="efd82-166">Библиотека [msgpack5](https://github.com/mcollina/msgpack5) , используемая клиентом SignalR JavaScript, не поддерживает тип `timestamp96` в MessagePack.</span><span class="sxs-lookup"><span data-stu-id="efd82-166">The [msgpack5](https://github.com/mcollina/msgpack5) library used by the SignalR JavaScript client doesn't support the `timestamp96` type in MessagePack.</span></span> <span data-ttu-id="efd82-167">Этот тип используется для кодирования очень больших значений даты (на ранних этапах прошлого или слишком далеко в будущем).</span><span class="sxs-lookup"><span data-stu-id="efd82-167">This type is used to encode very large date values (either very early in the past or very far in the future).</span></span> <span data-ttu-id="efd82-168">Значение `DateTime.MinValue` `January 1, 0001`, которое должно быть закодировано в `timestamp96` значение.</span><span class="sxs-lookup"><span data-stu-id="efd82-168">The value of `DateTime.MinValue` is `January 1, 0001` which must be encoded in a `timestamp96` value.</span></span> <span data-ttu-id="efd82-169">По этой причине отправка `DateTime.MinValue` клиенту JavaScript не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="efd82-169">Because of this, sending `DateTime.MinValue` to a JavaScript client isn't supported.</span></span> <span data-ttu-id="efd82-170">Когда клиент JavaScript получает `DateTime.MinValue`, выдается следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="efd82-170">When `DateTime.MinValue` is received by the JavaScript client, the following error is thrown:</span></span>

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

<span data-ttu-id="efd82-171">Обычно `DateTime.MinValue` используется для кодирования "отсутствующего" или `null` значения.</span><span class="sxs-lookup"><span data-stu-id="efd82-171">Usually, `DateTime.MinValue` is used to encode a "missing" or `null` value.</span></span> <span data-ttu-id="efd82-172">Если необходимо закодировать это значение в MessagePack, используйте значение NULL `DateTime` значения (`DateTime?`) или закодировать отдельное значение `bool`, указывающее, существует ли дата.</span><span class="sxs-lookup"><span data-stu-id="efd82-172">If you need to encode that value in MessagePack, use a nullable `DateTime` value (`DateTime?`) or encode a separate `bool` value indicating if the date is present.</span></span>

<span data-ttu-id="efd82-173">Дополнительные сведения об этом ограничении см. в разделе Проблема с GitHub [ASPNET/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span><span class="sxs-lookup"><span data-stu-id="efd82-173">For more information on this limitation, see GitHub issue [aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span></span>

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a><span data-ttu-id="efd82-174">Поддержка MessagePack в среде предварительной компиляции "до времени"</span><span class="sxs-lookup"><span data-stu-id="efd82-174">MessagePack support in "ahead-of-time" compilation environment</span></span>

<span data-ttu-id="efd82-175">Библиотека [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8) , используемая клиентом и сервером .NET, использует создание кода для оптимизации сериализации.</span><span class="sxs-lookup"><span data-stu-id="efd82-175">The [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8) library used by the .NET client and server uses code generation to optimize serialization.</span></span> <span data-ttu-id="efd82-176">Поэтому он не поддерживается по умолчанию в средах, использующих предварительную компиляцию (например, Xamarin iOS или Unity).</span><span class="sxs-lookup"><span data-stu-id="efd82-176">As a result, it isn't supported by default on environments that use "ahead-of-time" compilation (such as Xamarin iOS or Unity).</span></span> <span data-ttu-id="efd82-177">В этих средах можно использовать MessagePack, выполнив предварительное создание кода сериализатора или десериализации.</span><span class="sxs-lookup"><span data-stu-id="efd82-177">It's possible to use MessagePack in these environments by "pre-generating" the serializer/deserializer code.</span></span> <span data-ttu-id="efd82-178">Дополнительные сведения см. [в документации по MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8#pre-code-generationunityxamarin-supports).</span><span class="sxs-lookup"><span data-stu-id="efd82-178">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8#pre-code-generationunityxamarin-supports).</span></span> <span data-ttu-id="efd82-179">После предварительного создания сериализаторов их можно зарегистрировать с помощью делегата конфигурации, переданного в `AddMessagePackProtocol`:</span><span class="sxs-lookup"><span data-stu-id="efd82-179">Once you have pre-generated the serializers, you can register them using the configuration delegate passed to `AddMessagePackProtocol`:</span></span>

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

### <a name="type-checks-are-more-strict-in-messagepack"></a><span data-ttu-id="efd82-180">Проверки типов более строго в MessagePack</span><span class="sxs-lookup"><span data-stu-id="efd82-180">Type checks are more strict in MessagePack</span></span>

<span data-ttu-id="efd82-181">Протокол JSON-концентратора выполнит преобразования типов во время десериализации.</span><span class="sxs-lookup"><span data-stu-id="efd82-181">The JSON Hub Protocol will perform type conversions during deserialization.</span></span> <span data-ttu-id="efd82-182">Например, если входящий объект имеет значение свойства, которое является числом (`{ foo: 42 }`), но свойство класса .NET имеет тип `string`, значение будет преобразовано.</span><span class="sxs-lookup"><span data-stu-id="efd82-182">For example, if the incoming object has a property value that is a number (`{ foo: 42 }`) but the property on the .NET class is of type `string`, the value will be converted.</span></span> <span data-ttu-id="efd82-183">Однако MessagePack не выполняет это преобразование и вызовет исключение, которое можно увидеть в журналах на стороне сервера (и в консоли):</span><span class="sxs-lookup"><span data-stu-id="efd82-183">However, MessagePack doesn't perform this conversion and will throw an exception that can be seen in server-side logs (and in the console):</span></span>

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

<span data-ttu-id="efd82-184">Дополнительные сведения об этом ограничении см. в разделе Проблема с GitHub [ASPNET/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span><span class="sxs-lookup"><span data-stu-id="efd82-184">For more information on this limitation, see GitHub issue [aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span></span>

## <a name="related-resources"></a><span data-ttu-id="efd82-185">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="efd82-185">Related resources</span></span>

* [<span data-ttu-id="efd82-186">Начало работы</span><span class="sxs-lookup"><span data-stu-id="efd82-186">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="efd82-187">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="efd82-187">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="efd82-188">Клиент на JavaScript</span><span class="sxs-lookup"><span data-stu-id="efd82-188">JavaScript client</span></span>](xref:signalr/javascript-client)
