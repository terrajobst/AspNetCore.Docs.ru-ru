# <a name="aspnet-core-url-rewriting-sample"></a>Пример переопределения URL-адресов ASP.NET Core

Этот пример иллюстрирует использование ПО промежуточного слоя для переопределения URL-адресов ASP.NET Core. Это приложение демонстрирует варианты перенаправления и переопределения URL-адресов.

При запуске этого примера нефайловые отклики возвращают переопределенный или перенаправленный URL-адрес, если к URL-адресу запроса применяется одно из правил. Для примеров текстовых или XML-файлов ПО промежуточного слоя статических файлов обслуживает файл после того, как ПО промежуточного слоя переопределяет URL-адрес запроса.

## <a name="examples-in-this-sample"></a>Включенные примеры

* `AddRedirect("redirect-rule/(.*)", "redirected/$1")`
  - Код состояния успеха: 302 — объект найден
  - Пример (перенаправление): **/redirect-rule/{группа_записи}** в **/redirected/{группа_записи}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - Код состояния успеха: 200 OK;
  - Пример (переопределение): **/rewrite-rule/{группа_записи_1}/{группа_записи_2}** в **/rewritten?var1={группа_записи_1}&var2={группа_записи_2}**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - Код состояния успеха: 302 — объект найден
  - Пример (перенаправление): **/apache-mod-rules-redirect/{группа_записи}** в **/redirected?id={группа_записи}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - Код состояния успеха: 200 OK;
  - Пример (переопределение): **/iis-rules-rewrite/{группа_записи}** в **/rewritten?id={группа_записи}**
* `Add(RedirectXmlFileRequests)`
  - Код состояния успеха: 301 — перемещено навсегда
  - Пример (перенаправление): **/file.xml** в **/xmlfiles/file.xml**
* `Add(RewriteTextFileRequests)`
  - Код состояния успеха: 200 OK;
  - Пример (переопределение): **/some_file.txt** в **/file.txt**
* `Add(new RedirectImageRequests(".png", "/png-images")))`<br>`Add(new RedirectImageRequests(".jpg", "/jpg-images")))`
  - Код состояния успеха: 301 — перемещено навсегда
  - Пример (перенаправление): **/image.png** в **/png-images/image.png**
  - Пример (перенаправление): **/image.jpg** в **/jpg-images/image.jpg**

## <a name="use-a-physicalfileprovider"></a>Использование PhysicalFileProvider

Вы также можете получить `IFileProvider`, создав `PhysicalFileProvider` для передачи в методы `AddApacheModRewrite()` и `AddIISUrlRewrite()`:

```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```

## <a name="secure-redirection-extensions"></a>Расширения безопасного перенаправления

Этот пример включает в себя конфигурацию `WebHostBuilder`, чтобы приложение использовало URL-адреса (`https://localhost:5001`, `https://localhost`), и тестовый сертификат (*testCert.pfx*) с целью помочь вам в изучении этих методов безопасного перенаправления. Если порт 443 на сервере уже назначен или используется, пример `https://localhost` не работает &mdash; нужно удалить `ListenOptions` для порта 443 в методе `CreateWebHostBuilder` файла *Program.cs* или отменить привязку порта 443 на сервере, чтобы Kestrel мог использовать этот порт.

| Метод                           | Код состояния |    Порт    |
| -------------------------------- | :---------: | :--------: |
| `.AddRedirectToHttpsPermanent()` |     301     | null (465) |
| `.AddRedirectToHttps()`          |     302     | null (465) |
| `.AddRedirectToHttps(301)`       |     301     | null (465) |
| `.AddRedirectToHttps(301, 5001)` |     301     |    5001    |
