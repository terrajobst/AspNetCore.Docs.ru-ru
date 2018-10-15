---
title: Предотвращения межсайтовых (запросов XSRF/CSRF) атак с подделкой в ASP.NET Core
author: steve-smith
description: Узнайте, как предотвратить атаки, направленные на веб-приложений, где вредоносный сайт может влиять на взаимодействие между клиентским браузером и приложения.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/anti-request-forgery
ms.openlocfilehash: 213d6d09501b5428bdaad454ec487702ef2a02a6
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325917"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>Предотвращения межсайтовых (запросов XSRF/CSRF) атак с подделкой в ASP.NET Core

По [Стив Смит](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), и [Рик Андерсон](https://twitter.com/RickAndMSFT)

Подделки межсайтовых запросов (также известные как XSRF или CSRF, произносится *см. в статье работа в Интернете*) — это атака, от приложений, размещенных на веб сервере, при котором вредоносные веб-приложения может влиять на взаимодействие между клиентским браузером и веб-приложение, которому доверяет, Обозреватель. Эти атаки возможны, так как веб-браузеры отправляют некоторых типов маркеров проверки подлинности автоматически при каждом запросе веб-сайта. Такая форма эксплойта также называется *атаки одним щелчком* или *«session riding»* так, как атаки использует преимущества пользователь ранее проверку подлинности сеанса.

Примером атаки CSRF:

1. Пользователь входит в `www.good-banking-site.com` проверки подлинности с помощью форм. Сервер проверяет подлинность пользователя и выдает ответ, включающий файл cookie проверки подлинности. Сайт является уязвимым для атак, так как она доверяет любой запрос, получающий объект cookie проверки подлинности.
1. Пользователь посещает веб, `www.bad-crook-site.com`.

   Вредоносный сайт `www.bad-crook-site.com`, с HTML-формой следующего вида:

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   Обратите внимание, что формы `action` сообщения на уязвимом сайте, а не к вредоносный сайт. Это часть CSRF «cross-site».

1. Пользователь выбирает кнопку отправки. Браузер выполняет запрос и автоматически включает в себя файл cookie проверки подлинности для запрошенного домена `www.good-banking-site.com`.
1. Запрос выполняется на `www.good-banking-site.com` сервера с контекстом проверки подлинности пользователя и могут выполнять любые действия, прошедший проверку пользователь может выполнять.

В дополнение к сценарии, где пользователь выбирает кнопку, чтобы отправить форму вредоносный сайт может:

* Запустите скрипт, который автоматически отправляет форму.
* Отправка отправки формы в качестве AJAX-запросом.
* Скрывайте форму с помощью CSS.

Эти альтернативные сценарии не требуется, любые действия или входные данные пользователя, отличные от изначально узле вредоносных.

С помощью протокола HTTPS не предотвращает атаки CSRF. Можно отправить вредоносный сайт `https://www.good-banking-site.com/` запроса так же легко, он может отправлять небезопасный запрос.

Некоторые атаки направлены конечных точек, которые отвечают на запросы GET, в этом случае тег изображения может использоваться для выполнения действия. Эту форму атаки характерен для веб-узлов форума, которые обеспечивают образов, но блокировка JavaScript. Приложения, которые изменяют состояние для запросов GET, где переменные или ресурсов, изменяются, уязвимы для атак злоумышленников. **Запросы GET, которые изменяют состояние сделаны ненадежными. Рекомендуется никогда не изменить состояние в запрос GET.**

Для веб-приложений, использующих файлы cookie для проверки подлинности, так как возможны CSRF-атакам.

* Обозреватели сохраняют файлы Сookie, выпущенные веб-приложения.
* Хранимые файлы cookie содержат файлы cookie сеанса для прошедших проверку пользователей.
* Браузеры отправляют все файлы cookie, связанную с доменом, в веб-приложение каждого запроса, независимо от того, как был создан запрос на приложение в браузере.

Тем не менее, не ограничены CSRF-атакам использования файлов cookie. Например Basic и дайджест-проверки подлинности также уязвимы. После пользователь выполняет вход с обычной или дайджест-проверки подлинности, браузер автоматически отправляет учетные данные до систему&dagger; заканчивается.

&dagger;В этом контексте *сеанса* ссылается на сеанса на стороне клиента, во время которого пользователь проходит проверку подлинности. Это не имеющих отношения к сеансов на сервере или [сеанс по промежуточного слоя ASP.NET Core](xref:fundamentals/app-state).

Пользователей можно защититься от уязвимостей CSRF путем принятия мер предосторожности:

* Выйти из веб-приложения после завершения их использования.
* Очистить файлы cookie браузера периодически.

Тем не менее CSRF уязвимости, по сути, проблема с веб-приложения, а не конечным пользователем.

## <a name="authentication-fundamentals"></a>Основы проверки подлинности

Проверка подлинности на основе файлов cookie — это популярная форма проверки подлинности. Систем проверки подлинности на основе маркеров растет популярность, особенно для одностраничных приложений (SPA).

### <a name="cookie-based-authentication"></a>Проверка подлинности на основе файлов cookie

Когда пользователь проходит проверку подлинности с помощью имени пользователя и пароля, они все выданный маркер, содержащий билет проверки подлинности, который может использоваться для проверки подлинности и авторизации. Он хранится в файле cookie, который сопровождает запрос на каждый клиент делает. Проверка подлинности по промежуточного слоя выполняет формирования и проверки этот файл cookie. [По промежуточного слоя](xref:fundamentals/middleware/index) сериализует участника-пользователя в зашифрованном файле cookie. При последующих запросах по промежуточного слоя проверяет файл cookie, повторно создает основной и назначает участнику [пользователя](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) свойство [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).

### <a name="token-based-authentication"></a>Проверка подлинности на основе маркеров

Когда пользователь проходит проверку подлинности, они все выдачи маркера (не токен против подделки). Маркер содержит сведения о пользователе в форме [утверждений](/dotnet/framework/security/claims-based-identity-model) или маркера ссылки, указывающий приложение поддерживается в приложении состояние пользователя. Когда пользователь пытается получить доступ к ресурсу, требующей проверки подлинности, маркер отправляется в приложение с заголовком дополнительной авторизации в виде маркера носителя. В результате приложения без отслеживания состояния. В каждый последующий запрос этот маркер передается в запросе для проверки на стороне сервера. Этот токен не *зашифрованных*; он имеет *кодировке*. На сервере токен декодируется для доступа к его данным. Чтобы отправить маркер при последующих запросах, сохранения токена в локальном хранилище браузера. Не стоит беспокоиться о CSRF уязвимости, если он хранится в локальном хранилище браузера. CSRF имеет значения, когда он хранится в файле cookie.

### <a name="multiple-apps-hosted-at-one-domain"></a>Несколько приложений, размещенных в одном домене

Общими средами размещения уязвимы для захвата сеанса входа CSRF и других атак.

Несмотря на то что `example1.contoso.net` и `example2.contoso.net` находятся на разных узлах, есть неявные доверительные отношения между узлами в группе `*.contoso.net` домена. Неявные доверительные отношения с этой позволяет потенциально небезопасных узлам влиять на друг друга файлы cookie (политики одного источника, определяющие запросов AJAX не иметь отношения к файлы cookie HTTP).

Благодаря использованию доменов можно предотвратить атак, использующих доверенных файлов cookie в приложениях, размещенных на том же домене. При каждой приложение размещено на своем собственном домене, отсутствует отношение доверия неявное куки-файл для использования.

## <a name="aspnet-core-antiforgery-configuration"></a>Сложные конфигурации ASP.NET Core

> [!WARNING]
> ASP.NET Core реализует против подделки с помощью [защиты данных в ASP.NET Core](xref:security/data-protection/introduction). В стеке защиты данных должен быть настроен для работы в ферме серверов. См. в разделе [Настройка защиты данных](xref:security/data-protection/configuration/overview) Дополнительные сведения.

В ASP.NET Core 2.0 или более поздней версии [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) внедряет против подделки токенов в элементы HTML-формы. Следующая разметка в файле Razor автоматически создает против подделки токены:

```cshtml
<form method="post">
    ...
</form>
```

Аналогично, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) создает против подделки токенов по умолчанию, если метод формы не GET.

