---
title: Операции запросов и ответов в ASP.NET Core
author: jkotalik
description: Узнайте, как читать текст запроса и писать текст ответа в ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jukotali
ms.custom: mvc
ms.date: 02/26/2019
uid: fundamentals/middleware/request-response
ms.openlocfilehash: 0c321dad256e239b61907980c09d2c088c1407ff
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538577"
---
# <a name="request-and-response-operations-in-aspnet-core"></a><span data-ttu-id="ed47a-103">Операции запросов и ответов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ed47a-103">Request and response operations in ASP.NET Core</span></span>

<span data-ttu-id="ed47a-104">Автор [Джастин Коталик (Justin Kotalik)](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="ed47a-104">By [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="ed47a-105">В этой статье объясняется, как читать текст запроса и писать текст ответа.</span><span class="sxs-lookup"><span data-stu-id="ed47a-105">This article explains how to read from the request body and write to the response body.</span></span> <span data-ttu-id="ed47a-106">Возможно, вам потребуется написать код для этих операций при создании ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="ed47a-106">You might need to write code for these operations when you're writing middleware.</span></span> <span data-ttu-id="ed47a-107">В других случаях писать такой код обычно не нужно, так как эти операции обрабатывают MVC и Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="ed47a-107">Otherwise, you typically don't have to write this code because the operations are handled by MVC and Razor Pages.</span></span>

<span data-ttu-id="ed47a-108">В ASP.NET Core 3.0 есть две абстракции для текста запросов и ответов: <xref:System.IO.Stream> и <xref:System.IO.Pipelines.Pipe>.</span><span class="sxs-lookup"><span data-stu-id="ed47a-108">In ASP.NET Core 3.0, there are two abstractions for the request and response bodies: <xref:System.IO.Stream> and <xref:System.IO.Pipelines.Pipe>.</span></span> <span data-ttu-id="ed47a-109">При чтении запроса [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) — это <xref:System.IO.Stream>, а `HttpRequest.BodyPipe` — это <xref:System.IO.Pipelines.PipeReader>.</span><span class="sxs-lookup"><span data-stu-id="ed47a-109">For request reading, [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) is a <xref:System.IO.Stream>, and `HttpRequest.BodyPipe` is a <xref:System.IO.Pipelines.PipeReader>.</span></span> <span data-ttu-id="ed47a-110">При записи ответа [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) — это `HttpResponse.BodyPipe` и <xref:System.IO.Pipelines.PipeWriter>.</span><span class="sxs-lookup"><span data-stu-id="ed47a-110">For response writing, [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) is a `HttpResponse.BodyPipe` is a <xref:System.IO.Pipelines.PipeWriter>.</span></span>

<span data-ttu-id="ed47a-111">Мы рекомендуем использовать конвейеры, а не потоки.</span><span class="sxs-lookup"><span data-stu-id="ed47a-111">We recommend pipelines over streams.</span></span> <span data-ttu-id="ed47a-112">Потоки удобнее использовать для некоторых простых операций, но производительность конвейеров выше и с ними проще работать в большинстве сценариев.</span><span class="sxs-lookup"><span data-stu-id="ed47a-112">Streams can be easier to use for some simple operations, but pipelines have a performance advantage and are easier to use in most scenarios.</span></span> <span data-ttu-id="ed47a-113">Начиная с ASP.NET Core 3.0 преимущество отдается внутреннему использованию конвейеров вместо потоков.</span><span class="sxs-lookup"><span data-stu-id="ed47a-113">In 3.0, ASP.NET Core is starting to use pipelines instead of streams internally.</span></span> <span data-ttu-id="ed47a-114">Примеры:</span><span class="sxs-lookup"><span data-stu-id="ed47a-114">Examples include:</span></span>

- `FormReader`
- `TextReader`
- `TexWriter`
- `HttpResponse.WriteAsync`

<span data-ttu-id="ed47a-115">Потоки по-прежнему используются.</span><span class="sxs-lookup"><span data-stu-id="ed47a-115">Streams aren't going away.</span></span> <span data-ttu-id="ed47a-116">С ними продолжают работать в .NET, и многие типы потоков не имеют эквивалентного конвейера, например `FileStreams` и `ResponseCompression`.</span><span class="sxs-lookup"><span data-stu-id="ed47a-116">They continue to be used throughout .NET, and many stream types don't have pipe equivalents, like `FileStreams` and `ResponseCompression`.</span></span>

## <a name="stream-examples"></a><span data-ttu-id="ed47a-117">Примеры потоков</span><span class="sxs-lookup"><span data-stu-id="ed47a-117">Stream examples</span></span>

<span data-ttu-id="ed47a-118">Предположим, вам необходимо создать ПО промежуточного слоя, которое считывает весь текст запроса как список строк с разделением на новые строки.</span><span class="sxs-lookup"><span data-stu-id="ed47a-118">Suppose you want to create a middleware that reads the entire request body as a list of strings, splitting on new lines.</span></span> <span data-ttu-id="ed47a-119">Реализация простого потока может выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ed47a-119">A simple stream implementation might look like the following example:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]

<span data-ttu-id="ed47a-120">Этот код работает, но есть определенные проблемы:</span><span class="sxs-lookup"><span data-stu-id="ed47a-120">This code works, but there are some issues:</span></span>

- <span data-ttu-id="ed47a-121">Перед добавлением `StringBuilder` в коде создается другая строка (`encodedString`), которая сразу отбрасывается.</span><span class="sxs-lookup"><span data-stu-id="ed47a-121">Before appending to the `StringBuilder`, the example creates another string (`encodedString`) that is thrown away immediately.</span></span> <span data-ttu-id="ed47a-122">Этот процесс выполняется для всех байтов в потоке, и в результате выделяется дополнительный объем памяти для всего текста запроса.</span><span class="sxs-lookup"><span data-stu-id="ed47a-122">This process occurs for all bytes in the stream, so the result is extra memory allocation the size of the entire request body.</span></span>
- <span data-ttu-id="ed47a-123">Код считывает всю строку перед разделением на новые строки.</span><span class="sxs-lookup"><span data-stu-id="ed47a-123">The example reads the entire string before splitting on new lines.</span></span> <span data-ttu-id="ed47a-124">Было бы эффективнее искать новые строки в массиве байтов.</span><span class="sxs-lookup"><span data-stu-id="ed47a-124">It would be more efficient to check for new lines in the byte array.</span></span>

<span data-ttu-id="ed47a-125">Ниже приведен пример, в котором устранены некоторые из этих проблем.</span><span class="sxs-lookup"><span data-stu-id="ed47a-125">Here's an example that fixes some of these issues:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

<span data-ttu-id="ed47a-126">В этом примере:</span><span class="sxs-lookup"><span data-stu-id="ed47a-126">This example:</span></span>

- <span data-ttu-id="ed47a-127">Не создается буфер для всего текста запроса в `StringBuilder`, если нет символов новой строки.</span><span class="sxs-lookup"><span data-stu-id="ed47a-127">Doesn't buffer the entire request body in a `StringBuilder` unless there aren't any newline characters.</span></span>
- <span data-ttu-id="ed47a-128">В строке не вызывается `Split`.</span><span class="sxs-lookup"><span data-stu-id="ed47a-128">Doesn't call `Split` on the string.</span></span>

<span data-ttu-id="ed47a-129">Но некоторые проблемы при этом остаются:</span><span class="sxs-lookup"><span data-stu-id="ed47a-129">However, there are still are a few issues:</span></span>

- <span data-ttu-id="ed47a-130">Если символы новой строки разрежены, большая часть текста запроса буферизуется в строке.</span><span class="sxs-lookup"><span data-stu-id="ed47a-130">If newline characters are sparse, much of the request body is buffered in the string .</span></span>
- <span data-ttu-id="ed47a-131">Строки (`remainingString`) по-прежнему создаются и добавляются в буфер строки, что приводит к выделению дополнительной памяти.</span><span class="sxs-lookup"><span data-stu-id="ed47a-131">It still creates strings (`remainingString`) and adds them to the string buffer, which results in an extra allocation.</span></span>

<span data-ttu-id="ed47a-132">Эти проблемы можно решить, но ценой усложнения кода при незначительных его улучшениях.</span><span class="sxs-lookup"><span data-stu-id="ed47a-132">These issues are fixable, but the code is becoming more and more complicated with little improvement.</span></span> <span data-ttu-id="ed47a-133">Конвейеры позволяют решить эти проблемы при минимальной сложности кода.</span><span class="sxs-lookup"><span data-stu-id="ed47a-133">Pipelines provide a way to solve these problems with minimal code complexity.</span></span>

## <a name="pipelines"></a><span data-ttu-id="ed47a-134">Конвейеры</span><span class="sxs-lookup"><span data-stu-id="ed47a-134">Pipelines</span></span>

<span data-ttu-id="ed47a-135">В следующем примере показано, как тот же сценарий можно обработать с помощью `PipeReader`.</span><span class="sxs-lookup"><span data-stu-id="ed47a-135">The following example shows how the same scenario can be handled using a `PipeReader`:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

<span data-ttu-id="ed47a-136">В этом примере устранены многие проблемы, характерные для реализации посредством потоков.</span><span class="sxs-lookup"><span data-stu-id="ed47a-136">This example fixes many issues that the streams implementations had:</span></span>

- <span data-ttu-id="ed47a-137">В буфере строки теперь нет необходимости, так как `PipeReader` обрабатывает байты, которые не использовались.</span><span class="sxs-lookup"><span data-stu-id="ed47a-137">There is no need for a string buffer because the `PipeReader` handles bytes that haven't been used.</span></span>
- <span data-ttu-id="ed47a-138">Кодированные строки напрямую добавляются в список возвращенных строк.</span><span class="sxs-lookup"><span data-stu-id="ed47a-138">Encoded strings are directly added to the list of returned strings.</span></span>
- <span data-ttu-id="ed47a-139">Создание строк не связано с выделением памяти, кроме памяти, используемой строкой (за исключением вызова `ToArray()`).</span><span class="sxs-lookup"><span data-stu-id="ed47a-139">String creation is allocation-free besides the memory used by the string (except the `ToArray()` call).</span></span>

## <a name="adapters"></a><span data-ttu-id="ed47a-140">Адаптеры</span><span class="sxs-lookup"><span data-stu-id="ed47a-140">Adapters</span></span>

<span data-ttu-id="ed47a-141">Теперь, когда свойства `Body` и `BodyPipe` доступны для `HttpRequest` и `HttpResponse`, что произойдет, если назначить `Body` другому потоку?</span><span class="sxs-lookup"><span data-stu-id="ed47a-141">Now that both `Body` and `BodyPipe` properties are available for `HttpRequest` and `HttpResponse`, what happens when you set `Body` to a different stream?</span></span> <span data-ttu-id="ed47a-142">В версии 3.0 новый набор адаптеров автоматически адаптирует каждый тип к другому.</span><span class="sxs-lookup"><span data-stu-id="ed47a-142">In 3.0, a new set of adapters automatically adapt each type to the other.</span></span> <span data-ttu-id="ed47a-143">Например, если назначить `HttpRequest.Body` новому потоку, `HttpRequest.BodyPipe` автоматически назначается новому `PipeReader`, который создает оболочку для `HttpRequest.Body`.</span><span class="sxs-lookup"><span data-stu-id="ed47a-143">For example, if you set `HttpRequest.Body` to a new stream, `HttpRequest.BodyPipe` is automatically set to a new `PipeReader` that wraps `HttpRequest.Body`.</span></span> <span data-ttu-id="ed47a-144">То же относится и к заданию свойства `BodyPipe`.</span><span class="sxs-lookup"><span data-stu-id="ed47a-144">The same behavior applies to setting the `BodyPipe` property.</span></span> <span data-ttu-id="ed47a-145">Если `HttpResponse.BodyPipe` назначить новому `PipeWriter`, `HttpResponse.Body` автоматически назначается новому потоку, который создает оболочку для `HttpResponse.BodyPipe`.</span><span class="sxs-lookup"><span data-stu-id="ed47a-145">If `HttpResponse.BodyPipe` is set to a new `PipeWriter`, the `HttpResponse.Body` is automatically set to a new stream that wraps `HttpResponse.BodyPipe`.</span></span>

## <a name="startasync"></a><span data-ttu-id="ed47a-146">StartAsync</span><span class="sxs-lookup"><span data-stu-id="ed47a-146">StartAsync</span></span>

<span data-ttu-id="ed47a-147">Метод `HttpResponse.StartAsync` впервые появился в версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="ed47a-147">`HttpResponse.StartAsync` is new in 3.0.</span></span> <span data-ttu-id="ed47a-148">Он используется для указания того, что заголовки являются неизменяемыми, а также для запуска обратных вызовов `OnStarting`.</span><span class="sxs-lookup"><span data-stu-id="ed47a-148">It is used to indicate that headers are unmodifiable and to run `OnStarting` callbacks.</span></span> <span data-ttu-id="ed47a-149">В предварительной версии 3.0, необходимо вызвать `StartAsync` перед использованием `HttpRequest.BodyPipe`. В последующих выпусках это станет рекомендацией.</span><span class="sxs-lookup"><span data-stu-id="ed47a-149">In 3.0-preview3, you have to call `StartAsync` before using `HttpRequest.BodyPipe`, and in future releases, it will be a recommendation.</span></span> <span data-ttu-id="ed47a-150">При использования Kestrel в качестве сервера вызов StartAsync перед применением `PipeReader` гарантирует, что память, возвращенная `GetMemory`, относится к внутреннему методу <xref:System.IO.Pipelines.Pipe> Kestrel, а не к внешнему буферу.</span><span class="sxs-lookup"><span data-stu-id="ed47a-150">When using Kestrel as a server, calling StartAsync before using the `PipeReader` guarantees that memory returned by `GetMemory` will belong to Kestrel's internal <xref:System.IO.Pipelines.Pipe> rather than an external buffer.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ed47a-151">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ed47a-151">Additional resources</span></span>

- [<span data-ttu-id="ed47a-152">Ознакомительная статья о библиотеке System.IO.Pipelines</span><span class="sxs-lookup"><span data-stu-id="ed47a-152">Introducing System.IO.Pipelines</span></span>](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/)
- <xref:fundamentals/middleware/write>