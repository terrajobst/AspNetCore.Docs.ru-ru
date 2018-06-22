---
title: Ответ сжатия по промежуточного слоя ASP.NET Core
author: guardrex
description: Дополнительные сведения о сжатия ответов и использовании ответа сжатия по промежуточного слоя в приложениях ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/20/2017
uid: performance/response-compression
ms.openlocfilehash: 585a08d4a6c6e03785e87578d10f6050be8bb73c
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278223"
---
# <a name="response-compression-middleware-for-aspnet-core"></a><span data-ttu-id="5edc9-103">Ответ сжатия по промежуточного слоя ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5edc9-103">Response Compression Middleware for ASP.NET Core</span></span>

<span data-ttu-id="5edc9-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="5edc9-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5edc9-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5edc9-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="5edc9-106">Пропускная способность сети является ограниченным ресурсом.</span><span class="sxs-lookup"><span data-stu-id="5edc9-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="5edc9-107">Обычно уменьшить размер ответа часто значительно увеличивает скорость реагирования приложения.</span><span class="sxs-lookup"><span data-stu-id="5edc9-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="5edc9-108">Для сжатия ответов приложения является одним из способов уменьшить размер полезных данных.</span><span class="sxs-lookup"><span data-stu-id="5edc9-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="5edc9-109">Когда следует использовать ответ сжатия по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="5edc9-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="5edc9-110">Используйте технологии сжатия ответ на основе сервера в IIS, Apache или Nginx.</span><span class="sxs-lookup"><span data-stu-id="5edc9-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="5edc9-111">Производительность по промежуточного слоя, скорее всего, не будет соответствовать модулей сервера.</span><span class="sxs-lookup"><span data-stu-id="5edc9-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="5edc9-112">[HTTP.sys сервера](xref:fundamentals/servers/httpsys) и [Kestrel](xref:fundamentals/servers/kestrel) в настоящее время не предлагают поддержка встроенных сжатия.</span><span class="sxs-lookup"><span data-stu-id="5edc9-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) and [Kestrel](xref:fundamentals/servers/kestrel) don't currently offer built-in compression support.</span></span>

