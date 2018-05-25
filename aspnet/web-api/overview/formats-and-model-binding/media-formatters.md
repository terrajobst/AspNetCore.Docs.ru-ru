---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Модули форматирования мультимедиа в ASP.NET Web API 2 | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 1cb1c7e0f832a0a0160276fbd41facc017e2ae3e
ms.sourcegitcommit: 50d40c83fa641d283c097f986dde5341ebe1b44c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/22/2018
---
<a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="d75f6-102">Модули форматирования мультимедиа в ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="d75f6-102">Media Formatters in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="d75f6-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d75f6-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="d75f6-104">Этот учебник демонстрирует поддерживают дополнительные мультимедийные форматы в веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d75f6-104">This tutorial shows how to support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="d75f6-105">Типы носителей Интернета</span><span class="sxs-lookup"><span data-stu-id="d75f6-105">Internet Media Types</span></span>

<span data-ttu-id="d75f6-106">Тип носителя, также называемый MIME-тип, определяет формат фрагмента данных.</span><span class="sxs-lookup"><span data-stu-id="d75f6-106">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="d75f6-107">В HTTP типы носителей описания формата текста сообщения.</span><span class="sxs-lookup"><span data-stu-id="d75f6-107">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="d75f6-108">Тип носителя состоит из двух строк, тип и подтип.</span><span class="sxs-lookup"><span data-stu-id="d75f6-108">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="d75f6-109">Пример:</span><span class="sxs-lookup"><span data-stu-id="d75f6-109">For example:</span></span>

- <span data-ttu-id="d75f6-110">text/html</span><span class="sxs-lookup"><span data-stu-id="d75f6-110">text/html</span></span>
- <span data-ttu-id="d75f6-111">изображение или png</span><span class="sxs-lookup"><span data-stu-id="d75f6-111">image/png</span></span>
- <span data-ttu-id="d75f6-112">application/json</span><span class="sxs-lookup"><span data-stu-id="d75f6-112">application/json</span></span>

<span data-ttu-id="d75f6-113">Если сообщение HTTP содержит тело сущности, заголовка Content-Type указывает формат текста сообщения.</span><span class="sxs-lookup"><span data-stu-id="d75f6-113">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="d75f6-114">Узнает получатель синтаксического анализа содержимого тела сообщения.</span><span class="sxs-lookup"><span data-stu-id="d75f6-114">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="d75f6-115">Например если HTTP-ответа содержит изображения в формате PNG, ответ могут возникнуть следующие заголовки.</span><span class="sxs-lookup"><span data-stu-id="d75f6-115">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="d75f6-116">Когда клиент отправляет сообщение-запрос, может включать заголовок Accept.</span><span class="sxs-lookup"><span data-stu-id="d75f6-116">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="d75f6-117">Заголовок Accept сообщает, что сервер мультимедиа типы клиент хочет с сервера.</span><span class="sxs-lookup"><span data-stu-id="d75f6-117">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="d75f6-118">Пример:</span><span class="sxs-lookup"><span data-stu-id="d75f6-118">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="d75f6-119">Этот заголовок указывает, что сервер, что клиенту требуется HTML, XHTML или XML.</span><span class="sxs-lookup"><span data-stu-id="d75f6-119">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="d75f6-120">Тип носителя определяет, как веб-API сериализует и десериализует тело сообщения HTTP.</span><span class="sxs-lookup"><span data-stu-id="d75f6-120">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="d75f6-121">Веб-API имеет встроенную поддержку XML, JSON, BSON и формы urlencoded данных и может поддерживать дополнительные носители, написав *форматирования мультимедиа*.</span><span class="sxs-lookup"><span data-stu-id="d75f6-121">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="d75f6-122">Чтобы создать модуль форматирования мультимедиа, являются производными от одного из этих классов:</span><span class="sxs-lookup"><span data-stu-id="d75f6-122">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="d75f6-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="d75f6-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="d75f6-124">Этот класс использует асинхронное чтение и запись методы.</span><span class="sxs-lookup"><span data-stu-id="d75f6-124">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="d75f6-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="d75f6-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="d75f6-126">Этот класс является производным от **MediaTypeFormatter** , но использует sychronous методы чтения и записи.</span><span class="sxs-lookup"><span data-stu-id="d75f6-126">This class derives from **MediaTypeFormatter** but uses sychronous read/write methods.</span></span>

