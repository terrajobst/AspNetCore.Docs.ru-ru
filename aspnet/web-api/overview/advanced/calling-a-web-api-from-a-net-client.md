---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Вызов веб-API из клиента .NET (C#) | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 11/24/2017
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 6b034642f1616a9d5c6495b65d4adeeae9a6f2c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821395"
---
<a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="d9c08-102">Вызов веб-API из клиента .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="d9c08-102">Call a Web API From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="d9c08-103">по [Майк Уоссон](https://github.com/MikeWasson) и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d9c08-103">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d9c08-104">[Скачать завершенный проект](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span><span class="sxs-lookup"><span data-stu-id="d9c08-104">[Download Completed Project](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span></span> <span data-ttu-id="d9c08-105">[Указания по скачиванию](/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="d9c08-105">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> 

<span data-ttu-id="d9c08-106">Этом руководстве показано, как вызывать веб-API из приложения .NET с помощью [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="d9c08-106">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="d9c08-107">В этом руководстве клиентского приложения записываются, которые используют следующие веб-API:</span><span class="sxs-lookup"><span data-stu-id="d9c08-107">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="d9c08-108">Действие</span><span class="sxs-lookup"><span data-stu-id="d9c08-108">Action</span></span> | <span data-ttu-id="d9c08-109">Метод HTTP</span><span class="sxs-lookup"><span data-stu-id="d9c08-109">HTTP method</span></span> | <span data-ttu-id="d9c08-110">Относительный URI</span><span class="sxs-lookup"><span data-stu-id="d9c08-110">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d9c08-111">Получить продукт по Идентификатору</span><span class="sxs-lookup"><span data-stu-id="d9c08-111">Get a product by ID</span></span> | <span data-ttu-id="d9c08-112">GET</span><span class="sxs-lookup"><span data-stu-id="d9c08-112">GET</span></span> | <span data-ttu-id="d9c08-113">/API/продукты/*идентификатор*</span><span class="sxs-lookup"><span data-stu-id="d9c08-113">/api/products/*id*</span></span> |
| <span data-ttu-id="d9c08-114">Создать продукт</span><span class="sxs-lookup"><span data-stu-id="d9c08-114">Create a new product</span></span> | <span data-ttu-id="d9c08-115">ПОМЕСТИТЬ</span><span class="sxs-lookup"><span data-stu-id="d9c08-115">POST</span></span> | <span data-ttu-id="d9c08-116">/ api/продуктов</span><span class="sxs-lookup"><span data-stu-id="d9c08-116">/api/products</span></span> |
| <span data-ttu-id="d9c08-117">Обновления продукта</span><span class="sxs-lookup"><span data-stu-id="d9c08-117">Update a product</span></span> | <span data-ttu-id="d9c08-118">PUT</span><span class="sxs-lookup"><span data-stu-id="d9c08-118">PUT</span></span> | <span data-ttu-id="d9c08-119">/API/продукты/*идентификатор*</span><span class="sxs-lookup"><span data-stu-id="d9c08-119">/api/products/*id*</span></span> |
| <span data-ttu-id="d9c08-120">Удалить продукт</span><span class="sxs-lookup"><span data-stu-id="d9c08-120">Delete a product</span></span> | <span data-ttu-id="d9c08-121">DELETE</span><span class="sxs-lookup"><span data-stu-id="d9c08-121">DELETE</span></span> | <span data-ttu-id="d9c08-122">/API/продукты/*идентификатор*</span><span class="sxs-lookup"><span data-stu-id="d9c08-122">/api/products/*id*</span></span> |

<span data-ttu-id="d9c08-123">Чтобы узнать, как реализовать этот интерфейс API с веб-API ASP.NET, см. в разделе [Создание веб-API, поддерживает операции CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span><span class="sxs-lookup"><span data-stu-id="d9c08-123">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="d9c08-124">Для простоты клиентское приложение в этом руководстве используется консольное приложение Windows.</span><span class="sxs-lookup"><span data-stu-id="d9c08-124">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="d9c08-125">**HttpClient** также поддерживается для приложений Windows Phone и Windows Store.</span><span class="sxs-lookup"><span data-stu-id="d9c08-125">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="d9c08-126">Дополнительные сведения см. в разделе [код записи веб-API клиента для нескольких платформ с помощью переносимых библиотек](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="d9c08-126">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="d9c08-127">Создание консольного приложения</span><span class="sxs-lookup"><span data-stu-id="d9c08-127">Create the Console Application</span></span>

<span data-ttu-id="d9c08-128">В Visual Studio создайте новое консольное приложение Windows с именем **HttpClientSample** и вставьте в него следующий код:</span><span class="sxs-lookup"><span data-stu-id="d9c08-128">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="d9c08-129">Приведенный выше код является полный клиентским приложением.</span><span class="sxs-lookup"><span data-stu-id="d9c08-129">The preceding code is the complete client app.</span></span>

<span data-ttu-id="d9c08-130">`RunAsync` запуски и блокируется до его завершения.</span><span class="sxs-lookup"><span data-stu-id="d9c08-130">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="d9c08-131">Большинство **HttpClient** методы являются async, поскольку они выполняют подсистемы ввода/вывода.</span><span class="sxs-lookup"><span data-stu-id="d9c08-131">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="d9c08-132">Все асинхронные задачи выполняемые в `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="d9c08-132">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="d9c08-133">Обычно приложения не блокирует основной поток, но это приложение не допускает каких-либо взаимодействий.</span><span class="sxs-lookup"><span data-stu-id="d9c08-133">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="d9c08-134">Установка библиотеки Web API клиента</span><span class="sxs-lookup"><span data-stu-id="d9c08-134">Install the Web API Client Libraries</span></span>

<span data-ttu-id="d9c08-135">С помощью диспетчера пакетов NuGet для установки пакета Web API клиентских библиотек.</span><span class="sxs-lookup"><span data-stu-id="d9c08-135">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="d9c08-136">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="d9c08-136">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="d9c08-137">В консоли диспетчера пакетов (PMC), введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="d9c08-137">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="d9c08-138">Предыдущая команда добавляет в проект следующие пакеты NuGet:</span><span class="sxs-lookup"><span data-stu-id="d9c08-138">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="d9c08-139">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="d9c08-139">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="d9c08-140">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="d9c08-140">Newtonsoft.Json</span></span>

<span data-ttu-id="d9c08-141">Json.NET — это популярная платформа JSON высокой производительности для .NET.</span><span class="sxs-lookup"><span data-stu-id="d9c08-141">Json.NET is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="d9c08-142">Добавление класса модели</span><span class="sxs-lookup"><span data-stu-id="d9c08-142">Add a Model Class</span></span>

<span data-ttu-id="d9c08-143">Изучите `Product` класса:</span><span class="sxs-lookup"><span data-stu-id="d9c08-143">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="d9c08-144">Этот класс соответствует модели данных, используемых веб-API.</span><span class="sxs-lookup"><span data-stu-id="d9c08-144">This class matches the data model used by the web API.</span></span> <span data-ttu-id="d9c08-145">Приложение может использовать **HttpClient** для чтения `Product` экземпляр HTTP-ответа.</span><span class="sxs-lookup"><span data-stu-id="d9c08-145">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="d9c08-146">Приложение не нужно писать код десериализации.</span><span class="sxs-lookup"><span data-stu-id="d9c08-146">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="d9c08-147">Создание и инициализация HttpClient</span><span class="sxs-lookup"><span data-stu-id="d9c08-147">Create and Initialize HttpClient</span></span>

<span data-ttu-id="d9c08-148">Изучите статический **HttpClient** свойство:</span><span class="sxs-lookup"><span data-stu-id="d9c08-148">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="d9c08-149">**HttpClient** будет должен создаваться один раз и повторно использоваться на протяжении всего жизненного цикла приложения.</span><span class="sxs-lookup"><span data-stu-id="d9c08-149">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="d9c08-150">Можно привести следующие условия **SocketException** ошибок:</span><span class="sxs-lookup"><span data-stu-id="d9c08-150">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="d9c08-151">Создание нового **HttpClient** экземпляра на запрос.</span><span class="sxs-lookup"><span data-stu-id="d9c08-151">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="d9c08-152">Сервер в условиях большой нагрузки.</span><span class="sxs-lookup"><span data-stu-id="d9c08-152">Server under heavy load.</span></span>

<span data-ttu-id="d9c08-153">Создание нового **HttpClient** экземпляра на запрос может исчерпать все доступные сокеты.</span><span class="sxs-lookup"><span data-stu-id="d9c08-153">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="d9c08-154">В следующем примере кода инициализирует **HttpClient** экземпляр:</span><span class="sxs-lookup"><span data-stu-id="d9c08-154">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="d9c08-155">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="d9c08-155">The preceding code:</span></span>

* <span data-ttu-id="d9c08-156">Задает базовый URI для HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="d9c08-156">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="d9c08-157">Измените номер порта к порту, используемому в серверное приложение.</span><span class="sxs-lookup"><span data-stu-id="d9c08-157">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="d9c08-158">Если не используется порт для серверного приложения для работы приложения.</span><span class="sxs-lookup"><span data-stu-id="d9c08-158">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="d9c08-159">Задает заголовок Accept «application/json».</span><span class="sxs-lookup"><span data-stu-id="d9c08-159">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="d9c08-160">Параметр этот заголовок указывает, что сервер для отправки данных в формате JSON.</span><span class="sxs-lookup"><span data-stu-id="d9c08-160">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="d9c08-161">Отправка запроса GET для извлечения ресурса</span><span class="sxs-lookup"><span data-stu-id="d9c08-161">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="d9c08-162">Следующий код отправляет запрос GET для продукта:</span><span class="sxs-lookup"><span data-stu-id="d9c08-162">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="d9c08-163">**GetAsync** метод отправляет запрос HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="d9c08-163">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="d9c08-164">Если метод завершается, она возвращает **HttpResponseMessage** , содержащий HTTP-ответа.</span><span class="sxs-lookup"><span data-stu-id="d9c08-164">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="d9c08-165">Если код состояния в ответе код успешного выполнения, текст ответа содержит представление JSON продукта.</span><span class="sxs-lookup"><span data-stu-id="d9c08-165">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="d9c08-166">Вызовите **ReadAsAsync** для полезных данных JSON для десериализации `Product` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="d9c08-166">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="d9c08-167">**ReadAsAsync** метод является асинхронным, поскольку текст ответа может быть произвольно большим.</span><span class="sxs-lookup"><span data-stu-id="d9c08-167">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="d9c08-168">**HttpClient** не выдает исключение при HTTP-ответа содержит код ошибки.</span><span class="sxs-lookup"><span data-stu-id="d9c08-168">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="d9c08-169">Вместо этого **IsSuccessStatusCode** свойство **false** Если состояние — код ошибки.</span><span class="sxs-lookup"><span data-stu-id="d9c08-169">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="d9c08-170">Если вы предпочитаете обрабатывать коды ошибок HTTP как исключения, вызвать [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) объекта response.</span><span class="sxs-lookup"><span data-stu-id="d9c08-170">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="d9c08-171">`EnsureSuccessStatusCode` создает исключение, если код состояния находится вне диапазона 200&ndash;299.</span><span class="sxs-lookup"><span data-stu-id="d9c08-171">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="d9c08-172">Обратите внимание, что **HttpClient** может создавать исключения по другим причинам &mdash; к примеру, если тайм-аута запроса.</span><span class="sxs-lookup"><span data-stu-id="d9c08-172">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="d9c08-173">Модули форматирования типа мультимедиа для десериализации</span><span class="sxs-lookup"><span data-stu-id="d9c08-173">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="d9c08-174">Когда **ReadAsAsync** вызывается без параметров, он использует набор по умолчанию *модули форматирования мультимедиа* прочитать текст ответа.</span><span class="sxs-lookup"><span data-stu-id="d9c08-174">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="d9c08-175">Модули форматирования по умолчанию поддерживает JSON, XML и данные в форме кодировке к URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="d9c08-175">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="d9c08-176">Вместо того чтобы использовать модули форматирования по умолчанию, можно предоставить список модулей форматирования для **ReadAsAsync** метод.</span><span class="sxs-lookup"><span data-stu-id="d9c08-176">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="d9c08-177">Использование список модулей форматирования полезно в том случае, если у вас есть модуль форматирования типа мультимедиа.</span><span class="sxs-lookup"><span data-stu-id="d9c08-177">Using a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="d9c08-178">Дополнительные сведения см. в разделе [модули форматирования мультимедиа в ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="d9c08-178">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="d9c08-179">Отправить запрос POST для создания ресурса</span><span class="sxs-lookup"><span data-stu-id="d9c08-179">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="d9c08-180">Следующий код отправляет запрос POST, содержащий `Product` экземпляра в формате JSON:</span><span class="sxs-lookup"><span data-stu-id="d9c08-180">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="d9c08-181">**PostAsJsonAsync** метод:</span><span class="sxs-lookup"><span data-stu-id="d9c08-181">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="d9c08-182">Сериализует объект в формат JSON.</span><span class="sxs-lookup"><span data-stu-id="d9c08-182">Serializes an object to JSON.</span></span>
* <span data-ttu-id="d9c08-183">Отправляет полезные данные JSON в запросе POST.</span><span class="sxs-lookup"><span data-stu-id="d9c08-183">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="d9c08-184">Если запрос выполнен успешно:</span><span class="sxs-lookup"><span data-stu-id="d9c08-184">If the request succeeds:</span></span>

* <span data-ttu-id="d9c08-185">Он должен вернуть ответ 201 (создано).</span><span class="sxs-lookup"><span data-stu-id="d9c08-185">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="d9c08-186">Ответ должен содержать URL-адрес созданные ресурсы в заголовке Location.</span><span class="sxs-lookup"><span data-stu-id="d9c08-186">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="d9c08-187">Отправка запроса PUT для обновления ресурса</span><span class="sxs-lookup"><span data-stu-id="d9c08-187">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="d9c08-188">Следующий код отправляет запрос PUT для обновления продукта:</span><span class="sxs-lookup"><span data-stu-id="d9c08-188">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="d9c08-189">**PutAsJsonAsync** метод работает подобно **PostAsJsonAsync**, за исключением того, что он отправляет запрос PUT вместо POST.</span><span class="sxs-lookup"><span data-stu-id="d9c08-189">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="d9c08-190">Отправляя запрос DELETE для удаления ресурса</span><span class="sxs-lookup"><span data-stu-id="d9c08-190">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="d9c08-191">Следующий код отправляет запрос DELETE для удаления продукта:</span><span class="sxs-lookup"><span data-stu-id="d9c08-191">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="d9c08-192">Например GET запрос DELETE не имеет текста запроса.</span><span class="sxs-lookup"><span data-stu-id="d9c08-192">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="d9c08-193">Не нужно указать формат JSON или XML с помощью удаления.</span><span class="sxs-lookup"><span data-stu-id="d9c08-193">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="d9c08-194">Тестирование образца</span><span class="sxs-lookup"><span data-stu-id="d9c08-194">Test the sample</span></span>

<span data-ttu-id="d9c08-195">Чтобы проверить его:</span><span class="sxs-lookup"><span data-stu-id="d9c08-195">To test the client app:</span></span>

1. <span data-ttu-id="d9c08-196">[Скачайте](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) и запуск приложения сервера.</span><span class="sxs-lookup"><span data-stu-id="d9c08-196">[Download](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) and run the server app.</span></span> <span data-ttu-id="d9c08-197">[Указания по скачиванию](/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="d9c08-197">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> <span data-ttu-id="d9c08-198">Убедитесь, что работает серверное приложение.</span><span class="sxs-lookup"><span data-stu-id="d9c08-198">Verify the server app is working.</span></span> <span data-ttu-id="d9c08-199">Для exaxmple `http://localhost:64195/api/products` должен возвращать список продуктов.</span><span class="sxs-lookup"><span data-stu-id="d9c08-199">For exaxmple, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="d9c08-200">Задайте базовый URI для HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="d9c08-200">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="d9c08-201">Измените номер порта к порту, используемому в серверное приложение.</span><span class="sxs-lookup"><span data-stu-id="d9c08-201">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="d9c08-202">Запустите клиентское приложение.</span><span class="sxs-lookup"><span data-stu-id="d9c08-202">Run the client app.</span></span> <span data-ttu-id="d9c08-203">Выводятся следующие результаты.</span><span class="sxs-lookup"><span data-stu-id="d9c08-203">The following output is produced:</span></span>

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
