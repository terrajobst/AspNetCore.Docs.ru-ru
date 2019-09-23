---
title: Функции клиента SignalR
author: bradygaster
description: Узнайте, какие функции поддерживаются различными клиентами SignalR ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 55086673e0c9f9b73f07730ea25c3fa322f7fd98
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187392"
---
# <a name="aspnet-core-signalr-client-features"></a>Функции клиента SignalR ASP.NET Core

## <a name="feature-distribution"></a>Распространение компонентов

В таблице ниже приведены функции и поддержка для клиентов, которые предлагают поддержку в режиме реального времени.

| Функция | .NET Core | JavaScript | Java |
| ---- | :-: | :-: | :-: |
| [Потоковая передача из сервера в клиент](xref:signalr/streaming)          |✔|✔|✔|
| [Потоковая передача клиента в сервер](xref:signalr/streaming)          |✔|✔|✔|
| Автоматическое повторное подключение ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))          |✔|✔| |

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Приступая к работе с SignalR для ASP.NET Core](xref:tutorials/signalr)
* [Поддерживаемые платформы](xref:signalr/supported-platforms)
* [Центры](xref:signalr/hubs)
* [Клиент JavaScript](xref:signalr/javascript-client)
