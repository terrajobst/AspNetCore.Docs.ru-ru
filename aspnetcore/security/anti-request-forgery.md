---
title: Предотвратить межсайтовых запросов подделки XSRF-атак в ASP.NET Core
author: steve-smith
description: Узнайте, как предотвратить атаки, направленные на веб-приложений, где вредоносный веб-сайт может повлиять на взаимодействие между веб-браузер клиента и приложения.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: ad50f8b261447d40ccc24c0ee006239aa976bf20
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2018
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>Предотвратить межсайтовых запросов подделки XSRF-атак в ASP.NET Core

По [Стив Смит](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), и [Рик Андерсон](https://twitter.com/RickAndMSFT)

Подделки межсайтовых запросов (также известный как XSRF или CSRF, произносится *путешествие см. в разделе*) — это атака, в приложениях веб сервере, при котором вредоносных веб-приложения могут повлиять на взаимодействие между веб-браузер клиента и веб-приложение, которому доверяет, Обозреватель. Эти атаки возможны, так как веб-браузеры отправляют некоторые типы токены проверки подлинности автоматически при каждом запросе на веб-сайт. Эта форма эксплойта называется также *атаки одним щелчком* или *riding сеанса* так, как атака использует преимущества ранее проверить подлинность пользователя сеанса.

Примером атаки CSRF:

1. Пользователь входит в `www.good-banking-site.com` проверки подлинности с помощью форм. Сервер проверяет подлинность пользователя и отправляет ответ, который включает файл cookie проверки подлинности. Этот сайт является уязвимым для атак, так как она доверяет любому запросу, который получает объект cookie проверки подлинности.
1. Пользователь посещает вредоносный сайт, `www.bad-crook-site.com`.

   Вредоносный сайт `www.bad-crook-site.com`, содержит HTML-форму следующим образом:

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   Обратите внимание, что формы `action` уязвимым сайта, а не к вредоносный сайт в блогах. Это часть CSRF «между сайтами».

1. Пользователь выбирает кнопку «Отправить». Браузер отправляет запрос и автоматически включает файл cookie проверки подлинности для запрошенного домена `www.good-banking-site.com`.
1. Запрос выполняется на `www.good-banking-site.com` сервер с контекст проверки подлинности пользователя и может выполнять любое действие, которое может выполнять проверку подлинности пользователя.

Когда пользователь выбирает эту кнопку для отправки формы, вредоносный сайт может:

* Запустите скрипт, который автоматически отправляет форму.
* Отправляет отправки формы в качестве AJAX-запросом. 
* С помощью CSS с помощью скрытого формы. 

С помощью протокола HTTPS не предотвращает атаки CSRF. Можно отправить вредоносный сайт `https://www.good-banking-site.com/` запроса так же легко, как его можно отправить запрос небезопасен.

Некоторые атаки целевых конечных точек, которые отвечают на запросы GET, в этом случае тег изображения можно использовать для выполнения действия. Эта форма атаки проявляется на форуме сайтов, которые обеспечивают изображения, но блокирует JavaScript. Приложения, которые изменяют состояние для запросов GET, где переменные или ресурсов был изменен, уязвимы для атак злоумышленников. **Запросов GET, которые изменяют состояние, не являются безопасными. Рекомендуется никогда не изменяют состояние на запрос GET.**

Для веб-приложений, которые используют файлы cookie для проверки подлинности, так как возможны атаки CSRF:

* Браузеры хранения файлов cookie, выданные веб-приложения.
* Хранимые файлы cookie содержат файлы cookie сеансов для прошедших проверку пользователей.
* Браузеры отправляют все файлы cookie, связанные с домена к веб-приложению каждого запроса, независимо от того, как был создан запрос на приложение в браузере.

Тем не менее, не ограничиваясь атаки CSRF использовать файлы cookie. Например Basic и дайджест-проверки подлинности также уязвимы. После пользователь выполняет вход с обычная или краткая проверка подлинности, браузер автоматически отправляет учетные данные до систему&dagger; заканчивается.

&dagger;В этом контексте *сеанса* ссылается на клиентский сеанс, во время которого пользователь проходит проверку подлинности. Это не связанные сеансы на стороне сервера или [по промежуточного слоя сеанса ASP.NET Core](xref:fundamentals/app-state).

Пользователям защититься от уязвимостей CSRF соблюдать меры предосторожности:

* Выйти из веб-приложения после завершения их использования.
* Очистить файлы cookie в браузере периодически.

Тем не менее уязвимости CSRF фактически являются проблемы с веб-приложения, а не конечным пользователем.

## <a name="authentication-fundamentals"></a>Основы проверки подлинности

Проверка подлинности на основе файла Cookie является популярная форма проверки подлинности. Токены проверки подлинности системы рост популярности, особенно для приложений на одной странице (SPAs).

### <a name="cookie-based-authentication"></a>Проверка подлинности на основе файлов cookie

При прохождении пользователем проверки подлинности, используя имя пользователя и пароль, они выполняется выданный маркер, содержащий билет проверки подлинности, который может использоваться для проверки подлинности и авторизации. Маркер сохраняется как файл cookie, который сопровождает каждый запрос клиента. Создание и проверка этот файл cookie осуществляется по промежуточного слоя проверки подлинности файла Cookie. [По промежуточного слоя](xref:fundamentals/middleware/index) сериализует участника-пользователя в зашифрованном файле cookie. При последующих запросах по промежуточного слоя проверяет куки-файл, повторно создает основной и назначает участнику [пользователя](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) свойство [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).

### <a name="token-based-authentication"></a>Токены проверки подлинности

Если пользователь прошел проверку подлинности, выполняется выдаются ли они маркер (не сложные). Токен содержит сведения о пользователе в виде [утверждений](/dotnet/framework/security/claims-based-identity-model) или ссылка маркер, указывающий приложения состояния пользователя в приложении. Когда пользователь пытается получить доступ к ресурсу, требующей проверки подлинности, маркер отправляется в приложении с помощью заголовок дополнительных авторизации в виде токена носителя. Это делает приложение без сохранения состояния. Во все последующие запросы что токен передается в запросе для проверки на стороне сервера. Этот токен не *зашифрованные*; он имеет *кодировке*. На сервере декодирует токен для доступа к информации. Маркер отправляется в последующих запросах, сохранения токена в локальном хранилище браузера. Не стоит беспокоиться о CSRF уязвимости, если маркер хранится в локальном хранилище браузера. CSRF возникает, когда токен хранится в файле cookie.

### <a name="multiple-apps-hosted-at-one-domain"></a>Несколько приложений, размещенных в одном домене

Общими средами размещения уязвимы для захвата сеанса входа CSRF и других атак.

Несмотря на то что `example1.contoso.net` и `example2.contoso.net` разных узлах, которые есть неявные доверительные отношения между узлы на `*.contoso.net` домена. Неявные отношения доверия между позволяет потенциально небезопасных узлам влияет на файлы cookie друг друга (политики одного источника, которые управляют запросы AJAX не относиться к файлы cookie HTTP).

Можно предотвратить атак, использующих доверенных файлов cookie между приложения, размещенные на том же домене, не предоставив доменов. Когда каждое приложение размещается в собственном домене, не задано отношение доверия неявное куки-файл для использования.

## <a name="aspnet-core-antiforgery-configuration"></a>Сложные конфигурации ASP.NET Core

> [!WARNING]
> ASP.NET Core реализует сложные с помощью [защиты данных ASP.NET Core](xref:security/data-protection/introduction). Стек защиты данных должен быть настроен для работы в ферме серверов. В разделе [настройки защиты данных](xref:security/data-protection/configuration/overview) для получения дополнительной информации.

В ASP.NET Core 2.0 или более поздней версии [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) внедряет сложные токены в форме HTML-элементов. Приведенный ниже код в файле Razor автоматически создает сложные токены:

```cshtml
<form method="post">
    ...
</form>
```

Аналогично при попытке [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) создает сложные токенов по умолчанию, если метод формы не GET.

Происходит автоматическое создание сложные маркеров для элементов формы HTML при `<form>` тег содержит `method="post"` верны атрибут и одно из следующих:

  * Действие атрибута пуст (`action=""`).
  * Не указан атрибут действия (`<form method="post">`).

Можно отключить автоматическое создание сложные маркеров для элементов формы HTML.

* Явно отключить сложные токены с `asp-antiforgery` атрибута:

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* Элемент form согласился извлечения вспомогательных функций тегов с помощью вспомогательного тег [! символ отказаться](xref:mvc/views/tag-helpers/intro#opt-out):

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* Удалите `FormTagHelper` из представления. `FormTagHelper` Можно удалить из представления путем добавления представления Razor следующую директиву:

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Страниц Razor](xref:mvc/razor-pages/index) автоматически защищены от XSRF-CSRF. Дополнительные сведения см. в разделе [XSRF/CSRF и страниц Razor](xref:mvc/razor-pages/index#xsrf).

Чтобы защититься от атак CSRF наиболее распространенным подходом является использование *синхронизатор шаблона токена* (STP). STP используется в том случае, если пользователь запрашивает страницу с данными в форме:

1. Сервер отправляет токен отмены, связанный с удостоверением текущего пользователя для клиента.
1. Клиент отправляет маркер на сервер для проверки.
1. Если сервер получает маркер, который не совпадает с удостоверением пользователя, прошедшего проверку подлинности, запрос отклоняется.

Маркер является уникальным и непредсказуемым. Маркер может также использоваться для обеспечения правильной последовательности серию запросов (например, обеспечивая последовательность запроса: страницы 1 &ndash; страницу 2 &ndash; страница 3). Все формы в ASP.NET Core MVC и страниц Razor шаблоны создавать сложные маркеры. В следующей паре примеры режима создавать сложные маркеры:

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

Явным образом добавить сложные токен `<form>` элемент без использования вспомогательных функций тегов с вспомогательный метод HTML [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

Во всех перечисленных случаях ASP.NET Core добавляет скрытое поле формы следующего вида:

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

ASP.NET Core включает в себя три [фильтры](xref:mvc/controllers/filters) для работы с сложные токены:

* [ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a>Сложные параметры.

Настройка [сложные параметры](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) в `Startup.ConfigureServices`:

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "contoso.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

| Параметр | Описание |
| ------ | ----------- |
| [Cookie](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | Определяет параметры, используемые для создания сложные файлы cookie. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | Домен cookie. По умолчанию — `null`. Это свойство является устаревшим и будет удален в будущей версии. Взамен рекомендуется использовать — Cookie.Domain. |
| [CookieName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | Имя файла cookie. Если не задано, система создает уникальное имя которого начинается с [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (». AspNetCore.Antiforgery.»). Это свойство является устаревшим и будет удален в будущей версии. Взамен рекомендуется использовать — Cookie.Name. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | Путь задать в файле cookie. Это свойство является устаревшим и будет удален в будущей версии. Взамен рекомендуется использовать — Cookie.Path. |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | Имя скрытое поле формы используются сложные системой для подготовки к просмотру сложные токены в представлениях. |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | Имя заголовка, используемые системой сложные. Если `null`, то система рассматривает только данные формы. |
| [RequireSsl](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | Указывает, требуется ли SSL сложные системой. Если `true`, без использования SSL не установится. По умолчанию — `false`. Это свойство является устаревшим и будет удален в будущей версии. Взамен рекомендуется использовать является установка Cookie.SecurePolicy. |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | Указывает, следует ли подавлять поколение `X-Frame-Options` заголовок. По умолчанию заголовок создается со значением «SAMEORIGIN». По умолчанию — `false`. |

Дополнительные сведения см. в разделе [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).

## <a name="configure-antiforgery-features-with-iantiforgery"></a>Настроить сложные функции с IAntiforgery

[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) предоставляет API, чтобы настроить сложные функции. `IAntiforgery` можно запросить в `Configure` метод `Startup` класса. В следующем примере ПО промежуточного слоя с домашней страницы приложения для создания токена сложные и его отправки в ответ в виде куки-файл (с помощью именования по умолчанию углового описано далее в этом разделе):

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}
```

### <a name="require-antiforgery-validation"></a>Требовать проверку сложные

[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) является фильтром действий, могут применяться для отдельного действия, контроллера или глобально. Запросы, адресованные действий, имеющих этот фильтр, применяемый будут заблокированы, если запрос содержит сложные допустимый токен.

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

`ValidateAntiForgeryToken` Атрибута требуется маркер для запросов к методам действий, в которой относится, включая HTTP-запросы GET. Если `ValidateAntiForgeryToken` атрибут между контроллерами приложения, его можно переопределить с помощью `IgnoreAntiforgeryToken` атрибута.

> [!NOTE]
> ASP.NET Core не поддерживает автоматическое добавление маркеров сложные запросы GET.

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a>Автоматически проверки сложные токенов небезопасный HTTP только для методов

Приложения ASP.NET Core не создавать сложные маркерах безопасные методы HTTP (GET, HEAD, параметры и ТРАССИРОВКИ). Вместо применения широко `ValidateAntiForgeryToken` атрибута и затем переопределение его с `IgnoreAntiforgeryToken` атрибуты, [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) атрибут может использоваться. Этот атрибут работает идентично методу `ValidateAntiForgeryToken` атрибута, за исключением того, что он не требует токены для запросов, выполненных с помощью следующих методов HTTP:

* GET
* HEAD,
* OPTIONS
* TRACE

Рекомендуется использовать `AutoValidateAntiforgeryToken` широко для сценариев, отличных от API. Это гарантирует, что действия после защищены по умолчанию. Вместо этого должен игнорировать сложные токены по умолчанию, если не `ValidateAntiForgeryToken` применяется для отдельных методов действий. Он более вероятно в этом сценарии для метода POST действие должно быть оставлено незащищенными по ошибке, оставляя приложение уязвимым для атак CSRF. Все сообщения, необходимо отправить токен сложные.

API-интерфейсы не имеют автоматический механизм для отправки не cookie часть маркера. Реализация, скорее всего, зависит от реализации кода клиента. Ниже приведены некоторые примеры:

Пример уровня класса.

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

Пример глобального.

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a>Переопределение глобальных или сложные атрибутов контроллера

[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) фильтр используется для устранения необходимости в сложные маркера для данного действия (или контроллера). При применении этого фильтра переопределяет `ValidateAntiForgeryToken` и `AutoValidateAntiforgeryToken` фильтров, указанных на более высоком уровне (глобально или на контроллере).

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
    [HttpPost]
    [IgnoreAntiforgeryToken]
    public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
    {
        // no antiforgery token required
    }
}
```

## <a name="refresh-tokens-after-authentication"></a>Токенов обновления после проверки подлинности

Маркеры должны обновляться после проверки подлинности пользователя путем перенаправления пользователя на просмотр или страницу страниц Razor.

## <a name="javascript-ajax-and-spas"></a>JavaScript, AJAX и SPAs

В традиционные приложения на основе HTML сложные токены, передаются на сервер с помощью скрытые поля формы. В современных приложений на базе JavaScript и SPAs многие запросы выполняются программно. Маркер отправляется в эти запросы AJAX может использовать другие методы (например, заголовки запроса или куки-файлы).

Если файлы cookie используются для хранения токенов проверки подлинности и проверки подлинности запросов API на сервере, CSRF может стать проблемой. При использовании локального хранилища для сохранения токена CSRF уязвимость может устранить, так как значения из локального хранилища не отправляются автоматически на сервер с каждым запросом. Таким образом использует локальное хранилище для хранения сложные маркера на клиенте и отправка маркера как заголовок запроса является рекомендуемым.

### <a name="javascript"></a>JavaScript

С представлениями с использованием JavaScript, токен можно создать с помощью службы из представления. Вставить [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) в представлении и вызовите службу [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

Этот подход избавляет от необходимости работать напрямую с Установка с сервера файлы cookie или их считывания из клиента.

В предыдущем примере использовался JavaScript для чтения значение скрытого поля заголовка AJAX POST.

JavaScript можно также токенов в файлах cookie доступа и использовать содержимое файла cookie для создания заголовка с значение токена.

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

При условии, что скрипт запрашивает Отправка токена в заголовок с именем `X-CSRF-TOKEN`, настройте службу сложные искать `X-CSRF-TOKEN` заголовка:

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

В следующем примере используется JavaScript для выполнения запроса с соответствующий заголовок AJAX:

```javascript
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

var csrfToken = getCookie("CSRF-TOKEN");

var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (xhttp.readyState == XMLHttpRequest.DONE) {
        if (xhttp.status == 200) {
            alert(xhttp.responseText);
        } else {
            alert('There was an error processing the AJAX request.');
        }
    }
};
xhttp.open('POST', '/api/password/changepassword', true);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.setRequestHeader("X-CSRF-TOKEN", csrfToken);
xhttp.send(JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }));
```

### <a name="angularjs"></a>AngularJS

AngularJS используется соглашение по адресу CSRF. Если сервер отправляет куки-файл с именем `XSRF-TOKEN`, AngularJS `$http` служба добавляет значение файла cookie в заголовок при отправке запроса на сервер. Этот процесс выполняется автоматически. Заголовок не нужно задавать явно. Имя заголовка — `X-XSRF-TOKEN`. Сервер должен обнаружить этот заголовок и проверьте его содержимое.

Для ASP.NET Core API работают с этим соглашением.

* Настройте приложение, чтобы предоставить токен в файле cookie, вызывается `XSRF-TOKEN`.
* Настройте службу сложные поиск заголовка с именем `X-XSRF-TOKEN`.

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="extend-antiforgery"></a>Расширить antiforgery

[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) типа позволяет разработчикам расширять поведение системы защиты CSRF с циклической обработки дополнительных данных в каждом маркере. [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) при каждом вызове метода создается маркер поля, и возвращаемое значение внедряется в созданный токен. Разработчик может возвращать отметку времени, nonce или любое другое значение, а затем вызвать [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) для проверки данных, если маркер проверяется. Имя пользователя клиента уже внедрена в создаваемые маркеры, поэтому нет необходимости включать эти сведения. Если токен содержит дополнительные данные, но нет `IAntiForgeryAdditionalDataProvider` будет настроен, дополнительные данные не проверяются.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) на [откройте проект безопасности веб-приложения](https://www.owasp.org/index.php/Main_Page) (OWASP).
