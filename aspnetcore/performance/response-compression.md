---
title: Сжатие ответов в ASP.NET Core
author: guardrex
description: Сведения о сжатии откликов и способах использования ПО промежуточного слоя для сжатия откликов в приложениях ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/09/2019
uid: performance/response-compression
ms.openlocfilehash: e320e87179f9f1b9773a55c380684a3f3f712632
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/13/2019
ms.locfileid: "68993463"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="dae0e-103">Сжатие ответов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dae0e-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="dae0e-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="dae0e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="dae0e-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dae0e-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="dae0e-106">Пропускная способность сети является ограниченным ресурсом.</span><span class="sxs-lookup"><span data-stu-id="dae0e-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="dae0e-107">Уменьшение размера ответа обычно значительно увеличивает скорость реагирования приложения.</span><span class="sxs-lookup"><span data-stu-id="dae0e-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="dae0e-108">Одним из способов уменьшения размера полезных данных является сжатие ответов приложения.</span><span class="sxs-lookup"><span data-stu-id="dae0e-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="dae0e-109">Использование по промежуточного слоя сжатия ответа</span><span class="sxs-lookup"><span data-stu-id="dae0e-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="dae0e-110">Используйте технологии сжатия серверных ответов на базе IIS, Apache или nginx.</span><span class="sxs-lookup"><span data-stu-id="dae0e-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="dae0e-111">Возможно, производительность по промежуточного слоя не соответствует числу серверных модулей.</span><span class="sxs-lookup"><span data-stu-id="dae0e-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="dae0e-112">Сервер [http. sys](xref:fundamentals/servers/httpsys) и сервер [Kestrel](xref:fundamentals/servers/kestrel) в настоящее время не предлагают встроенную поддержку сжатия.</span><span class="sxs-lookup"><span data-stu-id="dae0e-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="dae0e-113">Используйте по промежуточного слоя сжатия ответов, если:</span><span class="sxs-lookup"><span data-stu-id="dae0e-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="dae0e-114">Не удается использовать следующие технологии сжатия на основе серверов:</span><span class="sxs-lookup"><span data-stu-id="dae0e-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="dae0e-115">Модуль динамического сжатия IIS</span><span class="sxs-lookup"><span data-stu-id="dae0e-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="dae0e-116">Модуль Apache mod_deflate</span><span class="sxs-lookup"><span data-stu-id="dae0e-116">Apache mod_deflate module</span></span>](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="dae0e-117">Сжатие и распаковка nginx</span><span class="sxs-lookup"><span data-stu-id="dae0e-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="dae0e-118">Размещение непосредственно в:</span><span class="sxs-lookup"><span data-stu-id="dae0e-118">Hosting directly on:</span></span>
  * <span data-ttu-id="dae0e-119">[Сервер HTTP. sys](xref:fundamentals/servers/httpsys) (ранее назывался «прослушиваемый»)</span><span class="sxs-lookup"><span data-stu-id="dae0e-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="dae0e-120">Сервер Kestrel</span><span class="sxs-lookup"><span data-stu-id="dae0e-120">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="dae0e-121">Сжатие ответов</span><span class="sxs-lookup"><span data-stu-id="dae0e-121">Response compression</span></span>

