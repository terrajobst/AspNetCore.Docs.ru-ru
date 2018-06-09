---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: Согласование в веб-API ASP.NET содержимого | Документы Microsoft
author: MikeWasson
description: Описывает, как веб-API ASP.NET реализует согласования содержимого HTTP.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/20/2012
ms.topic: article
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: ca373af6754e82889dc100b63f73b76aaa4e4f27
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "26507023"
---
<a name="content-negotiation-in-aspnet-web-api"></a><span data-ttu-id="e2546-103">Согласование содержимого в веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e2546-103">Content Negotiation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="e2546-104">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e2546-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e2546-105">В этой статье описывается, как веб-API ASP.NET реализует согласования содержимого.</span><span class="sxs-lookup"><span data-stu-id="e2546-105">This article describes how ASP.NET Web API implements content negotiation.</span></span>

<span data-ttu-id="e2546-106">Спецификации протокола HTTP (RFC 2616) определяет согласование содержимого, как «процесс выбора наилучшим представлением для указанного ответа при наличии нескольких представлений.»</span><span class="sxs-lookup"><span data-stu-id="e2546-106">The HTTP specification (RFC 2616) defines content negotiation as "the process of selecting the best representation for a given response when there are multiple representations available."</span></span> <span data-ttu-id="e2546-107">Эти заголовки запроса являются основным механизмом для согласования содержимого в HTTP:</span><span class="sxs-lookup"><span data-stu-id="e2546-107">The primary mechanism for content negotiation in HTTP are these request headers:</span></span>

- <span data-ttu-id="e2546-108">**Примите:** типов мультимедиа, которые допустимые для ответа, такие как «application/json», «application/xml», или тип пользовательского носитель, такой как &quot;application/vnd.example+xml&quot;</span><span class="sxs-lookup"><span data-stu-id="e2546-108">**Accept:** Which media types are acceptable for the response, such as "application/json," "application/xml," or a custom media type such as &quot;application/vnd.example+xml&quot;</span></span>
- <span data-ttu-id="e2546-109">**Accept-Charset:** какие наборы символов допустимы, например UTF-8 или ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="e2546-109">**Accept-Charset:** Which character sets are acceptable, such as UTF-8 or ISO 8859-1.</span></span>
- <span data-ttu-id="e2546-110">**Приемлемой кодировкой:** допустимы какие кодировки содержимого, таких как gzip.</span><span class="sxs-lookup"><span data-stu-id="e2546-110">**Accept-Encoding:** Which content encodings are acceptable, such as gzip.</span></span>
- <span data-ttu-id="e2546-111">**Принять язык:** естественный язык, такие как «en-us».</span><span class="sxs-lookup"><span data-stu-id="e2546-111">**Accept-Language:** The preferred natural language, such as "en-us".</span></span>

<span data-ttu-id="e2546-112">Сервер можно также просмотреть другие составные части HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="e2546-112">The server can also look at other portions of the HTTP request.</span></span> <span data-ttu-id="e2546-113">Например если запрос содержит заголовок X-Requested-With, указывающее, AJAX-запросом, сервер может по умолчанию JSON Если отсутствует заголовок Accept.</span><span class="sxs-lookup"><span data-stu-id="e2546-113">For example, if the request contains an X-Requested-With header, indicating an AJAX request, the server might default to JSON if there is no Accept header.</span></span>

<span data-ttu-id="e2546-114">В этой статье мы рассмотрим, как веб-API использует заголовки Accept и Accept-Charset.</span><span class="sxs-lookup"><span data-stu-id="e2546-114">In this article, we'll look at how Web API uses the Accept and Accept-Charset headers.</span></span> <span data-ttu-id="e2546-115">(В настоящее время не поддерживается встроенная Accept-Encoding или Accept-Language.)</span><span class="sxs-lookup"><span data-stu-id="e2546-115">(At this time, there is no built-in support for Accept-Encoding or Accept-Language.)</span></span>

## <a name="serialization"></a><span data-ttu-id="e2546-116">Сериализация</span><span class="sxs-lookup"><span data-stu-id="e2546-116">Serialization</span></span>

<span data-ttu-id="e2546-117">Если контроллер веб-API возвращает ресурсов как тип CLR, конвейер сериализует возвращаемое значение и записывает его в тексте ответа HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2546-117">If a Web API controller returns a resource as CLR type, the pipeline serializes the return value and writes it into the HTTP response body.</span></span>

