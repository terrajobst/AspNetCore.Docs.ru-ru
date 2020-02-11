---
title: Сжатие ответов в ASP.NET Core
author: guardrex
description: Сведения о сжатии откликов и способах использования ПО промежуточного слоя для сжатия откликов в приложениях ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: performance/response-compression
ms.openlocfilehash: d37b05edd55ac0d3910855563b819114cf815b43
ms.sourcegitcommit: 235623b6e5a5d1841139c82a11ac2b4b3f31a7a9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2020
ms.locfileid: "77114813"
---
# <a name="response-compression-in-aspnet-core"></a>Сжатие ответов в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

::: moniker range=">= aspnetcore-3.0"

Пропускная способность сети является ограниченным ресурсом. Уменьшение размера ответа обычно значительно увеличивает скорость реагирования приложения. Одним из способов уменьшения размера полезных данных является сжатие ответов приложения.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="when-to-use-response-compression-middleware"></a>Использование по промежуточного слоя сжатия ответа

Используйте технологии сжатия серверных ответов на базе IIS, Apache или nginx. Возможно, производительность по промежуточного слоя не соответствует числу серверных модулей. Сервер [http. sys](xref:fundamentals/servers/httpsys) и сервер [Kestrel](xref:fundamentals/servers/kestrel) в настоящее время не предлагают встроенную поддержку сжатия.

Используйте по промежуточного слоя сжатия ответов, если:

