---
title: "Межсайтовых запросов подделки XSRF-атак в ASP.NET Core"
author: steve-smith
ms.author: riande
description: "Межсайтовых запросов подделки XSRF-атак в ASP.NET Core"
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/anti-request-forgery
ms.openlocfilehash: d7df8f91e88290509c8751a4b69804b60138846e
ms.sourcegitcommit: 31a979c5d45fd3ed38aa13ef17cddb3389721588
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2017
---
# <a name="preventing-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="a1aa6-103">Межсайтовых запросов подделки XSRF-атак в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a1aa6-103">Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core</span></span>

<span data-ttu-id="a1aa6-104">[Стив Смит](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a1aa6-104">[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-attack-does-anti-forgery-prevent"></a><span data-ttu-id="a1aa6-105">Какие атаки предотвращения подделки?</span><span class="sxs-lookup"><span data-stu-id="a1aa6-105">What attack does anti-forgery prevent?</span></span>

<span data-ttu-id="a1aa6-106">Подделки межсайтовых запросов (также известный как XSRF или CSRF, произносится *путешествие см. в разделе*) — это атака, для приложений веб сервере, при котором вредоносных веб-сайта могут повлиять на взаимодействие между браузером клиента и веб-сайт, которому доверяет Этот браузер.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-106">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted applications whereby a malicious web site can influence the interaction between a client browser and a web site that trusts that browser.</span></span> <span data-ttu-id="a1aa6-107">Такие атаки становится возможным, так как веб-браузеры отправку некоторых типов маркеров проверки подлинности автоматически при каждом запросе веб-сайту.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-107">These attacks are made possible because web browsers send some types of authentication tokens automatically with every request to a web site.</span></span> <span data-ttu-id="a1aa6-108">Эта форма эксплойта называется также *атаки одним щелчком* или как *riding сеанса*, так как пользуется атаки пользователем прошедшим проверку подлинности сеанса.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-108">This form of exploit is also known as a *one-click attack* or as *session riding*, because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="a1aa6-109">Примером атаки CSRF:</span><span class="sxs-lookup"><span data-stu-id="a1aa6-109">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="a1aa6-110">Пользователь выполняет вход в `www.example.com`, с помощью проверки подлинности форм.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-110">A user logs into `www.example.com`, using forms authentication.</span></span>
2. <span data-ttu-id="a1aa6-111">Сервер проверяет подлинность пользователя и отправляет ответ, который включает файл cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-111">The server authenticates the user and issues a response that includes an authentication cookie.</span></span>
3. <span data-ttu-id="a1aa6-112">Пользователь посещает вредоносный сайт.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-112">The user visits a malicious site.</span></span>

   <span data-ttu-id="a1aa6-113">Вредоносный сайт с HTML-формой следующего вида:</span><span class="sxs-lookup"><span data-stu-id="a1aa6-113">The malicious site contains an HTML form similar to the following:</span></span>

```html
   <h1>You Are a Winner!</h1>
     <form action="http://example.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw" />
       <input type="hidden" name="Amount" value="1000000" />
     <input type="submit" value="Click Me"/>
   </form>
```

<span data-ttu-id="a1aa6-114">Обратите внимание, что действие формы в блогах уязвимым сайте, не вредоносный сайт.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-114">Notice that the form action posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="a1aa6-115">Это часть CSRF «между сайтами».</span><span class="sxs-lookup"><span data-stu-id="a1aa6-115">This is the “cross-site” part of CSRF.</span></span>

4. <span data-ttu-id="a1aa6-116">При нажатии кнопки "Отправить".</span><span class="sxs-lookup"><span data-stu-id="a1aa6-116">The user clicks the submit button.</span></span> <span data-ttu-id="a1aa6-117">Браузер автоматически включает файл cookie проверки подлинности для запрошенного домена (уязвимым сайта в данном случае) с запросом.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-117">The browser automatically includes the authentication cookie for the requested domain (the vulnerable site in this case) with the request.</span></span>
5. <span data-ttu-id="a1aa6-118">Запрос выполняется на сервере с контекстом проверки подлинности пользователя и делать все, что разрешено делать прошедшего проверку подлинности пользователя.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-118">The request runs on the server with the user’s authentication context and can do anything that an authenticated user is allowed to do.</span></span>

<span data-ttu-id="a1aa6-119">В этом примере требуется пользователь, нажав кнопку формы.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-119">This example requires the user to click the form button.</span></span> <span data-ttu-id="a1aa6-120">Удалось вредоносную страницу:</span><span class="sxs-lookup"><span data-stu-id="a1aa6-120">The malicious page could:</span></span>

* <span data-ttu-id="a1aa6-121">Запустите скрипт, который автоматически отправляет форму.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-121">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="a1aa6-122">Отправляет отправки формы в качестве AJAX-запросом.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-122">Sends a form submission as an AJAX request.</span></span> 
* <span data-ttu-id="a1aa6-123">С помощью CSS с помощью скрытого формы.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-123">Use a hidden form with CSS.</span></span> 

<span data-ttu-id="a1aa6-124">С помощью протокола SSL не предотвращает атаки CSRF, можно отправить вредоносный сайт `https://` запроса.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-124">Using SSL does not prevent a CSRF attack, the malicious site can send an `https://` request.</span></span> 

<span data-ttu-id="a1aa6-125">Некоторые атаки целевого сайта конечных точек, которые отвечают на `GET` запросы, в которых регистр тег изображения можно использовать для выполнения действия (эту форму атаки проявляется на форуме сайтов, которые обеспечивают изображения, но блокировать JavaScript).</span><span class="sxs-lookup"><span data-stu-id="a1aa6-125">Some attacks  target site endpoints that respond to `GET` requests, in which case an image tag can be used to perform the action (this form of attack is common on forum sites that permit images but block JavaScript).</span></span> <span data-ttu-id="a1aa6-126">Приложения, которые изменяют состояние с `GET` уязвимы запросов от атак злоумышленников.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-126">Applications that change state with `GET` requests are  vulnerable from malicious attacks.</span></span>

<span data-ttu-id="a1aa6-127">Атаки CSRF возможны с веб-сайтов, которые используют файлы cookie для проверки подлинности, так как браузеры отправляют все соответствующие файлы cookie в веб-узел назначения.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-127">CSRF attacks are possible against web sites that use cookies for authentication, because browsers send all relevant cookies to the destination web site.</span></span> <span data-ttu-id="a1aa6-128">Тем не менее атаки CSRF ограничены не использовать файлы cookie.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-128">However, CSRF attacks are not limited to exploiting cookies.</span></span> <span data-ttu-id="a1aa6-129">Например Basic и дайджест-проверки подлинности также уязвимы.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-129">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="a1aa6-130">После входа пользователя в систему с обычная или краткая проверка подлинности браузер автоматически отправляет учетные данные до завершения сеанса.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-130">After a user logs in with Basic or Digest authentication, the browser automatically sends the credentials until the session ends.</span></span>

<span data-ttu-id="a1aa6-131">Примечание: В этом контексте *сеанса* ссылается на клиентский сеанс, во время которого пользователь проходит проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-131">Note: In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="a1aa6-132">Он не связан с сеансы на стороне сервера или [сеанса по промежуточного слоя](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="a1aa6-132">It is unrelated to server-side sessions or [session middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="a1aa6-133">Пользователям организовать защиту от уязвимостей CSRF с:</span><span class="sxs-lookup"><span data-stu-id="a1aa6-133">Users can guard against CSRF vulnerabilities by:</span></span>
* <span data-ttu-id="a1aa6-134">Ведение журнала из веб-сайтов, завершении работы с их помощью.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-134">Logging off of web sites when they have finished using them.</span></span>
* <span data-ttu-id="a1aa6-135">Периодически очистить файлы cookie браузера.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-135">Clearing their browser's cookies periodically.</span></span>

<span data-ttu-id="a1aa6-136">Тем не менее уязвимости CSRF фактически являются проблемы с веб-приложения, а не конечным пользователем.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-136">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="how-does-aspnet-core-mvc-address-csrf"></a><span data-ttu-id="a1aa6-137">Как ASP.NET Core MVC решать CSRF</span><span class="sxs-lookup"><span data-stu-id="a1aa6-137">How does ASP.NET Core MVC address CSRF?</span></span>

> [!WARNING]
> <span data-ttu-id="a1aa6-138">ASP.NET Core реализуется с помощью приложения для защиты request подделки [стека защиты данных ASP.NET Core](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="a1aa6-138">ASP.NET Core implements anti-request-forgery using the [ASP.NET Core data protection stack](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="a1aa6-139">ASP.NET Core защиты данных необходимо настроить для работы в ферме серверов.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-139">ASP.NET Core data protection must be configured to work in a server farm.</span></span> <span data-ttu-id="a1aa6-140">В разделе [настройки защиты данных](xref:security/data-protection/configuration/overview) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-140">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="a1aa6-141">Конфигурация защиты данных по умолчанию защита request подделки ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a1aa6-141">ASP.NET Core anti-request-forgery  default data protection configuration</span></span> 

<span data-ttu-id="a1aa6-142">В ASP.NET Core MVC 2.0 [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) внедряет маркеров защиты от подделки для элементов формы HTML.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-142">In ASP.NET Core MVC 2.0 the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects anti-forgery tokens for HTML form elements.</span></span> <span data-ttu-id="a1aa6-143">Например приведенный ниже код в файле Razor автоматически создаст маркеров защиты от подделки:</span><span class="sxs-lookup"><span data-stu-id="a1aa6-143">For example, the following markup in a Razor file will automatically generate anti-forgery tokens:</span></span>

```html
<form method="post">
  <!-- form markup -->
</form>
```

<span data-ttu-id="a1aa6-144">Происходит автоматическое создание маркеров защиты от подделки элементы HTML-формы при:</span><span class="sxs-lookup"><span data-stu-id="a1aa6-144">The automatic generation of anti-forgery tokens for HTML form elements happens when:</span></span>

* <span data-ttu-id="a1aa6-145">`form` Тег содержит `method="post"` атрибут AND</span><span class="sxs-lookup"><span data-stu-id="a1aa6-145">The `form` tag contains the `method="post"` attribute AND</span></span>

  * <span data-ttu-id="a1aa6-146">Атрибут действия пуст.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-146">The action attribute is empty.</span></span> <span data-ttu-id="a1aa6-147">( `action=""`) ИЛИ</span><span class="sxs-lookup"><span data-stu-id="a1aa6-147">( `action=""`) OR</span></span>
  * <span data-ttu-id="a1aa6-148">Не задан атрибут действия.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-148">The action attribute is not supplied.</span></span> <span data-ttu-id="a1aa6-149">(`<form method="post">`)</span><span class="sxs-lookup"><span data-stu-id="a1aa6-149">(`<form method="post">`)</span></span>

<span data-ttu-id="a1aa6-150">Можно отключить автоматическое создание маркеров защиты от подделки элементов формы HTML по:</span><span class="sxs-lookup"><span data-stu-id="a1aa6-150">You can disable automatic generation of anti-forgery tokens for HTML form elements by:</span></span>

* <span data-ttu-id="a1aa6-151">Явное отключение `asp-antiforgery`.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-151">Explicitly disabling `asp-antiforgery`.</span></span> <span data-ttu-id="a1aa6-152">Пример</span><span class="sxs-lookup"><span data-stu-id="a1aa6-152">For example</span></span>

 ```html
  <form method="post" asp-antiforgery="false">
  </form>
  ```

* <span data-ttu-id="a1aa6-153">Необязательно элемент формы из вспомогательных функций тегов с помощью вспомогательного тег [! символ отказаться](xref:mvc/views/tag-helpers/intro#opt-out).</span><span class="sxs-lookup"><span data-stu-id="a1aa6-153">Opt the form element out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out).</span></span>

 ```html
  <!form method="post">
  </!form>
  ```

* <span data-ttu-id="a1aa6-154">Удалите `FormTagHelper` из представления.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-154">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="a1aa6-155">Можно удалить `FormTagHelper` из представления путем добавления представления Razor следующую директиву:</span><span class="sxs-lookup"><span data-stu-id="a1aa6-155">You can remove the `FormTagHelper` from a view by adding the following directive to the Razor view:</span></span>

 ```html
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="a1aa6-156">[Страниц Razor](xref:mvc/razor-pages/index) автоматически защищены от XSRF-CSRF.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-156">[Razor Pages](xref:mvc/razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="a1aa6-157">Нет необходимости создавать дополнительный код.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-157">You don't have to write any additional code.</span></span> <span data-ttu-id="a1aa6-158">В разделе [XSRF/CSRF и страниц Razor](xref:mvc/razor-pages/index#xsrf) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-158">See [XSRF/CSRF and Razor Pages](xref:mvc/razor-pages/index#xsrf) for more information.</span></span>

<span data-ttu-id="a1aa6-159">Наиболее распространенным подходом на защиту от атак CSRF является синхронизатор маркера (STP).</span><span class="sxs-lookup"><span data-stu-id="a1aa6-159">The most common approach to defending against CSRF attacks is the synchronizer token pattern (STP).</span></span> <span data-ttu-id="a1aa6-160">STP – методика, когда пользователь запрашивает страницу с данными в форме.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-160">STP is a technique used when the user requests a page with form data.</span></span> <span data-ttu-id="a1aa6-161">Сервер отправляет токен отмены, связанный с удостоверением текущего пользователя для клиента.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-161">The server sends a token associated with the current user's identity to the client.</span></span> <span data-ttu-id="a1aa6-162">Клиент отправляет маркер на сервер для проверки.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-162">The client sends back the token to the server for verification.</span></span> <span data-ttu-id="a1aa6-163">Если сервер получает маркер, который не совпадает с удостоверением пользователя, прошедшего проверку подлинности, запрос отклоняется.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-163">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span> <span data-ttu-id="a1aa6-164">Маркер является уникальным и непредсказуемым.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-164">The token is unique and unpredictable.</span></span> <span data-ttu-id="a1aa6-165">Маркер может также использоваться для обеспечения правильной последовательности серию запросов (обеспечение страница 1 предшествует страница 2, которая предшествует страница 3).</span><span class="sxs-lookup"><span data-stu-id="a1aa6-165">The token can also be used to ensure proper sequencing of a series of requests (ensuring page 1 precedes page 2 which precedes page 3).</span></span> <span data-ttu-id="a1aa6-166">Все формы в ASP.NET Core MVC шаблоны создавать сложные маркеры.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-166">All the forms in ASP.NET Core MVC templates generate antiforgery tokens.</span></span> <span data-ttu-id="a1aa6-167">Следующие два примера логики представления создавать сложные токены:</span><span class="sxs-lookup"><span data-stu-id="a1aa6-167">The following two examples of view logic generate antiforgery tokens:</span></span>

```html
<form asp-controller="Manage" asp-action="ChangePassword" method="post">

</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    
}
```

<span data-ttu-id="a1aa6-168">Можно явно добавить сложные токен ``<form>`` элемент без использования вспомогательных функций тегов с вспомогательный метод HTML ``@Html.AntiForgeryToken``:</span><span class="sxs-lookup"><span data-stu-id="a1aa6-168">You can explicitly add an antiforgery token to a ``<form>`` element without using tag helpers with the HTML helper ``@Html.AntiForgeryToken``:</span></span>


```html
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="a1aa6-169">Во всех перечисленных случаях ASP.NET Core добавит скрытое поле формы следующего вида:</span><span class="sxs-lookup"><span data-stu-id="a1aa6-169">In each of the preceding cases, ASP.NET Core will add a hidden form field similar to the following:</span></span>
```html
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkSldwD9CpLRyOtm6FiJB1Jr_F3FQJQDvhlHoLNJJrLA6zaMUmhjMsisu2D2tFkAiYgyWQawJk9vNm36sYP1esHOtamBEPvSk1_x--Sg8Ey2a-d9CV2zHVWIN9MVhvKHOSyKqdZFlYDVd69XYx-rOWPw3ilHGLN6K0Km-1p83jZzF0E4WU5OGg5ns2-m9Yw" />
```

<span data-ttu-id="a1aa6-170">ASP.NET Core включает в себя три [фильтры](xref:mvc/controllers/filters) для работы с сложные токены: ``ValidateAntiForgeryToken``, ``AutoValidateAntiforgeryToken``, и ``IgnoreAntiforgeryToken``.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-170">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens: ``ValidateAntiForgeryToken``, ``AutoValidateAntiforgeryToken``, and ``IgnoreAntiforgeryToken``.</span></span>

<a name="vaft"></a>

### <a name="validateantiforgerytoken"></a><span data-ttu-id="a1aa6-171">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="a1aa6-171">ValidateAntiForgeryToken</span></span>

<span data-ttu-id="a1aa6-172">``ValidateAntiForgeryToken`` Является фильтром действий, могут применяться для отдельного действия, контроллера или глобально.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-172">The ``ValidateAntiForgeryToken`` is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="a1aa6-173">Запросы, адресованные действий, имеющих этот фильтр, применяемый будут заблокированы, если запрос содержит сложные допустимый токен.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-173">Requests made to actions that have this filter applied will be blocked unless the request includes a valid antiforgery token.</span></span>

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

<span data-ttu-id="a1aa6-174">``ValidateAntiForgeryToken`` Атрибута требуется маркер для запросов к методам действий, в которой относится, включая `HTTP GET` запросов.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-174">The ``ValidateAntiForgeryToken`` attribute requires a token for requests to action methods it decorates, including `HTTP GET` requests.</span></span> <span data-ttu-id="a1aa6-175">При применении широко, его можно переопределить с помощью ``IgnoreAntiforgeryToken`` атрибута.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-175">If you apply it broadly, you can override it with the ``IgnoreAntiforgeryToken`` attribute.</span></span>

### <a name="autovalidateantiforgerytoken"></a><span data-ttu-id="a1aa6-176">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="a1aa6-176">AutoValidateAntiforgeryToken</span></span>

<span data-ttu-id="a1aa6-177">Приложения ASP.NET Core обычно не создают сложные маркерах безопасные методы HTTP (GET, HEAD, параметры и ТРАССИРОВКИ).</span><span class="sxs-lookup"><span data-stu-id="a1aa6-177">ASP.NET Core apps generally do not generate antiforgery tokens for HTTP safe methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="a1aa6-178">Вместо применения широко ``ValidateAntiForgeryToken`` атрибута и затем переопределение его с ``IgnoreAntiforgeryToken`` атрибуты, можно использовать ``AutoValidateAntiforgeryToken`` атрибута.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-178">Instead of broadly applying the ``ValidateAntiForgeryToken`` attribute and then overriding it with ``IgnoreAntiforgeryToken`` attributes, you can use the ``AutoValidateAntiforgeryToken`` attribute.</span></span> <span data-ttu-id="a1aa6-179">Этот атрибут работает идентично методу ``ValidateAntiForgeryToken`` атрибута, за исключением того, что он не требует токены для запросов, выполненных с помощью следующих методов HTTP:</span><span class="sxs-lookup"><span data-stu-id="a1aa6-179">This attribute works identically to the ``ValidateAntiForgeryToken`` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="a1aa6-180">GET</span><span class="sxs-lookup"><span data-stu-id="a1aa6-180">GET</span></span>
* <span data-ttu-id="a1aa6-181">HEAD,</span><span class="sxs-lookup"><span data-stu-id="a1aa6-181">HEAD</span></span>
* <span data-ttu-id="a1aa6-182">OPTIONS</span><span class="sxs-lookup"><span data-stu-id="a1aa6-182">OPTIONS</span></span>
* <span data-ttu-id="a1aa6-183">TRACE</span><span class="sxs-lookup"><span data-stu-id="a1aa6-183">TRACE</span></span>

<span data-ttu-id="a1aa6-184">Рекомендуется использовать ``AutoValidateAntiforgeryToken`` широко для сценариев, отличных от API.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-184">We recommend you use ``AutoValidateAntiforgeryToken`` broadly for non-API scenarios.</span></span> <span data-ttu-id="a1aa6-185">Это гарантирует, что ваши действия POST защищены по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-185">This ensures your POST actions are protected by default.</span></span> <span data-ttu-id="a1aa6-186">Вместо этого должен игнорировать сложные токены по умолчанию, если не ``ValidateAntiForgeryToken`` применяется к методу отдельные действия.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-186">The alternative is to ignore antiforgery tokens by default, unless ``ValidateAntiForgeryToken`` is applied to the individual action method.</span></span> <span data-ttu-id="a1aa6-187">Чаще всего в этом сценарии для метода POST действие для левой незащищенными, оставляя приложение уязвимым для атак CSRF.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-187">It's more likely in this scenario for a POST action method to be left unprotected, leaving your app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="a1aa6-188">Даже анонимного сообщения, необходимо отправить токен сложные.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-188">Even anonymous POSTS should send the antiforgery token.</span></span>

<span data-ttu-id="a1aa6-189">Примечание: API не имеют автоматический механизм для отправки не cookie часть маркера; Реализация скорее всего будет зависеть от реализации клиентского кода.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-189">Note: APIs don't have an automatic mechanism for sending the non-cookie part of the token; your implementation will likely depend on your client code implementation.</span></span> <span data-ttu-id="a1aa6-190">Ниже приведены некоторые примеры.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-190">Some examples are shown below.</span></span>


<span data-ttu-id="a1aa6-191">Пример (уровень класса).</span><span class="sxs-lookup"><span data-stu-id="a1aa6-191">Example (class level):</span></span>

```c#
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="a1aa6-192">Пример (глобальные).</span><span class="sxs-lookup"><span data-stu-id="a1aa6-192">Example (global):</span></span>

```c#
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

<a name="iaft"></a>

### <a name="ignoreantiforgerytoken"></a><span data-ttu-id="a1aa6-193">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="a1aa6-193">IgnoreAntiforgeryToken</span></span>

<span data-ttu-id="a1aa6-194">``IgnoreAntiforgeryToken`` Фильтр используется для устранения необходимости в сложные маркера должны присутствовать для данного действия (или контроллера).</span><span class="sxs-lookup"><span data-stu-id="a1aa6-194">The ``IgnoreAntiforgeryToken`` filter is used to eliminate the need for an antiforgery token to be present for a given action (or controller).</span></span> <span data-ttu-id="a1aa6-195">При применении этого фильтра переопределит ``ValidateAntiForgeryToken`` и/или ``AutoValidateAntiforgeryToken`` фильтров, указанных на более высоком уровне (глобально или на контроллере).</span><span class="sxs-lookup"><span data-stu-id="a1aa6-195">When applied, this filter will override ``ValidateAntiForgeryToken`` and/or ``AutoValidateAntiforgeryToken`` filters specified at a higher level (globally or on a controller).</span></span>

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

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="a1aa6-196">JavaScript, AJAX и SPAs</span><span class="sxs-lookup"><span data-stu-id="a1aa6-196">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="a1aa6-197">В традиционных приложениях на основе HTML сложные токены, передаются на сервер с помощью скрытые поля формы.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-197">In traditional HTML-based applications, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="a1aa6-198">В современных приложений на базе JavaScript и приложений на одной странице (SPAs) многие запросы выполняются программно.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-198">In modern JavaScript-based apps and single page applications (SPAs), many requests are made programmatically.</span></span> <span data-ttu-id="a1aa6-199">Маркер отправляется в эти запросы AJAX может использовать другие методы (например, заголовки запроса или куки-файлы).</span><span class="sxs-lookup"><span data-stu-id="a1aa6-199">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span> <span data-ttu-id="a1aa6-200">Если файлы cookie используются для хранения токенов проверки подлинности и проверки подлинности запросов API на сервере, CSRF будет потенциальных проблем.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-200">If cookies are used to store authentication tokens and to authenticate API requests on the server, then CSRF will be a potential problem.</span></span> <span data-ttu-id="a1aa6-201">Однако при использовании локального хранилища для сохранения токена CSRF уязвимость может устранить, так как значения из локального хранилища не отправляются автоматически на сервере каждый новый запрос.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-201">However, if local storage is used to store the token, CSRF vulnerability may be mitigated, since values from local storage are not sent automatically to the server with every new request.</span></span> <span data-ttu-id="a1aa6-202">Таким образом использует локальное хранилище для хранения сложные маркера на клиенте и отправка маркера как заголовок запроса является рекомендуемым.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-202">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="angularjs"></a><span data-ttu-id="a1aa6-203">AngularJS</span><span class="sxs-lookup"><span data-stu-id="a1aa6-203">AngularJS</span></span>

<span data-ttu-id="a1aa6-204">AngularJS используется соглашение по адресу CSRF.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-204">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="a1aa6-205">Если сервер отправляет куки-файл с именем ``XSRF-TOKEN``, угловая ``$http`` служба добавляет значение из этого файла cookie заголовком при отправке запроса на этот сервер.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-205">If the server sends a cookie with the name ``XSRF-TOKEN``, the Angular ``$http`` service will add the value from this cookie to a header when it sends a request to this server.</span></span> <span data-ttu-id="a1aa6-206">Этот процесс выполняется автоматически; Нет необходимости явно задавать заголовок.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-206">This process is automatic; you don't need to set the header explicitly.</span></span> <span data-ttu-id="a1aa6-207">Имя заголовка — ``X-XSRF-TOKEN``.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-207">The header name is ``X-XSRF-TOKEN``.</span></span> <span data-ttu-id="a1aa6-208">Сервер должен обнаружить этот заголовок и проверьте его содержимое.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-208">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="a1aa6-209">Для ASP.NET Core API работают с этим соглашением.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-209">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="a1aa6-210">Настройка приложения для предоставления токена в объект``XSRF-TOKEN``</span><span class="sxs-lookup"><span data-stu-id="a1aa6-210">Configure your app to provide a token in a cookie called ``XSRF-TOKEN``</span></span>
* <span data-ttu-id="a1aa6-211">Настройте службу сложные поиск заголовка с именем``X-XSRF-TOKEN``</span><span class="sxs-lookup"><span data-stu-id="a1aa6-211">Configure the antiforgery service to look for a header named ``X-XSRF-TOKEN``</span></span>

```c#
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="a1aa6-212">[Просмотр результата](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span><span class="sxs-lookup"><span data-stu-id="a1aa6-212">[View sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span></span>

### <a name="javascript"></a><span data-ttu-id="a1aa6-213">JavaScript</span><span class="sxs-lookup"><span data-stu-id="a1aa6-213">JavaScript</span></span>

<span data-ttu-id="a1aa6-214">С представлениями с использованием JavaScript, можно создать токен, используя службу в представлении.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-214">Using JavaScript with views, you can create the token using a service from within your view.</span></span> <span data-ttu-id="a1aa6-215">Чтобы сделать это, вставьте `Microsoft.AspNetCore.Antiforgery.IAntiforgery` в представлении и вызовите службу `GetAndStoreTokens`, как показано:</span><span class="sxs-lookup"><span data-stu-id="a1aa6-215">To do so, you inject the `Microsoft.AspNetCore.Antiforgery.IAntiforgery` service into the view and call `GetAndStoreTokens`, as shown:</span></span>

[!code-csharp[Main](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,24)]

<span data-ttu-id="a1aa6-216">Этот подход избавляет от необходимости работать напрямую с Установка с сервера файлы cookie или их считывания из клиента.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-216">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="a1aa6-217">JavaScript можно также получить доступ к токенов, предоставленных в файлах cookie и использовать содержимое файла cookie для создания заголовка значения маркера, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-217">JavaScript can also access tokens provided in cookies, and then use the cookie's contents to create a header with the token's value, as shown below.</span></span>

```c#
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
  new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="a1aa6-218">Затем, при условии, что создания сценария запрашивает Отправка токена в заголовок с именем ``X-CSRF-TOKEN``, настройте службу сложные искать ``X-CSRF-TOKEN`` заголовка:</span><span class="sxs-lookup"><span data-stu-id="a1aa6-218">Then, assuming you construct your script requests to send the token in a header called ``X-CSRF-TOKEN``, configure the antiforgery service to look for the ``X-CSRF-TOKEN`` header:</span></span>

```c#
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="a1aa6-219">В следующем примере jQuery AJAX-запросом соответствующий заголовок сделать:</span><span class="sxs-lookup"><span data-stu-id="a1aa6-219">The following example uses jQuery to make an AJAX request with the appropriate header:</span></span>

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

## <a name="configuring-antiforgery"></a><span data-ttu-id="a1aa6-220">Настройка Antiforgery</span><span class="sxs-lookup"><span data-stu-id="a1aa6-220">Configuring Antiforgery</span></span>

<span data-ttu-id="a1aa6-221">`IAntiforgery`предоставляет API для настройки сложные системы.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-221">`IAntiforgery` provides the API to configure the antiforgery system.</span></span> <span data-ttu-id="a1aa6-222">Это можно выполнить в `Configure` метод `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-222">It can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="a1aa6-223">Следующий пример использует по промежуточного слоя с домашней страницы приложения для создания токена сложные и отправлять их в ответ как куки-файлы (с помощью именования по умолчанию углового описано выше):</span><span class="sxs-lookup"><span data-stu-id="a1aa6-223">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described above):</span></span>


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

### <a name="options"></a><span data-ttu-id="a1aa6-224">Параметры</span><span class="sxs-lookup"><span data-stu-id="a1aa6-224">Options</span></span>

<span data-ttu-id="a1aa6-225">Вы можете настроить [сложные параметры](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a1aa6-225">You can customize [antiforgery options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) in `ConfigureServices`:</span></span>

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

|<span data-ttu-id="a1aa6-226">Параметр</span><span class="sxs-lookup"><span data-stu-id="a1aa6-226">Option</span></span>        | <span data-ttu-id="a1aa6-227">Описание</span><span class="sxs-lookup"><span data-stu-id="a1aa6-227">Description</span></span> |
|------------- | ----------- |
|<span data-ttu-id="a1aa6-228">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="a1aa6-228">CookieDomain</span></span>  | <span data-ttu-id="a1aa6-229">Домен cookie.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-229">The domain of the cookie.</span></span> <span data-ttu-id="a1aa6-230">По умолчанию — `null`.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-230">Defaults to `null`.</span></span> |
|<span data-ttu-id="a1aa6-231">CookieName</span><span class="sxs-lookup"><span data-stu-id="a1aa6-231">CookieName</span></span>    | <span data-ttu-id="a1aa6-232">Имя файла cookie.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-232">The name of the cookie.</span></span> <span data-ttu-id="a1aa6-233">Если не задано, система будет формировать уникальное имя которого начинается с `DefaultCookiePrefix` (». AspNetCore.Antiforgery.»).</span><span class="sxs-lookup"><span data-stu-id="a1aa6-233">If not set, the system will generate a unique name beginning with the `DefaultCookiePrefix` (".AspNetCore.Antiforgery.").</span></span> |
|<span data-ttu-id="a1aa6-234">CookiePath</span><span class="sxs-lookup"><span data-stu-id="a1aa6-234">CookiePath</span></span>    | <span data-ttu-id="a1aa6-235">Путь задать в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-235">The path set on the cookie.</span></span> |
|<span data-ttu-id="a1aa6-236">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="a1aa6-236">FormFieldName</span></span> | <span data-ttu-id="a1aa6-237">Имя скрытое поле формы используются сложные системой для подготовки к просмотру сложные токены в представлениях.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-237">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
|<span data-ttu-id="a1aa6-238">HeaderName</span><span class="sxs-lookup"><span data-stu-id="a1aa6-238">HeaderName</span></span>    | <span data-ttu-id="a1aa6-239">Имя заголовка, используемые системой сложные.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-239">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="a1aa6-240">Если `null`, система будет рассматривать только данные формы.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-240">If `null`, the system will consider only form data.</span></span> |
|<span data-ttu-id="a1aa6-241">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="a1aa6-241">RequireSsl</span></span>    | <span data-ttu-id="a1aa6-242">Указывает, требуется ли SSL сложные системой.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-242">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="a1aa6-243">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-243">Defaults to `false`.</span></span> <span data-ttu-id="a1aa6-244">Если `true`, без использования SSL запросы будут завершаться ошибкой.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-244">If `true`, non-SSL requests will fail.</span></span> |
|<span data-ttu-id="a1aa6-245">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="a1aa6-245">SuppressXFrameOptionsHeader</span></span>  | <span data-ttu-id="a1aa6-246">Указывает, следует ли подавлять поколение `X-Frame-Options` заголовок.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-246">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="a1aa6-247">По умолчанию заголовок создается со значением «SAMEORIGIN».</span><span class="sxs-lookup"><span data-stu-id="a1aa6-247">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="a1aa6-248">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-248">Defaults to `false`.</span></span> |

<span data-ttu-id="a1aa6-249">В разделе https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-249">See https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions for more info.</span></span>

### <a name="extending-antiforgery"></a><span data-ttu-id="a1aa6-250">Расширение Antiforgery</span><span class="sxs-lookup"><span data-stu-id="a1aa6-250">Extending Antiforgery</span></span>

<span data-ttu-id="a1aa6-251">[IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) типа позволяет разработчикам расширять поведение системы защиты от XSRF с циклической обработки дополнительных данных в каждом маркере.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-251">The [IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-XSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="a1aa6-252">[GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) при каждом вызове метода создается маркер поля, и возвращаемое значение внедряется в созданный токен.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-252">The [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="a1aa6-253">Разработчик может возвращать отметку времени, nonce или любое другое значение, а затем вызвать [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) для проверки данных, если маркер проверяется.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-253">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) to validate this data when the token is validated.</span></span> <span data-ttu-id="a1aa6-254">Имя пользователя клиента уже внедрена в создаваемые маркеры, поэтому нет необходимости включать эти сведения.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-254">The client's username is already embedded in the generated tokens, so there is no need to include this information.</span></span> <span data-ttu-id="a1aa6-255">Если токен содержит дополнительные данные, но нет `IAntiForgeryAdditionalDataProvider` был настроен, дополнительные данные не проверяются.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-255">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` has been configured, the supplemental data is not validated.</span></span>

## <a name="fundamentals"></a><span data-ttu-id="a1aa6-256">Основы</span><span class="sxs-lookup"><span data-stu-id="a1aa6-256">Fundamentals</span></span>

<span data-ttu-id="a1aa6-257">Атаки CSRF используется поведение по умолчанию браузер отправлять файлы cookie, связанные с доменом с каждый запрос к этому домену.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-257">CSRF attacks rely on the default browser behavior of sending cookies associated with a domain with every request made to that domain.</span></span> <span data-ttu-id="a1aa6-258">Эти файлы Cookie хранятся в браузере.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-258">These cookies are stored within the browser.</span></span> <span data-ttu-id="a1aa6-259">Они часто включают в себя файлы cookie сеансов для прошедших проверку пользователей.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-259">They frequently include session cookies for authenticated users.</span></span> <span data-ttu-id="a1aa6-260">Проверка подлинности на основе файла Cookie является популярная форма проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-260">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="a1aa6-261">Токены проверки подлинности систем росли в популярности, особенно для SPAs и других сценариев «смарт-клиент».</span><span class="sxs-lookup"><span data-stu-id="a1aa6-261">Token-based authentication systems have been growing in popularity, especially for SPAs and other "smart client" scenarios.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="a1aa6-262">Проверка подлинности на основе файлов cookie</span><span class="sxs-lookup"><span data-stu-id="a1aa6-262">Cookie-based authentication</span></span>

<span data-ttu-id="a1aa6-263">Как только пользователь проверку подлинности с помощью своих имени пользователя и пароль, они будут выпущены маркер, который может использоваться для их идентификации и проверки, что они прошли проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-263">Once a user has authenticated using their username and password, they are issued a token that can be used to identify them and validate that they have been authenticated.</span></span> <span data-ttu-id="a1aa6-264">Маркер сохраняется как файл cookie, который сопровождает каждый запрос клиента.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-264">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="a1aa6-265">Создание и проверка этот файл cookie выполняется по промежуточного слоя проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-265">Generating and validating this cookie is done by the cookie authentication middleware.</span></span> <span data-ttu-id="a1aa6-266">ASP.NET Core указывает куки-файл [по промежуточного слоя](../fundamentals/middleware.md) которого сериализует участника-пользователя в зашифрованном файле cookie, а затем, при последующих запросах проводится проверка куки-файл, повторно создает основной и присваивает его `User` свойство `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-266">ASP.NET Core provides cookie [middleware](../fundamentals/middleware.md) which serializes a user principal into an encrypted cookie and then, on subsequent requests, validates the cookie, recreates the principal and assigns it to the `User` property on `HttpContext`.</span></span>

<span data-ttu-id="a1aa6-267">Если используется файл cookie, файл cookie проверки подлинности является просто контейнером для билета проверки подлинности форм.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-267">When a cookie is used, The authentication cookie is just a container for the forms authentication ticket.</span></span> <span data-ttu-id="a1aa6-268">Билет передается как значение файла cookie проверки подлинности форм с каждым запросом и используется проверка подлинности форм на сервере, для идентификации пользователя, прошедшего аутентификацию.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-268">The ticket is passed as the value of the forms authentication cookie with each request and is used by forms authentication, on the server, to identify an authenticated user.</span></span>

<span data-ttu-id="a1aa6-269">Если пользователь вошел в систему, сеанса пользователя создается на стороне сервера и хранится в базе данных или другое постоянное хранилище.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-269">When a user is logged in to a system, a user session is created on the server-side and is stored in a database or some other persistent store.</span></span> <span data-ttu-id="a1aa6-270">Система создает ключ сеанса, который указывает фактическое сеанса в хранилище данных и отправкой в качестве файла cookie стороны клиента.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-270">The system generates a session key that points to the actual session in the data store and it is sent as a client side cookie.</span></span> <span data-ttu-id="a1aa6-271">Веб-сервер будет проверять этот ключ сеанса каждый раз пользователь запрашивает ресурс, который требует авторизации.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-271">The web server will check this session key any time a user requests a resource that requires authorization.</span></span> <span data-ttu-id="a1aa6-272">Система проверяет, имеет ли сеанс пользователя прав для доступа к запрошенному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-272">The system checks whether the associated user session has the privilege to access the requested resource.</span></span> <span data-ttu-id="a1aa6-273">В этом случае запроса продолжается.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-273">If so, the request continues.</span></span> <span data-ttu-id="a1aa6-274">В противном случае — запрос возвращает как не разрешено.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-274">Otherwise, the request returns as not authorized.</span></span> <span data-ttu-id="a1aa6-275">При таком подходе файлы cookie используются для создания приложений, которые могут быть с отслеживанием состояния, так как это «запомнить», который ранее проверка подлинности с сервером.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-275">In this approach, cookies are used to make the application appear to be stateful, since it is able to "remember" that the user has previously authenticated with the server.</span></span>

### <a name="user-tokens"></a><span data-ttu-id="a1aa6-276">Токены пользователя</span><span class="sxs-lookup"><span data-stu-id="a1aa6-276">User tokens</span></span>

<span data-ttu-id="a1aa6-277">Токены проверки подлинности не хранятся в сеанс на сервере.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-277">Token-based authentication doesn’t store session on the server.</span></span> <span data-ttu-id="a1aa6-278">Вместо этого когда пользователь входит в систему, выдаются ли они маркер (не сложные).</span><span class="sxs-lookup"><span data-stu-id="a1aa6-278">Instead, when a user is logged in they are issued a token (not an antiforgery token).</span></span> <span data-ttu-id="a1aa6-279">Этот маркер содержит все данные, необходимые для проверки токена.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-279">This token holds all the data that is required to validate the token.</span></span> <span data-ttu-id="a1aa6-280">Он также содержит сведения о пользователе, в виде [утверждений](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span><span class="sxs-lookup"><span data-stu-id="a1aa6-280">It also contains user information, in the form of [claims](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span></span> <span data-ttu-id="a1aa6-281">Если пользователь хочет получить доступ к ресурсу сервера, проверка подлинности, маркер отправляется на сервер с заголовком дополнительная авторизация в форме {маркер} носителя.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-281">When a user wants to access a server resource requiring authentication, the token is sent to the server with an additional authorization header in form of Bearer {token}.</span></span> <span data-ttu-id="a1aa6-282">Это делает приложение без сохранения состояния, так как в каждом последующем запросе маркер передается в запросе для проверки на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-282">This makes the application stateless since in each subsequent request the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="a1aa6-283">Этот маркер не *зашифрованные*; скорее *кодировке*.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-283">This token is not *encrypted*; rather it is *encoded*.</span></span> <span data-ttu-id="a1aa6-284">На стороне сервера, можно декодировать маркер для доступа к необработанные данные в токен.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-284">On the server-side the token can be decoded to access the raw information within the token.</span></span> <span data-ttu-id="a1aa6-285">Чтобы отправить токен в последующих запросах, вы либо его можно сохранить в локальном хранилище браузера или в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-285">To send the token in subsequent requests, you can either store it in browser’s local storage or in a cookie.</span></span> <span data-ttu-id="a1aa6-286">Не нужно беспокоиться о XSRF уязвимость, если срок действия токена сохраняется в локальном хранилище, но это представляет проблему, если маркер хранится в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-286">You don’t have to worry about XSRF vulnerability if your token is stored in the local storage, but it is an issue if the token is stored in a cookie.</span></span>

### <a name="multiple-applications-are-hosted-in-one-domain"></a><span data-ttu-id="a1aa6-287">Несколько приложений размещаются в одном домене</span><span class="sxs-lookup"><span data-stu-id="a1aa6-287">Multiple applications are hosted in one domain</span></span>

<span data-ttu-id="a1aa6-288">Несмотря на то что `example1.cloudapp.net` и `example2.cloudapp.net` разных узлах, которые есть неявные доверительные отношения между все узлы на `*.cloudapp.net` домена.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-288">Even though `example1.cloudapp.net` and `example2.cloudapp.net` are different hosts, there is an implicit trust relationship between all hosts under the `*.cloudapp.net` domain.</span></span> <span data-ttu-id="a1aa6-289">Неявные отношения доверия между позволяет потенциально небезопасных узлам влияет на файлы cookie друг друга (политики одного источника, которые управляют запросы AJAX неприменимы обязательно для файлов cookie HTTP).</span><span class="sxs-lookup"><span data-stu-id="a1aa6-289">This implicit trust relationship allows potentially untrusted hosts to affect each other’s cookies (the same-origin policies that govern AJAX requests do not necessarily apply to HTTP cookies).</span></span> <span data-ttu-id="a1aa6-290">Среда выполнения ASP.NET Core предоставляет некоторые по устранению рисков тем, что имя пользователя, внедряется в токен поля, таким образом, даже если вредоносный поддомен возможность перезаписывать маркер сеанса его не удалось создать маркер допустимого поля для пользователя.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-290">The ASP.NET Core runtime provides some mitigation in that the username is embedded into the field token, so even if a malicious subdomain is able to overwrite a session token it will be unable to generate a valid field token for the user.</span></span> <span data-ttu-id="a1aa6-291">Однако при размещении в такой среде подпрограммы встроенной защиты от XSRF по-прежнему не удается защитить от захвата сеанса или имя входа CSRF атак.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-291">However, when hosted in such an environment the built-in anti-XSRF routines still cannot defend against session hijacking or login CSRF attacks.</span></span> <span data-ttu-id="a1aa6-292">Общими средами размещения, уязвимы для захвата сеанса входа CSRF и других атак.</span><span class="sxs-lookup"><span data-stu-id="a1aa6-292">Shared hosting environments are vunerable to session hijacking, login CSRF, and other attacks.</span></span>


### <a name="additional-resources"></a><span data-ttu-id="a1aa6-293">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a1aa6-293">Additional Resources</span></span>

* <span data-ttu-id="a1aa6-294">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) на [откройте проект безопасности веб-приложения](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="a1aa6-294">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
