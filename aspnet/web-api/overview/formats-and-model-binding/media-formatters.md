---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Модули форматирования мультимедиа в ASP.NET Web API 2 | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 7b7ba2fb3f1bba0447e700c84a017266cba305e6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829853"
---
<a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="061ae-102">Модули форматирования мультимедиа в ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="061ae-102">Media Formatters in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="061ae-103">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="061ae-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="061ae-104">Этом руководстве показано, как обеспечить поддержку дополнительных носителей форматов в веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="061ae-104">This tutorial shows how to support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="061ae-105">Типов мультимедиа для Интернета</span><span class="sxs-lookup"><span data-stu-id="061ae-105">Internet Media Types</span></span>

<span data-ttu-id="061ae-106">Тип носителя, также называемый MIME-тип, определяет формат фрагмента данных.</span><span class="sxs-lookup"><span data-stu-id="061ae-106">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="061ae-107">В HTTP типы мультимедиа описывают формат тела сообщения.</span><span class="sxs-lookup"><span data-stu-id="061ae-107">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="061ae-108">Тип носителя состоит из двух строк, тип и подтип.</span><span class="sxs-lookup"><span data-stu-id="061ae-108">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="061ae-109">Пример:</span><span class="sxs-lookup"><span data-stu-id="061ae-109">For example:</span></span>

- <span data-ttu-id="061ae-110">text/html</span><span class="sxs-lookup"><span data-stu-id="061ae-110">text/html</span></span>
- <span data-ttu-id="061ae-111">изображение/png</span><span class="sxs-lookup"><span data-stu-id="061ae-111">image/png</span></span>
- <span data-ttu-id="061ae-112">application/json</span><span class="sxs-lookup"><span data-stu-id="061ae-112">application/json</span></span>

<span data-ttu-id="061ae-113">Если сообщение HTTP содержит тело сущности, заголовка Content-Type формат тела сообщения.</span><span class="sxs-lookup"><span data-stu-id="061ae-113">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="061ae-114">Это говорит получателю, как анализировать содержимое тела сообщения.</span><span class="sxs-lookup"><span data-stu-id="061ae-114">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="061ae-115">Например если ответ HTTP содержит изображения в формате PNG, ответ может содержать следующие заголовки.</span><span class="sxs-lookup"><span data-stu-id="061ae-115">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="061ae-116">Когда клиент отправляет сообщение-запрос, он может включать заголовок Accept.</span><span class="sxs-lookup"><span data-stu-id="061ae-116">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="061ae-117">Заголовок Accept говорит, что сервер мультимедиа типы клиент хочет с сервера.</span><span class="sxs-lookup"><span data-stu-id="061ae-117">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="061ae-118">Пример:</span><span class="sxs-lookup"><span data-stu-id="061ae-118">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="061ae-119">Этот заголовок указывает, что сервер, необходимый клиенту HTML, XHTML или XML.</span><span class="sxs-lookup"><span data-stu-id="061ae-119">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="061ae-120">Тип носителя определяет, как веб-API сериализует и десериализует текст сообщения HTTP.</span><span class="sxs-lookup"><span data-stu-id="061ae-120">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="061ae-121">Веб-API имеет встроенную поддержку XML, JSON, BSON и формы urlencoded данных и может поддерживать дополнительные носители, написав *форматирования мультимедиа*.</span><span class="sxs-lookup"><span data-stu-id="061ae-121">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="061ae-122">Чтобы создать модуль форматирования мультимедиа, являются производными от одного из этих классов:</span><span class="sxs-lookup"><span data-stu-id="061ae-122">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="061ae-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="061ae-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="061ae-124">Этот класс использует асинхронное чтение и запись методы.</span><span class="sxs-lookup"><span data-stu-id="061ae-124">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="061ae-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="061ae-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="061ae-126">Этот класс является производным от **MediaTypeFormatter** , но использует sychronous методы чтения и записи.</span><span class="sxs-lookup"><span data-stu-id="061ae-126">This class derives from **MediaTypeFormatter** but uses sychronous read/write methods.</span></span>

