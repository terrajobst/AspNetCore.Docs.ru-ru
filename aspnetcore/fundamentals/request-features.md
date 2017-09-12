---
title: "Параметры запроса в ASP.NET Core"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d1fbd23c-2ff9-4216-b908-0201ff3afb7c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/request-features
ms.openlocfilehash: a10aefe3819fb03019575c36274dd164faf7086c
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="request-features-in-aspnet-core"></a><span data-ttu-id="ec3c8-103">Параметры запроса в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ec3c8-103">Request Features in ASP.NET Core</span></span>

<span data-ttu-id="ec3c8-104">По [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ec3c8-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ec3c8-105">Сведения о реализации server Web, связанные с HTTP-запросов и ответов определяются в интерфейсах.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-105">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="ec3c8-106">Эти интерфейсы используются реализациями серверов и по промежуточного слоя для создания и изменения конвейера размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-106">These interfaces are used by server implementations and middleware to create and modify the application's hosting pipeline.</span></span>

## <a name="feature-interfaces"></a><span data-ttu-id="ec3c8-107">Интерфейсы компонентов</span><span class="sxs-lookup"><span data-stu-id="ec3c8-107">Feature interfaces</span></span>

<span data-ttu-id="ec3c8-108">ASP.NET Core определяет несколько интерфейсов компонента HTTP в `Microsoft.AspNetCore.Http.Features` которого будут использоваться серверами определить функции, которые они поддерживают.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-108">ASP.NET Core defines a number of HTTP feature interfaces in `Microsoft.AspNetCore.Http.Features` which are used by servers to identify the features they support.</span></span> <span data-ttu-id="ec3c8-109">Следующие интерфейсы компонента обрабатывать запросы и возвращать ответы:</span><span class="sxs-lookup"><span data-stu-id="ec3c8-109">The following feature interfaces handle requests and return responses:</span></span>

<span data-ttu-id="ec3c8-110">`IHttpRequestFeature`Определяет структуру HTTP-запроса, включая протокол, путь, строки запроса, заголовки и текст.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-110">`IHttpRequestFeature` Defines the structure of an HTTP request, including the protocol, path, query string, headers, and body.</span></span>

<span data-ttu-id="ec3c8-111">`IHttpResponseFeature`Определяет структуру для ответа HTTP, включая код состояния, заголовки и текст ответа.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-111">`IHttpResponseFeature` Defines the structure of an HTTP response, including the status code, headers, and body of the response.</span></span>

<span data-ttu-id="ec3c8-112">`IHttpAuthenticationFeature`Определяет поддержку для идентификации пользователей на основе `ClaimsPrincipal` и указав обработчик проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-112">`IHttpAuthenticationFeature` Defines support for identifying users based on a `ClaimsPrincipal` and specifying an authentication handler.</span></span>

<span data-ttu-id="ec3c8-113">`IHttpUpgradeFeature`Определяет поддержку для [обновления HTTP](https://tools.ietf.org/html/rfc2616.html#section-14.42), которые позволяют клиенту указать дополнительные протоколы, он бы хотели использовать Если переключение протоколов сервера.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-113">`IHttpUpgradeFeature` Defines support for [HTTP Upgrades](https://tools.ietf.org/html/rfc2616.html#section-14.42), which allow the client to specify which additional protocols it would like to use if the server wishes to switch protocols.</span></span>

<span data-ttu-id="ec3c8-114">`IHttpBufferingFeature`Определяет методы для отключения буферизации запросов или ответов.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-114">`IHttpBufferingFeature` Defines methods for disabling buffering of requests and/or responses.</span></span>

<span data-ttu-id="ec3c8-115">`IHttpConnectionFeature`Определяет свойства для локальных и удаленных адресов и портов.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-115">`IHttpConnectionFeature` Defines properties for local and remote addresses and ports.</span></span>

<span data-ttu-id="ec3c8-116">`IHttpRequestLifetimeFeature`Определяет поддержку для прерывания подключения или обнаружения, если запрос был завершен преждевременно, например как при отключении клиента.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-116">`IHttpRequestLifetimeFeature` Defines support for aborting connections, or detecting if a request has been terminated prematurely, such as by a client disconnect.</span></span>

<span data-ttu-id="ec3c8-117">`IHttpSendFileFeature`Определяет метод для передачи файлов в асинхронном режиме.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-117">`IHttpSendFileFeature` Defines a method for sending files asynchronously.</span></span>

<span data-ttu-id="ec3c8-118">`IHttpWebSocketFeature`Определяет интерфейс API для поддержки веб-сокеты.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-118">`IHttpWebSocketFeature` Defines an API for supporting web sockets.</span></span>

<span data-ttu-id="ec3c8-119">`IHttpRequestIdentifierFeature`Добавляет свойство, которое может быть реализовано для уникальной идентификации запросов.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-119">`IHttpRequestIdentifierFeature` Adds a property that can be implemented to uniquely identify requests.</span></span>

<span data-ttu-id="ec3c8-120">`ISessionFeature`Определяет `ISessionFactory` и `ISession` абстрактные классы для поддержки пользовательских сеансов.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-120">`ISessionFeature` Defines `ISessionFactory` and `ISession` abstractions for supporting user sessions.</span></span>

<span data-ttu-id="ec3c8-121">`ITlsConnectionFeature`Определяет интерфейс API для получения сертификатов клиента.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-121">`ITlsConnectionFeature` Defines an API for retrieving client certificates.</span></span>

<span data-ttu-id="ec3c8-122">`ITlsTokenBindingFeature`Определяет методы для работы с параметрами привязке токена TLS.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-122">`ITlsTokenBindingFeature` Defines methods for working with TLS token binding parameters.</span></span>

> [!NOTE]
> <span data-ttu-id="ec3c8-123">`ISessionFeature`не является компонентом сервера, но реализуется `SessionMiddleware` (см. [управление состоянием приложения](app-state.md)).</span><span class="sxs-lookup"><span data-stu-id="ec3c8-123">`ISessionFeature` is not a server feature, but is implemented by the `SessionMiddleware` (see [Managing Application State](app-state.md)).</span></span>

## <a name="feature-collections"></a><span data-ttu-id="ec3c8-124">Функции коллекции</span><span class="sxs-lookup"><span data-stu-id="ec3c8-124">Feature collections</span></span>

<span data-ttu-id="ec3c8-125">`Features` Свойство `HttpContext` предоставляет интерфейс для получения и установки доступных функций HTTP для текущего запроса.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-125">The `Features` property of `HttpContext` provides an interface for getting and setting the available HTTP features for the current request.</span></span> <span data-ttu-id="ec3c8-126">Поскольку компонент коллекции является изменяемым, даже внутри контекста запроса, по промежуточного слоя можно использовать для изменения коллекции и добавить поддержку дополнительных функций.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-126">Since the feature collection is mutable even within the context of a request, middleware can be used to modify the collection and add support for additional features.</span></span>

## <a name="middleware-and-request-features"></a><span data-ttu-id="ec3c8-127">Возможности по промежуточного слоя и запрос</span><span class="sxs-lookup"><span data-stu-id="ec3c8-127">Middleware and request features</span></span>

<span data-ttu-id="ec3c8-128">Хотя серверы отвечают за создание коллекции компонентов, по промежуточного слоя можно добавить в эту коллекцию и использовать функции из коллекции.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-128">While servers are responsible for creating the feature collection, middleware can both add to this collection and consume features from the collection.</span></span> <span data-ttu-id="ec3c8-129">Например `StaticFileMiddleware` обращается к `IHttpSendFileFeature` компонентов.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-129">For example, the `StaticFileMiddleware` accesses the `IHttpSendFileFeature` feature.</span></span> <span data-ttu-id="ec3c8-130">Если функция существует, он используется для отправки запрашиваемого статического файла его физическому пути.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-130">If the feature exists, it is used to send the requested static file from its physical path.</span></span> <span data-ttu-id="ec3c8-131">В противном случае медленнее альтернативный метод используется для отправки файла.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-131">Otherwise, a slower alternative method is used to send the file.</span></span> <span data-ttu-id="ec3c8-132">Если она доступна, `IHttpSendFileFeature` позволяет операционной системе, откройте файл и выполнить копирование режим прямого ядра в сетевом адаптере.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-132">When available, the `IHttpSendFileFeature` allows the operating system to open the file and perform a direct kernel mode copy to the network card.</span></span>

<span data-ttu-id="ec3c8-133">Кроме того по промежуточного слоя можно добавить в коллекцию компонентов, установленных на сервере.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-133">Additionally, middleware can add to the feature collection established by the server.</span></span> <span data-ttu-id="ec3c8-134">Существующие функциональные возможности могут быть заменены даже по промежуточного слоя, позволяя по промежуточного слоя расширить функциональные возможности сервера.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-134">Existing features can even be replaced by middleware, allowing the middleware to augment the functionality of the server.</span></span> <span data-ttu-id="ec3c8-135">Возможности, добавленные в коллекцию доступны немедленно другого по промежуточного слоя или базовой само приложение позже в конвейере.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-135">Features added to the collection are available immediately to other middleware or the underlying application itself later in the request pipeline.</span></span>

<span data-ttu-id="ec3c8-136">Объединив реализации пользовательского сервера и улучшения конкретных по промежуточного слоя, могут создаваться точный набор компонентов, необходимых приложению.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-136">By combining custom server implementations and specific middleware enhancements, the precise set of features an application requires can be constructed.</span></span> <span data-ttu-id="ec3c8-137">Это позволяет отсутствует функций для добавления без внесения изменений в сервер и гарантирует предоставляются только минимальные возможности, таким образом ограничивая атаки контактной зоны и повышения производительности.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-137">This allows missing features to be added without requiring a change in server, and ensures only the minimal amount of features are exposed, thus limiting attack surface area and improving performance.</span></span>

## <a name="summary"></a><span data-ttu-id="ec3c8-138">Сводка</span><span class="sxs-lookup"><span data-stu-id="ec3c8-138">Summary</span></span>

<span data-ttu-id="ec3c8-139">Интерфейсы компонентов определяют конкретные возможности HTTP может поддерживать данного запроса.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-139">Feature interfaces define specific HTTP features that a given request may support.</span></span> <span data-ttu-id="ec3c8-140">Серверы определить наборы функций и первоначального набора возможностей, поддерживаемых этим сервером, но по промежуточного слоя можно использовать для улучшения этих средств.</span><span class="sxs-lookup"><span data-stu-id="ec3c8-140">Servers define collections of features, and the initial set of features supported by that server, but middleware can be used to enhance these features.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ec3c8-141">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ec3c8-141">Additional Resources</span></span>

* [<span data-ttu-id="ec3c8-142">Серверы</span><span class="sxs-lookup"><span data-stu-id="ec3c8-142">Servers</span></span>](servers/index.md)

* [<span data-ttu-id="ec3c8-143">По промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="ec3c8-143">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="ec3c8-144">Открыть веб-интерфейс для .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="ec3c8-144">Open Web Interface for .NET (OWIN)</span></span>](owin.md)
