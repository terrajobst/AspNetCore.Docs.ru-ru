---
title: Используют протокол центра MessagePack в SignalR для ASP.NET Core
author: tdykstra
description: Добавьте протокол MessagePack концентратора SignalR ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 78b708c50ce7a8101c9eaa558171540e61c0d7f0
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2018
ms.locfileid: "39094999"
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

Обеспечивается поддержка MessagePack для клиента Javascript `@aspnet/signalr-protocol-msgpack` пакета NPM.

```console
npm install @aspnet/signalr-protocol-msgpack
```

После установки пакета npm, модуль можно использовать непосредственно с помощью загрузчика модулей JavaScript или импортированы в браузере, ссылаясь на *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* файла. В браузере `msgpack5` также должна быть указана библиотека. Используйте `<script>` тег, чтобы создать ссылку. Посетите библиотеку *node_modules\msgpack5\dist\msgpack5.js*.

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

## <a name="related-resources"></a>Связанные ресурсы

* [Начало работы](xref:tutorials/signalr)
* [Клиент .NET](xref:signalr/dotnet-client)
* [Клиент JavaScript](xref:signalr/javascript-client)
