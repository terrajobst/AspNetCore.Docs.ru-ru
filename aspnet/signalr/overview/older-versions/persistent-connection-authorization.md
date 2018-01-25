---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: "Проверка подлинности и авторизация для постоянного подключения SignalR (SignalR 1.x) | Документы Microsoft"
author: pfletcher
description: "В этом разделе описывается принудительной авторизации для постоянного подключения. Общие сведения об интеграции безопасности в приложении SignalR..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 2e97dfd03c61b110325c41a992b4af490fcd17de
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>Проверка подлинности и авторизация для постоянного подключения SignalR (SignalR 1.x)
====================
по [Флетчера Патрик](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> В этом разделе описывается принудительной авторизации для постоянного подключения. Общие сведения об интеграции безопасности в приложении SignalR см. в разделе [введение в безопасность](index.md).


## <a name="enforce-authorization"></a>Принудительной авторизации

Для применения правила авторизации при использовании [подключение PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) необходимо переопределить `AuthorizeRequest` метод. Нельзя использовать `Authorize` атрибута с постоянные подключения. `AuthorizeRequest` Метод вызывается инфраструктурой SignalR перед каждым запросом, чтобы убедиться, что пользователь авторизован для выполнения запрошенного действия. `AuthorizeRequest` Метод не вызывается из клиента; вместо этого проверки подлинности пользователя через приложения стандартным механизмом проверки подлинности.

В приведенном ниже примере показано, как ограничить запросы пользователям, прошедшим проверку.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Можно добавить логику авторизации, настроенные в методе AuthorizeRequest; например проверка принадлежности пользователя к определенной роли.