Происходит автоматическое создание против подделки токенов для элементы HTML-формы при `<form>` тег содержит `method="post"` атрибут и одно из следующих условий:

  * Атрибут действия пуст (`action=""`).
  * Не указан атрибут действия (`<form method="post">`).

Можно отключить автоматическое создание против подделки маркеры для элементы HTML-формы:

* Явно отключить против подделки маркеры с `asp-antiforgery` атрибут:

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* Элемент формы является согласились горизонтального вспомогательных функций тегов с помощью вспомогательной функции тега [! символ отказаться](xref:mvc/views/tag-helpers/intro#opt-out):

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* Удалить `FormTagHelper` из представления. `FormTagHelper` Можно удалить из представления, добавив следующую директиву в представление Razor:

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Razor Pages](xref:razor-pages/index) автоматически защищаются от XSRF/CSRF. Дополнительные сведения см. в разделе [XSRF/CSRF и Razor Pages](xref:razor-pages/index#xsrf).

Для защиты от атак CSRF наиболее распространенным подходом является использование *шаблона токена синхронизатор* (STP). STP используется в том случае, когда пользователь запрашивает страницу с данными формы:

1. Сервер отправляет токен, связанный с удостоверением текущего пользователя на клиент.
1. Клиент отправляет обратно токен на сервер для проверки подлинности.
1. Если сервер получает маркер, который не соответствует удостоверение пользователя, прошедшего проверку подлинности, запрос отклоняется.

Токен должно быть уникальным и непредсказуемым. Маркер можно использовать также для обеспечения правильной последовательности из ряда запросов (например, обеспечьте последовательности запроса: страницы 1 &ndash; странице 2 &ndash; странице 3). Все формы в ASP.NET Core MVC и Razor Pages шаблоны создают против подделки маркеры. В следующей паре Просмотр примеров создания против подделки маркеров:

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

Явным образом добавить токен против подделки для `<form>` элемент без использования вспомогательных функций тегов с помощью вспомогательного метода HTML [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

В каждом из указанных случаев ASP.NET Core добавляет скрытое поле формы следующего вида:

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

ASP.NET Core включает в себя три [фильтры](xref:mvc/controllers/filters) для работы с против подделки токенов:

* [ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a>Сложные параметры

Настройка [против подделки параметры](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) в `Startup.ConfigureServices`:

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddAntiforgery(options => 
{
    // Set Cookie properties using CookieBuilder properties†.
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.SuppressXFrameOptionsHeader = false;
});
```

&dagger;Задайте защиты от подделки `Cookie` свойства с помощью свойства [CookieBuilder](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder) класса.

| Параметр | Описание |
| ------ | ----------- |
| [Файл cookie](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | Определяет параметры, используемые для создания против подделки файлы cookie. |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | Имя скрытого поля формы используемые против подделки системой для подготовки к просмотру против подделки токенов в представлениях. |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | Имя заголовка, используемый системой против подделки. Если `null`, то система рассматривает только данные формы. |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | Указывает, следует ли подавлять поколение `X-Frame-Options` заголовка. По умолчанию заголовок создается со значением «SAMEORIGIN». По умолчанию — `false`. |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

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
| [Файл cookie](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | Определяет параметры, используемые для создания против подделки файлы cookie. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | Домен cookie. По умолчанию — `null`. Это свойство является устаревшим и будет удален в будущей версии. Рекомендуемой альтернативой является Cookie.Domain. |
| [CookieName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | Имя файла cookie. Если не задано, система создает уникальное имя которого начинается с [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (». AspNetCore.Antiforgery.»). Это свойство является устаревшим и будет удален в будущей версии. Рекомендуемой альтернативой является Cookie.Name. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | Путь в файле cookie. Это свойство является устаревшим и будет удален в будущей версии. Рекомендуемой альтернативой является Cookie.Path. |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | Имя скрытого поля формы используемые против подделки системой для подготовки к просмотру против подделки токенов в представлениях. |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | Имя заголовка, используемый системой против подделки. Если `null`, то система рассматривает только данные формы. |
| [RequireSsl](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | Указывает, требуется ли SSL против подделки системой. Если `true`, происходит сбой запросов без SSL. По умолчанию — `false`. Это свойство является устаревшим и будет удален в будущей версии. Рекомендуемой альтернативой является установка Cookie.SecurePolicy. |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | Указывает, следует ли подавлять поколение `X-Frame-Options` заголовка. По умолчанию заголовок создается со значением «SAMEORIGIN». По умолчанию — `false`. |

::: moniker-end

Дополнительные сведения см. в разделе [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).

## <a name="configure-antiforgery-features-with-iantiforgery"></a>Настройка против подделки функций работы с IAntiforgery

[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) предоставляет API для настройки функций против подделки. `IAntiforgery` можно запросить в `Configure` метод `Startup` класса. В следующем примере используется по промежуточного слоя с домашней страницы приложения, чтобы создать токен против подделки и отправить его в ответ как файл cookie (с помощью именования по умолчанию Angular описано далее в этом разделе):

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

### <a name="require-antiforgery-validation"></a>Требовать проверку против подделки

[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) является фильтром операции, которые могут применяться для отдельного действия, контроллера или глобально. Запросы к действиям, которые имеют этот фильтр, применяемый будут заблокированы, если запрос содержит допустимый токен против подделки.

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

`ValidateAntiForgeryToken` Атрибут требует маркер для запросов к методам действий, он сопровождает, включая HTTP-запросы GET. Если `ValidateAntiForgeryToken` атрибут применяется на контроллерах приложения, его можно переопределить с помощью `IgnoreAntiforgeryToken` атрибута.

> [!NOTE]
> ASP.NET Core не поддерживает добавление маркеры против подделки запросов GET автоматически.

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a>Автоматически проверьте против подделки маркеры на предмет только небезопасных методов HTTP

Приложения ASP.NET Core не создают против подделки маркеры для безопасные методы HTTP (GET, HEAD, параметры и ТРАССИРОВКИ). Вместо применения широко `ValidateAntiForgeryToken` атрибут и затем переопределение с помощью `IgnoreAntiforgeryToken` атрибуты, [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) атрибут может использоваться. Этот атрибут работает идентично `ValidateAntiForgeryToken` атрибут, за исключением того, что он не требует токены для запросов, выполненных с помощью следующих методов HTTP:

* GET
* HEAD,
* OPTIONS
* TRACE

Рекомендуется использовать `AutoValidateAntiforgeryToken` широко для сценариев, отличных от API. Это гарантирует, что действия после защищены по умолчанию. Вместо этого должен игнорировать против подделки токенов по умолчанию, если не `ValidateAntiForgeryToken` применяется для отдельных методов действий. Он более вероятно в этом сценарии для метода POST действие должно быть оставлено без защиты по ошибке, оставляя приложение уязвимым к CSRF-атакам. Все сообщения, необходимо отправить маркер защиты от подделки.

API-интерфейсы не имеют автоматический механизм для отправки не файл cookie частью маркера. Реализация, вероятно, зависит от реализации кода клиента. Ниже приведены некоторые примеры:

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

### <a name="override-global-or-controller-antiforgery-attributes"></a>Переопределение глобальных или против подделки атрибутов контроллера

[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) фильтр позволяет избежать необходимости токен от подделки, для заданного действия (или контроллера). При применении этого фильтра переопределяет `ValidateAntiForgeryToken` и `AutoValidateAntiforgeryToken` фильтрам, указанным на более высоком уровне (глобально или на контроллере).

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

## <a name="refresh-tokens-after-authentication"></a>Маркеры обновления после проверки подлинности

Маркеры должны обновляться после проверки подлинности пользователя, перенаправляя пользователя к представлению или страницы Razor Pages.

## <a name="javascript-ajax-and-spas"></a>JavaScript, AJAX и одностраничные приложения

В традиционных приложениях на базе HTML против подделки маркеры передаются на сервер с помощью скрытых полях формы. В современных приложений на базе JavaScript и одностраничные приложения много запросов выполняются программно. Эти запросы AJAX могут использовать другие технологии (например, заголовки запросов или файлы cookie) для отправки маркера.

Если файлы cookie используются для хранения маркеров проверки подлинности и проверки подлинности запросов API, на сервере, CSRF может стать проблемой. Если локальное хранилище используется для сохранения токена, CSRF уязвимость может устранен, так как значения из локального хранилища не автоматически отправляются на сервер с каждым запросом. Таким образом с помощью локального хранилища для хранения маркер защиты от подделки на клиенте, а также отправки маркера, как в заголовке запроса — это рекомендуемый подход.

### <a name="javascript"></a>JavaScript

Использование JavaScript с представлениями, маркер могут создаваться с использованием службу в представлении. Внедрить [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) службы в представления и вызовите [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

Такой подход избавляет от необходимости работать непосредственно с помощью параметра с сервера файлы cookie или их считывания из клиента.

В предыдущем примере JavaScript считывается значение скрытого поля для заголовка AJAX POST.

JavaScript можно также маркеров доступа в файлы cookie и использовать содержимое этого файла cookie для создания заголовка со значением маркера.

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

При условии, что скрипт запрашивает отправку маркера в заголовок, называемый `X-CSRF-TOKEN`, настроить службу против подделки искать `X-CSRF-TOKEN` заголовка:

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

В следующем примере используется JavaScript для выполнения запроса AJAX с соответствующий заголовок:

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

AngularJS используется соглашение по адресу CSRF. Если сервер отправляет файл cookie с именем `XSRF-TOKEN`, AngularJS `$http` служба добавляет значение файла cookie в заголовке, когда он отправляет запрос к серверу. Этот процесс выполняется автоматически. Заголовок не требуется явным образом задать. Имя заголовка — `X-XSRF-TOKEN`. Сервер должен обнаружить этот заголовок и проверить его содержимое.

Для ASP.NET Core API работы с этим соглашением:

* Настройте приложение, чтобы предоставить маркер в файл cookie с именем `XSRF-TOKEN`.
* Настроить службу против подделки искать заголовок с именем `X-XSRF-TOKEN`.

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="extend-antiforgery"></a>Расширение защиты от подделки

[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) типа позволяет разработчикам расширять поведение системы anti-CSRF, циклической обработки дополнительных данных в каждом маркере. [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) метод вызывается каждый раз создается маркер поля, и возвращаемое значение внедрены в созданный маркер. Разработчик может возвращать метку времени, nonce или любое другое значение, а затем вызвать [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) проверить эти данные, когда маркер проверяется. Имя пользователя клиента внедренных в создаваемые маркеры, поэтому нет необходимости, чтобы включить эту информацию. Если маркер содержит дополнительные данные, но не `IAntiForgeryAdditionalDataProvider` будет настроен, дополнительные данные не проверяются.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) на [откройте Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).
* <xref:host-and-deploy/web-farm>
