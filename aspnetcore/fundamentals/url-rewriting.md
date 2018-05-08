---
title: ПО промежуточного слоя для переопределения URL-адресов в ASP.NET Core
author: guardrex
description: Сведения о переопределении и перенаправлении URL-адресов с помощью ПО промежуточного слоя для переопределения URL-адресов в приложениях ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/url-rewriting
ms.openlocfilehash: b6465aa7b56450f43be64da19f2e2228a5d68f50
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a>ПО промежуточного слоя для переопределения URL-адресов в ASP.NET Core

Авторы: [Люк Лэтем](https://github.com/guardrex) (Luke Latham) и [Микаэль Менгисту](https://github.com/mikaelm12) (Mikael Mengistu)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

Переопределение URL-адресов представляет собой изменение URL-адресов запросов на основе одного или нескольких предопределенных правил. Переопределение URL-адресов создает абстракцию между расположениями ресурсов и их адресами, позволяя убрать тесную связь между ними. Существует несколько сценариев, где может пригодиться переопределение URL-адресов:
* Временное или постоянное перемещение или замещение ресурсов сервера с сохранением стабильных указателей для этих ресурсов.
* Разделение обработки запросов между разными приложениями или разными областями одного приложения.
* Удаление, добавление или переупорядочение сегментов URL-адресов для входящих запросов.
* Оптимизация общедоступных URL-адресов в целях оптимизации для поисковых систем (SEO).
* Разрешение использования понятных общедоступных URL-адресов, чтобы пользователи могли прогнозировать, какое именно содержимое они найдут, перейдя по ссылке.
* Перенаправление небезопасных запросов на защищенные конечные точки.
* Предотвращение использования активных ссылок на изображения.

Определить правила для изменения URL-адреса можно несколькими способами, включая регулярное выражение, правила модуля mod_rewrite Apache, правила модуля переопределения для IIS и логику настраиваемых правил. Этот документ содержит вводные сведения о переопределении URL-адресов и инструкции по использованию ПО промежуточного слоя для переопределения URL-адресов в приложениях ASP.NET Core.

> [!NOTE]
> Переопределение URL-адресов может снижать производительность приложения. Следует по возможности ограничить число и сложность правил.

## <a name="url-redirect-and-url-rewrite"></a>Перенаправление и переопределение URL-адресов
На первый взгляд разница между *перенаправлением* и *переопределением* URL-адресов может показаться незначительной, но она оказывает значительное влияние на предоставление ресурсов клиентам. ПО промежуточного слоя для переопределения URL-адресов в ASP.NET Core удовлетворяет потребность в обеих этих функциях.

*Перенаправление URL-адресов* является операцией на стороне клиента, которая предписывает клиенту обратиться к ресурсу по другому адресу. Для этого используется круговое обращение к серверу. URL-адрес перенаправления, возвращаемый клиенту, отображается в адресной строке браузера, когда клиент отправляет новый запрос для данного ресурса. 

Если `/resource` *перенаправляется* к `/different-resource`, клиент запрашивает `/resource`. Сервер отвечает, что клиенту следует получить ресурс в `/different-resource` с кодом состояния, указывающим, что такое перенаправление является временным или постоянным. Клиент выполняет новый запрос для ресурса по URL-адресу перенаправления.

![Конечная точка службы WebAPI временно изменена с версии 1 (v1) на версию 2 (v2) на сервере. Клиент выполняет запрос к службе по пути версии 1 (/v1/api). Сервер отправляет отклик "302 (найдено)" с новым временным путем для службы в версии 2 (/v2/api). Клиент выполняет второй запрос к службе по URL-адресу перенаправления. Сервер отвечает кодом состояния "200 (ОК)".](url-rewriting/_static/url_redirect.png)

При перенаправлении запросов на другой URL-адрес можно указать, является ли оно постоянным или временным. Код состояния "301 (перемещен окончательно)" используется, когда ресурс имеет новый постоянный URL-адрес и вы хотите сообщить клиенту, что все последующие запросы для этого ресурса должны использовать новый URL-адрес. *Клиент может кэшировать отклик при получении кода состояния 301.* Код состояния "302 (найдено)" используется, когда перенаправление является временным или обычно подвержено изменениям, из-за чего клиенту не требуется хранить и повторно использовать этот URL-адрес перенаправления в будущем. Дополнительные сведения см. в документе [RFC 2616: определения кодов состояния](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

*Переопределение URL-адреса* является операцией на стороне сервера для предоставления ресурса с другого адреса ресурса. Переопределение URL-адреса не требует кругового обращения к серверу. Переопределенный URL-адрес не возвращается клиенту и не отображается в адресной строке браузера. Когда `/resource` *переопределяется* на `/different-resource`, клиент запрашивает `/resource`, а сервер *внутренними средствами* получает этот ресурс в `/different-resource`. Хотя клиент может быть в состоянии получить ресурс по переопределенному URL-адресу, когда клиент отправляет запрос и получает отклик, он не уведомляется о наличии ресурса по переопределенному URL-адресу.

![Конечная точка службы WebAPI была изменена с версии 1 (v1) на версию 2 (v2) на сервере. Клиент выполняет запрос к службе по пути версии 1 (/v1/api). URL-адрес запроса переопределяется для обращения к службе по пути версии 2 (/v2/api). Служба отвечает клиенту кодом состояния "200 (ОК)".](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>Пример приложения с переопределением URL-адресов
Вы можете ознакомиться с функциями переопределения URL-адресов на [примере приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/). Это приложение применяет правила переопределения и перенаправления и отображает переопределенный или перенаправленный URL-адрес.

## <a name="when-to-use-url-rewriting-middleware"></a>Условия для использования ПО промежуточного слоя для переопределения URL-адресов
ПО промежуточного слоя для переопределения URL-адресов можно использовать, если не удается использовать [модуль переопределения URL-адресов](https://www.iis.net/downloads/microsoft/url-rewrite) для IIS в Windows Server, [модуль mod_rewrite Apache](https://httpd.apache.org/docs/2.4/rewrite/) на сервере Apache, [переопределение URL-адресов в Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/) либо если приложение размещено на [сервере HTTP.sys](xref:fundamentals/servers/httpsys) (предыдущее название [WebListener](xref:fundamentals/servers/weblistener)). Основные причины для использования серверных технологий переопределения URL-адресов в IIS, Apache или Nginx заключаются в том, что ПО промежуточного слоя не поддерживает все функции этих модулей и, скорее всего, не соответствует их уровню производительности. Однако существуют некоторые функции серверных модулей, которые не работают с проектами ASP.NET Core, например ограничения `IsFile` и `IsDirectory` для модуля переопределения IIS. В этих случаях следует использовать ПО промежуточного слоя.

## <a name="package"></a>Пакет
Чтобы включить ПО промежуточного слоя в проект, добавьте ссылку на пакет [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/). Эта функция доступна для приложений, предназначенных для ASP.NET Core 1.1 или более поздней версии.

## <a name="extension-and-options"></a>Расширение и параметры
Задайте правила переопределения и перенаправления URL-адресов, создав экземпляр класса `RewriteOptions` для каждого правила помощью методов расширения. Несколько правил можно объединить в цепочку в том порядке, в котором они должны обрабатываться. `RewriteOptions` передаются в ПО промежуточного слоя для переопределения URL-адресов по мере его добавления в конвейер запросов с помощью `app.UseRewriter(options);`.

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1")
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true)
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")
        .Add(RedirectXMLRequests)
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

* * *
### <a name="url-redirect"></a>Перенаправление URL-адресов
Используйте `AddRedirect` для перенаправления запросов. Первый параметр содержит регулярное выражение, соответствующее пути входящего URL-адреса. Второй параметр является строкой замены. Третий параметр (при его наличии) указывает код состояния. Если код состояния не задан, по умолчанию используется код "302 (найдено)", указывающий, что ресурс временно перемещен или заменен.

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

* * *
В браузере с включенными средствами разработчика выполните запрос для примера приложения по пути `/redirect-rule/1234/5678`. Регулярное выражение соответствует пути запроса в `redirect-rule/(.*)`, и этот путь заменяется на `/redirected/1234/5678`. URL-адрес перенаправления отправляется обратно клиенту с кодом состояния "302 (найдено)". Браузер выполняет новый запрос на URL-адрес перенаправления, отображаемый в адресной строке. Так как ни одно из правил в примере приложения не соответствует URL-адресу перенаправления, второй запрос получает от приложения отклик "200 (ОК)", в тексте которого указан этот URL-адрес перенаправления. При *перенаправлении* URL-адреса используется круговое обращение к серверу.

> [!WARNING]
> Соблюдайте осторожность при задании правил перенаправления. Они оцениваются для каждого запроса, отправляемого в приложение, даже после перенаправления. Можно легко случайно создать бесконечный цикл перенаправлений.

Исходный запрос: `/redirect-rule/1234/5678`

![Окно браузера, в котором средства разработчика отслеживают запросы и отклики](url-rewriting/_static/add_redirect.png)

Часть выражения, заключенная в скобки, называется *группой записи*. Точка (`.`) в выражении означает *совпадение с любым символом*. Звездочка (`*`) означает *совпадение с предыдущим символом ноль или более раз*. Таким образом, два последних сегмента пути URL-адреса, `1234/5678`, перехватываются группой записи `(.*)`. Любое значение, указанное в URL-адресе запроса после `redirect-rule/`, перехватывается этой группой записи.

В строке замены группы записи внедряются с помощью знака доллара (`$`), за которым следует порядковый номер записи. Первое значение группы записи получается с помощью `$1`, второе — с помощью `$2`, после чего они продолжают по очереди использоваться для групп записи в регулярном выражении. В регулярном выражении правила перенаправления в примере приложения присутствует всего одна группа записи, поэтому в строку замены внедряется тоже одна строка — `$1`. Когда применяется правило, URL-адрес становится `/redirected/1234/5678`.

<a name="url-redirect-to-secure-endpoint"></a>
### <a name="url-redirect-to-a-secure-endpoint"></a>Перенаправление URL-адресов на защищенную конечную точку
Используйте `AddRedirectToHttps`, чтобы перенаправлять HTTP-запросы на тот же узел и путь с использованием протокола HTTPS (`https://`). Если код состояния не указан, ПО промежуточного слоя по умолчанию использует значение "302 (найдено)". Если порт не указан, ПО промежуточного слоя по умолчанию использует значение `null`, в результате чего протокол изменяется на `https://`, а клиент обращается к ресурсу через порт 443. Этот пример показывает, как задать код состояния "301 (перемещен окончательно)" и изменить порт на 5001.

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

Используйте `AddRedirectToHttpsPermanent`, чтобы перенаправить небезопасные запросы на тот же узел и путь с использованием безопасного протокола HTTPS (`https://` через порт 443). ПО промежуточного слоя задает код состояния "301 (перемещен окончательно)".

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

Пример приложения позволяет продемонстрировать использование `AddRedirectToHttps` или `AddRedirectToHttpsPermanent`. Добавьте этот метод расширения в `RewriteOptions`. Выполните небезопасный запрос к приложению по любому URL-адресу. Закройте предостережение системы безопасности о том, что самозаверяющий сертификат не является доверенным.

Исходный запрос с использованием `AddRedirectToHttps(301, 5001)`: `/secure`

![Окно браузера, в котором средства разработчика отслеживают запросы и отклики](url-rewriting/_static/add_redirect_to_https.png)

Исходный запрос с использованием `AddRedirectToHttpsPermanent`: `/secure`

![Окно браузера, в котором средства разработчика отслеживают запросы и отклики](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>Переопределение URL-адресов
Используйте `AddRewrite`, чтобы создать правило для переопределения URL-адресов. Первый параметр содержит регулярное выражение, соответствующее пути входящего URL-адреса. Второй параметр является строкой замены. Третий параметр `skipRemainingRules: {true|false}` сообщает ПО промежуточного слоя, нужно ли пропустить дополнительные правила переопределения, если применяется текущее правило.

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

* * *
Исходный запрос: `/rewrite-rule/1234/5678`

![Окно браузера, в котором средства разработчика отслеживают запрос и отклик](url-rewriting/_static/add_rewrite.png)

Наиболее заметным символом в регулярном выражении является крышка (`^`) в его начале. Это означает, что сопоставление начинается с начала URL-адреса.

В приведенном до этого примере с правилом перенаправления `redirect-rule/(.*)` крышка в начале регулярного выражения отсутствует, поэтому для удачного совпадения перед `redirect-rule/` в пути могут стоять любые символы.

| Путь                               | Соответствие |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | Да   |
| `/my-cool-redirect-rule/1234/5678` | Да   |
| `/anotherredirect-rule/1234/5678`  | Да   |

Правило переопределения `^rewrite-rule/(\d+)/(\d+)` соответствует путям, только если они начинаются с `rewrite-rule/`. Обратите внимание на разницу в сопоставлении для приведенного ниже правила переопределения и приведенного выше правила перенаправления.

| Путь                              | Соответствие |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | Да   |
| `/my-cool-rewrite-rule/1234/5678` | Нет    |
| `/anotherrewrite-rule/1234/5678`  | Нет    |

После части `^rewrite-rule/` выражения стоят две группы записи `(\d+)/(\d+)`. `\d` означает *соответствие цифре (числу)*. Знак "плюс" (`+`) означает *соответствие одному или нескольким предшествующим символам*. Таким образом, URL-адрес должен содержать число, за которым идет прямая косая черта, за которой идет другое число. Эти группы записи внедряются в переопределенный URL-адрес как `$1` и `$2`. Строка замены для правила переопределения помещает группы записи в строку запроса. Запрошенный путь `/rewrite-rule/1234/5678` переопределяется, чтобы ресурс можно было получить по адресу `/rewritten?var1=1234&var2=5678`. Если в исходном запросе присутствует строка запроса, она сохраняется при переопределении URL-адреса.

Круговое обращение к серверу для получения ресурса не выполняется. Если ресурс существует, он извлекается и возвращается клиенту с кодом состояния "200 (ОК)". Так как клиент не перенаправляется, URL-адрес в адресной строке браузера не изменяется. Пока осуществляется работа с клиентом, операция переопределения URL-адреса не выполняется.

> [!NOTE]
> Использовать `skipRemainingRules: true` невозможно, так как правила сопоставления потребляют много ресурсов и ухудшают время отклика приложения. Чтобы максимально ускорить отклик приложения:
> * расположите правила переопределения в порядке от наиболее к наименее часто используемому;
> * пропускайте обработку оставшихся правил, когда совпадение найдено и обработка дополнительных правил не требуется.

### <a name="apache-modrewrite"></a>Apache mod_rewrite
Для применения правил mod_rewrite Apache можно использовать `AddApacheModRewrite`. Убедитесь, что файл правил развертывается вместе с приложением. Дополнительные сведения и примеры правил mod_rewrite см. в статье о [правилах mod_rewrite Apache](https://httpd.apache.org/docs/2.4/rewrite/).

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
`StreamReader` используется для чтения правил из файла *ApacheModRewrite.txt*.

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
Первый параметр принимает `IFileProvider`, предоставляемый посредством [внедрения зависимостей](dependency-injection.md). `IHostingEnvironment` внедряется для предоставления `ContentRootFileProvider`. Второй параметр является путем к файлу правил (в данном примере приложения это *ApacheModRewrite.txt*).

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

* * *
Пример приложения перенаправляет запросы из `/apache-mod-rules-redirect/(.\*)` в `/redirected?id=$1`. Отклик имеет код состояния "302 (найдено)".

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

Исходный запрос: `/apache-mod-rules-redirect/1234`

![Окно браузера, в котором средства разработчика отслеживают запросы и отклики](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a>Поддерживаемые переменные сервера
ПО промежуточного слоя поддерживает следующие переменные сервера в mod_rewrite Apache:
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
* TIME
* TIME_DAY
* TIME_HOUR
* TIME_MIN
* TIME_MON
* TIME_SEC
* TIME_WDAY
* TIME_YEAR

### <a name="iis-url-rewrite-module-rules"></a>Правила модуля переопределения URL-адресов для IIS
Чтобы использовать правила, применяемые к модулю переопределения URL-адресов для IIS, используйте `AddIISUrlRewrite`. Убедитесь, что файл правил развертывается вместе с приложением. Не направляйте ПО промежуточного слоя для использования вашего файла *web.config* при работе в службах IIS Windows Server. В IIS эти правила нужно хранить за пределами *web.config*, чтобы предотвратить конфликты с модулем переопределения для IIS. Дополнительные сведения и примеры правил модуля переопределения URL-адресов для IIS см. в разделах [Использование модуля переопределения URL-адресов 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) и [Справочник по конфигурации модуля переопределения URL-адресов](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
`StreamReader` используется для чтения правил из файла *IISUrlRewrite.xml*.

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
Первый параметр принимает `IFileProvider`, а второй является путем к файлу правил XML (в данном примере приложения это *IISUrlRewrite.xml*).

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

* * *
Пример приложения переопределяет запросы с `/iis-rules-rewrite/(.*)` на `/rewritten?id=$1`. Клиенту отправляется отклик с кодом состояния "200 (ОК)".

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

Исходный запрос: `/iis-rules-rewrite/1234`

![Окно браузера, в котором средства разработчика отслеживают запрос и отклик](url-rewriting/_static/add_iis_url_rewrite.png)

Если имеется активный модуль переопределения для IIS, где настроены правила брандмауэра уровня сервера, способные негативно повлиять на ваше приложение, можно отключить этот модуль для приложения. Дополнительные сведения см. в разделе [Отключение модулей IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).

#### <a name="unsupported-features"></a>Неподдерживаемые функции

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ПО промежуточного слоя, выпущенное вместе с ASP.NET Core 2.x, не поддерживает следующие функции в модуле переопределения URL-адресов для IIS:
* Правила для исходящих подключений
* Пользовательские переменные сервера
* Знаки подстановки
* LogRewrittenUrl

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ПО промежуточного слоя, выпущенное вместе с ASP.NET Core 1.x, не поддерживает следующие функции в модуле переопределения URL-адресов для IIS:
* Глобальные правила
* Правила для исходящих подключений
* Сопоставления переопределения
* Действие CustomResponse
* Пользовательские переменные сервера
* Знаки подстановки
* Action:CustomResponse
* LogRewrittenUrl

---

#### <a name="supported-server-variables"></a>Поддерживаемые переменные сервера
ПО промежуточного слоя поддерживает следующие переменные сервера в модуле переопределения URL-адресов для IIS:
* CONTENT_LENGTH
* CONTENT_TYPE
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
> Можно также получить `IFileProvider` через `PhysicalFileProvider`. Такой подход позволяет более гибко задавать расположение для файлов с правилами переопределения. Убедитесь, что эти файлы развертываются на сервере по указанному пути.
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>Правило, основанное на методе
Используйте `Add(Action<RewriteContext> applyRule)`, чтобы реализовать собственную логику правил в методе. `RewriteContext` предоставляет `HttpContext` для использования в методе. `context.Result` определяет, как осуществляется дополнительная обработка в конвейере.

| context.Result                       | Действие                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| `RuleResult.ContinueRules` (по умолчанию) | Продолжение применения правил                                         |
| `RuleResult.EndResponse`             | Остановка применения правил и отправка отклика                       |
| `RuleResult.SkipRemainingRules`      | Остановка применения правил и отправка контекста в следующий компонент ПО промежуточного слоя |

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

* * *
Пример приложения демонстрирует метод, который перенаправляет запросы для путей, заканчивающихся на *.xml*. Если сделать запрос `/file.xml`, он перенаправляется в `/xmlfiles/file.xml`. Для кода состояния задается значение "301 (перемещен окончательно)". Для перенаправления нужно явно задать код состояния ответа, в противном случае возвращается код состояния "200 (ОК)" и перенаправление для клиента не выполняется.

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

Исходный запрос: `/file.xml`

![Окно браузера, в котором средства разработчика отслеживают запросы и отклики для file.xml](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a>Правило, основанное на IRule
Используйте `Add(IRule)`, чтобы реализовать собственную логику правил в классе, являющемся производным от `IRule`. Использование `IRule` позволяет более гибко применять правила, основанные на методах. Производный класс может включать конструктор, куда можно передать параметры для метода `ApplyRule`.

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

* * *
Значения параметров в примере приложения для `extension` и `newPath` проверяются на соответствие нескольким условиям. `extension` должен содержать одно из значений: *.png*, *.jpg* или *.gif*. Если `newPath` не является допустимым, возникает исключение `ArgumentException`. Если сделать запрос *image.png*, он перенаправляется в `/png-images/image.png`. Если сделать запрос *image.jpg*, он перенаправляется в `/jpg-images/image.jpg`. Для кода состояния устанавливается значение "301 (перемещен окончательно)", а также задается `context.Result`, чтобы остановить обработку и отправить отклик.

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

Исходный запрос: `/image.png`

![Окно браузера, в котором средства разработчика отслеживают запросы и отклики для image.png](url-rewriting/_static/add_redirect_png_requests.png)

Исходный запрос: `/image.jpg`

![Окно браузера, в котором средства разработчика отслеживают запросы и отклики для image.jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>Примеры регулярных выражений

| Goal | Пример строки регулярного выражения<br>и совпадения | Пример строки замены и<br>выходных данных |
| ---- | :-----------------------------: | :------------------------------------: |
| Переопределение пути в строке запроса | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| Удаление косой черты в конце | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| Добавление косой черты в конце | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| Запрет переопределения отдельных запросов | `^(.*)(?<!\.axd)$` или `^(?!.*\.axd$)(.*)$`<br>Да: `/resource.htm`<br>Нет: `/resource.axd` | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| Переупорядочение сегментов URL-адреса | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| Замена сегмента URL-адреса | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a>Дополнительные ресурсы
* [Запуск приложения](startup.md)
* [ПО промежуточного слоя](xref:fundamentals/middleware/index)
* [Регулярные выражения в .NET](/dotnet/articles/standard/base-types/regular-expressions)
* [Элементы языка регулярных выражений — краткий справочник](/dotnet/articles/standard/base-types/quick-ref)
* [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)
* [Использование модуля переопределения URL-адресов 2.0 (для IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [Справочник по конфигурации модуля переопределения URL-адресов](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [Форум по модулю переопределения URL-адресов для IIS](https://forums.iis.net/1152.aspx)
* [Сохранение простой структуры URL-адресов](https://support.google.com/webmasters/answer/76329?hl=en)
* [10 советов по переопределению URL-адресов](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [Аспекты использования косой черты](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
