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
ms.openlocfilehash: 4c036ddf1e20e3a3be7b043d90b594292013f6c2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="fd8ab-104">Проверка подлинности и авторизация для постоянного подключения SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="fd8ab-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>
====================
<span data-ttu-id="fd8ab-105">по [Флетчера Патрик](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="fd8ab-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="fd8ab-106">В этом разделе описывается принудительной авторизации для постоянного подключения.</span><span class="sxs-lookup"><span data-stu-id="fd8ab-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="fd8ab-107">Общие сведения об интеграции безопасности в приложении SignalR см. в разделе [введение в безопасность](index.md).</span><span class="sxs-lookup"><span data-stu-id="fd8ab-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="fd8ab-108">Принудительной авторизации</span><span class="sxs-lookup"><span data-stu-id="fd8ab-108">Enforce authorization</span></span>

<span data-ttu-id="fd8ab-109">Для применения правила авторизации при использовании [подключение PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) необходимо переопределить `AuthorizeRequest` метод.</span><span class="sxs-lookup"><span data-stu-id="fd8ab-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="fd8ab-110">Нельзя использовать `Authorize` атрибута с постоянные подключения.</span><span class="sxs-lookup"><span data-stu-id="fd8ab-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="fd8ab-111">`AuthorizeRequest` Метод вызывается инфраструктурой SignalR перед каждым запросом, чтобы убедиться, что пользователь авторизован для выполнения запрошенного действия.</span><span class="sxs-lookup"><span data-stu-id="fd8ab-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="fd8ab-112">`AuthorizeRequest` Метод не вызывается из клиента; вместо этого проверки подлинности пользователя через приложения стандартным механизмом проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="fd8ab-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="fd8ab-113">В приведенном ниже примере показано, как ограничить запросы пользователям, прошедшим проверку.</span><span class="sxs-lookup"><span data-stu-id="fd8ab-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="fd8ab-114">Можно добавить логику авторизации, настроенные в методе AuthorizeRequest; например проверка принадлежности пользователя к определенной роли.</span><span class="sxs-lookup"><span data-stu-id="fd8ab-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