<span data-ttu-id="e2546-118">Например рассмотрим следующее действие контроллера:</span><span class="sxs-lookup"><span data-stu-id="e2546-118">For example, consider the following controller action:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

<span data-ttu-id="e2546-119">Клиент может передавать этого HTTP-запроса:</span><span class="sxs-lookup"><span data-stu-id="e2546-119">A client might send this HTTP request:</span></span>

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

<span data-ttu-id="e2546-120">В результате сервер может отправить:</span><span class="sxs-lookup"><span data-stu-id="e2546-120">In response, the server might send:</span></span>

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

<span data-ttu-id="e2546-121">В этом примере клиент запросил JSON, Javascript или «все» (\*/\*).</span><span class="sxs-lookup"><span data-stu-id="e2546-121">In this example, the client requested either JSON, Javascript, or "anything" (\*/\*).</span></span> <span data-ttu-id="e2546-122">Сервер в ответ получен представление JSON `Product` объекта.</span><span class="sxs-lookup"><span data-stu-id="e2546-122">The server responsed with a JSON representation of the `Product` object.</span></span> <span data-ttu-id="e2546-123">Обратите внимание, что имеет значение заголовка Content-Type в ответе &quot;приложение/json&quot;.</span><span class="sxs-lookup"><span data-stu-id="e2546-123">Notice that the Content-Type header in the response is set to &quot;application/json&quot;.</span></span>

<span data-ttu-id="e2546-124">Контроллер может также возвращать **HttpResponseMessage** объекта.</span><span class="sxs-lookup"><span data-stu-id="e2546-124">A controller can also return an **HttpResponseMessage** object.</span></span> <span data-ttu-id="e2546-125">Чтобы указать объект среды CLR, текст ответа, вызовите **CreateResponse** метода расширения:</span><span class="sxs-lookup"><span data-stu-id="e2546-125">To specify a CLR object for the response body, call the **CreateResponse** extension method:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

<span data-ttu-id="e2546-126">Данный параметр предоставляет больший контроль над сведения об ответе.</span><span class="sxs-lookup"><span data-stu-id="e2546-126">This option gives you more control over the details of the response.</span></span> <span data-ttu-id="e2546-127">Можно задать код состояния, добавление заголовков HTTP и так далее.</span><span class="sxs-lookup"><span data-stu-id="e2546-127">You can set the status code, add HTTP headers, and so forth.</span></span>

