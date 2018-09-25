---
title: Сжатие откликов в ASP.NET Core
author: guardrex
description: Дополнительные сведения о сжатие откликов и способах использования по промежуточного слоя для сжатия ответов в приложениях ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2018
uid: performance/response-compression
ms.openlocfilehash: 3a01c2d572c0026944347f736f9658a7872e6c35
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028288"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="17da7-103">Сжатие откликов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="17da7-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="17da7-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="17da7-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="17da7-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="17da7-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="17da7-106">Пропускная способность сети является ограниченным ресурсом.</span><span class="sxs-lookup"><span data-stu-id="17da7-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="17da7-107">Уменьшение размера ответа обычно часто значительно увеличивается скорость реагирования приложения.</span><span class="sxs-lookup"><span data-stu-id="17da7-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="17da7-108">Для сжатия ответов приложения является одним из способов уменьшить размеры полезной нагрузки.</span><span class="sxs-lookup"><span data-stu-id="17da7-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="17da7-109">Когда следует использовать по промежуточного слоя для сжатия ответов</span><span class="sxs-lookup"><span data-stu-id="17da7-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="17da7-110">Использование технологий сжатия ответ на основе сервера в IIS, Apache или Nginx.</span><span class="sxs-lookup"><span data-stu-id="17da7-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="17da7-111">Производительность по промежуточного слоя скорее всего, не соответствует серверных модулей.</span><span class="sxs-lookup"><span data-stu-id="17da7-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="17da7-112">[Сервер HTTP.sys](xref:fundamentals/servers/httpsys) и [Kestrel](xref:fundamentals/servers/kestrel) настоящее время не предлагает поддержку встроенную функцию сжатия.</span><span class="sxs-lookup"><span data-stu-id="17da7-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) and [Kestrel](xref:fundamentals/servers/kestrel) don't currently offer built-in compression support.</span></span>

