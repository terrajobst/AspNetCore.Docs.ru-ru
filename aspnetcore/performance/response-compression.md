---
title: "Ответ сжатия по промежуточного слоя ASP.NET Core"
author: guardrex
description: "Дополнительные сведения о сжатия ответов и использовании ответа сжатия по промежуточного слоя в приложениях ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 08/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/response-compression
ms.openlocfilehash: b93b3fc6c3fafd3e45a5cd42f43aa06dc730db0f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="response-compression-middleware-for-aspnet-core"></a>Ответ сжатия по промежуточного слоя ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

Пропускная способность сети является ограниченным ресурсом. Обычно уменьшить размер ответа часто значительно увеличивает скорость реагирования приложения. Для сжатия ответов приложения является одним из способов уменьшить размер полезных данных.

## <a name="when-to-use-response-compression-middleware"></a>Когда следует использовать ответ сжатия по промежуточного слоя
Используйте технологии сжатия ответ на основе сервера в IIS, Apache или Nginx. Производительность по промежуточного слоя, скорее всего, не будет соответствовать модулей сервера. [HTTP.sys сервера](xref:fundamentals/servers/httpsys) и [Kestrel](xref:fundamentals/servers/kestrel) в настоящее время не предлагают поддержка встроенных сжатия.

Используйте ответа сжатия по промежуточного слоя, когда вы:

