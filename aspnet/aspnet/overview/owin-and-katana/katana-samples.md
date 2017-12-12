---
uid: aspnet/overview/owin-and-katana/katana-samples
title: "Образцы Katana | Документы Microsoft"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 5540164bda8db31c4e78b49ecb7f7c573acca013
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="katana-samples"></a>Образцы Katana
====================
по [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Образцы Katana

**ASP.NET маршрутизирует образец** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)  
В некоторых приложениях требуется подключить компонентами OWIN в таблице маршрутов Asp.Net параллельно с компонентами не OWIN. В этом примере показано, как использовать методы расширения RouteCollection MapOwinPath и MapOwinRoute, предоставляемые Microsoft.Owin.Host.SystemWeb.

**Ветвление конвейеров образец** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)  
Конвейера обработки запросов OWIN не обязательно должны быть линейными, они могут быть разветвлен для обработки запросов по-разному. В этом примере показано, как для создания ветвления конвейера, на основе пути запроса или другие данные запроса, таких как заголовки. Следующие компоненты доступны в пакете nuget Microsoft.Owin.Mapping.

**Образец пользовательского сервера** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs)   
Показано, как использовать пользовательский сервер OWIN при размещении OWIN.

**Внедренные образец** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)  
Некоторые серверы OWIN может выполняться внутри собственного процесса (&quot;резидентной&quot;). В этом примере показано, как запуск приложения с помощью средств, предоставляемых пакетом nuget Microsoft.Owin.Hosting OWIN.

**Образец HelloWorld** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)  
OWIN — это сервер HTTP API абстракции, обеспечивающей переносимость приложения по различным серверам. В этом примере показано, как написать приложение Hello World, с помощью некоторых **простыми оболочками** вокруг необработанные абстракции OWIN и запустите его на веб-сервере, например ASP.NET.

**Hello World-пример необработанных OWIN** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)  
В этом примере показано, как разрабатывать приложения Hello World с использованием **необработанные** абстракции OWIN и запустите его на веб-сервере как Asp.Net.

**Образец SignalR** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)  
Показано, как для самостоятельного размещения SignalR с помощью OWIN и Katana. Дополнительные сведения о резидентным размещением SignalR см. в разделе [учебника: SignalR резидентной](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Статические файлы образца** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs)   
Показано, как для поддержки HTTP-запросов для статических файлов с помощью OWIN и Katana.

**Веб-API** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt)   
В этом примере показано, как разместить OWIN в службах IIS и добавить веб-API в конвейер OWIN.

**Веб-сокета образец** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs)   
Показано, как обеспечить поддержку веб-сокеты в OWIN с помощью [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket(v=vs.110).aspx) класса.
