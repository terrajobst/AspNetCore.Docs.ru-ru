---
uid: web-api/overview/advanced/httpclient-message-handlers
title: "Обработчики сообщений HttpClient веб-API ASP.NET | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2012
ms.topic: article
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 805741b0ac682b7479ce82127df48b1b9a49a427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a>Обработчики сообщений HttpClient веб-API ASP.NET
====================
по [Mike Wasson](https://github.com/MikeWasson)

Объект *обработчик сообщений* является классом, который получает HTTP-запрос и возвращает ответ HTTP.

Как правило ряд обработчики сообщений соединяются друг с другом. Первый обработчик получает запрос HTTP, выполняет некоторую обработку и дает в обработчик следующий запрос. На некотором этапе ответ создается и возвращается в цепочке. Эта модель называется *делегирование* обработчика.

![](httpclient-message-handlers/_static/image1.png)

На стороне клиента **HttpClient** класс использует обработчик сообщений для обработки запросов. Обработчик по умолчанию является **HttpClientHandler**, который отправляет запрос по сети и получает ответ от сервера. Можно вставить обработчики пользовательских сообщений в конвейер клиента:

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> Веб-API ASP.NET также использует обработчики сообщений на стороне сервера. Дополнительные сведения см. в разделе [обработчиков сообщений HTTP](http-message-handlers.md).


## <a name="custom-message-handlers"></a>Обработчики пользовательских сообщений

Чтобы написать обработчик пользовательского сообщения, являются производными от **System.Net.Http.DelegatingHandler** и Переопределите **SendAsync** метод. Ниже представлена подпись метода:

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

Этот метод принимает **HttpRequestMessage** как входные данные и асинхронно возвращает **HttpResponseMessage**. Типичная реализация выполняет следующие задачи.

1. Обрабатывает сообщение запроса.
2. Вызовите `base.SendAsync` отправлять запрос внутреннему обработчику.
3. Внутренний обработчик возвращает ответное сообщение. (Этот шаг является асинхронной.)
4. Обработать ответ и возвращается вызывающему объекту.

В следующем примере обработчик сообщений, который необходимо добавить пользовательский заголовок в исходящий запрос:

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

Вызов `base.SendAsync` является асинхронным. Если обработчик не выполняет никаких действий после этого вызова, используйте **await** ключевое слово, чтобы возобновить выполнение после завершения метода. Следующий пример показывает обработчик, который регистрирует коды ошибок. Ведение журнала, сам не представляют интереса, но в примере показано, как получать в ответ внутри обработчика.

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>Добавление обработчиков сообщений в конвейер клиента

Чтобы добавить пользовательские обработчики для **HttpClient**, используйте **HttpClientFactory.Create** метод:

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

Обработчики сообщений вызываются в порядке, в котором можно передать их в **создать** метод. Поскольку обработчики являются вложенными, ответное сообщение перемещается в обратном направлении. Последним обработчиком является первым получает ответное сообщение.