* Не удается использовать следующие технологии сжатия на основе сервера:
  * [Модуль сжатия динамического IIS](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [Модуль mod_deflate Apache](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [Nginx сжатия и распаковки](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* Размещение непосредственно на:
  * [HTTP.sys сервера](xref:fundamentals/servers/httpsys) (ранее называвшиеся [WebListener](xref:fundamentals/servers/weblistener))
  * [Kestrel](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a>Сжатие ответа
Как правило любой ответ, изначально не сжаты могут выиграть от сжатия ответов. Ответы, изначально не сжаты обычно включают: CSS, JavaScript, HTML, XML и JSON. Не следует сжимать изначально сжатых ресурсов, таких как файлы PNG. При попытке выполнить дальнейшее сжатие изначально сжатого ответа любой небольшой дополнительное сокращение времени, размер и передачи скорее всего будет злоумышленниками время, затраченное на обработку сжатие. Не сжимать файлы размером меньше примерно 150-1000 байт (в зависимости от его содержимого и повысить эффективность сжатия). Затраты на сжатие небольших файлов может привести к сжатый файл, размер которых превышает несжатого файла.

Когда клиент может обрабатывать сжатое содержимое, клиент должен уведомить сервер его возможности, отправляя `Accept-Encoding` заголовок с запросом. Когда сервер отправляет сжатое содержимое, он должен содержать сведения в `Content-Encoding` заголовок кодируется как сжатого ответа. В следующей таблице показаны содержимого обозначения кодировки, поддерживаемые по промежуточного слоя.

| `Accept-Encoding`значения заголовка | Поддерживается по промежуточного слоя | Описание:                                                 |
| :-----------------------------: | :------------------: | ----------------------------------------------------------- |
| `br`                            | Нет                   | Формат Brotli сжатых данных                               |
| `compress`                      | Нет                   | Формат данных «сжатие» UNIX                                 |
| `deflate`                       | Нет                   | сжатые данные в формат данных «zlib» «deflate»     |
| `exi`                           | Нет                   | W3C XML эффективного обмена                               |
| `gzip`                          | Да (по умолчанию)        | формат файла gzip                                            |
| `identity`                      | Да                  | Идентификатор «Без кодировки»: ответ не должны быть закодированы. |
| `pack200-gzip`                  | Нет                   | Формат сети передачи архивов Java                   |
| `*`                             | Да                  | Все доступное содержимое, кодировка не явно запрошенный     |

Дополнительные сведения см. в разделе [IANA официальный кодирования список содержимое](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).

По промежуточного слоя можно добавить дополнительное сжатие поставщиков для пользовательских `Accept-Encoding` значения заголовка. Дополнительные сведения см. в разделе [настраиваемые поставщики](#custom-providers) ниже.

По промежуточного слоя способен отклик на значение качества (qvalue, `q`) взвешивание при отправке клиентом приоритет схемы сжатия. Дополнительные сведения см. в разделе [RFC 7231: приемлемой кодировкой](https://tools.ietf.org/html/rfc7231#section-5.3.4).

Алгоритмы сжатия подвержены компромисса между скоростью сжатие и эффективность сжатия. *Эффективность* в этом контексте ссылается размер выходных данных после сжатия. Минимальный размер достигается за счет наиболее *оптимальной* сжатия.

Заголовки, участвующих в запросе, отправки, кэширование и получения содержимого сжатого описаны в следующей таблице.

| Header             | Роль |
| ------------------ | ---- |
| `Accept-Encoding`  | Отправленные клиентом на сервер для указания содержимого, кодировки схем, допустимых для клиента. |
| `Content-Encoding` | Отправляются с сервера на клиент для указания кодировки содержимого в полезных данных. |
| `Content-Length`   | Когда происходит сжатие, `Content-Length` заголовок удаляется с момента изменения содержимого текста при сжатии ответа. |
| `Content-MD5`      | Когда происходит сжатие, `Content-MD5` заголовок удаляется, поскольку содержимое текста был изменен и хэш-код больше не действительна. |
| `Content-Type`     | Указывает тип MIME содержимого. Каждый ответ должен указать его `Content-Type`. По промежуточного слоя проверяет это значение, чтобы определить, должны ли сжиматься ответа. По промежуточного слоя задает набор [по умолчанию типы MIME](#mime-types) , что позволяет кодировать, но можно заменить или добавить типы MIME. |
| `Vary`             | При отправке сервером со значением `Accept-Encoding` для клиентов и прокси-серверы, `Vary` заголовок указывает клиенту или прокси-сервера, кэшируется (меняться) ответы на основе значения `Accept-Encoding` заголовок запроса. Возврат содержимого с результатом `Vary: Accept-Encoding` заголовок — что оба сжатые и несжатые ответы, кэшируются отдельно. |

Можно ознакомиться с функциями по промежуточного слоя сжатия ответа с [пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples). В образце кода:
* Сжатие ответов приложения с помощью gzip и сжатие пользовательских поставщиков.
* Как добавить тип MIME по умолчанию в список типов MIME для сжатия.

## <a name="package"></a>Пакет
Чтобы включить по промежуточного слоя в проекте, добавьте ссылку на [ `Microsoft.AspNetCore.ResponseCompression` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) пакета или использовать [ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) пакета. Эта функция предназначена для приложений, предназначенных для ASP.NET Core 1.1 или более поздней версии.

## <a name="configuration"></a>Конфигурация
Ниже показано включение по промежуточного слоя сжатия ответа с со сжатием gzip по умолчанию и для типов MIME по умолчанию.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/StartupBasic.cs?name=snippet1&highlight=4,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/StartupBasic.cs?name=snippet1&highlight=3,8)]

---

> [!NOTE]
> Использование таких средств, как [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), или [почтальон](https://www.getpostman.com/) для задания `Accept-Encoding` заголовок запроса и изучения заголовки ответа, размер и текст.

Запрос отправляется в пример приложения без `Accept-Encoding` заголовок и обратите внимание, что ответ без сжатия. `Content-Encoding` И `Vary` заголовки не присутствуют в ответе.

![Fiddler окно, отображающее результат запроса без заголовка Accept-Encoding. Ответ не сжимаются.](response-compression/_static/request-uncompressed.png)

Отправить запрос на пример приложения с `Accept-Encoding: gzip` заголовок и обратите внимание, что ответ сжимается. `Content-Encoding` И `Vary` заголовки присутствуют в ответе.

![Fiddler окно, отображающее результат запроса с заголовком Accept-Encoding и значение gzip. Заголовки Vary и Content-Encoding, добавляются в ответ. Ответ сжимаются.](response-compression/_static/request-compressed.png)

## <a name="providers"></a>Поставщики
### <a name="gzipcompressionprovider"></a>GzipCompressionProvider
Используйте `GzipCompressionProvider` сжатия ответов с помощью gzip. Это поставщик сжатия по умолчанию, если не указан ни один. Сжатие можно установить уровень с `GzipCompressionProviderOptions`. 

По умолчанию используется поставщик сжатие gzip быстрым уровень сжатия (`CompressionLevel.Fastest`), который может не обеспечить наиболее эффективное сжатие. При желании наиболее эффективный сжатия можно настроить по промежуточного слоя для оптимального сжатия.

| Уровень сжатия                | Описание:                                                                                                   |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `CompressionLevel.Fastest`       | Сжатие следует выполнить как можно скорее, даже если результат не сжаты оптимально. |
| `CompressionLevel.NoCompression` | Должно осуществляться без сжатия.                                                                           |
| `CompressionLevel.Optimal`       | Ответы должны оптимально сжиматься, даже если сжатие занимает больше времени для завершения.                |


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=3,8-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,10-13)]

---

## <a name="mime-types"></a>типы MIME
По промежуточного слоя указывает набор типов MIME для сжатия по умолчанию:
* `text/plain`
* `text/css`
* `application/javascript`
* `text/html`
* `application/xml`
* `text/xml`
* `application/json`
* `text/json`

Можно заменить или добавить типы MIME с параметрами по промежуточного слоя сжатия ответов. Обратите внимание, что подстановочные MIME типы, такие как `text/*` не поддерживаются. Пример приложения добавляет тип MIME для `image/svg+xml` и сжимает и служит ASP.NET Core изображение баннера (*banner.svg*).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7)]

---

### <a name="custom-providers"></a>Настраиваемые поставщики
Можно создать реализаций сжатия с `ICompressionProvider`. `EncodingName` Представляет содержимое, кодировка этим `ICompressionProvider` выводятся. По промежуточного слоя использует эти сведения можно выбрать поставщика, на основе списка, указанный в `Accept-Encoding` заголовок запроса.

Используя пример приложения, клиент отправляет запрос с `Accept-Encoding: mycustomcompression` заголовок. По промежуточного слоя использует реализацию пользовательского сжатия и возвращает ответ с `Content-Encoding: mycustomcompression` заголовок. Клиент должен иметь возможность распаковки пользовательских кодировок в порядке для реализации пользовательских сжатия для работы.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=4)]

