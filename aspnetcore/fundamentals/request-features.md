---
title: "Параметры запроса в ASP.NET Core"
author: ardalis
description: "Дополнительные сведения о web server реализации подробности, связанные с HTTP-запросов и ответов, которые определены в интерфейсах для ASP.NET Core."
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d1fbd23c-2ff9-4216-b908-0201ff3afb7c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/request-features
ms.openlocfilehash: b689d82d16c6ef55485691b3474a070765c8144b
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/01/2017
---
# <a name="request-features-in-aspnet-core"></a><span data-ttu-id="03159-104">Параметры запроса в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="03159-104">Request Features in ASP.NET Core</span></span>

<span data-ttu-id="03159-105">По [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="03159-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="03159-106">Сведения о реализации server Web, связанные с HTTP-запросов и ответов определяются в интерфейсах.</span><span class="sxs-lookup"><span data-stu-id="03159-106">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="03159-107">Эти интерфейсы используются реализациями серверов и по промежуточного слоя для создания и изменения конвейера размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="03159-107">These interfaces are used by server implementations and middleware to create and modify the application's hosting pipeline.</span></span>

## <a name="feature-interfaces"></a><span data-ttu-id="03159-108">Интерфейсы компонентов</span><span class="sxs-lookup"><span data-stu-id="03159-108">Feature interfaces</span></span>

<span data-ttu-id="03159-109">ASP.NET Core определяет несколько интерфейсов компонента HTTP в `Microsoft.AspNetCore.Http.Features` которого будут использоваться серверами определить функции, которые они поддерживают.</span><span class="sxs-lookup"><span data-stu-id="03159-109">ASP.NET Core defines a number of HTTP feature interfaces in `Microsoft.AspNetCore.Http.Features` which are used by servers to identify the features they support.</span></span> <span data-ttu-id="03159-110">Следующие интерфейсы компонента обрабатывать запросы и возвращать ответы:</span><span class="sxs-lookup"><span data-stu-id="03159-110">The following feature interfaces handle requests and return responses:</span></span>

<span data-ttu-id="03159-111">`IHttpRequestFeature`Определяет структуру HTTP-запроса, включая протокол, путь, строки запроса, заголовки и текст.</span><span class="sxs-lookup"><span data-stu-id="03159-111">`IHttpRequestFeature` Defines the structure of an HTTP request, including the protocol, path, query string, headers, and body.</span></span>

<span data-ttu-id="03159-112">`IHttpResponseFeature`Определяет структуру для ответа HTTP, включая код состояния, заголовки и текст ответа.</span><span class="sxs-lookup"><span data-stu-id="03159-112">`IHttpResponseFeature` Defines the structure of an HTTP response, including the status code, headers, and body of the response.</span></span>

<span data-ttu-id="03159-113">`IHttpAuthenticationFeature`Определяет поддержку для идентификации пользователей на основе `ClaimsPrincipal` и указав обработчик проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="03159-113">`IHttpAuthenticationFeature` Defines support for identifying users based on a `ClaimsPrincipal` and specifying an authentication handler.</span></span>

<span data-ttu-id="03159-114">`IHttpUpgradeFeature`Определяет поддержку для [обновления HTTP](https://tools.ietf.org/html/rfc2616.html#section-14.42), которые позволяют клиенту указать дополнительные протоколы, он бы хотели использовать Если переключение протоколов сервера.</span><span class="sxs-lookup"><span data-stu-id="03159-114">`IHttpUpgradeFeature` Defines support for [HTTP Upgrades](https://tools.ietf.org/html/rfc2616.html#section-14.42), which allow the client to specify which additional protocols it would like to use if the server wishes to switch protocols.</span></span>

<span data-ttu-id="03159-115">`IHttpBufferingFeature`Определяет методы для отключения буферизации запросов или ответов.</span><span class="sxs-lookup"><span data-stu-id="03159-115">`IHttpBufferingFeature` Defines methods for disabling buffering of requests and/or responses.</span></span>

<span data-ttu-id="03159-116">`IHttpConnectionFeature`Определяет свойства для локальных и удаленных адресов и портов.</span><span class="sxs-lookup"><span data-stu-id="03159-116">`IHttpConnectionFeature` Defines properties for local and remote addresses and ports.</span></span>

<span data-ttu-id="03159-117">`IHttpRequestLifetimeFeature`Определяет поддержку для прерывания подключения или обнаружения, если запрос был завершен преждевременно, например как при отключении клиента.</span><span class="sxs-lookup"><span data-stu-id="03159-117">`IHttpRequestLifetimeFeature` Defines support for aborting connections, or detecting if a request has been terminated prematurely, such as by a client disconnect.</span></span>

<span data-ttu-id="03159-118">`IHttpSendFileFeature`Определяет метод для передачи файлов в асинхронном режиме.</span><span class="sxs-lookup"><span data-stu-id="03159-118">`IHttpSendFileFeature` Defines a method for sending files asynchronously.</span></span>

<span data-ttu-id="03159-119">`IHttpWebSocketFeature`Определяет интерфейс API для поддержки веб-сокеты.</span><span class="sxs-lookup"><span data-stu-id="03159-119">`IHttpWebSocketFeature` Defines an API for supporting web sockets.</span></span>

<span data-ttu-id="03159-120">`IHttpRequestIdentifierFeature`Добавляет свойство, которое может быть реализовано для уникальной идентификации запросов.</span><span class="sxs-lookup"><span data-stu-id="03159-120">`IHttpRequestIdentifierFeature` Adds a property that can be implemented to uniquely identify requests.</span></span>

<span data-ttu-id="03159-121">`ISessionFeature`Определяет `ISessionFactory` и `ISession` абстрактные классы для поддержки пользовательских сеансов.</span><span class="sxs-lookup"><span data-stu-id="03159-121">`ISessionFeature` Defines `ISessionFactory` and `ISession` abstractions for supporting user sessions.</span></span>

<span data-ttu-id="03159-122">`ITlsConnectionFeature`Определяет интерфейс API для получения сертификатов клиента.</span><span class="sxs-lookup"><span data-stu-id="03159-122">`ITlsConnectionFeature` Defines an API for retrieving client certificates.</span></span>

<span data-ttu-id="03159-123">`ITlsTokenBindingFeature`Определяет методы для работы с параметрами привязке токена TLS.</span><span class="sxs-lookup"><span data-stu-id="03159-123">`ITlsTokenBindingFeature` Defines methods for working with TLS token binding parameters.</span></span>

> [!NOTE]
> <span data-ttu-id="03159-124">`ISessionFeature`не является компонентом сервера, но реализуется `SessionMiddleware` (см. [управление состоянием приложения](app-state.md)).</span><span class="sxs-lookup"><span data-stu-id="03159-124">`ISessionFeature` is not a server feature, but is implemented by the `SessionMiddleware` (see [Managing Application State](app-state.md)).</span></span>

## <a name="feature-collections"></a><span data-ttu-id="03159-125">Функции коллекции</span><span class="sxs-lookup"><span data-stu-id="03159-125">Feature collections</span></span>

<span data-ttu-id="03159-126">`Features` Свойство `HttpContext` предоставляет интерфейс для получения и установки доступных функций HTTP для текущего запроса.</span><span class="sxs-lookup"><span data-stu-id="03159-126">The `Features` property of `HttpContext` provides an interface for getting and setting the available HTTP features for the current request.</span></span> <span data-ttu-id="03159-127">Поскольку компонент коллекции является изменяемым, даже внутри контекста запроса, по промежуточного слоя можно использовать для изменения коллекции и добавить поддержку дополнительных функций.</span><span class="sxs-lookup"><span data-stu-id="03159-127">Since the feature collection is mutable even within the context of a request, middleware can be used to modify the collection and add support for additional features.</span></span>

## <a name="middleware-and-request-features"></a><span data-ttu-id="03159-128">Возможности по промежуточного слоя и запрос</span><span class="sxs-lookup"><span data-stu-id="03159-128">Middleware and request features</span></span>

<span data-ttu-id="03159-129">Хотя серверы отвечают за создание коллекции компонентов, по промежуточного слоя можно добавить в эту коллекцию и использовать функции из коллекции.</span><span class="sxs-lookup"><span data-stu-id="03159-129">While servers are responsible for creating the feature collection, middleware can both add to this collection and consume features from the collection.</span></span> <span data-ttu-id="03159-130">Например `StaticFileMiddleware` обращается к `IHttpSendFileFeature` компонентов.</span><span class="sxs-lookup"><span data-stu-id="03159-130">For example, the `StaticFileMiddleware` accesses the `IHttpSendFileFeature` feature.</span></span> <span data-ttu-id="03159-131">Если функция существует, он используется для отправки запрашиваемого статического файла его физическому пути.</span><span class="sxs-lookup"><span data-stu-id="03159-131">If the feature exists, it is used to send the requested static file from its physical path.</span></span> <span data-ttu-id="03159-132">В противном случае медленнее альтернативный метод используется для отправки файла.</span><span class="sxs-lookup"><span data-stu-id="03159-132">Otherwise, a slower alternative method is used to send the file.</span></span> <span data-ttu-id="03159-133">Если она доступна, `IHttpSendFileFeature` позволяет операционной системе, откройте файл и выполнить копирование режим прямого ядра в сетевом адаптере.</span><span class="sxs-lookup"><span data-stu-id="03159-133">When available, the `IHttpSendFileFeature` allows the operating system to open the file and perform a direct kernel mode copy to the network card.</span></span>

<span data-ttu-id="03159-134">Кроме того по промежуточного слоя можно добавить в коллекцию компонентов, установленных на сервере.</span><span class="sxs-lookup"><span data-stu-id="03159-134">Additionally, middleware can add to the feature collection established by the server.</span></span> <span data-ttu-id="03159-135">Существующие функциональные возможности могут быть заменены даже по промежуточного слоя, позволяя по промежуточного слоя расширить функциональные возможности сервера.</span><span class="sxs-lookup"><span data-stu-id="03159-135">Existing features can even be replaced by middleware, allowing the middleware to augment the functionality of the server.</span></span> <span data-ttu-id="03159-136">Возможности, добавленные в коллекцию доступны немедленно другого по промежуточного слоя или базовой само приложение позже в конвейере.</span><span class="sxs-lookup"><span data-stu-id="03159-136">Features added to the collection are available immediately to other middleware or the underlying application itself later in the request pipeline.</span></span>

<span data-ttu-id="03159-137">Объединив реализации пользовательского сервера и улучшения конкретных по промежуточного слоя, могут создаваться точный набор компонентов, необходимых приложению.</span><span class="sxs-lookup"><span data-stu-id="03159-137">By combining custom server implementations and specific middleware enhancements, the precise set of features an application requires can be constructed.</span></span> <span data-ttu-id="03159-138">Это позволяет отсутствует функций для добавления без внесения изменений в сервер и гарантирует предоставляются только минимальные возможности, таким образом ограничивая атаки контактной зоны и повышения производительности.</span><span class="sxs-lookup"><span data-stu-id="03159-138">This allows missing features to be added without requiring a change in server, and ensures only the minimal amount of features are exposed, thus limiting attack surface area and improving performance.</span></span>

## <a name="summary"></a><span data-ttu-id="03159-139">Сводка</span><span class="sxs-lookup"><span data-stu-id="03159-139">Summary</span></span>

<span data-ttu-id="03159-140">Интерфейсы компонентов определяют конкретные возможности HTTP может поддерживать данного запроса.</span><span class="sxs-lookup"><span data-stu-id="03159-140">Feature interfaces define specific HTTP features that a given request may support.</span></span> <span data-ttu-id="03159-141">Серверы определить наборы функций и первоначального набора возможностей, поддерживаемых этим сервером, но по промежуточного слоя можно использовать для улучшения этих средств.</span><span class="sxs-lookup"><span data-stu-id="03159-141">Servers define collections of features, and the initial set of features supported by that server, but middleware can be used to enhance these features.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="03159-142">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="03159-142">Additional Resources</span></span>

* [<span data-ttu-id="03159-143">Серверы</span><span class="sxs-lookup"><span data-stu-id="03159-143">Servers</span></span>](servers/index.md)

* [<span data-ttu-id="03159-144">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="03159-144">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="03159-145">Открытый веб-интерфейс для .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="03159-145">Open Web Interface for .NET (OWIN)</span></span>](owin.md)
