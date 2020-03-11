---
title: Различия между SignalR и ASP.NET Core SignalR
author: bradygaster
description: Различия между SignalR и ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/21/2019
no-loc:
- SignalR
uid: signalr/version-differences
ms.openlocfilehash: cca9a0cb0c46fc25eb5d1f7127d31fd3ab92f0b4
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78653536"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a>Различия между ASP.NET SignalR и SignalR ASP.NET Core

ASP.NET Core SignalR несовместим с клиентами или серверами для ASP.NET SignalR. В этой статье подробно описаны функции, которые были удалены или изменены в ASP.NET Core SignalR.

## <a name="how-to-identify-the-signalr-version"></a>Как опознать версию SignalR

::: moniker range=">= aspnetcore-3.0"

|                      | ASP.NET SignalR | SignalR для ASP.NET Core |
| -------------------- | --------------- | -------------------- |
| Пакет NuGet сервера | [Microsoft. AspNet. SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | Нет. Входит в состав общей платформы [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) . |
| Клиентские пакеты NuGet | [Microsoft. AspNet. SignalR. Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[Microsoft. AspNet. SignalR. JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [Microsoft. AspNetCore. SignalR. Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| Пакет NPM клиента JavaScript | [signalr](https://www.npmjs.com/package/signalr) | [`@microsoft/signalr`](https://www.npmjs.com/package/@microsoft/signalr) |
| Клиент Java | [Репозиторий GitHub](https://github.com/SignalR/java-client) (не рекомендуется)  | Пакет Maven [com. Microsoft. SignalR](https://search.maven.org/artifact/com.microsoft.signalr/signalr) |
| Тип серверного приложения | ASP.NET (System. Web) или OWIN Self-Host | ASP.NET Core |
| Поддерживаемые серверные платформы | .NET Framework 4,5 или более поздней версии | .NET Core 3,0 или более поздней версии |

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

|                      | ASP.NET SignalR | SignalR для ASP.NET Core |
| -------------------- | --------------- | -------------------- |
| Пакет NuGet сервера | [Microsoft. AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | [Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)<br>[Microsoft. AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework) |
| Клиентские пакеты NuGet | [Microsoft. AspNet.SignalR. Компьютера](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[Microsoft. AspNet.SignalR. JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [Microsoft. AspNetCore.SignalR. Компьютера](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| Пакет NPM клиента JavaScript | [signalr](https://www.npmjs.com/package/signalr) | [`@aspnet/signalr`](https://www.npmjs.com/package/@aspnet/signalr) |
| Клиент Java | [Репозиторий GitHub](https://github.com/SignalR/java-client) (не рекомендуется)  | Пакет Maven [com. Microsoft. SignalR](https://search.maven.org/artifact/com.microsoft.signalr/signalr) |
| Тип серверного приложения | ASP.NET (System. Web) или OWIN Self-Host | ASP.NET Core |
| Поддерживаемые серверные платформы | .NET Framework 4,5 или более поздней версии | .NET Framework 4.6.1 или более поздней версии<br>.NET Core 2,1 или более поздней версии |

::: moniker-end

## <a name="feature-differences"></a>Различия в функциях

### <a name="automatic-reconnects"></a>Автоматические повторное подключения

::: moniker range=">= aspnetcore-3.0"

В SignalRASP.NET:

* По умолчанию SignalR пытается повторно подключиться к серверу при разрыве соединения. 

В ASP.NET Core SignalR:

* Автоматические повторное подключение — это явное согласие с [клиентом .NET](xref:signalr/dotnet-client#automatically-reconnect) и [клиентом JavaScript](xref:signalr/javascript-client#automatically-reconnect):

```csharp
HubConnection connection = new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

До ASP.NET Core 3,0 SignalR не поддерживает автоматическое повторное подключение. Если Клиент отключен, пользователь должен явно запустить новое подключение для повторного подключения. В SignalRASP.NET SignalR пытается повторно подключиться к серверу при разрыве соединения.

::: moniker-end

### <a name="protocol-support"></a>Поддержка протоколов

ASP.NET Core SignalR поддерживает JSON, а также новый двоичный протокол на основе [MessagePack](xref:signalr/messagepackhubprotocol). Кроме того, можно создавать пользовательские протоколы.

### <a name="transports"></a>Транспорты

Непрерывный многокадровый транспорт не поддерживается в ASP.NET Core SignalR.

## <a name="differences-on-the-server"></a>Различия на сервере

Библиотеки ASP.NET Core SignalR на стороне сервера включены в [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), который используется в шаблоне **веб-приложения ASP.NET Core** для проектов Razor и MVC.

ASP.NET Core SignalR является ASP.NET Core по промежуточного слоя. Его необходимо настроить, вызвав <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddSignalR%2A> в `Startup.ConfigureServices`.

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

Чтобы настроить маршрутизацию, сопоставьте маршруты с концентраторами внутри вызова метода <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints%2A> в методе `Startup.Configure`.

```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Чтобы настроить маршрутизацию, сопоставьте маршруты с концентраторами внутри вызова метода <xref:Microsoft.AspNetCore.Builder.SignalRAppBuilderExtensions.UseSignalR%2A> в методе `Startup.Configure`.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a>Закрепленные сеансы

Модель масштабирования для ASP.NET SignalR позволяет клиентам повторно подключаться и передавать сообщения на любой сервер в ферме. В ASP.NET Core SignalRклиент должен взаимодействовать с тем же сервером в течение всего соединения. Для масштабирования с помощью Redis, это означает, что требуются прикрепленные сеансы. Для масштабирования с помощью [службы SignalR Azure](/azure/azure-signalr/)прикрепленные сеансы не требуются, так как служба обрабатывает соединения с клиентами.

### <a name="single-hub-per-connection"></a>Один концентратор на подключение

В ASP.NET Core SignalRмодель подключения была упрощена. Подключения осуществляются непосредственно в одном концентраторе, а не в одном соединении, используемом для совместного доступа к нескольким концентраторам.

### <a name="streaming"></a>Потоковая передача

ASP.NET Core SignalR теперь поддерживает [потоковую передачу данных](xref:signalr/streaming) от концентратора клиенту.

### <a name="state"></a>Штат

Возможность передачи произвольного состояния между клиентами и концентратором (часто называется `HubState`) была удалена, а также поддержкой сообщений о ходе выполнения. В данный момент не существует аналога прокси-серверов концентратора.

### <a name="persistentconnection-removal"></a>Удаление Персистентконнектион

В ASP.NET Core SignalRкласс [персистентконнектион](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) был удален.

### <a name="globalhost"></a>GlobalHost

ASP.NET Core в платформе встроена встраивание зависимостей (DI). Службы могут использовать DI для доступа к [хубконтекст](xref:signalr/hubcontext). Объект `GlobalHost`, используемый в ASP.NET SignalR для получения `HubContext`, не существует в ASP.NET Core SignalR.

### <a name="hubpipeline"></a>хубпипелине

ASP.NET Core SignalR не поддерживает модули `HubPipeline`.

## <a name="differences-on-the-client"></a>Различия в клиенте

### <a name="typescript"></a>TypeScript

Клиент ASP.NET Core SignalR записывается в [TypeScript](https://www.typescriptlang.org/). При использовании [клиента JavaScript](xref:signalr/javascript-client)можно писать на JavaScript или TypeScript.

### <a name="the-javascript-client-is-hosted-at-npm"></a>Клиент JavaScript размещается по адресу NPM

::: moniker range=">= aspnetcore-3.0"

В ASP.NET версиях клиент JavaScript был получен с помощью пакета NuGet в Visual Studio. В ASP.NET Core версиях пакет [`@microsoft/signalr`](https://www.npmjs.com/package/@microsoft/signalr) NPM содержит библиотеки JavaScript. Этот пакет не входит в шаблон **веб-приложения ASP.NET Core** . Используйте NPM для получения и установки пакета NPM `@microsoft/signalr`.

```console
npm init -y
npm install @microsoft/signalr
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

В ASP.NET версиях клиент JavaScript был получен с помощью пакета NuGet в Visual Studio. В ASP.NET Core версиях пакет [`@aspnet/signalr`](https://www.npmjs.com/package/@aspnet/signalr) NPM содержит библиотеки JavaScript. Этот пакет не входит в шаблон **веб-приложения ASP.NET Core** . Используйте NPM для получения и установки пакета NPM `@aspnet/signalr`.

```console
npm init -y
npm install @aspnet/signalr
```

::: moniker-end

### <a name="jquery"></a>jQuery

Зависимость от jQuery удалена, однако проекты по-прежнему могут использовать jQuery.

### <a name="internet-explorer-support"></a>Поддержка Internet Explorer

Для ASP.NET Core SignalR требуется Microsoft Internet Explorer 11 или более поздняя версия (ASP.NET SignalR поддерживал Microsoft Internet Explorer 8 и более поздние версии).

### <a name="javascript-client-method-syntax"></a>Синтаксис клиентского метода JavaScript

::: moniker range=">= aspnetcore-3.0"

Синтаксис JavaScript изменился с ASP.NET версии SignalR. Вместо использования объекта `$connection` создайте подключение с помощью API [хубконнектионбуилдер](/javascript/api/@aspnet/signalr/hubconnectionbuilder) .

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Используйте метод [On](/javascript/api/@microsoft/signalr/HubConnection#on) , чтобы указать клиентские методы, которые может вызывать концентратор.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Синтаксис JavaScript изменился с ASP.NET версии SignalR. Вместо использования объекта `$connection` создайте подключение с помощью API [хубконнектионбуилдер](/javascript/api/@microsoft/signalr/hubconnectionbuilder) .

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Используйте метод [On](/javascript/api/@aspnet/signalr/HubConnection#on) , чтобы указать клиентские методы, которые может вызывать концентратор.

::: moniker-end

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = `${user} says ${msg}`;
    console.log(encodedMsg);
});
```

После создания клиентского метода запустите подключение концентратора. Привязать метод [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) для ведения журнала или обработке ошибок.

```javascript
connection.start().catch(err => console.error(err));
```

### <a name="hub-proxies"></a>Прокси-серверы концентратора

::: moniker range=">= aspnetcore-3.0"

Прокси-серверы концентратора больше не создаются автоматически. Вместо этого имя метода передается в API [вызова](/javascript/api/@microsoft/signalr/hubconnection#invoke) в виде строки.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Прокси-серверы концентратора больше не создаются автоматически. Вместо этого имя метода передается в API [вызова](/javascript/api/@aspnet/signalr/hubconnection#invoke) в виде строки.

::: moniker-end

### <a name="net-and-other-clients"></a>.NET и другие клиенты

Объект [Microsoft. AspNetCore.SignalR. Клиентский](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client) пакет NuGet содержит клиентские библиотеки .NET для ASP.NET Core SignalR.

Используйте <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>, чтобы создать и построить экземпляр подключения к концентратору.

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a>Различия масштабирования

ASP.NET SignalR поддерживает SQL Server и Redis. ASP.NET Core SignalR поддерживает службу SignalR Azure и Redis.

### <a name="aspnet"></a>ASP.NET

* [SignalR масштабирование с помощью служебной шины Azure](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [SignalR масштабирование с помощью Redis](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [SignalR масштабирование с помощью SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a>ASP.NET Core

* [Служба SignalR Azure](/azure/azure-signalr/)
* [Объединительная плата Redis](xref:signalr/redis-backplane)

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Центры](xref:signalr/hubs)
* [Клиент на JavaScript](xref:signalr/javascript-client)
* [Клиент .NET](xref:signalr/dotnet-client)
* [Поддерживаемые платформы](xref:signalr/supported-platforms)
