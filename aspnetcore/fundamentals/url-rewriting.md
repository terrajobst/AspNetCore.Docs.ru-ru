---
title: "По промежуточного слоя в ASP.NET Core видоизменения URL-адресов"
author: guardrex
description: "Дополнительные сведения о URL-адрес, перезаписи и перенаправления с по промежуточного слоя перезаписи URL-адрес в приложениях ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 08/17/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/url-rewriting
ms.openlocfilehash: 99f8d1cc73fdcbd99cffe595ae89f3c61a6f9a53
ms.sourcegitcommit: 3d512ea991ac36dfd4c800b7d1f8a27bfc50635e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2018
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a>По промежуточного слоя в ASP.NET Core видоизменения URL-адресов

По [Latham Люк](https://github.com/guardrex) и [Mengistu Микаэль](https://github.com/mikaelm12)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

Перезапись URL-адрес является изменение запроса URL-адреса на основании одного или нескольких стандартных правил. Переписывание URL-адресов создает абстракции между расположений ресурсов и их адресов, позволяя местоположения и адреса не связаны тесно. Существует несколько сценариев, где полезен перезаписи URL-адресов:
* Перемещение или замещение ресурсы сервера временно или навсегда при сохранении стабильный указатели для этих ресурсов
* Разделение запросов обработки в различных приложениях или в области одного приложения
* Удаление, добавление или реорганизации сегменты URL-адреса входящих запросов
* Оптимизация общедоступные URL-адреса для оптимизации поиска подсистемы (SEO)
* Разрешить использование общих поисковых помогает пользователям прогнозирования содержимое, которое они найдут по ссылке
* Перенаправление небезопасного запросов для защиты конечных точек
* Предотвращение hotlinking изображения

Можно определить правила для изменения URL-адрес несколькими способами, включая регулярное выражение, модуль правил Apache mod_rewrite, IIS модуль переопределения правил и с помощью настраиваемого правила логики. В этом документе представлены перезаписи URL-адресов с инструкциями о способах использования по промежуточного слоя перезаписи URL-адрес в приложениях ASP.NET Core.

> [!NOTE]
> Переписывание URL-адресов может привести к снижению производительности приложения. Если возможно, следует ограничить количество и сложность правил.

## <a name="url-redirect-and-url-rewrite"></a>Перепишите перенаправления URL-адреса и URL-адрес
Разница в текст между *URL-адрес перенаправления* и *перезапись URL-адреса* может показаться слабая на первый, но содержится важные последствия для предоставления ресурсов клиентам. По промежуточного слоя ASP.NET Core URL-адрес перезаписи способен удовлетворяет потребность в обоих.

Объект *URL-адрес перенаправления* является операции на стороне клиента, где указано клиента для доступа к ресурсу по другому адресу. Это требует дополнительного обращения к серверу. URL-адрес перенаправления, возвращаемый клиенту появляется в адресной строке браузера, когда клиент отправляет новый запрос для ресурса. 

Если `/resource` — *перенаправлено* для `/different-resource`, клиент запрашивает `/resource`. Сервер отвечает, что клиент должен получить ресурс по адресу `/different-resource` код состояния, указывающую, что перенаправления быть временным или постоянным. Клиент выполняет новый запрос для ресурса на URL-адрес перенаправления.

![Конечная точка службы WebAPI временно изменен с версии 1 (v1) до версии 2 (v2) на сервере. Клиент делает запрос к службе в /v1/api путь к версии 1. Сервер отправляет ответ 302 (найдено) на новый, временный путь для службы в /v2/api версии 2. Клиент отправляет второй запрос к службе в URL-адрес перенаправления. Сервер отвечает с кодом состояния 200 (ОК).](url-rewriting/_static/url_redirect.png)

Когда перенаправления запросов на другой URL-адрес, можно указать ли перенаправления постоянных и временных. 301 (перемещен постоянно) код состояния позволяет где ресурс содержит URL-адрес нового, постоянный и хотите указать клиента, что все последующие запросы для ресурса следует использовать новый URL-адрес. *Клиент может кэшировать ответ при получении код 301 состояния.* Код состояния 302 (найдено) используется где перенаправление временные или обычно субъекта для изменения таким образом, что клиент не должен хранить и повторно использовать URL-адрес перенаправления, в будущем. Дополнительные сведения см. в разделе [RFC 2616: определения кодов состояния](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

Объект *перезапись URL-адреса* является операции на стороне сервера для предоставления ресурса, с адресом другого ресурса. Перезапись URL-адрес не требует дополнительного обращения к серверу. URL-адрес перезаписанного не возвращаются клиенту и не будет отображаться в адресной строке браузера. При `/resource` — *переписать* для `/different-resource`, клиентские запросы `/resource`и сервер *внутренне* получает ресурс на `/different-resource`. Несмотря на то, что клиент может иметь возможность получить ресурс по перезаписанного URL-АДРЕСУ, клиент не быть в курсе, что ресурс существует перезаписанного URL-адрес, если она его запрашивает и получает ответ.

![Конечная точка службы WebAPI был изменен с версии 1 (v1) до версии 2 (v2) на сервере. Клиент делает запрос к службе в /v1/api путь к версии 1. URL-адрес запроса переписать, чтобы получить доступ к службе в /v2/api путь версии 2. Служба отвечает клиенту с кодом состояния 200 (ОК).](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>Пример приложения URL-адрес для перезаписи
Можно ознакомиться с функциями URL-адрес перезаписи по промежуточного слоя с [пример приложения URL-адрес для перезаписи](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/). Перепишите применяется приложение и перенаправления правила и показывает перезаписанного или перенаправленный URL-адрес.

## <a name="when-to-use-url-rewriting-middleware"></a>Когда следует использовать URL-адрес перезаписи по промежуточного слоя
По промежуточного слоя перезаписи URL-адрес используется, если не удается использовать [модуль переопределения URL-адрес](https://www.iis.net/downloads/microsoft/url-rewrite) со службами IIS в Windows Server [модуля Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/) на сервере Apache [перезаписи URL-адресов на Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), или приложения, размещенного на [HTTP.sys сервера](xref:fundamentals/servers/httpsys) (ранее называвшиеся [WebListener](xref:fundamentals/servers/weblistener)). Основных причин для использования серверных URL-адрес перезаписи технологии в IIS, Apache или Nginx, что по промежуточного слоя не поддерживает все возможности этих модулей и производительности по промежуточного слоя, вероятно, могут не соответствовать, модулей. Однако существуют некоторые возможности серверных модулей, которые не работают с проекты ASP.NET Core, таких как `IsFile` и `IsDirectory` ограничения модуль переопределения IIS. В этих сценариях вместо этого используйте по промежуточного слоя.

## <a name="package"></a>Пакет
Чтобы включить по промежуточного слоя в проекте, добавьте ссылку на [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) пакета. Эта функция предназначена для приложений, предназначенных для ASP.NET Core 1.1 или более поздней версии.

## <a name="extension-and-options"></a>Расширение и параметры
Установить на перезапись URL-адреса и перенаправить правила, создав экземпляр `RewriteOptions` класса с помощью методов расширения для каждого правила. Привязать несколько правил в порядке, что вы хотите их обработать. `RewriteOptions` Передаются в по промежуточного слоя перезаписи URL-адрес, как оно добавляется в конвейер обработки запросов с `app.UseRewriter(options);`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1)]

---

### <a name="url-redirect"></a>URL-адрес перенаправления
Используйте `AddRedirect` для перенаправления запросов. Первый параметр содержит регулярное выражение для сопоставления по пути входящего URL-адреса. Второй параметр является строка замены. Третий параметр, если он присутствует, указывает код состояния. Если не указать код состояния, по умолчанию 302 (найдено), который указывает, что ресурс временно переместить или заменить.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=2)]

