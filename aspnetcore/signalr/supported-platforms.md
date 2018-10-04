---
title: ASP.NET Core SignalR поддерживаемые платформы
author: tdykstra
description: Поддерживаемые платформы для ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/26/2018
uid: signalr/supported-platforms
ms.openlocfilehash: d6d74a55d35ddb34a6f66a171bfe3f343dd61b63
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577630"
---
# <a name="aspnet-core-signalr-supported-platforms"></a>ASP.NET Core SignalR поддерживаемые платформы

## <a name="server-system-requirements"></a>Системные требования для сервера

SignalR для ASP.NET Core поддерживает любой серверной платформой ASP.NET Core поддерживает.

## <a name="javascript-client"></a>Клиент JavaScript

[Клиента JavaScript](https://www.npmjs.com/package/@aspnet/signalr) работает на NodeJS 8 и более поздних версиях, а также следующие браузеры:

| Браузер | Версия |
| ------- | ------- |
| Microsoft Edge | текущий |
| Mozilla Firefox | текущий |
| Google Chrome; включает в себя Android | текущий |
| Safari; включает в себя iOS | текущий |
| Microsoft Internet Explorer | 11 |
 
## <a name="net-client"></a>Клиент .NET

[Клиента .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) выполняется на любой платформе сервера, поддерживаемых ASP.NET Core.

Когда на сервере IIS, транспортный протокол WebSocket требует IIS 8.0 или более поздней версии, в Windows Server 2012 или более поздней версии. Другие транспорты, поддерживаются на всех платформах.

## <a name="java-client"></a>Клиент Java

[Клиента Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) поддерживает Java 8 и более поздних версий.

## <a name="unsupported-clients"></a>Неподдерживаемый клиентов

Следующие клиенты доступны, но экспериментальной или неофициальный. Они теперь не поддерживаются и могут никогда не будут поддерживаться.

* [Клиент C++](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [SWIFT клиента](https://github.com/moozzyk/SignalR-Client-Swift)
