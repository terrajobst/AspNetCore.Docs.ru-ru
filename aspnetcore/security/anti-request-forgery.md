---
title: "Межсайтовых запросов подделки XSRF-атак в ASP.NET Core"
author: steve-smith
description: "Межсайтовых запросов подделки XSRF-атак в ASP.NET Core"
manager: wpickett
ms.author: riande
ms.date: 7/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: e076e301004c04b5c516d775353a4b6e50a3f36e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="preventing-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>Межсайтовых запросов подделки XSRF-атак в ASP.NET Core

[Стив Смит](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), и [Рик Андерсон](https://twitter.com/RickAndMSFT)

## <a name="what-attack-does-anti-forgery-prevent"></a>Какие атаки предотвращения подделки?

Подделки межсайтовых запросов (также известный как XSRF или CSRF, произносится *путешествие см. в разделе*) — это атака, для приложений веб сервере, при котором вредоносных веб-сайта могут повлиять на взаимодействие между браузером клиента и веб-сайт, которому доверяет Этот браузер. Такие атаки становится возможным, так как веб-браузеры отправку некоторых типов маркеров проверки подлинности автоматически при каждом запросе веб-сайту. Уязвимости в этой форме, также известный как *атаки одним щелчком* или как *riding сеанса*, так как пользуется атаки пользователем прошедшим проверку подлинности сеанса.

Примером атаки CSRF:

1. Пользователь выполняет вход в `www.example.com`, с помощью проверки подлинности форм.
2. Сервер проверяет подлинность пользователя и отправляет ответ, который включает файл cookie проверки подлинности.
3. Пользователь посещает вредоносный сайт.

   Вредоносный сайт с HTML-формой следующего вида:

```html
   <h1>You Are a Winner!</h1>
     <form action="http://example.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw" />
       <input type="hidden" name="Amount" value="1000000" />
     <input type="submit" value="Click Me"/>
   </form>
```

Обратите внимание, что действие формы в блогах уязвимым сайте, не вредоносный сайт. Это часть CSRF «между сайтами».

4. При нажатии кнопки "Отправить". Браузер автоматически включает файл cookie проверки подлинности для запрошенного домена (уязвимым сайта в данном случае) с запросом.
5. Запрос выполняется на сервере с помощью контекста проверки подлинности пользователей и могут выполнять любые действия, прошедшего проверку подлинности пользователя разрешено делать.

В этом примере требуется пользователь, нажав кнопку формы. Удалось вредоносную страницу:

* Запустите скрипт, который автоматически отправляет форму.
* Отправляет отправки формы в качестве AJAX-запросом. 
* С помощью CSS с помощью скрытого формы. 

С помощью протокола SSL не предотвращает атаки CSRF, можно отправить вредоносный сайт `https://` запроса. 

Некоторые атаки целевого сайта конечных точек, которые отвечают на `GET` запросы, в которых регистр тег изображения можно использовать для выполнения действия (эту форму атаки проявляется на форуме сайтов, которые обеспечивают изображения, но блокировать JavaScript). Приложения, которые изменяют состояние с `GET` уязвимы запросов от атак злоумышленников.

Атаки CSRF возможны с веб-сайтов, которые используют файлы cookie для проверки подлинности, так как браузеры отправляют все соответствующие файлы cookie в веб-узел назначения. Тем не менее атаки CSRF ограничены не использовать файлы cookie. Например Basic и дайджест-проверки подлинности также уязвимы. После входа пользователя в систему с обычная или краткая проверка подлинности браузер автоматически отправляет учетные данные до завершения сеанса.

Примечание: В этом контексте *сеанса* ссылается на клиентский сеанс, во время которого пользователь проходит проверку подлинности. Это не связанные сеансы на стороне сервера или [сеанса по промежуточного слоя](xref:fundamentals/app-state).

Пользователям организовать защиту от уязвимостей CSRF с:
* Ведение журнала из веб-сайтов, завершении работы с их помощью.
* Периодически очистить файлы cookie браузера.

Тем не менее уязвимости CSRF фактически являются проблемы с веб-приложения, а не конечным пользователем.

## <a name="how-does-aspnet-core-mvc-address-csrf"></a>Как ASP.NET Core MVC решать CSRF

> [!WARNING]
> ASP.NET Core реализуется с помощью приложения для защиты request подделки [стека защиты данных ASP.NET Core](xref:security/data-protection/introduction). ASP.NET Core защиты данных необходимо настроить для работы в ферме серверов. В разделе [настройки защиты данных](xref:security/data-protection/configuration/overview) для получения дополнительной информации.

Конфигурация защиты данных по умолчанию защита request подделки ASP.NET Core 

В ASP.NET Core MVC 2.0 [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) внедряет маркеров защиты от подделки для элементов формы HTML. Например приведенный ниже код в файле Razor автоматически создаст маркеров защиты от подделки:

```html
<form method="post">
  <!-- form markup -->
</form>
```

Происходит автоматическое создание маркеров защиты от подделки элементы HTML-формы при:

* `form` Тег содержит `method="post"` атрибут AND

  * Атрибут действия пуст. ( `action=""`) ИЛИ
  * Не указан атрибут действия. (`<form method="post">`)

Можно отключить автоматическое создание маркеров защиты от подделки элементов формы HTML по:

* Явное отключение `asp-antiforgery`. Пример

 ```html
  <form method="post" asp-antiforgery="false">
  </form>
  ```

* Необязательно элемент формы из вспомогательных функций тегов с помощью вспомогательного тег [! символ отказаться](xref:mvc/views/tag-helpers/intro#opt-out).

 ```html
  <!form method="post">
  </!form>
  ```

* Удалите `FormTagHelper` из представления. Можно удалить `FormTagHelper` из представления путем добавления представления Razor следующую директиву:

 ```html
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Страниц Razor](xref:mvc/razor-pages/index) автоматически защищены от XSRF-CSRF. Нет необходимости создавать дополнительный код. В разделе [XSRF/CSRF и страниц Razor](xref:mvc/razor-pages/index#xsrf) для получения дополнительной информации.

Наиболее распространенным подходом на защиту от атак CSRF является синхронизатор маркера (STP). STP – методика, когда пользователь запрашивает страницу с данными в форме. Сервер отправляет токен отмены, связанный с удостоверением текущего пользователя для клиента. Клиент отправляет маркер на сервер для проверки. Если сервер получает маркер, который не совпадает с удостоверением пользователя, прошедшего проверку подлинности, запрос отклоняется. Маркер является уникальным и непредсказуемым. Маркер может также использоваться для обеспечения правильной последовательности серию запросов (обеспечение страница 1 предшествует страница 2, которая предшествует страница 3). Все формы в ASP.NET Core MVC шаблоны создавать сложные маркеры. Следующие два примера логики представления создавать сложные токены:

```html
<form asp-controller="Manage" asp-action="ChangePassword" method="post">

</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    
}
```

Можно явно добавить сложные токен ``<form>`` элемент без использования вспомогательных функций тегов с вспомогательный метод HTML ``@Html.AntiForgeryToken``:


```html
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

Во всех перечисленных случаях ASP.NET Core добавит скрытое поле формы следующего вида:
```html
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkSldwD9CpLRyOtm6FiJB1Jr_F3FQJQDvhlHoLNJJrLA6zaMUmhjMsisu2D2tFkAiYgyWQawJk9vNm36sYP1esHOtamBEPvSk1_x--Sg8Ey2a-d9CV2zHVWIN9MVhvKHOSyKqdZFlYDVd69XYx-rOWPw3ilHGLN6K0Km-1p83jZzF0E4WU5OGg5ns2-m9Yw" />
```

ASP.NET Core включает в себя три [фильтры](xref:mvc/controllers/filters) для работы с сложные токены: ``ValidateAntiForgeryToken``, ``AutoValidateAntiforgeryToken``, и ``IgnoreAntiforgeryToken``.

<a name="vaft"></a>

### <a name="validateantiforgerytoken"></a>ValidateAntiForgeryToken

``ValidateAntiForgeryToken`` Является фильтром действий, могут применяться для отдельного действия, контроллера или глобально. Запросы, адресованные действий, имеющих этот фильтр, применяемый будут заблокированы, если запрос содержит сложные допустимый токен.

```c#
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();
    if (user != null)
    {
        var result = await _userManager.RemoveLoginAsync(user, account.LoginProvider, account.ProviderKey);
        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }
    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

``ValidateAntiForgeryToken`` Атрибута требуется маркер для запросов к методам действий, в которой относится, включая `HTTP GET` запросов. При применении широко, его можно переопределить с помощью ``IgnoreAntiforgeryToken`` атрибута.

### <a name="autovalidateantiforgerytoken"></a>AutoValidateAntiforgeryToken

Приложения ASP.NET Core обычно не создавать сложные маркерах безопасные методы HTTP (GET, HEAD, параметры и ТРАССИРОВКИ). Вместо применения широко ``ValidateAntiForgeryToken`` атрибута и затем переопределение его с ``IgnoreAntiforgeryToken`` атрибуты, можно использовать ``AutoValidateAntiforgeryToken`` атрибута. Этот атрибут работает идентично методу ``ValidateAntiForgeryToken`` атрибута, за исключением того, что он не требует токены для запросов, выполненных с помощью следующих методов HTTP:

* GET
* HEAD,
* OPTIONS
* TRACE

Рекомендуется использовать ``AutoValidateAntiforgeryToken`` широко для сценариев, отличных от API. Это гарантирует, что ваши действия POST защищены по умолчанию. Вместо этого должен игнорировать сложные токены по умолчанию, если не ``ValidateAntiForgeryToken`` применяется к методу отдельные действия. Чаще всего в этом сценарии для метода POST действие для левой незащищенными, оставляя приложение уязвимым для атак CSRF. Даже анонимного сообщения, необходимо отправить токен сложные.

Примечание: API не имеют автоматический механизм для отправки не cookie часть маркера; Реализация скорее всего будет зависеть от реализации клиентского кода. Ниже приведены некоторые примеры.


Пример (уровень класса).

```c#
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

Пример (глобальные).

```c#
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

<a name="iaft"></a>

### <a name="ignoreantiforgerytoken"></a>IgnoreAntiforgeryToken

``IgnoreAntiforgeryToken`` Фильтр используется для устранения необходимости в сложные маркера должны присутствовать для данного действия (или контроллера). При применении этого фильтра переопределит ``ValidateAntiForgeryToken`` и/или ``AutoValidateAntiforgeryToken`` фильтров, указанных на более высоком уровне (глобально или на контроллере).

```c#
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

## <a name="javascript-ajax-and-spas"></a>JavaScript, AJAX и SPAs

В традиционных приложениях на основе HTML сложные токены, передаются на сервер с помощью скрытые поля формы. В современных приложений на базе JavaScript и приложений на одной странице (SPAs) многие запросы выполняются программно. Маркер отправляется в эти запросы AJAX может использовать другие методы (например, заголовки запроса или куки-файлы). Если файлы cookie используются для хранения токенов проверки подлинности и проверки подлинности запросов API на сервере, CSRF будет потенциальных проблем. Однако при использовании локального хранилища для сохранения токена CSRF уязвимость может устранить, так как значения из локального хранилища не отправляются автоматически на сервере каждый новый запрос. Таким образом использует локальное хранилище для хранения сложные маркера на клиенте и отправка маркера как заголовок запроса является рекомендуемым.

### <a name="angularjs"></a>AngularJS

AngularJS используется соглашение по адресу CSRF. Если сервер отправляет куки-файл с именем ``XSRF-TOKEN``, угловая ``$http`` служба добавляет значение из этого файла cookie заголовком при отправке запроса на этот сервер. Этот процесс выполняется автоматически; Нет необходимости явно задавать заголовок. Имя заголовка — ``X-XSRF-TOKEN``. Сервер должен обнаружить этот заголовок и проверьте его содержимое.

Для ASP.NET Core API работают с этим соглашением.

* Настройка приложения для предоставления токена в объект``XSRF-TOKEN``
* Настройте службу сложные поиск заголовка с именем``X-XSRF-TOKEN``

```c#
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

[Просмотр результата](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).

### <a name="javascript"></a>JavaScript

С представлениями с использованием JavaScript, можно создать токен, используя службу в представлении. Чтобы сделать это, вставьте `Microsoft.AspNetCore.Antiforgery.IAntiforgery` в представлении и вызовите службу `GetAndStoreTokens`, как показано:

[!code-csharp[Main](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,24)]

Этот подход избавляет от необходимости работать напрямую с Установка с сервера файлы cookie или их считывания из клиента.

JavaScript можно также получить доступ к токенов, предоставленных в файлах cookie и использовать содержимое файла cookie для создания заголовка значения маркера, как показано ниже.

```c#
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
  new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

Затем, при условии, что создания сценария запрашивает Отправка токена в заголовок с именем ``X-CSRF-TOKEN``, настройте службу сложные искать ``X-CSRF-TOKEN`` заголовка:

```c#
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

В следующем примере jQuery AJAX-запросом соответствующий заголовок сделать:

```javascript
var csrfToken = $.cookie("CSRF-TOKEN");

$.ajax({
    url: "/api/password/changepassword",
    contentType: "application/json",
    data: JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }),
    type: "POST",
    headers: {
        "X-CSRF-TOKEN": csrfToken
    }
});
```

## <a name="configuring-antiforgery"></a>Настройка Antiforgery

`IAntiforgery`предоставляет API для настройки сложные системы. Это можно выполнить в `Configure` метод `Startup` класса. Следующий пример использует по промежуточного слоя с домашней страницы приложения для создания токена сложные и отправлять их в ответ как куки-файлы (с помощью именования по умолчанию углового описано выше):


```c#
public void Configure(IApplicationBuilder app, 
    IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;
        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // We can send the request token as a JavaScript-readable cookie, 
            // and Angular will use it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
    //
}
```

### <a name="options"></a>Параметры

Вы можете настроить [сложные параметры](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) в `ConfigureServices`:

```c#
services.AddAntiforgery(options => 
{
  options.CookieDomain = "mydomain.com";
  options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
  options.CookiePath = "Path";
  options.FormFieldName = "AntiforgeryFieldname";
  options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
  options.RequireSsl = false;
  options.SuppressXFrameOptionsHeader = false;
});
```

<!-- QAfix fix table -->

|Параметр        | Описание: |
|------------- | ----------- |
|CookieDomain  | Домен cookie. По умолчанию — `null`. |
|CookieName    | Имя файла cookie. Если не задано, система будет формировать уникальное имя которого начинается с `DefaultCookiePrefix` (». AspNetCore.Antiforgery.»). |
|CookiePath    | Путь задать в файле cookie. |
|FormFieldName | Имя скрытое поле формы используются сложные системой для подготовки к просмотру сложные токены в представлениях. |
|HeaderName    | Имя заголовка, используемые системой сложные. Если `null`, система будет рассматривать только данные формы. |
|RequireSsl    | Указывает, требуется ли SSL сложные системой. По умолчанию — `false`. Если `true`, без использования SSL запросы будут завершаться ошибкой. |
|SuppressXFrameOptionsHeader  | Указывает, следует ли подавлять поколение `X-Frame-Options` заголовок. По умолчанию заголовок создается со значением «SAMEORIGIN». По умолчанию — `false`. |

В разделе https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions Дополнительные сведения.

### <a name="extending-antiforgery"></a>Расширение Antiforgery

[IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) типа позволяет разработчикам расширять поведение системы защиты от XSRF с циклической обработки дополнительных данных в каждом маркере. [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) при каждом вызове метода создается маркер поля, и возвращаемое значение внедряется в созданный токен. Разработчик может возвращать отметку времени, nonce или любое другое значение, а затем вызвать [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) для проверки данных, если маркер проверяется. Имя пользователя клиента уже внедрена в создаваемые маркеры, поэтому нет необходимости включать эти сведения. Если токен содержит дополнительные данные, но нет `IAntiForgeryAdditionalDataProvider` был настроен, дополнительные данные не проверяются.

## <a name="fundamentals"></a>Основы

Атаки CSRF используется поведение по умолчанию браузер отправлять файлы cookie, связанные с доменом с каждый запрос к этому домену. Эти файлы Cookie хранятся в браузере. Они часто включают в себя файлы cookie сеансов для прошедших проверку пользователей. Проверка подлинности на основе файла Cookie является популярная форма проверки подлинности. Токены проверки подлинности систем росли в популярности, особенно для SPAs и других сценариев «смарт-клиент».

### <a name="cookie-based-authentication"></a>Проверка подлинности на основе файлов cookie

Как только пользователь проверку подлинности с помощью своих имени пользователя и пароль, вы выдаются ли они маркер, который может использоваться для их идентификации и проверки, что они прошли проверку подлинности. Маркер сохраняется как файл cookie, который сопровождает каждый запрос клиента. Создание и проверка этот файл cookie выполняется по промежуточного слоя проверки подлинности файла cookie. ASP.NET Core указывает куки-файл [по промежуточного слоя](../fundamentals/middleware.md) которого сериализует участника-пользователя в зашифрованном файле cookie, а затем, при последующих запросах проводится проверка куки-файл, повторно создает основной и присваивает его `User` свойство `HttpContext`.

Если используется файл cookie, файл cookie проверки подлинности является просто контейнером для билета проверки подлинности форм. Билет передается как значение файла cookie проверки подлинности форм с каждым запросом и используется проверка подлинности форм на сервере, для идентификации пользователя, прошедшего аутентификацию.

Если пользователь вошел в систему, сеанса пользователя создается на стороне сервера и хранится в базе данных или другое постоянное хранилище. Система создает ключ сеанса, который указывает фактическое сеанса в хранилище данных и отправкой в качестве файла cookie стороны клиента. Веб-сервер будет проверять этот ключ сеанса каждый раз пользователь запрашивает ресурс, который требует авторизации. Система проверяет, имеет ли сеанс пользователя прав для доступа к запрошенному ресурсу. В этом случае запроса продолжается. В противном случае — запрос возвращает как не разрешено. При таком подходе файлы cookie используются для создания приложений, которые могут быть с отслеживанием состояния, так как это «запомнить», который ранее проверка подлинности с сервером.

### <a name="user-tokens"></a>Токены пользователя

Токены проверки подлинности не хранятся в сеанс на сервере. Когда пользователь входит в систему, они выполняется выданного маркера (не сложные маркер). Этот маркер содержит данные, необходимые для проверки токена. Он также содержит сведения о пользователе в виде [утверждений](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model). Если пользователь хочет получить доступ к ресурсу сервера, проверка подлинности, маркер отправляется на сервер с заголовком дополнительная авторизация в форме {маркер} носителя. Это делает приложение без сохранения состояния, так как в каждом последующем запросе маркер передается в запросе для проверки на стороне сервера. Этот токен не *зашифрованные*; скорее *кодировке*. На стороне сервера можно декодировать токен для доступа к необработанные данные в токен. Чтобы отправить токен в последующих запросах, либо сохранить в локальном хранилище браузера или в файле cookie. Не беспокойтесь о XSRF уязвимости, если маркер сохраняется в локальном хранилище, но это представляет проблему, если маркер хранится в файле cookie.

### <a name="multiple-applications-are-hosted-in-one-domain"></a>Несколько приложений размещаются в одном домене

Несмотря на то что `example1.cloudapp.net` и `example2.cloudapp.net` разных узлах, которые есть неявные доверительные отношения между узлы на `*.cloudapp.net` домена. Неявные отношения доверия между позволяет потенциально небезопасных узлам влияет на файлы cookie друг друга (политики одного источника, которые управляют запросы AJAX не относиться к файлы cookie HTTP). Среда выполнения ASP.NET Core предоставляет некоторые по устранению рисков тем, что имя пользователя, внедряется в токен поля. Даже если вредоносный поддомен возможность перезаписывать маркер сеанса, он не может создать токен допустимого поля для пользователя. При размещении в такой среде подпрограммы встроенной защиты от XSRF по-прежнему не удается защитить от захвата сеанса или имя входа CSRF атак. Общими средами размещения, уязвимы для захвата сеанса входа CSRF и других атак.

### <a name="additional-resources"></a>Дополнительные ресурсы

* [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) на [откройте проект безопасности веб-приложения](https://www.owasp.org/index.php/Main_Page) (OWASP).
