---
title: Сжатие откликов в ASP.NET Core
author: guardrex
description: Дополнительные сведения о сжатие откликов и способах использования по промежуточного слоя для сжатия ответов в приложениях ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2018
uid: performance/response-compression
ms.openlocfilehash: d5e0b6ed21c14f2e76396cde846c69a76ad40794
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578150"
---
# <a name="response-compression-in-aspnet-core"></a>Сжатие откликов в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

Пропускная способность сети является ограниченным ресурсом. Уменьшение размера ответа обычно часто значительно увеличивается скорость реагирования приложения. Для сжатия ответов приложения является одним из способов уменьшить размеры полезной нагрузки.

## <a name="when-to-use-response-compression-middleware"></a>Когда следует использовать по промежуточного слоя для сжатия ответов

Использование технологий сжатия ответ на основе сервера в IIS, Apache или Nginx. Производительность по промежуточного слоя скорее всего, не соответствует серверных модулей. [Сервер HTTP.sys](xref:fundamentals/servers/httpsys) и [Kestrel](xref:fundamentals/servers/kestrel) настоящее время не предлагает поддержку встроенную функцию сжатия.

Используйте по промежуточного слоя для сжатия ответов, когда вы будете:

* Не удается использовать со следующими технологиями сжатия на основе сервера:
  * [Модуль динамического сжатия служб IIS](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [Модуль mod_deflate Apache](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [Nginx сжатия и распаковки](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* Размещение непосредственно на:
  * [Сервер HTTP.sys](xref:fundamentals/servers/httpsys) (которая раньше называлась [WebListener](xref:fundamentals/servers/weblistener))
  * [Kestrel](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a>Сжатие откликов

Как правило любой ответ, изначально не сжаты могут использовать преимущества сжатия отклика. Ответы, изначально не сжаты обычно включают: CSS, JavaScript, HTML, XML и JSON. Не следует сжимать изначально сжатых ресурсов, таких как PNG-файлы. При попытке дальнейшее сжатие изначально сжатый ответ, небольшого дополнительного сокращения размера и передачи времени скорее всего будет злоумышленниками время, которое потребовалось для обработки сжатия. Не сжимать файлы размером меньше примерно 150 – 1000 байт (в зависимости от его содержимого и повысить эффективность сжатия). Затраты на сжатие небольших файлов могут давать сжатый файл, размер которых превышает несжатый файл.

Когда клиент может обрабатывать сжатое содержимое, клиент должен уведомления сервера его возможности, отправляя `Accept-Encoding` заголовок с запросом. Когда сервер отправляет сжатое содержимое, она должна содержать сведения в `Content-Encoding` заголовка на способах кодировки сжатого ответа. В следующей таблице показаны содержимого обозначения кодировки, поддерживаемые по промежуточного слоя.

::: moniker range=">= aspnetcore-2.2"

| `Accept-Encoding` значения заголовка | Поддерживается по промежуточного слоя | Описание |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | Да (по умолчанию)        | [Формат сжатых данных Brotli](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | Нет                   | [Формат DEFLATE сжатых данных](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | Нет                   | [W3C XML для эффективного обмена](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | Да                  | [Формат файла gzip](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | Да                  | Идентификатор «Без кодировки»: ответ не должен быть закодирован. |
| `pack200-gzip`                  | Нет                   | [Сетевой формат передачи архивы Java](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | Да                  | Любое доступное содержимое, кодировка не явно запрошенного |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| `Accept-Encoding` значения заголовка | Поддерживается по промежуточного слоя | Описание |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | Нет                   | [Формат сжатых данных Brotli](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | Нет                   | [Формат DEFLATE сжатых данных](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | Нет                   | [W3C XML для эффективного обмена](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | Да (по умолчанию)        | [Формат файла gzip](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | Да                  | Идентификатор «Без кодировки»: ответ не должен быть закодирован. |
| `pack200-gzip`                  | Нет                   | [Сетевой формат передачи архивы Java](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | Да                  | Любое доступное содержимое, кодировка не явно запрошенного |

::: moniker-end

Дополнительные сведения см. в разделе [IANA официальный кодирования списка содержимого](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).

По промежуточного слоя можно добавить дополнительное сжатие поставщиков для пользовательских `Accept-Encoding` значения заголовка. Дополнительные сведения см. в разделе [настраиваемые поставщики](#custom-providers) ниже.

По промежуточного слоя способен реагирование на значение качества (qvalue, `q`) вес при отправке клиентом для определения приоритетов схемы сжатия. Дополнительные сведения см. в разделе [RFC 7231: приемлемой кодировкой](https://tools.ietf.org/html/rfc7231#section-5.3.4).

Алгоритмы сжатия, распространяются компромисс между скоростью сжатие и эффективность сжатия. *Эффективность* в данном контексте означает размер выходных данных после сжатия. Наименьший размер достигается за счет наиболее *оптимальной* сжатия.

Заголовки, участвующих в запросе, отправки, кэширование и получать сжатое содержимое описаны в следующей таблице.

| Header             | Роль |
| ------------------ | ---- |
| `Accept-Encoding`  | Отправленные клиентом на сервер для указания содержимого, кодирование схемы, допустимых для клиента. |
| `Content-Encoding` | Клиенту с сервера для обозначения кодировки содержимого в полезных данных. |
| `Content-Length`   | В случае сжатия `Content-Length` заголовок удаляется с момента изменения содержимого текста при ответе сжимается. |
| `Content-MD5`      | В случае сжатия `Content-MD5` заголовок удаляется, так как содержимое тела был изменен и хэш-код больше не является допустимым. |
| `Content-Type`     | Указывает тип MIME содержимого. Каждый ответ следует указать его `Content-Type`. По промежуточного слоя проверяет это значение, чтобы определить, следует ли сжимать ответа. По промежуточного слоя задает набор [по умолчанию типы MIME](#mime-types) , его можно закодировать, но можно заменить или добавить типы MIME. |
| `Vary`             | При отправке сервером со значением `Accept-Encoding` для клиентов и прокси-серверы, `Vary` заголовок указывает клиенту или прокси-сервер, его следует кэшировать (различаться) ответы на основе значения из `Accept-Encoding` заголовок запроса. Результат возврата содержимого с помощью `Vary: Accept-Encoding` заголовка является как краткая, так и без сжатия ответов, кэшируются отдельно. |

Изучите возможности по промежуточного слоя сжатия ответов с [пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples). В образце показано:

* Сжатие ответов приложения с помощью Gzip и сжатие пользовательских поставщиков.
* Как добавить тип MIME по умолчанию список типов MIME для сжатия.

## <a name="package"></a>Пакет

::: moniker range=">= aspnetcore-2.1"

Чтобы включить по промежуточного слоя в проекте, добавьте ссылку на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), который включает [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) пакета.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Чтобы включить по промежуточного слоя в проекте, добавьте ссылку на [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage), который включает [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) пакета.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Чтобы включить по промежуточного слоя в проекте, добавьте ссылку на [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) пакета.

::: moniker-end

## <a name="configuration"></a>Конфигурация

::: moniker range=">= aspnetcore-2.2"

Ниже показано, как включить по промежуточного слоя сжатия ответов типы MIME по умолчанию, а также поставщиков сжатия ([Brotli](#brotli-compression-provider) и [Gzip](#gzip-compression-provider)):

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Ниже показано, как включить по промежуточного слоя сжатия ответов для типов MIME по умолчанию и [поставщика сжатие Gzip](#gzip-compression-provider):

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
> Используйте инструменты, например [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), или [Postman](https://www.getpostman.com/) присвоить `Accept-Encoding` заголовок запроса и изучите заголовки ответа, размер и текст.

Отправить запрос в пример приложения без `Accept-Encoding` заголовка и обратите внимание, что ответ без сжатия. `Content-Encoding` И `Vary` заголовки не присутствуют в ответе.

![Fiddler окно, отображающее результат запроса без заголовка Accept-Encoding. Ответ не сжимается.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

Отправить запрос в пример приложения с `Accept-Encoding: br` заголовка (Brotli сжатие) и обратите внимание, что ответ сжата. `Content-Encoding` И `Vary` заголовки присутствуют в ответе.

![Fiddler окно, отображающее результат запроса с заголовком Accept-Encoding и значение br. Заголовки Vary и Content-Encoding, добавляются в ответ. Ответ сжимается.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Отправить запрос в пример приложения с `Accept-Encoding: gzip` заголовка и обратите внимание, что ответ сжата. `Content-Encoding` И `Vary` заголовки присутствуют в ответе.

![Fiddler окно, отображающее результат запроса с заголовком Accept-Encoding и значение gzip. Заголовки Vary и Content-Encoding, добавляются в ответ. Ответ сжимается.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a>Поставщики

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a>Поставщик сжатие Brotli

Используйте `BrotliCompressionProvider` для сжатия ответов с [формат сжатых данных Brotli](https://tools.ietf.org/html/rfc7932).

Если поставщики отсутствуют сжатия явно добавляются <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:

* Массив поставщиков сжатия вместе с по умолчанию добавляется поставщик сжатие Brotli [поставщика сжатие Gzip](#gzip-compression-provider).
* По умолчанию сжатие Brotli сжатие при Brotli формат сжатых данных, поддерживаемые клиентом. Если клиент не поддерживается Brotli, сжатие по умолчанию в Gzip Если клиент поддерживает сжатие Gzip.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

Поставщик сжатия Brotoli должен быть добавлен, при любых поставщиков сжатия добавляются явным образом:

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

Установка уровня с помощью сжатия `BrotliCompressionProviderOptions`. По умолчанию используется поставщик сжатие Brotli наиболее быстрый уровень сжатия ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), который может не обеспечить наиболее эффективное сжатие. При необходимости наиболее эффективного сжатия настройте по промежуточного слоя для оптимального сжатия.

| Уровень сжатия | Описание |
| ----------------- | ----------- |
| [CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel) | Сжатие следует выполнить как можно быстрее, даже если полученный результат не сжат оптимально. |
| [CompressionLevel.NoCompression](xref:System.IO.Compression.CompressionLevel) | Не требуется сжимать. |
| [CompressionLevel.Optimal](xref:System.IO.Compression.CompressionLevel) | Ответы должно применяться оптимальное сжатие, даже если сжатие занимает больше времени. |

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

### <a name="gzip-compression-provider"></a>Поставщик сжатие gzip

Используйте <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> для сжатия ответов с [формате Gzip](https://tools.ietf.org/html/rfc1952).

Если поставщики отсутствуют сжатия явно добавляются <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:

::: moniker range=">= aspnetcore-2.2"

* Массив поставщиков сжатия вместе с по умолчанию добавляется поставщик сжатие Gzip [поставщика сжатие Brotli](#brotli-compression-provider).
* По умолчанию сжатие Brotli сжатие при Brotli формат сжатых данных, поддерживаемые клиентом. Если клиент не поддерживается Brotli, сжатие по умолчанию в Gzip Если клиент поддерживает сжатие Gzip.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* Массив поставщиков сжатия по умолчанию добавляется поставщик сжатия Gzip.
* По умолчанию сжатие Gzip, если клиент поддерживает сжатие Gzip.

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

Сжатие Gzip поставщик должен быть добавлен, при любых поставщиков сжатия явно добавляются:

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

Установка уровня с помощью сжатия <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>. По умолчанию используется поставщик сжатие Gzip наиболее быстрый уровень сжатия ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), который может не обеспечить наиболее эффективное сжатие. При необходимости наиболее эффективного сжатия настройте по промежуточного слоя для оптимального сжатия.

| Уровень сжатия | Описание |
| ----------------- | ----------- |
| [CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel) | Сжатие следует выполнить как можно быстрее, даже если полученный результат не сжат оптимально. |
| [CompressionLevel.NoCompression](xref:System.IO.Compression.CompressionLevel) | Не требуется сжимать. |
| [CompressionLevel.Optimal](xref:System.IO.Compression.CompressionLevel) | Ответы должно применяться оптимальное сжатие, даже если сжатие занимает больше времени. |

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

### <a name="custom-providers"></a>Настраиваемые поставщики

Создание реализации сжатия с <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>. <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> Представляет содержимое, кодировка, что этот `ICompressionProvider` создает. По промежуточного слоя использует эти сведения можно выбрать поставщика, на основе списка, указанное в `Accept-Encoding` заголовок запроса.

С помощью примера приложения, клиент отправляет запрос с помощью `Accept-Encoding: mycustomcompression` заголовка. По промежуточного слоя использует реализацию сжатия и возвращает ответ с `Content-Encoding: mycustomcompression` заголовка. Клиент должен быть способен распаковать настраиваемую кодировку в порядке для реализации пользовательских сжатия для работы.

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

Отправить запрос в пример приложения с `Accept-Encoding: mycustomcompression` заголовка и обратите внимание на заголовки ответа. `Vary` И `Content-Encoding` заголовки присутствуют в ответе. Текст ответа (не показано) не сжимается в образце. Отсутствует реализация сжатия в `CustomCompressionProvider` класс образца. Тем не менее в образце показано, где вам понадобилось бы реализовать алгоритм сжатия.

![Fiddler окно, отображающее результат запроса с заголовком Accept-Encoding и значение mycustomcompression. Заголовки Vary и Content-Encoding, добавляются в ответ.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a>типы MIME

По промежуточного слоя задает набор по умолчанию типы MIME для сжатия:

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

Заменить или добавить типы MIME с параметрами по промежуточного слоя для сжатия ответов. Обратите внимание, что подстановочный знак MIME типы, такие как `text/*` не поддерживаются. Пример приложения добавляет тип MIME для `image/svg+xml` и сжимает и служит изображение баннера в ASP.NET Core (*banner.svg*).

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

## <a name="compression-with-secure-protocol"></a>Сжатие с помощью безопасного протокола

Сжатые ответы по безопасным соединениям можно управлять с помощью `EnableForHttps` параметр, который по умолчанию отключена. Использование сжатия с динамически созданных страниц может привести к проблемам безопасности таких как [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) и [нарушения](https://wikipedia.org/wiki/BREACH_(security_exploit)) атак.

## <a name="adding-the-vary-header"></a>Добавление заголовка Vary

::: moniker range=">= aspnetcore-2.0"

Если сжатие ответов на основе `Accept-Encoding` заголовок, потенциально нескольких версий сжатого ответа и несжатую версию. Чтобы настроить кэш клиента и прокси-сервера, существует несколько версий, а также должны быть сохранены, `Vary` заголовок добавляется с `Accept-Encoding` значение. В ASP.NET Core 2.0 или более поздней версии, по промежуточного слоя добавляет `Vary` заголовка автоматически в том случае, когда ответ сжимается.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Если сжатие ответов на основе `Accept-Encoding` заголовок, потенциально нескольких версий сжатого ответа и несжатую версию. Чтобы настроить кэш клиента и прокси-сервера, существует несколько версий, а также должны быть сохранены, `Vary` заголовок добавляется с `Accept-Encoding` значение. В ASP.NET Core 1.x, добавление `Vary` в ответ заголовок выполняется вручную:

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a>По промежуточного слоя проблемы при работе за Nginx обратный прокси-сервер

При наличии запроса, передаются Nginx, `Accept-Encoding` заголовок удаляется. Это предотвращает сжатие ответ по промежуточного слоя. Дополнительные сведения см. в разделе [NGINX: сжатия и распаковки](https://www.nginx.com/resources/admin-guide/compression-and-decompression/). Эта проблема отслеживается [выяснить сквозной сжатие для Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).

## <a name="working-with-iis-dynamic-compression"></a>Работа с динамического сжатия служб IIS

При наличии активных динамического сжатия модуль IIS настроен на уровне сервера, который вы хотите отключить для приложений, отключите модуль с дополнением к *web.config* файл. Дополнительные сведения см. в разделе [Отключение модулей IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).

## <a name="troubleshooting"></a>Устранение неполадок

Использовать такой инструмент, как [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), или [Postman](https://www.getpostman.com/), которые позволяют задать `Accept-Encoding` заголовок запроса и изучите заголовки ответа, размер и текст. По умолчанию по промежуточного слоя для сжатия ответов сжимает ответы, которые отвечают следующим условиям:

::: moniker range=">= aspnetcore-2.2"

* `Accept-Encoding` Заголовок присутствует со значением `br`, `gzip`, `*`, или настраиваемую кодировку, соответствующий поставщику сжатия, который вы определили. Значение не должно быть `identity` или иметь значение качества (qvalue, `q`) значение 0 (ноль).
* Тип MIME (`Content-Type`) должны быть заданы и должен совпадать с типом MIME, настроенный на <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.
* Запрос не должен содержать `Content-Range` заголовка.
* Запрос должен использовать незащищенный протокол (http), только если в параметрах по промежуточного слоя для сжатия ответов настроена безопасный протокол (https). *Обратите внимание, что приведет к неисправности [описанных выше](#compression-with-secure-protocol) при включении безопасного сжатие содержимого.*

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* `Accept-Encoding` Заголовок присутствует со значением `gzip`, `*`, или настраиваемую кодировку, соответствующий поставщику сжатия, который вы определили. Значение не должно быть `identity` или иметь значение качества (qvalue, `q`) значение 0 (ноль).
* Тип MIME (`Content-Type`) должны быть заданы и должен совпадать с типом MIME, настроенный на <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.
* Запрос не должен содержать `Content-Range` заголовка.
* Запрос должен использовать незащищенный протокол (http), только если в параметрах по промежуточного слоя для сжатия ответов настроена безопасный протокол (https). *Обратите внимание, что приведет к неисправности [описанных выше](#compression-with-secure-protocol) при включении безопасного сжатие содержимого.*

::: moniker-end

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [Сеть разработчиков Mozilla: Приемлемой кодировкой](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [RFC 7231 раздел 3.1.2.1: Содержимого Codings](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [RFC 7230 разделе 4.2.3: Кодирования Gzip](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [Версия спецификации формата файла GZIP 4.3](http://www.ietf.org/rfc/rfc1952.txt)
