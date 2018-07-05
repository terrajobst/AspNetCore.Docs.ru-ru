---
uid: web-api/overview/advanced/http-cookies
title: Файлы cookie HTTP в веб-API ASP.NET | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 5885586df1d0f67d4e7e04ad88bc4fd1af71dc80
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368409"
---
<a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="63012-102">Файлы cookie HTTP в веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="63012-102">HTTP Cookies in ASP.NET Web API</span></span>
====================
<span data-ttu-id="63012-103">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="63012-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="63012-104">В этом разделе описывается, как отправлять и получать файлы cookie HTTP в веб-API.</span><span class="sxs-lookup"><span data-stu-id="63012-104">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="63012-105">Фон с помощью файлов cookie HTTP</span><span class="sxs-lookup"><span data-stu-id="63012-105">Background on HTTP Cookies</span></span>

<span data-ttu-id="63012-106">В этом разделе дается краткий обзор реализации файлы cookie на уровне HTTP.</span><span class="sxs-lookup"><span data-stu-id="63012-106">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="63012-107">Дополнительные сведения см. в [RFC 6265](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="63012-107">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="63012-108">Файл cookie — это часть данных, которые сервер отправляет HTTP-ответов.</span><span class="sxs-lookup"><span data-stu-id="63012-108">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="63012-109">Клиент (при необходимости) сохраняет файл cookie и возвращает его при запросах subsequet.</span><span class="sxs-lookup"><span data-stu-id="63012-109">The client (optionally) stores the cookie and returns it on subsequet requests.</span></span> <span data-ttu-id="63012-110">Это позволяет использовать общие состояния клиента и сервера.</span><span class="sxs-lookup"><span data-stu-id="63012-110">This allows the client and server to share state.</span></span> <span data-ttu-id="63012-111">Чтобы задать файл cookie, сервер включает заголовок Set-Cookie в ответе.</span><span class="sxs-lookup"><span data-stu-id="63012-111">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="63012-112">Формат файла cookie — это пара имя значение, с дополнительными атрибутами.</span><span class="sxs-lookup"><span data-stu-id="63012-112">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="63012-113">Пример:</span><span class="sxs-lookup"><span data-stu-id="63012-113">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="63012-114">Ниже приведен пример с атрибутами:</span><span class="sxs-lookup"><span data-stu-id="63012-114">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="63012-115">Чтобы вернуть файл cookie на сервер, клиент включает заголовок Cookie в последующих запросах.</span><span class="sxs-lookup"><span data-stu-id="63012-115">To return a cookie to the server, the client includes a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="63012-116">HTTP-ответ может включать несколько заголовков Set-Cookie.</span><span class="sxs-lookup"><span data-stu-id="63012-116">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="63012-117">Клиент возвращает несколько файлов cookie, используя один заголовок файла Cookie.</span><span class="sxs-lookup"><span data-stu-id="63012-117">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="63012-118">Объема и продолжительности файла cookie управляются следующие атрибуты в заголовке Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="63012-118">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="63012-119">**Домен**: сообщает клиенту, какой домен должен получать куки-файл.</span><span class="sxs-lookup"><span data-stu-id="63012-119">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="63012-120">Например если домен — «example.com», клиент возвращает файл cookie в каждом поддомен example.com.</span><span class="sxs-lookup"><span data-stu-id="63012-120">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com.</span></span> <span data-ttu-id="63012-121">Если не указан, домен — на сервере-источнике.</span><span class="sxs-lookup"><span data-stu-id="63012-121">If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="63012-122">**Путь**: ограничивает куки-файл по указанному пути в пределах домена.</span><span class="sxs-lookup"><span data-stu-id="63012-122">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="63012-123">Если не указан, используется путь к URI запроса.</span><span class="sxs-lookup"><span data-stu-id="63012-123">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="63012-124">**Срок действия истекает**: задает дату окончания срока действия файла cookie.</span><span class="sxs-lookup"><span data-stu-id="63012-124">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="63012-125">Клиент удаляет файл cookie, когда истечет срок его действия.</span><span class="sxs-lookup"><span data-stu-id="63012-125">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="63012-126">**Max-Age**: задает максимальное время существования файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="63012-126">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="63012-127">Клиент удаляет файл cookie, когда будет достигнуто максимальное время существования.</span><span class="sxs-lookup"><span data-stu-id="63012-127">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="63012-128">Если оба `Expires` и `Max-Age` заданы, `Max-Age` имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="63012-128">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="63012-129">Если не задан, клиент удаляет файл cookie после завершения текущего сеанса.</span><span class="sxs-lookup"><span data-stu-id="63012-129">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="63012-130">(Точное значение «сеанс» определяется агентом пользователя.)</span><span class="sxs-lookup"><span data-stu-id="63012-130">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="63012-131">Однако имейте в виду, что клиенты могут игнорировать файлы cookie.</span><span class="sxs-lookup"><span data-stu-id="63012-131">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="63012-132">Например пользователь может отключить файлы cookie для соблюдения конфиденциальности.</span><span class="sxs-lookup"><span data-stu-id="63012-132">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="63012-133">Клиенты могут удалять файлы cookie, до истечения срока действия или ограничить количество хранящихся файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="63012-133">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="63012-134">По соображениям конфиденциальности клиенты отклоняют часто файлы cookie «third party», где домен соответствует на сервере-источнике.</span><span class="sxs-lookup"><span data-stu-id="63012-134">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="63012-135">Короче говоря сервер не следует полагаться на получение, на которые он задает файлы cookie.</span><span class="sxs-lookup"><span data-stu-id="63012-135">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="63012-136">Файлы cookie в веб-API</span><span class="sxs-lookup"><span data-stu-id="63012-136">Cookies in Web API</span></span>

<span data-ttu-id="63012-137">Чтобы добавить маркер в HTTP-ответ, создайте **CookieHeaderValue** экземпляр, представляющий файл cookie.</span><span class="sxs-lookup"><span data-stu-id="63012-137">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="63012-138">Затем вызовите **AddCookies** метод расширения, который определен в **System.Net.Http. HttpResponseHeadersExtensions** класс, чтобы добавить файл cookie.</span><span class="sxs-lookup"><span data-stu-id="63012-138">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="63012-139">Например следующий код добавляет файл cookie в действие контроллера:</span><span class="sxs-lookup"><span data-stu-id="63012-139">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="63012-140">Обратите внимание, что **AddCookies** принимает массив **CookieHeaderValue** экземпляров.</span><span class="sxs-lookup"><span data-stu-id="63012-140">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="63012-141">Чтобы извлечь файлы cookie запроса клиента, вызовите **GetCookies** метод:</span><span class="sxs-lookup"><span data-stu-id="63012-141">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="63012-142">Объект **CookieHeaderValue** содержит коллекцию **CookieState** экземпляров.</span><span class="sxs-lookup"><span data-stu-id="63012-142">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="63012-143">Каждый **CookieState** представляет один файл cookie.</span><span class="sxs-lookup"><span data-stu-id="63012-143">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="63012-144">Метод индексатор для получения **CookieState** по имени, как показано.</span><span class="sxs-lookup"><span data-stu-id="63012-144">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="63012-145">Файл Cookie структурированных данных</span><span class="sxs-lookup"><span data-stu-id="63012-145">Structured Cookie Data</span></span>

<span data-ttu-id="63012-146">Многие браузеры ограничить количество файлов cookie, они будут храниться&#8212;как общее количество, так и номер каждого домена.</span><span class="sxs-lookup"><span data-stu-id="63012-146">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="63012-147">Таким образом можно попробовать установить структурированных данных в единый файл cookie, а не задавать несколько файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="63012-147">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="63012-148">RFC 6265 не определяет структуру данных файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="63012-148">RFC 6265 does not define the structure of cookie data.</span></span>


<span data-ttu-id="63012-149">С помощью **CookieHeaderValue** класса, можно передать список пар имя значение для данных файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="63012-149">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="63012-150">Эти пары "имя значение" кодируются в виде URL-адреса формы данных в заголовке Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="63012-150">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="63012-151">Приведенный выше код создает следующий заголовок Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="63012-151">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="63012-152">**CookieState** класс предоставляет метод индексатора для считывания вложенных значений из файла cookie в сообщении запроса:</span><span class="sxs-lookup"><span data-stu-id="63012-152">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="63012-153">Пример: Задание и получение файлов cookie в обработчик сообщений</span><span class="sxs-lookup"><span data-stu-id="63012-153">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="63012-154">В предыдущих примерах показано, как использовать файлы cookie из контроллера веб-API.</span><span class="sxs-lookup"><span data-stu-id="63012-154">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="63012-155">Другой вариант — использовать [обработчиков сообщений](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="63012-155">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="63012-156">Ранее в конвейере контроллеры вызываются обработчики сообщений.</span><span class="sxs-lookup"><span data-stu-id="63012-156">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="63012-157">Обработчик сообщений можно считать файлы cookie из запроса, прежде чем они достигнут контроллера или добавьте файлы cookie в ответ после контроллер создает ответ.</span><span class="sxs-lookup"><span data-stu-id="63012-157">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="63012-158">В следующем коде показано обработчик сообщений для создания идентификаторов сеансов.</span><span class="sxs-lookup"><span data-stu-id="63012-158">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="63012-159">Идентификатор сеанса хранится в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="63012-159">The session ID is stored in a cookie.</span></span> <span data-ttu-id="63012-160">Обработчик проверяет запрос файл cookie сеанса.</span><span class="sxs-lookup"><span data-stu-id="63012-160">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="63012-161">Если запрос не содержит файл cookie, обработчик создает идентификатор нового сеанса.</span><span class="sxs-lookup"><span data-stu-id="63012-161">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="63012-162">В любом случае обработчик сохраняет идентификатор сеанса в **HttpRequestMessage.Properties** контейнер свойств.</span><span class="sxs-lookup"><span data-stu-id="63012-162">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="63012-163">Он также добавляет файл cookie сеанса для HTTP-ответа.</span><span class="sxs-lookup"><span data-stu-id="63012-163">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="63012-164">Эта реализация не проверяет, что идентификатор сеанса от клиента, фактически был выдан сервером.</span><span class="sxs-lookup"><span data-stu-id="63012-164">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="63012-165">Не используйте его как форму проверки подлинности!</span><span class="sxs-lookup"><span data-stu-id="63012-165">Don't use it as a form of authentication!</span></span> <span data-ttu-id="63012-166">Точка примера — Показать куки-файлы HTTP.</span><span class="sxs-lookup"><span data-stu-id="63012-166">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="63012-167">Контроллер можно получить идентификатор сеанса из **HttpRequestMessage.Properties** контейнер свойств.</span><span class="sxs-lookup"><span data-stu-id="63012-167">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
