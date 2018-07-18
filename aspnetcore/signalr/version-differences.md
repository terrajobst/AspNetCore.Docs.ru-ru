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
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a>Различия между SignalR и ASP.NET Core SignalR

ASP.NET Core SignalR не совместим с клиентами или серверами для ASP.NET SignalR. В этой статье описаны возможности, которые были удалены или изменены в ASP.NET Core SignalR.

## <a name="feature-differences"></a>Различия в функциях

### <a name="automatic-reconnects"></a>Автоматическое повторных подключений

Автоматическое повторных подключений больше не поддерживаются. Ранее SignalR попытался подключиться к серверу, если соединение было разорвано. Теперь, если клиент отключен, необходимо явным образом запустить новое соединение для повторного подключения.

### <a name="protocol-support"></a>Поддержка протоколов

ASP.NET Core SignalR поддерживает JSON, а также новый двоичный протокол, основанный на [MessagePack](xref:signalr/messagepackhubprotocol). Кроме того можно создать пользовательские протоколы.

## <a name="differences-on-the-server"></a>Различия на сервере

Серверные библиотеки SignalR, включаются в `Microsoft.AspNetCore.App` пакет, который является частью **веб-приложение ASP.NET Core** шаблон для проектов MVC и Razor.

SignalR представляет по промежуточного слоя ASP.NET Core, поэтому он должен быть настроен путем вызова `AddSignalR` в `Startup.ConfigureServices`.

```csharp
services.AddSignalR();
```

Чтобы настроить маршрутизацию, сопоставляются с маршруты концентраторов внутри `UseSignalR` вызов метода `Startup.Configure` метод.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a>Теперь требуется прикрепленные сеансы

Из-за как горизонтальное масштабирование работали в предыдущих версиях SignalR клиенты сможет повторно подключиться и отправлять сообщения на любой сервер в ферме. Из-за изменения модели горизонтального масштабирования, а также не поддерживает повторных подключений это больше не поддерживается. Теперь когда клиент подключается к серверу, ей нужно взаимодействовать с тем же сервером в течение всего соединения.

### <a name="single-hub-per-connection"></a>Единый центр на соединение

В ASP.NET Core SignalR упрощена модель соединения. Соединения устанавливаются непосредственно в одном центре, а не одного соединения, используемого для общий доступ для нескольких центров.

### <a name="streaming"></a>Потоковые операторы

Теперь поддерживает SignalR [потоковой передачи данных](xref:signalr/streaming) от концентратора клиенту.

### <a name="state"></a>Регион

Возможность передавать произвольное состояние между клиентами и концентратор (часто называемые HubState) был удален, а также поддержка сообщения о ходе выполнения. На данный момент нет не существовал центра учетных записей-посредников.

## <a name="differences-on-the-client"></a>Различия на стороне клиента

### <a name="typescript"></a>TypeScript

Создается на языке версии ASP.NET Core SignalR [TypeScript](https://www.typescriptlang.org/). Можно написать на языке JavaScript или TypeScript при использовании [клиента JavaScript](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>Клиент JavaScript размещается в [npm](https://www.npmjs.com/)

В предыдущих версиях клиента JavaScript был получен через пакет NuGet в Visual Studio. Для версий ядра [ @aspnet/signalr пакета npm](https://www.npmjs.com/package/@aspnet/signalr) содержит библиотеки JavaScript. Этот пакет не входит в **веб-приложение ASP.NET Core** шаблона. Получить и установить с помощью npm `@aspnet/signalr` пакета npm.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

Зависимость от jQuery был удален, однако проекты по-прежнему можете использовать jQuery.

### <a name="javascript-client-method-syntax"></a>Синтаксис метода клиента JavaScript

Синтаксис JavaScript был изменен с предыдущей версии SignalR. Вместо использования `$connection` следует создать соединения с помощью `HubConnectionBuilder` API.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Используйте `connection.on` для указания клиентских методов, которые могут вызывать концентратора.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

После создания метода клиента, запустите подключение концентратора. Цепочка `catch` метод входа или обработки ошибок.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Центр учетных записей-посредников

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
