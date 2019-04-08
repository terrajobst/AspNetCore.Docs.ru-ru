---
title: ASP.NET Core SignalR поддерживаемые платформы
author: bradygaster
description: Дополнительные сведения о поддерживаемых платформ для ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/06/2019
uid: signalr/supported-platforms
ms.openlocfilehash: fefaaf97de3f1fabf8f3154bf5b4ccb37195ccff
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068226"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="99be4-103">ASP.NET Core SignalR поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="99be4-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="99be4-104">Системные требования для сервера</span><span class="sxs-lookup"><span data-stu-id="99be4-104">Server system requirements</span></span>

<span data-ttu-id="99be4-105">SignalR для ASP.NET Core поддерживает любой платформе сервера, которая поддерживает ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="99be4-105">SignalR for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="99be4-106">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="99be4-106">JavaScript client</span></span>

<span data-ttu-id="99be4-107">[Клиента JavaScript](https://www.npmjs.com/package/@aspnet/signalr) работает на NodeJS 8 и более поздних версиях, а также следующие браузеры:</span><span class="sxs-lookup"><span data-stu-id="99be4-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="99be4-108">Браузер</span><span class="sxs-lookup"><span data-stu-id="99be4-108">Browser</span></span>                         | <span data-ttu-id="99be4-109">Версия</span><span class="sxs-lookup"><span data-stu-id="99be4-109">Version</span></span> |
| ------------------------------- | ------- |
| <span data-ttu-id="99be4-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="99be4-110">Microsoft Edge</span></span>                  | <span data-ttu-id="99be4-111">текущий</span><span class="sxs-lookup"><span data-stu-id="99be4-111">current</span></span> |
| <span data-ttu-id="99be4-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="99be4-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="99be4-113">текущий</span><span class="sxs-lookup"><span data-stu-id="99be4-113">current</span></span> |
| <span data-ttu-id="99be4-114">Google Chrome; включает в себя Android</span><span class="sxs-lookup"><span data-stu-id="99be4-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="99be4-115">текущий</span><span class="sxs-lookup"><span data-stu-id="99be4-115">current</span></span> |
| <span data-ttu-id="99be4-116">Safari; включает в себя iOS</span><span class="sxs-lookup"><span data-stu-id="99be4-116">Safari; includes iOS</span></span>            | <span data-ttu-id="99be4-117">текущий</span><span class="sxs-lookup"><span data-stu-id="99be4-117">current</span></span> |
| <span data-ttu-id="99be4-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="99be4-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="99be4-119">11</span><span class="sxs-lookup"><span data-stu-id="99be4-119">11</span></span>      |
 
## <a name="net-client"></a><span data-ttu-id="99be4-120">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="99be4-120">.NET client</span></span>

<span data-ttu-id="99be4-121">[Клиента .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) выполняется на любой платформе, поддерживаемой ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="99be4-121">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any platform supported by ASP.NET Core.</span></span> <span data-ttu-id="99be4-122">Например [Xamarin разработчики могут использовать SignalR](https://github.com/aspnet/Announcements/issues/305) для создания приложения Android с помощью Xamarin.Android 8.4.0.1 и более поздней версии и приложений iOS с помощью Xamarin.iOS 11.14.0.4 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="99be4-122">For example, [Xamarin developers can use SignalR](https://github.com/aspnet/Announcements/issues/305) for building Android apps using Xamarin.Android 8.4.0.1 and later and iOS apps using Xamarin.iOS 11.14.0.4 and later.</span></span>

<span data-ttu-id="99be4-123">Если сервер работает IIS, транспортный протокол WebSocket требует IIS 8.0 или более поздней версии на Windows Server 2012 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="99be4-123">If the server runs IIS, the WebSockets transport requires IIS 8.0 or later on Windows Server 2012 or later.</span></span> <span data-ttu-id="99be4-124">Другие транспорты, поддерживаются на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="99be4-124">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="99be4-125">Клиент Java</span><span class="sxs-lookup"><span data-stu-id="99be4-125">Java client</span></span>

<span data-ttu-id="99be4-126">[Клиента Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) поддерживает Java 8 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="99be4-126">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="99be4-127">Неподдерживаемый клиентов</span><span class="sxs-lookup"><span data-stu-id="99be4-127">Unsupported clients</span></span>

<span data-ttu-id="99be4-128">Следующие клиенты доступны, но экспериментальной или неофициальный.</span><span class="sxs-lookup"><span data-stu-id="99be4-128">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="99be4-129">Они уже не поддерживаются и никогда не может быть.</span><span class="sxs-lookup"><span data-stu-id="99be4-129">They aren't currently supported and may never be.</span></span>

* [<span data-ttu-id="99be4-130">Клиент C++</span><span class="sxs-lookup"><span data-stu-id="99be4-130">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="99be4-131">SWIFT клиента</span><span class="sxs-lookup"><span data-stu-id="99be4-131">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