---

В обозревателе со средствами разработчика включена, сделать запрос в пример приложения с путем `/redirect-rule/1234/5678`. Регулярное выражение соответствует пути запроса на `redirect-rule/(.*)`, и путь должен быть заменен параметром `/redirected/1234/5678`. Перенаправление URL-адрес отправляется обратно клиенту с кодом состояния 302 (найдено). Браузер отправляет новый запрос на URL-адрес перенаправления, который отображается в адресной строке браузера. Поскольку соответствие не найдено в пример приложения на URL-адрес перенаправления, второй запрос получает ответ 200 (ОК) из приложения и текст ответа показан URL-адрес перенаправления. Обмен данными сделаны на сервере, когда требуется URL-адрес *перенаправлено*.

> [!WARNING]
> Будьте осторожны при установке правила перенаправления. Правила перенаправления вычисляются при каждом запросе данного приложения, включая после перенаправления. Можно легко случайно создать перенаправлений бесконечный цикл.

Исходный запрос:`/redirect-rule/1234/5678`

![Окно браузера со средствами разработчика отслеживания запросов и ответов](url-rewriting/_static/add_redirect.png)

Часть выражения в скобках называется *группа захвата*. Точка (`.`) выражения означает *соответствует любому символу*. Звездочка (`*`) указывает *соответствует предыдущему символу ноль или более раз*. Таким образом, два последних сегментов URL-адреса, `1234/5678`, захваченные по группа захвата `(.*)`. Любое значение, указываемые в URL-АДРЕСЕ запроса после `redirect-rule/` охваченного этой группы один символ.

