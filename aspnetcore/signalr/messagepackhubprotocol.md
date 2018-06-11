---
title: Использовать протокол MessagePack концентратора SignalR для ASP.NET Core
author: rachelappel
description: Добавление протокола MessagePack концентратора SignalR ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: b6c33c4da47a19d67bffbaf84f54d59013edadbe
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252486"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a>Использовать протокол MessagePack концентратора SignalR для ASP.NET Core

По [Brennan Conroy](https://github.com/BrennanConroy)

В этой статье предполагается, средство чтения знакомы с темы, рассматриваемые в [начать](xref:signalr/get-started).

## <a name="what-is-messagepack"></a>Что такое MessagePack?

[MessagePack](https://msgpack.org/index.html) -формат двоичной сериализации, быстрый и compact. Это полезно, когда производительность и пропускную способность являются проблемой, так как он создает сообщения меньшего размера, по сравнению с [JSON](https://www.json.org/). Поскольку это двоичный формат, сообщения не читаются, при просмотре трассировок сети и журналы, если байты передаются через средство синтаксического анализа MessagePack. SignalR имеет встроенную поддержку формата MessagePack и предоставляет API для клиента и сервера для использования.

## <a name="configure-messagepack-on-the-server"></a>Настройка MessagePack на сервере

Чтобы включить протокол концентратора MessagePack на сервере, установите `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` пакета в вашем приложении. Добавьте в файл файла Startup.cs `AddMessagePackProtocol` для `AddSignalR` вызов для включения поддержки MessagePack на сервере.

> [!NOTE]
> JSON включена по умолчанию. Добавление MessagePack включает поддержку JSON и MessagePack клиентов.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

Чтобы настроить как MessagePack будет форматирования данных, `AddMessagePackProtocol` принимает делегат для настройки параметров. В этом делегате `FormatterResolvers` свойство можно использовать для настройки параметров MessagePack сериализации. Дополнительные сведения о работе сопоставители посетите библиотеке MessagePack по [MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp). Можно использовать атрибуты для объектов, которые необходимо сериализовать, чтобы определить, как они должны обрабатываться.

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

Чтобы включить MessagePack в клиент .NET, установить `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` пакета и вызова `AddMessagePackProtocol` на `HubConnectionBuilder`.

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> Это `AddMessagePackProtocol` вызова принимает делегат для настройки параметров так же, как сервер.

### <a name="javascript-client"></a>Клиент JavaScript

Обеспечивается поддержка MessagePack клиента Javascript `@aspnet/signalr-protocol-msgpack` пакета NPM.

```console
npm install @aspnet/signalr-protocol-msgpack
```

После установки пакета npm, можно использовать непосредственно через загрузчик модуля JavaScript или импортируются в браузере с помощью ссылки на модуль *node_modules\\ @aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*  файла. В браузере `msgpack5` также должна быть указана библиотека. Используйте `<script>` тег, чтобы создать ссылку. Можно найти в библиотеке *node_modules\msgpack5\dist\msgpack5.js*.

> [!NOTE]
> При использовании `<script>` элемент, порядок важен. Если *signalr протокола msgpack.js* указывается перед *msgpack5.js*, произошла ошибка при попытке подключения с MessagePack. *SignalR.js* перед также требуется *signalr протокола msgpack.js*.

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

Добавление `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` для `HubConnectionBuilder` будет настроить клиент для использования протокола MessagePack при соединении с сервером.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> В настоящее время существует каких-либо параметров протокола MessagePack на стороне клиента JavaScript.

## <a name="related-resources"></a>Связанные ресурсы

* [Начало работы](xref:signalr/get-started)
* [Клиент .NET](xref:signalr/dotnet-client)
* [Клиент JavaScript](xref:signalr/javascript-client)
