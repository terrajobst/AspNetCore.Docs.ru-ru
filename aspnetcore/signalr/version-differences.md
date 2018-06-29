---
title: Различия между SignalR и ASP.NET Core SignalR
author: rachelappel
description: Различия между SignalR и ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: fd314d93333bd159aef4bb4863be50c646809cf0
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090071"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a>Различия между SignalR и ASP.NET Core SignalR

ASP.NET Core SignalR не совместим с клиентами или серверами для ASP.NET SignalR. В этой статье описаны функции, которые были удалены или изменены в ASP.NET Core SignalR.

## <a name="feature-differences"></a>Различия в функциях

### <a name="automatic-reconnects"></a>Автоматическое повторных подключений к

Автоматическое повторных подключений к больше не поддерживаются. Ранее SignalR попытался подключиться к серверу, если соединение было разорвано. Теперь, если клиент отключен, необходимо явным образом запустить новое соединение для повторного подключения.

### <a name="protocol-support"></a>Поддержка протоколов

ASP.NET Core SignalR поддерживает JSON, а также новый двоичный протокол на основе [MessagePack](xref:signalr/messagepackhubprotocol). Кроме того можно создать пользовательские протоколы.

## <a name="differences-on-the-server"></a>Различия на сервере

На стороне сервера библиотеки SignalR включаются в `Microsoft.AspNetCore.App` пакет, который является частью **веб-приложения ASP.NET Core** шаблона для проектов MVC и Razor.

SignalR представляет по промежуточного слоя ASP.NET Core, поэтому он должен быть настроен путем вызова `AddSignalR` в `Startup.ConfigureServices`.

```csharp
services.AddSignalR();
```

Чтобы настроить маршрутизацию, сопоставьте маршруты к концентраторам внутри `UseSignalR` вызов метода `Startup.Configure` метод.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a>Теперь требуется прикрепленные сеансы

Из-за как масштабирование, работали в предыдущих версиях SignalR клиенты могут повторного подключения и отправки сообщений на любом сервере в ферме. Из-за изменения в модель горизонтального масштабирования, а также не поддерживает повторных подключений к больше не поддерживается. Теперь когда клиент подключается к серверу должен взаимодействовать с одного сервера в течение всего соединения.

### <a name="single-hub-per-connection"></a>Одном концентраторе на подключение

В ASP.NET SignalR Core был упрощен модель подключения. Соединения устанавливаются непосредственно в одном концентраторе, а не одного соединения, используемого для открывать общий доступ к нескольким концентраторов.

### <a name="streaming"></a>Потоковые операторы

Теперь поддерживает SignalR [потоковой передачи данных](xref:signalr/streaming) от концентратора клиенту.

### <a name="state"></a>Регион

Возможность передавать произвольное состояние между клиентами и концентратора (часто называемые HubState) был удален, а также поддерживает сообщения о ходе выполнения. В данный момент нет не аналог концентратора учетных записей-посредников.

## <a name="differences-on-the-client"></a>Различия на стороне клиента

### <a name="typescript"></a>TypeScript

Версия ASP.NET Core SignalR записывается [TypeScript](https://www.typescriptlang.org/). Можно написать на языке JavaScript или TypeScript при использовании [клиента JavaScript](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>Клиент JavaScript, размещенного в [npm](https://www.npmjs.com/)

В предыдущих версиях клиент JavaScript был получен с помощью пакета NuGet в Visual Studio. Для версий ядра [ @aspnet/signalr пакета npm](https://www.npmjs.com/package/@aspnet/signalr) содержит библиотек JavaScript. Этот пакет не включен в **веб-приложения ASP.NET Core** шаблона. Использовать для получения и установки npm `@aspnet/signalr` пакета npm.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

Зависимость от jQuery был удален, однако по-прежнему могут использовать проекты jQuery.

### <a name="javascript-client-method-syntax"></a>Синтаксис метода JavaScript клиента

Синтаксис JavaScript отличается от предыдущей версии SignalR. Вместо использования `$connection` объектов, создания соединения с помощью `HubConnectionBuilder` API.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Используйте `connection.on` для указания клиентских методы, которые можно вызывать концентратора.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

После создания метода клиента, запустите подключение концентратора. Цепочка `catch` метод для сохранения или обработки ошибок.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Учетные записи-посредники концентратора

Больше не будет автоматически создаются прокси концентратора. Вместо этого передается имя метода `invoke` API как строка.

### <a name="net-and-other-clients"></a>.NET и других клиентов

`Microsoft.AspNetCore.SignalR.Client` Пакет NuGet содержит клиентские библиотеки .NET для ASP.NET Core SignalR.

Используйте `HubConnectionBuilder` Создание и построение экземпляр подключения к концентратору.

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Центры](xref:signalr/hubs)
* [Клиент JavaScript](xref:signalr/javascript-client)
* [Клиент .NET](xref:signalr/dotnet-client)
* [Поддерживаемые платформы](xref:signalr/supported-platforms)