Захватываемые группы в строке замены, вводится в строку с знак доллара (`$`) следуют порядковый номер записи. Первое значение группы захвата получен с `$1`, другой — с `$2`, и они по-прежнему в последовательности для групп захвата в регулярное выражение. Установлен только один захватываемой группы в регулярное выражение правила перенаправления в пример приложения, то есть только одна группа, введенный в строке замены, который является `$1`. Если применяется правило, URL-адрес становится `/redirected/1234/5678`.

<a name="url-redirect-to-secure-endpoint"></a>
### <a name="url-redirect-to-a-secure-endpoint"></a>URL-адрес перенаправления для защищенной конечной точки
Используйте `AddRedirectToHttps` для перенаправления HTTP-запросов на один и тот же узел и путь, с помощью протокола HTTPS (`https://`). Если код состояния не указано, по промежуточного слоя по умолчанию 302 (найдено). Если порт не указан, по умолчанию используется по промежуточного слоя `null`, означающее протокол примет `https://` и клиент обращается к ресурсу через порт 443. В примере показано установить код состояния для 301 (перемещен постоянно) и изменить номер порта 5001.
```csharp
var options = new RewriteOptions()
    .AddRedirectToHttps(301, 5001);

app.UseRewriter(options);
```
Используйте `AddRedirectToHttpsPermanent` для перенаправления небезопасных запросов на один и тот же узел и путь с безопасным протоколом HTTPS (`https://` через порт 443). По промежуточного слоя задает код состояния для 301 (перемещен постоянно).

Пример приложения способен демонстрирующие использование `AddRedirectToHttps` или `AddRedirectToHttpsPermanent`. Добавьте метод расширения `RewriteOptions`. Выполните небезопасных запрос к приложению в любой URL-адрес. Закройте предупреждение, что самозаверяющий сертификат не является доверенным безопасности браузера.

С помощью исходного запроса `AddRedirectToHttps(301, 5001)`:`/secure`

![Окно браузера со средствами разработчика отслеживания запросов и ответов](url-rewriting/_static/add_redirect_to_https.png)

С помощью исходного запроса `AddRedirectToHttpsPermanent`:`/secure`

![Окно браузера со средствами разработчика отслеживания запросов и ответов](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>Перезапись URL-адреса
Используйте `AddRewrite` для создания правила для перезаписи URL-адреса. Первый параметр содержит регулярное выражение для сопоставления на входящий URL-адрес. Второй параметр является строка замены. Третий параметр `skipRemainingRules: {true|false}`, указывает по промежуточного слоя ли пропустить правила дополнительных подстановки, если применяется текущего правила.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=6)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=3)]

