---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Вызов веб-API из клиента .NET (C#) | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/24/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: fdb74b0eb74ce7f387f49a0b25ceebd3fc389da9
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/26/2018
ms.locfileid: "32006161"
---
<a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="a5220-102">Вызов веб-API из клиента .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="a5220-102">Call a Web API From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="a5220-103">по [Mike Wasson](https://github.com/MikeWasson) и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a5220-103">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a5220-104">[Загрузка завершенного проекта](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span><span class="sxs-lookup"><span data-stu-id="a5220-104">[Download Completed Project](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span></span> <span data-ttu-id="a5220-105">[Указания по скачиванию](/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="a5220-105">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> 

<span data-ttu-id="a5220-106">Этого учебника показано, как вызывать из приложения .NET, веб-API с помощью [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="a5220-106">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="a5220-107">В этом учебнике клиентском приложении записываются, которые используют следующие веб-API:</span><span class="sxs-lookup"><span data-stu-id="a5220-107">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="a5220-108">Действие</span><span class="sxs-lookup"><span data-stu-id="a5220-108">Action</span></span> | <span data-ttu-id="a5220-109">Метод HTTP</span><span class="sxs-lookup"><span data-stu-id="a5220-109">HTTP method</span></span> | <span data-ttu-id="a5220-110">Относительный URI</span><span class="sxs-lookup"><span data-stu-id="a5220-110">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a5220-111">Получение продукта по Идентификатору</span><span class="sxs-lookup"><span data-stu-id="a5220-111">Get a product by ID</span></span> | <span data-ttu-id="a5220-112">GET</span><span class="sxs-lookup"><span data-stu-id="a5220-112">GET</span></span> | <span data-ttu-id="a5220-113">/API/продукты/*идентификатор*</span><span class="sxs-lookup"><span data-stu-id="a5220-113">/api/products/*id*</span></span> |
| <span data-ttu-id="a5220-114">Создать продукт</span><span class="sxs-lookup"><span data-stu-id="a5220-114">Create a new product</span></span> | <span data-ttu-id="a5220-115">ПОМЕСТИТЬ</span><span class="sxs-lookup"><span data-stu-id="a5220-115">POST</span></span> | <span data-ttu-id="a5220-116">/ api/продуктов</span><span class="sxs-lookup"><span data-stu-id="a5220-116">/api/products</span></span> |
| <span data-ttu-id="a5220-117">Обновления продукта</span><span class="sxs-lookup"><span data-stu-id="a5220-117">Update a product</span></span> | <span data-ttu-id="a5220-118">PUT</span><span class="sxs-lookup"><span data-stu-id="a5220-118">PUT</span></span> | <span data-ttu-id="a5220-119">/API/продукты/*идентификатор*</span><span class="sxs-lookup"><span data-stu-id="a5220-119">/api/products/*id*</span></span> |
| <span data-ttu-id="a5220-120">Удаление продукта</span><span class="sxs-lookup"><span data-stu-id="a5220-120">Delete a product</span></span> | <span data-ttu-id="a5220-121">DELETE</span><span class="sxs-lookup"><span data-stu-id="a5220-121">DELETE</span></span> | <span data-ttu-id="a5220-122">/API/продукты/*идентификатор*</span><span class="sxs-lookup"><span data-stu-id="a5220-122">/api/products/*id*</span></span> |

<span data-ttu-id="a5220-123">Чтобы узнать, как реализовать этот интерфейс API с веб-API ASP.NET, в разделе [Создание веб-API, поддерживает операции CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span><span class="sxs-lookup"><span data-stu-id="a5220-123">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="a5220-124">Для простоты клиентское приложение в этом учебнике используется консольное приложение Windows.</span><span class="sxs-lookup"><span data-stu-id="a5220-124">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="a5220-125">**HttpClient** также поддерживается для приложений Windows Phone и магазина Windows.</span><span class="sxs-lookup"><span data-stu-id="a5220-125">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="a5220-126">Дополнительные сведения см. в разделе [код записи веб-API клиента для нескольких платформ с помощью переносимой библиотеки](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="a5220-126">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="a5220-127">Создайте консольное приложение</span><span class="sxs-lookup"><span data-stu-id="a5220-127">Create the Console Application</span></span>

<span data-ttu-id="a5220-128">В Visual Studio создайте новое консольное приложение Windows с именем **HttpClientSample** и вставьте следующий код:</span><span class="sxs-lookup"><span data-stu-id="a5220-128">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="a5220-129">Приведенный выше код является полный клиентского приложения.</span><span class="sxs-lookup"><span data-stu-id="a5220-129">The preceding code is the complete client app.</span></span>

<span data-ttu-id="a5220-130">`RunAsync` Запуск и блокируется до ее завершения.</span><span class="sxs-lookup"><span data-stu-id="a5220-130">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="a5220-131">Большинство **HttpClient** методы являются async, поскольку они выполняют сетевых операций ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="a5220-131">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="a5220-132">Все асинхронные задачи выполняемые в `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="a5220-132">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="a5220-133">Обычно приложения не блокирует основной поток, но это приложение не допускает каких-либо действий.</span><span class="sxs-lookup"><span data-stu-id="a5220-133">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="a5220-134">Установите клиентские API библиотеки Web</span><span class="sxs-lookup"><span data-stu-id="a5220-134">Install the Web API Client Libraries</span></span>

<span data-ttu-id="a5220-135">Используйте диспетчер пакетов NuGet для установки пакета Web API Client Libraries.</span><span class="sxs-lookup"><span data-stu-id="a5220-135">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="a5220-136">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="a5220-136">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="a5220-137">В пакет Диспетчер консоли (PMC), введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="a5220-137">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="a5220-138">Эта команда добавляет следующие пакеты NuGet в проект:</span><span class="sxs-lookup"><span data-stu-id="a5220-138">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="a5220-139">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="a5220-139">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="a5220-140">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="a5220-140">Newtonsoft.Json</span></span>

<span data-ttu-id="a5220-141">Json.NET — это популярный платформа JSON высокой производительности для .NET.</span><span class="sxs-lookup"><span data-stu-id="a5220-141">Json.NET is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="a5220-142">Добавьте класс модели</span><span class="sxs-lookup"><span data-stu-id="a5220-142">Add a Model Class</span></span>

<span data-ttu-id="a5220-143">Изучите `Product` класса:</span><span class="sxs-lookup"><span data-stu-id="a5220-143">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="a5220-144">Этот класс соответствует модели данных, используемых веб-API.</span><span class="sxs-lookup"><span data-stu-id="a5220-144">This class matches the data model used by the web API.</span></span> <span data-ttu-id="a5220-145">Можно использовать приложение **HttpClient** для чтения `Product` экземпляр HTTP-ответа.</span><span class="sxs-lookup"><span data-stu-id="a5220-145">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="a5220-146">Приложения не нужно писать никакой код для десериализации.</span><span class="sxs-lookup"><span data-stu-id="a5220-146">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="a5220-147">Создание и инициализация HttpClient</span><span class="sxs-lookup"><span data-stu-id="a5220-147">Create and Initialize HttpClient</span></span>

<span data-ttu-id="a5220-148">Проверки статических **HttpClient** свойства:</span><span class="sxs-lookup"><span data-stu-id="a5220-148">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="a5220-149">**HttpClient** предназначен для создания экземпляров один раз и использовать повторно в течение жизненного цикла приложения.</span><span class="sxs-lookup"><span data-stu-id="a5220-149">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="a5220-150">Следующие причины могут приводить к **SocketException** ошибки:</span><span class="sxs-lookup"><span data-stu-id="a5220-150">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="a5220-151">Создание нового **HttpClient** экземпляра на запрос.</span><span class="sxs-lookup"><span data-stu-id="a5220-151">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="a5220-152">Сервер максимально загружен.</span><span class="sxs-lookup"><span data-stu-id="a5220-152">Server under heavy load.</span></span>

<span data-ttu-id="a5220-153">Создание нового **HttpClient** экземпляра на запрос может использовать доступные сокеты.</span><span class="sxs-lookup"><span data-stu-id="a5220-153">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="a5220-154">Следующий код инициализирует **HttpClient** экземпляр:</span><span class="sxs-lookup"><span data-stu-id="a5220-154">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="a5220-155">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="a5220-155">The preceding code:</span></span>

* <span data-ttu-id="a5220-156">Задает базовый URI для HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="a5220-156">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="a5220-157">Измените номер порта к порту, используемому в приложении сервера.</span><span class="sxs-lookup"><span data-stu-id="a5220-157">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="a5220-158">Приложение не будет работать, если не используется порт для серверного приложения.</span><span class="sxs-lookup"><span data-stu-id="a5220-158">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="a5220-159">Задает заголовок Accept для «application/json».</span><span class="sxs-lookup"><span data-stu-id="a5220-159">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="a5220-160">Установка этот заголовок указывает, что сервер для отправки данных в формате JSON.</span><span class="sxs-lookup"><span data-stu-id="a5220-160">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="a5220-161">Отправка запроса GET для извлечения ресурса</span><span class="sxs-lookup"><span data-stu-id="a5220-161">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="a5220-162">Следующий код отправляет запрос GET для продукта:</span><span class="sxs-lookup"><span data-stu-id="a5220-162">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="a5220-163">**GetAsync** метод отправляет запрос HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="a5220-163">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="a5220-164">Если метод завершается, она возвращает **HttpResponseMessage** , содержащий HTTP-ответа.</span><span class="sxs-lookup"><span data-stu-id="a5220-164">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="a5220-165">Если код состояния в ответе код успешного завершения, текст ответа содержит представление JSON продукта.</span><span class="sxs-lookup"><span data-stu-id="a5220-165">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="a5220-166">Вызовите **ReadAsAsync** для полезных данных JSON для десериализации `Product` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="a5220-166">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="a5220-167">**ReadAsAsync** метод является асинхронным, поскольку текст ответа может быть произвольно большое.</span><span class="sxs-lookup"><span data-stu-id="a5220-167">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="a5220-168">**HttpClient** не вызывает исключение, если HTTP-ответ содержит код ошибки.</span><span class="sxs-lookup"><span data-stu-id="a5220-168">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="a5220-169">Вместо этого **IsSuccessStatusCode** свойство **false** Если состояние представляет собой код ошибки.</span><span class="sxs-lookup"><span data-stu-id="a5220-169">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="a5220-170">Если вы предпочитаете обрабатывать коды ошибок HTTP как исключения, вызовите [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) в объекте отклика.</span><span class="sxs-lookup"><span data-stu-id="a5220-170">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="a5220-171">`EnsureSuccessStatusCode` вызывает исключение, если код состояния находится вне диапазона 200&ndash;299.</span><span class="sxs-lookup"><span data-stu-id="a5220-171">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="a5220-172">Обратите внимание, что **HttpClient** может создавать исключения по другим причинам &mdash; к примеру, при истечении времени ожидания запроса.</span><span class="sxs-lookup"><span data-stu-id="a5220-172">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="a5220-173">Модули форматирования типа мультимедиа для десериализации</span><span class="sxs-lookup"><span data-stu-id="a5220-173">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="a5220-174">Когда **ReadAsAsync** вызывается без параметров, она использует набор по умолчанию *модули форматирования мультимедиа* для чтения текста ответа.</span><span class="sxs-lookup"><span data-stu-id="a5220-174">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="a5220-175">Модули форматирования по умолчанию поддерживает JSON, XML и данные в кодировке форма URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="a5220-175">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="a5220-176">Вместо использования модулей форматирования по умолчанию, необходимо предоставить список модулей форматирования для **ReadAsAsync** метод.</span><span class="sxs-lookup"><span data-stu-id="a5220-176">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="a5220-177">Список модулей форматирования удобно использовать при наличии пользовательский модуль форматирования мультимедиа:</span><span class="sxs-lookup"><span data-stu-id="a5220-177">Using a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="a5220-178">Дополнительные сведения см. в разделе [модулей форматирования мультимедиа в ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="a5220-178">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="a5220-179">Отправка запроса POST для создания ресурса</span><span class="sxs-lookup"><span data-stu-id="a5220-179">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="a5220-180">Следующий код отправляет запрос POST, содержащий `Product` экземпляра в формате JSON:</span><span class="sxs-lookup"><span data-stu-id="a5220-180">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="a5220-181">**PostAsJsonAsync** метод:</span><span class="sxs-lookup"><span data-stu-id="a5220-181">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="a5220-182">Сериализует объект в JSON.</span><span class="sxs-lookup"><span data-stu-id="a5220-182">Serializes an object to JSON.</span></span>
* <span data-ttu-id="a5220-183">Отправляет полезные данные JSON в запросе POST.</span><span class="sxs-lookup"><span data-stu-id="a5220-183">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="a5220-184">Если запрос выполняется успешно:</span><span class="sxs-lookup"><span data-stu-id="a5220-184">If the request succeeds:</span></span>

* <span data-ttu-id="a5220-185">Она должна возвращать ответа 201 (создано).</span><span class="sxs-lookup"><span data-stu-id="a5220-185">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="a5220-186">Ответ должен содержать созданные ресурсы URL-адрес в заголовке Location.</span><span class="sxs-lookup"><span data-stu-id="a5220-186">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="a5220-187">Отправка запроса PUT для обновления ресурса</span><span class="sxs-lookup"><span data-stu-id="a5220-187">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="a5220-188">Следующий код отправляет запрос PUT для обновления продукта:</span><span class="sxs-lookup"><span data-stu-id="a5220-188">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="a5220-189">**PutAsJsonAsync** действия метода **PostAsJsonAsync**, за исключением того, что он отправляет запрос PUT вместо POST.</span><span class="sxs-lookup"><span data-stu-id="a5220-189">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="a5220-190">Отправка запроса DELETE, чтобы удалить ресурс</span><span class="sxs-lookup"><span data-stu-id="a5220-190">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="a5220-191">Следующий код отправляет запрос DELETE для удаления продукта:</span><span class="sxs-lookup"><span data-stu-id="a5220-191">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="a5220-192">Как GET запрос DELETE не имеет текста запроса.</span><span class="sxs-lookup"><span data-stu-id="a5220-192">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="a5220-193">Не нужно указать формат JSON или XML с "Удалить".</span><span class="sxs-lookup"><span data-stu-id="a5220-193">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="a5220-194">Тестирование образца</span><span class="sxs-lookup"><span data-stu-id="a5220-194">Test the sample</span></span>

<span data-ttu-id="a5220-195">Для проверки клиентского приложения:</span><span class="sxs-lookup"><span data-stu-id="a5220-195">To test the client app:</span></span>

1. <span data-ttu-id="a5220-196">[Загрузить](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) и запустить приложение сервера.</span><span class="sxs-lookup"><span data-stu-id="a5220-196">[Download](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) and run the server app.</span></span> <span data-ttu-id="a5220-197">[Указания по скачиванию](/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="a5220-197">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> <span data-ttu-id="a5220-198">Убедитесь, что работа приложения сервера.</span><span class="sxs-lookup"><span data-stu-id="a5220-198">Verify the server app is working.</span></span> <span data-ttu-id="a5220-199">Для exaxmple `http://localhost:64195/api/products` должен возвращать список продуктов.</span><span class="sxs-lookup"><span data-stu-id="a5220-199">For exaxmple, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="a5220-200">Задайте базовый универсальный код Ресурса для HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="a5220-200">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="a5220-201">Измените номер порта к порту, используемому в приложении сервера.</span><span class="sxs-lookup"><span data-stu-id="a5220-201">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="a5220-202">Запустите клиентское приложение.</span><span class="sxs-lookup"><span data-stu-id="a5220-202">Run the client app.</span></span> <span data-ttu-id="a5220-203">Выводятся следующие результаты.</span><span class="sxs-lookup"><span data-stu-id="a5220-203">The following output is produced:</span></span>

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
