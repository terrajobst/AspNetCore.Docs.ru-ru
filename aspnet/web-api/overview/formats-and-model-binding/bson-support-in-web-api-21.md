---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Поддержка BSON в веб-API 2.1 ASP.NET | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 01/20/2014
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 7b9a0c2def4703f6fad65790dfe4227117189933
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839886"
---
<a name="bson-support-in-aspnet-web-api-21"></a><span data-ttu-id="bb689-102">Поддержка BSON в веб-API 2.1 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bb689-102">BSON Support in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="bb689-103">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bb689-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="bb689-104">Веб-API 2.1 введена поддержка BSON.</span><span class="sxs-lookup"><span data-stu-id="bb689-104">Web API 2.1 introduces support for BSON.</span></span> <span data-ttu-id="bb689-105">В этом разделе показано, как использовать BSON в контроллере веб-API (на стороне сервера) и в клиентском приложении .NET.</span><span class="sxs-lookup"><span data-stu-id="bb689-105">This topic shows how to use BSON in your Web API controller (server side) and in a .NET client app.</span></span>

## <a name="what-is-bson"></a><span data-ttu-id="bb689-106">Что такое BSON?</span><span class="sxs-lookup"><span data-stu-id="bb689-106">What is BSON?</span></span>

<span data-ttu-id="bb689-107">[BSON](http://bsonspec.org/) — это формат двоичной сериализации.</span><span class="sxs-lookup"><span data-stu-id="bb689-107">[BSON](http://bsonspec.org/) is a binary serialization format.</span></span> <span data-ttu-id="bb689-108">«BSON» означает «Двоичный JSON», но BSON и JSON сериализуются совсем по-другому.</span><span class="sxs-lookup"><span data-stu-id="bb689-108">"BSON" stands for "Binary JSON", but BSON and JSON are serialized very differently.</span></span> <span data-ttu-id="bb689-109">BSON — «Похожих на JSON-», так как объекты отображаются в виде пары имя значение, аналогичную JSON.</span><span class="sxs-lookup"><span data-stu-id="bb689-109">BSON is "JSON-like", because objects are represented as name-value pairs, similar to JSON.</span></span> <span data-ttu-id="bb689-110">В отличие от JSON числовых типов данных, хранятся в виде байтов, не строки</span><span class="sxs-lookup"><span data-stu-id="bb689-110">Unlike JSON, numeric data types are stored as bytes, not strings</span></span>

<span data-ttu-id="bb689-111">BSON была разработана как упрощенная, легко просматривать и быстро для шифрования или расшифровки.</span><span class="sxs-lookup"><span data-stu-id="bb689-111">BSON was designed to be lightweight, easy to scan, and fast to encode/decode.</span></span>

- <span data-ttu-id="bb689-112">BSON можно сравнить размер, в формат JSON.</span><span class="sxs-lookup"><span data-stu-id="bb689-112">BSON is comparable in size to JSON.</span></span> <span data-ttu-id="bb689-113">В зависимости от данных полезные данные BSON могут занять меньше или больше, чем полезные данные JSON.</span><span class="sxs-lookup"><span data-stu-id="bb689-113">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="bb689-114">Для сериализации двоичных данных, таких как файл изображения, BSON меньше, чем JSON, так как двоичные данные не кодировке base64.</span><span class="sxs-lookup"><span data-stu-id="bb689-114">For serializing binary data, such as an image file, BSON is smaller than JSON, because the binary data is not base64-encoded.</span></span>
- <span data-ttu-id="bb689-115">BSON документы являются удобной для быстрого просмотра, так как элементы должны иметь префикс длины поля, средство синтаксического анализа можно пропустить элементы без их декодирование.</span><span class="sxs-lookup"><span data-stu-id="bb689-115">BSON documents are easy to scan because elements are prefixed with a length field, so a parser can skip elements without decoding them.</span></span>
- <span data-ttu-id="bb689-116">Эффективны, так как числовые типы данных хранятся в виде числа, строки не кодирования и декодирования.</span><span class="sxs-lookup"><span data-stu-id="bb689-116">Encoding and decoding are efficient, because numeric data types are stored as numbers, not strings.</span></span>

<span data-ttu-id="bb689-117">Собственные клиенты, такие как клиентские приложения .NET, можно использовать преимущества BSON вместо текстовых форматов, например JSON или XML.</span><span class="sxs-lookup"><span data-stu-id="bb689-117">Native clients, such as .NET client apps, can benefit from using BSON in place of text-based formats such as JSON or XML.</span></span> <span data-ttu-id="bb689-118">Для обозревателя клиента возможно, необходимо отказаться от использования JSON, так как JavaScript, напрямую, можно преобразовать в полезных данных JSON.</span><span class="sxs-lookup"><span data-stu-id="bb689-118">For browser clients, you will probably want to stick with JSON, because JavaScript can directly convert the JSON payload.</span></span>

<span data-ttu-id="bb689-119">К счастью, веб-API использует [согласования содержимого](content-negotiation.md), поэтому можно поддерживать оба формата и дать клиенту выбрать API.</span><span class="sxs-lookup"><span data-stu-id="bb689-119">Fortunately, Web API uses [content negotiation](content-negotiation.md), so your API can support both formats and let the client choose.</span></span>

## <a name="enabling-bson-on-the-server"></a><span data-ttu-id="bb689-120">Включение BSON на сервере</span><span class="sxs-lookup"><span data-stu-id="bb689-120">Enabling BSON on the Server</span></span>

<span data-ttu-id="bb689-121">Добавьте в конфигурацию веб-API, **BsonMediaTypeFormatter** в коллекцию модулей форматирования.</span><span class="sxs-lookup"><span data-stu-id="bb689-121">In your Web API configuration, add the **BsonMediaTypeFormatter** to the formatters collection.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

<span data-ttu-id="bb689-122">Теперь если клиент запрашивает «application/bson», веб-API будет использовать модуль форматирования BSON.</span><span class="sxs-lookup"><span data-stu-id="bb689-122">Now if the client requests "application/bson", Web API will use the BSON formatter.</span></span>

<span data-ttu-id="bb689-123">Чтобы связать BSON с другими типами мультимедиа, их необходимо добавьте в коллекцию SupportedMediaTypes.</span><span class="sxs-lookup"><span data-stu-id="bb689-123">To associate BSON with other media types, add them to the SupportedMediaTypes collection.</span></span> <span data-ttu-id="bb689-124">Следующий код добавляет «application/vnd.contoso» поддерживаемые типы носителей:</span><span class="sxs-lookup"><span data-stu-id="bb689-124">The following code adds "application/vnd.contoso" to the supported media types:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a><span data-ttu-id="bb689-125">Пример HTTP-сеанса</span><span class="sxs-lookup"><span data-stu-id="bb689-125">Example HTTP Session</span></span>

<span data-ttu-id="bb689-126">В этом примере мы будем использовать класс модели, а также простой контроллер веб-API:</span><span class="sxs-lookup"><span data-stu-id="bb689-126">For this example, we'll use the following model class plus a simple Web API controller:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

<span data-ttu-id="bb689-127">Клиент может отправить следующий запрос HTTP:</span><span class="sxs-lookup"><span data-stu-id="bb689-127">A client might send the following HTTP request:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

<span data-ttu-id="bb689-128">Ниже представлен ответ:</span><span class="sxs-lookup"><span data-stu-id="bb689-128">Here is the response:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

<span data-ttu-id="bb689-129">Здесь я заменил двоичные данные с &quot;.&quot; символов.</span><span class="sxs-lookup"><span data-stu-id="bb689-129">Here I've replaced the binary data with &quot;.&quot; characters.</span></span> <span data-ttu-id="bb689-130">На следующем снимке экрана из Fiddler показаны необработанные шестнадцатеричные значения.</span><span class="sxs-lookup"><span data-stu-id="bb689-130">The following screen shot from Fiddler shows the raw hex values.</span></span>

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a><span data-ttu-id="bb689-131">С помощью BSON с HttpClient</span><span class="sxs-lookup"><span data-stu-id="bb689-131">Using BSON with HttpClient</span></span>

<span data-ttu-id="bb689-132">Приложения клиенты .NET могут использовать модуль форматирования BSON **HttpClient**.</span><span class="sxs-lookup"><span data-stu-id="bb689-132">.NET clients apps can use the BSON formatter with **HttpClient**.</span></span> <span data-ttu-id="bb689-133">Дополнительные сведения о **HttpClient**, см. в разделе [вызова Web API из клиента .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="bb689-133">For more information about **HttpClient**, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="bb689-134">Следующий код отправляет запрос GET, который принимает BSON и затем десериализует полезные данные BSON в ответе.</span><span class="sxs-lookup"><span data-stu-id="bb689-134">The following code sends a GET request that accepts BSON, and then deserializes the BSON payload in the response.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

<span data-ttu-id="bb689-135">Чтобы запросить BSON с сервера, задайте заголовок Accept для «application/bson»:</span><span class="sxs-lookup"><span data-stu-id="bb689-135">To request BSON from the server, set the Accept header to "application/bson":</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

<span data-ttu-id="bb689-136">Чтобы десериализовать текст ответа, используйте **BsonMediaTypeFormatter**.</span><span class="sxs-lookup"><span data-stu-id="bb689-136">To deserialize the response body, use the **BsonMediaTypeFormatter**.</span></span> <span data-ttu-id="bb689-137">Этот модуль форматирования не в коллекции модулей форматирования по умолчанию, поэтому вам нужно будет указать его при чтении текста ответа:</span><span class="sxs-lookup"><span data-stu-id="bb689-137">This formatter is not in the default formatters collection, so you have to specify it when you read the response body:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

<span data-ttu-id="bb689-138">Далее примере показано, как отправлять запрос POST, содержащий BSON.</span><span class="sxs-lookup"><span data-stu-id="bb689-138">The next example shows how to send a POST request that contains BSON.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

<span data-ttu-id="bb689-139">Большая часть этого кода совпадает со значением предыдущего примера.</span><span class="sxs-lookup"><span data-stu-id="bb689-139">Much of this code is the same as the previous example.</span></span> <span data-ttu-id="bb689-140">Однако в **PostAsync** метод, укажите **BsonMediaTypeFormatter** как модуль форматирования:</span><span class="sxs-lookup"><span data-stu-id="bb689-140">But in the **PostAsync** method, specify **BsonMediaTypeFormatter** as the formatter:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a><span data-ttu-id="bb689-141">Сериализация верхнего уровня типов-примитивов</span><span class="sxs-lookup"><span data-stu-id="bb689-141">Serializing Top-Level Primitive Types</span></span>

<span data-ttu-id="bb689-142">Каждый документ BSON приведен список пар "ключ значение". Спецификация BSON определяет синтаксис для сериализации необработанные единственное значение, например целое число или строка.</span><span class="sxs-lookup"><span data-stu-id="bb689-142">Every BSON document is a list of key/value pairs.The BSON specification does not define a syntax for serializing a single raw value, such as an integer or string.</span></span>

<span data-ttu-id="bb689-143">Чтобы обойти это ограничение, **BsonMediaTypeFormatter** рассматривает типы-примитивы как особый случай.</span><span class="sxs-lookup"><span data-stu-id="bb689-143">To work around this limitation, the **BsonMediaTypeFormatter** treats primitive types as a special case.</span></span> <span data-ttu-id="bb689-144">Перед сериализацией, он преобразует значение в паре ключ/значение с ключом «Value».</span><span class="sxs-lookup"><span data-stu-id="bb689-144">Before serializing, it converts the value into a key/value pair with the key "Value".</span></span> <span data-ttu-id="bb689-145">Например предположим, что контроллер API возвращает целое число:</span><span class="sxs-lookup"><span data-stu-id="bb689-145">For example, suppose your API controller returns an integer:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

<span data-ttu-id="bb689-146">Перед сериализацией, модуль форматирования BSON преобразует это следующую пару "ключ значение":</span><span class="sxs-lookup"><span data-stu-id="bb689-146">Before serializing, the BSON formatter converts this to the following key/value pair:</span></span>

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

<span data-ttu-id="bb689-147">При десериализации, модуль форматирования преобразует данные обратно в исходное значение.</span><span class="sxs-lookup"><span data-stu-id="bb689-147">When you deserialize, the formatter converts the data back to the original value.</span></span> <span data-ttu-id="bb689-148">Тем не менее клиенты, использующие разные средства синтаксического анализа BSON потребуется в этом случае веб-API возвращает исходные значения.</span><span class="sxs-lookup"><span data-stu-id="bb689-148">However, clients using a different BSON parser will need to handle this case, if your web API returns raw values.</span></span> <span data-ttu-id="bb689-149">В общем случае следует рассмотрите возможность возврата структурированных данных, а не исходные значения.</span><span class="sxs-lookup"><span data-stu-id="bb689-149">In general, you should consider returning structured data, rather than raw values.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bb689-150">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="bb689-150">Additional Resources</span></span>

[<span data-ttu-id="bb689-151">Веб-API BSON пример</span><span class="sxs-lookup"><span data-stu-id="bb689-151">Web API BSON Sample</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[<span data-ttu-id="bb689-152">Модули форматирования мультимедиа</span><span class="sxs-lookup"><span data-stu-id="bb689-152">Media Formatters</span></span>](media-formatters.md)