[!code-csharp[Main](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6)]

[!code-csharp[Main](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

---

Отправить запрос на пример приложения с `Accept-Encoding: mycustomcompression` заголовок и просмотрите заголовки ответа. `Vary` И `Content-Encoding` заголовки присутствуют в ответе. Текст ответа (не показано) не сжаты в образце. Отсутствует реализация сжатия в `CustomCompressionProvider` класс образца. Тем не менее в образце показано, где следует реализовать алгоритм сжатия.

![Fiddler окно, отображающее результат запроса с заголовком Accept-Encoding и значение mycustomcompression. Заголовки Vary и Content-Encoding, добавляются в ответ.](response-compression/_static/request-custom-compression.png)

## <a name="compression-with-secure-protocol"></a>Сжатие с безопасным протоколом
Сжатые ответы по безопасным соединениям можно управлять при помощи `EnableForHttps` параметр, который по умолчанию отключена. С помощью сжатия с динамической страницы может привести к проблемам безопасности таких как [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) и [нарушения](https://wikipedia.org/wiki/BREACH_(security_exploit)) атак.

## <a name="adding-the-vary-header"></a>Добавление заголовка Vary
При сжатии ответы на основе `Accept-Encoding` заголовок, потенциально несколько версий сжатого ответа и несжатую версию. Чтобы настроить кэш клиента и прокси-сервера, существует несколько версий и должны быть сохранены, `Vary` добавить заголовок с `Accept-Encoding` значение. В ASP.NET Core 1.x, добавление `Vary` заголовок в ответ выполняется вручную. В ASP.NET Core добавляет по промежуточного слоя 2.x `Vary` заголовка автоматически при ответе сжимается.

**ASP.NET Core только 1.x**

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet1)]

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a>Проблемы по промежуточного слоя за Nginx обратного прокси-сервера
При получении запроса через прокси, Nginx, `Accept-Encoding` заголовок удаляется. Это предотвращает сжатие ответ по промежуточного слоя. Дополнительные сведения см. в разделе [NGINX: сжатия и распаковки](https://www.nginx.com/resources/admin-guide/compression-and-decompression/). Эта проблема отслеживается [выяснить сквозной сжатия для Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).

## <a name="working-with-iis-dynamic-compression"></a>Работа с динамическое сжатие IIS
При наличии активных динамического сжатия модуль IIS настроен на уровне сервера, который вы хотите отключить для приложения, это можно сделать с дополнением к вашей *web.config* файла. Дополнительные сведения см. в разделе [модули IIS Отключение](xref:host-and-deploy/iis/modules#disabling-iis-modules).

## <a name="troubleshooting"></a>Устранение неполадок
Использование таких средств, как [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), или [почтальон](https://www.getpostman.com/), позволяют задавать `Accept-Encoding` заголовок запроса и изучения заголовки ответа, размер и текст. По промежуточного слоя сжатия ответа сжимает ответов, которые удовлетворяют следующим условиям:
* `Accept-Encoding` Присутствует заголовок со значением `gzip`, `*`, или настраиваемую кодировку, соответствующий поставщик сжатия, который вы определили. Значение не должно быть `identity` или иметь значение качества (qvalue, `q`) значение 0 (ноль).
* Тип MIME (`Content-Type`) должны быть заданы, должны совпадать с типом MIME, настроенный на `ResponseCompressionOptions`.
* Запрос не должен включать `Content-Range` заголовок.
* Запрос должен использовать незащищенный протокол (http), если безопасный протокол (https) не настроен в параметрах ответа сжатия по промежуточного слоя. *Обратите внимание, опасности [описанных выше](#compression-with-secure-protocol) при включении сжатия безопасного содержимого.*

## <a name="additional-resources"></a>Дополнительные ресурсы
* [Запуск приложения](xref:fundamentals/startup)
* [ПО промежуточного слоя](xref:fundamentals/middleware)
* [Mozilla Developer Network: Accept-Encoding](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [Раздела RFC 7231 3.1.2.1: Codings содержимого](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [RFC 7230 разделе 4.2.3: Кодирование Gzip](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [Версия спецификации формата файла GZIP 4.3](http://www.ietf.org/rfc/rfc1952.txt)