* Не удается использовать следующие технологии сжатия на основе серверов:
  * [Модуль динамического сжатия IIS](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [Модуль Apache mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [Сжатие и распаковка nginx](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* Размещение непосредственно в:
  * [Сервер HTTP. sys](xref:fundamentals/servers/httpsys) (ранее именуемый прослушиваемей)
  * [Сервер Kestrel](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a>Сжатие ответов

Как правило, любой ответ, не сжатый в собственном формате, может выиграть от сжатия ответа. Ответы, не сжатые в собственном формате, обычно включают в себя CSS, JavaScript, HTML, XML и JSON. Не следует сжимать в собственном формате активы, такие как PNG-файлы. При попытке дальнейшего сжатия отклика, сжатого в собственном формате, любое небольшое уменьшение размера и времени передачи, скорее всего, будет превышено на время, затраченное на обработку сжатия. Не сжимать файлы размером менее 150-1000 байт (в зависимости от содержимого файла и эффективности сжатия). Затраты на сжатие мелких файлов могут привести к созданию сжатого файла, большего, чем несжатый файл.

Когда клиент может обработать сжатое содержимое, клиент должен сообщить серверу о своих возможностях, отправив заголовок `Accept-Encoding` с запросом. Когда сервер отправляет сжатое содержимое, он должен содержать сведения в заголовке `Content-Encoding` о кодировании сжатого ответа. В следующей таблице показаны конструкции кодирования содержимого, поддерживаемые по промежуточного слоя.

| значения заголовков `Accept-Encoding` | Поддерживается по промежуточного слоя | Description |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | Да (по умолчанию)        | [Формат сжатых данных Brotli](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | нет                   | [Сжатый формат сжатых данных](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | нет                   | [Эффективный XML-обмен в формате W3C](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | Да                  | [Формат файла gzip](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | Да                  | Идентификатор "без кодирования": ответ не должен быть закодирован. |
| `pack200-gzip`                  | нет                   | [Формат сетевой пересылки для архивов Java](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | Да                  | Любая доступная кодировка содержимого, которая не запрашивается явно |

Дополнительные сведения см. в [списке официального кодирования содержимого IANA](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).

По промежуточного слоя позволяет добавлять дополнительные поставщики сжатия для настраиваемых значений заголовков `Accept-Encoding`. Дополнительные сведения см. в разделе [Настраиваемые поставщики](#custom-providers) ниже.

По промежуточного слоя может реагировать на весовые значения качества (квалуе, `q`) при отправке клиентом для определения приоритета схем сжатия. Дополнительные сведения см. в [документе RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).

Алгоритмы сжатия подчиняются компромиссу между скоростью сжатия и эффективностью сжатия. *Эффективность* в этом контексте означает размер выходных данных после сжатия. Наименьший размер достигается самым *оптимальным* сжатием.

Заголовки, используемые для запроса, отправки, кэширования и получения сжатого содержимого, описаны в таблице ниже.

| Заголовок             | Роль |
| ------------------ | ---- |
| `Accept-Encoding`  | Отправляется с клиента на сервер, чтобы указать схемы кодировки содержимого, приемлемые для клиента. |
| `Content-Encoding` | Отправляется с сервера клиенту для указания кодировки содержимого в полезных данных. |
| `Content-Length`   | Когда происходит сжатие, заголовок `Content-Length` удаляется, так как содержимое текста изменяется при сжатии ответа. |
| `Content-MD5`      | Когда происходит сжатие, заголовок `Content-MD5` удаляется, так как содержимое текста изменилось и хэш больше не является допустимым. |
| `Content-Type`     | Указывает тип MIME содержимого. Каждый ответ должен указывать его `Content-Type`. По промежуточного слоя проверяет это значение, чтобы определить, следует ли сжимать ответ. По промежуточного слоя указывает набор [типов MIME по умолчанию](#mime-types) , которые он может кодировать, но можно заменить или добавить типы MIME. |
| `Vary`             | При отправке сервером значения `Accept-Encoding` клиентам и прокси заголовок `Vary` указывает клиенту или прокси-серверу, что он должен кэшировать (Vary) ответы на основе значения заголовка `Accept-Encoding` запроса. Результат возврата содержимого с заголовком `Vary: Accept-Encoding` заключается в том, что как сжатые, так и несжатые ответы кэшируются отдельно. |

Изучите функции по промежуточного слоя сжатия ответов с [примером приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples). В примере показано следующее:

* Сжатие ответов приложений с помощью gzip и пользовательских поставщиков сжатия.
* Добавление типа MIME в список по умолчанию типов MIME для сжатия.

## <a name="package"></a>Пакет

По промежуточного слоя сжатия ответа предоставляется пакетом [Microsoft. AspNetCore. респонсекомпрессион](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) , который неявно включается в ASP.NET Core приложения.

## <a name="configuration"></a>Конфигурация

В следующем коде показано, как включить по промежуточного слоя сжатия ответа для типов MIME по умолчанию и поставщиков сжатия ([Brotli](#brotli-compression-provider) и [gzip](#gzip-compression-provider)):

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

Примечания.

* `app.UseResponseCompression` должны вызываться до какого бы то ни было по промежуточного слоя, которое сжимает ответы. Дополнительные сведения см. в разделе <xref:fundamentals/middleware/index#middleware-order>.
* Используйте такое средство, как [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)или [POST](https://www.getpostman.com/) , чтобы задать заголовок запроса `Accept-Encoding` и изучить заголовки, размер и текст ответа.

Отправьте запрос в пример приложения без заголовка `Accept-Encoding` и убедитесь, что ответ не сжат. Заголовки `Content-Encoding` и `Vary` отсутствуют в ответе.

![Окно Fiddler, показывающее результат запроса без заголовка Accept-Encoding. Ответ не сжат.](response-compression/_static/request-uncompressed.png)

Отправьте запрос в пример приложения с заголовком `Accept-Encoding: br` (сжатие Brotli) и обратите внимание на то, что ответ сжат. В ответе имеются заголовки `Content-Encoding` и `Vary`.

![Окно Fiddler, показывающее результат запроса с заголовком Accept-Encoding и значением br. Заголовки Vary и Content-Encoding добавляются в ответ. Ответ сжимается.](response-compression/_static/request-compressed-br.png)

## <a name="providers"></a>Поставщики

### <a name="brotli-compression-provider"></a>Поставщик сжатия Brotli

Используйте <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> для сжатия ответов с [форматом сжатых данных Brotli](https://tools.ietf.org/html/rfc7932).

Если поставщики сжатия явно не добавляются в <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:

* Поставщик сжатия Brotli по умолчанию добавляется в массив поставщиков сжатия вместе с [поставщиком сжатия Gzip](#gzip-compression-provider).
* Сжатие по умолчанию используется для сжатия Brotli, если клиент поддерживает формат сжатых данных Brotli. Если Brotli не поддерживается клиентом, то сжатие по умолчанию используется gzip, если клиент поддерживает сжатие Gzip.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

Поставщик сжатия Бротоли должен быть добавлен при явном добавлении любых поставщиков сжатия:

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

Задайте уровень сжатия <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>. Поставщик сжатия Brotli по умолчанию имеет самый быстрый уровень сжатия ([CompressionLevel. самый быстрый](xref:System.IO.Compression.CompressionLevel)), что может привести к неэффективному сжатию. Если требуется наиболее эффективное сжатие, настройте по промежуточного слоя для оптимального сжатия.

| Compression Level | Description |
| ----------------- | ----------- |
| [CompressionLevel. самый быстрый](xref:System.IO.Compression.CompressionLevel) | Сжатие должно завершаться как можно быстрее, даже если полученные выходные данные не будут оптимально сжаты. |
| [CompressionLevel. уплотнение](xref:System.IO.Compression.CompressionLevel) | Сжатие выполнять не нужно. |
| [CompressionLevel. оптимально](xref:System.IO.Compression.CompressionLevel) | Ответы должны быть оптимально сжаты, даже если сжатие занимает больше времени. |

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

### <a name="gzip-compression-provider"></a>Поставщик сжатия GZIP

Используйте <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> для сжатия ответов с [форматом файла gzip](https://tools.ietf.org/html/rfc1952).

Если поставщики сжатия явно не добавляются в <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:

* Поставщик сжатия Gzip добавляется по умолчанию в массив поставщиков сжатия вместе с [поставщиком сжатия Brotli](#brotli-compression-provider).
* Сжатие по умолчанию используется для сжатия Brotli, если клиент поддерживает формат сжатых данных Brotli. Если Brotli не поддерживается клиентом, то сжатие по умолчанию используется gzip, если клиент поддерживает сжатие Gzip.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

При явном добавлении поставщиков сжатия необходимо добавить поставщик сжатия gzip:

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

Задайте уровень сжатия <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>. Поставщик сжатия Gzip по умолчанию имеет самый быстрый уровень сжатия ([CompressionLevel. самый быстрый](xref:System.IO.Compression.CompressionLevel)), что может не привести к максимально эффективному сжатию. Если требуется наиболее эффективное сжатие, настройте по промежуточного слоя для оптимального сжатия.

| Compression Level | Description |
| ----------------- | ----------- |
| [CompressionLevel. самый быстрый](xref:System.IO.Compression.CompressionLevel) | Сжатие должно завершаться как можно быстрее, даже если полученные выходные данные не будут оптимально сжаты. |
| [CompressionLevel. уплотнение](xref:System.IO.Compression.CompressionLevel) | Сжатие выполнять не нужно. |
| [CompressionLevel. оптимально](xref:System.IO.Compression.CompressionLevel) | Ответы должны быть оптимально сжаты, даже если сжатие занимает больше времени. |

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

Создание пользовательских реализаций сжатия с помощью <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>. <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> представляет кодировку содержимого, которую `ICompressionProvider` создает. По промежуточного слоя использует эти сведения для выбора поставщика на основе списка, указанного в заголовке `Accept-Encoding` запроса.

С помощью примера приложения клиент отправляет запрос с заголовком `Accept-Encoding: mycustomcompression`. По промежуточного слоя использует реализацию пользовательского сжатия и возвращает ответ с заголовком `Content-Encoding: mycustomcompression`. Клиент должен иметь возможность распаковать пользовательскую кодировку, чтобы обеспечить работу пользовательской реализации сжатия.

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/3.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]


Отправьте запрос в пример приложения с заголовком `Accept-Encoding: mycustomcompression` и просмотрите заголовки ответа. В ответе имеются заголовки `Vary` и `Content-Encoding`. Текст ответа (не показан) не сжимается образцом. В классе `CustomCompressionProvider` образца отсутствует реализация сжатия. Однако в примере показано, где следует реализовать такой алгоритм сжатия.

![Окно Fiddler, показывающее результат запроса с заголовком Accept-Encoding и значением микустомкомпрессион. Заголовки Vary и Content-Encoding добавляются в ответ.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a>типы MIME

По промежуточного слоя указывает набор типов MIME по умолчанию для сжатия:

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

Замените или добавьте типы MIME с помощью параметров по промежуточного слоя сжатия ответа. Обратите внимание, что типы MIME с подстановочными знаками, такие как `text/*`, не поддерживаются. Пример приложения добавляет тип MIME для `image/svg+xml` и сжимает и обслуживает изображение ASP.NET Core баннера (*Banner. SVG*).

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

## <a name="compression-with-secure-protocol"></a>Сжатие по защищенному протоколу

Сжатые ответы по защищенным подключениям можно контролировать с помощью параметра `EnableForHttps`, который по умолчанию отключен. Использование сжатия с динамически создаваемыми страницами может привести к проблемам безопасности, таким как [преступления](https://wikipedia.org/wiki/CRIME_(security_exploit)) и [нарушение](https://wikipedia.org/wiki/BREACH_(security_exploit)) атак.

## <a name="adding-the-vary-header"></a>Добавление заголовка Vary

При сжатии ответов на основе заголовка `Accept-Encoding` существует потенциально несколько сжатых версий ответа и несжатая версия. Чтобы настроить кэширование клиента и прокси-сервера на наличие нескольких версий и их хранения, `Vary` заголовок добавляется со значением `Accept-Encoding`. В ASP.NET Core 2,0 или более поздней версии по промежуточного слоя автоматически добавляет заголовок `Vary` при сжатии ответа.

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a>Проблемы по промежуточного слоя, когда nginx обратный прокси-сервер

При выполнении запроса через прокси-сервер nginx заголовок `Accept-Encoding` удаляется. Удаление заголовка `Accept-Encoding` предотвращает сжатие ответа по промежуточного слоя. Дополнительные сведения см. в разделе [nginx: сжатие и распаковка](https://www.nginx.com/resources/admin-guide/compression-and-decompression/). Эта проблема проявляется на [рисунке сквозного сжатия для nginx (ASPNET/басикмиддлеваре #123)](https://github.com/aspnet/BasicMiddleware/issues/123).

## <a name="working-with-iis-dynamic-compression"></a>Работа с динамическим сжатием IIS

При наличии активного модуля динамического сжатия IIS, настроенного на уровне сервера, который вы хотите отключить для приложения, отключите модуль, дополнив добавление к файлу *Web. config* . Дополнительные сведения см. в разделе [Отключение модулей IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).

## <a name="troubleshooting"></a>Устранение неполадок

Используйте такие средства, как [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)или [POST](https://www.getpostman.com/), которые позволяют задать заголовок запроса `Accept-Encoding` и изучить заголовки, размер и текст ответа. По умолчанию по промежуточного слоя для сжатия ответов сжимает ответы, соответствующие следующим условиям.

* Заголовок `Accept-Encoding` имеет значение `br`, `gzip`, `*`или пользовательское кодирование, соответствующее настраиваемому поставщику сжатия, который вы установили. Значение не должно быть `identity` или иметь значение свойства Value (квалуе, `q`), равное 0 (нулю).
* Тип MIME (`Content-Type`) должен быть установлен и должен соответствовать типу MIME, настроенному в <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.
* Запрос не должен включать заголовок `Content-Range`.
* В запросе должен использоваться небезопасный протокол (http), если в параметрах по промежуточного слоя сжатия отклика не настроен защищенный протокол (HTTPS). *Обратите внимание на опасность, [описанную выше](#compression-with-secure-protocol) , при включении безопасного сжатия содержимого.*

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [Сеть для разработчиков Mozilla: Accept-Encoding](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [Раздел RFC 7231 3.1.2.1. Кодирование содержимого](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [Раздел RFC 7230 4.2.3: кодирование gzip](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [Спецификация формата файла GZIP, версия 4,3](https://www.ietf.org/rfc/rfc1952.txt)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Пропускная способность сети является ограниченным ресурсом. Уменьшение размера ответа обычно значительно увеличивает скорость реагирования приложения. Одним из способов уменьшения размера полезных данных является сжатие ответов приложения.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="when-to-use-response-compression-middleware"></a>Использование по промежуточного слоя сжатия ответа

Используйте технологии сжатия серверных ответов на базе IIS, Apache или nginx. Возможно, производительность по промежуточного слоя не соответствует числу серверных модулей. Сервер [http. sys](xref:fundamentals/servers/httpsys) и сервер [Kestrel](xref:fundamentals/servers/kestrel) в настоящее время не предлагают встроенную поддержку сжатия.

Используйте по промежуточного слоя сжатия ответов, если:

* Не удается использовать следующие технологии сжатия на основе серверов:
  * [Модуль динамического сжатия IIS](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [Модуль Apache mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [Сжатие и распаковка nginx](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* Размещение непосредственно в:
  * [Сервер HTTP. sys](xref:fundamentals/servers/httpsys) (ранее именуемый прослушиваемей)
  * [Сервер Kestrel](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a>Сжатие ответов

Как правило, любой ответ, не сжатый в собственном формате, может выиграть от сжатия ответа. Ответы, не сжатые в собственном формате, обычно включают в себя CSS, JavaScript, HTML, XML и JSON. Не следует сжимать в собственном формате активы, такие как PNG-файлы. При попытке дальнейшего сжатия отклика, сжатого в собственном формате, любое небольшое уменьшение размера и времени передачи, скорее всего, будет превышено на время, затраченное на обработку сжатия. Не сжимать файлы размером менее 150-1000 байт (в зависимости от содержимого файла и эффективности сжатия). Затраты на сжатие мелких файлов могут привести к созданию сжатого файла, большего, чем несжатый файл.

Когда клиент может обработать сжатое содержимое, клиент должен сообщить серверу о своих возможностях, отправив заголовок `Accept-Encoding` с запросом. Когда сервер отправляет сжатое содержимое, он должен содержать сведения в заголовке `Content-Encoding` о кодировании сжатого ответа. В следующей таблице показаны конструкции кодирования содержимого, поддерживаемые по промежуточного слоя.

| значения заголовков `Accept-Encoding` | Поддерживается по промежуточного слоя | Description |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | Да (по умолчанию)        | [Формат сжатых данных Brotli](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | нет                   | [Сжатый формат сжатых данных](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | нет                   | [Эффективный XML-обмен в формате W3C](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | Да                  | [Формат файла gzip](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | Да                  | Идентификатор "без кодирования": ответ не должен быть закодирован. |
| `pack200-gzip`                  | нет                   | [Формат сетевой пересылки для архивов Java](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | Да                  | Любая доступная кодировка содержимого, которая не запрашивается явно |

Дополнительные сведения см. в [списке официального кодирования содержимого IANA](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).

По промежуточного слоя позволяет добавлять дополнительные поставщики сжатия для настраиваемых значений заголовков `Accept-Encoding`. Дополнительные сведения см. в разделе [Настраиваемые поставщики](#custom-providers) ниже.

По промежуточного слоя может реагировать на весовые значения качества (квалуе, `q`) при отправке клиентом для определения приоритета схем сжатия. Дополнительные сведения см. в [документе RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).

Алгоритмы сжатия подчиняются компромиссу между скоростью сжатия и эффективностью сжатия. *Эффективность* в этом контексте означает размер выходных данных после сжатия. Наименьший размер достигается самым *оптимальным* сжатием.

Заголовки, используемые для запроса, отправки, кэширования и получения сжатого содержимого, описаны в таблице ниже.

| Заголовок             | Роль |
| ------------------ | ---- |
| `Accept-Encoding`  | Отправляется с клиента на сервер, чтобы указать схемы кодировки содержимого, приемлемые для клиента. |
| `Content-Encoding` | Отправляется с сервера клиенту для указания кодировки содержимого в полезных данных. |
| `Content-Length`   | Когда происходит сжатие, заголовок `Content-Length` удаляется, так как содержимое текста изменяется при сжатии ответа. |
| `Content-MD5`      | Когда происходит сжатие, заголовок `Content-MD5` удаляется, так как содержимое текста изменилось и хэш больше не является допустимым. |
| `Content-Type`     | Указывает тип MIME содержимого. Каждый ответ должен указывать его `Content-Type`. По промежуточного слоя проверяет это значение, чтобы определить, следует ли сжимать ответ. По промежуточного слоя указывает набор [типов MIME по умолчанию](#mime-types) , которые он может кодировать, но можно заменить или добавить типы MIME. |
| `Vary`             | При отправке сервером значения `Accept-Encoding` клиентам и прокси заголовок `Vary` указывает клиенту или прокси-серверу, что он должен кэшировать (Vary) ответы на основе значения заголовка `Accept-Encoding` запроса. Результат возврата содержимого с заголовком `Vary: Accept-Encoding` заключается в том, что как сжатые, так и несжатые ответы кэшируются отдельно. |

Изучите функции по промежуточного слоя сжатия ответов с [примером приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples). В примере показано следующее:

* Сжатие ответов приложений с помощью gzip и пользовательских поставщиков сжатия.
* Добавление типа MIME в список по умолчанию типов MIME для сжатия.

## <a name="package"></a>Пакет

Чтобы включить по промежуточного слоя в проект, добавьте ссылку на [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app), которая включает пакет [Microsoft. AspNetCore. респонсекомпрессион](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) .

## <a name="configuration"></a>Конфигурация

В следующем коде показано, как включить по промежуточного слоя сжатия ответа для типов MIME по умолчанию и поставщиков сжатия ([Brotli](#brotli-compression-provider) и [gzip](#gzip-compression-provider)):

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

Примечания.

* `app.UseResponseCompression` должны вызываться до какого бы то ни было по промежуточного слоя, которое сжимает ответы. Дополнительные сведения см. в разделе <xref:fundamentals/middleware/index#middleware-order>.
* Используйте такое средство, как [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)или [POST](https://www.getpostman.com/) , чтобы задать заголовок запроса `Accept-Encoding` и изучить заголовки, размер и текст ответа.

Отправьте запрос в пример приложения без заголовка `Accept-Encoding` и убедитесь, что ответ не сжат. Заголовки `Content-Encoding` и `Vary` отсутствуют в ответе.

![Окно Fiddler, показывающее результат запроса без заголовка Accept-Encoding. Ответ не сжат.](response-compression/_static/request-uncompressed.png)

Отправьте запрос в пример приложения с заголовком `Accept-Encoding: br` (сжатие Brotli) и обратите внимание на то, что ответ сжат. В ответе имеются заголовки `Content-Encoding` и `Vary`.

![Окно Fiddler, показывающее результат запроса с заголовком Accept-Encoding и значением br. Заголовки Vary и Content-Encoding добавляются в ответ. Ответ сжимается.](response-compression/_static/request-compressed-br.png)

## <a name="providers"></a>Поставщики

### <a name="brotli-compression-provider"></a>Поставщик сжатия Brotli

Используйте <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> для сжатия ответов с [форматом сжатых данных Brotli](https://tools.ietf.org/html/rfc7932).

Если поставщики сжатия явно не добавляются в <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:

* Поставщик сжатия Brotli по умолчанию добавляется в массив поставщиков сжатия вместе с [поставщиком сжатия Gzip](#gzip-compression-provider).
* Сжатие по умолчанию используется для сжатия Brotli, если клиент поддерживает формат сжатых данных Brotli. Если Brotli не поддерживается клиентом, то сжатие по умолчанию используется gzip, если клиент поддерживает сжатие Gzip.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

Поставщик сжатия Бротоли должен быть добавлен при явном добавлении любых поставщиков сжатия:

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

Задайте уровень сжатия <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>. Поставщик сжатия Brotli по умолчанию имеет самый быстрый уровень сжатия ([CompressionLevel. самый быстрый](xref:System.IO.Compression.CompressionLevel)), что может привести к неэффективному сжатию. Если требуется наиболее эффективное сжатие, настройте по промежуточного слоя для оптимального сжатия.

| Compression Level | Description |
| ----------------- | ----------- |
| [CompressionLevel. самый быстрый](xref:System.IO.Compression.CompressionLevel) | Сжатие должно завершаться как можно быстрее, даже если полученные выходные данные не будут оптимально сжаты. |
| [CompressionLevel. уплотнение](xref:System.IO.Compression.CompressionLevel) | Сжатие выполнять не нужно. |
| [CompressionLevel. оптимально](xref:System.IO.Compression.CompressionLevel) | Ответы должны быть оптимально сжаты, даже если сжатие занимает больше времени. |

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

### <a name="gzip-compression-provider"></a>Поставщик сжатия GZIP

Используйте <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> для сжатия ответов с [форматом файла gzip](https://tools.ietf.org/html/rfc1952).

Если поставщики сжатия явно не добавляются в <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:

* Поставщик сжатия Gzip добавляется по умолчанию в массив поставщиков сжатия вместе с [поставщиком сжатия Brotli](#brotli-compression-provider).
* Сжатие по умолчанию используется для сжатия Brotli, если клиент поддерживает формат сжатых данных Brotli. Если Brotli не поддерживается клиентом, то сжатие по умолчанию используется gzip, если клиент поддерживает сжатие Gzip.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

При явном добавлении поставщиков сжатия необходимо добавить поставщик сжатия gzip:

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

Задайте уровень сжатия <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>. Поставщик сжатия Gzip по умолчанию имеет самый быстрый уровень сжатия ([CompressionLevel. самый быстрый](xref:System.IO.Compression.CompressionLevel)), что может не привести к максимально эффективному сжатию. Если требуется наиболее эффективное сжатие, настройте по промежуточного слоя для оптимального сжатия.

| Compression Level | Description |
| ----------------- | ----------- |
| [CompressionLevel. самый быстрый](xref:System.IO.Compression.CompressionLevel) | Сжатие должно завершаться как можно быстрее, даже если полученные выходные данные не будут оптимально сжаты. |
| [CompressionLevel. уплотнение](xref:System.IO.Compression.CompressionLevel) | Сжатие выполнять не нужно. |
| [CompressionLevel. оптимально](xref:System.IO.Compression.CompressionLevel) | Ответы должны быть оптимально сжаты, даже если сжатие занимает больше времени. |

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

Создание пользовательских реализаций сжатия с помощью <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>. <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> представляет кодировку содержимого, которую `ICompressionProvider` создает. По промежуточного слоя использует эти сведения для выбора поставщика на основе списка, указанного в заголовке `Accept-Encoding` запроса.

С помощью примера приложения клиент отправляет запрос с заголовком `Accept-Encoding: mycustomcompression`. По промежуточного слоя использует реализацию пользовательского сжатия и возвращает ответ с заголовком `Content-Encoding: mycustomcompression`. Клиент должен иметь возможность распаковать пользовательскую кодировку, чтобы обеспечить работу пользовательской реализации сжатия.

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

Отправьте запрос в пример приложения с заголовком `Accept-Encoding: mycustomcompression` и просмотрите заголовки ответа. В ответе имеются заголовки `Vary` и `Content-Encoding`. Текст ответа (не показан) не сжимается образцом. В классе `CustomCompressionProvider` образца отсутствует реализация сжатия. Однако в примере показано, где следует реализовать такой алгоритм сжатия.

![Окно Fiddler, показывающее результат запроса с заголовком Accept-Encoding и значением микустомкомпрессион. Заголовки Vary и Content-Encoding добавляются в ответ.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a>типы MIME

По промежуточного слоя указывает набор типов MIME по умолчанию для сжатия:

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

Замените или добавьте типы MIME с помощью параметров по промежуточного слоя сжатия ответа. Обратите внимание, что типы MIME с подстановочными знаками, такие как `text/*`, не поддерживаются. Пример приложения добавляет тип MIME для `image/svg+xml` и сжимает и обслуживает изображение ASP.NET Core баннера (*Banner. SVG*).

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

## <a name="compression-with-secure-protocol"></a>Сжатие по защищенному протоколу

Сжатые ответы по защищенным подключениям можно контролировать с помощью параметра `EnableForHttps`, который по умолчанию отключен. Использование сжатия с динамически создаваемыми страницами может привести к проблемам безопасности, таким как [преступления](https://wikipedia.org/wiki/CRIME_(security_exploit)) и [нарушение](https://wikipedia.org/wiki/BREACH_(security_exploit)) атак.

## <a name="adding-the-vary-header"></a>Добавление заголовка Vary

При сжатии ответов на основе заголовка `Accept-Encoding` существует потенциально несколько сжатых версий ответа и несжатая версия. Чтобы настроить кэширование клиента и прокси-сервера на наличие нескольких версий и их хранения, `Vary` заголовок добавляется со значением `Accept-Encoding`. В ASP.NET Core 2,0 или более поздней версии по промежуточного слоя автоматически добавляет заголовок `Vary` при сжатии ответа.

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a>Проблемы по промежуточного слоя, когда nginx обратный прокси-сервер

При выполнении запроса через прокси-сервер nginx заголовок `Accept-Encoding` удаляется. Удаление заголовка `Accept-Encoding` предотвращает сжатие ответа по промежуточного слоя. Дополнительные сведения см. в разделе [nginx: сжатие и распаковка](https://www.nginx.com/resources/admin-guide/compression-and-decompression/). Эта проблема проявляется на [рисунке сквозного сжатия для nginx (ASPNET/басикмиддлеваре #123)](https://github.com/aspnet/BasicMiddleware/issues/123).

## <a name="working-with-iis-dynamic-compression"></a>Работа с динамическим сжатием IIS

При наличии активного модуля динамического сжатия IIS, настроенного на уровне сервера, который вы хотите отключить для приложения, отключите модуль, дополнив добавление к файлу *Web. config* . Дополнительные сведения см. в разделе [Отключение модулей IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).

## <a name="troubleshooting"></a>Устранение неполадок

Используйте такие средства, как [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)или [POST](https://www.getpostman.com/), которые позволяют задать заголовок запроса `Accept-Encoding` и изучить заголовки, размер и текст ответа. По умолчанию по промежуточного слоя для сжатия ответов сжимает ответы, соответствующие следующим условиям.

* Заголовок `Accept-Encoding` имеет значение `br`, `gzip`, `*`или пользовательское кодирование, соответствующее настраиваемому поставщику сжатия, который вы установили. Значение не должно быть `identity` или иметь значение свойства Value (квалуе, `q`), равное 0 (нулю).
* Тип MIME (`Content-Type`) должен быть установлен и должен соответствовать типу MIME, настроенному в <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.
* Запрос не должен включать заголовок `Content-Range`.
* В запросе должен использоваться небезопасный протокол (http), если в параметрах по промежуточного слоя сжатия отклика не настроен защищенный протокол (HTTPS). *Обратите внимание на опасность, [описанную выше](#compression-with-secure-protocol) , при включении безопасного сжатия содержимого.*

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [Сеть для разработчиков Mozilla: Accept-Encoding](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [Раздел RFC 7231 3.1.2.1. Кодирование содержимого](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [Раздел RFC 7230 4.2.3: кодирование gzip](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [Спецификация формата файла GZIP, версия 4,3](https://www.ietf.org/rfc/rfc1952.txt)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Пропускная способность сети является ограниченным ресурсом. Уменьшение размера ответа обычно значительно увеличивает скорость реагирования приложения. Одним из способов уменьшения размера полезных данных является сжатие ответов приложения.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="when-to-use-response-compression-middleware"></a>Использование по промежуточного слоя сжатия ответа

Используйте технологии сжатия серверных ответов на базе IIS, Apache или nginx. Возможно, производительность по промежуточного слоя не соответствует числу серверных модулей. Сервер [http. sys](xref:fundamentals/servers/httpsys) и сервер [Kestrel](xref:fundamentals/servers/kestrel) в настоящее время не предлагают встроенную поддержку сжатия.

Используйте по промежуточного слоя сжатия ответов, если:

* Не удается использовать следующие технологии сжатия на основе серверов:
  * [Модуль динамического сжатия IIS](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [Модуль Apache mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [Сжатие и распаковка nginx](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* Размещение непосредственно в:
  * [Сервер HTTP. sys](xref:fundamentals/servers/httpsys) (ранее именуемый прослушиваемей)
  * [Сервер Kestrel](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a>Сжатие ответов

Как правило, любой ответ, не сжатый в собственном формате, может выиграть от сжатия ответа. Ответы, не сжатые в собственном формате, обычно включают в себя CSS, JavaScript, HTML, XML и JSON. Не следует сжимать в собственном формате активы, такие как PNG-файлы. При попытке дальнейшего сжатия отклика, сжатого в собственном формате, любое небольшое уменьшение размера и времени передачи, скорее всего, будет превышено на время, затраченное на обработку сжатия. Не сжимать файлы размером менее 150-1000 байт (в зависимости от содержимого файла и эффективности сжатия). Затраты на сжатие мелких файлов могут привести к созданию сжатого файла, большего, чем несжатый файл.

Когда клиент может обработать сжатое содержимое, клиент должен сообщить серверу о своих возможностях, отправив заголовок `Accept-Encoding` с запросом. Когда сервер отправляет сжатое содержимое, он должен содержать сведения в заголовке `Content-Encoding` о кодировании сжатого ответа. В следующей таблице показаны конструкции кодирования содержимого, поддерживаемые по промежуточного слоя.

| значения заголовков `Accept-Encoding` | Поддерживается по промежуточного слоя | Description |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | нет                   | [Формат сжатых данных Brotli](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | нет                   | [Сжатый формат сжатых данных](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | нет                   | [Эффективный XML-обмен в формате W3C](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | Да (по умолчанию)        | [Формат файла gzip](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | Да                  | Идентификатор "без кодирования": ответ не должен быть закодирован. |
| `pack200-gzip`                  | нет                   | [Формат сетевой пересылки для архивов Java](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | Да                  | Любая доступная кодировка содержимого, которая не запрашивается явно |

Дополнительные сведения см. в [списке официального кодирования содержимого IANA](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).

По промежуточного слоя позволяет добавлять дополнительные поставщики сжатия для настраиваемых значений заголовков `Accept-Encoding`. Дополнительные сведения см. в разделе [Настраиваемые поставщики](#custom-providers) ниже.

По промежуточного слоя может реагировать на весовые значения качества (квалуе, `q`) при отправке клиентом для определения приоритета схем сжатия. Дополнительные сведения см. в [документе RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).

Алгоритмы сжатия подчиняются компромиссу между скоростью сжатия и эффективностью сжатия. *Эффективность* в этом контексте означает размер выходных данных после сжатия. Наименьший размер достигается самым *оптимальным* сжатием.

Заголовки, используемые для запроса, отправки, кэширования и получения сжатого содержимого, описаны в таблице ниже.

| Заголовок             | Роль |
| ------------------ | ---- |
| `Accept-Encoding`  | Отправляется с клиента на сервер, чтобы указать схемы кодировки содержимого, приемлемые для клиента. |
| `Content-Encoding` | Отправляется с сервера клиенту для указания кодировки содержимого в полезных данных. |
| `Content-Length`   | Когда происходит сжатие, заголовок `Content-Length` удаляется, так как содержимое текста изменяется при сжатии ответа. |
| `Content-MD5`      | Когда происходит сжатие, заголовок `Content-MD5` удаляется, так как содержимое текста изменилось и хэш больше не является допустимым. |
| `Content-Type`     | Указывает тип MIME содержимого. Каждый ответ должен указывать его `Content-Type`. По промежуточного слоя проверяет это значение, чтобы определить, следует ли сжимать ответ. По промежуточного слоя указывает набор [типов MIME по умолчанию](#mime-types) , которые он может кодировать, но можно заменить или добавить типы MIME. |
| `Vary`             | При отправке сервером значения `Accept-Encoding` клиентам и прокси заголовок `Vary` указывает клиенту или прокси-серверу, что он должен кэшировать (Vary) ответы на основе значения заголовка `Accept-Encoding` запроса. Результат возврата содержимого с заголовком `Vary: Accept-Encoding` заключается в том, что как сжатые, так и несжатые ответы кэшируются отдельно. |

Изучите функции по промежуточного слоя сжатия ответов с [примером приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples). В примере показано следующее:

* Сжатие ответов приложений с помощью gzip и пользовательских поставщиков сжатия.
* Добавление типа MIME в список по умолчанию типов MIME для сжатия.

## <a name="package"></a>Пакет

Чтобы включить по промежуточного слоя в проект, добавьте ссылку на [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app), которая включает пакет [Microsoft. AspNetCore. респонсекомпрессион](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) .

## <a name="configuration"></a>Конфигурация

В следующем коде показано, как включить по промежуточного слоя сжатия ответов для типов MIME по умолчанию и [поставщика сжатия Gzip](#gzip-compression-provider):

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

Примечания.

* `app.UseResponseCompression` должны вызываться до какого бы то ни было по промежуточного слоя, которое сжимает ответы. Дополнительные сведения см. в разделе <xref:fundamentals/middleware/index#middleware-order>.
* Используйте такое средство, как [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)или [POST](https://www.getpostman.com/) , чтобы задать заголовок запроса `Accept-Encoding` и изучить заголовки, размер и текст ответа.

Отправьте запрос в пример приложения без заголовка `Accept-Encoding` и убедитесь, что ответ не сжат. Заголовки `Content-Encoding` и `Vary` отсутствуют в ответе.

![Окно Fiddler, показывающее результат запроса без заголовка Accept-Encoding. Ответ не сжат.](response-compression/_static/request-uncompressed.png)

Отправьте запрос в пример приложения с заголовком `Accept-Encoding: gzip` и убедитесь, что ответ сжат. В ответе имеются заголовки `Content-Encoding` и `Vary`.

![Окно Fiddler, показывающее результат запроса с заголовком Accept-Encoding и значением gzip. Заголовки Vary и Content-Encoding добавляются в ответ. Ответ сжимается.](response-compression/_static/request-compressed.png)

## <a name="providers"></a>Поставщики

### <a name="gzip-compression-provider"></a>Поставщик сжатия GZIP

Используйте <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> для сжатия ответов с [форматом файла gzip](https://tools.ietf.org/html/rfc1952).

Если поставщики сжатия явно не добавляются в <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:

* Поставщик сжатия Gzip добавляется по умолчанию в массив поставщиков сжатия.
* Сжатие по умолчанию используется gzip, если клиент поддерживает сжатие Gzip.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

При явном добавлении поставщиков сжатия необходимо добавить поставщик сжатия gzip:

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

Задайте уровень сжатия <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>. Поставщик сжатия Gzip по умолчанию имеет самый быстрый уровень сжатия ([CompressionLevel. самый быстрый](xref:System.IO.Compression.CompressionLevel)), что может не привести к максимально эффективному сжатию. Если требуется наиболее эффективное сжатие, настройте по промежуточного слоя для оптимального сжатия.

| Compression Level | Description |
| ----------------- | ----------- |
| [CompressionLevel. самый быстрый](xref:System.IO.Compression.CompressionLevel) | Сжатие должно завершаться как можно быстрее, даже если полученные выходные данные не будут оптимально сжаты. |
| [CompressionLevel. уплотнение](xref:System.IO.Compression.CompressionLevel) | Сжатие выполнять не нужно. |
| [CompressionLevel. оптимально](xref:System.IO.Compression.CompressionLevel) | Ответы должны быть оптимально сжаты, даже если сжатие занимает больше времени. |

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

Создание пользовательских реализаций сжатия с помощью <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>. <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> представляет кодировку содержимого, которую `ICompressionProvider` создает. По промежуточного слоя использует эти сведения для выбора поставщика на основе списка, указанного в заголовке `Accept-Encoding` запроса.

С помощью примера приложения клиент отправляет запрос с заголовком `Accept-Encoding: mycustomcompression`. По промежуточного слоя использует реализацию пользовательского сжатия и возвращает ответ с заголовком `Content-Encoding: mycustomcompression`. Клиент должен иметь возможность распаковать пользовательскую кодировку, чтобы обеспечить работу пользовательской реализации сжатия.

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

Отправьте запрос в пример приложения с заголовком `Accept-Encoding: mycustomcompression` и просмотрите заголовки ответа. В ответе имеются заголовки `Vary` и `Content-Encoding`. Текст ответа (не показан) не сжимается образцом. В классе `CustomCompressionProvider` образца отсутствует реализация сжатия. Однако в примере показано, где следует реализовать такой алгоритм сжатия.

![Окно Fiddler, показывающее результат запроса с заголовком Accept-Encoding и значением микустомкомпрессион. Заголовки Vary и Content-Encoding добавляются в ответ.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a>типы MIME

По промежуточного слоя указывает набор типов MIME по умолчанию для сжатия:

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

Замените или добавьте типы MIME с помощью параметров по промежуточного слоя сжатия ответа. Обратите внимание, что типы MIME с подстановочными знаками, такие как `text/*`, не поддерживаются. Пример приложения добавляет тип MIME для `image/svg+xml` и сжимает и обслуживает изображение ASP.NET Core баннера (*Banner. SVG*).

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

## <a name="compression-with-secure-protocol"></a>Сжатие по защищенному протоколу

Сжатые ответы по защищенным подключениям можно контролировать с помощью параметра `EnableForHttps`, который по умолчанию отключен. Использование сжатия с динамически создаваемыми страницами может привести к проблемам безопасности, таким как [преступления](https://wikipedia.org/wiki/CRIME_(security_exploit)) и [нарушение](https://wikipedia.org/wiki/BREACH_(security_exploit)) атак.

## <a name="adding-the-vary-header"></a>Добавление заголовка Vary

При сжатии ответов на основе заголовка `Accept-Encoding` существует потенциально несколько сжатых версий ответа и несжатая версия. Чтобы настроить кэширование клиента и прокси-сервера на наличие нескольких версий и их хранения, `Vary` заголовок добавляется со значением `Accept-Encoding`. В ASP.NET Core 2,0 или более поздней версии по промежуточного слоя автоматически добавляет заголовок `Vary` при сжатии ответа.

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a>Проблемы по промежуточного слоя, когда nginx обратный прокси-сервер

При выполнении запроса через прокси-сервер nginx заголовок `Accept-Encoding` удаляется. Удаление заголовка `Accept-Encoding` предотвращает сжатие ответа по промежуточного слоя. Дополнительные сведения см. в разделе [nginx: сжатие и распаковка](https://www.nginx.com/resources/admin-guide/compression-and-decompression/). Эта проблема проявляется на [рисунке сквозного сжатия для nginx (ASPNET/басикмиддлеваре #123)](https://github.com/aspnet/BasicMiddleware/issues/123).

## <a name="working-with-iis-dynamic-compression"></a>Работа с динамическим сжатием IIS

При наличии активного модуля динамического сжатия IIS, настроенного на уровне сервера, который вы хотите отключить для приложения, отключите модуль, дополнив добавление к файлу *Web. config* . Дополнительные сведения см. в разделе [Отключение модулей IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).

## <a name="troubleshooting"></a>Устранение неполадок

Используйте такие средства, как [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)или [POST](https://www.getpostman.com/), которые позволяют задать заголовок запроса `Accept-Encoding` и изучить заголовки, размер и текст ответа. По умолчанию по промежуточного слоя для сжатия ответов сжимает ответы, соответствующие следующим условиям.

* Заголовок `Accept-Encoding` имеет значение `gzip`, `*`или пользовательское кодирование, соответствующее настраиваемому поставщику сжатия, который вы установили. Значение не должно быть `identity` или иметь значение свойства Value (квалуе, `q`), равное 0 (нулю).
* Тип MIME (`Content-Type`) должен быть установлен и должен соответствовать типу MIME, настроенному в <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.
* Запрос не должен включать заголовок `Content-Range`.
* В запросе должен использоваться небезопасный протокол (http), если в параметрах по промежуточного слоя сжатия отклика не настроен защищенный протокол (HTTPS). *Обратите внимание на опасность, [описанную выше](#compression-with-secure-protocol) , при включении безопасного сжатия содержимого.*

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [Сеть для разработчиков Mozilla: Accept-Encoding](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [Раздел RFC 7231 3.1.2.1. Кодирование содержимого](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [Раздел RFC 7230 4.2.3: кодирование gzip](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [Спецификация формата файла GZIP, версия 4,3](https://www.ietf.org/rfc/rfc1952.txt)

::: moniker-end