<span data-ttu-id="061ae-127">Наследование от **BufferedMediaTypeFormatter** проще использовать, так как нет асинхронного кода, но это также значит, можно блокировать вызывающий поток ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="061ae-127">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="061ae-128">Пример: Создание CSV-ФАЙЛ форматирования мультимедиа</span><span class="sxs-lookup"><span data-stu-id="061ae-128">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="061ae-129">В следующем примере показано форматирования типа мультимедиа, который может сериализовать объект Product в формат с разделителями-запятыми (CSV).</span><span class="sxs-lookup"><span data-stu-id="061ae-129">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="061ae-130">В этом примере используется тип продукта, определенный в этом руководстве [Создание веб-API, поддерживает операции CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span><span class="sxs-lookup"><span data-stu-id="061ae-130">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="061ae-131">Вот определение объекта продукта:</span><span class="sxs-lookup"><span data-stu-id="061ae-131">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="061ae-132">Чтобы реализовать модуль форматирования CSV, определите класс, который является производным от **BufferedMediaTypeFormater**:</span><span class="sxs-lookup"><span data-stu-id="061ae-132">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormater**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="061ae-133">Добавьте в конструктор, типы объектов, которые поддерживает модуль форматирования.</span><span class="sxs-lookup"><span data-stu-id="061ae-133">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="061ae-134">В этом примере модуль форматирования поддерживает устройств одного типа, &quot;text/csv&quot;:</span><span class="sxs-lookup"><span data-stu-id="061ae-134">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="061ae-135">Переопределить **CanWriteType** метод, чтобы указать, какие типы форматирования можно сериализовать:</span><span class="sxs-lookup"><span data-stu-id="061ae-135">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="061ae-136">В этом примере модуль форматирования может сериализовать единый `Product` объекты, а также коллекции `Product` объектов.</span><span class="sxs-lookup"><span data-stu-id="061ae-136">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="061ae-137">Аналогичным образом переопределить **методов CanReadType** метод, чтобы указать, какие типы модуль форматирования может десериализовать.</span><span class="sxs-lookup"><span data-stu-id="061ae-137">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="061ae-138">В этом примере модуль форматирования не поддерживает десериализацию, поэтому метод просто возвращает **false**.</span><span class="sxs-lookup"><span data-stu-id="061ae-138">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="061ae-139">Наконец, переопределение **WriteToStream** метод.</span><span class="sxs-lookup"><span data-stu-id="061ae-139">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="061ae-140">Этот метод сериализует тип с помощью записи в поток.</span><span class="sxs-lookup"><span data-stu-id="061ae-140">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="061ae-141">Если модуль форматирования поддерживает десериализацию, также переопределить **ReadFromStream** метод.</span><span class="sxs-lookup"><span data-stu-id="061ae-141">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="061ae-142">Добавление форматирования мультимедиа в конвейер веб-API</span><span class="sxs-lookup"><span data-stu-id="061ae-142">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="061ae-143">Чтобы добавить тип мультимедиа модуля форматирования в конвейер веб-API, используйте **модули форматирования** свойство **HttpConfiguration** объекта.</span><span class="sxs-lookup"><span data-stu-id="061ae-143">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="061ae-144">Кодировки символов</span><span class="sxs-lookup"><span data-stu-id="061ae-144">Character Encodings</span></span>

<span data-ttu-id="061ae-145">Кроме того модуль форматирования мультимедиа может поддерживать несколько кодировок, например UTF-8 или ISO-8859-1.</span><span class="sxs-lookup"><span data-stu-id="061ae-145">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="061ae-146">В конструкторе, добавьте один или несколько [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) типов **SupportedEncodings** коллекции.</span><span class="sxs-lookup"><span data-stu-id="061ae-146">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="061ae-147">Поместите первый кодировку по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="061ae-147">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="061ae-148">В **WriteToStream** и **ReadFromStream** вызывать методы, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) выберите предпочитаемую кодировку символов.</span><span class="sxs-lookup"><span data-stu-id="061ae-148">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="061ae-149">Этот метод соответствует заголовки запроса со списком поддерживаемых кодировок.</span><span class="sxs-lookup"><span data-stu-id="061ae-149">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="061ae-150">Используйте возвращенный **кодировка** при чтении или записи из потока:</span><span class="sxs-lookup"><span data-stu-id="061ae-150">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
