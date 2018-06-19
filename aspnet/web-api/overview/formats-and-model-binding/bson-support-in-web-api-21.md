---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Поддержка BSON в ASP.NET Web API 2.1 | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 53ad705fad6d2225cecca4d73355bd6ebfcf56d5
ms.sourcegitcommit: 459cb3289741a3f46325e605a617dc926ee0563d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/22/2018
ms.locfileid: "27984587"
---
<a name="bson-support-in-aspnet-web-api-21"></a><span data-ttu-id="9ee1b-102">Поддержка BSON в ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="9ee1b-102">BSON Support in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="9ee1b-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9ee1b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="9ee1b-104">Веб-API 2.1 появилась поддержка BSON.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-104">Web API 2.1 introduces support for BSON.</span></span> <span data-ttu-id="9ee1b-105">В этом разделе показано, как использовать BSON в контроллере веб-API (на стороне сервера) или в клиентском приложении .NET.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-105">This topic shows how to use BSON in your Web API controller (server side) and in a .NET client app.</span></span>

## <a name="what-is-bson"></a><span data-ttu-id="9ee1b-106">Что такое BSON?</span><span class="sxs-lookup"><span data-stu-id="9ee1b-106">What is BSON?</span></span>

<span data-ttu-id="9ee1b-107">[BSON](http://bsonspec.org/) — это формат двоичной сериализации.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-107">[BSON](http://bsonspec.org/) is a binary serialization format.</span></span> <span data-ttu-id="9ee1b-108">«BSON» означает «Двоичный JSON», но очень по-разному сериализация BSON и JSON.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-108">"BSON" stands for "Binary JSON", but BSON and JSON are serialized very differently.</span></span> <span data-ttu-id="9ee1b-109">BSON — «JSON по принципу», так как объекты в виде пар имя значение, аналогично JSON.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-109">BSON is "JSON-like", because objects are represented as name-value pairs, similar to JSON.</span></span> <span data-ttu-id="9ee1b-110">В отличие от JSON числовые типы данных хранятся в виде байтов, а не строк</span><span class="sxs-lookup"><span data-stu-id="9ee1b-110">Unlike JSON, numeric data types are stored as bytes, not strings</span></span>

<span data-ttu-id="9ee1b-111">BSON была разработана для упрощенная, легко просматривать и быстро для шифрования или расшифровки.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-111">BSON was designed to be lightweight, easy to scan, and fast to encode/decode.</span></span>

- <span data-ttu-id="9ee1b-112">BSON сопоставим по размер в JSON.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-112">BSON is comparable in size to JSON.</span></span> <span data-ttu-id="9ee1b-113">В зависимости от данных BSON полезных данных может быть меньше или больше полезные данные JSON.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-113">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="9ee1b-114">Для сериализации двоичных данных, таких как файл изображения BSON меньше, чем JSON, так как двоичные данные не кодировке base64.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-114">For serializing binary data, such as an image file, BSON is smaller than JSON, because the binary data is not base64-encoded.</span></span>
- <span data-ttu-id="9ee1b-115">Документы BSON являются удобной для быстрого просмотра, так как элементы начинаются с префикса длины поля, поэтому средство синтаксического анализа можно пропустить элементы без их декодирования.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-115">BSON documents are easy to scan because elements are prefixed with a length field, so a parser can skip elements without decoding them.</span></span>
- <span data-ttu-id="9ee1b-116">Эффективны, так как числовые типы данных хранятся в виде числа, строки не кодирования и декодирования.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-116">Encoding and decoding are efficient, because numeric data types are stored as numbers, not strings.</span></span>

<span data-ttu-id="9ee1b-117">Собственных клиентов, например клиентские приложения .NET, можно использовать преимущества BSON вместо текстовые форматы, такие как JSON или XML.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-117">Native clients, such as .NET client apps, can benefit from using BSON in place of text-based formats such as JSON or XML.</span></span> <span data-ttu-id="9ee1b-118">Для обозревателя клиента, скорее всего вы заходите с JSON, поскольку JavaScript может напрямую обрабатывать полезные данные JSON.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-118">For browser clients, you will probably want to stick with JSON, because JavaScript can directly convert the JSON payload.</span></span>

<span data-ttu-id="9ee1b-119">К счастью, использует веб-API [содержимого согласования](content-negotiation.md), поэтому API может поддерживать оба формата и позволяют выбрать клиента.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-119">Fortunately, Web API uses [content negotiation](content-negotiation.md), so your API can support both formats and let the client choose.</span></span>

## <a name="enabling-bson-on-the-server"></a><span data-ttu-id="9ee1b-120">Включение BSON на сервере</span><span class="sxs-lookup"><span data-stu-id="9ee1b-120">Enabling BSON on the Server</span></span>

<span data-ttu-id="9ee1b-121">Добавьте в конфигурацию веб-API **BsonMediaTypeFormatter** в коллекцию модулей форматирования.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-121">In your Web API configuration, add the **BsonMediaTypeFormatter** to the formatters collection.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

<span data-ttu-id="9ee1b-122">Теперь если клиент запрашивает «application/bson», веб-API будет использовать модуль форматирования BSON.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-122">Now if the client requests "application/bson", Web API will use the BSON formatter.</span></span>

<span data-ttu-id="9ee1b-123">Чтобы связать BSON с другими типами мультимедиа, добавьте их в коллекцию SupportedMediaTypes.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-123">To associate BSON with other media types, add them to the SupportedMediaTypes collection.</span></span> <span data-ttu-id="9ee1b-124">Следующий код добавляет «application/vnd.contoso» поддерживаемые типы носителей:</span><span class="sxs-lookup"><span data-stu-id="9ee1b-124">The following code adds "application/vnd.contoso" to the supported media types:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a><span data-ttu-id="9ee1b-125">Пример сеанса HTTP</span><span class="sxs-lookup"><span data-stu-id="9ee1b-125">Example HTTP Session</span></span>

<span data-ttu-id="9ee1b-126">В этом примере мы будем использовать следующие классы модели, а также простой контроллер веб-API:</span><span class="sxs-lookup"><span data-stu-id="9ee1b-126">For this example, we'll use the following model class plus a simple Web API controller:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

<span data-ttu-id="9ee1b-127">Клиент может отправить следующий запрос HTTP:</span><span class="sxs-lookup"><span data-stu-id="9ee1b-127">A client might send the following HTTP request:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

<span data-ttu-id="9ee1b-128">Ниже представлен ответ:</span><span class="sxs-lookup"><span data-stu-id="9ee1b-128">Here is the response:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

<span data-ttu-id="9ee1b-129">Здесь я заменить двоичные данные с &quot;.&quot; символов.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-129">Here I've replaced the binary data with &quot;.&quot; characters.</span></span> <span data-ttu-id="9ee1b-130">На следующем снимке экрана из Fiddler показаны необработанные шестнадцатеричные значения.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-130">The following screen shot from Fiddler shows the raw hex values.</span></span>

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a><span data-ttu-id="9ee1b-131">Использование BSON с HttpClient</span><span class="sxs-lookup"><span data-stu-id="9ee1b-131">Using BSON with HttpClient</span></span>

<span data-ttu-id="9ee1b-132">Приложения .NET клиенты могут использовать форматирования BSON **HttpClient**.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-132">.NET clients apps can use the BSON formatter with **HttpClient**.</span></span> <span data-ttu-id="9ee1b-133">Дополнительные сведения о **HttpClient**, в разделе [вызов Web API из клиента .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="9ee1b-133">For more information about **HttpClient**, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="9ee1b-134">Следующий код отправляет запрос GET, которая принимает BSON и затем выполняет десериализацию полезных данных BSON в ответе.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-134">The following code sends a GET request that accepts BSON, and then deserializes the BSON payload in the response.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

<span data-ttu-id="9ee1b-135">Чтобы запросить BSON с сервера, задайте заголовок Accept для «application/bson»:</span><span class="sxs-lookup"><span data-stu-id="9ee1b-135">To request BSON from the server, set the Accept header to "application/bson":</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

<span data-ttu-id="9ee1b-136">Чтобы десериализовать текст ответа, используйте **BsonMediaTypeFormatter**.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-136">To deserialize the response body, use the **BsonMediaTypeFormatter**.</span></span> <span data-ttu-id="9ee1b-137">Этот модуль форматирования не в коллекцию модулей форматирования по умолчанию, поэтому необходимо указать его при чтении текста ответа:</span><span class="sxs-lookup"><span data-stu-id="9ee1b-137">This formatter is not in the default formatters collection, so you have to specify it when you read the response body:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

<span data-ttu-id="9ee1b-138">В следующем примере показано, как отправить запрос POST, содержащий BSON.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-138">The next example shows how to send a POST request that contains BSON.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

<span data-ttu-id="9ee1b-139">Большая часть кода совпадает со значением предыдущего примера.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-139">Much of this code is the same as the previous example.</span></span> <span data-ttu-id="9ee1b-140">Однако в **PostAsync** метод, укажите **BsonMediaTypeFormatter** как модуль форматирования:</span><span class="sxs-lookup"><span data-stu-id="9ee1b-140">But in the **PostAsync** method, specify **BsonMediaTypeFormatter** as the formatter:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a><span data-ttu-id="9ee1b-141">Сериализация верхнего уровня типы-примитивы</span><span class="sxs-lookup"><span data-stu-id="9ee1b-141">Serializing Top-Level Primitive Types</span></span>

<span data-ttu-id="9ee1b-142">Каждый документ BSON приведен список пар «ключ значение». Спецификация BSON определяет синтаксис для сериализации одного необработанное значение, например целое число или строка.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-142">Every BSON document is a list of key/value pairs.The BSON specification does not define a syntax for serializing a single raw value, such as an integer or string.</span></span>

<span data-ttu-id="9ee1b-143">Чтобы обойти это ограничение, **BsonMediaTypeFormatter** рассматривает как особый случай типов-примитивов.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-143">To work around this limitation, the **BsonMediaTypeFormatter** treats primitive types as a special case.</span></span> <span data-ttu-id="9ee1b-144">До сериализации, он преобразует значение в виде пары ключ значение с ключом «Значение».</span><span class="sxs-lookup"><span data-stu-id="9ee1b-144">Before serializing, it converts the value into a key/value pair with the key "Value".</span></span> <span data-ttu-id="9ee1b-145">Например предположим, что контроллер API возвращает целое число:</span><span class="sxs-lookup"><span data-stu-id="9ee1b-145">For example, suppose your API controller returns an integer:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

<span data-ttu-id="9ee1b-146">Перед началом сериализации, BSON форматирования преобразует этот следующая пара ключ значение:</span><span class="sxs-lookup"><span data-stu-id="9ee1b-146">Before serializing, the BSON formatter converts this to the following key/value pair:</span></span>

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

<span data-ttu-id="9ee1b-147">При попытке десериализации модуль форматирования преобразует данные обратно в исходное значение.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-147">When you deserialize, the formatter converts the data back to the original value.</span></span> <span data-ttu-id="9ee1b-148">Тем не менее клиенты, использующие разные средства синтаксического анализа BSON потребуется этот случай, если веб-API Возвращает необработанные значения.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-148">However, clients using a different BSON parser will need to handle this case, if your web API returns raw values.</span></span> <span data-ttu-id="9ee1b-149">В общем случае следует возврат структурированных данных, а не необработанных значений.</span><span class="sxs-lookup"><span data-stu-id="9ee1b-149">In general, you should consider returning structured data, rather than raw values.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9ee1b-150">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="9ee1b-150">Additional Resources</span></span>

[<span data-ttu-id="9ee1b-151">Веб-API BSON пример</span><span class="sxs-lookup"><span data-stu-id="9ee1b-151">Web API BSON Sample</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[<span data-ttu-id="9ee1b-152">Модули форматирования мультимедиа</span><span class="sxs-lookup"><span data-stu-id="9ee1b-152">Media Formatters</span></span>](media-formatters.md)
