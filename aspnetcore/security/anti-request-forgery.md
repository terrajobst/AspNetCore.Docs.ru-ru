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
ms.openlocfilehash: 3bca96f4a2e247eeeb93140df93221371d88d4d3
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341864"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="5cdd0-103">Предотвратить межсайтовых запросов подделки XSRF-атак в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5cdd0-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="5cdd0-104">По [Стив Смит](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5cdd0-104">By [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5cdd0-105">Подделки межсайтовых запросов (также известный как XSRF или CSRF, произносится *путешествие см. в разделе*) — это атака, в приложениях веб сервере, при котором вредоносных веб-приложения могут повлиять на взаимодействие между веб-браузер клиента и веб-приложение, которому доверяет, Обозреватель.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-105">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted apps whereby a malicious web app can influence the interaction between a client browser and a web app that trusts that browser.</span></span> <span data-ttu-id="5cdd0-106">Эти атаки возможны, так как веб-браузеры отправляют некоторые типы токены проверки подлинности автоматически при каждом запросе на веб-сайт.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-106">These attacks are possible because web browsers send some types of authentication tokens automatically with every request to a website.</span></span> <span data-ttu-id="5cdd0-107">Эта форма эксплойта называется также *атаки одним щелчком* или *riding сеанса* так, как атака использует преимущества ранее проверить подлинность пользователя сеанса.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-107">This form of exploit is also known as a *one-click attack* or *session riding* because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="5cdd0-108">Примером атаки CSRF:</span><span class="sxs-lookup"><span data-stu-id="5cdd0-108">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="5cdd0-109">Пользователь входит в `www.good-banking-site.com` проверки подлинности с помощью форм.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-109">A user signs into `www.good-banking-site.com` using forms authentication.</span></span> <span data-ttu-id="5cdd0-110">Сервер проверяет подлинность пользователя и отправляет ответ, который включает файл cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-110">The server authenticates the user and issues a response that includes an authentication cookie.</span></span> <span data-ttu-id="5cdd0-111">Этот сайт является уязвимым для атак, так как она доверяет любому запросу, который получает объект cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-111">The site is vulnerable to attack because it trusts any request that it receives with a valid authentication cookie.</span></span>
1. <span data-ttu-id="5cdd0-112">Пользователь посещает вредоносный сайт, `www.bad-crook-site.com`.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-112">The user visits a malicious site, `www.bad-crook-site.com`.</span></span>

   <span data-ttu-id="5cdd0-113">Вредоносный сайт `www.bad-crook-site.com`, содержит HTML-форму следующим образом:</span><span class="sxs-lookup"><span data-stu-id="5cdd0-113">The malicious site, `www.bad-crook-site.com`, contains an HTML form similar to the following:</span></span>

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   <span data-ttu-id="5cdd0-114">Обратите внимание, что формы `action` уязвимым сайта, а не к вредоносный сайт в блогах.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-114">Notice that the form's `action` posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="5cdd0-115">Это часть CSRF «между сайтами».</span><span class="sxs-lookup"><span data-stu-id="5cdd0-115">This is the "cross-site" part of CSRF.</span></span>

1. <span data-ttu-id="5cdd0-116">Пользователь выбирает кнопку «Отправить».</span><span class="sxs-lookup"><span data-stu-id="5cdd0-116">The user selects the submit button.</span></span> <span data-ttu-id="5cdd0-117">Браузер отправляет запрос и автоматически включает файл cookie проверки подлинности для запрошенного домена `www.good-banking-site.com`.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-117">The browser makes the request and automatically includes the authentication cookie for the requested domain, `www.good-banking-site.com`.</span></span>
1. <span data-ttu-id="5cdd0-118">Запрос выполняется на `www.good-banking-site.com` сервер с контекст проверки подлинности пользователя и может выполнять любое действие, которое может выполнять проверку подлинности пользователя.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-118">The request runs on the `www.good-banking-site.com` server with the user's authentication context and can perform any action that an authenticated user is allowed to perform.</span></span>

<span data-ttu-id="5cdd0-119">В дополнение к ситуации, когда пользователь выбирает эту кнопку для отправки формы вредоносный сайт может:</span><span class="sxs-lookup"><span data-stu-id="5cdd0-119">In addition to the scenario where the user selects the button to submit the form, the malicious site could:</span></span>

* <span data-ttu-id="5cdd0-120">Запустите скрипт, который автоматически отправляет форму.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-120">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="5cdd0-121">Отправка отправки формы в качестве AJAX-запросом.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-121">Send the form submission as an AJAX request.</span></span>
* <span data-ttu-id="5cdd0-122">Скройте форму с помощью CSS.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-122">Hide the form using CSS.</span></span>

<span data-ttu-id="5cdd0-123">Эти альтернативные сценарии не требуется, любое действие или входные данные пользователя, отличные от изначально посещения вредоносный сайт.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-123">These alternative scenarios don't require any action or input from the user other than initially visiting the malicious site.</span></span>

<span data-ttu-id="5cdd0-124">С помощью протокола HTTPS не предотвращает атаки CSRF.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-124">Using HTTPS doesn't prevent a CSRF attack.</span></span> <span data-ttu-id="5cdd0-125">Можно отправить вредоносный сайт `https://www.good-banking-site.com/` запроса так же легко, как его можно отправить запрос небезопасен.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-125">The malicious site can send an `https://www.good-banking-site.com/` request just as easily as it can send an insecure request.</span></span>

<span data-ttu-id="5cdd0-126">Некоторые атаки целевых конечных точек, которые отвечают на запросы GET, в этом случае тег изображения можно использовать для выполнения действия.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-126">Some attacks target endpoints that respond to GET requests, in which case an image tag can be used to perform the action.</span></span> <span data-ttu-id="5cdd0-127">Эта форма атаки проявляется на форуме сайтов, которые обеспечивают изображения, но блокирует JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-127">This form of attack is common on forum sites that permit images but block JavaScript.</span></span> <span data-ttu-id="5cdd0-128">Приложения, которые изменяют состояние для запросов GET, где переменные или ресурсов был изменен, уязвимы для атак злоумышленников.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-128">Apps that change state on GET requests, where variables or resources are altered, are vulnerable to malicious attacks.</span></span> <span data-ttu-id="5cdd0-129">**Запросов GET, которые изменяют состояние, не являются безопасными. Рекомендуется никогда не изменяют состояние на запрос GET.**</span><span class="sxs-lookup"><span data-stu-id="5cdd0-129">**GET requests that change state are insecure. A best practice is to never change state on a GET request.**</span></span>

<span data-ttu-id="5cdd0-130">Для веб-приложений, которые используют файлы cookie для проверки подлинности, так как возможны атаки CSRF:</span><span class="sxs-lookup"><span data-stu-id="5cdd0-130">CSRF attacks are possible against web apps that use cookies for authentication because:</span></span>

* <span data-ttu-id="5cdd0-131">Браузеры хранения файлов cookie, выданные веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-131">Browsers store cookies issued by a web app.</span></span>
* <span data-ttu-id="5cdd0-132">Хранимые файлы cookie содержат файлы cookie сеансов для прошедших проверку пользователей.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-132">Stored cookies include session cookies for authenticated users.</span></span>
* <span data-ttu-id="5cdd0-133">Браузеры отправляют все файлы cookie, связанные с домена к веб-приложению каждого запроса, независимо от того, как был создан запрос на приложение в браузере.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-133">Browsers send all of the cookies associated with a domain to the web app every request regardless of how the request to app was generated within the browser.</span></span>

<span data-ttu-id="5cdd0-134">Тем не менее, не ограничиваясь атаки CSRF использовать файлы cookie.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-134">However, CSRF attacks aren't limited to exploiting cookies.</span></span> <span data-ttu-id="5cdd0-135">Например Basic и дайджест-проверки подлинности также уязвимы.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-135">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="5cdd0-136">После пользователь выполняет вход с обычная или краткая проверка подлинности, браузер автоматически отправляет учетные данные до систему&dagger; заканчивается.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-136">After a user signs in with Basic or Digest authentication, the browser automatically sends the credentials until the session&dagger; ends.</span></span>

<span data-ttu-id="5cdd0-137">&dagger;В этом контексте *сеанса* ссылается на клиентский сеанс, во время которого пользователь проходит проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-137">&dagger;In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="5cdd0-138">Это не связанные сеансы на стороне сервера или [по промежуточного слоя сеанса ASP.NET Core](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="5cdd0-138">It's unrelated to server-side sessions or [ASP.NET Core Session Middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="5cdd0-139">Пользователям защититься от уязвимостей CSRF соблюдать меры предосторожности:</span><span class="sxs-lookup"><span data-stu-id="5cdd0-139">Users can guard against CSRF vulnerabilities by taking precautions:</span></span>

* <span data-ttu-id="5cdd0-140">Выйти из веб-приложения после завершения их использования.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-140">Sign off of web apps when finished using them.</span></span>
* <span data-ttu-id="5cdd0-141">Очистить файлы cookie в браузере периодически.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-141">Clear browser cookies periodically.</span></span>

<span data-ttu-id="5cdd0-142">Тем не менее уязвимости CSRF фактически являются проблемы с веб-приложения, а не конечным пользователем.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-142">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="authentication-fundamentals"></a><span data-ttu-id="5cdd0-143">Основы проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="5cdd0-143">Authentication fundamentals</span></span>

<span data-ttu-id="5cdd0-144">Проверка подлинности на основе файла Cookie является популярная форма проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-144">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="5cdd0-145">Токены проверки подлинности системы рост популярности, особенно для приложений на одной странице (SPAs).</span><span class="sxs-lookup"><span data-stu-id="5cdd0-145">Token-based authentication systems are growing in popularity, especially for Single Page Applications (SPAs).</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="5cdd0-146">Проверка подлинности на основе файлов cookie</span><span class="sxs-lookup"><span data-stu-id="5cdd0-146">Cookie-based authentication</span></span>

<span data-ttu-id="5cdd0-147">При прохождении пользователем проверки подлинности, используя имя пользователя и пароль, они выполняется выданный маркер, содержащий билет проверки подлинности, который может использоваться для проверки подлинности и авторизации.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-147">When a user authenticates using their username and password, they're issued a token, containing an authentication ticket that can be used for authentication and authorization.</span></span> <span data-ttu-id="5cdd0-148">Маркер сохраняется как файл cookie, который сопровождает каждый запрос клиента.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-148">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="5cdd0-149">Создание и проверка этот файл cookie осуществляется по промежуточного слоя проверки подлинности файла Cookie.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-149">Generating and validating this cookie is performed by the Cookie Authentication Middleware.</span></span> <span data-ttu-id="5cdd0-150">[По промежуточного слоя](xref:fundamentals/middleware/index) сериализует участника-пользователя в зашифрованном файле cookie.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-150">The [middleware](xref:fundamentals/middleware/index) serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="5cdd0-151">При последующих запросах по промежуточного слоя проверяет куки-файл, повторно создает основной и назначает участнику [пользователя](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) свойство [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="5cdd0-151">On subsequent requests, the middleware validates the cookie, recreates the principal, and assigns the principal to the [User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) property of [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span>

### <a name="token-based-authentication"></a><span data-ttu-id="5cdd0-152">Токены проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="5cdd0-152">Token-based authentication</span></span>

<span data-ttu-id="5cdd0-153">Если пользователь прошел проверку подлинности, выполняется выдаются ли они маркер (не сложные).</span><span class="sxs-lookup"><span data-stu-id="5cdd0-153">When a user is authenticated, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="5cdd0-154">Токен содержит сведения о пользователе в виде [утверждений](/dotnet/framework/security/claims-based-identity-model) или ссылка маркер, указывающий приложения состояния пользователя в приложении.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-154">The token contains user information in the form of [claims](/dotnet/framework/security/claims-based-identity-model) or a reference token that points the app to user state maintained in the app.</span></span> <span data-ttu-id="5cdd0-155">Когда пользователь пытается получить доступ к ресурсу, требующей проверки подлинности, маркер отправляется в приложении с помощью заголовок дополнительных авторизации в виде токена носителя.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-155">When a user attempts to access a resource requiring authentication, the token is sent to the app with an additional authorization header in form of Bearer token.</span></span> <span data-ttu-id="5cdd0-156">Это делает приложение без сохранения состояния.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-156">This makes the app stateless.</span></span> <span data-ttu-id="5cdd0-157">Во все последующие запросы что токен передается в запросе для проверки на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-157">In each subsequent request, the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="5cdd0-158">Этот токен не *зашифрованные*; он имеет *кодировке*.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-158">This token isn't *encrypted*; it's *encoded*.</span></span> <span data-ttu-id="5cdd0-159">На сервере декодирует токен для доступа к информации.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-159">On the server, the token is decoded to access its information.</span></span> <span data-ttu-id="5cdd0-160">Маркер отправляется в последующих запросах, сохранения токена в локальном хранилище браузера.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-160">To send the token on subsequent requests, store the token in the browser's local storage.</span></span> <span data-ttu-id="5cdd0-161">Не стоит беспокоиться о CSRF уязвимости, если маркер хранится в локальном хранилище браузера.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-161">Don't be concerned about CSRF vulnerability if the token is stored in the browser's local storage.</span></span> <span data-ttu-id="5cdd0-162">CSRF возникает, когда токен хранится в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-162">CSRF is a concern when the token is stored in a cookie.</span></span>

### <a name="multiple-apps-hosted-at-one-domain"></a><span data-ttu-id="5cdd0-163">Несколько приложений, размещенных в одном домене</span><span class="sxs-lookup"><span data-stu-id="5cdd0-163">Multiple apps hosted at one domain</span></span>

<span data-ttu-id="5cdd0-164">Общими средами размещения уязвимы для захвата сеанса входа CSRF и других атак.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-164">Shared hosting environments are vulnerable to session hijacking, login CSRF, and other attacks.</span></span>

<span data-ttu-id="5cdd0-165">Несмотря на то что `example1.contoso.net` и `example2.contoso.net` разных узлах, которые есть неявные доверительные отношения между узлы на `*.contoso.net` домена.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-165">Although `example1.contoso.net` and `example2.contoso.net` are different hosts, there's an implicit trust relationship between hosts under the `*.contoso.net` domain.</span></span> <span data-ttu-id="5cdd0-166">Неявные отношения доверия между позволяет потенциально небезопасных узлам влияет на файлы cookie друг друга (политики одного источника, которые управляют запросы AJAX не относиться к файлы cookie HTTP).</span><span class="sxs-lookup"><span data-stu-id="5cdd0-166">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span>

<span data-ttu-id="5cdd0-167">Можно предотвратить атак, использующих доверенных файлов cookie между приложения, размещенные на том же домене, не предоставив доменов.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-167">Attacks that exploit trusted cookies between apps hosted on the same domain can be prevented by not sharing domains.</span></span> <span data-ttu-id="5cdd0-168">Когда каждое приложение размещается в собственном домене, не задано отношение доверия неявное куки-файл для использования.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-168">When each app is hosted on its own domain, there is no implicit cookie trust relationship to exploit.</span></span>

## <a name="aspnet-core-antiforgery-configuration"></a><span data-ttu-id="5cdd0-169">Сложные конфигурации ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5cdd0-169">ASP.NET Core antiforgery configuration</span></span>

> [!WARNING]
> <span data-ttu-id="5cdd0-170">ASP.NET Core реализует сложные с помощью [защиты данных ASP.NET Core](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="5cdd0-170">ASP.NET Core implements antiforgery using [ASP.NET Core Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="5cdd0-171">Стек защиты данных должен быть настроен для работы в ферме серверов.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-171">The data protection stack must be configured to work in a server farm.</span></span> <span data-ttu-id="5cdd0-172">В разделе [настройки защиты данных](xref:security/data-protection/configuration/overview) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-172">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="5cdd0-173">В ASP.NET Core 2.0 или более поздней версии [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) внедряет сложные токены в форме HTML-элементов.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-173">In ASP.NET Core 2.0 or later, the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span> <span data-ttu-id="5cdd0-174">Приведенный ниже код в файле Razor автоматически создает сложные токены:</span><span class="sxs-lookup"><span data-stu-id="5cdd0-174">The following markup in a Razor file automatically generates antiforgery tokens:</span></span>

```cshtml
<form method="post">
    ...
</form>
```

<span data-ttu-id="5cdd0-175">Аналогично при попытке [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) создает сложные токенов по умолчанию, если метод формы не GET.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-175">Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generates antiforgery tokens by default if the form's method isn't GET.</span></span>

<span data-ttu-id="5cdd0-176">Происходит автоматическое создание сложные маркеров для элементов формы HTML при `<form>` тег содержит `method="post"` верны атрибут и одно из следующих:</span><span class="sxs-lookup"><span data-stu-id="5cdd0-176">The automatic generation of antiforgery tokens for HTML form elements happens when the `<form>` tag contains the `method="post"` attribute and either of the following are true:</span></span>

  * <span data-ttu-id="5cdd0-177">Действие атрибута пуст (`action=""`).</span><span class="sxs-lookup"><span data-stu-id="5cdd0-177">The action attribute is empty (`action=""`).</span></span>
  * <span data-ttu-id="5cdd0-178">Не указан атрибут действия (`<form method="post">`).</span><span class="sxs-lookup"><span data-stu-id="5cdd0-178">The action attribute isn't supplied (`<form method="post">`).</span></span>

<span data-ttu-id="5cdd0-179">Можно отключить автоматическое создание сложные маркеров для элементов формы HTML.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-179">Automatic generation of antiforgery tokens for HTML form elements can be disabled:</span></span>

* <span data-ttu-id="5cdd0-180">Явно отключить сложные токены с `asp-antiforgery` атрибута:</span><span class="sxs-lookup"><span data-stu-id="5cdd0-180">Explicitly disable antiforgery tokens with the `asp-antiforgery` attribute:</span></span>

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* <span data-ttu-id="5cdd0-181">Элемент form согласился извлечения вспомогательных функций тегов с помощью вспомогательного тег [! символ отказаться](xref:mvc/views/tag-helpers/intro#opt-out):</span><span class="sxs-lookup"><span data-stu-id="5cdd0-181">The form element is opted-out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out):</span></span>

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* <span data-ttu-id="5cdd0-182">Удалите `FormTagHelper` из представления.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-182">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="5cdd0-183">`FormTagHelper` Можно удалить из представления путем добавления представления Razor следующую директиву:</span><span class="sxs-lookup"><span data-stu-id="5cdd0-183">The `FormTagHelper` can be removed from a view by adding the following directive to the Razor view:</span></span>

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="5cdd0-184">[Страниц Razor](xref:mvc/razor-pages/index) автоматически защищены от XSRF-CSRF.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-184">[Razor Pages](xref:mvc/razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="5cdd0-185">Дополнительные сведения см. в разделе [XSRF/CSRF и страниц Razor](xref:mvc/razor-pages/index#xsrf).</span><span class="sxs-lookup"><span data-stu-id="5cdd0-185">For more information, see [XSRF/CSRF and Razor Pages](xref:mvc/razor-pages/index#xsrf).</span></span>

<span data-ttu-id="5cdd0-186">Чтобы защититься от атак CSRF наиболее распространенным подходом является использование *синхронизатор шаблона токена* (STP).</span><span class="sxs-lookup"><span data-stu-id="5cdd0-186">The most common approach to defending against CSRF attacks is to use the *Synchronizer Token Pattern* (STP).</span></span> <span data-ttu-id="5cdd0-187">STP используется в том случае, если пользователь запрашивает страницу с данными в форме:</span><span class="sxs-lookup"><span data-stu-id="5cdd0-187">STP is used when the user requests a page with form data:</span></span>

1. <span data-ttu-id="5cdd0-188">Сервер отправляет токен отмены, связанный с удостоверением текущего пользователя для клиента.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-188">The server sends a token associated with the current user's identity to the client.</span></span>
1. <span data-ttu-id="5cdd0-189">Клиент отправляет маркер на сервер для проверки.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-189">The client sends back the token to the server for verification.</span></span>
1. <span data-ttu-id="5cdd0-190">Если сервер получает маркер, который не совпадает с удостоверением пользователя, прошедшего проверку подлинности, запрос отклоняется.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-190">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span>

<span data-ttu-id="5cdd0-191">Маркер является уникальным и непредсказуемым.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-191">The token is unique and unpredictable.</span></span> <span data-ttu-id="5cdd0-192">Маркер может также использоваться для обеспечения правильной последовательности серию запросов (например, обеспечивая последовательность запроса: страницы 1 &ndash; страницу 2 &ndash; страница 3).</span><span class="sxs-lookup"><span data-stu-id="5cdd0-192">The token can also be used to ensure proper sequencing of a series of requests (for example, ensuring the request sequence of: page 1 &ndash; page 2 &ndash; page 3).</span></span> <span data-ttu-id="5cdd0-193">Все формы в ASP.NET Core MVC и страниц Razor шаблоны создавать сложные маркеры.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-193">All of the forms in ASP.NET Core MVC and Razor Pages templates generate antiforgery tokens.</span></span> <span data-ttu-id="5cdd0-194">В следующей паре примеры режима создавать сложные маркеры:</span><span class="sxs-lookup"><span data-stu-id="5cdd0-194">The following pair of view examples generate antiforgery tokens:</span></span>

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

<span data-ttu-id="5cdd0-195">Явным образом добавить сложные токен `<form>` элемент без использования вспомогательных функций тегов с вспомогательный метод HTML [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span><span class="sxs-lookup"><span data-stu-id="5cdd0-195">Explicitly add an antiforgery token to a `<form>` element without using Tag Helpers with the HTML helper [@Html.AntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span></span>

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="5cdd0-196">Во всех перечисленных случаях ASP.NET Core добавляет скрытое поле формы следующего вида:</span><span class="sxs-lookup"><span data-stu-id="5cdd0-196">In each of the preceding cases, ASP.NET Core adds a hidden form field similar to the following:</span></span>

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

<span data-ttu-id="5cdd0-197">ASP.NET Core включает в себя три [фильтры](xref:mvc/controllers/filters) для работы с сложные токены:</span><span class="sxs-lookup"><span data-stu-id="5cdd0-197">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens:</span></span>

* [<span data-ttu-id="5cdd0-198">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="5cdd0-198">ValidateAntiForgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [<span data-ttu-id="5cdd0-199">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="5cdd0-199">AutoValidateAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [<span data-ttu-id="5cdd0-200">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="5cdd0-200">IgnoreAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a><span data-ttu-id="5cdd0-201">Сложные параметры.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-201">Antiforgery options</span></span>

<span data-ttu-id="5cdd0-202">Настройка [сложные параметры](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5cdd0-202">Customize [antiforgery options](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span></span>

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

| <span data-ttu-id="5cdd0-203">Параметр</span><span class="sxs-lookup"><span data-stu-id="5cdd0-203">Option</span></span> | <span data-ttu-id="5cdd0-204">Описание:</span><span class="sxs-lookup"><span data-stu-id="5cdd0-204">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="5cdd0-205">Файл cookie</span><span class="sxs-lookup"><span data-stu-id="5cdd0-205">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="5cdd0-206">Определяет параметры, используемые для создания сложные файлы cookie.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-206">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="5cdd0-207">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="5cdd0-207">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | <span data-ttu-id="5cdd0-208">Домен cookie.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-208">The domain of the cookie.</span></span> <span data-ttu-id="5cdd0-209">По умолчанию — `null`.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-209">Defaults to `null`.</span></span> <span data-ttu-id="5cdd0-210">Это свойство является устаревшим и будет удален в будущей версии.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-210">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="5cdd0-211">Взамен рекомендуется использовать — Cookie.Domain.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-211">The recommended alternative is Cookie.Domain.</span></span> |
| [<span data-ttu-id="5cdd0-212">CookieName</span><span class="sxs-lookup"><span data-stu-id="5cdd0-212">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | <span data-ttu-id="5cdd0-213">Имя файла cookie.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-213">The name of the cookie.</span></span> <span data-ttu-id="5cdd0-214">Если не задано, система создает уникальное имя которого начинается с [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (». AspNetCore.Antiforgery.»).</span><span class="sxs-lookup"><span data-stu-id="5cdd0-214">If not set, the system generates a unique name beginning with the [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (".AspNetCore.Antiforgery.").</span></span> <span data-ttu-id="5cdd0-215">Это свойство является устаревшим и будет удален в будущей версии.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-215">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="5cdd0-216">Взамен рекомендуется использовать — Cookie.Name.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-216">The recommended alternative is Cookie.Name.</span></span> |
| [<span data-ttu-id="5cdd0-217">CookiePath</span><span class="sxs-lookup"><span data-stu-id="5cdd0-217">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | <span data-ttu-id="5cdd0-218">Путь задать в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-218">The path set on the cookie.</span></span> <span data-ttu-id="5cdd0-219">Это свойство является устаревшим и будет удален в будущей версии.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-219">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="5cdd0-220">Взамен рекомендуется использовать — Cookie.Path.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-220">The recommended alternative is Cookie.Path.</span></span> |
| [<span data-ttu-id="5cdd0-221">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="5cdd0-221">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="5cdd0-222">Имя скрытое поле формы используются сложные системой для подготовки к просмотру сложные токены в представлениях.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-222">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="5cdd0-223">HeaderName</span><span class="sxs-lookup"><span data-stu-id="5cdd0-223">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="5cdd0-224">Имя заголовка, используемые системой сложные.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-224">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="5cdd0-225">Если `null`, то система рассматривает только данные формы.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-225">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="5cdd0-226">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="5cdd0-226">RequireSsl</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | <span data-ttu-id="5cdd0-227">Указывает, требуется ли SSL сложные системой.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-227">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="5cdd0-228">Если `true`, без использования SSL не установится.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-228">If `true`, non-SSL requests fail.</span></span> <span data-ttu-id="5cdd0-229">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-229">Defaults to `false`.</span></span> <span data-ttu-id="5cdd0-230">Это свойство является устаревшим и будет удален в будущей версии.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-230">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="5cdd0-231">Взамен рекомендуется использовать является установка Cookie.SecurePolicy.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-231">The recommended alternative is to set Cookie.SecurePolicy.</span></span> |
| [<span data-ttu-id="5cdd0-232">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="5cdd0-232">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="5cdd0-233">Указывает, следует ли подавлять поколение `X-Frame-Options` заголовок.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-233">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="5cdd0-234">По умолчанию заголовок создается со значением «SAMEORIGIN».</span><span class="sxs-lookup"><span data-stu-id="5cdd0-234">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="5cdd0-235">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-235">Defaults to `false`.</span></span> |

<span data-ttu-id="5cdd0-236">Дополнительные сведения см. в разделе [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span><span class="sxs-lookup"><span data-stu-id="5cdd0-236">For more information, see [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span></span>

## <a name="configure-antiforgery-features-with-iantiforgery"></a><span data-ttu-id="5cdd0-237">Настроить сложные функции с IAntiforgery</span><span class="sxs-lookup"><span data-stu-id="5cdd0-237">Configure antiforgery features with IAntiforgery</span></span>

<span data-ttu-id="5cdd0-238">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) предоставляет API, чтобы настроить сложные функции.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-238">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) provides the API to configure antiforgery features.</span></span> <span data-ttu-id="5cdd0-239">`IAntiforgery` можно запросить в `Configure` метод `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-239">`IAntiforgery` can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="5cdd0-240">В следующем примере ПО промежуточного слоя с домашней страницы приложения для создания токена сложные и его отправки в ответ в виде куки-файл (с помощью именования по умолчанию углового описано далее в этом разделе):</span><span class="sxs-lookup"><span data-stu-id="5cdd0-240">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described later in this topic):</span></span>

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

### <a name="require-antiforgery-validation"></a><span data-ttu-id="5cdd0-241">Требовать проверку сложные</span><span class="sxs-lookup"><span data-stu-id="5cdd0-241">Require antiforgery validation</span></span>

<span data-ttu-id="5cdd0-242">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) является фильтром действий, могут применяться для отдельного действия, контроллера или глобально.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-242">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="5cdd0-243">Запросы, адресованные действий, имеющих этот фильтр, применяемый будут заблокированы, если запрос содержит сложные допустимый токен.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-243">Requests made to actions that have this filter applied are blocked unless the request includes a valid antiforgery token.</span></span>

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

<span data-ttu-id="5cdd0-244">`ValidateAntiForgeryToken` Атрибута требуется маркер для запросов к методам действий, в которой относится, включая HTTP-запросы GET.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-244">The `ValidateAntiForgeryToken` attribute requires a token for requests to the action methods it decorates, including HTTP GET requests.</span></span> <span data-ttu-id="5cdd0-245">Если `ValidateAntiForgeryToken` атрибут между контроллерами приложения, его можно переопределить с помощью `IgnoreAntiforgeryToken` атрибута.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-245">If the `ValidateAntiForgeryToken` attribute is applied across the app's controllers, it can be overridden with the `IgnoreAntiforgeryToken` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="5cdd0-246">ASP.NET Core не поддерживает автоматическое добавление маркеров сложные запросы GET.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-246">ASP.NET Core doesn't support adding antiforgery tokens to GET requests automatically.</span></span>

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a><span data-ttu-id="5cdd0-247">Автоматически проверки сложные токенов небезопасный HTTP только для методов</span><span class="sxs-lookup"><span data-stu-id="5cdd0-247">Automatically validate antiforgery tokens for unsafe HTTP methods only</span></span>

<span data-ttu-id="5cdd0-248">Приложения ASP.NET Core не создавать сложные маркерах безопасные методы HTTP (GET, HEAD, параметры и ТРАССИРОВКИ).</span><span class="sxs-lookup"><span data-stu-id="5cdd0-248">ASP.NET Core apps don't generate antiforgery tokens for safe HTTP methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="5cdd0-249">Вместо применения широко `ValidateAntiForgeryToken` атрибута и затем переопределение его с `IgnoreAntiforgeryToken` атрибуты, [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) атрибут может использоваться.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-249">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, the [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribute can be used.</span></span> <span data-ttu-id="5cdd0-250">Этот атрибут работает идентично методу `ValidateAntiForgeryToken` атрибута, за исключением того, что он не требует токены для запросов, выполненных с помощью следующих методов HTTP:</span><span class="sxs-lookup"><span data-stu-id="5cdd0-250">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="5cdd0-251">GET</span><span class="sxs-lookup"><span data-stu-id="5cdd0-251">GET</span></span>
* <span data-ttu-id="5cdd0-252">HEAD,</span><span class="sxs-lookup"><span data-stu-id="5cdd0-252">HEAD</span></span>
* <span data-ttu-id="5cdd0-253">OPTIONS</span><span class="sxs-lookup"><span data-stu-id="5cdd0-253">OPTIONS</span></span>
* <span data-ttu-id="5cdd0-254">TRACE</span><span class="sxs-lookup"><span data-stu-id="5cdd0-254">TRACE</span></span>

<span data-ttu-id="5cdd0-255">Рекомендуется использовать `AutoValidateAntiforgeryToken` широко для сценариев, отличных от API.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-255">We recommend use of `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="5cdd0-256">Это гарантирует, что действия после защищены по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-256">This ensures POST actions are protected by default.</span></span> <span data-ttu-id="5cdd0-257">Вместо этого должен игнорировать сложные токены по умолчанию, если не `ValidateAntiForgeryToken` применяется для отдельных методов действий.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-257">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to individual action methods.</span></span> <span data-ttu-id="5cdd0-258">Он более вероятно в этом сценарии для метода POST действие должно быть оставлено незащищенными по ошибке, оставляя приложение уязвимым для атак CSRF.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-258">It's more likely in this scenario for a POST action method to be left unprotected by mistake, leaving the app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="5cdd0-259">Все сообщения, необходимо отправить токен сложные.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-259">All POSTs should send the antiforgery token.</span></span>

<span data-ttu-id="5cdd0-260">API-интерфейсы не имеют автоматический механизм для отправки не cookie часть маркера.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-260">APIs don't have an automatic mechanism for sending the non-cookie part of the token.</span></span> <span data-ttu-id="5cdd0-261">Реализация, скорее всего, зависит от реализации кода клиента.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-261">The implementation probably depends on the client code implementation.</span></span> <span data-ttu-id="5cdd0-262">Ниже приведены некоторые примеры:</span><span class="sxs-lookup"><span data-stu-id="5cdd0-262">Some examples are shown below:</span></span>

<span data-ttu-id="5cdd0-263">Пример уровня класса.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-263">Class-level example:</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="5cdd0-264">Пример глобального.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-264">Global example:</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a><span data-ttu-id="5cdd0-265">Переопределение глобальных или сложные атрибутов контроллера</span><span class="sxs-lookup"><span data-stu-id="5cdd0-265">Override global or controller antiforgery attributes</span></span>

<span data-ttu-id="5cdd0-266">[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) фильтр используется для устранения необходимости в сложные маркера для данного действия (или контроллера).</span><span class="sxs-lookup"><span data-stu-id="5cdd0-266">The [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filter is used to eliminate the need for an antiforgery token for a given action (or controller).</span></span> <span data-ttu-id="5cdd0-267">При применении этого фильтра переопределяет `ValidateAntiForgeryToken` и `AutoValidateAntiforgeryToken` фильтров, указанных на более высоком уровне (глобально или на контроллере).</span><span class="sxs-lookup"><span data-stu-id="5cdd0-267">When applied, this filter overrides `ValidateAntiForgeryToken` and `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

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

## <a name="refresh-tokens-after-authentication"></a><span data-ttu-id="5cdd0-268">Токенов обновления после проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="5cdd0-268">Refresh tokens after authentication</span></span>

<span data-ttu-id="5cdd0-269">Маркеры должны обновляться после проверки подлинности пользователя путем перенаправления пользователя на просмотр или страницу страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-269">Tokens should be refreshed after the user is authenticated by redirecting the user to a view or Razor Pages page.</span></span>

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="5cdd0-270">JavaScript, AJAX и SPAs</span><span class="sxs-lookup"><span data-stu-id="5cdd0-270">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="5cdd0-271">В традиционные приложения на основе HTML сложные токены, передаются на сервер с помощью скрытые поля формы.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-271">In traditional HTML-based apps, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="5cdd0-272">В современных приложений на базе JavaScript и SPAs многие запросы выполняются программно.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-272">In modern JavaScript-based apps and SPAs, many requests are made programmatically.</span></span> <span data-ttu-id="5cdd0-273">Маркер отправляется в эти запросы AJAX может использовать другие методы (например, заголовки запроса или куки-файлы).</span><span class="sxs-lookup"><span data-stu-id="5cdd0-273">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span>

<span data-ttu-id="5cdd0-274">Если файлы cookie используются для хранения токенов проверки подлинности и проверки подлинности запросов API на сервере, CSRF может стать проблемой.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-274">If cookies are used to store authentication tokens and to authenticate API requests on the server, CSRF is a potential problem.</span></span> <span data-ttu-id="5cdd0-275">При использовании локального хранилища для сохранения токена CSRF уязвимость может устранить, так как значения из локального хранилища не отправляются автоматически на сервер с каждым запросом.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-275">If local storage is used to store the token, CSRF vulnerability might be mitigated because values from local storage aren't sent automatically to the server with every request.</span></span> <span data-ttu-id="5cdd0-276">Таким образом использует локальное хранилище для хранения сложные маркера на клиенте и отправка маркера как заголовок запроса является рекомендуемым.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-276">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="javascript"></a><span data-ttu-id="5cdd0-277">JavaScript</span><span class="sxs-lookup"><span data-stu-id="5cdd0-277">JavaScript</span></span>

<span data-ttu-id="5cdd0-278">С представлениями с использованием JavaScript, токен можно создать с помощью службы из представления.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-278">Using JavaScript with views, the token can be created using a service from within the view.</span></span> <span data-ttu-id="5cdd0-279">Вставить [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) в представлении и вызовите службу [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span><span class="sxs-lookup"><span data-stu-id="5cdd0-279">Inject the [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service into the view and call [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

<span data-ttu-id="5cdd0-280">Этот подход избавляет от необходимости работать напрямую с Установка с сервера файлы cookie или их считывания из клиента.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-280">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="5cdd0-281">В предыдущем примере использовался JavaScript для чтения значение скрытого поля заголовка AJAX POST.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-281">The preceding example uses JavaScript to read the hidden field value for the AJAX POST header.</span></span>

<span data-ttu-id="5cdd0-282">JavaScript можно также токенов в файлах cookie доступа и использовать содержимое файла cookie для создания заголовка с значение токена.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-282">JavaScript can also access tokens in cookies and use the cookie's contents to create a header with the token's value.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="5cdd0-283">При условии, что скрипт запрашивает Отправка токена в заголовок с именем `X-CSRF-TOKEN`, настройте службу сложные искать `X-CSRF-TOKEN` заголовка:</span><span class="sxs-lookup"><span data-stu-id="5cdd0-283">Assuming the script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="5cdd0-284">В следующем примере используется JavaScript для выполнения запроса с соответствующий заголовок AJAX:</span><span class="sxs-lookup"><span data-stu-id="5cdd0-284">The following example uses JavaScript to make an AJAX request with the appropriate header:</span></span>

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

### <a name="angularjs"></a><span data-ttu-id="5cdd0-285">AngularJS</span><span class="sxs-lookup"><span data-stu-id="5cdd0-285">AngularJS</span></span>

<span data-ttu-id="5cdd0-286">AngularJS используется соглашение по адресу CSRF.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-286">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="5cdd0-287">Если сервер отправляет куки-файл с именем `XSRF-TOKEN`, AngularJS `$http` служба добавляет значение файла cookie в заголовок при отправке запроса на сервер.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-287">If the server sends a cookie with the name `XSRF-TOKEN`, the AngularJS `$http` service adds the cookie value to a header when it sends a request to the server.</span></span> <span data-ttu-id="5cdd0-288">Этот процесс выполняется автоматически.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-288">This process is automatic.</span></span> <span data-ttu-id="5cdd0-289">Заголовок не нужно задавать явно.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-289">The header doesn't need to be set explicitly.</span></span> <span data-ttu-id="5cdd0-290">Имя заголовка — `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-290">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="5cdd0-291">Сервер должен обнаружить этот заголовок и проверьте его содержимое.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-291">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="5cdd0-292">Для ASP.NET Core API работают с этим соглашением.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-292">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="5cdd0-293">Настройте приложение, чтобы предоставить токен в файле cookie, вызывается `XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-293">Configure your app to provide a token in a cookie called `XSRF-TOKEN`.</span></span>
* <span data-ttu-id="5cdd0-294">Настройте службу сложные поиск заголовка с именем `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-294">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`.</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="5cdd0-295">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5cdd0-295">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="extend-antiforgery"></a><span data-ttu-id="5cdd0-296">Расширить antiforgery</span><span class="sxs-lookup"><span data-stu-id="5cdd0-296">Extend antiforgery</span></span>

<span data-ttu-id="5cdd0-297">[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) типа позволяет разработчикам расширять поведение системы защиты CSRF с циклической обработки дополнительных данных в каждом маркере.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-297">The [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-CSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="5cdd0-298">[GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) при каждом вызове метода создается маркер поля, и возвращаемое значение внедряется в созданный токен.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-298">The [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="5cdd0-299">Разработчик может возвращать отметку времени, nonce или любое другое значение, а затем вызвать [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) для проверки данных, если маркер проверяется.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-299">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) to validate this data when the token is validated.</span></span> <span data-ttu-id="5cdd0-300">Имя пользователя клиента уже внедрена в создаваемые маркеры, поэтому нет необходимости включать эти сведения.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-300">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="5cdd0-301">Если токен содержит дополнительные данные, но нет `IAntiForgeryAdditionalDataProvider` будет настроен, дополнительные данные не проверяются.</span><span class="sxs-lookup"><span data-stu-id="5cdd0-301">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` is configured, the supplemental data isn't validated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5cdd0-302">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="5cdd0-302">Additional resources</span></span>

* <span data-ttu-id="5cdd0-303">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) на [откройте проект безопасности веб-приложения](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="5cdd0-303">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
