# <a name="aspnet-core-url-rewriting-sample-aspnet-core-1x"></a>URL-адреса ASP.NET Core перезаписи образец (ASP.NET Core 1.x)

В этом примере описывается использование ASP.NET Core 1.x по промежуточного слоя перезаписи URL-адрес. Приложение демонстрирует перенаправления URL-адреса и параметров исправления URL-адрес. Для образца ASP.NET Core 2.x. в разделе [образец перезаписи URL-адреса ASP.NET Core (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/2.x).

При выполнении этого образца, ответ будет обслуживаться, показывающий перезаписанного или перенаправленный URL-адрес, если одно из правил применяется к URL-АДРЕСЕ запроса.

## <a name="examples-in-this-sample"></a>Примеры в этом образце

* `AddRedirect("redirect-rule/(.*)", "$1")`
  - Код состояния успеха: 302 (найдено)
  - Пример (перенаправление): **/redirect-rule / {capture_group}** для **/redirected/ {capture_group}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - Код состояния успеха: 200 (ОК)
  - Пример (версии): **/rewrite-rule / {capture_group_1} / {capture_group_2}** для **/ переписать? переменная1 = {capture_group_1} & var2 = {capture_group_2}**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - Код состояния успеха: 302 (найдено)
  - Пример (перенаправление): **/apache-mod-rules-redirect / {capture_group}** для **/ перенаправлено? id = {capture_group}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - Код состояния успеха: 200 (ОК)
  - Пример (версии): **/iis-rules-rewrite / {capture_group}** для **/ переписать? id = {capture_group}**
* `Add(RedirectXMLRequests)`
  - Код состояния успеха: 301 (перемещен постоянно)
  - Пример (перенаправление): **/file.xml** для **/xmlfiles/file.xml**
* `Add(new RedirectPNGRequests(".png", "/png-images")))`<br>`Add(new RedirectPNGRequests(".jpg", "/jpg-images")))`
  - Код состояния успеха: 301 (перемещен постоянно)
  - Пример (перенаправление): **/image.png** для **/png-images/image.png**
  - Пример (перенаправление): **/image.jpg** для **/jpg-images/image.jpg**

## <a name="using-a-physicalfileprovider"></a>С помощью`PhysicalFileProvider`
Можно также получить `IFileProvider` путем создания `PhysicalFileProvider` для передачи в `AddApacheModRewrite()` и `AddIISUrlRewrite()` методов:
```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```
## <a name="secure-redirection-extensions"></a>Защищенные расширения перенаправления
Этот пример включает `WebHostBuilder` конфигурации для приложения использовать URL-адреса (**https://localhost:5001**, **https://localhost**) и тестовый сертификат (**testCert.pfx**) для просмотра этих перенаправления методы. Добавьте любой из них `RewriteOptions()` в **файла Startup.cs** для изучения их поведения.

Метод | Код состояния | Порт
--- | :---: | :---:
`.AddRedirectToHttpsPermanent()` | 301 | NULL (465)
`.AddRedirectToHttps()` | 302 | NULL (465)
`.AddRedirectToHttps(301)` | 301 | NULL (465)
`.AddRedirectToHttps(301, 5001)` | 301 | 5001
