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
ms.sourcegitcommit: 6ddd8a7675c1c1d997c8ab2d4498538e44954cac
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2019
ms.locfileid: "57400675"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a>Используют протокол центра MessagePack в SignalR для ASP.NET Core

По [Бреннан Конрой](https://github.com/BrennanConroy)

В этой статье предполагается, читатель знаком с подразделах, рассматриваемых в [приступить к работе](xref:tutorials/signalr).

## <a name="what-is-messagepack"></a>Что такое MessagePack?

[MessagePack](https://msgpack.org/index.html) — это быстрый и компактный формат двоичной сериализации. Это удобно при производительности и пропускной способности являются проблемой, так как он создает относительно мелкие сообщения, по сравнению с [JSON](https://www.json.org/). Поскольку это двоичный формат, сообщения являются нечитаемыми, когда просмотрев журналы и трассировки сети, если только байты передаются через MessagePack средство синтаксического анализа. SignalR имеет встроенную поддержку формата MessagePack, а также предоставляет API-интерфейсы для клиента и сервера для использования.

## <a name="configure-messagepack-on-the-server"></a>Настройка MessagePack на сервере

Чтобы включить протокол концентратора MessagePack на сервере, установите `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` в приложении пакет. В файле Startup.cs добавьте `AddMessagePackProtocol` для `AddSignalR` вызов, чтобы включить поддержку MessagePack на сервере.

> [!NOTE]
> JSON включена по умолчанию. Добавление MessagePack включает поддержку JSON и MessagePack клиентов.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

Для настройки, как MessagePack будет форматирования данных, `AddMessagePackProtocol` принимает делегат для настройки параметров. В этот делегат `FormatterResolvers` свойство может использоваться для настройки параметров MessagePack сериализации. Дополнительные сведения о работе сопоставители посетить MessagePack библиотеки [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp). Атрибуты можно использовать на объекты, которые необходимо сериализовать, чтобы определить, как они должны обрабатываться.

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

## <a name="configure-messagepack-on-the-client"></a>Настройка MessagePack на стороне клиента

> [!NOTE]
> JSON включена по умолчанию для поддерживаемых клиентов. Клиенты, поддерживают только один протокол. Добавление поддержки MessagePack заменяет любой ранее настроены протоколы.

### <a name="net-client"></a>Клиент .NET

Чтобы включить MessagePack в клиенте .NET, установите `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` пакета и вызов `AddMessagePackProtocol` на `HubConnectionBuilder`.

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> Это `AddMessagePackProtocol` вызов принимает делегат для настройки параметров так же, как сервер.

### <a name="javascript-client"></a>Клиент JavaScript

Обеспечивается поддержка MessagePack для клиента JavaScript `@aspnet/signalr-protocol-msgpack` пакета npm.

```console
npm install @aspnet/signalr-protocol-msgpack
```

После установки пакета npm, модуль можно использовать непосредственно с помощью загрузчика модулей JavaScript или импортированы в браузере, ссылаясь на *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* файла. В обозревателе `msgpack5` также должна быть указана библиотека. Используйте `<script>` тег, чтобы создать ссылку. Посетите библиотеку *node_modules\msgpack5\dist\msgpack5.js*.

> [!NOTE]
> При использовании `<script>` элемент, порядок важен. Если *signalr-protocol-msgpack.js* указывается перед *msgpack5.js*, произошла ошибка при попытке подключения с MessagePack. *SignalR.js* также необходима перед *signalr-protocol-msgpack.js*.

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

Добавление `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` для `HubConnectionBuilder` будет настроить клиент для использования протокола MessagePack, при подключении к серверу.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> В настоящее время существуют не для протокола MessagePack параметры конфигурации на стороне клиента JavaScript.

## <a name="messagepack-quirks"></a>MessagePack совместимости

Существует несколько проблем, которые следует учитывать при использовании протокола MessagePack концентратора.

### <a name="messagepack-is-case-sensitive"></a>MessagePack с учетом регистра

Протокол MessagePack — с учетом регистра. Например, рассмотрим следующие C# класса:

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

При отправке из клиентского кода JavaScript, необходимо использовать `PascalCased` имена свойств, так как регистр должен соответствовать C# точно класса. Пример:

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

С помощью `camelCased` имена не будут правильно привязки к C# класса. Это можно обойти с помощью `Key` атрибут, чтобы указать другое имя для свойства MessagePack. Дополнительные сведения см. в разделе [документации по c# MessagePack](https://github.com/neuecc/MessagePack-CSharp#object-serialization).

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a>При сериализации/десериализации DateTime.Kind не сохраняется

MessagePack протокол не предоставляет способ кодирования `Kind` значение `DateTime`. Таким образом при десериализации дату, протокол концентратора MessagePack предполагается, что входящие дата находится в формате UTC. Если вы работаете с `DateTime` значения в формате местного времени, рекомендуется преобразовать в формат UTC перед их отправкой. Преобразуйте их от времени UTC в местное время при их получении.

Дополнительные сведения об этих ограничениях см. в разделе GitHub проблема [aspnet/SignalR #2632](https://github.com/aspnet/SignalR/issues/2632).

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a>DateTime.MinValue MessagePack в JavaScript не поддерживается

[Msgpack5](https://github.com/mcollina/msgpack5) библиотека, используемая клиентом SignalR JavaScript не поддерживает `timestamp96` типа в MessagePack. Этот тип используется для кодирования значений очень больших даты (или очень рано в прошлом очень далеко в будущем). Значение `DateTime.MinValue` — `January 1, 0001` которого должны быть закодированы в `timestamp96` значение. Из-за этого, отправка `DateTime.MinValue` для JavaScript клиента не поддерживается. Когда `DateTime.MinValue` получении клиентом JavaScript, возникает следующая ошибка:

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

Как правило `DateTime.MinValue` , используемый для кодирования «отсутствует» или `null` значение. Если необходимо закодировать это значение в MessagePack, используйте значение необязательной определенности `DateTime` значение (`DateTime?`) или закодировать отдельного `bool` значение, указывающее, присутствует ли дата.

Дополнительные сведения об этих ограничениях см. в разделе GitHub проблема [aspnet/SignalR #2228](https://github.com/aspnet/SignalR/issues/2228).

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a>Поддержка MessagePack в среде компиляции «ahead-of-time»

[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) библиотеки, используемой клиентом .NET и сервером используется создание кода для оптимизации сериализации. Таким образом он не поддерживается в средах, использующих компиляции «ahead-of-time» (например, Xamarin iOS или Unity) по умолчанию. Это можно использовать MessagePack в этих средах, «предварительно создав» код сериализатора/десериализатора. Дополнительные сведения см. в разделе [документации по c# MessagePack](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports). После того, как предварительно сериализаторов, вы можете зарегистрировать их с помощью конфигурации делегат, передаваемый `AddMessagePackProtocol`:

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

### <a name="type-checks-are-more-strict-in-messagepack"></a>Тип проверки не действуют более строгие в MessagePack

Протокол JSON концентратора выполнит преобразования типов во время десериализации. Например, если входящий объект имеет значение свойства, является числом (`{ foo: 42 }`), но свойство класса .NET имеет тип `string`, значение будет преобразовано. Тем не менее MessagePack не выполняет это преобразование и вызовет исключение, которое можно увидеть в журналах на стороне сервера (и в консоли):

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

Дополнительные сведения об этих ограничениях см. в разделе GitHub проблема [aspnet/SignalR #2937](https://github.com/aspnet/SignalR/issues/2937).

## <a name="related-resources"></a>Связанные ресурсы

* [Начало работы](xref:tutorials/signalr)
* [Клиент .NET](xref:signalr/dotnet-client)
* [Клиент JavaScript](xref:signalr/javascript-client)