<span data-ttu-id="17da7-113">Используйте по промежуточного слоя для сжатия ответов, когда вы будете:</span><span class="sxs-lookup"><span data-stu-id="17da7-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="17da7-114">Не удается использовать со следующими технологиями сжатия на основе сервера:</span><span class="sxs-lookup"><span data-stu-id="17da7-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="17da7-115">Модуль динамического сжатия служб IIS</span><span class="sxs-lookup"><span data-stu-id="17da7-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="17da7-116">Модуль mod_deflate Apache</span><span class="sxs-lookup"><span data-stu-id="17da7-116">Apache mod_deflate module</span></span>](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="17da7-117">Nginx сжатия и распаковки</span><span class="sxs-lookup"><span data-stu-id="17da7-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="17da7-118">Размещение непосредственно на:</span><span class="sxs-lookup"><span data-stu-id="17da7-118">Hosting directly on:</span></span>
  * <span data-ttu-id="17da7-119">[Сервер HTTP.sys](xref:fundamentals/servers/httpsys) (которая раньше называлась [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="17da7-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>
  * [<span data-ttu-id="17da7-120">Kestrel</span><span class="sxs-lookup"><span data-stu-id="17da7-120">Kestrel</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="17da7-121">Сжатие откликов</span><span class="sxs-lookup"><span data-stu-id="17da7-121">Response compression</span></span>

<span data-ttu-id="17da7-122">Как правило любой ответ, изначально не сжаты могут использовать преимущества сжатия отклика.</span><span class="sxs-lookup"><span data-stu-id="17da7-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="17da7-123">Ответы, изначально не сжаты обычно включают: CSS, JavaScript, HTML, XML и JSON.</span><span class="sxs-lookup"><span data-stu-id="17da7-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="17da7-124">Не следует сжимать изначально сжатых ресурсов, таких как PNG-файлы.</span><span class="sxs-lookup"><span data-stu-id="17da7-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="17da7-125">При попытке дальнейшее сжатие изначально сжатый ответ, небольшого дополнительного сокращения размера и передачи времени скорее всего будет злоумышленниками время, которое потребовалось для обработки сжатия.</span><span class="sxs-lookup"><span data-stu-id="17da7-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="17da7-126">Не сжимать файлы размером меньше примерно 150 – 1000 байт (в зависимости от его содержимого и повысить эффективность сжатия).</span><span class="sxs-lookup"><span data-stu-id="17da7-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="17da7-127">Затраты на сжатие небольших файлов могут давать сжатый файл, размер которых превышает несжатый файл.</span><span class="sxs-lookup"><span data-stu-id="17da7-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="17da7-128">Когда клиент может обрабатывать сжатое содержимое, клиент должен уведомления сервера его возможности, отправляя `Accept-Encoding` заголовок с запросом.</span><span class="sxs-lookup"><span data-stu-id="17da7-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="17da7-129">Когда сервер отправляет сжатое содержимое, она должна содержать сведения в `Content-Encoding` заголовка на способах кодировки сжатого ответа.</span><span class="sxs-lookup"><span data-stu-id="17da7-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="17da7-130">В следующей таблице показаны содержимого обозначения кодировки, поддерживаемые по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="17da7-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="17da7-131">`Accept-Encoding` значения заголовка</span><span class="sxs-lookup"><span data-stu-id="17da7-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="17da7-132">Поддерживается по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="17da7-132">Middleware Supported</span></span> | <span data-ttu-id="17da7-133">Описание</span><span class="sxs-lookup"><span data-stu-id="17da7-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="17da7-134">Да (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="17da7-134">Yes (default)</span></span>        | [<span data-ttu-id="17da7-135">Формат сжатых данных Brotli</span><span class="sxs-lookup"><span data-stu-id="17da7-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="17da7-136">Нет</span><span class="sxs-lookup"><span data-stu-id="17da7-136">No</span></span>                   | [<span data-ttu-id="17da7-137">Формат DEFLATE сжатых данных</span><span class="sxs-lookup"><span data-stu-id="17da7-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="17da7-138">Нет</span><span class="sxs-lookup"><span data-stu-id="17da7-138">No</span></span>                   | [<span data-ttu-id="17da7-139">W3C XML для эффективного обмена</span><span class="sxs-lookup"><span data-stu-id="17da7-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="17da7-140">Да</span><span class="sxs-lookup"><span data-stu-id="17da7-140">Yes</span></span>                  | [<span data-ttu-id="17da7-141">Формат файла gzip</span><span class="sxs-lookup"><span data-stu-id="17da7-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="17da7-142">Да</span><span class="sxs-lookup"><span data-stu-id="17da7-142">Yes</span></span>                  | <span data-ttu-id="17da7-143">Идентификатор «Без кодировки»: ответ не должен быть закодирован.</span><span class="sxs-lookup"><span data-stu-id="17da7-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="17da7-144">Нет</span><span class="sxs-lookup"><span data-stu-id="17da7-144">No</span></span>                   | [<span data-ttu-id="17da7-145">Сетевой формат передачи архивы Java</span><span class="sxs-lookup"><span data-stu-id="17da7-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="17da7-146">Да</span><span class="sxs-lookup"><span data-stu-id="17da7-146">Yes</span></span>                  | <span data-ttu-id="17da7-147">Любое доступное содержимое, кодировка не явно запрошенного</span><span class="sxs-lookup"><span data-stu-id="17da7-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="17da7-148">`Accept-Encoding` значения заголовка</span><span class="sxs-lookup"><span data-stu-id="17da7-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="17da7-149">Поддерживается по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="17da7-149">Middleware Supported</span></span> | <span data-ttu-id="17da7-150">Описание</span><span class="sxs-lookup"><span data-stu-id="17da7-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="17da7-151">Нет</span><span class="sxs-lookup"><span data-stu-id="17da7-151">No</span></span>                   | [<span data-ttu-id="17da7-152">Формат сжатых данных Brotli</span><span class="sxs-lookup"><span data-stu-id="17da7-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="17da7-153">Нет</span><span class="sxs-lookup"><span data-stu-id="17da7-153">No</span></span>                   | [<span data-ttu-id="17da7-154">Формат DEFLATE сжатых данных</span><span class="sxs-lookup"><span data-stu-id="17da7-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="17da7-155">Нет</span><span class="sxs-lookup"><span data-stu-id="17da7-155">No</span></span>                   | [<span data-ttu-id="17da7-156">W3C XML для эффективного обмена</span><span class="sxs-lookup"><span data-stu-id="17da7-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="17da7-157">Да (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="17da7-157">Yes (default)</span></span>        | [<span data-ttu-id="17da7-158">Формат файла gzip</span><span class="sxs-lookup"><span data-stu-id="17da7-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="17da7-159">Да</span><span class="sxs-lookup"><span data-stu-id="17da7-159">Yes</span></span>                  | <span data-ttu-id="17da7-160">Идентификатор «Без кодировки»: ответ не должен быть закодирован.</span><span class="sxs-lookup"><span data-stu-id="17da7-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="17da7-161">Нет</span><span class="sxs-lookup"><span data-stu-id="17da7-161">No</span></span>                   | [<span data-ttu-id="17da7-162">Сетевой формат передачи архивы Java</span><span class="sxs-lookup"><span data-stu-id="17da7-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="17da7-163">Да</span><span class="sxs-lookup"><span data-stu-id="17da7-163">Yes</span></span>                  | <span data-ttu-id="17da7-164">Любое доступное содержимое, кодировка не явно запрошенного</span><span class="sxs-lookup"><span data-stu-id="17da7-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="17da7-165">Дополнительные сведения см. в разделе [IANA официальный кодирования списка содержимого](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="17da7-165">For more information, see the [IANA Official Content Coding List](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="17da7-166">По промежуточного слоя можно добавить дополнительное сжатие поставщиков для пользовательских `Accept-Encoding` значения заголовка.</span><span class="sxs-lookup"><span data-stu-id="17da7-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="17da7-167">Дополнительные сведения см. в разделе [настраиваемые поставщики](#custom-providers) ниже.</span><span class="sxs-lookup"><span data-stu-id="17da7-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="17da7-168">По промежуточного слоя способен реагирование на значение качества (qvalue, `q`) вес при отправке клиентом для определения приоритетов схемы сжатия.</span><span class="sxs-lookup"><span data-stu-id="17da7-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="17da7-169">Дополнительные сведения см. в разделе [RFC 7231: приемлемой кодировкой](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="17da7-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="17da7-170">Алгоритмы сжатия, распространяются компромисс между скоростью сжатие и эффективность сжатия.</span><span class="sxs-lookup"><span data-stu-id="17da7-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="17da7-171">*Эффективность* в данном контексте означает размер выходных данных после сжатия.</span><span class="sxs-lookup"><span data-stu-id="17da7-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="17da7-172">Наименьший размер достигается за счет наиболее *оптимальной* сжатия.</span><span class="sxs-lookup"><span data-stu-id="17da7-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="17da7-173">Заголовки, участвующих в запросе, отправки, кэширование и получать сжатое содержимое описаны в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="17da7-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="17da7-174">Header</span><span class="sxs-lookup"><span data-stu-id="17da7-174">Header</span></span>             | <span data-ttu-id="17da7-175">Роль</span><span class="sxs-lookup"><span data-stu-id="17da7-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="17da7-176">Отправленные клиентом на сервер для указания содержимого, кодирование схемы, допустимых для клиента.</span><span class="sxs-lookup"><span data-stu-id="17da7-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="17da7-177">Клиенту с сервера для обозначения кодировки содержимого в полезных данных.</span><span class="sxs-lookup"><span data-stu-id="17da7-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="17da7-178">В случае сжатия `Content-Length` заголовок удаляется с момента изменения содержимого текста при ответе сжимается.</span><span class="sxs-lookup"><span data-stu-id="17da7-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="17da7-179">В случае сжатия `Content-MD5` заголовок удаляется, так как содержимое тела был изменен и хэш-код больше не является допустимым.</span><span class="sxs-lookup"><span data-stu-id="17da7-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="17da7-180">Указывает тип MIME содержимого.</span><span class="sxs-lookup"><span data-stu-id="17da7-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="17da7-181">Каждый ответ следует указать его `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="17da7-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="17da7-182">По промежуточного слоя проверяет это значение, чтобы определить, следует ли сжимать ответа.</span><span class="sxs-lookup"><span data-stu-id="17da7-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="17da7-183">По промежуточного слоя задает набор [по умолчанию типы MIME](#mime-types) , его можно закодировать, но можно заменить или добавить типы MIME.</span><span class="sxs-lookup"><span data-stu-id="17da7-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="17da7-184">При отправке сервером со значением `Accept-Encoding` для клиентов и прокси-серверы, `Vary` заголовок указывает клиенту или прокси-сервер, его следует кэшировать (различаться) ответы на основе значения из `Accept-Encoding` заголовок запроса.</span><span class="sxs-lookup"><span data-stu-id="17da7-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="17da7-185">Результат возврата содержимого с помощью `Vary: Accept-Encoding` заголовка является как краткая, так и без сжатия ответов, кэшируются отдельно.</span><span class="sxs-lookup"><span data-stu-id="17da7-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="17da7-186">Изучите возможности по промежуточного слоя сжатия ответов с [пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="17da7-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="17da7-187">В образце показано:</span><span class="sxs-lookup"><span data-stu-id="17da7-187">The sample illustrates:</span></span>

* <span data-ttu-id="17da7-188">Сжатие ответов приложения с помощью Gzip и сжатие пользовательских поставщиков.</span><span class="sxs-lookup"><span data-stu-id="17da7-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="17da7-189">Как добавить тип MIME по умолчанию список типов MIME для сжатия.</span><span class="sxs-lookup"><span data-stu-id="17da7-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="17da7-190">Пакет</span><span class="sxs-lookup"><span data-stu-id="17da7-190">Package</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="17da7-191">Чтобы включить по промежуточного слоя в проекте, добавьте ссылку на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), который включает [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) пакета.</span><span class="sxs-lookup"><span data-stu-id="17da7-191">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="17da7-192">Чтобы включить по промежуточного слоя в проекте, добавьте ссылку на [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage), который включает [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) пакета.</span><span class="sxs-lookup"><span data-stu-id="17da7-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="17da7-193">Чтобы включить по промежуточного слоя в проекте, добавьте ссылку на [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) пакета.</span><span class="sxs-lookup"><span data-stu-id="17da7-193">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="17da7-194">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="17da7-194">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="17da7-195">Ниже показано, как включить по промежуточного слоя сжатия ответов типы MIME по умолчанию, а также поставщиков сжатия ([Brotli](#brotli-compression-provider) и [Gzip](#gzip-compression-provider)):</span><span class="sxs-lookup"><span data-stu-id="17da7-195">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="17da7-196">Ниже показано, как включить по промежуточного слоя сжатия ответов для типов MIME по умолчанию и [поставщика сжатие Gzip](#gzip-compression-provider):</span><span class="sxs-lookup"><span data-stu-id="17da7-196">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

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

> [!NOTE]
> <span data-ttu-id="17da7-197">Используйте инструменты, например [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), или [Postman](https://www.getpostman.com/) присвоить `Accept-Encoding` заголовок запроса и изучите заголовки ответа, размер и текст.</span><span class="sxs-lookup"><span data-stu-id="17da7-197">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="17da7-198">Отправить запрос в пример приложения без `Accept-Encoding` заголовка и обратите внимание, что ответ без сжатия.</span><span class="sxs-lookup"><span data-stu-id="17da7-198">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="17da7-199">`Content-Encoding` И `Vary` заголовки не присутствуют в ответе.</span><span class="sxs-lookup"><span data-stu-id="17da7-199">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Fiddler окно, отображающее результат запроса без заголовка Accept-Encoding.](response-compression/_static/request-uncompressed.png)

<span data-ttu-id="17da7-202">Отправить запрос в пример приложения с `Accept-Encoding: gzip` заголовка и обратите внимание, что ответ сжата.</span><span class="sxs-lookup"><span data-stu-id="17da7-202">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="17da7-203">`Content-Encoding` И `Vary` заголовки присутствуют в ответе.</span><span class="sxs-lookup"><span data-stu-id="17da7-203">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Fiddler окно, отображающее результат запроса с заголовком Accept-Encoding и значение gzip.](response-compression/_static/request-compressed.png)

## <a name="providers"></a><span data-ttu-id="17da7-207">Поставщики</span><span class="sxs-lookup"><span data-stu-id="17da7-207">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="17da7-208">Поставщик сжатие Brotli</span><span class="sxs-lookup"><span data-stu-id="17da7-208">Brotli Compression Provider</span></span>

<span data-ttu-id="17da7-209">Используйте `BrotliCompressionProvider` для сжатия ответов с [формат сжатых данных Brotli](https://tools.ietf.org/html/rfc7932).</span><span class="sxs-lookup"><span data-stu-id="17da7-209">Use the `BrotliCompressionProvider` to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="17da7-210">Если поставщики отсутствуют сжатия явно добавляются <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="17da7-210">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="17da7-211">Массив поставщиков сжатия вместе с по умолчанию добавляется поставщик сжатие Brotli [поставщика сжатие Gzip](#gzip-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="17da7-211">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="17da7-212">По умолчанию сжатие Brotli сжатие при Brotli формат сжатых данных, поддерживаемые клиентом.</span><span class="sxs-lookup"><span data-stu-id="17da7-212">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="17da7-213">Если клиент не поддерживается Brotli, сжатие по умолчанию в Gzip Если клиент поддерживает сжатие Gzip.</span><span class="sxs-lookup"><span data-stu-id="17da7-213">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="17da7-214">Поставщик сжатия Brotoli должен быть добавлен, при любых поставщиков сжатия добавляются явным образом:</span><span class="sxs-lookup"><span data-stu-id="17da7-214">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

<span data-ttu-id="17da7-215">Установка уровня с помощью сжатия `BrotliCompressionProviderOptions`.</span><span class="sxs-lookup"><span data-stu-id="17da7-215">Set the compression level with `BrotliCompressionProviderOptions`.</span></span> <span data-ttu-id="17da7-216">По умолчанию используется поставщик сжатие Brotli наиболее быстрый уровень сжатия ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), который может не обеспечить наиболее эффективное сжатие.</span><span class="sxs-lookup"><span data-stu-id="17da7-216">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="17da7-217">При необходимости наиболее эффективного сжатия настройте по промежуточного слоя для оптимального сжатия.</span><span class="sxs-lookup"><span data-stu-id="17da7-217">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="17da7-218">Уровень сжатия</span><span class="sxs-lookup"><span data-stu-id="17da7-218">Compression Level</span></span> | <span data-ttu-id="17da7-219">Описание</span><span class="sxs-lookup"><span data-stu-id="17da7-219">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="17da7-220">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="17da7-220">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="17da7-221">Сжатие следует выполнить как можно быстрее, даже если полученный результат не сжат оптимально.</span><span class="sxs-lookup"><span data-stu-id="17da7-221">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="17da7-222">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="17da7-222">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="17da7-223">Не требуется сжимать.</span><span class="sxs-lookup"><span data-stu-id="17da7-223">No compression should be performed.</span></span> |
| [<span data-ttu-id="17da7-224">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="17da7-224">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="17da7-225">Ответы должно применяться оптимальное сжатие, даже если сжатие занимает больше времени.</span><span class="sxs-lookup"><span data-stu-id="17da7-225">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="gzip-compression-provider"></a><span data-ttu-id="17da7-226">Поставщик сжатие gzip</span><span class="sxs-lookup"><span data-stu-id="17da7-226">Gzip Compression Provider</span></span>

<span data-ttu-id="17da7-227">Используйте <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> для сжатия ответов с [формате Gzip](https://tools.ietf.org/html/rfc1952).</span><span class="sxs-lookup"><span data-stu-id="17da7-227">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="17da7-228">Если поставщики отсутствуют сжатия явно добавляются <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="17da7-228">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="17da7-229">Массив поставщиков сжатия вместе с по умолчанию добавляется поставщик сжатие Gzip [поставщика сжатие Brotli](#brotli-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="17da7-229">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="17da7-230">По умолчанию сжатие Brotli сжатие при Brotli формат сжатых данных, поддерживаемые клиентом.</span><span class="sxs-lookup"><span data-stu-id="17da7-230">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="17da7-231">Если клиент не поддерживается Brotli, сжатие по умолчанию в Gzip Если клиент поддерживает сжатие Gzip.</span><span class="sxs-lookup"><span data-stu-id="17da7-231">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="17da7-232">Массив поставщиков сжатия по умолчанию добавляется поставщик сжатия Gzip.</span><span class="sxs-lookup"><span data-stu-id="17da7-232">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="17da7-233">По умолчанию сжатие Gzip, если клиент поддерживает сжатие Gzip.</span><span class="sxs-lookup"><span data-stu-id="17da7-233">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="17da7-234">Сжатие Gzip поставщик должен быть добавлен, при любых поставщиков сжатия явно добавляются:</span><span class="sxs-lookup"><span data-stu-id="17da7-234">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5)]

::: moniker-end

<span data-ttu-id="17da7-235">Установка уровня с помощью сжатия <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="17da7-235">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="17da7-236">По умолчанию используется поставщик сжатие Gzip наиболее быстрый уровень сжатия ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), который может не обеспечить наиболее эффективное сжатие.</span><span class="sxs-lookup"><span data-stu-id="17da7-236">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="17da7-237">При необходимости наиболее эффективного сжатия настройте по промежуточного слоя для оптимального сжатия.</span><span class="sxs-lookup"><span data-stu-id="17da7-237">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="17da7-238">Уровень сжатия</span><span class="sxs-lookup"><span data-stu-id="17da7-238">Compression Level</span></span> | <span data-ttu-id="17da7-239">Описание</span><span class="sxs-lookup"><span data-stu-id="17da7-239">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="17da7-240">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="17da7-240">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="17da7-241">Сжатие следует выполнить как можно быстрее, даже если полученный результат не сжат оптимально.</span><span class="sxs-lookup"><span data-stu-id="17da7-241">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="17da7-242">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="17da7-242">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="17da7-243">Не требуется сжимать.</span><span class="sxs-lookup"><span data-stu-id="17da7-243">No compression should be performed.</span></span> |
| [<span data-ttu-id="17da7-244">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="17da7-244">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="17da7-245">Ответы должно применяться оптимальное сжатие, даже если сжатие занимает больше времени.</span><span class="sxs-lookup"><span data-stu-id="17da7-245">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="custom-providers"></a><span data-ttu-id="17da7-246">Настраиваемые поставщики</span><span class="sxs-lookup"><span data-stu-id="17da7-246">Custom providers</span></span>

<span data-ttu-id="17da7-247">Создание реализации сжатия с <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span><span class="sxs-lookup"><span data-stu-id="17da7-247">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="17da7-248"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> Представляет содержимое, кодировка, что этот `ICompressionProvider` создает.</span><span class="sxs-lookup"><span data-stu-id="17da7-248">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="17da7-249">По промежуточного слоя использует эти сведения можно выбрать поставщика, на основе списка, указанное в `Accept-Encoding` заголовок запроса.</span><span class="sxs-lookup"><span data-stu-id="17da7-249">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="17da7-250">С помощью примера приложения, клиент отправляет запрос с помощью `Accept-Encoding: mycustomcompression` заголовка.</span><span class="sxs-lookup"><span data-stu-id="17da7-250">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="17da7-251">По промежуточного слоя использует реализацию сжатия и возвращает ответ с `Content-Encoding: mycustomcompression` заголовка.</span><span class="sxs-lookup"><span data-stu-id="17da7-251">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="17da7-252">Клиент должен быть способен распаковать настраиваемую кодировку в порядке для реализации пользовательских сжатия для работы.</span><span class="sxs-lookup"><span data-stu-id="17da7-252">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

```csharp
public class CustomCompressionProvider : ICompressionProvider
{
    public string EncodingName => "mycustomcompression";
    public bool SupportsFlush => true;

    public Stream CreateStream(Stream outputStream)
    {
        // Create a custom compression stream wrapper here
        return outputStream;
    }
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=6,12-15)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6,12-15)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="17da7-253">Отправить запрос в пример приложения с `Accept-Encoding: mycustomcompression` заголовка и обратите внимание на заголовки ответа.</span><span class="sxs-lookup"><span data-stu-id="17da7-253">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="17da7-254">`Vary` И `Content-Encoding` заголовки присутствуют в ответе.</span><span class="sxs-lookup"><span data-stu-id="17da7-254">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="17da7-255">Текст ответа (не показано) не сжимается в образце.</span><span class="sxs-lookup"><span data-stu-id="17da7-255">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="17da7-256">Отсутствует реализация сжатия в `CustomCompressionProvider` класс образца.</span><span class="sxs-lookup"><span data-stu-id="17da7-256">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="17da7-257">Тем не менее в образце показано, где вам понадобилось бы реализовать алгоритм сжатия.</span><span class="sxs-lookup"><span data-stu-id="17da7-257">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Fiddler окно, отображающее результат запроса с заголовком Accept-Encoding и значение mycustomcompression.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="17da7-260">типы MIME</span><span class="sxs-lookup"><span data-stu-id="17da7-260">MIME types</span></span>

<span data-ttu-id="17da7-261">По промежуточного слоя задает набор по умолчанию типы MIME для сжатия:</span><span class="sxs-lookup"><span data-stu-id="17da7-261">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="17da7-262">Заменить или добавить типы MIME с параметрами по промежуточного слоя для сжатия ответов.</span><span class="sxs-lookup"><span data-stu-id="17da7-262">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="17da7-263">Обратите внимание, что подстановочный знак MIME типы, такие как `text/*` не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="17da7-263">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="17da7-264">Пример приложения добавляет тип MIME для `image/svg+xml` и сжимает и служит изображение баннера в ASP.NET Core (*banner.svg*).</span><span class="sxs-lookup"><span data-stu-id="17da7-264">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7-9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="17da7-265">Сжатие с помощью безопасного протокола</span><span class="sxs-lookup"><span data-stu-id="17da7-265">Compression with secure protocol</span></span>

<span data-ttu-id="17da7-266">Сжатые ответы по безопасным соединениям можно управлять с помощью `EnableForHttps` параметр, который по умолчанию отключена.</span><span class="sxs-lookup"><span data-stu-id="17da7-266">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="17da7-267">Использование сжатия с динамически созданных страниц может привести к проблемам безопасности таких как [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) и [нарушения](https://wikipedia.org/wiki/BREACH_(security_exploit)) атак.</span><span class="sxs-lookup"><span data-stu-id="17da7-267">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="17da7-268">Добавление заголовка Vary</span><span class="sxs-lookup"><span data-stu-id="17da7-268">Adding the Vary header</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="17da7-269">Если сжатие ответов на основе `Accept-Encoding` заголовок, потенциально нескольких версий сжатого ответа и несжатую версию.</span><span class="sxs-lookup"><span data-stu-id="17da7-269">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="17da7-270">Чтобы настроить кэш клиента и прокси-сервера, существует несколько версий, а также должны быть сохранены, `Vary` заголовок добавляется с `Accept-Encoding` значение.</span><span class="sxs-lookup"><span data-stu-id="17da7-270">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="17da7-271">В ASP.NET Core 2.0 или более поздней версии, по промежуточного слоя добавляет `Vary` заголовка автоматически в том случае, когда ответ сжимается.</span><span class="sxs-lookup"><span data-stu-id="17da7-271">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="17da7-272">Если сжатие ответов на основе `Accept-Encoding` заголовок, потенциально нескольких версий сжатого ответа и несжатую версию.</span><span class="sxs-lookup"><span data-stu-id="17da7-272">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="17da7-273">Чтобы настроить кэш клиента и прокси-сервера, существует несколько версий, а также должны быть сохранены, `Vary` заголовок добавляется с `Accept-Encoding` значение.</span><span class="sxs-lookup"><span data-stu-id="17da7-273">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="17da7-274">В ASP.NET Core 1.x, добавление `Vary` в ответ заголовок выполняется вручную:</span><span class="sxs-lookup"><span data-stu-id="17da7-274">In ASP.NET Core 1.x, adding the `Vary` header to the response is accomplished manually:</span></span>

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="17da7-275">По промежуточного слоя проблемы при работе за Nginx обратный прокси-сервер</span><span class="sxs-lookup"><span data-stu-id="17da7-275">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="17da7-276">При наличии запроса, передаются Nginx, `Accept-Encoding` заголовок удаляется.</span><span class="sxs-lookup"><span data-stu-id="17da7-276">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="17da7-277">Это предотвращает сжатие ответ по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="17da7-277">This prevents the middleware from compressing the response.</span></span> <span data-ttu-id="17da7-278">Дополнительные сведения см. в разделе [NGINX: сжатия и распаковки](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="17da7-278">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="17da7-279">Эта проблема отслеживается [выяснить сквозной сжатие для Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="17da7-279">This issue is tracked by [Figure out pass-through compression for Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="17da7-280">Работа с динамического сжатия служб IIS</span><span class="sxs-lookup"><span data-stu-id="17da7-280">Working with IIS dynamic compression</span></span>

<span data-ttu-id="17da7-281">При наличии активных динамического сжатия модуль IIS настроен на уровне сервера, который вы хотите отключить для приложений, отключите модуль с дополнением к *web.config* файл.</span><span class="sxs-lookup"><span data-stu-id="17da7-281">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="17da7-282">Дополнительные сведения см. в разделе [Отключение модулей IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="17da7-282">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="17da7-283">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="17da7-283">Troubleshooting</span></span>

<span data-ttu-id="17da7-284">Использовать такой инструмент, как [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), или [Postman](https://www.getpostman.com/), которые позволяют задать `Accept-Encoding` заголовок запроса и изучите заголовки ответа, размер и текст.</span><span class="sxs-lookup"><span data-stu-id="17da7-284">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="17da7-285">По умолчанию по промежуточного слоя для сжатия ответов сжимает ответы, которые отвечают следующим условиям:</span><span class="sxs-lookup"><span data-stu-id="17da7-285">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="17da7-286">`Accept-Encoding` Заголовок присутствует со значением `br`, `gzip`, `*`, или настраиваемую кодировку, соответствующий поставщику сжатия, который вы определили.</span><span class="sxs-lookup"><span data-stu-id="17da7-286">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="17da7-287">Значение не должно быть `identity` или иметь значение качества (qvalue, `q`) значение 0 (ноль).</span><span class="sxs-lookup"><span data-stu-id="17da7-287">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="17da7-288">Тип MIME (`Content-Type`) должны быть заданы и должен совпадать с типом MIME, настроенный на <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="17da7-288">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="17da7-289">Запрос не должен содержать `Content-Range` заголовка.</span><span class="sxs-lookup"><span data-stu-id="17da7-289">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="17da7-290">Запрос должен использовать незащищенный протокол (http), только если в параметрах по промежуточного слоя для сжатия ответов настроена безопасный протокол (https).</span><span class="sxs-lookup"><span data-stu-id="17da7-290">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="17da7-291">*Обратите внимание, что приведет к неисправности [описанных выше](#compression-with-secure-protocol) при включении безопасного сжатие содержимого.*</span><span class="sxs-lookup"><span data-stu-id="17da7-291">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="17da7-292">`Accept-Encoding` Заголовок присутствует со значением `gzip`, `*`, или настраиваемую кодировку, соответствующий поставщику сжатия, который вы определили.</span><span class="sxs-lookup"><span data-stu-id="17da7-292">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="17da7-293">Значение не должно быть `identity` или иметь значение качества (qvalue, `q`) значение 0 (ноль).</span><span class="sxs-lookup"><span data-stu-id="17da7-293">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="17da7-294">Тип MIME (`Content-Type`) должны быть заданы и должен совпадать с типом MIME, настроенный на <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="17da7-294">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="17da7-295">Запрос не должен содержать `Content-Range` заголовка.</span><span class="sxs-lookup"><span data-stu-id="17da7-295">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="17da7-296">Запрос должен использовать незащищенный протокол (http), только если в параметрах по промежуточного слоя для сжатия ответов настроена безопасный протокол (https).</span><span class="sxs-lookup"><span data-stu-id="17da7-296">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="17da7-297">*Обратите внимание, что приведет к неисправности [описанных выше](#compression-with-secure-protocol) при включении безопасного сжатие содержимого.*</span><span class="sxs-lookup"><span data-stu-id="17da7-297">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="17da7-298">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="17da7-298">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="17da7-299">Сеть разработчиков Mozilla: Приемлемой кодировкой</span><span class="sxs-lookup"><span data-stu-id="17da7-299">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="17da7-300">RFC 7231 раздел 3.1.2.1: Содержимого Codings</span><span class="sxs-lookup"><span data-stu-id="17da7-300">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="17da7-301">RFC 7230 разделе 4.2.3: Кодирования Gzip</span><span class="sxs-lookup"><span data-stu-id="17da7-301">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="17da7-302">Версия спецификации формата файла GZIP 4.3</span><span class="sxs-lookup"><span data-stu-id="17da7-302">GZIP file format specification version 4.3</span></span>](http://www.ietf.org/rfc/rfc1952.txt)