---

Исходный запрос:`/rewrite-rule/1234/5678`

![Окно браузера со средствами разработчика отслеживания запросов и ответов](url-rewriting/_static/add_rewrite.png)

Первое, что в регулярное выражение — начальный (`^`) в начале выражения. Это означает, что сопоставление начинается в начале URL-адрес.

В предыдущем примере с помощью правила перенаправления `redirect-rule/(.*)`, отсутствует в начале регулярных выражений не начальный; таким образом, может предшествовать символы `redirect-rule/` в пути, для успешного сопоставления.

| Путь                               | Соответствие |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | Да   |
| `/my-cool-redirect-rule/1234/5678` | Да   |
| `/anotherredirect-rule/1234/5678`  | Да   |

Правило подстановки, `^rewrite-rule/(\d+)/(\d+)`, соответствует пути, только если они начинаются с `rewrite-rule/`. Обратите внимание на разницу в сопоставлении ниже правила подстановки и выше правила перенаправления.

| Путь                              | Соответствие |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | Да   |
| `/my-cool-rewrite-rule/1234/5678` | Нет    |
| `/anotherrewrite-rule/1234/5678`  | Нет    |

После `^rewrite-rule/` часть выражения, имеются две группы захвата, `(\d+)/(\d+)`. `\d` Означает *сопоставления цифр (number)*. Знак «плюс» (`+`) означает *сопоставление одного или нескольких предшествующий символ*. Таким образом URL-адрес должен содержать числом следуют прямой косой черты следуют другой номер. Эти записи группы вводится в перезаписанного URL-адрес как `$1` и `$2`. Строка замены правило перезаписи помещает записанной группы в строке запроса. Запрошенный путь `/rewrite-rule/1234/5678` переписать так, чтобы получить ресурс по адресу `/rewritten?var1=1234&var2=5678`. Если строку запроса присутствует в исходном запросе, он сохраняется при переписать URL-адрес.

Без обращения к серверу, чтобы получить ресурс не существует. Если ресурс существует, она имеет выборке и возвращаются клиенту с кодом состояния 200 (ОК). Поскольку клиент не перенаправлен, URL-адрес в адресной строке браузера не изменяется. Как клиент отвечает, произошло никогда не операции перезаписи URL-адрес.

> [!NOTE]
> Используйте `skipRemainingRules: true` по возможности, поскольку правила сопоставления ресурсоемким процессом и сократить время отклика приложений. Самый быстрый ответ приложения:
> * Порядок правила подстановки из наиболее часто соответствующие правила реже всего соответствующие правила.
> * Пропустите обработку остальных правил при совпадении и обработка дополнительное правило не является обязательным.

### <a name="apache-modrewrite"></a>Apache mod_rewrite
Применение правил Apache mod_rewrite с `AddApacheModRewrite`. Убедитесь, что файл правил развертывается с приложением. Дополнительные сведения и примеры правил mod_rewrite см. в разделе [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Объект `StreamReader` используется для чтения правил из *ApacheModRewrite.txt* файлу правил.

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=1,7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Первый параметр принимает `IFileProvider`, которая обеспечивается за счет [внедрения зависимостей](dependency-injection.md). `IHostingEnvironment` Встраивается для предоставления `ContentRootFileProvider`. Второй параметр — путь к файлу правил, являющийся *ApacheModRewrite.txt* в пример приложения.

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=4)]

---

Пример приложения перенаправляет запросы с `/apache-mod-rules-redirect/(.\*)` для `/redirected?id=$1`. Код состояния ответа: 302 (найдено).

[!code[Main](url-rewriting/samples/2.x/ApacheModRewrite.txt)]

Исходный запрос:`/apache-mod-rules-redirect/1234`