<span data-ttu-id="d75f6-127">Наследование от **BufferedMediaTypeFormatter** проще, так как нет асинхронного кода, но это также означает, что вызывающий поток можно заблокировать во время ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="d75f6-127">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="d75f6-128">Пример: Создание модуль форматирования мультимедиа CSV</span><span class="sxs-lookup"><span data-stu-id="d75f6-128">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="d75f6-129">В следующем примере показано форматирования типа мультимедиа, который может сериализовать объект продукта в формате с разделителями-запятыми (CSV).</span><span class="sxs-lookup"><span data-stu-id="d75f6-129">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="d75f6-130">В этом примере используется тип продукта, определенные в этом учебнике [Создание веб-API, поддерживает операции CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span><span class="sxs-lookup"><span data-stu-id="d75f6-130">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="d75f6-131">Вот определение объекта продукта.</span><span class="sxs-lookup"><span data-stu-id="d75f6-131">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="d75f6-132">Чтобы реализовать модуль форматирования CSV, определите класс, производный от **BufferedMediaTypeFormater**:</span><span class="sxs-lookup"><span data-stu-id="d75f6-132">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormater**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="d75f6-133">В конструкторе добавьте типами мультимедиа, которые поддерживает модуль форматирования.</span><span class="sxs-lookup"><span data-stu-id="d75f6-133">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="d75f6-134">В этом примере модуль форматирования поддерживает устройств одного типа &quot;текст/csv&quot;:</span><span class="sxs-lookup"><span data-stu-id="d75f6-134">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="d75f6-135">Переопределить **CanWriteType** можно сериализовать метод, чтобы указать, какие типы форматирования:</span><span class="sxs-lookup"><span data-stu-id="d75f6-135">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="d75f6-136">В этом примере можно сериализовать один модуль форматирования `Product` объектов и коллекций `Product` объектов.</span><span class="sxs-lookup"><span data-stu-id="d75f6-136">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="d75f6-137">Аналогичным образом переопределить **CanReadType** метод, чтобы указать, какие типы форматирования может десериализовать.</span><span class="sxs-lookup"><span data-stu-id="d75f6-137">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="d75f6-138">В этом примере модуль форматирования не поддерживает десериализации, поэтому метод просто возвращает **false**.</span><span class="sxs-lookup"><span data-stu-id="d75f6-138">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="d75f6-139">Наконец, переопределения **WriteToStream** метод.</span><span class="sxs-lookup"><span data-stu-id="d75f6-139">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="d75f6-140">Этот метод сериализует тип путем записи в поток.</span><span class="sxs-lookup"><span data-stu-id="d75f6-140">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="d75f6-141">Если модуль форматирования поддерживает десериализации, также переопределить **ReadFromStream** метод.</span><span class="sxs-lookup"><span data-stu-id="d75f6-141">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="d75f6-142">Добавление форматирования мультимедиа в конвейер веб-API</span><span class="sxs-lookup"><span data-stu-id="d75f6-142">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="d75f6-143">Чтобы добавить тип мультимедиа модуля форматирования в конвейер веб-API, используйте **модули форматирования** свойство **HttpConfiguration** объекта.</span><span class="sxs-lookup"><span data-stu-id="d75f6-143">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="d75f6-144">Кодировки символов</span><span class="sxs-lookup"><span data-stu-id="d75f6-144">Character Encodings</span></span>

<span data-ttu-id="d75f6-145">При необходимости форматирования мультимедиа может поддерживать несколько кодировок, например UTF-8 или ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="d75f6-145">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="d75f6-146">В конструкторе, добавьте один или несколько [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) типы **SupportedEncodings** коллекции.</span><span class="sxs-lookup"><span data-stu-id="d75f6-146">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="d75f6-147">Поместите первый кодировку по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d75f6-147">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="d75f6-148">В **WriteToStream** и **ReadFromStream** вызывать методы, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) для выбора предпочтительной кодировки.</span><span class="sxs-lookup"><span data-stu-id="d75f6-148">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="d75f6-149">Этот метод совпадает со списком поддерживаемых кодировок заголовки запроса.</span><span class="sxs-lookup"><span data-stu-id="d75f6-149">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="d75f6-150">Используйте возвращенный **кодировка** при чтении или записи из потока:</span><span class="sxs-lookup"><span data-stu-id="d75f6-150">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
