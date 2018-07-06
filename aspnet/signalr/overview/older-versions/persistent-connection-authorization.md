---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Проверка подлинности и авторизация для постоянных подключений SignalR (SignalR 1.x) | Документация Майкрософт
author: pfletcher
description: В этом разделе описывается, как требовать принудительной авторизации на постоянное подключение. Общие сведения об интеграции безопасности в приложении SignalR...
ms.author: aspnetcontent
ms.date: 10/21/2013
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 28984518346ef7e79c976a565dae5e5ab924b678
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805331"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>Проверка подлинности и авторизация для постоянных подключений SignalR (SignalR 1.x)
====================
по [Флетчера Патрик](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> В этом разделе описывается, как требовать принудительной авторизации на постоянное подключение. Общие сведения об интеграции безопасности в приложении SignalR, см. в разделе [Общие сведения о безопасности](index.md).


## <a name="enforce-authorization"></a>Требовать принудительной авторизации

Для принудительного выполнения правил авторизации, при использовании [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) необходимо переопределить `AuthorizeRequest` метод. Нельзя использовать `Authorize` атрибута с постоянные подключения. `AuthorizeRequest` Метод вызывается инфраструктурой SignalR перед каждым запросом, чтобы убедиться, что пользователь авторизован для выполнения запрошенного действия. `AuthorizeRequest` Метод не вызывается из клиента; вместо этого проверки подлинности пользователя через механизм стандартной проверки подлинности приложения.

В приведенном ниже примере показано, как ограничить запросы пользователям, прошедшим проверку.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Можно добавить логику настраиваемой авторизации в методе AuthorizeRequest; например проверка, принадлежит ли пользователь к определенной роли.