![Окно браузера со средствами разработчика отслеживания запросов и ответов](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a>Поддерживаемые серверные переменные
По промежуточного слоя поддерживает следующие переменные сервера Apache mod_rewrite:
* CONN_REMOTE_ADDR
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_FORWARDED
* HTTP_HOST
* HTTP_REFERER
* HTTP_USER_AGENT
* HTTPS
* IPV6
* QUERY_STRING
* REMOTE_ADDR
* REMOTE_PORT
* REQUEST_FILENAME
* REQUEST_METHOD
* REQUEST_SCHEME
* REQUEST_URI
* SCRIPT_FILENAME
* SERVER_ADDR
* SERVER_PORT
* SERVER_PROTOCOL
* ВРЕМЯ
* TIME_DAY
* TIME_HOUR
* TIME_MIN
* TIME_MON
* TIME_SEC
* TIME_WDAY
* TIME_YEAR

### <a name="iis-url-rewrite-module-rules"></a>Правила модуль переопределения URL-адреса IIS
Чтобы использовать правила, применяемые к модуль переопределения URL-адреса IIS, используйте `AddIISUrlRewrite`. Убедитесь, что файл правил развертывается с приложением. Не направлять по промежуточного слоя для использования вашей *web.config* файлов при работе в Windows Server IIS. В службах IIS, эти правила должны храниться вне вашего *web.config* для предотвращения конфликтов с модуль переопределения IIS. Дополнительные сведения и примеры правил модуль переопределения URL-адрес служб IIS см. в разделе [2.0 с помощью модуля перепишите URL-адрес](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) и [URL-адрес ссылки перепишите конфигурации модуль](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Объект `StreamReader` используется для чтения правил из *IISUrlRewrite.xml* файлу правил.

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=2,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Первый параметр принимает `IFileProvider`, а второй параметр — это путь к XML-файл правил, который является *IISUrlRewrite.xml* в пример приложения.

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=5)]

---

Пример приложения переписывает запросы от `/iis-rules-rewrite/(.*)` для `/rewritten?id=$1`. Ответ отправляется клиенту с кодом состояния 200 (ОК).

[!code-xml[Main](url-rewriting/samples/2.x/IISUrlRewrite.xml)]

Исходный запрос:`/iis-rules-rewrite/1234`

![Окно браузера со средствами разработчика отслеживания запросов и ответов](url-rewriting/_static/add_iis_url_rewrite.png)

Если имеется активный модуль IIS переписать с настроены правила брандмауэра уровня сервера, которые повлияло бы на ваше приложение нежелательных способами, можно отключить модуль переопределения IIS для приложения. Дополнительные сведения см. в разделе [модули IIS Отключение](xref:host-and-deploy/iis/modules#disabling-iis-modules).

#### <a name="unsupported-features"></a>Неподдерживаемые функции

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

По промежуточного слоя выпущен с ASP.NET Core 2.x не поддерживает следующие функции модуль переопределения URL-адреса IIS:
* Правила для исходящих подключений
* Переменные пользовательского сервера
* Знаки подстановки
* LogRewrittenUrl

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

По промежуточного слоя выпущен с ASP.NET Core 1.x не поддерживает следующие функции модуль переопределения URL-адреса IIS:
* Глобальные правила
* Правила для исходящих подключений
* Перепишите карты
* Действие CustomResponse
* Переменные пользовательского сервера
* Знаки подстановки
* Действие: CustomResponse
* LogRewrittenUrl

---

#### <a name="supported-server-variables"></a>Поддерживаемые серверные переменные
По промежуточного слоя поддерживает следующие переменные сервера модуль переопределения URL-адреса IIS.
* CONTENT_LENGTH
* УКАЗЫВАЕТСЯ
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_HOST
* HTTP_REFERER
* HTTP_URL
* HTTP_USER_AGENT
* HTTPS
* LOCAL_ADDR
* QUERY_STRING
* REMOTE_ADDR
* REMOTE_PORT
* REQUEST_FILENAME
* REQUEST_URI

> [!NOTE]
> Можно также получить `IFileProvider` через `PhysicalFileProvider`. Этот подход может обеспечивают большую гибкость для расположения вашей перезаписи файлов правил. Убедитесь, что правила перезаписи файлов развертываются в указании пути к серверу.
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>Правила на основе метода
Используйте `Add(Action<RewriteContext> applyRule)` реализовать собственную логику правила в методе. `RewriteContext` Предоставляет `HttpContext` для использования в методе. `context.Result` Определяет, как увеличение конвейера осуществляется обработка.

| контекст. Результат                       | Действие                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| `RuleResult.ContinueRules` (по умолчанию) | Продолжить применение правил                                         |
| `RuleResult.EndResponse`             | Остановить применение правил и отправки ответа                       |
| `RuleResult.SkipRemainingRules`      | Остановить применение правил и отправить контекст следующее по промежуточного слоя |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=6)]

