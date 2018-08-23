---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Примеры Katana | Документация Майкрософт
author: microsoft
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: b8ce2b40a19e0429f1ccedb03b8f829582652d24
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829080"
---
<a name="katana-samples"></a>Примеры Katana
====================
по [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Примеры Katana

**ASP.NET направляет образец** | [исходный код](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)  
В некоторых приложениях требуется подключить компоненты OWIN в таблице маршрутов Asp.Net параллельно с компонентами не OWIN. В этом примере показано, как использовать методы расширения RouteCollection MapOwinPath и MapOwinRoute, предоставляемые Microsoft.Owin.Host.SystemWeb.

**Ветвление конвейеров образец** | [исходный код](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)  
Конвейера обработки запросов OWIN не обязательно должны быть линейными, они могут быть разветвлены для обработки запросов по-разному. В этом примере показано, как для формирования ветвления конвейера, на основе путей запросов или другие данные запроса, такие как заголовки. Эти компоненты доступны в пакете nuget Microsoft.Owin.Mapping.

**Пример пользовательского сервера** | [исходный код](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer)   
Показано, как использовать пользовательский сервер OWIN при саморазмещением OWIN.

**Образец Embedded** | [исходный код](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)  
Некоторые серверы OWIN может выполняться внутри собственного процесса (&quot;резидентным&quot;). В этом примере показано, как запустить приложение OWIN с помощью средств, предоставляемых пакетом nuget Microsoft.Owin.Hosting.

**Образец HelloWorld** | [исходный код](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)  
OWIN — это сервер HTTP API абстракции, который обеспечивает возможность переноса приложений на различных серверах. В этом примере показано, как создать приложение Hello World с помощью некоторых **простыми оболочками** вокруг необработанные абстракции OWIN и запустите его на веб-сервере, такие как ASP.NET.

**Пример Hello World необработанные OWIN** | [исходный код](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)  
В этом примере показано, как писать приложения Hello World с помощью **необработанные** абстракции OWIN и запустите его на веб-сервере, такие как Asp.Net.

**Пример SignalR** | [исходный код](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)  
Показано, как Резидентное размещение SignalR с помощью OWIN и Katana. Дополнительные сведения о SignalR размещения на собственном сервере, см. в разделе [учебник: Резидентное размещение SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Статические файлы образца** | [исходный код](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample)   
Показано, как для поддержки HTTP-запросы для статических файлов с помощью OWIN и Katana.

**Веб-API** | [исходный код](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi)   
В этом примере показано, как размещение OWIN в службах IIS и добавление веб-API в конвейер OWIN.

**Веб-сокета образец** | [исходный код](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample)   
Показано, как обеспечить поддержку веб-сокеты в OWIN с помощью [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) класса.