<span data-ttu-id="dae0e-122">Как правило, любой ответ, не сжатый в собственном формате, может выиграть от сжатия ответа.</span><span class="sxs-lookup"><span data-stu-id="dae0e-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="dae0e-123">Ответы, не сжатые в собственном формате, обычно включают: CSS, JavaScript, HTML, XML и JSON.</span><span class="sxs-lookup"><span data-stu-id="dae0e-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="dae0e-124">Не следует сжимать в собственном формате активы, такие как PNG-файлы.</span><span class="sxs-lookup"><span data-stu-id="dae0e-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="dae0e-125">При попытке дальнейшего сжатия отклика, сжатого в собственном формате, любое небольшое уменьшение размера и времени передачи, скорее всего, будет превышено на время, затраченное на обработку сжатия.</span><span class="sxs-lookup"><span data-stu-id="dae0e-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="dae0e-126">Не сжимать файлы размером менее 150-1000 байт (в зависимости от содержимого файла и эффективности сжатия).</span><span class="sxs-lookup"><span data-stu-id="dae0e-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="dae0e-127">Затраты на сжатие мелких файлов могут привести к созданию сжатого файла, большего, чем несжатый файл.</span><span class="sxs-lookup"><span data-stu-id="dae0e-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="dae0e-128">Когда клиент может обработать сжатое содержимое, клиент должен сообщить серверу о своих возможностях, отправив `Accept-Encoding` заголовок с запросом.</span><span class="sxs-lookup"><span data-stu-id="dae0e-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="dae0e-129">Когда сервер отправляет сжатое содержимое, он должен содержать сведения в `Content-Encoding` заголовке процесса кодирования сжатого ответа.</span><span class="sxs-lookup"><span data-stu-id="dae0e-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="dae0e-130">В следующей таблице показаны конструкции кодирования содержимого, поддерживаемые по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="dae0e-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="dae0e-131">`Accept-Encoding`значения заголовка</span><span class="sxs-lookup"><span data-stu-id="dae0e-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="dae0e-132">Поддерживается по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="dae0e-132">Middleware Supported</span></span> | <span data-ttu-id="dae0e-133">Описание</span><span class="sxs-lookup"><span data-stu-id="dae0e-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="dae0e-134">Да (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="dae0e-134">Yes (default)</span></span>        | [<span data-ttu-id="dae0e-135">Формат сжатых данных Brotli</span><span class="sxs-lookup"><span data-stu-id="dae0e-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="dae0e-136">Нет</span><span class="sxs-lookup"><span data-stu-id="dae0e-136">No</span></span>                   | [<span data-ttu-id="dae0e-137">Сжатый формат сжатых данных</span><span class="sxs-lookup"><span data-stu-id="dae0e-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="dae0e-138">Нет</span><span class="sxs-lookup"><span data-stu-id="dae0e-138">No</span></span>                   | [<span data-ttu-id="dae0e-139">Эффективный XML-обмен в формате W3C</span><span class="sxs-lookup"><span data-stu-id="dae0e-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="dae0e-140">Да</span><span class="sxs-lookup"><span data-stu-id="dae0e-140">Yes</span></span>                  | [<span data-ttu-id="dae0e-141">Формат файла gzip</span><span class="sxs-lookup"><span data-stu-id="dae0e-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="dae0e-142">Да</span><span class="sxs-lookup"><span data-stu-id="dae0e-142">Yes</span></span>                  | <span data-ttu-id="dae0e-143">Идентификатор "без кодирования": Ответ не должен быть закодирован.</span><span class="sxs-lookup"><span data-stu-id="dae0e-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="dae0e-144">Нет</span><span class="sxs-lookup"><span data-stu-id="dae0e-144">No</span></span>                   | [<span data-ttu-id="dae0e-145">Формат сетевой пересылки для архивов Java</span><span class="sxs-lookup"><span data-stu-id="dae0e-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="dae0e-146">Да</span><span class="sxs-lookup"><span data-stu-id="dae0e-146">Yes</span></span>                  | <span data-ttu-id="dae0e-147">Любая доступная кодировка содержимого, которая не запрашивается явно</span><span class="sxs-lookup"><span data-stu-id="dae0e-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="dae0e-148">`Accept-Encoding`значения заголовка</span><span class="sxs-lookup"><span data-stu-id="dae0e-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="dae0e-149">Поддерживается по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="dae0e-149">Middleware Supported</span></span> | <span data-ttu-id="dae0e-150">Описание</span><span class="sxs-lookup"><span data-stu-id="dae0e-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="dae0e-151">Нет</span><span class="sxs-lookup"><span data-stu-id="dae0e-151">No</span></span>                   | [<span data-ttu-id="dae0e-152">Формат сжатых данных Brotli</span><span class="sxs-lookup"><span data-stu-id="dae0e-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="dae0e-153">Нет</span><span class="sxs-lookup"><span data-stu-id="dae0e-153">No</span></span>                   | [<span data-ttu-id="dae0e-154">Сжатый формат сжатых данных</span><span class="sxs-lookup"><span data-stu-id="dae0e-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="dae0e-155">Нет</span><span class="sxs-lookup"><span data-stu-id="dae0e-155">No</span></span>                   | [<span data-ttu-id="dae0e-156">Эффективный XML-обмен в формате W3C</span><span class="sxs-lookup"><span data-stu-id="dae0e-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="dae0e-157">Да (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="dae0e-157">Yes (default)</span></span>        | [<span data-ttu-id="dae0e-158">Формат файла gzip</span><span class="sxs-lookup"><span data-stu-id="dae0e-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="dae0e-159">Да</span><span class="sxs-lookup"><span data-stu-id="dae0e-159">Yes</span></span>                  | <span data-ttu-id="dae0e-160">Идентификатор "без кодирования": Ответ не должен быть закодирован.</span><span class="sxs-lookup"><span data-stu-id="dae0e-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="dae0e-161">Нет</span><span class="sxs-lookup"><span data-stu-id="dae0e-161">No</span></span>                   | [<span data-ttu-id="dae0e-162">Формат сетевой пересылки для архивов Java</span><span class="sxs-lookup"><span data-stu-id="dae0e-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="dae0e-163">Да</span><span class="sxs-lookup"><span data-stu-id="dae0e-163">Yes</span></span>                  | <span data-ttu-id="dae0e-164">Любая доступная кодировка содержимого, которая не запрашивается явно</span><span class="sxs-lookup"><span data-stu-id="dae0e-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="dae0e-165">Дополнительные сведения см. в [списке официального кодирования содержимого IANA](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="dae0e-165">For more information, see the [IANA Official Content Coding List](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="dae0e-166">По промежуточного слоя позволяет добавлять дополнительные поставщики сжатия для значений `Accept-Encoding` пользовательских заголовков.</span><span class="sxs-lookup"><span data-stu-id="dae0e-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="dae0e-167">Дополнительные сведения см. в разделе [Настраиваемые поставщики](#custom-providers) ниже.</span><span class="sxs-lookup"><span data-stu-id="dae0e-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="dae0e-168">По промежуточного слоя может отреагировать на весовые коэффициенты качества `q`(квалуе), которые отправляются клиентом для определения приоритета схем сжатия.</span><span class="sxs-lookup"><span data-stu-id="dae0e-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="dae0e-169">Дополнительные сведения см [. в RFC 7231: Accept — Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="dae0e-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="dae0e-170">Алгоритмы сжатия подчиняются компромиссу между скоростью сжатия и эффективностью сжатия.</span><span class="sxs-lookup"><span data-stu-id="dae0e-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="dae0e-171">*Эффективность* в этом контексте означает размер выходных данных после сжатия.</span><span class="sxs-lookup"><span data-stu-id="dae0e-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="dae0e-172">Наименьший размер достигается самым оптимальным сжатием.</span><span class="sxs-lookup"><span data-stu-id="dae0e-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="dae0e-173">Заголовки, используемые для запроса, отправки, кэширования и получения сжатого содержимого, описаны в таблице ниже.</span><span class="sxs-lookup"><span data-stu-id="dae0e-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="dae0e-174">Header</span><span class="sxs-lookup"><span data-stu-id="dae0e-174">Header</span></span>             | <span data-ttu-id="dae0e-175">Роль</span><span class="sxs-lookup"><span data-stu-id="dae0e-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="dae0e-176">Отправляется с клиента на сервер, чтобы указать схемы кодировки содержимого, приемлемые для клиента.</span><span class="sxs-lookup"><span data-stu-id="dae0e-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="dae0e-177">Отправляется с сервера клиенту для указания кодировки содержимого в полезных данных.</span><span class="sxs-lookup"><span data-stu-id="dae0e-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="dae0e-178">Когда происходит сжатие, `Content-Length` заголовок удаляется, так как содержимое текста изменяется при сжатии ответа.</span><span class="sxs-lookup"><span data-stu-id="dae0e-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="dae0e-179">Когда происходит сжатие, `Content-MD5` заголовок удаляется, так как содержимое текста изменилось и хэш больше не является допустимым.</span><span class="sxs-lookup"><span data-stu-id="dae0e-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="dae0e-180">Указывает тип MIME содержимого.</span><span class="sxs-lookup"><span data-stu-id="dae0e-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="dae0e-181">Каждый ответ должен указывать его `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="dae0e-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="dae0e-182">По промежуточного слоя проверяет это значение, чтобы определить, следует ли сжимать ответ.</span><span class="sxs-lookup"><span data-stu-id="dae0e-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="dae0e-183">По промежуточного слоя указывает набор [типов MIME по умолчанию](#mime-types) , которые он может кодировать, но можно заменить или добавить типы MIME.</span><span class="sxs-lookup"><span data-stu-id="dae0e-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="dae0e-184">При отправке сервером значения `Accept-Encoding` для клиентов и прокси `Vary` заголовок указывает клиенту или прокси-серверу, что он должен кэшировать (варьировать) ответы в зависимости от значения `Accept-Encoding` заголовка запроса.</span><span class="sxs-lookup"><span data-stu-id="dae0e-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="dae0e-185">Результат возврата содержимого с `Vary: Accept-Encoding` заголовком заключается в том, что как сжатые, так и несжатые ответы кэшируются отдельно.</span><span class="sxs-lookup"><span data-stu-id="dae0e-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="dae0e-186">Изучите функции по промежуточного слоя сжатия ответов с [примером приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="dae0e-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="dae0e-187">В примере показано следующее:</span><span class="sxs-lookup"><span data-stu-id="dae0e-187">The sample illustrates:</span></span>

* <span data-ttu-id="dae0e-188">Сжатие ответов приложений с помощью gzip и пользовательских поставщиков сжатия.</span><span class="sxs-lookup"><span data-stu-id="dae0e-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="dae0e-189">Добавление типа MIME в список по умолчанию типов MIME для сжатия.</span><span class="sxs-lookup"><span data-stu-id="dae0e-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="dae0e-190">Пакет</span><span class="sxs-lookup"><span data-stu-id="dae0e-190">Package</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="dae0e-191">По промежуточного слоя сжатия ответа предоставляется пакетом [Microsoft. AspNetCore. респонсекомпрессион](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) , который неявно включается в ASP.NET Core приложения.</span><span class="sxs-lookup"><span data-stu-id="dae0e-191">Response Compression Middleware is provided by the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package, which is implicitly included in ASP.NET Core apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="dae0e-192">Чтобы включить по промежуточного слоя в проект, добавьте ссылку на [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app), которая включает пакет [Microsoft. AspNetCore. респонсекомпрессион](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) .</span><span class="sxs-lookup"><span data-stu-id="dae0e-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="dae0e-193">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="dae0e-193">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="dae0e-194">В следующем коде показано, как включить по промежуточного слоя сжатия ответа для типов MIME по умолчанию и поставщиков сжатия ([Brotli](#brotli-compression-provider) и [gzip](#gzip-compression-provider)):</span><span class="sxs-lookup"><span data-stu-id="dae0e-194">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="dae0e-195">В следующем коде показано, как включить по промежуточного слоя сжатия ответов для типов MIME по умолчанию и [поставщика сжатия Gzip](#gzip-compression-provider):</span><span class="sxs-lookup"><span data-stu-id="dae0e-195">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

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

<span data-ttu-id="dae0e-196">Примечания.</span><span class="sxs-lookup"><span data-stu-id="dae0e-196">Notes:</span></span>

* <span data-ttu-id="dae0e-197">`app.UseResponseCompression`должен вызываться до `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="dae0e-197">`app.UseResponseCompression` must be called before `app.UseMvc`.</span></span>
* <span data-ttu-id="dae0e-198">Используйте такое средство, как [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)или [POST](https://www.getpostman.com/) , чтобы задать `Accept-Encoding` заголовок запроса и изучить заголовки, размер и текст ответа.</span><span class="sxs-lookup"><span data-stu-id="dae0e-198">Use a tool such as [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="dae0e-199">Отправьте запрос в пример приложения без `Accept-Encoding` заголовка и обратите внимание, что ответ не сжат.</span><span class="sxs-lookup"><span data-stu-id="dae0e-199">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="dae0e-200">Заголовки `Content-Encoding` и`Vary` отсутствуют в ответе.</span><span class="sxs-lookup"><span data-stu-id="dae0e-200">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Окно Fiddler, показывающее результат запроса без заголовка Accept-Encoding.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="dae0e-203">Отправьте запрос в пример приложения с `Accept-Encoding: br` заголовком (Brotli Compression) и обратите внимание на то, что ответ сжат.</span><span class="sxs-lookup"><span data-stu-id="dae0e-203">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="dae0e-204">В ответе имеются заголовки `Vary` и.`Content-Encoding`</span><span class="sxs-lookup"><span data-stu-id="dae0e-204">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Окно Fiddler, показывающее результат запроса с заголовком Accept-Encoding и значением br.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="dae0e-208">Отправьте запрос в пример приложения с `Accept-Encoding: gzip` заголовком и убедитесь, что ответ сжат.</span><span class="sxs-lookup"><span data-stu-id="dae0e-208">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="dae0e-209">В ответе имеются заголовки `Vary` и.`Content-Encoding`</span><span class="sxs-lookup"><span data-stu-id="dae0e-209">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Окно Fiddler, показывающее результат запроса с заголовком Accept-Encoding и значением gzip.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a><span data-ttu-id="dae0e-213">Поставщики</span><span class="sxs-lookup"><span data-stu-id="dae0e-213">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="dae0e-214">Поставщик сжатия Brotli</span><span class="sxs-lookup"><span data-stu-id="dae0e-214">Brotli Compression Provider</span></span>

<span data-ttu-id="dae0e-215">Используйте для сжатия ответов с форматом [сжатых данных Brotli.](https://tools.ietf.org/html/rfc7932) <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider></span><span class="sxs-lookup"><span data-stu-id="dae0e-215">Use the <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="dae0e-216">Если поставщики сжатия явно не добавляются в <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="dae0e-216">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="dae0e-217">Поставщик сжатия Brotli по умолчанию добавляется в массив поставщиков сжатия вместе с [поставщиком сжатия Gzip](#gzip-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="dae0e-217">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="dae0e-218">Сжатие по умолчанию используется для сжатия Brotli, если клиент поддерживает формат сжатых данных Brotli.</span><span class="sxs-lookup"><span data-stu-id="dae0e-218">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="dae0e-219">Если Brotli не поддерживается клиентом, то сжатие по умолчанию используется gzip, если клиент поддерживает сжатие Gzip.</span><span class="sxs-lookup"><span data-stu-id="dae0e-219">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="dae0e-220">Поставщик сжатия Бротоли должен быть добавлен при явном добавлении любых поставщиков сжатия:</span><span class="sxs-lookup"><span data-stu-id="dae0e-220">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="dae0e-221">Задайте уровень сжатия с помощью <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="dae0e-221">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span></span> <span data-ttu-id="dae0e-222">Поставщик сжатия Brotli по умолчанию имеет самый быстрый уровень сжатия ([CompressionLevel. самый быстрый](xref:System.IO.Compression.CompressionLevel)), что может привести к неэффективному сжатию.</span><span class="sxs-lookup"><span data-stu-id="dae0e-222">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="dae0e-223">Если требуется наиболее эффективное сжатие, настройте по промежуточного слоя для оптимального сжатия.</span><span class="sxs-lookup"><span data-stu-id="dae0e-223">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="dae0e-224">Уровень сжатия</span><span class="sxs-lookup"><span data-stu-id="dae0e-224">Compression Level</span></span> | <span data-ttu-id="dae0e-225">Описание</span><span class="sxs-lookup"><span data-stu-id="dae0e-225">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="dae0e-226">CompressionLevel. самый быстрый</span><span class="sxs-lookup"><span data-stu-id="dae0e-226">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="dae0e-227">Сжатие должно завершаться как можно быстрее, даже если полученные выходные данные не будут оптимально сжаты.</span><span class="sxs-lookup"><span data-stu-id="dae0e-227">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="dae0e-228">CompressionLevel. уплотнение</span><span class="sxs-lookup"><span data-stu-id="dae0e-228">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="dae0e-229">Сжатие выполнять не нужно.</span><span class="sxs-lookup"><span data-stu-id="dae0e-229">No compression should be performed.</span></span> |
| [<span data-ttu-id="dae0e-230">CompressionLevel. оптимально</span><span class="sxs-lookup"><span data-stu-id="dae0e-230">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="dae0e-231">Ответы должны быть оптимально сжаты, даже если сжатие занимает больше времени.</span><span class="sxs-lookup"><span data-stu-id="dae0e-231">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="gzip-compression-provider"></a><span data-ttu-id="dae0e-232">Поставщик сжатия GZIP</span><span class="sxs-lookup"><span data-stu-id="dae0e-232">Gzip Compression Provider</span></span>

<span data-ttu-id="dae0e-233">Используйте для сжатия ответов с форматом [файла gzip.](https://tools.ietf.org/html/rfc1952) <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider></span><span class="sxs-lookup"><span data-stu-id="dae0e-233">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="dae0e-234">Если поставщики сжатия явно не добавляются в <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="dae0e-234">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="dae0e-235">Поставщик сжатия Gzip добавляется по умолчанию в массив поставщиков сжатия вместе с [поставщиком сжатия Brotli](#brotli-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="dae0e-235">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="dae0e-236">Сжатие по умолчанию используется для сжатия Brotli, если клиент поддерживает формат сжатых данных Brotli.</span><span class="sxs-lookup"><span data-stu-id="dae0e-236">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="dae0e-237">Если Brotli не поддерживается клиентом, то сжатие по умолчанию используется gzip, если клиент поддерживает сжатие Gzip.</span><span class="sxs-lookup"><span data-stu-id="dae0e-237">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="dae0e-238">Поставщик сжатия Gzip добавляется по умолчанию в массив поставщиков сжатия.</span><span class="sxs-lookup"><span data-stu-id="dae0e-238">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="dae0e-239">Сжатие по умолчанию используется gzip, если клиент поддерживает сжатие Gzip.</span><span class="sxs-lookup"><span data-stu-id="dae0e-239">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="dae0e-240">При явном добавлении поставщиков сжатия необходимо добавить поставщик сжатия gzip:</span><span class="sxs-lookup"><span data-stu-id="dae0e-240">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

<span data-ttu-id="dae0e-241">Задайте уровень сжатия с помощью <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="dae0e-241">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="dae0e-242">Поставщик сжатия Gzip по умолчанию имеет самый быстрый уровень сжатия ([CompressionLevel. самый быстрый](xref:System.IO.Compression.CompressionLevel)), что может не привести к максимально эффективному сжатию.</span><span class="sxs-lookup"><span data-stu-id="dae0e-242">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="dae0e-243">Если требуется наиболее эффективное сжатие, настройте по промежуточного слоя для оптимального сжатия.</span><span class="sxs-lookup"><span data-stu-id="dae0e-243">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="dae0e-244">Уровень сжатия</span><span class="sxs-lookup"><span data-stu-id="dae0e-244">Compression Level</span></span> | <span data-ttu-id="dae0e-245">Описание</span><span class="sxs-lookup"><span data-stu-id="dae0e-245">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="dae0e-246">CompressionLevel. самый быстрый</span><span class="sxs-lookup"><span data-stu-id="dae0e-246">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="dae0e-247">Сжатие должно завершаться как можно быстрее, даже если полученные выходные данные не будут оптимально сжаты.</span><span class="sxs-lookup"><span data-stu-id="dae0e-247">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="dae0e-248">CompressionLevel. уплотнение</span><span class="sxs-lookup"><span data-stu-id="dae0e-248">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="dae0e-249">Сжатие выполнять не нужно.</span><span class="sxs-lookup"><span data-stu-id="dae0e-249">No compression should be performed.</span></span> |
| [<span data-ttu-id="dae0e-250">CompressionLevel. оптимально</span><span class="sxs-lookup"><span data-stu-id="dae0e-250">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="dae0e-251">Ответы должны быть оптимально сжаты, даже если сжатие занимает больше времени.</span><span class="sxs-lookup"><span data-stu-id="dae0e-251">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="custom-providers"></a><span data-ttu-id="dae0e-252">Настраиваемые поставщики</span><span class="sxs-lookup"><span data-stu-id="dae0e-252">Custom providers</span></span>

<span data-ttu-id="dae0e-253">Создание пользовательских реализаций сжатия с <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>помощью.</span><span class="sxs-lookup"><span data-stu-id="dae0e-253">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="dae0e-254">Представляет <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> кодировку содержимого, которую создает `ICompressionProvider` этот объект.</span><span class="sxs-lookup"><span data-stu-id="dae0e-254">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="dae0e-255">По промежуточного слоя использует эти сведения для выбора поставщика на основе списка, указанного в `Accept-Encoding` заголовке запроса.</span><span class="sxs-lookup"><span data-stu-id="dae0e-255">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="dae0e-256">С помощью примера приложения клиент отправляет запрос с `Accept-Encoding: mycustomcompression` заголовком.</span><span class="sxs-lookup"><span data-stu-id="dae0e-256">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="dae0e-257">По промежуточного слоя использует реализацию пользовательского сжатия и возвращает ответ с `Content-Encoding: mycustomcompression` заголовком.</span><span class="sxs-lookup"><span data-stu-id="dae0e-257">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="dae0e-258">Клиент должен иметь возможность распаковать пользовательскую кодировку, чтобы обеспечить работу пользовательской реализации сжатия.</span><span class="sxs-lookup"><span data-stu-id="dae0e-258">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/3.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="dae0e-259">Отправьте запрос в пример приложения с `Accept-Encoding: mycustomcompression` заголовком и просмотрите заголовки ответа.</span><span class="sxs-lookup"><span data-stu-id="dae0e-259">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="dae0e-260">В ответе имеются заголовки `Content-Encoding` и.`Vary`</span><span class="sxs-lookup"><span data-stu-id="dae0e-260">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="dae0e-261">Текст ответа (не показан) не сжимается образцом.</span><span class="sxs-lookup"><span data-stu-id="dae0e-261">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="dae0e-262">В `CustomCompressionProvider` классе примера нет реализации сжатия.</span><span class="sxs-lookup"><span data-stu-id="dae0e-262">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="dae0e-263">Однако в примере показано, где следует реализовать такой алгоритм сжатия.</span><span class="sxs-lookup"><span data-stu-id="dae0e-263">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Окно Fiddler, показывающее результат запроса с заголовком Accept-Encoding и значением микустомкомпрессион.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="dae0e-266">типы MIME</span><span class="sxs-lookup"><span data-stu-id="dae0e-266">MIME types</span></span>

<span data-ttu-id="dae0e-267">По промежуточного слоя указывает набор типов MIME по умолчанию для сжатия:</span><span class="sxs-lookup"><span data-stu-id="dae0e-267">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="dae0e-268">Замените или добавьте типы MIME с помощью параметров по промежуточного слоя сжатия ответа.</span><span class="sxs-lookup"><span data-stu-id="dae0e-268">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="dae0e-269">Обратите внимание, что типы MIME с `text/*` подстановочными знаками, такие как, не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="dae0e-269">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="dae0e-270">Пример приложения добавляет тип MIME для `image/svg+xml` и сжимает и обслуживает изображение ASP.NET Core баннера (*Banner. SVG*).</span><span class="sxs-lookup"><span data-stu-id="dae0e-270">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="dae0e-271">Сжатие по защищенному протоколу</span><span class="sxs-lookup"><span data-stu-id="dae0e-271">Compression with secure protocol</span></span>

<span data-ttu-id="dae0e-272">Сжатые ответы по защищенным подключениям можно контролировать `EnableForHttps` с помощью параметра, который по умолчанию отключен.</span><span class="sxs-lookup"><span data-stu-id="dae0e-272">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="dae0e-273">Использование сжатия с динамически создаваемыми страницами может привести к проблемам безопасности, таким как [преступления](https://wikipedia.org/wiki/CRIME_(security_exploit)) и [нарушение](https://wikipedia.org/wiki/BREACH_(security_exploit)) атак.</span><span class="sxs-lookup"><span data-stu-id="dae0e-273">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="dae0e-274">Добавление заголовка Vary</span><span class="sxs-lookup"><span data-stu-id="dae0e-274">Adding the Vary header</span></span>

<span data-ttu-id="dae0e-275">При сжатии ответов на основе `Accept-Encoding` заголовка существует потенциально несколько сжатых версий ответа и несжатая версия.</span><span class="sxs-lookup"><span data-stu-id="dae0e-275">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="dae0e-276">Чтобы настроить кэш клиента и прокси-сервера на наличие нескольких версий и их хранения, `Vary` заголовок добавляется `Accept-Encoding` со значением.</span><span class="sxs-lookup"><span data-stu-id="dae0e-276">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="dae0e-277">В ASP.NET Core 2,0 или более поздней версии по промежуточного слоя добавляет `Vary` заголовок автоматически при сжатии ответа.</span><span class="sxs-lookup"><span data-stu-id="dae0e-277">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="dae0e-278">Проблемы по промежуточного слоя, когда nginx обратный прокси-сервер</span><span class="sxs-lookup"><span data-stu-id="dae0e-278">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="dae0e-279">При выполнении запроса через прокси-сервер nginx `Accept-Encoding` заголовок удаляется.</span><span class="sxs-lookup"><span data-stu-id="dae0e-279">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="dae0e-280">`Accept-Encoding` Удаление заголовка предотвращает сжатие ответа по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="dae0e-280">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="dae0e-281">Дополнительные сведения см. в статье об [использовании Сжатие и](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)распаковка.</span><span class="sxs-lookup"><span data-stu-id="dae0e-281">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="dae0e-282">Эта проблема проявляется на [рисунке сквозного сжатия для nginx (ASPNET/ \#басикмиддлеваре 123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="dae0e-282">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="dae0e-283">Работа с динамическим сжатием IIS</span><span class="sxs-lookup"><span data-stu-id="dae0e-283">Working with IIS dynamic compression</span></span>

<span data-ttu-id="dae0e-284">При наличии активного модуля динамического сжатия IIS, настроенного на уровне сервера, который вы хотите отключить для приложения, отключите модуль, дополнив добавление к файлу *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="dae0e-284">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="dae0e-285">Дополнительные сведения см. в разделе [Отключение модулей IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="dae0e-285">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="dae0e-286">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="dae0e-286">Troubleshooting</span></span>

<span data-ttu-id="dae0e-287">Используйте такие средства, как [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)или [POST](https://www.getpostman.com/), которые `Accept-Encoding` позволяют задать заголовок запроса и изучить заголовки, размер и текст ответа.</span><span class="sxs-lookup"><span data-stu-id="dae0e-287">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="dae0e-288">По умолчанию по промежуточного слоя для сжатия ответов сжимает ответы, соответствующие следующим условиям.</span><span class="sxs-lookup"><span data-stu-id="dae0e-288">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="dae0e-289">Заголовок представлен со `br`значением `gzip` ,`*`, или настраиваемой кодировкой, соответствующей настраиваемому поставщику сжатия, который вы установили. `Accept-Encoding`</span><span class="sxs-lookup"><span data-stu-id="dae0e-289">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="dae0e-290">Значение не должно быть `identity` или иметь значение свойства (квалуе,), `q`равное 0 (нулю).</span><span class="sxs-lookup"><span data-stu-id="dae0e-290">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="dae0e-291">Тип MIME (`Content-Type`) должен быть установлен и должен соответствовать типу MIME, настроенному <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>в.</span><span class="sxs-lookup"><span data-stu-id="dae0e-291">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="dae0e-292">Запрос не должен включать `Content-Range` заголовок.</span><span class="sxs-lookup"><span data-stu-id="dae0e-292">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="dae0e-293">В запросе должен использоваться небезопасный протокол (http), если в параметрах по промежуточного слоя сжатия отклика не настроен защищенный протокол (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="dae0e-293">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="dae0e-294">*Обратите внимание на опасность, [описанную выше](#compression-with-secure-protocol) , при включении безопасного сжатия содержимого.*</span><span class="sxs-lookup"><span data-stu-id="dae0e-294">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="dae0e-295">Заголовок представлен со `gzip`значением, `*`или пользовательской кодировкой, соответствующей настраиваемому поставщику сжатия, который вы установили. `Accept-Encoding`</span><span class="sxs-lookup"><span data-stu-id="dae0e-295">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="dae0e-296">Значение не должно быть `identity` или иметь значение свойства (квалуе,), `q`равное 0 (нулю).</span><span class="sxs-lookup"><span data-stu-id="dae0e-296">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="dae0e-297">Тип MIME (`Content-Type`) должен быть установлен и должен соответствовать типу MIME, настроенному <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>в.</span><span class="sxs-lookup"><span data-stu-id="dae0e-297">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="dae0e-298">Запрос не должен включать `Content-Range` заголовок.</span><span class="sxs-lookup"><span data-stu-id="dae0e-298">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="dae0e-299">В запросе должен использоваться небезопасный протокол (http), если в параметрах по промежуточного слоя сжатия отклика не настроен защищенный протокол (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="dae0e-299">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="dae0e-300">*Обратите внимание на опасность, [описанную выше](#compression-with-secure-protocol) , при включении безопасного сжатия содержимого.*</span><span class="sxs-lookup"><span data-stu-id="dae0e-300">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="dae0e-301">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="dae0e-301">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="dae0e-302">Сеть для разработчиков Mozilla: Accept-Encoding</span><span class="sxs-lookup"><span data-stu-id="dae0e-302">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="dae0e-303">RFC 7231 раздел 3.1.2.1: Кодирование содержимого</span><span class="sxs-lookup"><span data-stu-id="dae0e-303">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="dae0e-304">RFC 7230 раздел 4.2.3: Кодирование gzip</span><span class="sxs-lookup"><span data-stu-id="dae0e-304">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="dae0e-305">Спецификация формата файла GZIP, версия 4,3</span><span class="sxs-lookup"><span data-stu-id="dae0e-305">GZIP file format specification version 4.3</span></span>](https://www.ietf.org/rfc/rfc1952.txt)
