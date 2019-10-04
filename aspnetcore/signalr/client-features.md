---
title: Функции клиентов SignalR
author: bradygaster
description: Узнайте, какие функции поддерживаются различными клиентами SignalR ASP.NET Core.
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 6718722cdbcfae500026fcd429ca6b9de8f0f103
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925359"
---
# <a name="aspnet-core-signalr-client-features"></a>Функции клиента SignalR ASP.NET Core

## <a name="feature-distribution"></a>Распространение компонентов

В таблице ниже приведены функции и поддержка для клиентов, которые предлагают поддержку в режиме реального времени. Для каждого компонента отображается *Минимальная* версия, поддерживающая эту функцию. Если версия не указана, эта функция не поддерживается.

| Компонент | .NET | JavaScript | Java |
| ---- | :-: | :-: | :-: |
| Поддержка службы SignalR Azure |1.0.0|1.0.0|1.0.0|
| [Потоковая передача из сервера в клиент](xref:signalr/streaming)          |1.0.0|1.0.0|1.0.0|
| [Потоковая передача клиента в сервер](xref:signalr/streaming)          |3.0.0|3.0.0|3.0.0|
| Автоматическое повторное подключение ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))          |3.0.0|3.0.0|❌|
| Транспорт WebSockets |1.0.0|1.0.0|1.0.0|
| Транспорт событий, отправленных сервером |1.0.0|1.0.0|❌|
| Длинный опрашивающий транспорт |1.0.0|1.0.0|3.0.0|
| Протокол концентратора JSON |1.0.0|1.0.0|1.0.0|
| Протокол MessagePack для концентратора |1.0.0|1.0.0|❌|

Поддержка автоматического повторного подключения в клиенте Java осуществляется с помощью средства [записи вопросов](https://github.com/aspnet/AspNetCore/issues/8711).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Приступая к работе с SignalR для ASP.NET Core](xref:tutorials/signalr)
* [Поддерживаемые платформы](xref:signalr/supported-platforms)
* [Центры](xref:signalr/hubs)
* [Клиент JavaScript](xref:signalr/javascript-client)
