---
title: Функции клиентов SignalR
author: bradygaster
description: Узнайте, какие функции поддерживаются различными клиентами SignalR ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 2d6759a5484c37aee6db3d22b3127414231605ae
ms.sourcegitcommit: 14b25156e34c82ed0495b4aff5776ac5b1950b5e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/26/2019
ms.locfileid: "71301192"
---
# <a name="aspnet-core-signalr-client-features"></a>Функции клиента SignalR ASP.NET Core

## <a name="feature-distribution"></a>Распространение компонентов

В таблице ниже приведены функции и поддержка для клиентов, которые предлагают поддержку в режиме реального времени.

| Функция | .NET | JavaScript | Java |
| ---- | :-: | :-: | :-: |
| Поддержка службы SignalR Azure |✔|✔|✔|
| [Потоковая передача из сервера в клиент](xref:signalr/streaming)          |✔|✔|✔|
| [Потоковая передача клиента в сервер](xref:signalr/streaming)          |✔|✔|✔|
| Автоматическое повторное подключение ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))          |✔|✔| |
| Транспорт WebSockets |✔|✔|✔|
| Транспорт событий, отправленных сервером |✔|✔| |
| Длинный опрашивающий транспорт |✔|✔|✔|
| Протокол концентратора JSON |✔|✔|✔|
| Протокол MessagePack для концентратора |✔|✔| |

Поддержка автоматического повторного подключения в клиенте Java осуществляется с помощью средства [записи вопросов](https://github.com/aspnet/AspNetCore/issues/8711).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Приступая к работе с SignalR для ASP.NET Core](xref:tutorials/signalr)
* [Поддерживаемые платформы](xref:signalr/supported-platforms)
* [Центры](xref:signalr/hubs)
* [Клиент JavaScript](xref:signalr/javascript-client)
