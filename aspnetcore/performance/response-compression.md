---
title: Сжатие ответов в ASP.NET Core
author: guardrex
description: Сведения о сжатии откликов и способах использования ПО промежуточного слоя для сжатия откликов в приложениях ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
uid: performance/response-compression
ms.openlocfilehash: b8a84418a3258e9ac43b4eadd8564c0708590bce
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726962"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="6317e-103">Сжатие ответов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6317e-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="6317e-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="6317e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6317e-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6317e-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6317e-106">Пропускная способность сети является ограниченным ресурсом.</span><span class="sxs-lookup"><span data-stu-id="6317e-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="6317e-107">Уменьшение размера ответа обычно значительно увеличивает скорость реагирования приложения.</span><span class="sxs-lookup"><span data-stu-id="6317e-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="6317e-108">Одним из способов уменьшения размера полезных данных является сжатие ответов приложения.</span><span class="sxs-lookup"><span data-stu-id="6317e-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="6317e-109">Использование по промежуточного слоя сжатия ответа</span><span class="sxs-lookup"><span data-stu-id="6317e-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="6317e-110">Используйте технологии сжатия серверных ответов на базе IIS, Apache или nginx.</span><span class="sxs-lookup"><span data-stu-id="6317e-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="6317e-111">Возможно, производительность по промежуточного слоя не соответствует числу серверных модулей.</span><span class="sxs-lookup"><span data-stu-id="6317e-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="6317e-112">Сервер [http. sys](xref:fundamentals/servers/httpsys) и сервер [Kestrel](xref:fundamentals/servers/kestrel) в настоящее время не предлагают встроенную поддержку сжатия.</span><span class="sxs-lookup"><span data-stu-id="6317e-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="6317e-113">Используйте по промежуточного слоя сжатия ответов, если:</span><span class="sxs-lookup"><span data-stu-id="6317e-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="6317e-114">Не удается использовать следующие технологии сжатия на основе серверов:</span><span class="sxs-lookup"><span data-stu-id="6317e-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="6317e-115">Модуль динамического сжатия IIS</span><span class="sxs-lookup"><span data-stu-id="6317e-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="6317e-116">Модуль Apache mod_deflate</span><span class="sxs-lookup"><span data-stu-id="6317e-116">Apache mod_deflate module</span></span>](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="6317e-117">Сжатие и распаковка nginx</span><span class="sxs-lookup"><span data-stu-id="6317e-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="6317e-118">Размещение непосредственно в:</span><span class="sxs-lookup"><span data-stu-id="6317e-118">Hosting directly on:</span></span>
  * <span data-ttu-id="6317e-119">[Сервер HTTP. sys](xref:fundamentals/servers/httpsys) (ранее именуемый прослушиваемей)</span><span class="sxs-lookup"><span data-stu-id="6317e-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="6317e-120">Сервер Kestrel</span><span class="sxs-lookup"><span data-stu-id="6317e-120">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="6317e-121">Сжатие ответов</span><span class="sxs-lookup"><span data-stu-id="6317e-121">Response compression</span></span>

<span data-ttu-id="6317e-122">Как правило, любой ответ, не сжатый в собственном формате, может выиграть от сжатия ответа.</span><span class="sxs-lookup"><span data-stu-id="6317e-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="6317e-123">Ответы, не сжатые в собственном формате, обычно включают в себя CSS, JavaScript, HTML, XML и JSON.</span><span class="sxs-lookup"><span data-stu-id="6317e-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="6317e-124">Не следует сжимать в собственном формате активы, такие как PNG-файлы.</span><span class="sxs-lookup"><span data-stu-id="6317e-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="6317e-125">При попытке дальнейшего сжатия отклика, сжатого в собственном формате, любое небольшое уменьшение размера и времени передачи, скорее всего, будет превышено на время, затраченное на обработку сжатия.</span><span class="sxs-lookup"><span data-stu-id="6317e-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="6317e-126">Не сжимать файлы размером менее 150-1000 байт (в зависимости от содержимого файла и эффективности сжатия).</span><span class="sxs-lookup"><span data-stu-id="6317e-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="6317e-127">Затраты на сжатие мелких файлов могут привести к созданию сжатого файла, большего, чем несжатый файл.</span><span class="sxs-lookup"><span data-stu-id="6317e-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="6317e-128">Когда клиент может обработать сжатое содержимое, клиент должен сообщить серверу о своих возможностях, отправив заголовок `Accept-Encoding` с запросом.</span><span class="sxs-lookup"><span data-stu-id="6317e-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="6317e-129">Когда сервер отправляет сжатое содержимое, он должен содержать сведения в заголовке `Content-Encoding` о кодировании сжатого ответа.</span><span class="sxs-lookup"><span data-stu-id="6317e-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="6317e-130">В следующей таблице показаны конструкции кодирования содержимого, поддерживаемые по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="6317e-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="6317e-131">значения заголовков `Accept-Encoding`</span><span class="sxs-lookup"><span data-stu-id="6317e-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="6317e-132">Поддерживается по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="6317e-132">Middleware Supported</span></span> | <span data-ttu-id="6317e-133">Описание:</span><span class="sxs-lookup"><span data-stu-id="6317e-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="6317e-134">Да (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="6317e-134">Yes (default)</span></span>        | [<span data-ttu-id="6317e-135">Формат сжатых данных Brotli</span><span class="sxs-lookup"><span data-stu-id="6317e-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="6317e-136">Нет</span><span class="sxs-lookup"><span data-stu-id="6317e-136">No</span></span>                   | [<span data-ttu-id="6317e-137">Сжатый формат сжатых данных</span><span class="sxs-lookup"><span data-stu-id="6317e-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="6317e-138">Нет</span><span class="sxs-lookup"><span data-stu-id="6317e-138">No</span></span>                   | [<span data-ttu-id="6317e-139">Эффективный XML-обмен в формате W3C</span><span class="sxs-lookup"><span data-stu-id="6317e-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="6317e-140">Да</span><span class="sxs-lookup"><span data-stu-id="6317e-140">Yes</span></span>                  | [<span data-ttu-id="6317e-141">Формат файла gzip</span><span class="sxs-lookup"><span data-stu-id="6317e-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="6317e-142">Да</span><span class="sxs-lookup"><span data-stu-id="6317e-142">Yes</span></span>                  | <span data-ttu-id="6317e-143">Идентификатор "без кодирования": ответ не должен быть закодирован.</span><span class="sxs-lookup"><span data-stu-id="6317e-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="6317e-144">Нет</span><span class="sxs-lookup"><span data-stu-id="6317e-144">No</span></span>                   | [<span data-ttu-id="6317e-145">Формат сетевой пересылки для архивов Java</span><span class="sxs-lookup"><span data-stu-id="6317e-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="6317e-146">Да</span><span class="sxs-lookup"><span data-stu-id="6317e-146">Yes</span></span>                  | <span data-ttu-id="6317e-147">Любая доступная кодировка содержимого, которая не запрашивается явно</span><span class="sxs-lookup"><span data-stu-id="6317e-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="6317e-148">значения заголовков `Accept-Encoding`</span><span class="sxs-lookup"><span data-stu-id="6317e-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="6317e-149">Поддерживается по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="6317e-149">Middleware Supported</span></span> | <span data-ttu-id="6317e-150">Описание:</span><span class="sxs-lookup"><span data-stu-id="6317e-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="6317e-151">Нет</span><span class="sxs-lookup"><span data-stu-id="6317e-151">No</span></span>                   | [<span data-ttu-id="6317e-152">Формат сжатых данных Brotli</span><span class="sxs-lookup"><span data-stu-id="6317e-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="6317e-153">Нет</span><span class="sxs-lookup"><span data-stu-id="6317e-153">No</span></span>                   | [<span data-ttu-id="6317e-154">Сжатый формат сжатых данных</span><span class="sxs-lookup"><span data-stu-id="6317e-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="6317e-155">Нет</span><span class="sxs-lookup"><span data-stu-id="6317e-155">No</span></span>                   | [<span data-ttu-id="6317e-156">Эффективный XML-обмен в формате W3C</span><span class="sxs-lookup"><span data-stu-id="6317e-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="6317e-157">Да (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="6317e-157">Yes (default)</span></span>        | [<span data-ttu-id="6317e-158">Формат файла gzip</span><span class="sxs-lookup"><span data-stu-id="6317e-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="6317e-159">Да</span><span class="sxs-lookup"><span data-stu-id="6317e-159">Yes</span></span>                  | <span data-ttu-id="6317e-160">Идентификатор "без кодирования": ответ не должен быть закодирован.</span><span class="sxs-lookup"><span data-stu-id="6317e-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="6317e-161">Нет</span><span class="sxs-lookup"><span data-stu-id="6317e-161">No</span></span>                   | [<span data-ttu-id="6317e-162">Формат сетевой пересылки для архивов Java</span><span class="sxs-lookup"><span data-stu-id="6317e-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="6317e-163">Да</span><span class="sxs-lookup"><span data-stu-id="6317e-163">Yes</span></span>                  | <span data-ttu-id="6317e-164">Любая доступная кодировка содержимого, которая не запрашивается явно</span><span class="sxs-lookup"><span data-stu-id="6317e-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="6317e-165">Дополнительные сведения см. в [списке официального кодирования содержимого IANA](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="6317e-165">For more information, see the [IANA Official Content Coding List](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="6317e-166">По промежуточного слоя позволяет добавлять дополнительные поставщики сжатия для настраиваемых значений заголовков `Accept-Encoding`.</span><span class="sxs-lookup"><span data-stu-id="6317e-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="6317e-167">Дополнительные сведения см. в разделе [Настраиваемые поставщики](#custom-providers) ниже.</span><span class="sxs-lookup"><span data-stu-id="6317e-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="6317e-168">По промежуточного слоя может реагировать на весовые значения качества (квалуе, `q`) при отправке клиентом для определения приоритета схем сжатия.</span><span class="sxs-lookup"><span data-stu-id="6317e-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="6317e-169">Дополнительные сведения см. в [документе RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="6317e-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="6317e-170">Алгоритмы сжатия подчиняются компромиссу между скоростью сжатия и эффективностью сжатия.</span><span class="sxs-lookup"><span data-stu-id="6317e-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="6317e-171">*Эффективность* в этом контексте означает размер выходных данных после сжатия.</span><span class="sxs-lookup"><span data-stu-id="6317e-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="6317e-172">Наименьший размер достигается самым *оптимальным* сжатием.</span><span class="sxs-lookup"><span data-stu-id="6317e-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="6317e-173">Заголовки, используемые для запроса, отправки, кэширования и получения сжатого содержимого, описаны в таблице ниже.</span><span class="sxs-lookup"><span data-stu-id="6317e-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="6317e-174">Header</span><span class="sxs-lookup"><span data-stu-id="6317e-174">Header</span></span>             | <span data-ttu-id="6317e-175">Роль</span><span class="sxs-lookup"><span data-stu-id="6317e-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="6317e-176">Отправляется с клиента на сервер, чтобы указать схемы кодировки содержимого, приемлемые для клиента.</span><span class="sxs-lookup"><span data-stu-id="6317e-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="6317e-177">Отправляется с сервера клиенту для указания кодировки содержимого в полезных данных.</span><span class="sxs-lookup"><span data-stu-id="6317e-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="6317e-178">Когда происходит сжатие, заголовок `Content-Length` удаляется, так как содержимое текста изменяется при сжатии ответа.</span><span class="sxs-lookup"><span data-stu-id="6317e-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="6317e-179">Когда происходит сжатие, заголовок `Content-MD5` удаляется, так как содержимое текста изменилось и хэш больше не является допустимым.</span><span class="sxs-lookup"><span data-stu-id="6317e-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="6317e-180">Указывает тип MIME содержимого.</span><span class="sxs-lookup"><span data-stu-id="6317e-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="6317e-181">Каждый ответ должен указывать его `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="6317e-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="6317e-182">По промежуточного слоя проверяет это значение, чтобы определить, следует ли сжимать ответ.</span><span class="sxs-lookup"><span data-stu-id="6317e-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="6317e-183">По промежуточного слоя указывает набор [типов MIME по умолчанию](#mime-types) , которые он может кодировать, но можно заменить или добавить типы MIME.</span><span class="sxs-lookup"><span data-stu-id="6317e-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="6317e-184">При отправке сервером значения `Accept-Encoding` клиентам и прокси заголовок `Vary` указывает клиенту или прокси-серверу, что он должен кэшировать (Vary) ответы на основе значения заголовка `Accept-Encoding` запроса.</span><span class="sxs-lookup"><span data-stu-id="6317e-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="6317e-185">Результат возврата содержимого с заголовком `Vary: Accept-Encoding` заключается в том, что как сжатые, так и несжатые ответы кэшируются отдельно.</span><span class="sxs-lookup"><span data-stu-id="6317e-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="6317e-186">Изучите функции по промежуточного слоя сжатия ответов с [примером приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="6317e-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="6317e-187">В примере показано следующее:</span><span class="sxs-lookup"><span data-stu-id="6317e-187">The sample illustrates:</span></span>

* <span data-ttu-id="6317e-188">Сжатие ответов приложений с помощью gzip и пользовательских поставщиков сжатия.</span><span class="sxs-lookup"><span data-stu-id="6317e-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="6317e-189">Добавление типа MIME в список по умолчанию типов MIME для сжатия.</span><span class="sxs-lookup"><span data-stu-id="6317e-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="6317e-190">Пакет</span><span class="sxs-lookup"><span data-stu-id="6317e-190">Package</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6317e-191">По промежуточного слоя сжатия ответа предоставляется пакетом [Microsoft. AspNetCore. респонсекомпрессион](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) , который неявно включается в ASP.NET Core приложения.</span><span class="sxs-lookup"><span data-stu-id="6317e-191">Response Compression Middleware is provided by the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package, which is implicitly included in ASP.NET Core apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="6317e-192">Чтобы включить по промежуточного слоя в проект, добавьте ссылку на [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app), которая включает пакет [Microsoft. AspNetCore. респонсекомпрессион](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) .</span><span class="sxs-lookup"><span data-stu-id="6317e-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="6317e-193">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="6317e-193">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6317e-194">В следующем коде показано, как включить по промежуточного слоя сжатия ответа для типов MIME по умолчанию и поставщиков сжатия ([Brotli](#brotli-compression-provider) и [gzip](#gzip-compression-provider)):</span><span class="sxs-lookup"><span data-stu-id="6317e-194">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="6317e-195">В следующем коде показано, как включить по промежуточного слоя сжатия ответов для типов MIME по умолчанию и [поставщика сжатия Gzip](#gzip-compression-provider):</span><span class="sxs-lookup"><span data-stu-id="6317e-195">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

::: moniker-end

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddResponseCompression();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        app.UseResponseCompression();
    }
}
```

<span data-ttu-id="6317e-196">Примечания.</span><span class="sxs-lookup"><span data-stu-id="6317e-196">Notes:</span></span>

* <span data-ttu-id="6317e-197">`app.UseResponseCompression` должны вызываться до какого бы то ни было по промежуточного слоя, которое сжимает ответы.</span><span class="sxs-lookup"><span data-stu-id="6317e-197">`app.UseResponseCompression` must be called before any middleware that compresses responses.</span></span> <span data-ttu-id="6317e-198">Дополнительные сведения см. в разделе <xref:fundamentals/middleware/index#middleware-order>.</span><span class="sxs-lookup"><span data-stu-id="6317e-198">For more information, see <xref:fundamentals/middleware/index#middleware-order>.</span></span>
* <span data-ttu-id="6317e-199">Используйте такое средство, как [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)или [POST](https://www.getpostman.com/) , чтобы задать заголовок запроса `Accept-Encoding` и изучить заголовки, размер и текст ответа.</span><span class="sxs-lookup"><span data-stu-id="6317e-199">Use a tool such as [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="6317e-200">Отправьте запрос в пример приложения без заголовка `Accept-Encoding` и убедитесь, что ответ не сжат.</span><span class="sxs-lookup"><span data-stu-id="6317e-200">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="6317e-201">Заголовки `Content-Encoding` и `Vary` отсутствуют в ответе.</span><span class="sxs-lookup"><span data-stu-id="6317e-201">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Окно Fiddler, показывающее результат запроса без заголовка Accept-Encoding.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6317e-204">Отправьте запрос в пример приложения с заголовком `Accept-Encoding: br` (сжатие Brotli) и обратите внимание на то, что ответ сжат.</span><span class="sxs-lookup"><span data-stu-id="6317e-204">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="6317e-205">В ответе имеются заголовки `Content-Encoding` и `Vary`.</span><span class="sxs-lookup"><span data-stu-id="6317e-205">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Окно Fiddler, показывающее результат запроса с заголовком Accept-Encoding и значением br.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="6317e-209">Отправьте запрос в пример приложения с заголовком `Accept-Encoding: gzip` и убедитесь, что ответ сжат.</span><span class="sxs-lookup"><span data-stu-id="6317e-209">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="6317e-210">В ответе имеются заголовки `Content-Encoding` и `Vary`.</span><span class="sxs-lookup"><span data-stu-id="6317e-210">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Окно Fiddler, показывающее результат запроса с заголовком Accept-Encoding и значением gzip.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a><span data-ttu-id="6317e-214">Поставщики</span><span class="sxs-lookup"><span data-stu-id="6317e-214">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="6317e-215">Поставщик сжатия Brotli</span><span class="sxs-lookup"><span data-stu-id="6317e-215">Brotli Compression Provider</span></span>

<span data-ttu-id="6317e-216">Используйте <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> для сжатия ответов с [форматом сжатых данных Brotli](https://tools.ietf.org/html/rfc7932).</span><span class="sxs-lookup"><span data-stu-id="6317e-216">Use the <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="6317e-217">Если поставщики сжатия явно не добавляются в <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="6317e-217">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="6317e-218">Поставщик сжатия Brotli по умолчанию добавляется в массив поставщиков сжатия вместе с [поставщиком сжатия Gzip](#gzip-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="6317e-218">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="6317e-219">Сжатие по умолчанию используется для сжатия Brotli, если клиент поддерживает формат сжатых данных Brotli.</span><span class="sxs-lookup"><span data-stu-id="6317e-219">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="6317e-220">Если Brotli не поддерживается клиентом, то сжатие по умолчанию используется gzip, если клиент поддерживает сжатие Gzip.</span><span class="sxs-lookup"><span data-stu-id="6317e-220">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="6317e-221">Поставщик сжатия Бротоли должен быть добавлен при явном добавлении любых поставщиков сжатия:</span><span class="sxs-lookup"><span data-stu-id="6317e-221">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6317e-222">Задайте уровень сжатия <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="6317e-222">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span></span> <span data-ttu-id="6317e-223">Поставщик сжатия Brotli по умолчанию имеет самый быстрый уровень сжатия ([CompressionLevel. самый быстрый](xref:System.IO.Compression.CompressionLevel)), что может привести к неэффективному сжатию.</span><span class="sxs-lookup"><span data-stu-id="6317e-223">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="6317e-224">Если требуется наиболее эффективное сжатие, настройте по промежуточного слоя для оптимального сжатия.</span><span class="sxs-lookup"><span data-stu-id="6317e-224">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="6317e-225">Compression Level</span><span class="sxs-lookup"><span data-stu-id="6317e-225">Compression Level</span></span> | <span data-ttu-id="6317e-226">Описание:</span><span class="sxs-lookup"><span data-stu-id="6317e-226">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="6317e-227">CompressionLevel. самый быстрый</span><span class="sxs-lookup"><span data-stu-id="6317e-227">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="6317e-228">Сжатие должно завершаться как можно быстрее, даже если полученные выходные данные не будут оптимально сжаты.</span><span class="sxs-lookup"><span data-stu-id="6317e-228">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="6317e-229">CompressionLevel. уплотнение</span><span class="sxs-lookup"><span data-stu-id="6317e-229">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="6317e-230">Сжатие выполнять не нужно.</span><span class="sxs-lookup"><span data-stu-id="6317e-230">No compression should be performed.</span></span> |
| [<span data-ttu-id="6317e-231">CompressionLevel. оптимально</span><span class="sxs-lookup"><span data-stu-id="6317e-231">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="6317e-232">Ответы должны быть оптимально сжаты, даже если сжатие занимает больше времени.</span><span class="sxs-lookup"><span data-stu-id="6317e-232">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<BrotliCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

::: moniker-end

### <a name="gzip-compression-provider"></a><span data-ttu-id="6317e-233">Поставщик сжатия GZIP</span><span class="sxs-lookup"><span data-stu-id="6317e-233">Gzip Compression Provider</span></span>

<span data-ttu-id="6317e-234">Используйте <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> для сжатия ответов с [форматом файла gzip](https://tools.ietf.org/html/rfc1952).</span><span class="sxs-lookup"><span data-stu-id="6317e-234">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="6317e-235">Если поставщики сжатия явно не добавляются в <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="6317e-235">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="6317e-236">Поставщик сжатия Gzip добавляется по умолчанию в массив поставщиков сжатия вместе с [поставщиком сжатия Brotli](#brotli-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="6317e-236">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="6317e-237">Сжатие по умолчанию используется для сжатия Brotli, если клиент поддерживает формат сжатых данных Brotli.</span><span class="sxs-lookup"><span data-stu-id="6317e-237">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="6317e-238">Если Brotli не поддерживается клиентом, то сжатие по умолчанию используется gzip, если клиент поддерживает сжатие Gzip.</span><span class="sxs-lookup"><span data-stu-id="6317e-238">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="6317e-239">Поставщик сжатия Gzip добавляется по умолчанию в массив поставщиков сжатия.</span><span class="sxs-lookup"><span data-stu-id="6317e-239">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="6317e-240">Сжатие по умолчанию используется gzip, если клиент поддерживает сжатие Gzip.</span><span class="sxs-lookup"><span data-stu-id="6317e-240">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="6317e-241">При явном добавлении поставщиков сжатия необходимо добавить поставщик сжатия gzip:</span><span class="sxs-lookup"><span data-stu-id="6317e-241">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

<span data-ttu-id="6317e-242">Задайте уровень сжатия <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="6317e-242">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="6317e-243">Поставщик сжатия Gzip по умолчанию имеет самый быстрый уровень сжатия ([CompressionLevel. самый быстрый](xref:System.IO.Compression.CompressionLevel)), что может не привести к максимально эффективному сжатию.</span><span class="sxs-lookup"><span data-stu-id="6317e-243">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="6317e-244">Если требуется наиболее эффективное сжатие, настройте по промежуточного слоя для оптимального сжатия.</span><span class="sxs-lookup"><span data-stu-id="6317e-244">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="6317e-245">Compression Level</span><span class="sxs-lookup"><span data-stu-id="6317e-245">Compression Level</span></span> | <span data-ttu-id="6317e-246">Описание:</span><span class="sxs-lookup"><span data-stu-id="6317e-246">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="6317e-247">CompressionLevel. самый быстрый</span><span class="sxs-lookup"><span data-stu-id="6317e-247">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="6317e-248">Сжатие должно завершаться как можно быстрее, даже если полученные выходные данные не будут оптимально сжаты.</span><span class="sxs-lookup"><span data-stu-id="6317e-248">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="6317e-249">CompressionLevel. уплотнение</span><span class="sxs-lookup"><span data-stu-id="6317e-249">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="6317e-250">Сжатие выполнять не нужно.</span><span class="sxs-lookup"><span data-stu-id="6317e-250">No compression should be performed.</span></span> |
| [<span data-ttu-id="6317e-251">CompressionLevel. оптимально</span><span class="sxs-lookup"><span data-stu-id="6317e-251">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="6317e-252">Ответы должны быть оптимально сжаты, даже если сжатие занимает больше времени.</span><span class="sxs-lookup"><span data-stu-id="6317e-252">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<GzipCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

### <a name="custom-providers"></a><span data-ttu-id="6317e-253">Настраиваемые поставщики</span><span class="sxs-lookup"><span data-stu-id="6317e-253">Custom providers</span></span>

<span data-ttu-id="6317e-254">Создание пользовательских реализаций сжатия с помощью <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span><span class="sxs-lookup"><span data-stu-id="6317e-254">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="6317e-255"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> представляет кодировку содержимого, которую `ICompressionProvider` создает.</span><span class="sxs-lookup"><span data-stu-id="6317e-255">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="6317e-256">По промежуточного слоя использует эти сведения для выбора поставщика на основе списка, указанного в заголовке `Accept-Encoding` запроса.</span><span class="sxs-lookup"><span data-stu-id="6317e-256">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="6317e-257">С помощью примера приложения клиент отправляет запрос с заголовком `Accept-Encoding: mycustomcompression`.</span><span class="sxs-lookup"><span data-stu-id="6317e-257">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="6317e-258">По промежуточного слоя использует реализацию пользовательского сжатия и возвращает ответ с заголовком `Content-Encoding: mycustomcompression`.</span><span class="sxs-lookup"><span data-stu-id="6317e-258">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="6317e-259">Клиент должен иметь возможность распаковать пользовательскую кодировку, чтобы обеспечить работу пользовательской реализации сжатия.</span><span class="sxs-lookup"><span data-stu-id="6317e-259">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/3.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6317e-260">Отправьте запрос в пример приложения с заголовком `Accept-Encoding: mycustomcompression` и просмотрите заголовки ответа.</span><span class="sxs-lookup"><span data-stu-id="6317e-260">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="6317e-261">В ответе имеются заголовки `Vary` и `Content-Encoding`.</span><span class="sxs-lookup"><span data-stu-id="6317e-261">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="6317e-262">Текст ответа (не показан) не сжимается образцом.</span><span class="sxs-lookup"><span data-stu-id="6317e-262">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="6317e-263">В классе `CustomCompressionProvider` образца отсутствует реализация сжатия.</span><span class="sxs-lookup"><span data-stu-id="6317e-263">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="6317e-264">Однако в примере показано, где следует реализовать такой алгоритм сжатия.</span><span class="sxs-lookup"><span data-stu-id="6317e-264">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Окно Fiddler, показывающее результат запроса с заголовком Accept-Encoding и значением микустомкомпрессион.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="6317e-267">типы MIME</span><span class="sxs-lookup"><span data-stu-id="6317e-267">MIME types</span></span>

<span data-ttu-id="6317e-268">По промежуточного слоя указывает набор типов MIME по умолчанию для сжатия:</span><span class="sxs-lookup"><span data-stu-id="6317e-268">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="6317e-269">Замените или добавьте типы MIME с помощью параметров по промежуточного слоя сжатия ответа.</span><span class="sxs-lookup"><span data-stu-id="6317e-269">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="6317e-270">Обратите внимание, что типы MIME с подстановочными знаками, такие как `text/*`, не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="6317e-270">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="6317e-271">Пример приложения добавляет тип MIME для `image/svg+xml` и сжимает и обслуживает изображение ASP.NET Core баннера (*Banner. SVG*).</span><span class="sxs-lookup"><span data-stu-id="6317e-271">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="6317e-272">Сжатие по защищенному протоколу</span><span class="sxs-lookup"><span data-stu-id="6317e-272">Compression with secure protocol</span></span>

<span data-ttu-id="6317e-273">Сжатые ответы по защищенным подключениям можно контролировать с помощью параметра `EnableForHttps`, который по умолчанию отключен.</span><span class="sxs-lookup"><span data-stu-id="6317e-273">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="6317e-274">Использование сжатия с динамически создаваемыми страницами может привести к проблемам безопасности, таким как [преступления](https://wikipedia.org/wiki/CRIME_(security_exploit)) и [нарушение](https://wikipedia.org/wiki/BREACH_(security_exploit)) атак.</span><span class="sxs-lookup"><span data-stu-id="6317e-274">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="6317e-275">Добавление заголовка Vary</span><span class="sxs-lookup"><span data-stu-id="6317e-275">Adding the Vary header</span></span>

<span data-ttu-id="6317e-276">При сжатии ответов на основе заголовка `Accept-Encoding` существует потенциально несколько сжатых версий ответа и несжатая версия.</span><span class="sxs-lookup"><span data-stu-id="6317e-276">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="6317e-277">Чтобы настроить кэширование клиента и прокси-сервера на наличие нескольких версий и их хранения, `Vary` заголовок добавляется со значением `Accept-Encoding`.</span><span class="sxs-lookup"><span data-stu-id="6317e-277">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="6317e-278">В ASP.NET Core 2,0 или более поздней версии по промежуточного слоя автоматически добавляет заголовок `Vary` при сжатии ответа.</span><span class="sxs-lookup"><span data-stu-id="6317e-278">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="6317e-279">Проблемы по промежуточного слоя, когда nginx обратный прокси-сервер</span><span class="sxs-lookup"><span data-stu-id="6317e-279">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="6317e-280">При выполнении запроса через прокси-сервер nginx заголовок `Accept-Encoding` удаляется.</span><span class="sxs-lookup"><span data-stu-id="6317e-280">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="6317e-281">Удаление заголовка `Accept-Encoding` предотвращает сжатие ответа по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="6317e-281">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="6317e-282">Дополнительные сведения см. в разделе [nginx: сжатие и распаковка](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="6317e-282">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="6317e-283">Эта проблема проявляется на [рисунке сквозного сжатия для nginx (ASPNET/басикмиддлеваре #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="6317e-283">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="6317e-284">Работа с динамическим сжатием IIS</span><span class="sxs-lookup"><span data-stu-id="6317e-284">Working with IIS dynamic compression</span></span>

<span data-ttu-id="6317e-285">При наличии активного модуля динамического сжатия IIS, настроенного на уровне сервера, который вы хотите отключить для приложения, отключите модуль, дополнив добавление к файлу *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="6317e-285">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="6317e-286">Дополнительные сведения см. в разделе [Отключение модулей IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="6317e-286">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="6317e-287">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="6317e-287">Troubleshooting</span></span>

<span data-ttu-id="6317e-288">Используйте такие средства, как [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)или [POST](https://www.getpostman.com/), которые позволяют задать заголовок запроса `Accept-Encoding` и изучить заголовки, размер и текст ответа.</span><span class="sxs-lookup"><span data-stu-id="6317e-288">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="6317e-289">По умолчанию по промежуточного слоя для сжатия ответов сжимает ответы, соответствующие следующим условиям.</span><span class="sxs-lookup"><span data-stu-id="6317e-289">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="6317e-290">Заголовок `Accept-Encoding` имеет значение `br`, `gzip`, `*`или пользовательское кодирование, соответствующее настраиваемому поставщику сжатия, который вы установили.</span><span class="sxs-lookup"><span data-stu-id="6317e-290">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="6317e-291">Значение не должно быть `identity` или иметь значение свойства Value (квалуе, `q`), равное 0 (нулю).</span><span class="sxs-lookup"><span data-stu-id="6317e-291">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="6317e-292">Тип MIME (`Content-Type`) должен быть установлен и должен соответствовать типу MIME, настроенному в <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="6317e-292">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="6317e-293">Запрос не должен включать заголовок `Content-Range`.</span><span class="sxs-lookup"><span data-stu-id="6317e-293">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="6317e-294">В запросе должен использоваться небезопасный протокол (http), если в параметрах по промежуточного слоя сжатия отклика не настроен защищенный протокол (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="6317e-294">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="6317e-295">*Обратите внимание на опасность, [описанную выше](#compression-with-secure-protocol) , при включении безопасного сжатия содержимого.*</span><span class="sxs-lookup"><span data-stu-id="6317e-295">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="6317e-296">Заголовок `Accept-Encoding` имеет значение `gzip`, `*`или пользовательское кодирование, соответствующее настраиваемому поставщику сжатия, который вы установили.</span><span class="sxs-lookup"><span data-stu-id="6317e-296">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="6317e-297">Значение не должно быть `identity` или иметь значение свойства Value (квалуе, `q`), равное 0 (нулю).</span><span class="sxs-lookup"><span data-stu-id="6317e-297">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="6317e-298">Тип MIME (`Content-Type`) должен быть установлен и должен соответствовать типу MIME, настроенному в <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="6317e-298">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="6317e-299">Запрос не должен включать заголовок `Content-Range`.</span><span class="sxs-lookup"><span data-stu-id="6317e-299">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="6317e-300">В запросе должен использоваться небезопасный протокол (http), если в параметрах по промежуточного слоя сжатия отклика не настроен защищенный протокол (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="6317e-300">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="6317e-301">*Обратите внимание на опасность, [описанную выше](#compression-with-secure-protocol) , при включении безопасного сжатия содержимого.*</span><span class="sxs-lookup"><span data-stu-id="6317e-301">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="6317e-302">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6317e-302">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="6317e-303">Сеть для разработчиков Mozilla: Accept-Encoding</span><span class="sxs-lookup"><span data-stu-id="6317e-303">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="6317e-304">Раздел RFC 7231 3.1.2.1. Кодирование содержимого</span><span class="sxs-lookup"><span data-stu-id="6317e-304">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="6317e-305">Раздел RFC 7230 4.2.3: кодирование gzip</span><span class="sxs-lookup"><span data-stu-id="6317e-305">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="6317e-306">Спецификация формата файла GZIP, версия 4,3</span><span class="sxs-lookup"><span data-stu-id="6317e-306">GZIP file format specification version 4.3</span></span>](https://www.ietf.org/rfc/rfc1952.txt)
