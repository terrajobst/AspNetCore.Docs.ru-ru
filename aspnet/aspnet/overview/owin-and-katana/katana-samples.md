---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Примеры Katana | Документация Майкрософт
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: e81da1e650d8dfd24a3e0fda6aa42b7f360ce12d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393323"
---
<a name="katana-samples"></a>Примеры Katana
====================
по [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Примеры Katana

**ASP.NET направляет образец** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)  
В некоторых приложениях требуется подключить компоненты OWIN в таблице маршрутов Asp.Net параллельно с компонентами не OWIN. В этом примере показано, как использовать методы расширения RouteCollection MapOwinPath и MapOwinRoute, предоставляемые Microsoft.Owin.Host.SystemWeb.

**Ветвление конвейеров образец** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)  
Конвейера обработки запросов OWIN не обязательно должны быть линейными, они могут быть разветвлены для обработки запросов по-разному. В этом примере показано, как для формирования ветвления конвейера, на основе путей запросов или другие данные запроса, такие как заголовки. Эти компоненты доступны в пакете nuget Microsoft.Owin.Mapping.

**Пример пользовательского сервера** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs)   
Показано, как использовать пользовательский сервер OWIN при саморазмещением OWIN.

**Образец Embedded** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)  
Некоторые серверы OWIN может выполняться внутри собственного процесса (&quot;резидентным&quot;). В этом примере показано, как запустить приложение OWIN с помощью средств, предоставляемых пакетом nuget Microsoft.Owin.Hosting.

**Образец HelloWorld** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)  
OWIN — это сервер HTTP API абстракции, который обеспечивает возможность переноса приложений на различных серверах. В этом примере показано, как создать приложение Hello World с помощью некоторых **простыми оболочками** вокруг необработанные абстракции OWIN и запустите его на веб-сервере, такие как ASP.NET.

**Пример Hello World необработанные OWIN** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)  
В этом примере показано, как писать приложения Hello World с помощью **необработанные** абстракции OWIN и запустите его на веб-сервере, такие как Asp.Net.

**Пример SignalR** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)  
Показано, как Резидентное размещение SignalR с помощью OWIN и Katana. Дополнительные сведения о SignalR размещения на собственном сервере, см. в разделе [учебник: Резидентное размещение SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Статические файлы образца** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs)   
Показано, как для поддержки HTTP-запросы для статических файлов с помощью OWIN и Katana.

**Веб-API** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt)   
В этом примере показано, как размещение OWIN в службах IIS и добавление веб-API в конвейер OWIN.

**Веб-сокета образец** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs)   
Показано, как обеспечить поддержку веб-сокеты в OWIN с помощью [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) класса.
