---
title: Сжатие откликов в ASP.NET Core
author: guardrex
description: Сведения о сжатии откликов и способах использования ПО промежуточного слоя для сжатия откликов в приложениях ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: performance/response-compression
ms.openlocfilehash: d5d2da3dc0a8a452de97d98161d429389d2f7638
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815605"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="003a0-103">Сжатие откликов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="003a0-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="003a0-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="003a0-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="003a0-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="003a0-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="003a0-106">Пропускная способность сети является ограниченным ресурсом.</span><span class="sxs-lookup"><span data-stu-id="003a0-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="003a0-107">Уменьшение размера ответа обычно часто значительно увеличивается скорость реагирования приложения.</span><span class="sxs-lookup"><span data-stu-id="003a0-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="003a0-108">Для сжатия ответов приложения является одним из способов уменьшить размеры полезной нагрузки.</span><span class="sxs-lookup"><span data-stu-id="003a0-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="003a0-109">Когда следует использовать по промежуточного слоя для сжатия ответов</span><span class="sxs-lookup"><span data-stu-id="003a0-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="003a0-110">Использование технологий сжатия ответ на основе сервера в IIS, Apache или Nginx.</span><span class="sxs-lookup"><span data-stu-id="003a0-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="003a0-111">Производительность по промежуточного слоя скорее всего, не соответствует серверных модулей.</span><span class="sxs-lookup"><span data-stu-id="003a0-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="003a0-112">[Сервер HTTP.sys](xref:fundamentals/servers/httpsys) сервера и [Kestrel](xref:fundamentals/servers/kestrel) server сейчас не предлагают поддержку в встроенную функцию сжатия.</span><span class="sxs-lookup"><span data-stu-id="003a0-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="003a0-113">Используйте по промежуточного слоя для сжатия ответов, когда вы будете:</span><span class="sxs-lookup"><span data-stu-id="003a0-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="003a0-114">Не удается использовать со следующими технологиями сжатия на основе сервера:</span><span class="sxs-lookup"><span data-stu-id="003a0-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="003a0-115">Модуль динамического сжатия служб IIS</span><span class="sxs-lookup"><span data-stu-id="003a0-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="003a0-116">Модуль mod_deflate Apache</span><span class="sxs-lookup"><span data-stu-id="003a0-116">Apache mod_deflate module</span></span>](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="003a0-117">Nginx сжатия и распаковки</span><span class="sxs-lookup"><span data-stu-id="003a0-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="003a0-118">Размещение непосредственно на:</span><span class="sxs-lookup"><span data-stu-id="003a0-118">Hosting directly on:</span></span>
  * <span data-ttu-id="003a0-119">[Сервер HTTP.sys](xref:fundamentals/servers/httpsys) (ранее называвшихся WebListener)</span><span class="sxs-lookup"><span data-stu-id="003a0-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="003a0-120">Сервер kestrel</span><span class="sxs-lookup"><span data-stu-id="003a0-120">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="003a0-121">Сжатие ответов</span><span class="sxs-lookup"><span data-stu-id="003a0-121">Response compression</span></span>

<span data-ttu-id="003a0-122">Как правило любой ответ, изначально не сжаты могут использовать преимущества сжатия отклика.</span><span class="sxs-lookup"><span data-stu-id="003a0-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="003a0-123">Ответы, изначально не сжаты обычно включают: CSS, JavaScript, HTML, XML и JSON.</span><span class="sxs-lookup"><span data-stu-id="003a0-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="003a0-124">Не следует сжимать изначально сжатых ресурсов, таких как PNG-файлы.</span><span class="sxs-lookup"><span data-stu-id="003a0-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="003a0-125">При попытке дальнейшее сжатие изначально сжатый ответ, небольшого дополнительного сокращения размера и передачи времени скорее всего будет злоумышленниками время, которое потребовалось для обработки сжатия.</span><span class="sxs-lookup"><span data-stu-id="003a0-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="003a0-126">Не сжимать файлы размером меньше примерно 150 – 1000 байт (в зависимости от его содержимого и повысить эффективность сжатия).</span><span class="sxs-lookup"><span data-stu-id="003a0-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="003a0-127">Затраты на сжатие небольших файлов могут давать сжатый файл, размер которых превышает несжатый файл.</span><span class="sxs-lookup"><span data-stu-id="003a0-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="003a0-128">Когда клиент может обрабатывать сжатое содержимое, клиент должен уведомления сервера его возможности, отправляя `Accept-Encoding` заголовок с запросом.</span><span class="sxs-lookup"><span data-stu-id="003a0-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="003a0-129">Когда сервер отправляет сжатое содержимое, она должна содержать сведения в `Content-Encoding` заголовка на способах кодировки сжатого ответа.</span><span class="sxs-lookup"><span data-stu-id="003a0-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="003a0-130">В следующей таблице показаны содержимого обозначения кодировки, поддерживаемые по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="003a0-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="003a0-131">`Accept-Encoding` значения заголовка</span><span class="sxs-lookup"><span data-stu-id="003a0-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="003a0-132">Поддерживается по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="003a0-132">Middleware Supported</span></span> | <span data-ttu-id="003a0-133">Описание</span><span class="sxs-lookup"><span data-stu-id="003a0-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="003a0-134">Да (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="003a0-134">Yes (default)</span></span>        | [<span data-ttu-id="003a0-135">Формат сжатых данных Brotli</span><span class="sxs-lookup"><span data-stu-id="003a0-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="003a0-136">Нет</span><span class="sxs-lookup"><span data-stu-id="003a0-136">No</span></span>                   | [<span data-ttu-id="003a0-137">Формат DEFLATE сжатых данных</span><span class="sxs-lookup"><span data-stu-id="003a0-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="003a0-138">Нет</span><span class="sxs-lookup"><span data-stu-id="003a0-138">No</span></span>                   | [<span data-ttu-id="003a0-139">W3C XML для эффективного обмена</span><span class="sxs-lookup"><span data-stu-id="003a0-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="003a0-140">Да</span><span class="sxs-lookup"><span data-stu-id="003a0-140">Yes</span></span>                  | [<span data-ttu-id="003a0-141">Формат файла gzip</span><span class="sxs-lookup"><span data-stu-id="003a0-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="003a0-142">Да</span><span class="sxs-lookup"><span data-stu-id="003a0-142">Yes</span></span>                  | <span data-ttu-id="003a0-143">Идентификатор «Без кодировки»: Ответ не должен быть закодирован.</span><span class="sxs-lookup"><span data-stu-id="003a0-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="003a0-144">Нет</span><span class="sxs-lookup"><span data-stu-id="003a0-144">No</span></span>                   | [<span data-ttu-id="003a0-145">Сетевой формат передачи архивы Java</span><span class="sxs-lookup"><span data-stu-id="003a0-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="003a0-146">Да</span><span class="sxs-lookup"><span data-stu-id="003a0-146">Yes</span></span>                  | <span data-ttu-id="003a0-147">Любое доступное содержимое, кодировка не явно запрошенного</span><span class="sxs-lookup"><span data-stu-id="003a0-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="003a0-148">`Accept-Encoding` значения заголовка</span><span class="sxs-lookup"><span data-stu-id="003a0-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="003a0-149">Поддерживается по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="003a0-149">Middleware Supported</span></span> | <span data-ttu-id="003a0-150">Описание</span><span class="sxs-lookup"><span data-stu-id="003a0-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="003a0-151">Нет</span><span class="sxs-lookup"><span data-stu-id="003a0-151">No</span></span>                   | [<span data-ttu-id="003a0-152">Формат сжатых данных Brotli</span><span class="sxs-lookup"><span data-stu-id="003a0-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="003a0-153">Нет</span><span class="sxs-lookup"><span data-stu-id="003a0-153">No</span></span>                   | [<span data-ttu-id="003a0-154">Формат DEFLATE сжатых данных</span><span class="sxs-lookup"><span data-stu-id="003a0-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="003a0-155">Нет</span><span class="sxs-lookup"><span data-stu-id="003a0-155">No</span></span>                   | [<span data-ttu-id="003a0-156">W3C XML для эффективного обмена</span><span class="sxs-lookup"><span data-stu-id="003a0-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="003a0-157">Да (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="003a0-157">Yes (default)</span></span>        | [<span data-ttu-id="003a0-158">Формат файла gzip</span><span class="sxs-lookup"><span data-stu-id="003a0-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="003a0-159">Да</span><span class="sxs-lookup"><span data-stu-id="003a0-159">Yes</span></span>                  | <span data-ttu-id="003a0-160">Идентификатор «Без кодировки»: Ответ не должен быть закодирован.</span><span class="sxs-lookup"><span data-stu-id="003a0-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="003a0-161">Нет</span><span class="sxs-lookup"><span data-stu-id="003a0-161">No</span></span>                   | [<span data-ttu-id="003a0-162">Сетевой формат передачи архивы Java</span><span class="sxs-lookup"><span data-stu-id="003a0-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="003a0-163">Да</span><span class="sxs-lookup"><span data-stu-id="003a0-163">Yes</span></span>                  | <span data-ttu-id="003a0-164">Любое доступное содержимое, кодировка не явно запрошенного</span><span class="sxs-lookup"><span data-stu-id="003a0-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="003a0-165">Дополнительные сведения см. в разделе [IANA официальный кодирования списка содержимого](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="003a0-165">For more information, see the [IANA Official Content Coding List](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="003a0-166">По промежуточного слоя можно добавить дополнительное сжатие поставщиков для пользовательских `Accept-Encoding` значения заголовка.</span><span class="sxs-lookup"><span data-stu-id="003a0-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="003a0-167">Дополнительные сведения см. в разделе [настраиваемые поставщики](#custom-providers) ниже.</span><span class="sxs-lookup"><span data-stu-id="003a0-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="003a0-168">По промежуточного слоя способен реагирование на значение качества (qvalue, `q`) вес при отправке клиентом для определения приоритетов схемы сжатия.</span><span class="sxs-lookup"><span data-stu-id="003a0-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="003a0-169">Дополнительные сведения см. в разделе [RFC 7231: Приемлемой кодировкой](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="003a0-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="003a0-170">Алгоритмы сжатия, распространяются компромисс между скоростью сжатие и эффективность сжатия.</span><span class="sxs-lookup"><span data-stu-id="003a0-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="003a0-171">*Эффективность* в данном контексте означает размер выходных данных после сжатия.</span><span class="sxs-lookup"><span data-stu-id="003a0-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="003a0-172">Наименьший размер достигается за счет наиболее *оптимальной* сжатия.</span><span class="sxs-lookup"><span data-stu-id="003a0-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="003a0-173">Заголовки, участвующих в запросе, отправки, кэширование и получать сжатое содержимое описаны в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="003a0-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="003a0-174">Header</span><span class="sxs-lookup"><span data-stu-id="003a0-174">Header</span></span>             | <span data-ttu-id="003a0-175">Роль</span><span class="sxs-lookup"><span data-stu-id="003a0-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="003a0-176">Отправленные клиентом на сервер для указания содержимого, кодирование схемы, допустимых для клиента.</span><span class="sxs-lookup"><span data-stu-id="003a0-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="003a0-177">Клиенту с сервера для обозначения кодировки содержимого в полезных данных.</span><span class="sxs-lookup"><span data-stu-id="003a0-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="003a0-178">В случае сжатия `Content-Length` заголовок удаляется с момента изменения содержимого текста при ответе сжимается.</span><span class="sxs-lookup"><span data-stu-id="003a0-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="003a0-179">В случае сжатия `Content-MD5` заголовок удаляется, так как содержимое тела был изменен и хэш-код больше не является допустимым.</span><span class="sxs-lookup"><span data-stu-id="003a0-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="003a0-180">Указывает тип MIME содержимого.</span><span class="sxs-lookup"><span data-stu-id="003a0-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="003a0-181">Каждый ответ следует указать его `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="003a0-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="003a0-182">По промежуточного слоя проверяет это значение, чтобы определить, следует ли сжимать ответа.</span><span class="sxs-lookup"><span data-stu-id="003a0-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="003a0-183">По промежуточного слоя задает набор [по умолчанию типы MIME](#mime-types) , его можно закодировать, но можно заменить или добавить типы MIME.</span><span class="sxs-lookup"><span data-stu-id="003a0-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="003a0-184">При отправке сервером со значением `Accept-Encoding` для клиентов и прокси-серверы, `Vary` заголовок указывает клиенту или прокси-сервер, его следует кэшировать (различаться) ответы на основе значения из `Accept-Encoding` заголовок запроса.</span><span class="sxs-lookup"><span data-stu-id="003a0-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="003a0-185">Результат возврата содержимого с помощью `Vary: Accept-Encoding` заголовка является как краткая, так и без сжатия ответов, кэшируются отдельно.</span><span class="sxs-lookup"><span data-stu-id="003a0-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="003a0-186">Изучите возможности по промежуточного слоя сжатия ответов с [пример приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="003a0-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="003a0-187">В образце показано:</span><span class="sxs-lookup"><span data-stu-id="003a0-187">The sample illustrates:</span></span>

* <span data-ttu-id="003a0-188">Сжатие ответов приложения с помощью Gzip и сжатие пользовательских поставщиков.</span><span class="sxs-lookup"><span data-stu-id="003a0-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="003a0-189">Как добавить тип MIME по умолчанию список типов MIME для сжатия.</span><span class="sxs-lookup"><span data-stu-id="003a0-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="003a0-190">Пакет</span><span class="sxs-lookup"><span data-stu-id="003a0-190">Package</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="003a0-191">Чтобы включить по промежуточного слоя в проекте, добавьте ссылку на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), который включает [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) пакета.</span><span class="sxs-lookup"><span data-stu-id="003a0-191">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="003a0-192">Чтобы включить по промежуточного слоя в проекте, добавьте ссылку на [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage), который включает [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) пакета.</span><span class="sxs-lookup"><span data-stu-id="003a0-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="003a0-193">Чтобы включить по промежуточного слоя в проекте, добавьте ссылку на [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) пакета.</span><span class="sxs-lookup"><span data-stu-id="003a0-193">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="003a0-194">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="003a0-194">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="003a0-195">Ниже показано, как включить по промежуточного слоя сжатия ответов типы MIME по умолчанию, а также поставщиков сжатия ([Brotli](#brotli-compression-provider) и [Gzip](#gzip-compression-provider)):</span><span class="sxs-lookup"><span data-stu-id="003a0-195">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="003a0-196">Ниже показано, как включить по промежуточного слоя сжатия ответов для типов MIME по умолчанию и [поставщика сжатие Gzip](#gzip-compression-provider):</span><span class="sxs-lookup"><span data-stu-id="003a0-196">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

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

<span data-ttu-id="003a0-197">Примечания.</span><span class="sxs-lookup"><span data-stu-id="003a0-197">Notes:</span></span>

* <span data-ttu-id="003a0-198">`app.UseResponseCompression` должен вызываться перед `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="003a0-198">`app.UseResponseCompression` must be called before `app.UseMvc`.</span></span>
* <span data-ttu-id="003a0-199">Используйте это средство, например [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), или [Postman](https://www.getpostman.com/) присвоить `Accept-Encoding` заголовок запроса и изучите заголовки ответа, размер и текст.</span><span class="sxs-lookup"><span data-stu-id="003a0-199">Use a tool such as [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="003a0-200">Отправить запрос в пример приложения без `Accept-Encoding` заголовка и обратите внимание, что ответ без сжатия.</span><span class="sxs-lookup"><span data-stu-id="003a0-200">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="003a0-201">`Content-Encoding` И `Vary` заголовки не присутствуют в ответе.</span><span class="sxs-lookup"><span data-stu-id="003a0-201">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Fiddler окно, отображающее результат запроса без заголовка Accept-Encoding.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="003a0-204">Отправить запрос в пример приложения с `Accept-Encoding: br` заголовка (Brotli сжатие) и обратите внимание, что ответ сжата.</span><span class="sxs-lookup"><span data-stu-id="003a0-204">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="003a0-205">`Content-Encoding` И `Vary` заголовки присутствуют в ответе.</span><span class="sxs-lookup"><span data-stu-id="003a0-205">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Fiddler окно, отображающее результат запроса с заголовком Accept-Encoding и значение br.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="003a0-209">Отправить запрос в пример приложения с `Accept-Encoding: gzip` заголовка и обратите внимание, что ответ сжата.</span><span class="sxs-lookup"><span data-stu-id="003a0-209">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="003a0-210">`Content-Encoding` И `Vary` заголовки присутствуют в ответе.</span><span class="sxs-lookup"><span data-stu-id="003a0-210">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Fiddler окно, отображающее результат запроса с заголовком Accept-Encoding и значение gzip.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a><span data-ttu-id="003a0-214">Поставщики</span><span class="sxs-lookup"><span data-stu-id="003a0-214">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="003a0-215">Поставщик сжатие Brotli</span><span class="sxs-lookup"><span data-stu-id="003a0-215">Brotli Compression Provider</span></span>

<span data-ttu-id="003a0-216">Используйте <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> для сжатия ответов с [формат сжатых данных Brotli](https://tools.ietf.org/html/rfc7932).</span><span class="sxs-lookup"><span data-stu-id="003a0-216">Use the <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="003a0-217">Если поставщики отсутствуют сжатия явно добавляются <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="003a0-217">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="003a0-218">Массив поставщиков сжатия вместе с по умолчанию добавляется поставщик сжатие Brotli [поставщика сжатие Gzip](#gzip-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="003a0-218">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="003a0-219">По умолчанию сжатие Brotli сжатие при Brotli формат сжатых данных, поддерживаемые клиентом.</span><span class="sxs-lookup"><span data-stu-id="003a0-219">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="003a0-220">Если клиент не поддерживается Brotli, сжатие по умолчанию в Gzip Если клиент поддерживает сжатие Gzip.</span><span class="sxs-lookup"><span data-stu-id="003a0-220">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="003a0-221">Поставщик сжатия Brotoli должен быть добавлен, при любых поставщиков сжатия добавляются явным образом:</span><span class="sxs-lookup"><span data-stu-id="003a0-221">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="003a0-222">Установка уровня с помощью сжатия <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="003a0-222">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span></span> <span data-ttu-id="003a0-223">По умолчанию используется поставщик сжатие Brotli наиболее быстрый уровень сжатия ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), который может не обеспечить наиболее эффективное сжатие.</span><span class="sxs-lookup"><span data-stu-id="003a0-223">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="003a0-224">При необходимости наиболее эффективного сжатия настройте по промежуточного слоя для оптимального сжатия.</span><span class="sxs-lookup"><span data-stu-id="003a0-224">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="003a0-225">Уровень сжатия</span><span class="sxs-lookup"><span data-stu-id="003a0-225">Compression Level</span></span> | <span data-ttu-id="003a0-226">Описание</span><span class="sxs-lookup"><span data-stu-id="003a0-226">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="003a0-227">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="003a0-227">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="003a0-228">Сжатие следует выполнить как можно быстрее, даже если полученный результат не сжат оптимально.</span><span class="sxs-lookup"><span data-stu-id="003a0-228">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="003a0-229">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="003a0-229">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="003a0-230">Не требуется сжимать.</span><span class="sxs-lookup"><span data-stu-id="003a0-230">No compression should be performed.</span></span> |
| [<span data-ttu-id="003a0-231">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="003a0-231">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="003a0-232">Ответы должно применяться оптимальное сжатие, даже если сжатие занимает больше времени.</span><span class="sxs-lookup"><span data-stu-id="003a0-232">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="gzip-compression-provider"></a><span data-ttu-id="003a0-233">Поставщик сжатие gzip</span><span class="sxs-lookup"><span data-stu-id="003a0-233">Gzip Compression Provider</span></span>

<span data-ttu-id="003a0-234">Используйте <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> для сжатия ответов с [формате Gzip](https://tools.ietf.org/html/rfc1952).</span><span class="sxs-lookup"><span data-stu-id="003a0-234">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="003a0-235">Если поставщики отсутствуют сжатия явно добавляются <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="003a0-235">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="003a0-236">Массив поставщиков сжатия вместе с по умолчанию добавляется поставщик сжатие Gzip [поставщика сжатие Brotli](#brotli-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="003a0-236">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="003a0-237">По умолчанию сжатие Brotli сжатие при Brotli формат сжатых данных, поддерживаемые клиентом.</span><span class="sxs-lookup"><span data-stu-id="003a0-237">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="003a0-238">Если клиент не поддерживается Brotli, сжатие по умолчанию в Gzip Если клиент поддерживает сжатие Gzip.</span><span class="sxs-lookup"><span data-stu-id="003a0-238">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="003a0-239">Массив поставщиков сжатия по умолчанию добавляется поставщик сжатия Gzip.</span><span class="sxs-lookup"><span data-stu-id="003a0-239">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="003a0-240">По умолчанию сжатие Gzip, если клиент поддерживает сжатие Gzip.</span><span class="sxs-lookup"><span data-stu-id="003a0-240">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="003a0-241">Сжатие Gzip поставщик должен быть добавлен, при любых поставщиков сжатия явно добавляются:</span><span class="sxs-lookup"><span data-stu-id="003a0-241">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5)]

::: moniker-end

<span data-ttu-id="003a0-242">Установка уровня с помощью сжатия <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="003a0-242">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="003a0-243">По умолчанию используется поставщик сжатие Gzip наиболее быстрый уровень сжатия ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), который может не обеспечить наиболее эффективное сжатие.</span><span class="sxs-lookup"><span data-stu-id="003a0-243">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="003a0-244">При необходимости наиболее эффективного сжатия настройте по промежуточного слоя для оптимального сжатия.</span><span class="sxs-lookup"><span data-stu-id="003a0-244">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="003a0-245">Уровень сжатия</span><span class="sxs-lookup"><span data-stu-id="003a0-245">Compression Level</span></span> | <span data-ttu-id="003a0-246">Описание</span><span class="sxs-lookup"><span data-stu-id="003a0-246">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="003a0-247">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="003a0-247">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="003a0-248">Сжатие следует выполнить как можно быстрее, даже если полученный результат не сжат оптимально.</span><span class="sxs-lookup"><span data-stu-id="003a0-248">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="003a0-249">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="003a0-249">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="003a0-250">Не требуется сжимать.</span><span class="sxs-lookup"><span data-stu-id="003a0-250">No compression should be performed.</span></span> |
| [<span data-ttu-id="003a0-251">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="003a0-251">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="003a0-252">Ответы должно применяться оптимальное сжатие, даже если сжатие занимает больше времени.</span><span class="sxs-lookup"><span data-stu-id="003a0-252">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="custom-providers"></a><span data-ttu-id="003a0-253">Настраиваемые поставщики</span><span class="sxs-lookup"><span data-stu-id="003a0-253">Custom providers</span></span>

<span data-ttu-id="003a0-254">Создание реализации сжатия с <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span><span class="sxs-lookup"><span data-stu-id="003a0-254">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="003a0-255"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> Представляет содержимое, кодировка, что этот `ICompressionProvider` создает.</span><span class="sxs-lookup"><span data-stu-id="003a0-255">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="003a0-256">По промежуточного слоя использует эти сведения можно выбрать поставщика, на основе списка, указанное в `Accept-Encoding` заголовок запроса.</span><span class="sxs-lookup"><span data-stu-id="003a0-256">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="003a0-257">С помощью примера приложения, клиент отправляет запрос с помощью `Accept-Encoding: mycustomcompression` заголовка.</span><span class="sxs-lookup"><span data-stu-id="003a0-257">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="003a0-258">По промежуточного слоя использует реализацию сжатия и возвращает ответ с `Content-Encoding: mycustomcompression` заголовка.</span><span class="sxs-lookup"><span data-stu-id="003a0-258">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="003a0-259">Клиент должен быть способен распаковать настраиваемую кодировку в порядке для реализации пользовательских сжатия для работы.</span><span class="sxs-lookup"><span data-stu-id="003a0-259">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="003a0-260">Отправить запрос в пример приложения с `Accept-Encoding: mycustomcompression` заголовка и обратите внимание на заголовки ответа.</span><span class="sxs-lookup"><span data-stu-id="003a0-260">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="003a0-261">`Vary` И `Content-Encoding` заголовки присутствуют в ответе.</span><span class="sxs-lookup"><span data-stu-id="003a0-261">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="003a0-262">Текст ответа (не показано) не сжимается в образце.</span><span class="sxs-lookup"><span data-stu-id="003a0-262">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="003a0-263">Отсутствует реализация сжатия в `CustomCompressionProvider` класс образца.</span><span class="sxs-lookup"><span data-stu-id="003a0-263">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="003a0-264">Тем не менее в образце показано, где вам понадобилось бы реализовать алгоритм сжатия.</span><span class="sxs-lookup"><span data-stu-id="003a0-264">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Fiddler окно, отображающее результат запроса с заголовком Accept-Encoding и значение mycustomcompression.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="003a0-267">типы MIME</span><span class="sxs-lookup"><span data-stu-id="003a0-267">MIME types</span></span>

<span data-ttu-id="003a0-268">По промежуточного слоя задает набор по умолчанию типы MIME для сжатия:</span><span class="sxs-lookup"><span data-stu-id="003a0-268">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="003a0-269">Заменить или добавить типы MIME с параметрами по промежуточного слоя для сжатия ответов.</span><span class="sxs-lookup"><span data-stu-id="003a0-269">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="003a0-270">Обратите внимание, что подстановочный знак MIME типы, такие как `text/*` не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="003a0-270">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="003a0-271">Пример приложения добавляет тип MIME для `image/svg+xml` и сжимает и служит изображение баннера в ASP.NET Core (*banner.svg*).</span><span class="sxs-lookup"><span data-stu-id="003a0-271">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="003a0-272">Сжатие с помощью безопасного протокола</span><span class="sxs-lookup"><span data-stu-id="003a0-272">Compression with secure protocol</span></span>

<span data-ttu-id="003a0-273">Сжатые ответы по безопасным соединениям можно управлять с помощью `EnableForHttps` параметр, который по умолчанию отключена.</span><span class="sxs-lookup"><span data-stu-id="003a0-273">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="003a0-274">Использование сжатия с динамически созданных страниц может привести к проблемам безопасности таких как [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) и [нарушения](https://wikipedia.org/wiki/BREACH_(security_exploit)) атак.</span><span class="sxs-lookup"><span data-stu-id="003a0-274">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="003a0-275">Добавление заголовка Vary</span><span class="sxs-lookup"><span data-stu-id="003a0-275">Adding the Vary header</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="003a0-276">Если сжатие ответов на основе `Accept-Encoding` заголовок, потенциально нескольких версий сжатого ответа и несжатую версию.</span><span class="sxs-lookup"><span data-stu-id="003a0-276">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="003a0-277">Чтобы настроить кэш клиента и прокси-сервера, существует несколько версий, а также должны быть сохранены, `Vary` заголовок добавляется с `Accept-Encoding` значение.</span><span class="sxs-lookup"><span data-stu-id="003a0-277">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="003a0-278">В ASP.NET Core 2.0 или более поздней версии, по промежуточного слоя добавляет `Vary` заголовка автоматически в том случае, когда ответ сжимается.</span><span class="sxs-lookup"><span data-stu-id="003a0-278">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="003a0-279">Если сжатие ответов на основе `Accept-Encoding` заголовок, потенциально нескольких версий сжатого ответа и несжатую версию.</span><span class="sxs-lookup"><span data-stu-id="003a0-279">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="003a0-280">Чтобы настроить кэш клиента и прокси-сервера, существует несколько версий, а также должны быть сохранены, `Vary` заголовок добавляется с `Accept-Encoding` значение.</span><span class="sxs-lookup"><span data-stu-id="003a0-280">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="003a0-281">В ASP.NET Core 1.x, добавление `Vary` в ответ заголовок выполняется вручную:</span><span class="sxs-lookup"><span data-stu-id="003a0-281">In ASP.NET Core 1.x, adding the `Vary` header to the response is accomplished manually:</span></span>

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="003a0-282">По промежуточного слоя проблемы при работе за Nginx обратный прокси-сервер</span><span class="sxs-lookup"><span data-stu-id="003a0-282">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="003a0-283">При наличии запроса, передаются Nginx, `Accept-Encoding` заголовок удаляется.</span><span class="sxs-lookup"><span data-stu-id="003a0-283">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="003a0-284">Удаление `Accept-Encoding` заголовок запрещает сжатие ответ по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="003a0-284">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="003a0-285">Дополнительные сведения см. в статье об [использовании Сжатие и распаковку](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="003a0-285">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="003a0-286">Эта проблема отслеживается [выяснить сквозной сжатие для Nginx (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="003a0-286">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="003a0-287">Работа с динамического сжатия служб IIS</span><span class="sxs-lookup"><span data-stu-id="003a0-287">Working with IIS dynamic compression</span></span>

<span data-ttu-id="003a0-288">При наличии активных динамического сжатия модуль IIS настроен на уровне сервера, который вы хотите отключить для приложений, отключите модуль с дополнением к *web.config* файл.</span><span class="sxs-lookup"><span data-stu-id="003a0-288">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="003a0-289">Дополнительные сведения см. в разделе [Отключение модулей IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="003a0-289">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="003a0-290">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="003a0-290">Troubleshooting</span></span>

<span data-ttu-id="003a0-291">Использовать такой инструмент, как [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), или [Postman](https://www.getpostman.com/), которые позволяют задать `Accept-Encoding` заголовок запроса и изучите заголовки ответа, размер и текст.</span><span class="sxs-lookup"><span data-stu-id="003a0-291">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="003a0-292">По умолчанию по промежуточного слоя для сжатия ответов сжимает ответы, которые отвечают следующим условиям:</span><span class="sxs-lookup"><span data-stu-id="003a0-292">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="003a0-293">`Accept-Encoding` Заголовок присутствует со значением `br`, `gzip`, `*`, или настраиваемую кодировку, соответствующий поставщику сжатия, который вы определили.</span><span class="sxs-lookup"><span data-stu-id="003a0-293">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="003a0-294">Значение не должно быть `identity` или иметь значение качества (qvalue, `q`) значение 0 (ноль).</span><span class="sxs-lookup"><span data-stu-id="003a0-294">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="003a0-295">Тип MIME (`Content-Type`) должны быть заданы и должен совпадать с типом MIME, настроенный на <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="003a0-295">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="003a0-296">Запрос не должен содержать `Content-Range` заголовка.</span><span class="sxs-lookup"><span data-stu-id="003a0-296">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="003a0-297">Запрос должен использовать незащищенный протокол (http), только если в параметрах по промежуточного слоя для сжатия ответов настроена безопасный протокол (https).</span><span class="sxs-lookup"><span data-stu-id="003a0-297">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="003a0-298">*Обратите внимание, что приведет к неисправности [описанных выше](#compression-with-secure-protocol) при включении безопасного сжатие содержимого.*</span><span class="sxs-lookup"><span data-stu-id="003a0-298">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="003a0-299">`Accept-Encoding` Заголовок присутствует со значением `gzip`, `*`, или настраиваемую кодировку, соответствующий поставщику сжатия, который вы определили.</span><span class="sxs-lookup"><span data-stu-id="003a0-299">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="003a0-300">Значение не должно быть `identity` или иметь значение качества (qvalue, `q`) значение 0 (ноль).</span><span class="sxs-lookup"><span data-stu-id="003a0-300">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="003a0-301">Тип MIME (`Content-Type`) должны быть заданы и должен совпадать с типом MIME, настроенный на <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="003a0-301">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="003a0-302">Запрос не должен содержать `Content-Range` заголовка.</span><span class="sxs-lookup"><span data-stu-id="003a0-302">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="003a0-303">Запрос должен использовать незащищенный протокол (http), только если в параметрах по промежуточного слоя для сжатия ответов настроена безопасный протокол (https).</span><span class="sxs-lookup"><span data-stu-id="003a0-303">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="003a0-304">*Обратите внимание, что приведет к неисправности [описанных выше](#compression-with-secure-protocol) при включении безопасного сжатие содержимого.*</span><span class="sxs-lookup"><span data-stu-id="003a0-304">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="003a0-305">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="003a0-305">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="003a0-306">Сеть разработчиков Mozilla: Приемлемой кодировкой</span><span class="sxs-lookup"><span data-stu-id="003a0-306">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="003a0-307">Раздел RFC 7231 3.1.2.1: Codings содержимого</span><span class="sxs-lookup"><span data-stu-id="003a0-307">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="003a0-308">RFC 7230 разделе 4.2.3: Кодирование в gzip</span><span class="sxs-lookup"><span data-stu-id="003a0-308">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="003a0-309">Версия спецификации формата файла GZIP 4.3</span><span class="sxs-lookup"><span data-stu-id="003a0-309">GZIP file format specification version 4.3</span></span>](https://www.ietf.org/rfc/rfc1952.txt)
