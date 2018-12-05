---
title: ASP.NET Core SignalR поддерживаемые платформы
author: tdykstra
description: Дополнительные сведения о поддерживаемых платформ для ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/12/2018
uid: signalr/supported-platforms
ms.openlocfilehash: be3d4d0049395fb2499bd0b4aac126e953ce7910
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861723"
---
# <a name="aspnet-core-signalr-supported-platforms"></a>ASP.NET Core SignalR поддерживаемые платформы

## <a name="server-system-requirements"></a>Системные требования для сервера

SignalR для ASP.NET Core поддерживает любой платформе сервера, которая поддерживает ASP.NET Core.

## <a name="javascript-client"></a>Клиент JavaScript

[Клиента JavaScript](https://www.npmjs.com/package/@aspnet/signalr) работает на NodeJS 8 и более поздних версиях, а также следующие браузеры:

| Браузер                         | Версия |
| ------------------------------- | ------- |
| Microsoft Edge                  | текущий |
| Mozilla Firefox                 | текущий |
| Google Chrome; включает в себя Android | текущий |
| Safari; включает в себя iOS            | текущий |
| Microsoft Internet Explorer     | 11      |
 
## <a name="net-client"></a>Клиент .NET

[Клиента .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) выполняется на любой платформе, поддерживаемой ASP.NET Core. Например [Xamarin разработчики могут использовать SignalR](https://github.com/aspnet/Announcements/issues/305) для создания приложения Android с помощью Xamarin.Android 8.4.0.1 и более поздней версии и приложений iOS с помощью Xamarin.iOS 11.14.0.4 и более поздних версий.

Если сервер работает IIS, транспортный протокол WebSocket требует IIS 8.0 или более поздней версии на Windows Server 2012 или более поздней версии. Другие транспорты, поддерживаются на всех платформах.

## <a name="java-client"></a>Клиент Java

[Клиента Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) поддерживает Java 8 и более поздних версий.

## <a name="unsupported-clients"></a>Неподдерживаемый клиентов

Следующие клиенты доступны, но экспериментальной или неофициальный. Они уже не поддерживаются и никогда не может быть.

* [Клиент C++](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [SWIFT клиента](https://github.com/moozzyk/SignalR-Client-Swift)
