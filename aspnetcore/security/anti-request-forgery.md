---
title: "Предотвратить межсайтовых запросов подделки XSRF-атак в ASP.NET Core"
author: steve-smith
description: "Узнайте, как предотвратить атаки, направленные на веб-приложений, где вредоносный веб-сайт может повлиять на взаимодействие между веб-браузер клиента и приложения."
manager: wpickett
ms.author: riande
ms.date: 7/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: 80651a3c3e4c722e0cb96d7cc07de366819f8d1d
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2018
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="5219a-103">Предотвратить межсайтовых запросов подделки XSRF-атак в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5219a-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="5219a-104">[Стив Смит](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5219a-104">[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-attack-does-anti-forgery-prevent"></a><span data-ttu-id="5219a-105">Какие атаки предотвращения подделки?</span><span class="sxs-lookup"><span data-stu-id="5219a-105">What attack does anti-forgery prevent?</span></span>

<span data-ttu-id="5219a-106">Подделки межсайтовых запросов (также известный как XSRF или CSRF, произносится *путешествие см. в разделе*) — это атака, для приложений веб сервере, при котором вредоносных веб-сайта могут повлиять на взаимодействие между браузером клиента и веб-сайт, которому доверяет Этот браузер.</span><span class="sxs-lookup"><span data-stu-id="5219a-106">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted applications whereby a malicious web site can influence the interaction between a client browser and a web site that trusts that browser.</span></span> <span data-ttu-id="5219a-107">Такие атаки становится возможным, так как веб-браузеры отправку некоторых типов маркеров проверки подлинности автоматически при каждом запросе веб-сайту.</span><span class="sxs-lookup"><span data-stu-id="5219a-107">These attacks are made possible because web browsers send some types of authentication tokens automatically with every request to a web site.</span></span> <span data-ttu-id="5219a-108">Уязвимости в этой форме, также известный как *атаки одним щелчком* или как *riding сеанса*, так как пользуется атаки пользователем прошедшим проверку подлинности сеанса.</span><span class="sxs-lookup"><span data-stu-id="5219a-108">This form of exploit's also known as a *one-click attack* or as *session riding*, because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="5219a-109">Примером атаки CSRF:</span><span class="sxs-lookup"><span data-stu-id="5219a-109">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="5219a-110">Пользователь выполняет вход в `www.example.com`, с помощью проверки подлинности форм.</span><span class="sxs-lookup"><span data-stu-id="5219a-110">A user logs into `www.example.com`, using forms authentication.</span></span>
2. <span data-ttu-id="5219a-111">Сервер проверяет подлинность пользователя и отправляет ответ, который включает файл cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="5219a-111">The server authenticates the user and issues a response that includes an authentication cookie.</span></span>
3. <span data-ttu-id="5219a-112">Пользователь посещает вредоносный сайт.</span><span class="sxs-lookup"><span data-stu-id="5219a-112">The user visits a malicious site.</span></span>

   <span data-ttu-id="5219a-113">Вредоносный сайт с HTML-формой следующего вида:</span><span class="sxs-lookup"><span data-stu-id="5219a-113">The malicious site contains an HTML form similar to the following:</span></span>

   ```html
   <h1>You Are a Winner!</h1>
   <form action="http://example.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click Me">
   </form>
   ```

<span data-ttu-id="5219a-114">Обратите внимание, что действие формы в блогах уязвимым сайте, не вредоносный сайт.</span><span class="sxs-lookup"><span data-stu-id="5219a-114">Notice that the form action posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="5219a-115">Это часть CSRF «между сайтами».</span><span class="sxs-lookup"><span data-stu-id="5219a-115">This is the “cross-site” part of CSRF.</span></span>

4. <span data-ttu-id="5219a-116">При нажатии кнопки "Отправить".</span><span class="sxs-lookup"><span data-stu-id="5219a-116">The user clicks the submit button.</span></span> <span data-ttu-id="5219a-117">Браузер автоматически включает файл cookie проверки подлинности для запрошенного домена (уязвимым сайта в данном случае) с запросом.</span><span class="sxs-lookup"><span data-stu-id="5219a-117">The browser automatically includes the authentication cookie for the requested domain (the vulnerable site in this case) with the request.</span></span>
5. <span data-ttu-id="5219a-118">Запрос выполняется на сервере с помощью контекста проверки подлинности пользователей и могут выполнять любые действия, прошедшего проверку подлинности пользователя разрешено делать.</span><span class="sxs-lookup"><span data-stu-id="5219a-118">The request runs on the server with the user's authentication context and can perform any action that an authenticated user is allowed to do.</span></span>

<span data-ttu-id="5219a-119">В этом примере требуется пользователь, нажав кнопку формы.</span><span class="sxs-lookup"><span data-stu-id="5219a-119">This example requires the user to click the form button.</span></span> <span data-ttu-id="5219a-120">Удалось вредоносную страницу:</span><span class="sxs-lookup"><span data-stu-id="5219a-120">The malicious page could:</span></span>

* <span data-ttu-id="5219a-121">Запустите скрипт, который автоматически отправляет форму.</span><span class="sxs-lookup"><span data-stu-id="5219a-121">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="5219a-122">Отправляет отправки формы в качестве AJAX-запросом.</span><span class="sxs-lookup"><span data-stu-id="5219a-122">Sends a form submission as an AJAX request.</span></span> 
* <span data-ttu-id="5219a-123">С помощью CSS с помощью скрытого формы.</span><span class="sxs-lookup"><span data-stu-id="5219a-123">Use a hidden form with CSS.</span></span> 

<span data-ttu-id="5219a-124">С помощью протокола SSL не предотвращает атаки CSRF, можно отправить вредоносный сайт `https://` запроса.</span><span class="sxs-lookup"><span data-stu-id="5219a-124">Using SSL doesn't prevent a CSRF attack, the malicious site can send an `https://` request.</span></span> 

<span data-ttu-id="5219a-125">Некоторые атаки целевого сайта конечных точек, которые отвечают на `GET` запросы, в которых регистр тег изображения можно использовать для выполнения действия (эту форму атаки проявляется на форуме сайтов, которые обеспечивают изображения, но блокировать JavaScript).</span><span class="sxs-lookup"><span data-stu-id="5219a-125">Some attacks target site endpoints that respond to `GET` requests, in which case an image tag can be used to perform the action (this form of attack is common on forum sites that permit images but block JavaScript).</span></span> <span data-ttu-id="5219a-126">Приложения, которые изменяют состояние с `GET` уязвимы запросов от атак злоумышленников.</span><span class="sxs-lookup"><span data-stu-id="5219a-126">Applications that change state with `GET` requests are vulnerable from malicious attacks.</span></span>

<span data-ttu-id="5219a-127">Атаки CSRF возможны с веб-сайтов, которые используют файлы cookie для проверки подлинности, так как браузеры отправляют все соответствующие файлы cookie в веб-узел назначения.</span><span class="sxs-lookup"><span data-stu-id="5219a-127">CSRF attacks are possible against web sites that use cookies for authentication, because browsers send all relevant cookies to the destination web site.</span></span> <span data-ttu-id="5219a-128">Тем не менее атаки CSRF ограничены не использовать файлы cookie.</span><span class="sxs-lookup"><span data-stu-id="5219a-128">However, CSRF attacks are not limited to exploiting cookies.</span></span> <span data-ttu-id="5219a-129">Например Basic и дайджест-проверки подлинности также уязвимы.</span><span class="sxs-lookup"><span data-stu-id="5219a-129">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="5219a-130">После входа пользователя в систему с обычная или краткая проверка подлинности браузер автоматически отправляет учетные данные до завершения сеанса.</span><span class="sxs-lookup"><span data-stu-id="5219a-130">After a user logs in with Basic or Digest authentication, the browser automatically sends the credentials until the session ends.</span></span>

<span data-ttu-id="5219a-131">Примечание: В этом контексте *сеанса* ссылается на клиентский сеанс, во время которого пользователь проходит проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="5219a-131">Note: In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="5219a-132">Это не связанные сеансы на стороне сервера или [сеанса по промежуточного слоя](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="5219a-132">It's unrelated to server-side sessions or [session middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="5219a-133">Пользователям организовать защиту от уязвимостей CSRF с:</span><span class="sxs-lookup"><span data-stu-id="5219a-133">Users can guard against CSRF vulnerabilities by:</span></span>
* <span data-ttu-id="5219a-134">Ведение журнала из веб-сайтов, завершении работы с их помощью.</span><span class="sxs-lookup"><span data-stu-id="5219a-134">Logging off of web sites when they have finished using them.</span></span>
* <span data-ttu-id="5219a-135">Периодически очистить файлы cookie браузера.</span><span class="sxs-lookup"><span data-stu-id="5219a-135">Clearing their browser's cookies periodically.</span></span>

<span data-ttu-id="5219a-136">Тем не менее уязвимости CSRF фактически являются проблемы с веб-приложения, а не конечным пользователем.</span><span class="sxs-lookup"><span data-stu-id="5219a-136">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="how-does-aspnet-core-mvc-address-csrf"></a><span data-ttu-id="5219a-137">Как ASP.NET Core MVC решать CSRF</span><span class="sxs-lookup"><span data-stu-id="5219a-137">How does ASP.NET Core MVC address CSRF?</span></span>

> [!WARNING]
> <span data-ttu-id="5219a-138">ASP.NET Core реализуется с помощью приложения для защиты request подделки [стека защиты данных ASP.NET Core](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="5219a-138">ASP.NET Core implements anti-request-forgery using the [ASP.NET Core data protection stack](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="5219a-139">ASP.NET Core защиты данных необходимо настроить для работы в ферме серверов.</span><span class="sxs-lookup"><span data-stu-id="5219a-139">ASP.NET Core data protection must be configured to work in a server farm.</span></span> <span data-ttu-id="5219a-140">В разделе [настройки защиты данных](xref:security/data-protection/configuration/overview) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="5219a-140">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="5219a-141">Конфигурация защиты данных по умолчанию защита request подделки ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5219a-141">ASP.NET Core anti-request-forgery default data protection configuration</span></span> 

<span data-ttu-id="5219a-142">В ASP.NET Core MVC 2.0 [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) внедряет маркеров защиты от подделки для элементов формы HTML.</span><span class="sxs-lookup"><span data-stu-id="5219a-142">In ASP.NET Core MVC 2.0 the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects anti-forgery tokens for HTML form elements.</span></span> <span data-ttu-id="5219a-143">Например приведенный ниже код в файле Razor автоматически создаст маркеров защиты от подделки:</span><span class="sxs-lookup"><span data-stu-id="5219a-143">For example, the following markup in a Razor file will automatically generate anti-forgery tokens:</span></span>

```html
<form method="post">
  <!-- form markup -->
</form>
```

<span data-ttu-id="5219a-144">Происходит автоматическое создание маркеров защиты от подделки элементы HTML-формы при:</span><span class="sxs-lookup"><span data-stu-id="5219a-144">The automatic generation of anti-forgery tokens for HTML form elements happens when:</span></span>

* <span data-ttu-id="5219a-145">`form` Тег содержит `method="post"` атрибут AND</span><span class="sxs-lookup"><span data-stu-id="5219a-145">The `form` tag contains the `method="post"` attribute AND</span></span>

  * <span data-ttu-id="5219a-146">Атрибут действия пуст.</span><span class="sxs-lookup"><span data-stu-id="5219a-146">The action attribute is empty.</span></span> <span data-ttu-id="5219a-147">( `action=""`) ИЛИ</span><span class="sxs-lookup"><span data-stu-id="5219a-147">( `action=""`) OR</span></span>
  * <span data-ttu-id="5219a-148">Не указан атрибут действия.</span><span class="sxs-lookup"><span data-stu-id="5219a-148">The action attribute isn't supplied.</span></span> <span data-ttu-id="5219a-149">(`<form method="post">`)</span><span class="sxs-lookup"><span data-stu-id="5219a-149">(`<form method="post">`)</span></span>

<span data-ttu-id="5219a-150">Можно отключить автоматическое создание маркеров защиты от подделки элементов формы HTML по:</span><span class="sxs-lookup"><span data-stu-id="5219a-150">You can disable automatic generation of anti-forgery tokens for HTML form elements by:</span></span>

* <span data-ttu-id="5219a-151">Явное отключение `asp-antiforgery`.</span><span class="sxs-lookup"><span data-stu-id="5219a-151">Explicitly disabling `asp-antiforgery`.</span></span> <span data-ttu-id="5219a-152">Пример</span><span class="sxs-lookup"><span data-stu-id="5219a-152">For example</span></span>

  ```html
  <form method="post" asp-antiforgery="false">
  </form>
  ```

* <span data-ttu-id="5219a-153">Необязательно элемент формы из вспомогательных функций тегов с помощью вспомогательного тег [! символ отказаться](xref:mvc/views/tag-helpers/intro#opt-out).</span><span class="sxs-lookup"><span data-stu-id="5219a-153">Opt the form element out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out).</span></span>

  ```html
  <!form method="post">
  </!form>
  ```

* <span data-ttu-id="5219a-154">Удалите `FormTagHelper` из представления.</span><span class="sxs-lookup"><span data-stu-id="5219a-154">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="5219a-155">Можно удалить `FormTagHelper` из представления путем добавления представления Razor следующую директиву:</span><span class="sxs-lookup"><span data-stu-id="5219a-155">You can remove the `FormTagHelper` from a view by adding the following directive to the Razor view:</span></span>

  ```html
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="5219a-156">[Страниц Razor](xref:mvc/razor-pages/index) автоматически защищены от XSRF-CSRF.</span><span class="sxs-lookup"><span data-stu-id="5219a-156">[Razor Pages](xref:mvc/razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="5219a-157">Нет необходимости создавать дополнительный код.</span><span class="sxs-lookup"><span data-stu-id="5219a-157">You don't have to write any additional code.</span></span> <span data-ttu-id="5219a-158">В разделе [XSRF/CSRF и страниц Razor](xref:mvc/razor-pages/index#xsrf) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="5219a-158">See [XSRF/CSRF and Razor Pages](xref:mvc/razor-pages/index#xsrf) for more information.</span></span>

<span data-ttu-id="5219a-159">Наиболее распространенным подходом на защиту от атак CSRF является синхронизатор маркера (STP).</span><span class="sxs-lookup"><span data-stu-id="5219a-159">The most common approach to defending against CSRF attacks is the synchronizer token pattern (STP).</span></span> <span data-ttu-id="5219a-160">STP – методика, когда пользователь запрашивает страницу с данными в форме.</span><span class="sxs-lookup"><span data-stu-id="5219a-160">STP is a technique used when the user requests a page with form data.</span></span> <span data-ttu-id="5219a-161">Сервер отправляет токен отмены, связанный с удостоверением текущего пользователя для клиента.</span><span class="sxs-lookup"><span data-stu-id="5219a-161">The server sends a token associated with the current user's identity to the client.</span></span> <span data-ttu-id="5219a-162">Клиент отправляет маркер на сервер для проверки.</span><span class="sxs-lookup"><span data-stu-id="5219a-162">The client sends back the token to the server for verification.</span></span> <span data-ttu-id="5219a-163">Если сервер получает маркер, который не совпадает с удостоверением пользователя, прошедшего проверку подлинности, запрос отклоняется.</span><span class="sxs-lookup"><span data-stu-id="5219a-163">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span> <span data-ttu-id="5219a-164">Маркер является уникальным и непредсказуемым.</span><span class="sxs-lookup"><span data-stu-id="5219a-164">The token is unique and unpredictable.</span></span> <span data-ttu-id="5219a-165">Маркер может также использоваться для обеспечения правильной последовательности серию запросов (обеспечение страница 1 предшествует страница 2, которая предшествует страница 3).</span><span class="sxs-lookup"><span data-stu-id="5219a-165">The token can also be used to ensure proper sequencing of a series of requests (ensuring page 1 precedes page 2 which precedes page 3).</span></span> <span data-ttu-id="5219a-166">Все формы в ASP.NET Core MVC шаблоны создавать сложные маркеры.</span><span class="sxs-lookup"><span data-stu-id="5219a-166">All the forms in ASP.NET Core MVC templates generate antiforgery tokens.</span></span> <span data-ttu-id="5219a-167">Следующие два примера логики представления создавать сложные токены:</span><span class="sxs-lookup"><span data-stu-id="5219a-167">The following two examples of view logic generate antiforgery tokens:</span></span>

```html
<form asp-controller="Manage" asp-action="ChangePassword" method="post">

</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    
}
```

<span data-ttu-id="5219a-168">Можно явно добавить сложные токен `<form>` элемент без использования вспомогательных функций тегов с вспомогательный метод HTML `@Html.AntiForgeryToken`:</span><span class="sxs-lookup"><span data-stu-id="5219a-168">You can explicitly add an antiforgery token to a `<form>` element without using tag helpers with the HTML helper `@Html.AntiForgeryToken`:</span></span>


```html
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="5219a-169">Во всех перечисленных случаях ASP.NET Core добавит скрытое поле формы следующего вида:</span><span class="sxs-lookup"><span data-stu-id="5219a-169">In each of the preceding cases, ASP.NET Core will add a hidden form field similar to the following:</span></span>
```html
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkSldwD9CpLRyOtm6FiJB1Jr_F3FQJQDvhlHoLNJJrLA6zaMUmhjMsisu2D2tFkAiYgyWQawJk9vNm36sYP1esHOtamBEPvSk1_x--Sg8Ey2a-d9CV2zHVWIN9MVhvKHOSyKqdZFlYDVd69XYx-rOWPw3ilHGLN6K0Km-1p83jZzF0E4WU5OGg5ns2-m9Yw">
```

<span data-ttu-id="5219a-170">ASP.NET Core включает в себя три [фильтры](xref:mvc/controllers/filters) для работы с сложные токены: `ValidateAntiForgeryToken`, `AutoValidateAntiforgeryToken`, и `IgnoreAntiforgeryToken`.</span><span class="sxs-lookup"><span data-stu-id="5219a-170">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens: `ValidateAntiForgeryToken`, `AutoValidateAntiforgeryToken`, and `IgnoreAntiforgeryToken`.</span></span>

### <a name="validateantiforgerytoken"></a><span data-ttu-id="5219a-171">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="5219a-171">ValidateAntiForgeryToken</span></span>

<span data-ttu-id="5219a-172">`ValidateAntiForgeryToken` Является фильтром действий, могут применяться для отдельного действия, контроллера или глобально.</span><span class="sxs-lookup"><span data-stu-id="5219a-172">The `ValidateAntiForgeryToken` is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="5219a-173">Запросы, адресованные действий, имеющих этот фильтр, применяемый будут заблокированы, если запрос содержит сложные допустимый токен.</span><span class="sxs-lookup"><span data-stu-id="5219a-173">Requests made to actions that have this filter applied will be blocked unless the request includes a valid antiforgery token.</span></span>

```csharp
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

<span data-ttu-id="5219a-174">`ValidateAntiForgeryToken` Атрибута требуется маркер для запросов к методам действий, в которой относится, включая `HTTP GET` запросов.</span><span class="sxs-lookup"><span data-stu-id="5219a-174">The `ValidateAntiForgeryToken` attribute requires a token for requests to action methods it decorates, including `HTTP GET` requests.</span></span> <span data-ttu-id="5219a-175">При применении широко, его можно переопределить с помощью `IgnoreAntiforgeryToken` атрибута.</span><span class="sxs-lookup"><span data-stu-id="5219a-175">If you apply it broadly, you can override it with the `IgnoreAntiforgeryToken` attribute.</span></span>

### <a name="autovalidateantiforgerytoken"></a><span data-ttu-id="5219a-176">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="5219a-176">AutoValidateAntiforgeryToken</span></span>

<span data-ttu-id="5219a-177">Приложения ASP.NET Core обычно не создавать сложные маркерах безопасные методы HTTP (GET, HEAD, параметры и ТРАССИРОВКИ).</span><span class="sxs-lookup"><span data-stu-id="5219a-177">ASP.NET Core apps generally don't generate antiforgery tokens for HTTP safe methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="5219a-178">Вместо применения широко `ValidateAntiForgeryToken` атрибута и затем переопределение его с `IgnoreAntiforgeryToken` атрибуты, можно использовать ``AutoValidateAntiforgeryToken`` атрибута.</span><span class="sxs-lookup"><span data-stu-id="5219a-178">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, you can use the ``AutoValidateAntiforgeryToken`` attribute.</span></span> <span data-ttu-id="5219a-179">Этот атрибут работает идентично методу `ValidateAntiForgeryToken` атрибута, за исключением того, что он не требует токены для запросов, выполненных с помощью следующих методов HTTP:</span><span class="sxs-lookup"><span data-stu-id="5219a-179">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="5219a-180">GET</span><span class="sxs-lookup"><span data-stu-id="5219a-180">GET</span></span>
* <span data-ttu-id="5219a-181">HEAD,</span><span class="sxs-lookup"><span data-stu-id="5219a-181">HEAD</span></span>
* <span data-ttu-id="5219a-182">OPTIONS</span><span class="sxs-lookup"><span data-stu-id="5219a-182">OPTIONS</span></span>
* <span data-ttu-id="5219a-183">TRACE</span><span class="sxs-lookup"><span data-stu-id="5219a-183">TRACE</span></span>

<span data-ttu-id="5219a-184">Рекомендуется использовать `AutoValidateAntiforgeryToken` широко для сценариев, отличных от API.</span><span class="sxs-lookup"><span data-stu-id="5219a-184">We recommend you use `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="5219a-185">Это гарантирует, что ваши действия POST защищены по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5219a-185">This ensures your POST actions are protected by default.</span></span> <span data-ttu-id="5219a-186">Вместо этого должен игнорировать сложные токены по умолчанию, если не `ValidateAntiForgeryToken` применяется к методу отдельные действия.</span><span class="sxs-lookup"><span data-stu-id="5219a-186">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to the individual action method.</span></span> <span data-ttu-id="5219a-187">Чаще всего в этом сценарии для метода POST действие для левой незащищенными, оставляя приложение уязвимым для атак CSRF.</span><span class="sxs-lookup"><span data-stu-id="5219a-187">It's more likely in this scenario for a POST action method to be left unprotected, leaving your app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="5219a-188">Даже анонимного сообщения, необходимо отправить токен сложные.</span><span class="sxs-lookup"><span data-stu-id="5219a-188">Even anonymous POSTS should send the antiforgery token.</span></span>

<span data-ttu-id="5219a-189">Примечание: API не имеют автоматический механизм для отправки не cookie часть маркера; Реализация скорее всего будет зависеть от реализации клиентского кода.</span><span class="sxs-lookup"><span data-stu-id="5219a-189">Note: APIs don't have an automatic mechanism for sending the non-cookie part of the token; your implementation will likely depend on your client code implementation.</span></span> <span data-ttu-id="5219a-190">Ниже приведены некоторые примеры.</span><span class="sxs-lookup"><span data-stu-id="5219a-190">Some examples are shown below.</span></span>

<span data-ttu-id="5219a-191">Пример (уровень класса).</span><span class="sxs-lookup"><span data-stu-id="5219a-191">Example (class level):</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="5219a-192">Пример (глобальные).</span><span class="sxs-lookup"><span data-stu-id="5219a-192">Example (global):</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

<a name="iaft"></a>

### <a name="ignoreantiforgerytoken"></a><span data-ttu-id="5219a-193">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="5219a-193">IgnoreAntiforgeryToken</span></span>

<span data-ttu-id="5219a-194">`IgnoreAntiforgeryToken` Фильтр используется для устранения необходимости в сложные маркера должны присутствовать для данного действия (или контроллера).</span><span class="sxs-lookup"><span data-stu-id="5219a-194">The `IgnoreAntiforgeryToken` filter is used to eliminate the need for an antiforgery token to be present for a given action (or controller).</span></span> <span data-ttu-id="5219a-195">При применении этого фильтра переопределит `ValidateAntiForgeryToken` и/или `AutoValidateAntiforgeryToken` фильтров, указанных на более высоком уровне (глобально или на контроллере).</span><span class="sxs-lookup"><span data-stu-id="5219a-195">When applied, this filter will override `ValidateAntiForgeryToken` and/or `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

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

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="5219a-196">JavaScript, AJAX и SPAs</span><span class="sxs-lookup"><span data-stu-id="5219a-196">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="5219a-197">В традиционных приложениях на основе HTML сложные токены, передаются на сервер с помощью скрытые поля формы.</span><span class="sxs-lookup"><span data-stu-id="5219a-197">In traditional HTML-based applications, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="5219a-198">В современных приложений на базе JavaScript и приложений на одной странице (SPAs) многие запросы выполняются программно.</span><span class="sxs-lookup"><span data-stu-id="5219a-198">In modern JavaScript-based apps and single page applications (SPAs), many requests are made programmatically.</span></span> <span data-ttu-id="5219a-199">Маркер отправляется в эти запросы AJAX может использовать другие методы (например, заголовки запроса или куки-файлы).</span><span class="sxs-lookup"><span data-stu-id="5219a-199">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span> <span data-ttu-id="5219a-200">Если файлы cookie используются для хранения токенов проверки подлинности и проверки подлинности запросов API на сервере, CSRF будет потенциальных проблем.</span><span class="sxs-lookup"><span data-stu-id="5219a-200">If cookies are used to store authentication tokens and to authenticate API requests on the server, then CSRF will be a potential problem.</span></span> <span data-ttu-id="5219a-201">Однако при использовании локального хранилища для сохранения токена CSRF уязвимость может устранить, так как значения из локального хранилища не отправляются автоматически на сервере каждый новый запрос.</span><span class="sxs-lookup"><span data-stu-id="5219a-201">However, if local storage is used to store the token, CSRF vulnerability may be mitigated, since values from local storage are not sent automatically to the server with every new request.</span></span> <span data-ttu-id="5219a-202">Таким образом использует локальное хранилище для хранения сложные маркера на клиенте и отправка маркера как заголовок запроса является рекомендуемым.</span><span class="sxs-lookup"><span data-stu-id="5219a-202">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="angularjs"></a><span data-ttu-id="5219a-203">AngularJS</span><span class="sxs-lookup"><span data-stu-id="5219a-203">AngularJS</span></span>

<span data-ttu-id="5219a-204">AngularJS используется соглашение по адресу CSRF.</span><span class="sxs-lookup"><span data-stu-id="5219a-204">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="5219a-205">Если сервер отправляет куки-файл с именем `XSRF-TOKEN`, угловая `$http` служба добавляет значение из этого файла cookie заголовком при отправке запроса на этот сервер.</span><span class="sxs-lookup"><span data-stu-id="5219a-205">If the server sends a cookie with the name `XSRF-TOKEN`, the Angular `$http` service will add the value from this cookie to a header when it sends a request to this server.</span></span> <span data-ttu-id="5219a-206">Этот процесс выполняется автоматически; Нет необходимости явно задавать заголовок.</span><span class="sxs-lookup"><span data-stu-id="5219a-206">This process is automatic; you don't need to set the header explicitly.</span></span> <span data-ttu-id="5219a-207">Имя заголовка — `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="5219a-207">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="5219a-208">Сервер должен обнаружить этот заголовок и проверьте его содержимое.</span><span class="sxs-lookup"><span data-stu-id="5219a-208">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="5219a-209">Для ASP.NET Core API работают с этим соглашением.</span><span class="sxs-lookup"><span data-stu-id="5219a-209">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="5219a-210">Настройка приложения для предоставления токена в объект `XSRF-TOKEN`</span><span class="sxs-lookup"><span data-stu-id="5219a-210">Configure your app to provide a token in a cookie called `XSRF-TOKEN`</span></span>
* <span data-ttu-id="5219a-211">Настройте службу сложные поиск заголовка с именем `X-XSRF-TOKEN`</span><span class="sxs-lookup"><span data-stu-id="5219a-211">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="5219a-212">[Просмотр результата](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span><span class="sxs-lookup"><span data-stu-id="5219a-212">[View sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span></span>

### <a name="javascript"></a><span data-ttu-id="5219a-213">JavaScript</span><span class="sxs-lookup"><span data-stu-id="5219a-213">JavaScript</span></span>

<span data-ttu-id="5219a-214">С представлениями с использованием JavaScript, можно создать токен, используя службу в представлении.</span><span class="sxs-lookup"><span data-stu-id="5219a-214">Using JavaScript with views, you can create the token using a service from within your view.</span></span> <span data-ttu-id="5219a-215">Чтобы сделать это, вставьте `Microsoft.AspNetCore.Antiforgery.IAntiforgery` в представлении и вызовите службу `GetAndStoreTokens`, как показано:</span><span class="sxs-lookup"><span data-stu-id="5219a-215">To do so, you inject the `Microsoft.AspNetCore.Antiforgery.IAntiforgery` service into the view and call `GetAndStoreTokens`, as shown:</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,28)]

<span data-ttu-id="5219a-216">Этот подход избавляет от необходимости работать напрямую с Установка с сервера файлы cookie или их считывания из клиента.</span><span class="sxs-lookup"><span data-stu-id="5219a-216">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="5219a-217">В предыдущем примере использовался jQuery считать значение скрытого поля заголовка AJAX POST.</span><span class="sxs-lookup"><span data-stu-id="5219a-217">The preceding example uses jQuery to read the hidden field value for the AJAX POST header.</span></span> <span data-ttu-id="5219a-218">Чтобы использовать JavaScript для получения маркера значения, используйте `document.getElementById('RequestVerificationToken').value`.</span><span class="sxs-lookup"><span data-stu-id="5219a-218">To use JavaScript to obtain the token's value, use `document.getElementById('RequestVerificationToken').value`.</span></span>

<span data-ttu-id="5219a-219">JavaScript можно также получить доступ к токенов, предоставленных в файлах cookie и использовать содержимое файла cookie для создания заголовка значения маркера, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="5219a-219">JavaScript can also access tokens provided in cookies, and then use the cookie's contents to create a header with the token's value, as shown below.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
  new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="5219a-220">Затем, при условии, что создания сценария запрашивает Отправка токена в заголовок с именем `X-CSRF-TOKEN`, настройте службу сложные искать `X-CSRF-TOKEN` заголовка:</span><span class="sxs-lookup"><span data-stu-id="5219a-220">Then, assuming you construct your script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="5219a-221">В следующем примере jQuery AJAX-запросом соответствующий заголовок сделать:</span><span class="sxs-lookup"><span data-stu-id="5219a-221">The following example uses jQuery to make an AJAX request with the appropriate header:</span></span>

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

## <a name="configuring-antiforgery"></a><span data-ttu-id="5219a-222">Настройка Antiforgery</span><span class="sxs-lookup"><span data-stu-id="5219a-222">Configuring Antiforgery</span></span>

<span data-ttu-id="5219a-223">`IAntiforgery` предоставляет API для настройки сложные системы.</span><span class="sxs-lookup"><span data-stu-id="5219a-223">`IAntiforgery` provides the API to configure the antiforgery system.</span></span> <span data-ttu-id="5219a-224">Это можно выполнить в `Configure` метод `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="5219a-224">It can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="5219a-225">Следующий пример использует по промежуточного слоя с домашней страницы приложения для создания токена сложные и отправлять их в ответ как куки-файлы (с помощью именования по умолчанию углового описано выше):</span><span class="sxs-lookup"><span data-stu-id="5219a-225">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described above):</span></span>


```csharp
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

### <a name="options"></a><span data-ttu-id="5219a-226">Параметры</span><span class="sxs-lookup"><span data-stu-id="5219a-226">Options</span></span>

<span data-ttu-id="5219a-227">Вы можете настроить [сложные параметры](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5219a-227">You can customize [antiforgery options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) in `ConfigureServices`:</span></span>

```csharp
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

|<span data-ttu-id="5219a-228">Параметр</span><span class="sxs-lookup"><span data-stu-id="5219a-228">Option</span></span>        | <span data-ttu-id="5219a-229">Описание:</span><span class="sxs-lookup"><span data-stu-id="5219a-229">Description</span></span> |
|------------- | ----------- |
|<span data-ttu-id="5219a-230">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="5219a-230">CookieDomain</span></span>  | <span data-ttu-id="5219a-231">Домен cookie.</span><span class="sxs-lookup"><span data-stu-id="5219a-231">The domain of the cookie.</span></span> <span data-ttu-id="5219a-232">По умолчанию — `null`.</span><span class="sxs-lookup"><span data-stu-id="5219a-232">Defaults to `null`.</span></span> |
|<span data-ttu-id="5219a-233">CookieName</span><span class="sxs-lookup"><span data-stu-id="5219a-233">CookieName</span></span>    | <span data-ttu-id="5219a-234">Имя файла cookie.</span><span class="sxs-lookup"><span data-stu-id="5219a-234">The name of the cookie.</span></span> <span data-ttu-id="5219a-235">Если не задано, система будет формировать уникальное имя которого начинается с `DefaultCookiePrefix` (». AspNetCore.Antiforgery.»).</span><span class="sxs-lookup"><span data-stu-id="5219a-235">If not set, the system will generate a unique name beginning with the `DefaultCookiePrefix` (".AspNetCore.Antiforgery.").</span></span> |
|<span data-ttu-id="5219a-236">CookiePath</span><span class="sxs-lookup"><span data-stu-id="5219a-236">CookiePath</span></span>    | <span data-ttu-id="5219a-237">Путь задать в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="5219a-237">The path set on the cookie.</span></span> |
|<span data-ttu-id="5219a-238">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="5219a-238">FormFieldName</span></span> | <span data-ttu-id="5219a-239">Имя скрытое поле формы используются сложные системой для подготовки к просмотру сложные токены в представлениях.</span><span class="sxs-lookup"><span data-stu-id="5219a-239">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
|<span data-ttu-id="5219a-240">HeaderName</span><span class="sxs-lookup"><span data-stu-id="5219a-240">HeaderName</span></span>    | <span data-ttu-id="5219a-241">Имя заголовка, используемые системой сложные.</span><span class="sxs-lookup"><span data-stu-id="5219a-241">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="5219a-242">Если `null`, система будет рассматривать только данные формы.</span><span class="sxs-lookup"><span data-stu-id="5219a-242">If `null`, the system will consider only form data.</span></span> |
|<span data-ttu-id="5219a-243">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="5219a-243">RequireSsl</span></span>    | <span data-ttu-id="5219a-244">Указывает, требуется ли SSL сложные системой.</span><span class="sxs-lookup"><span data-stu-id="5219a-244">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="5219a-245">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="5219a-245">Defaults to `false`.</span></span> <span data-ttu-id="5219a-246">Если `true`, без использования SSL запросы будут завершаться ошибкой.</span><span class="sxs-lookup"><span data-stu-id="5219a-246">If `true`, non-SSL requests will fail.</span></span> |
|<span data-ttu-id="5219a-247">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="5219a-247">SuppressXFrameOptionsHeader</span></span> | <span data-ttu-id="5219a-248">Указывает, следует ли подавлять поколение `X-Frame-Options` заголовок.</span><span class="sxs-lookup"><span data-stu-id="5219a-248">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="5219a-249">По умолчанию заголовок создается со значением «SAMEORIGIN».</span><span class="sxs-lookup"><span data-stu-id="5219a-249">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="5219a-250">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="5219a-250">Defaults to `false`.</span></span> |

<span data-ttu-id="5219a-251">В разделе https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="5219a-251">See https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions for more info.</span></span>

### <a name="extending-antiforgery"></a><span data-ttu-id="5219a-252">Расширение Antiforgery</span><span class="sxs-lookup"><span data-stu-id="5219a-252">Extending Antiforgery</span></span>

<span data-ttu-id="5219a-253">[IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) типа позволяет разработчикам расширять поведение системы защиты от XSRF с циклической обработки дополнительных данных в каждом маркере.</span><span class="sxs-lookup"><span data-stu-id="5219a-253">The [IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-XSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="5219a-254">[GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) при каждом вызове метода создается маркер поля, и возвращаемое значение внедряется в созданный токен.</span><span class="sxs-lookup"><span data-stu-id="5219a-254">The [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="5219a-255">Разработчик может возвращать отметку времени, nonce или любое другое значение, а затем вызвать [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) для проверки данных, если маркер проверяется.</span><span class="sxs-lookup"><span data-stu-id="5219a-255">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) to validate this data when the token is validated.</span></span> <span data-ttu-id="5219a-256">Имя пользователя клиента уже внедрена в создаваемые маркеры, поэтому нет необходимости включать эти сведения.</span><span class="sxs-lookup"><span data-stu-id="5219a-256">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="5219a-257">Если токен содержит дополнительные данные, но нет `IAntiForgeryAdditionalDataProvider` был настроен, дополнительные данные не проверяются.</span><span class="sxs-lookup"><span data-stu-id="5219a-257">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` has been configured, the supplemental data isn't validated.</span></span>

## <a name="fundamentals"></a><span data-ttu-id="5219a-258">Основы</span><span class="sxs-lookup"><span data-stu-id="5219a-258">Fundamentals</span></span>

<span data-ttu-id="5219a-259">Атаки CSRF используется поведение по умолчанию браузер отправлять файлы cookie, связанные с доменом с каждый запрос к этому домену.</span><span class="sxs-lookup"><span data-stu-id="5219a-259">CSRF attacks rely on the default browser behavior of sending cookies associated with a domain with every request made to that domain.</span></span> <span data-ttu-id="5219a-260">Эти файлы Cookie хранятся в браузере.</span><span class="sxs-lookup"><span data-stu-id="5219a-260">These cookies are stored within the browser.</span></span> <span data-ttu-id="5219a-261">Они часто включают в себя файлы cookie сеансов для прошедших проверку пользователей.</span><span class="sxs-lookup"><span data-stu-id="5219a-261">They frequently include session cookies for authenticated users.</span></span> <span data-ttu-id="5219a-262">Проверка подлинности на основе файла Cookie является популярная форма проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="5219a-262">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="5219a-263">Токены проверки подлинности систем росли в популярности, особенно для SPAs и других сценариев «смарт-клиент».</span><span class="sxs-lookup"><span data-stu-id="5219a-263">Token-based authentication systems have been growing in popularity, especially for SPAs and other "smart client" scenarios.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="5219a-264">Проверка подлинности на основе файлов cookie</span><span class="sxs-lookup"><span data-stu-id="5219a-264">Cookie-based authentication</span></span>

<span data-ttu-id="5219a-265">Как только пользователь проверку подлинности с помощью своих имени пользователя и пароль, вы выдаются ли они маркер, который может использоваться для их идентификации и проверки, что они прошли проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="5219a-265">Once a user has authenticated using their username and password, they're issued a token that can be used to identify them and validate that they have been authenticated.</span></span> <span data-ttu-id="5219a-266">Маркер сохраняется как файл cookie, который сопровождает каждый запрос клиента.</span><span class="sxs-lookup"><span data-stu-id="5219a-266">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="5219a-267">Создание и проверка этот файл cookie выполняется по промежуточного слоя проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="5219a-267">Generating and validating this cookie is done by the cookie authentication middleware.</span></span> <span data-ttu-id="5219a-268">ASP.NET Core указывает куки-файл [по промежуточного слоя](xref:fundamentals/middleware/index) которого сериализует участника-пользователя в зашифрованном файле cookie, а затем, при последующих запросах проводится проверка куки-файл, повторно создает основной и присваивает его `User` свойство `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="5219a-268">ASP.NET Core provides cookie [middleware](xref:fundamentals/middleware/index) which serializes a user principal into an encrypted cookie and then, on subsequent requests, validates the cookie, recreates the principal and assigns it to the `User` property on `HttpContext`.</span></span>

<span data-ttu-id="5219a-269">Если используется файл cookie, файл cookie проверки подлинности является просто контейнером для билета проверки подлинности форм.</span><span class="sxs-lookup"><span data-stu-id="5219a-269">When a cookie is used, The authentication cookie is just a container for the forms authentication ticket.</span></span> <span data-ttu-id="5219a-270">Билет передается как значение файла cookie проверки подлинности форм с каждым запросом и используется проверка подлинности форм на сервере, для идентификации пользователя, прошедшего аутентификацию.</span><span class="sxs-lookup"><span data-stu-id="5219a-270">The ticket is passed as the value of the forms authentication cookie with each request and is used by forms authentication, on the server, to identify an authenticated user.</span></span>

<span data-ttu-id="5219a-271">Если пользователь вошел в систему, сеанса пользователя создается на стороне сервера и хранится в базе данных или другое постоянное хранилище.</span><span class="sxs-lookup"><span data-stu-id="5219a-271">When a user is logged in to a system, a user session is created on the server-side and is stored in a database or some other persistent store.</span></span> <span data-ttu-id="5219a-272">Система создает ключ сеанса, который указывает фактическое сеанса в хранилище данных и отправкой в качестве файла cookie стороны клиента.</span><span class="sxs-lookup"><span data-stu-id="5219a-272">The system generates a session key that points to the actual session in the data store and it's sent as a client side cookie.</span></span> <span data-ttu-id="5219a-273">Веб-сервер будет проверять этот ключ сеанса каждый раз пользователь запрашивает ресурс, который требует авторизации.</span><span class="sxs-lookup"><span data-stu-id="5219a-273">The web server will check this session key any time a user requests a resource that requires authorization.</span></span> <span data-ttu-id="5219a-274">Система проверяет, имеет ли сеанс пользователя прав для доступа к запрошенному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="5219a-274">The system checks whether the associated user session has the privilege to access the requested resource.</span></span> <span data-ttu-id="5219a-275">В этом случае запроса продолжается.</span><span class="sxs-lookup"><span data-stu-id="5219a-275">If so, the request continues.</span></span> <span data-ttu-id="5219a-276">В противном случае — запрос возвращает как не разрешено.</span><span class="sxs-lookup"><span data-stu-id="5219a-276">Otherwise, the request returns as not authorized.</span></span> <span data-ttu-id="5219a-277">При таком подходе файлы cookie используются для создания приложений, которые могут быть с отслеживанием состояния, так как это «запомнить», который ранее проверка подлинности с сервером.</span><span class="sxs-lookup"><span data-stu-id="5219a-277">In this approach, cookies are used to make the application appear to be stateful, since it's able to "remember" that the user has previously authenticated with the server.</span></span>

### <a name="user-tokens"></a><span data-ttu-id="5219a-278">Токены пользователя</span><span class="sxs-lookup"><span data-stu-id="5219a-278">User tokens</span></span>

<span data-ttu-id="5219a-279">Токены проверки подлинности не хранятся в сеанс на сервере.</span><span class="sxs-lookup"><span data-stu-id="5219a-279">Token-based authentication doesn't store session on the server.</span></span> <span data-ttu-id="5219a-280">Когда пользователь входит в систему, они выполняется выданного маркера (не сложные маркер).</span><span class="sxs-lookup"><span data-stu-id="5219a-280">When a user is logged in, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="5219a-281">Этот маркер содержит данные, необходимые для проверки токена.</span><span class="sxs-lookup"><span data-stu-id="5219a-281">This token holds the data that's required to validate the token.</span></span> <span data-ttu-id="5219a-282">Он также содержит сведения о пользователе в виде [утверждений](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span><span class="sxs-lookup"><span data-stu-id="5219a-282">It also contains user information in the form of [claims](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span></span> <span data-ttu-id="5219a-283">Если пользователь хочет получить доступ к ресурсу сервера, проверка подлинности, маркер отправляется на сервер с заголовком дополнительная авторизация в форме {маркер} носителя.</span><span class="sxs-lookup"><span data-stu-id="5219a-283">When a user wants to access a server resource requiring authentication, the token is sent to the server with an additional authorization header in form of Bearer {token}.</span></span> <span data-ttu-id="5219a-284">Это делает приложение без сохранения состояния, так как в каждом последующем запросе маркер передается в запросе для проверки на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="5219a-284">This makes the application stateless since in each subsequent request the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="5219a-285">Этот токен не *зашифрованные*; скорее *кодировке*.</span><span class="sxs-lookup"><span data-stu-id="5219a-285">This token isn't *encrypted*; rather it's *encoded*.</span></span> <span data-ttu-id="5219a-286">На стороне сервера можно декодировать токен для доступа к необработанные данные в токен.</span><span class="sxs-lookup"><span data-stu-id="5219a-286">On the server-side, the token can be decoded to access the raw information within the token.</span></span> <span data-ttu-id="5219a-287">Чтобы отправить токен в последующих запросах, либо сохранить в локальном хранилище браузера или в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="5219a-287">To send the token in subsequent requests, either store it in the browser's local storage or in a cookie.</span></span> <span data-ttu-id="5219a-288">Не беспокойтесь о XSRF уязвимости, если маркер сохраняется в локальном хранилище, но это представляет проблему, если маркер хранится в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="5219a-288">Don't worry about XSRF vulnerability if the token is stored in the local storage, but it's an issue if the token is stored in a cookie.</span></span>

### <a name="multiple-applications-are-hosted-in-one-domain"></a><span data-ttu-id="5219a-289">Несколько приложений размещаются в одном домене</span><span class="sxs-lookup"><span data-stu-id="5219a-289">Multiple applications are hosted in one domain</span></span>

<span data-ttu-id="5219a-290">Несмотря на то что `example1.cloudapp.net` и `example2.cloudapp.net` разных узлах, которые есть неявные доверительные отношения между узлы на `*.cloudapp.net` домена.</span><span class="sxs-lookup"><span data-stu-id="5219a-290">Although `example1.cloudapp.net` and `example2.cloudapp.net` are different hosts, there's an implicit trust relationship between hosts under the `*.cloudapp.net` domain.</span></span> <span data-ttu-id="5219a-291">Неявные отношения доверия между позволяет потенциально небезопасных узлам влияет на файлы cookie друг друга (политики одного источника, которые управляют запросы AJAX не относиться к файлы cookie HTTP).</span><span class="sxs-lookup"><span data-stu-id="5219a-291">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span> <span data-ttu-id="5219a-292">Среда выполнения ASP.NET Core предоставляет некоторые по устранению рисков тем, что имя пользователя, внедряется в токен поля.</span><span class="sxs-lookup"><span data-stu-id="5219a-292">The ASP.NET Core runtime provides some mitigation in that the username is embedded into the field token.</span></span> <span data-ttu-id="5219a-293">Даже если вредоносный поддомен возможность перезаписывать маркер сеанса, он не может создать токен допустимого поля для пользователя.</span><span class="sxs-lookup"><span data-stu-id="5219a-293">Even if a malicious subdomain is able to overwrite a session token, it can't generate a valid field token for the user.</span></span> <span data-ttu-id="5219a-294">При размещении в такой среде подпрограммы встроенной защиты от XSRF по-прежнему не удается защитить от захвата сеанса или имя входа CSRF атак.</span><span class="sxs-lookup"><span data-stu-id="5219a-294">When hosted in such an environment, the built-in anti-XSRF routines still can't defend against session hijacking or login CSRF attacks.</span></span> <span data-ttu-id="5219a-295">Общими средами размещения, уязвимы для захвата сеанса входа CSRF и других атак.</span><span class="sxs-lookup"><span data-stu-id="5219a-295">Shared hosting environments are vunerable to session hijacking, login CSRF, and other attacks.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="5219a-296">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="5219a-296">Additional resources</span></span>

* <span data-ttu-id="5219a-297">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) на [откройте проект безопасности веб-приложения](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="5219a-297">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
