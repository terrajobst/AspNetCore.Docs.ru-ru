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
ms.sourcegitcommit: 0b0e485a8a6dfcc65a7a58b365622b3839f4d624
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2020
ms.locfileid: "76928179"
---
# <a name="use-messagepack-hub-protocol-in-opno-locsignalr-for-aspnet-core"></a>Использование протокола концентратора MessagePack в SignalR для ASP.NET Core

По [Бреннан Конрой](https://github.com/BrennanConroy)

В этой статье предполагается, что читатель знаком с разделами, изложенными в разделе [Приступая к работе](xref:tutorials/signalr).

## <a name="what-is-messagepack"></a>Что такое MessagePack?

[MessagePack](https://msgpack.org/index.html) — это формат двоичной сериализации, который является быстрым и компактным. Это полезно, когда производительность и пропускная способность являются важным фактором, поскольку они создают меньшие сообщения по сравнению с [JSON](https://www.json.org/). Так как это двоичный формат, сообщения не читаются при просмотре трассировки сети и журналов, если только эти байты не передаются через средство синтаксического анализа MessagePack. SignalR имеет встроенную поддержку формата MessagePack и предоставляет API-интерфейсы для использования клиентом и сервером.

## <a name="configure-messagepack-on-the-server"></a>Настройка MessagePack на сервере

Чтобы включить протокол концентратора MessagePack на сервере, установите пакет `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` в приложении. В файле Startup.cs добавьте `AddMessagePackProtocol` в вызов `AddSignalR`, чтобы включить поддержку MessagePack на сервере.

> [!NOTE]
> JSON включен по умолчанию. Добавление MessagePack обеспечивает поддержку для клиентов JSON и MessagePack.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

Чтобы настроить, как MessagePack будет форматировать данные, `AddMessagePackProtocol` принимает делегат для настройки параметров. В этом делегате свойство `FormatterResolvers` можно использовать для настройки параметров сериализации MessagePack. Дополнительные сведения о том, как работают арбитры конфликтов, см. в библиотеке MessagePack по адресу [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp). Атрибуты можно использовать для объектов, которые необходимо сериализовать, чтобы определить, как они должны обрабатываться.

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
> Настоятельно рекомендуем ознакомиться с [CVE-2020-5234](https://github.com/neuecc/MessagePack-CSharp/security/advisories/GHSA-7q36-4xx7-xcxf) и применить Рекомендуемые исправления. Например, задание статического свойства `MessagePackSecurity.Active` `MessagePackSecurity.UntrustedData`. Для настройки `MessagePackSecurity.Active` требуется ручная установка [версии 1,9. x MessagePack](https://www.nuget.org/packages/MessagePack/1.9.3). При установке `MessagePack` 1,9. x обновляются версии, используемые SignalR. Если для `MessagePackSecurity.Active` не задано значение `MessagePackSecurity.UntrustedData`, вредоносный клиент может вызвать отказ в обслуживании. Задайте `MessagePackSecurity.Active` в `Program.Main`, как показано в следующем коде:

```csharp
public static void Main(string[] args)
{
  MessagePackSecurity.Active = MessagePackSecurity.UntrustedData;

  CreateHostBuilder(args).Build().Run();
}
```

## <a name="configure-messagepack-on-the-client"></a>Настройка MessagePack на клиенте

> [!NOTE]
> JSON включен по умолчанию для поддерживаемых клиентов. Клиенты могут поддерживать только один протокол. Добавление поддержки MessagePack заменит все ранее настроенные протоколы.

### <a name="net-client"></a>Клиент .NET

Чтобы включить MessagePack в клиенте .NET, установите пакет `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` и вызовите `AddMessagePackProtocol` на `HubConnectionBuilder`.

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> Этот `AddMessagePackProtocol` вызов принимает делегат для настройки параметров так же, как и сервер.

### <a name="javascript-client"></a>Клиент JavaScript

::: moniker range=">= aspnetcore-3.0"

Поддержка MessagePack для клиента JavaScript предоставляется пакетом `@microsoft/signalr-protocol-msgpack` NPM. Установите пакет, выполнив следующую команду в командной оболочке:

```console
npm install @microsoft/signalr-protocol-msgpack
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Поддержка MessagePack для клиента JavaScript предоставляется пакетом `@aspnet/signalr-protocol-msgpack` NPM. Установите пакет, выполнив следующую команду в командной оболочке:

```console
npm install @aspnet/signalr-protocol-msgpack
```

::: moniker-end

После установки пакета NPM модуль можно использовать непосредственно через загрузчик модуля JavaScript или импортировать в браузер, обратившись к следующему файлу:

::: moniker range=">= aspnetcore-3.0"

*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* 

::: moniker-end

::: moniker range="< aspnetcore-3.0"

*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* 

::: moniker-end

В браузере также необходимо ссылаться на библиотеку `msgpack5`. Чтобы создать ссылку, используйте тег `<script>`. Библиотеку можно найти по адресу *node_modules \msgpack5\dist\msgpack5.js*.

> [!NOTE]
> При использовании элемента `<script>` важен порядок. Если перед *msgpack5. js*указана ссылка на *сигналр-протокол-мсгпакк. js* , возникает ошибка при попытке подключения с помощью MessagePack. *SignalR. js* также требуется перед *сигналр-протокол-мсгпакк. js*.

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

Добавление `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` в `HubConnectionBuilder` настраивает клиент для использования протокола MessagePack при соединении с сервером.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> В настоящее время параметры конфигурации для протокола MessagePack на клиенте JavaScript отсутствуют.

## <a name="messagepack-quirks"></a>Особенности MessagePack

Существует несколько вопросов, которые следует учитывать при использовании протокола концентратора MessagePack.

### <a name="messagepack-is-case-sensitive"></a>MessagePack учитывает регистр

В протоколе MessagePack учитывается регистр. Например, рассмотрим следующий C# класс:

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

При отправке из клиента JavaScript необходимо использовать `PascalCased` имена свойств, поскольку регистр должен точно совпадать с C# классом. Например:

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

Использование `camelCased` имен не будет правильно привязано C# к классу. Это можно обойти, используя атрибут `Key`, чтобы указать другое имя для свойства MessagePack. Дополнительные сведения см. [в документации по MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#object-serialization).

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a>DateTime. Kind не сохраняется при сериализации или десериализации

Протокол MessagePack не предоставляет способ кодирования `Kind` значения `DateTime`. В результате при десериализации даты протокол концентратора MessagePack предполагает, что входящая Дата находится в формате UTC. Если вы работаете со `DateTime` значениями в местном времени, перед их отправкой рекомендуется преобразовать в формат UTC. Преобразуйте их из времени в формате UTC в местное время при их получении.

Дополнительные сведения об этом ограничении см. в разделе Проблема с GitHub [ASPNET/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a>DateTime. MinValue не поддерживается в MessagePack в JavaScript

Библиотека [msgpack5](https://github.com/mcollina/msgpack5) , используемая клиентом SignalR JavaScript, не поддерживает тип `timestamp96` в MessagePack. Этот тип используется для кодирования очень больших значений даты (на ранних этапах прошлого или слишком далеко в будущем). Значение `DateTime.MinValue` `January 1, 0001`, которое должно быть закодировано в `timestamp96` значение. По этой причине отправка `DateTime.MinValue` клиенту JavaScript не поддерживается. Когда клиент JavaScript получает `DateTime.MinValue`, выдается следующая ошибка:

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

Обычно `DateTime.MinValue` используется для кодирования "отсутствующего" или `null` значения. Если необходимо закодировать это значение в MessagePack, используйте значение NULL `DateTime` значения (`DateTime?`) или закодировать отдельное значение `bool`, указывающее, существует ли дата.

Дополнительные сведения об этом ограничении см. в разделе Проблема с GitHub [ASPNET/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a>Поддержка MessagePack в среде предварительной компиляции "до времени"

Библиотека [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8) , используемая клиентом и сервером .NET, использует создание кода для оптимизации сериализации. Поэтому он не поддерживается по умолчанию в средах, использующих предварительную компиляцию (например, Xamarin iOS или Unity). В этих средах можно использовать MessagePack, выполнив предварительное создание кода сериализатора или десериализации. Дополнительные сведения см. [в документации по MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8#pre-code-generationunityxamarin-supports). После предварительного создания сериализаторов их можно зарегистрировать с помощью делегата конфигурации, переданного в `AddMessagePackProtocol`:

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

### <a name="type-checks-are-more-strict-in-messagepack"></a>Проверки типов более строго в MessagePack

Протокол JSON-концентратора выполнит преобразования типов во время десериализации. Например, если входящий объект имеет значение свойства, которое является числом (`{ foo: 42 }`), но свойство класса .NET имеет тип `string`, значение будет преобразовано. Однако MessagePack не выполняет это преобразование и вызовет исключение, которое можно увидеть в журналах на стороне сервера (и в консоли):

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

Дополнительные сведения об этом ограничении см. в разделе Проблема с GitHub [ASPNET/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).

## <a name="related-resources"></a>Связанные ресурсы

* [Начало работы](xref:tutorials/signalr)
* [Клиент .NET](xref:signalr/dotnet-client)
* [Клиент JavaScript](xref:signalr/javascript-client)