---

Пример приложения показан метод, который перенаправляет запросы для путей, заканчиваться *.xml*. Если сделать запрос `/file.xml`, оно перенаправляется в `/xmlfiles/file.xml`. Код состояния равно 301 (перемещен постоянно). Для перенаправления необходимо явно задать код состояния ответа; в противном случае возвращается код состояния 200 (ОК) и перенаправление не будет выполняться на клиенте.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet2)]

---

Исходный запрос:`/file.xml`

![Окно браузера со средствами разработчика отслеживания запросов и ответов для file.xml](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a>Правила на основе IRule
Используйте `Add(IRule)` реализовать собственную логику правила в класс, производный от `IRule`. С помощью `IRule` обеспечивает большую гибкость по сравнению с использованием подхода правила на основе методов. Производный класс может включать конструктор, где можно передать в параметры для `ApplyRule` метод.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=7-8)]

---

Значения параметров в пример приложения для `extension` и `newPath` проверяются, чтобы удовлетворять нескольким условиям. `extension` Должен содержать значение, а значение должно быть *.png*, *.jpg*, или *.gif*. Если `newPath` является недопустимой, `ArgumentException` возникает исключение. Если сделать запрос *image.png*, оно перенаправляется в `/png-images/image.png`. Если сделать запрос *image.jpg*, оно перенаправляется в `/jpg-images/image.jpg`. Чтобы 301 (перемещен постоянно), задается код состояния и `context.Result` прекратить обработку правил и отправки ответа.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/RewriteRule.cs?name=snippet1)]

---

Исходный запрос:`/image.png`

![Окно браузера со средствами разработчика отслеживания запросов и ответов для image.png](url-rewriting/_static/add_redirect_png_requests.png)

Исходный запрос:`/image.jpg`

![Окно браузера со средствами разработчика отслеживания запросов и ответов для image.jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>Примеры регулярных выражений

| Goal | Строка Regex &<br>Пример соответствия | Строка замены &<br>Пример выходных данных |
| ---- | :-----------------------------: | :------------------------------------: |
| Путь перезаписи в строки запроса | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| Полоса косой чертой | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| Применять косой чертой | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| Избежать перезаписи определенные запросы | `^(.*)(?<!\.axd)$` или `^(?!.*\.axd$)(.*)$`<br>Да:`/resource.htm`<br>Нет:`/resource.axd` | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| Изменение порядка сегменты URL-адреса | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| Замените сегмент URL-адреса | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a>Дополнительные ресурсы
* [Запуск приложения](startup.md)
* [ПО промежуточного слоя](middleware.md)
* [Регулярные выражения в .NET](/dotnet/articles/standard/base-types/regular-expressions)
* [Элементы языка регулярных выражений — краткий справочник](/dotnet/articles/standard/base-types/quick-ref)
* [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)
* [Используя модуль переопределения URL-адрес 2.0 (для IIS)](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [Справочник по конфигурации модуля перезаписи URL-адрес](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [Перепишите модуля IIS URL-адрес форума](https://forums.iis.net/1152.aspx)
* [Сохранять простую структуру с URL-адрес](https://support.google.com/webmasters/answer/76329?hl=en)
* [10 перезаписи URL-адресов, советы и рекомендации](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [Косая черта или не косая черта](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
