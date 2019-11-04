---
title: Платформы, поддерживаемые SignalR ASP.NET Core
author: bradygaster
description: Сведения о поддерживаемых платформах для ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/01/2019
uid: signalr/supported-platforms
ms.openlocfilehash: 1be7a307710e6e522c0088fd1ca01da11a13eda1
ms.sourcegitcommit: 77c8be22d5e88dd710f42c739748869f198865dd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/01/2019
ms.locfileid: "73426981"
---
# <a name="aspnet-core-signalr-supported-platforms"></a>Платформы, поддерживаемые SignalR ASP.NET Core

## <a name="server-system-requirements"></a>Требования к системе сервера

SignalR для ASP.NET Core поддерживает любую серверную платформу, поддерживаемую ASP.NET Core.

## <a name="javascript-client"></a>Клиент JavaScript

[Клиент JavaScript](https://www.npmjs.com/package/@aspnet/signalr) работает на NodeJS 8 и более поздних версиях и в следующих браузерах:

| Браузер                         | Version         |
| ------------------------------- | --------------- |
| Microsoft Edge                  | Текущее&dagger; |
| Mozilla Firefox                 | Текущее&dagger; |
| Google Chrome; включает Android | Текущее&dagger; |
| Обозревателе включает iOS            | Текущее&dagger; |
| Microsoft Internet Explorer     | 11              |

&dagger;*Current* относится к последней версии браузера.

## <a name="net-client"></a>Клиент .NET

[Клиент .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) выполняется на любой платформе, поддерживаемой ASP.NET Core. Например, [разработчики Xamarin могут использовать SignalR](https://github.com/aspnet/Announcements/issues/305) для создания приложений Android с помощью Xamarin. Android 8.4.0.1 и более поздних версий и приложений iOS с помощью Xamarin. iOS 11.14.0.4 и более поздних версий.

Если сервер использует службы IIS, для транспорта WebSocket требуется IIS 8,0 или более поздняя версия на Windows Server 2012 или более поздней версии. Другие транспорты поддерживаются на всех платформах.

## <a name="java-client"></a>Клиент Java

[Клиент Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) поддерживает Java 8 и более поздних версий.

## <a name="unsupported-clients"></a>Неподдерживаемые клиенты

Следующие клиенты доступны, но являются экспериментальными или неофициальными. В настоящее время они не поддерживаются и никогда не могут быть.

* [C++компьютера](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [Клиент SWIFT](https://github.com/moozzyk/SignalR-Client-Swift)