<span data-ttu-id="e2546-128">Объект, который сериализует ресурс называется *форматирования мультимедиа*.</span><span class="sxs-lookup"><span data-stu-id="e2546-128">The object that serializes the resource is called a *media formatter*.</span></span> <span data-ttu-id="e2546-129">Модули форматирования мультимедиа являются производными от **MediaTypeFormatter** класса.</span><span class="sxs-lookup"><span data-stu-id="e2546-129">Media formatters derive from the **MediaTypeFormatter** class.</span></span> <span data-ttu-id="e2546-130">Веб-API предоставляет модули форматирования мультимедиа для XML и JSON, и можно создать пользовательские модули форматирования для поддержки других типов носителей.</span><span class="sxs-lookup"><span data-stu-id="e2546-130">Web API provides media formatters for XML and JSON, and you can create custom formatters to support other media types.</span></span> <span data-ttu-id="e2546-131">Сведения о написании пользовательский модуль форматирования см. в разделе [модули форматирования мультимедиа](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="e2546-131">For information about writing a custom formatter, see [Media Formatters](media-formatters.md).</span></span>

## <a name="how-content-negotiation-works"></a><span data-ttu-id="e2546-132">Работает как содержимого согласования</span><span class="sxs-lookup"><span data-stu-id="e2546-132">How Content Negotiation Works</span></span>

<span data-ttu-id="e2546-133">Во-первых, конвейер получает **IContentNegotiator** из **HttpConfiguration** объекта.</span><span class="sxs-lookup"><span data-stu-id="e2546-133">First, the pipeline gets the **IContentNegotiator** service from the **HttpConfiguration** object.</span></span> <span data-ttu-id="e2546-134">Он также возвращает список модулей форматирования мультимедиа из **HttpConfiguration.Formatters** коллекции.</span><span class="sxs-lookup"><span data-stu-id="e2546-134">It also gets the list of media formatters from the **HttpConfiguration.Formatters** collection.</span></span>

<span data-ttu-id="e2546-135">Затем вызывает конвейер **IContentNegotiatior.Negotiate**, передавая:</span><span class="sxs-lookup"><span data-stu-id="e2546-135">Next, the pipeline calls **IContentNegotiatior.Negotiate**, passing in:</span></span>

- <span data-ttu-id="e2546-136">Тип объекта для сериализации</span><span class="sxs-lookup"><span data-stu-id="e2546-136">The type of object to serialize</span></span>
- <span data-ttu-id="e2546-137">Коллекция модулей форматирования мультимедиа</span><span class="sxs-lookup"><span data-stu-id="e2546-137">The collection of media formatters</span></span>
- <span data-ttu-id="e2546-138">HTTP-запроса</span><span class="sxs-lookup"><span data-stu-id="e2546-138">The HTTP request</span></span>

<span data-ttu-id="e2546-139">**Negotiate** метод возвращает два блока данных:</span><span class="sxs-lookup"><span data-stu-id="e2546-139">The **Negotiate** method returns two pieces of information:</span></span>

- <span data-ttu-id="e2546-140">Форматер, используя</span><span class="sxs-lookup"><span data-stu-id="e2546-140">Which formatter to use</span></span>
- <span data-ttu-id="e2546-141">Тип носителя для ответа</span><span class="sxs-lookup"><span data-stu-id="e2546-141">The media type for the response</span></span>

<span data-ttu-id="e2546-142">Если модуль форматирования не найден, **Negotiate** возвращает **null**и ошибка клиента получении HTTP 406 (неприемлемо).</span><span class="sxs-lookup"><span data-stu-id="e2546-142">If no formatter is found, the **Negotiate** method returns **null**, and the client recevies HTTP error 406 (Not Acceptable).</span></span>

<span data-ttu-id="e2546-143">В следующем коде показано, как контроллер можно напрямую вызвать согласование содержимого:</span><span class="sxs-lookup"><span data-stu-id="e2546-143">The following code shows how a controller can directly invoke content negotiation:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

<span data-ttu-id="e2546-144">Этот код является эквивалентом на то, что делает конвейера автоматически.</span><span class="sxs-lookup"><span data-stu-id="e2546-144">This code is equivalent to the what the pipeline does automatically.</span></span>

## <a name="default-content-negotiator"></a><span data-ttu-id="e2546-145">Согласователь содержимого по умолчанию</span><span class="sxs-lookup"><span data-stu-id="e2546-145">Default Content Negotiator</span></span>

<span data-ttu-id="e2546-146">**DefaultContentNegotiator** класс предоставляет реализацию по умолчанию **IContentNegotiator**.</span><span class="sxs-lookup"><span data-stu-id="e2546-146">The **DefaultContentNegotiator** class provides the default implementation of **IContentNegotiator**.</span></span> <span data-ttu-id="e2546-147">Он использует нескольких критериев для выбора модуля форматирования.</span><span class="sxs-lookup"><span data-stu-id="e2546-147">It uses several criteria to select a formatter.</span></span>

<span data-ttu-id="e2546-148">Во-первых модуль форматирования должен иметь возможность сериализовать тип.</span><span class="sxs-lookup"><span data-stu-id="e2546-148">First, the formatter must be able to serialize the type.</span></span> <span data-ttu-id="e2546-149">Это подтверждается вызов **MediaTypeFormatter.CanWriteType**.</span><span class="sxs-lookup"><span data-stu-id="e2546-149">This is verified by calling **MediaTypeFormatter.CanWriteType**.</span></span>

<span data-ttu-id="e2546-150">Затем согласователь содержимого просматривает каждого модуля форматирования и вычисляет степень его соответствия HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="e2546-150">Next, the content negotiator looks at each formatter and evaluates how well it matches the HTTP request.</span></span> <span data-ttu-id="e2546-151">Для оценки соответствия, согласователь содержимого проверяет два действия в модуль форматирования:</span><span class="sxs-lookup"><span data-stu-id="e2546-151">To evaluate the match, the content negotiator looks at two things on the formatter:</span></span>

- <span data-ttu-id="e2546-152">**SupportedMediaTypes** коллекции, которая содержит список типов носителей, поддерживаемых.</span><span class="sxs-lookup"><span data-stu-id="e2546-152">The **SupportedMediaTypes** collection, which contains a list of supported media types.</span></span> <span data-ttu-id="e2546-153">Согласователь содержимого предпринимается попытка сопоставить этот список для заголовка Accept запроса.</span><span class="sxs-lookup"><span data-stu-id="e2546-153">The content negotiator tries to match this list against the request Accept header.</span></span> <span data-ttu-id="e2546-154">Обратите внимание, что заголовок Accept может содержать диапазонов.</span><span class="sxs-lookup"><span data-stu-id="e2546-154">Note that the Accept header can include ranges.</span></span> <span data-ttu-id="e2546-155">Например, «text/plain» совпадение для текста или\* или \* / \*.</span><span class="sxs-lookup"><span data-stu-id="e2546-155">For example, "text/plain" is a match for text/\* or \*/\*.</span></span>
- <span data-ttu-id="e2546-156">**MediaTypeMappings** коллекции, которая содержит список **MediaTypeMapping** объектов.</span><span class="sxs-lookup"><span data-stu-id="e2546-156">The **MediaTypeMappings** collection, which contains a list of **MediaTypeMapping** objects.</span></span> <span data-ttu-id="e2546-157">**MediaTypeMapping** класс предоставляет универсальный способ для сопоставления с типами мультимедиа HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="e2546-157">The **MediaTypeMapping** class provides a generic way to match HTTP requests with media types.</span></span> <span data-ttu-id="e2546-158">Например он сопоставляется заголовок HTTP для определенного типа носителя.</span><span class="sxs-lookup"><span data-stu-id="e2546-158">For example, it could map a custom HTTP header to a particular media type.</span></span>

<span data-ttu-id="e2546-159">Если используется несколько совпадает, совпадение с наивысшим wins коэффициент качества.</span><span class="sxs-lookup"><span data-stu-id="e2546-159">If there are multiple matches, the match with the highest quality factor wins.</span></span> <span data-ttu-id="e2546-160">Пример:</span><span class="sxs-lookup"><span data-stu-id="e2546-160">For example:</span></span>

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

<span data-ttu-id="e2546-161">В этом примере приложение/json имеет коэффициент подразумеваемых качества, 1.0, поэтому он предпочтительнее, чем приложения и xml.</span><span class="sxs-lookup"><span data-stu-id="e2546-161">In this example, application/json has an implied quality factor of 1.0, so it is preferred over application/xml.</span></span>

<span data-ttu-id="e2546-162">Если соответствий не найдено, согласователь содержимого пытается сопоставить с типом мультимедиа тела запроса, если таковые имеются.</span><span class="sxs-lookup"><span data-stu-id="e2546-162">If no matches are found, the content negotiator tries to match on the media type of the request body, if any.</span></span> <span data-ttu-id="e2546-163">Например если запрос содержит данные JSON, модуль форматирования JSON выглядит согласователь содержимого.</span><span class="sxs-lookup"><span data-stu-id="e2546-163">For example, if the request contains JSON data, the content negotiator looks for a JSON formatter.</span></span>

<span data-ttu-id="e2546-164">Если по-прежнему не найдено совпадений, согласователь содержимого просто выбирает первый модуль форматирования, который может сериализовать тип.</span><span class="sxs-lookup"><span data-stu-id="e2546-164">If there are still no matches, the content negotiator simply picks the first formatter that can serialize the type.</span></span>

## <a name="selecting-a-character-encoding"></a><span data-ttu-id="e2546-165">При выборе кодировки символов</span><span class="sxs-lookup"><span data-stu-id="e2546-165">Selecting a Character Encoding</span></span>

<span data-ttu-id="e2546-166">После выбора модуля форматирования согласователь содержимого выбирает лучшую кодировку символов, просмотрев **SupportedEncodings** свойства форматирования и сопоставление его в запросе заголовок Accept-Charset (если таковые имеются).</span><span class="sxs-lookup"><span data-stu-id="e2546-166">After a formatter is selected, the content negotiator chooses the best character encoding by looking at the **SupportedEncodings** property on the formatter, and matching it against the Accept-Charset header in the request (if any).</span></span>
