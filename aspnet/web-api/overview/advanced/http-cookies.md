---
uid: web-api/overview/advanced/http-cookies
title: Файлы cookie HTTP в ASP.NET Web API | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 363ca975cf75b635b766a53eeda87cf957eed60c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
ms.locfileid: "30071633"
---
<a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="af5d1-102">Файлы cookie HTTP в веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="af5d1-102">HTTP Cookies in ASP.NET Web API</span></span>
====================
<span data-ttu-id="af5d1-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="af5d1-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="af5d1-104">Описывается, как отправлять и получать файлы cookie HTTP в веб-API.</span><span class="sxs-lookup"><span data-stu-id="af5d1-104">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="af5d1-105">Основные сведения о файлы cookie HTTP</span><span class="sxs-lookup"><span data-stu-id="af5d1-105">Background on HTTP Cookies</span></span>

<span data-ttu-id="af5d1-106">В этом разделе содержится краткий обзор о реализации файлы cookie на уровне HTTP.</span><span class="sxs-lookup"><span data-stu-id="af5d1-106">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="af5d1-107">Дополнительные сведения, обратитесь к [RFC 6265](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="af5d1-107">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="af5d1-108">Файл cookie — это часть данных, сервер отправляет в ответ HTTP.</span><span class="sxs-lookup"><span data-stu-id="af5d1-108">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="af5d1-109">Клиент (необязательно) сохраняет файл cookie и возвращает его на subsequet запросы.</span><span class="sxs-lookup"><span data-stu-id="af5d1-109">The client (optionally) stores the cookie and returns it on subsequet requests.</span></span> <span data-ttu-id="af5d1-110">Это позволяет совместно использовать состояние клиента и сервера.</span><span class="sxs-lookup"><span data-stu-id="af5d1-110">This allows the client and server to share state.</span></span> <span data-ttu-id="af5d1-111">Чтобы задать файл cookie, сервер включает заголовок Set-Cookie в ответе.</span><span class="sxs-lookup"><span data-stu-id="af5d1-111">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="af5d1-112">Формат файла cookie — это пара имя значение с дополнительными атрибутами.</span><span class="sxs-lookup"><span data-stu-id="af5d1-112">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="af5d1-113">Пример:</span><span class="sxs-lookup"><span data-stu-id="af5d1-113">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="af5d1-114">Ниже приведен пример с атрибутами:</span><span class="sxs-lookup"><span data-stu-id="af5d1-114">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="af5d1-115">Чтобы вернуть куки-файл на сервер, клиент включает заголовок файла Cookie в последующих запросах.</span><span class="sxs-lookup"><span data-stu-id="af5d1-115">To return a cookie to the server, the client includes a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="af5d1-116">HTTP-ответа, может включать несколько заголовков Set-Cookie.</span><span class="sxs-lookup"><span data-stu-id="af5d1-116">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="af5d1-117">Клиент возвращает несколько файлов cookie, с помощью одного заголовка файла Cookie.</span><span class="sxs-lookup"><span data-stu-id="af5d1-117">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="af5d1-118">Область и длительность файла cookie, определяются следующие атрибуты в заголовок Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="af5d1-118">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="af5d1-119">**Домен**: сообщает клиенту, какой домен должен получить куки-файл.</span><span class="sxs-lookup"><span data-stu-id="af5d1-119">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="af5d1-120">Например если домен «example.com», клиент возвращает куки-файл каждый поддомен example.com. Если не указан, указан домен исходного сервера.</span><span class="sxs-lookup"><span data-stu-id="af5d1-120">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com. If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="af5d1-121">**Путь**: ограничивает куки-файл по указанному пути в пределах домена.</span><span class="sxs-lookup"><span data-stu-id="af5d1-121">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="af5d1-122">Если не указан, используется путь запроса URI.</span><span class="sxs-lookup"><span data-stu-id="af5d1-122">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="af5d1-123">**Срок действия истекает**: задает дату окончания срока действия файла cookie.</span><span class="sxs-lookup"><span data-stu-id="af5d1-123">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="af5d1-124">Клиент удаляет файл cookie, после истечения срока его действия.</span><span class="sxs-lookup"><span data-stu-id="af5d1-124">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="af5d1-125">**Max-Age**: задает максимальное время существования файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="af5d1-125">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="af5d1-126">Клиент удаляет файл cookie, при достижении максимального возраста.</span><span class="sxs-lookup"><span data-stu-id="af5d1-126">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="af5d1-127">Если оба `Expires` и `Max-Age` заданы, `Max-Age` имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="af5d1-127">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="af5d1-128">Если не задан ни один клиент удаляет файл cookie при завершении текущего сеанса.</span><span class="sxs-lookup"><span data-stu-id="af5d1-128">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="af5d1-129">(Точное значение «сеанс» определяется агентом пользователя.)</span><span class="sxs-lookup"><span data-stu-id="af5d1-129">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="af5d1-130">Однако имейте в виду, что клиенты могут игнорировать файлы cookie.</span><span class="sxs-lookup"><span data-stu-id="af5d1-130">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="af5d1-131">Например пользователь может отключить файлы cookie для соблюдения конфиденциальности.</span><span class="sxs-lookup"><span data-stu-id="af5d1-131">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="af5d1-132">Клиенты могут удалять файлы cookie, до истечения срока действия или ограничить количество хранящихся файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="af5d1-132">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="af5d1-133">По соображениям конфиденциальности часто отклонять куки-файлы «third party», где домен не совпадает с исходного сервера.</span><span class="sxs-lookup"><span data-stu-id="af5d1-133">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="af5d1-134">Иными словами сервер не следует полагаться на получение файлов cookie, которые он задает.</span><span class="sxs-lookup"><span data-stu-id="af5d1-134">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="af5d1-135">Файлы cookie в веб-API</span><span class="sxs-lookup"><span data-stu-id="af5d1-135">Cookies in Web API</span></span>

<span data-ttu-id="af5d1-136">Чтобы добавить файл cookie HTTP-ответа, создать **CookieHeaderValue** экземпляр, представляющий файл cookie.</span><span class="sxs-lookup"><span data-stu-id="af5d1-136">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="af5d1-137">Затем вызовите **AddCookies** метод расширения, который определен в **System.Net.Http. HttpResponseHeadersExtensions** класса, чтобы добавить файл cookie.</span><span class="sxs-lookup"><span data-stu-id="af5d1-137">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="af5d1-138">Например следующий код добавляет файл cookie в пределах действия контроллера:</span><span class="sxs-lookup"><span data-stu-id="af5d1-138">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="af5d1-139">Обратите внимание, что **AddCookies** принимает массив **CookieHeaderValue** экземпляров.</span><span class="sxs-lookup"><span data-stu-id="af5d1-139">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="af5d1-140">Чтобы извлечь файлы cookie из запроса клиента, вызовите **GetCookies** метод:</span><span class="sxs-lookup"><span data-stu-id="af5d1-140">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="af5d1-141">Объект **CookieHeaderValue** содержит коллекцию **CookieState** экземпляров.</span><span class="sxs-lookup"><span data-stu-id="af5d1-141">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="af5d1-142">Каждый **CookieState** представляет один файл cookie.</span><span class="sxs-lookup"><span data-stu-id="af5d1-142">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="af5d1-143">Метод индексатор для получения **CookieState** по имени, как показано.</span><span class="sxs-lookup"><span data-stu-id="af5d1-143">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="af5d1-144">Cookie структурированных данных</span><span class="sxs-lookup"><span data-stu-id="af5d1-144">Structured Cookie Data</span></span>

<span data-ttu-id="af5d1-145">Большинство браузеров ограничить количество файлов cookie, они будут храниться&#8212;общее число и число в домене.</span><span class="sxs-lookup"><span data-stu-id="af5d1-145">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="af5d1-146">Таким образом его можно использовать для размещения структурированных данных в одном файле cookie, вместо задания нескольких файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="af5d1-146">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="af5d1-147">RFC 6265 не определяет структуру данных файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="af5d1-147">RFC 6265 does not define the structure of cookie data.</span></span>


<span data-ttu-id="af5d1-148">С помощью **CookieHeaderValue** класса, можно передать список пар имя значение для данных файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="af5d1-148">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="af5d1-149">Эти пары "имя значение", кодируются как данные формы, закодированные в URL-адрес в заголовке Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="af5d1-149">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="af5d1-150">Предыдущий код выводит следующий заголовок Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="af5d1-150">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="af5d1-151">**CookieState** класс предоставляет метод индексатора для чтения вложенные значения из куки-файла в сообщении запроса:</span><span class="sxs-lookup"><span data-stu-id="af5d1-151">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="af5d1-152">Пример: Задание и получение файлов cookie в обработчике сообщений</span><span class="sxs-lookup"><span data-stu-id="af5d1-152">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="af5d1-153">В предыдущих примерах показано, как использовать файлы cookie из контроллера веб-API.</span><span class="sxs-lookup"><span data-stu-id="af5d1-153">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="af5d1-154">Другой вариант — использовать [обработчиков сообщений](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="af5d1-154">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="af5d1-155">Обработчики сообщений вызываются ранее в конвейере контроллеры.</span><span class="sxs-lookup"><span data-stu-id="af5d1-155">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="af5d1-156">Обработчик сообщений можно считывать файлы cookie из запроса, прежде чем запрос достигнет контроллер или добавьте файлы cookie в ответ после контроллера формирует ответ.</span><span class="sxs-lookup"><span data-stu-id="af5d1-156">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="af5d1-157">В следующем коде показано создание идентификаторов сеансов обработчик сообщений.</span><span class="sxs-lookup"><span data-stu-id="af5d1-157">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="af5d1-158">Идентификатор сеанса хранится в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="af5d1-158">The session ID is stored in a cookie.</span></span> <span data-ttu-id="af5d1-159">Обработчик проверяет запрос файл cookie сеанса.</span><span class="sxs-lookup"><span data-stu-id="af5d1-159">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="af5d1-160">Если запрос не содержит файл cookie, обработчик создает идентификатор нового сеанса.</span><span class="sxs-lookup"><span data-stu-id="af5d1-160">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="af5d1-161">В любом случае обработчик сохраняет идентификатор сеанса в **HttpRequestMessage.Properties** контейнер свойств.</span><span class="sxs-lookup"><span data-stu-id="af5d1-161">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="af5d1-162">Он также добавляет файл cookie сеанса HTTP-ответа.</span><span class="sxs-lookup"><span data-stu-id="af5d1-162">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="af5d1-163">Эта реализация не выполняет проверку, сервер фактически выдан идентификатор сеанса от клиента.</span><span class="sxs-lookup"><span data-stu-id="af5d1-163">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="af5d1-164">Не используйте его как один из видов проверки подлинности!</span><span class="sxs-lookup"><span data-stu-id="af5d1-164">Don't use it as a form of authentication!</span></span> <span data-ttu-id="af5d1-165">Точка примера является демонстрация управления файла cookie HTTP.</span><span class="sxs-lookup"><span data-stu-id="af5d1-165">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="af5d1-166">Контроллер можно получить идентификатор сеанса из **HttpRequestMessage.Properties** контейнер свойств.</span><span class="sxs-lookup"><span data-stu-id="af5d1-166">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
