# <a name="aspnet-core-url-rewriting-sample-aspnet-core-1x"></a>Пример переопределения URL-адресов ASP.NET Core (ASP.NET Core 1.x)

Этот пример иллюстрирует использование ПО промежуточного слоя для переопределения URL-адресов ASP.NET Core 1.x. Это приложение демонстрирует варианты перенаправления и переопределения URL-адресов. Пример ASP.NET Core 2.x см. в разделе [Пример переопределения URL-адресов ASP.NET Core (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/2.x).

При запуске этого примера предоставляется отклик, указывающий переопределенный или перенаправленный URL-адрес, если к URL-адресу запроса применяется одно из правил.

## <a name="examples-in-this-sample"></a>Включенные примеры

* `AddRedirect("redirect-rule/(.*)", "redirected/$1")`
  - Код состояния успеха: "302 (объект найден)"
  - Пример (перенаправление): **/redirect-rule/{группа_записи}** в **/redirected/{группа_записи}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - Код состояния успеха: "200 (OK)"
  - Пример (переопределение): **/rewrite-rule/{группа_записи_1}/{группа_записи_2}** в **/rewritten?var1={группа_записи_1}&var2={группа_записи_2}**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - Код состояния успеха: "302 (объект найден)"
  - Пример (перенаправление): **/apache-mod-rules-redirect/{группа_записи}** в **/redirected?id={группа_записи}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - Код состояния успеха: "200 (OK)"
  - Пример (переопределение): **/iis-rules-rewrite/{группа_записи}** в **/rewritten?id={группа_записи}**
* `Add(RedirectXMLRequests)`
  - Код состояния успеха: 301 (перемещен окончательно)
  - Пример (перенаправление): **/file.xml** в **/xmlfiles/file.xml**
* `Add(new RedirectPNGRequests(".png", "/png-images")))`<br>`Add(new RedirectPNGRequests(".jpg", "/jpg-images")))`
  - Код состояния успеха: 301 (перемещен окончательно)
  - Пример (перенаправление): **/image.png** в **/png-images/image.png**
  - Пример (перенаправление): **/image.jpg** в **/jpg-images/image.jpg**

## <a name="using-a-physicalfileprovider"></a>Использование `PhysicalFileProvider`
Вы также можете получить `IFileProvider`, создав `PhysicalFileProvider` для передачи в методы `AddApacheModRewrite()` и `AddIISUrlRewrite()`:
```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```
## <a name="secure-redirection-extensions"></a>Расширения безопасного перенаправления
Этот пример включает в себя конфигурацию `WebHostBuilder`, чтобы приложение использовало URL-адреса (**https://localhost:5001**, **https://localhost**), и тестовый сертификат (**testCert.pfx**), чтобы помочь вам в изучении этих методов перенаправления. Добавьте любой из них в `RewriteOptions()` в файле **Startup.cs**, чтобы изучить их поведение.

Метод | Код состояния | Порт
--- | :---: | :---:
`.AddRedirectToHttpsPermanent()` | 301 | null (465)
`.AddRedirectToHttps()` | 302 | null (465)
`.AddRedirectToHttps(301)` | 301 | null (465)
`.AddRedirectToHttps(301, 5001)` | 301 | 5001