<span data-ttu-id="5edc9-113">Используйте ответа сжатия по промежуточного слоя, когда вы:</span><span class="sxs-lookup"><span data-stu-id="5edc9-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="5edc9-114">Не удается использовать следующие технологии сжатия на основе сервера:</span><span class="sxs-lookup"><span data-stu-id="5edc9-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="5edc9-115">Модуль сжатия динамического IIS</span><span class="sxs-lookup"><span data-stu-id="5edc9-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="5edc9-116">Модуль mod_deflate Apache</span><span class="sxs-lookup"><span data-stu-id="5edc9-116">Apache mod_deflate module</span></span>](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="5edc9-117">Nginx сжатия и распаковки</span><span class="sxs-lookup"><span data-stu-id="5edc9-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="5edc9-118">Размещение непосредственно на:</span><span class="sxs-lookup"><span data-stu-id="5edc9-118">Hosting directly on:</span></span>
  * <span data-ttu-id="5edc9-119">[HTTP.sys сервера](xref:fundamentals/servers/httpsys) (ранее называвшиеся [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="5edc9-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>
  * [<span data-ttu-id="5edc9-120">Kestrel</span><span class="sxs-lookup"><span data-stu-id="5edc9-120">Kestrel</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="5edc9-121">Сжатие ответа</span><span class="sxs-lookup"><span data-stu-id="5edc9-121">Response compression</span></span>

<span data-ttu-id="5edc9-122">Как правило любой ответ, изначально не сжаты могут выиграть от сжатия ответов.</span><span class="sxs-lookup"><span data-stu-id="5edc9-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="5edc9-123">Ответы, изначально не сжаты обычно включают: CSS, JavaScript, HTML, XML и JSON.</span><span class="sxs-lookup"><span data-stu-id="5edc9-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="5edc9-124">Не следует сжимать изначально сжатых ресурсов, таких как файлы PNG.</span><span class="sxs-lookup"><span data-stu-id="5edc9-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="5edc9-125">При попытке выполнить дальнейшее сжатие изначально сжатого ответа любой небольшой дополнительное сокращение времени, размер и передачи скорее всего будет злоумышленниками время, затраченное на обработку сжатие.</span><span class="sxs-lookup"><span data-stu-id="5edc9-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="5edc9-126">Не сжимать файлы размером меньше примерно 150-1000 байт (в зависимости от его содержимого и повысить эффективность сжатия).</span><span class="sxs-lookup"><span data-stu-id="5edc9-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="5edc9-127">Затраты на сжатие небольших файлов может привести к сжатый файл, размер которых превышает несжатого файла.</span><span class="sxs-lookup"><span data-stu-id="5edc9-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="5edc9-128">Когда клиент может обрабатывать сжатое содержимое, клиент должен уведомить сервер его возможности, отправляя `Accept-Encoding` заголовок с запросом.</span><span class="sxs-lookup"><span data-stu-id="5edc9-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="5edc9-129">Когда сервер отправляет сжатое содержимое, он должен содержать сведения в `Content-Encoding` заголовок кодируется как сжатого ответа.</span><span class="sxs-lookup"><span data-stu-id="5edc9-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="5edc9-130">В следующей таблице показаны содержимого обозначения кодировки, поддерживаемые по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="5edc9-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

| <span data-ttu-id="5edc9-131">`Accept-Encoding` значения заголовка</span><span class="sxs-lookup"><span data-stu-id="5edc9-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="5edc9-132">Поддерживается по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="5edc9-132">Middleware Supported</span></span> | <span data-ttu-id="5edc9-133">Описание:</span><span class="sxs-lookup"><span data-stu-id="5edc9-133">Description</span></span>                                                 |
| ------------------------------- | :------------------: | ----------------------------------------------------------- |
| `br`                            | <span data-ttu-id="5edc9-134">Нет</span><span class="sxs-lookup"><span data-stu-id="5edc9-134">No</span></span>                   | <span data-ttu-id="5edc9-135">Формат Brotli сжатых данных</span><span class="sxs-lookup"><span data-stu-id="5edc9-135">Brotli Compressed Data Format</span></span>                               |
| `compress`                      | <span data-ttu-id="5edc9-136">Нет</span><span class="sxs-lookup"><span data-stu-id="5edc9-136">No</span></span>                   | <span data-ttu-id="5edc9-137">Формат данных «сжатие» UNIX</span><span class="sxs-lookup"><span data-stu-id="5edc9-137">UNIX "compress" data format</span></span>                                 |
| `deflate`                       | <span data-ttu-id="5edc9-138">Нет</span><span class="sxs-lookup"><span data-stu-id="5edc9-138">No</span></span>                   | <span data-ttu-id="5edc9-139">сжатые данные в формат данных «zlib» «deflate»</span><span class="sxs-lookup"><span data-stu-id="5edc9-139">"deflate" compressed data inside the "zlib" data format</span></span>     |
| `exi`                           | <span data-ttu-id="5edc9-140">Нет</span><span class="sxs-lookup"><span data-stu-id="5edc9-140">No</span></span>                   | <span data-ttu-id="5edc9-141">W3C XML эффективного обмена</span><span class="sxs-lookup"><span data-stu-id="5edc9-141">W3C Efficient XML Interchange</span></span>                               |
| `gzip`                          | <span data-ttu-id="5edc9-142">Да (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="5edc9-142">Yes (default)</span></span>        | <span data-ttu-id="5edc9-143">формат файла gzip</span><span class="sxs-lookup"><span data-stu-id="5edc9-143">gzip file format</span></span>                                            |
| `identity`                      | <span data-ttu-id="5edc9-144">Да</span><span class="sxs-lookup"><span data-stu-id="5edc9-144">Yes</span></span>                  | <span data-ttu-id="5edc9-145">Идентификатор «Без кодировки»: ответ не должны быть закодированы.</span><span class="sxs-lookup"><span data-stu-id="5edc9-145">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="5edc9-146">Нет</span><span class="sxs-lookup"><span data-stu-id="5edc9-146">No</span></span>                   | <span data-ttu-id="5edc9-147">Формат сети передачи архивов Java</span><span class="sxs-lookup"><span data-stu-id="5edc9-147">Network Transfer Format for Java Archives</span></span>                   |
| `*`                             | <span data-ttu-id="5edc9-148">Да</span><span class="sxs-lookup"><span data-stu-id="5edc9-148">Yes</span></span>                  | <span data-ttu-id="5edc9-149">Все доступное содержимое, кодировка не явно запрошенный</span><span class="sxs-lookup"><span data-stu-id="5edc9-149">Any available content encoding not explicitly requested</span></span>     |

<span data-ttu-id="5edc9-150">Дополнительные сведения см. в разделе [IANA официальный кодирования список содержимое](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="5edc9-150">For more information, see the [IANA Official Content Coding List](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="5edc9-151">По промежуточного слоя можно добавить дополнительное сжатие поставщиков для пользовательских `Accept-Encoding` значения заголовка.</span><span class="sxs-lookup"><span data-stu-id="5edc9-151">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="5edc9-152">Дополнительные сведения см. в разделе [настраиваемые поставщики](#custom-providers) ниже.</span><span class="sxs-lookup"><span data-stu-id="5edc9-152">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="5edc9-153">По промежуточного слоя способен отклик на значение качества (qvalue, `q`) взвешивание при отправке клиентом приоритет схемы сжатия.</span><span class="sxs-lookup"><span data-stu-id="5edc9-153">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="5edc9-154">Дополнительные сведения см. в разделе [RFC 7231: приемлемой кодировкой](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="5edc9-154">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="5edc9-155">Алгоритмы сжатия подвержены компромисса между скоростью сжатие и эффективность сжатия.</span><span class="sxs-lookup"><span data-stu-id="5edc9-155">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="5edc9-156">*Эффективность* в этом контексте ссылается размер выходных данных после сжатия.</span><span class="sxs-lookup"><span data-stu-id="5edc9-156">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="5edc9-157">Минимальный размер достигается за счет наиболее *оптимальной* сжатия.</span><span class="sxs-lookup"><span data-stu-id="5edc9-157">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="5edc9-158">Заголовки, участвующих в запросе, отправки, кэширование и получения содержимого сжатого описаны в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="5edc9-158">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="5edc9-159">Header</span><span class="sxs-lookup"><span data-stu-id="5edc9-159">Header</span></span>             | <span data-ttu-id="5edc9-160">Роль</span><span class="sxs-lookup"><span data-stu-id="5edc9-160">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="5edc9-161">Отправленные клиентом на сервер для указания содержимого, кодировки схем, допустимых для клиента.</span><span class="sxs-lookup"><span data-stu-id="5edc9-161">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="5edc9-162">Отправляются с сервера на клиент для указания кодировки содержимого в полезных данных.</span><span class="sxs-lookup"><span data-stu-id="5edc9-162">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="5edc9-163">Когда происходит сжатие, `Content-Length` заголовок удаляется с момента изменения содержимого текста при сжатии ответа.</span><span class="sxs-lookup"><span data-stu-id="5edc9-163">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="5edc9-164">Когда происходит сжатие, `Content-MD5` заголовок удаляется, поскольку содержимое текста был изменен и хэш-код больше не действительна.</span><span class="sxs-lookup"><span data-stu-id="5edc9-164">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="5edc9-165">Указывает тип MIME содержимого.</span><span class="sxs-lookup"><span data-stu-id="5edc9-165">Specifies the MIME type of the content.</span></span> <span data-ttu-id="5edc9-166">Каждый ответ должен указать его `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="5edc9-166">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="5edc9-167">По промежуточного слоя проверяет это значение, чтобы определить, должны ли сжиматься ответа.</span><span class="sxs-lookup"><span data-stu-id="5edc9-167">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="5edc9-168">По промежуточного слоя задает набор [по умолчанию типы MIME](#mime-types) , что позволяет кодировать, но можно заменить или добавить типы MIME.</span><span class="sxs-lookup"><span data-stu-id="5edc9-168">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="5edc9-169">При отправке сервером со значением `Accept-Encoding` для клиентов и прокси-серверы, `Vary` заголовок указывает клиенту или прокси-сервера, кэшируется (меняться) ответы на основе значения `Accept-Encoding` заголовок запроса.</span><span class="sxs-lookup"><span data-stu-id="5edc9-169">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="5edc9-170">Возврат содержимого с результатом `Vary: Accept-Encoding` заголовок — что оба сжатые и несжатые ответы, кэшируются отдельно.</span><span class="sxs-lookup"><span data-stu-id="5edc9-170">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="5edc9-171">Можно ознакомиться с функциями по промежуточного слоя сжатия ответа с [пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="5edc9-171">You can explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="5edc9-172">В образце кода:</span><span class="sxs-lookup"><span data-stu-id="5edc9-172">The sample illustrates:</span></span>

* <span data-ttu-id="5edc9-173">Сжатие ответов приложения с помощью gzip и сжатие пользовательских поставщиков.</span><span class="sxs-lookup"><span data-stu-id="5edc9-173">The compression of app responses using gzip and custom compression providers.</span></span>
* <span data-ttu-id="5edc9-174">Как добавить тип MIME по умолчанию в список типов MIME для сжатия.</span><span class="sxs-lookup"><span data-stu-id="5edc9-174">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="5edc9-175">Пакет</span><span class="sxs-lookup"><span data-stu-id="5edc9-175">Package</span></span>

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="5edc9-176">Чтобы включить по промежуточного слоя в проекте, добавьте ссылку на [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) пакета.</span><span class="sxs-lookup"><span data-stu-id="5edc9-176">To include the middleware in your project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span> <span data-ttu-id="5edc9-177">Эта функция доступна для приложений, предназначенных для ASP.NET Core 1.1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="5edc9-177">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="5edc9-178">Чтобы включить по промежуточного слоя в проекте, добавьте ссылку на [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) пакета или использовать [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="5edc9-178">To include the middleware in your project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package or use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="5edc9-179">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="5edc9-179">Configuration</span></span>

<span data-ttu-id="5edc9-180">Ниже показано включение ответа сжатия по промежуточного слоя со сжатием gzip по умолчанию и для типов MIME по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5edc9-180">The following code shows how to enable the Response Compression Middleware with the default gzip compression and for default MIME types.</span></span>

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
> <span data-ttu-id="5edc9-181">Использование таких средств, как [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), или [почтальон](https://www.getpostman.com/) для задания `Accept-Encoding` заголовок запроса и изучения заголовки ответа, размер и текст.</span><span class="sxs-lookup"><span data-stu-id="5edc9-181">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="5edc9-182">Запрос отправляется в пример приложения без `Accept-Encoding` заголовок и обратите внимание, что ответ без сжатия.</span><span class="sxs-lookup"><span data-stu-id="5edc9-182">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="5edc9-183">`Content-Encoding` И `Vary` заголовки не присутствуют в ответе.</span><span class="sxs-lookup"><span data-stu-id="5edc9-183">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Fiddler окно, отображающее результат запроса без заголовка Accept-Encoding.](response-compression/_static/request-uncompressed.png)

<span data-ttu-id="5edc9-186">Отправить запрос на пример приложения с `Accept-Encoding: gzip` заголовок и обратите внимание, что ответ сжимается.</span><span class="sxs-lookup"><span data-stu-id="5edc9-186">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="5edc9-187">`Content-Encoding` И `Vary` заголовки присутствуют в ответе.</span><span class="sxs-lookup"><span data-stu-id="5edc9-187">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Fiddler окно, отображающее результат запроса с заголовком Accept-Encoding и значение gzip.](response-compression/_static/request-compressed.png)

## <a name="providers"></a><span data-ttu-id="5edc9-191">Поставщики</span><span class="sxs-lookup"><span data-stu-id="5edc9-191">Providers</span></span>

### <a name="gzipcompressionprovider"></a><span data-ttu-id="5edc9-192">GzipCompressionProvider</span><span class="sxs-lookup"><span data-stu-id="5edc9-192">GzipCompressionProvider</span></span>

<span data-ttu-id="5edc9-193">Используйте [GzipCompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovider) сжатия ответов с помощью gzip.</span><span class="sxs-lookup"><span data-stu-id="5edc9-193">Use the [GzipCompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovider) to compress responses with gzip.</span></span> <span data-ttu-id="5edc9-194">Это поставщик сжатия по умолчанию, если не указан ни один.</span><span class="sxs-lookup"><span data-stu-id="5edc9-194">This is the default compression provider if none are specified.</span></span> <span data-ttu-id="5edc9-195">Сжатие можно установить уровень с [GzipCompressionProviderOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovideroptions).</span><span class="sxs-lookup"><span data-stu-id="5edc9-195">You can set the compression level with the [GzipCompressionProviderOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovideroptions).</span></span>

<span data-ttu-id="5edc9-196">По умолчанию используется поставщик сжатие gzip быстрым уровень сжатия ([CompressionLevel.Fastest](/dotnet/api/system.io.compression.compressionlevel)), который может не обеспечить наиболее эффективное сжатие.</span><span class="sxs-lookup"><span data-stu-id="5edc9-196">The gzip compression provider defaults to the fastest compression level ([CompressionLevel.Fastest](/dotnet/api/system.io.compression.compressionlevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="5edc9-197">При желании наиболее эффективный сжатия можно настроить по промежуточного слоя для оптимального сжатия.</span><span class="sxs-lookup"><span data-stu-id="5edc9-197">If the most efficient compression is desired, you can configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="5edc9-198">Уровень сжатия</span><span class="sxs-lookup"><span data-stu-id="5edc9-198">Compression Level</span></span> | <span data-ttu-id="5edc9-199">Описание:</span><span class="sxs-lookup"><span data-stu-id="5edc9-199">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="5edc9-200">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="5edc9-200">CompressionLevel.Fastest</span></span>](/dotnet/api/system.io.compression.compressionlevel) | <span data-ttu-id="5edc9-201">Сжатие следует выполнить как можно скорее, даже если результат не сжаты оптимально.</span><span class="sxs-lookup"><span data-stu-id="5edc9-201">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="5edc9-202">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="5edc9-202">CompressionLevel.NoCompression</span></span>](/dotnet/api/system.io.compression.compressionlevel) | <span data-ttu-id="5edc9-203">Должно осуществляться без сжатия.</span><span class="sxs-lookup"><span data-stu-id="5edc9-203">No compression should be performed.</span></span> |
| [<span data-ttu-id="5edc9-204">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="5edc9-204">CompressionLevel.Optimal</span></span>](/dotnet/api/system.io.compression.compressionlevel) | <span data-ttu-id="5edc9-205">Ответы должны оптимально сжиматься, даже если сжатие занимает больше времени для завершения.</span><span class="sxs-lookup"><span data-stu-id="5edc9-205">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5edc9-206">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5edc9-206">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5,12-15)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5edc9-207">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5edc9-207">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,12-15)]

---

## <a name="mime-types"></a><span data-ttu-id="5edc9-208">типы MIME</span><span class="sxs-lookup"><span data-stu-id="5edc9-208">MIME types</span></span>

<span data-ttu-id="5edc9-209">По промежуточного слоя указывает набор типов MIME для сжатия по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="5edc9-209">The middleware specifies a default set of MIME types for compression:</span></span>

* `text/plain`
* `text/css`
* `application/javascript`
* `text/html`
* `application/xml`
* `text/xml`
* `application/json`
* `text/json`

<span data-ttu-id="5edc9-210">Можно заменить или добавить типы MIME с параметрами по промежуточного слоя сжатия ответов.</span><span class="sxs-lookup"><span data-stu-id="5edc9-210">You can replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="5edc9-211">Обратите внимание, что подстановочные MIME типы, такие как `text/*` не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="5edc9-211">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="5edc9-212">Пример приложения добавляет тип MIME для `image/svg+xml` и сжимает и служит ASP.NET Core изображение баннера (*banner.svg*).</span><span class="sxs-lookup"><span data-stu-id="5edc9-212">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5edc9-213">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5edc9-213">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7-9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5edc9-214">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5edc9-214">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

---

### <a name="custom-providers"></a><span data-ttu-id="5edc9-215">Настраиваемые поставщики</span><span class="sxs-lookup"><span data-stu-id="5edc9-215">Custom providers</span></span>

<span data-ttu-id="5edc9-216">Можно создать реализаций сжатия с [ICompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider).</span><span class="sxs-lookup"><span data-stu-id="5edc9-216">You can create custom compression implementations with [ICompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider).</span></span> <span data-ttu-id="5edc9-217">[EncodingName](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider.encodingname) представляет содержимое, кодировка этим `ICompressionProvider` выводятся.</span><span class="sxs-lookup"><span data-stu-id="5edc9-217">The [EncodingName](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider.encodingname) represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="5edc9-218">По промежуточного слоя использует эти сведения можно выбрать поставщика, на основе списка, указанный в `Accept-Encoding` заголовок запроса.</span><span class="sxs-lookup"><span data-stu-id="5edc9-218">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="5edc9-219">Используя пример приложения, клиент отправляет запрос с `Accept-Encoding: mycustomcompression` заголовок.</span><span class="sxs-lookup"><span data-stu-id="5edc9-219">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="5edc9-220">По промежуточного слоя использует реализацию пользовательского сжатия и возвращает ответ с `Content-Encoding: mycustomcompression` заголовок.</span><span class="sxs-lookup"><span data-stu-id="5edc9-220">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="5edc9-221">Клиент должен иметь возможность распаковки пользовательских кодировок в порядке для реализации пользовательских сжатия для работы.</span><span class="sxs-lookup"><span data-stu-id="5edc9-221">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5edc9-222">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5edc9-222">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5,12-15)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5edc9-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5edc9-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,12-15)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

---

<span data-ttu-id="5edc9-224">Отправить запрос на пример приложения с `Accept-Encoding: mycustomcompression` заголовок и просмотрите заголовки ответа.</span><span class="sxs-lookup"><span data-stu-id="5edc9-224">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="5edc9-225">`Vary` И `Content-Encoding` заголовки присутствуют в ответе.</span><span class="sxs-lookup"><span data-stu-id="5edc9-225">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="5edc9-226">Текст ответа (не показано) не сжаты в образце.</span><span class="sxs-lookup"><span data-stu-id="5edc9-226">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="5edc9-227">Отсутствует реализация сжатия в `CustomCompressionProvider` класс образца.</span><span class="sxs-lookup"><span data-stu-id="5edc9-227">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="5edc9-228">Тем не менее в образце показано, где следует реализовать алгоритм сжатия.</span><span class="sxs-lookup"><span data-stu-id="5edc9-228">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Fiddler окно, отображающее результат запроса с заголовком Accept-Encoding и значение mycustomcompression.](response-compression/_static/request-custom-compression.png)

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="5edc9-231">Сжатие с безопасным протоколом</span><span class="sxs-lookup"><span data-stu-id="5edc9-231">Compression with secure protocol</span></span>

<span data-ttu-id="5edc9-232">Сжатые ответы по безопасным соединениям можно управлять при помощи `EnableForHttps` параметр, который по умолчанию отключена.</span><span class="sxs-lookup"><span data-stu-id="5edc9-232">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="5edc9-233">С помощью сжатия с динамической страницы может привести к проблемам безопасности таких как [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) и [нарушения](https://wikipedia.org/wiki/BREACH_(security_exploit)) атак.</span><span class="sxs-lookup"><span data-stu-id="5edc9-233">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="5edc9-234">Добавление заголовка Vary</span><span class="sxs-lookup"><span data-stu-id="5edc9-234">Adding the Vary header</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5edc9-235">При сжатии ответы на основе `Accept-Encoding` заголовок, потенциально несколько версий сжатого ответа и несжатую версию.</span><span class="sxs-lookup"><span data-stu-id="5edc9-235">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="5edc9-236">Чтобы настроить кэш клиента и прокси-сервера, существует несколько версий и должны быть сохранены, `Vary` добавить заголовок с `Accept-Encoding` значение.</span><span class="sxs-lookup"><span data-stu-id="5edc9-236">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="5edc9-237">В ASP.NET Core 2.0 или более поздней версии, добавляет по промежуточного слоя `Vary` заголовка автоматически при ответе сжимается.</span><span class="sxs-lookup"><span data-stu-id="5edc9-237">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5edc9-238">При сжатии ответы на основе `Accept-Encoding` заголовок, потенциально несколько версий сжатого ответа и несжатую версию.</span><span class="sxs-lookup"><span data-stu-id="5edc9-238">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="5edc9-239">Чтобы настроить кэш клиента и прокси-сервера, существует несколько версий и должны быть сохранены, `Vary` добавить заголовок с `Accept-Encoding` значение.</span><span class="sxs-lookup"><span data-stu-id="5edc9-239">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="5edc9-240">В ASP.NET Core 1.x, добавление `Vary` заголовок в ответ выполняется вручную:</span><span class="sxs-lookup"><span data-stu-id="5edc9-240">In ASP.NET Core 1.x, adding the `Vary` header to the response is accomplished manually:</span></span>

<span data-ttu-id="5edc9-241">[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="5edc9-241">[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]</span></span>

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="5edc9-242">Проблемы по промежуточного слоя за Nginx обратного прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="5edc9-242">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="5edc9-243">При получении запроса через прокси, Nginx, `Accept-Encoding` заголовок удаляется.</span><span class="sxs-lookup"><span data-stu-id="5edc9-243">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="5edc9-244">Это предотвращает сжатие ответ по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="5edc9-244">This prevents the middleware from compressing the response.</span></span> <span data-ttu-id="5edc9-245">Дополнительные сведения см. в разделе [NGINX: сжатия и распаковки](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="5edc9-245">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="5edc9-246">Эта проблема отслеживается [выяснить сквозной сжатия для Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="5edc9-246">This issue is tracked by [Figure out pass-through compression for Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="5edc9-247">Работа с динамическое сжатие IIS</span><span class="sxs-lookup"><span data-stu-id="5edc9-247">Working with IIS dynamic compression</span></span>

<span data-ttu-id="5edc9-248">При наличии активных динамического сжатия модуль IIS настроен на уровне сервера, который вы хотите отключить для приложения, это можно сделать с дополнением к вашей *web.config* файла.</span><span class="sxs-lookup"><span data-stu-id="5edc9-248">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, you can do so with an addition to your *web.config* file.</span></span> <span data-ttu-id="5edc9-249">Дополнительные сведения см. в разделе [Отключение модулей IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="5edc9-249">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="5edc9-250">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="5edc9-250">Troubleshooting</span></span>

<span data-ttu-id="5edc9-251">Использование таких средств, как [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), или [почтальон](https://www.getpostman.com/), позволяют задавать `Accept-Encoding` заголовок запроса и изучения заголовки ответа, размер и текст.</span><span class="sxs-lookup"><span data-stu-id="5edc9-251">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="5edc9-252">По промежуточного слоя сжатия ответа сжимает ответов, которые удовлетворяют следующим условиям:</span><span class="sxs-lookup"><span data-stu-id="5edc9-252">The Response Compression Middleware compresses responses that meet the following conditions:</span></span>

* <span data-ttu-id="5edc9-253">`Accept-Encoding` Присутствует заголовок со значением `gzip`, `*`, или настраиваемую кодировку, соответствующий поставщик сжатия, который вы определили.</span><span class="sxs-lookup"><span data-stu-id="5edc9-253">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="5edc9-254">Значение не должно быть `identity` или иметь значение качества (qvalue, `q`) значение 0 (ноль).</span><span class="sxs-lookup"><span data-stu-id="5edc9-254">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="5edc9-255">Тип MIME (`Content-Type`) должны быть заданы, должны совпадать с типом MIME, настроенный на [ResponseCompressionOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.responsecompressionoptions).</span><span class="sxs-lookup"><span data-stu-id="5edc9-255">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the [ResponseCompressionOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.responsecompressionoptions).</span></span>
* <span data-ttu-id="5edc9-256">Запрос не должен включать `Content-Range` заголовок.</span><span class="sxs-lookup"><span data-stu-id="5edc9-256">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="5edc9-257">Запрос должен использовать незащищенный протокол (http), если безопасный протокол (https) не настроен в параметрах ответа сжатия по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="5edc9-257">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="5edc9-258">*Обратите внимание, опасности [описанных выше](#compression-with-secure-protocol) при включении сжатия безопасного содержимого.*</span><span class="sxs-lookup"><span data-stu-id="5edc9-258">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5edc9-259">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="5edc9-259">Additional resources</span></span>

* [<span data-ttu-id="5edc9-260">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="5edc9-260">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="5edc9-261">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="5edc9-261">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="5edc9-262">Сеть разработчиков Mozilla: Приемлемой кодировкой</span><span class="sxs-lookup"><span data-stu-id="5edc9-262">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="5edc9-263">Раздела RFC 7231 3.1.2.1: Codings содержимого</span><span class="sxs-lookup"><span data-stu-id="5edc9-263">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="5edc9-264">RFC 7230 разделе 4.2.3: Кодирование Gzip</span><span class="sxs-lookup"><span data-stu-id="5edc9-264">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="5edc9-265">Версия спецификации формата файла GZIP 4.3</span><span class="sxs-lookup"><span data-stu-id="5edc9-265">GZIP file format specification version 4.3</span></span>](http://www.ietf.org/rfc/rfc1952.txt)
