---
title: Различия между SignalR и ASP.NET Core SignalR
author: bradygaster
description: Различия между SignalR и ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/14/2018
uid: signalr/version-differences
ms.openlocfilehash: 70b09493d9b4c96c897465d60e53e93a793c42f9
ms.sourcegitcommit: 387cf29f5d5addef2cbc70670a11d612806b36b2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70746538"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a>Различия между ASP.NET SignalR и SignalR ASP.NET Core

ASP.NET Core SignalR несовместим с клиентами или серверами для ASP.NET SignalR. В этой статье подробно описаны функции, которые были удалены или изменены в ASP.NET Core SignalR.

## <a name="how-to-identify-the-signalr-version"></a>Как опознать версию SignalR

|                      | ASP.NET SignalR | ASP.NET Core SignalR |
| -------------------- | --------------- | -------------------- |
| Пакет NuGet сервера | [Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | [Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)<br>[Microsoft. AspNetCore. SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework) |
| Клиентские пакеты NuGet | [Microsoft. AspNet. SignalR. Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[Microsoft. AspNet. SignalR. JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| Клиентский пакет NPM | [SignalR](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| Клиент Java | [Репозиторий GitHub](https://github.com/SignalR/java-client) не рекомендуется  | Пакет Maven [com. Microsoft. SignalR](https://search.maven.org/artifact/com.microsoft.signalr/signalr) |
| Тип серверного приложения | ASP.NET (System. Web) или OWIN Self-Host | ASP.NET Core |
| Поддерживаемые серверные платформы | .NET Framework 4,5 или более поздней версии | .NET Framework 4.6.1 или более поздней версии<br>.NET Core 2,1 или более поздней версии |

## <a name="feature-differences"></a>Различия в функциях

### <a name="automatic-reconnects"></a>Автоматические повторное подключения

Автоматические повторное подключения не поддерживаются в ASP.NET Core SignalR. Если Клиент отключен, пользователь должен явно запустить новое подключение, если требуется повторное подключение. В ASP.NET SignalR SignalR пытается повторно подключиться к серверу при разрыве соединения.

### <a name="protocol-support"></a>Поддержка протокола

ASP.NET Core SignalR поддерживает JSON, а также новый двоичный протокол на основе [MessagePack](xref:signalr/messagepackhubprotocol). Кроме того, можно создавать пользовательские протоколы.

### <a name="transports"></a>Транспорты

Непостоянное многокадровый транспорт не поддерживается в ASP.NET Core SignalR.

## <a name="differences-on-the-server"></a>Различия на сервере

Серверные библиотеки ASP.NET Core SignalR включены в пакет [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app) , который входит в состав шаблона **ASP.NET Core веб-приложения** для проектов Razor и MVC.

ASP.NET Core SignalR является ASP.NET Core по промежуточного слоя, поэтому его необходимо настроить, вызвав [аддсигналр](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) в `Startup.ConfigureServices`.

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

Чтобы настроить маршрутизацию, сопоставьте маршруты с концентраторами внутри вызова метода [усиндпоинтс](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) в `Startup.Configure` методе.


```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Чтобы настроить маршрутизацию, сопоставьте маршруты с концентраторами внутри вызова метода [усесигналр](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) в `Startup.Configure` методе.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a>Закрепленные сеансы

Модель масштабирования для ASP.NET SignalR позволяет клиентам повторно подключаться и передавать сообщения на любой сервер в ферме. В ASP.NET Core SignalR клиент должен взаимодействовать с тем же сервером на время соединения. Для масштабирования с помощью Redis, это означает, что требуются прикрепленные сеансы. Для масштабирования с помощью [службы Azure SignalR](/azure/azure-signalr/)не требуется использовать закрепленные сеансы, так как служба обрабатывает соединения с клиентами.

### <a name="single-hub-per-connection"></a>Один концентратор на подключение

В ASP.NET Core SignalR модель подключения была упрощена. Подключения осуществляются непосредственно в одном концентраторе, а не в одном соединении, используемом для совместного доступа к нескольким концентраторам.

### <a name="streaming"></a>Потоковые операторы

ASP.NET Core SignalR теперь поддерживает [потоковую передачу данных](xref:signalr/streaming) от концентратора клиенту.

### <a name="state"></a>Область

Возможность передачи произвольного состояния между клиентами и концентратором (часто называется Хубстате) была удалена, а также поддержкой сообщений о ходе выполнения. В данный момент не существует аналога прокси-серверов концентратора.

### <a name="persistentconnection-removal"></a>Удаление Персистентконнектион

В ASP.NET Core SignalR был удален класс [персистентконнектион](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) .

### <a name="globalhost"></a>GlobalHost

ASP.NET Core в платформе встроена встраивание зависимостей (DI). Службы могут использовать DI для доступа к [хубконтекст](xref:signalr/hubcontext). Объект, используемый в ASP.NET SignalR для получения, `HubContext` не существует в ASP.NET Core SignalR. `GlobalHost`

### <a name="hubpipeline"></a>хубпипелине

ASP.NET Core SignalR не поддерживает `HubPipeline` модули.

## <a name="differences-on-the-client"></a>Различия в клиенте

### <a name="typescript"></a>TypeScript

Клиент SignalR ASP.NET Core записывается в [TypeScript](https://www.typescriptlang.org/). При использовании [клиента JavaScript](xref:signalr/javascript-client)можно писать на JavaScript или TypeScript.

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>Клиент JavaScript размещается по адресу [NPM](https://www.npmjs.com/)

В предыдущих версиях клиент JavaScript был получен с помощью пакета NuGet в Visual Studio. Для основных версий [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) пакет NPM содержит библиотеки JavaScript. Этот пакет не входит в шаблон **веб-приложения ASP.NET Core** . Используйте NPM для получения и установки `@aspnet/signalr` пакета NPM.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

Зависимость от jQuery удалена, однако проекты по-прежнему могут использовать jQuery.

### <a name="internet-explorer-support"></a>Поддержка Internet Explorer

Для ASP.NET Core SignalR требуется Microsoft Internet Explorer 11 или более поздняя версия (ASP.NET SignalR поддерживал Microsoft Internet Explorer 8 и более поздние версии).

### <a name="javascript-client-method-syntax"></a>Синтаксис клиентского метода JavaScript

Синтаксис JavaScript был изменен с предыдущей версии SignalR. Вместо использования `$connection` объекта создайте подключение с помощью API [хубконнектионбуилдер](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) .

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Используйте метод [On](/javascript/api/@aspnet/signalr/HubConnection#on) , чтобы указать клиентские методы, которые может вызывать концентратор.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

После создания клиентского метода запустите подключение концентратора. Привязать метод [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) для ведения журнала или обработке ошибок.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Прокси-серверы концентратора

Прокси-серверы концентратора больше не создаются автоматически. Вместо этого имя метода передается в API [вызова](/javascript/api/%40aspnet/signalr/hubconnection#invoke) в виде строки.

### <a name="net-and-other-clients"></a>.NET и другие клиенты

Пакет `Microsoft.AspNetCore.SignalR.Client` NuGet содержит клиентские библиотеки .NET для ASP.NET Core SignalR.

Используйте [хубконнектионбуилдер](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) , чтобы создать и построить экземпляр соединения с концентратором.

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a>Различия масштабирования

ASP.NET SignalR поддерживает SQL Server и Redis. ASP.NET Core SignalR поддерживает службу Azure SignalR и Redis.

### <a name="aspnet"></a>ASP.NET

* [Масштабирование SignalR с помощью служебной шины Azure](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [Масштабирование SignalR с помощью Redis](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [Масштабирование SignalR с SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a>ASP.NET Core

* [Служба Azure SignalR](/azure/azure-signalr/)
* [Объединительная плата Redis](xref:signalr/redis-backplane)

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Центры](xref:signalr/hubs)
* [Клиент JavaScript](xref:signalr/javascript-client)
* [Клиент .NET](xref:signalr/dotnet-client)
* [Поддерживаемые платформы](xref:signalr/supported-platforms)
