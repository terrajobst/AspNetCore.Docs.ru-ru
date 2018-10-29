---
title: Предотвращения межсайтовых (запросов XSRF/CSRF) атак с подделкой в ASP.NET Core
author: steve-smith
description: Узнайте, как предотвратить атаки, направленные на веб-приложений, где вредоносный сайт может влиять на взаимодействие между клиентским браузером и приложения.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/anti-request-forgery
ms.openlocfilehash: c4a512e5518380f5f0a43d08cd0bcba2f8c26141
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207671"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="14953-103">Предотвращения межсайтовых (запросов XSRF/CSRF) атак с подделкой в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="14953-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="14953-104">По [Стив Смит](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="14953-104">By [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="14953-105">Подделки межсайтовых запросов (также известные как XSRF или CSRF, произносится *см. в статье работа в Интернете*) — это атака, от приложений, размещенных на веб сервере, при котором вредоносные веб-приложения может влиять на взаимодействие между клиентским браузером и веб-приложение, которому доверяет, Обозреватель.</span><span class="sxs-lookup"><span data-stu-id="14953-105">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted apps whereby a malicious web app can influence the interaction between a client browser and a web app that trusts that browser.</span></span> <span data-ttu-id="14953-106">Эти атаки возможны, так как веб-браузеры отправляют некоторых типов маркеров проверки подлинности автоматически при каждом запросе веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="14953-106">These attacks are possible because web browsers send some types of authentication tokens automatically with every request to a website.</span></span> <span data-ttu-id="14953-107">Такая форма эксплойта также называется *атаки одним щелчком* или *«session riding»* так, как атаки использует преимущества пользователь ранее проверку подлинности сеанса.</span><span class="sxs-lookup"><span data-stu-id="14953-107">This form of exploit is also known as a *one-click attack* or *session riding* because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="14953-108">Примером атаки CSRF:</span><span class="sxs-lookup"><span data-stu-id="14953-108">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="14953-109">Пользователь входит в `www.good-banking-site.com` проверки подлинности с помощью форм.</span><span class="sxs-lookup"><span data-stu-id="14953-109">A user signs into `www.good-banking-site.com` using forms authentication.</span></span> <span data-ttu-id="14953-110">Сервер проверяет подлинность пользователя и выдает ответ, включающий файл cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="14953-110">The server authenticates the user and issues a response that includes an authentication cookie.</span></span> <span data-ttu-id="14953-111">Сайт является уязвимым для атак, так как она доверяет любой запрос, получающий объект cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="14953-111">The site is vulnerable to attack because it trusts any request that it receives with a valid authentication cookie.</span></span>
1. <span data-ttu-id="14953-112">Пользователь посещает веб, `www.bad-crook-site.com`.</span><span class="sxs-lookup"><span data-stu-id="14953-112">The user visits a malicious site, `www.bad-crook-site.com`.</span></span>

   <span data-ttu-id="14953-113">Вредоносный сайт `www.bad-crook-site.com`, с HTML-формой следующего вида:</span><span class="sxs-lookup"><span data-stu-id="14953-113">The malicious site, `www.bad-crook-site.com`, contains an HTML form similar to the following:</span></span>

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   <span data-ttu-id="14953-114">Обратите внимание, что формы `action` сообщения на уязвимом сайте, а не к вредоносный сайт.</span><span class="sxs-lookup"><span data-stu-id="14953-114">Notice that the form's `action` posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="14953-115">Это часть CSRF «cross-site».</span><span class="sxs-lookup"><span data-stu-id="14953-115">This is the "cross-site" part of CSRF.</span></span>

1. <span data-ttu-id="14953-116">Пользователь выбирает кнопку отправки.</span><span class="sxs-lookup"><span data-stu-id="14953-116">The user selects the submit button.</span></span> <span data-ttu-id="14953-117">Браузер выполняет запрос и автоматически включает в себя файл cookie проверки подлинности для запрошенного домена `www.good-banking-site.com`.</span><span class="sxs-lookup"><span data-stu-id="14953-117">The browser makes the request and automatically includes the authentication cookie for the requested domain, `www.good-banking-site.com`.</span></span>
1. <span data-ttu-id="14953-118">Запрос выполняется на `www.good-banking-site.com` сервера с контекстом проверки подлинности пользователя и могут выполнять любые действия, прошедший проверку пользователь может выполнять.</span><span class="sxs-lookup"><span data-stu-id="14953-118">The request runs on the `www.good-banking-site.com` server with the user's authentication context and can perform any action that an authenticated user is allowed to perform.</span></span>

<span data-ttu-id="14953-119">В дополнение к сценарии, где пользователь выбирает кнопку, чтобы отправить форму вредоносный сайт может:</span><span class="sxs-lookup"><span data-stu-id="14953-119">In addition to the scenario where the user selects the button to submit the form, the malicious site could:</span></span>

* <span data-ttu-id="14953-120">Запустите скрипт, который автоматически отправляет форму.</span><span class="sxs-lookup"><span data-stu-id="14953-120">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="14953-121">Отправка отправки формы в качестве AJAX-запросом.</span><span class="sxs-lookup"><span data-stu-id="14953-121">Send the form submission as an AJAX request.</span></span>
* <span data-ttu-id="14953-122">Скрывайте форму с помощью CSS.</span><span class="sxs-lookup"><span data-stu-id="14953-122">Hide the form using CSS.</span></span>

<span data-ttu-id="14953-123">Эти альтернативные сценарии не требуется, любые действия или входные данные пользователя, отличные от изначально узле вредоносных.</span><span class="sxs-lookup"><span data-stu-id="14953-123">These alternative scenarios don't require any action or input from the user other than initially visiting the malicious site.</span></span>

<span data-ttu-id="14953-124">С помощью протокола HTTPS не предотвращает атаки CSRF.</span><span class="sxs-lookup"><span data-stu-id="14953-124">Using HTTPS doesn't prevent a CSRF attack.</span></span> <span data-ttu-id="14953-125">Можно отправить вредоносный сайт `https://www.good-banking-site.com/` запроса так же легко, он может отправлять небезопасный запрос.</span><span class="sxs-lookup"><span data-stu-id="14953-125">The malicious site can send an `https://www.good-banking-site.com/` request just as easily as it can send an insecure request.</span></span>

<span data-ttu-id="14953-126">Некоторые атаки направлены конечных точек, которые отвечают на запросы GET, в этом случае тег изображения может использоваться для выполнения действия.</span><span class="sxs-lookup"><span data-stu-id="14953-126">Some attacks target endpoints that respond to GET requests, in which case an image tag can be used to perform the action.</span></span> <span data-ttu-id="14953-127">Эту форму атаки характерен для веб-узлов форума, которые обеспечивают образов, но блокировка JavaScript.</span><span class="sxs-lookup"><span data-stu-id="14953-127">This form of attack is common on forum sites that permit images but block JavaScript.</span></span> <span data-ttu-id="14953-128">Приложения, которые изменяют состояние для запросов GET, где переменные или ресурсов, изменяются, уязвимы для атак злоумышленников.</span><span class="sxs-lookup"><span data-stu-id="14953-128">Apps that change state on GET requests, where variables or resources are altered, are vulnerable to malicious attacks.</span></span> <span data-ttu-id="14953-129">**Запросы GET, которые изменяют состояние сделаны ненадежными. Рекомендуется никогда не изменить состояние в запрос GET.**</span><span class="sxs-lookup"><span data-stu-id="14953-129">**GET requests that change state are insecure. A best practice is to never change state on a GET request.**</span></span>

<span data-ttu-id="14953-130">Для веб-приложений, использующих файлы cookie для проверки подлинности, так как возможны CSRF-атакам.</span><span class="sxs-lookup"><span data-stu-id="14953-130">CSRF attacks are possible against web apps that use cookies for authentication because:</span></span>

* <span data-ttu-id="14953-131">Обозреватели сохраняют файлы Сookie, выпущенные веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="14953-131">Browsers store cookies issued by a web app.</span></span>
* <span data-ttu-id="14953-132">Хранимые файлы cookie содержат файлы cookie сеанса для прошедших проверку пользователей.</span><span class="sxs-lookup"><span data-stu-id="14953-132">Stored cookies include session cookies for authenticated users.</span></span>
* <span data-ttu-id="14953-133">Браузеры отправляют все файлы cookie, связанную с доменом, в веб-приложение каждого запроса, независимо от того, как был создан запрос на приложение в браузере.</span><span class="sxs-lookup"><span data-stu-id="14953-133">Browsers send all of the cookies associated with a domain to the web app every request regardless of how the request to app was generated within the browser.</span></span>

<span data-ttu-id="14953-134">Тем не менее, не ограничены CSRF-атакам использования файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="14953-134">However, CSRF attacks aren't limited to exploiting cookies.</span></span> <span data-ttu-id="14953-135">Например Basic и дайджест-проверки подлинности также уязвимы.</span><span class="sxs-lookup"><span data-stu-id="14953-135">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="14953-136">После пользователь выполняет вход с обычной или дайджест-проверки подлинности, браузер автоматически отправляет учетные данные до систему&dagger; заканчивается.</span><span class="sxs-lookup"><span data-stu-id="14953-136">After a user signs in with Basic or Digest authentication, the browser automatically sends the credentials until the session&dagger; ends.</span></span>

<span data-ttu-id="14953-137">&dagger;В этом контексте *сеанса* ссылается на сеанса на стороне клиента, во время которого пользователь проходит проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="14953-137">&dagger;In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="14953-138">Это не имеющих отношения к сеансов на сервере или [сеанс по промежуточного слоя ASP.NET Core](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="14953-138">It's unrelated to server-side sessions or [ASP.NET Core Session Middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="14953-139">Пользователей можно защититься от уязвимостей CSRF путем принятия мер предосторожности:</span><span class="sxs-lookup"><span data-stu-id="14953-139">Users can guard against CSRF vulnerabilities by taking precautions:</span></span>

* <span data-ttu-id="14953-140">Выйти из веб-приложения после завершения их использования.</span><span class="sxs-lookup"><span data-stu-id="14953-140">Sign off of web apps when finished using them.</span></span>
* <span data-ttu-id="14953-141">Очистить файлы cookie браузера периодически.</span><span class="sxs-lookup"><span data-stu-id="14953-141">Clear browser cookies periodically.</span></span>

<span data-ttu-id="14953-142">Тем не менее CSRF уязвимости, по сути, проблема с веб-приложения, а не конечным пользователем.</span><span class="sxs-lookup"><span data-stu-id="14953-142">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="authentication-fundamentals"></a><span data-ttu-id="14953-143">Основы проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="14953-143">Authentication fundamentals</span></span>

<span data-ttu-id="14953-144">Проверка подлинности на основе файлов cookie — это популярная форма проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="14953-144">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="14953-145">Систем проверки подлинности на основе маркеров растет популярность, особенно для одностраничных приложений (SPA).</span><span class="sxs-lookup"><span data-stu-id="14953-145">Token-based authentication systems are growing in popularity, especially for Single Page Applications (SPAs).</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="14953-146">Проверка подлинности на основе файлов cookie</span><span class="sxs-lookup"><span data-stu-id="14953-146">Cookie-based authentication</span></span>

<span data-ttu-id="14953-147">Когда пользователь проходит проверку подлинности с помощью имени пользователя и пароля, они все выданный маркер, содержащий билет проверки подлинности, который может использоваться для проверки подлинности и авторизации.</span><span class="sxs-lookup"><span data-stu-id="14953-147">When a user authenticates using their username and password, they're issued a token, containing an authentication ticket that can be used for authentication and authorization.</span></span> <span data-ttu-id="14953-148">Он хранится в файле cookie, который сопровождает запрос на каждый клиент делает.</span><span class="sxs-lookup"><span data-stu-id="14953-148">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="14953-149">Проверка подлинности по промежуточного слоя выполняет формирования и проверки этот файл cookie.</span><span class="sxs-lookup"><span data-stu-id="14953-149">Generating and validating this cookie is performed by the Cookie Authentication Middleware.</span></span> <span data-ttu-id="14953-150">[По промежуточного слоя](xref:fundamentals/middleware/index) сериализует участника-пользователя в зашифрованном файле cookie.</span><span class="sxs-lookup"><span data-stu-id="14953-150">The [middleware](xref:fundamentals/middleware/index) serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="14953-151">При последующих запросах по промежуточного слоя проверяет файл cookie, повторно создает основной и назначает участнику [пользователя](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) свойство [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="14953-151">On subsequent requests, the middleware validates the cookie, recreates the principal, and assigns the principal to the [User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) property of [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span>

### <a name="token-based-authentication"></a><span data-ttu-id="14953-152">Проверка подлинности на основе маркеров</span><span class="sxs-lookup"><span data-stu-id="14953-152">Token-based authentication</span></span>

<span data-ttu-id="14953-153">Когда пользователь проходит проверку подлинности, они все выдачи маркера (не токен против подделки).</span><span class="sxs-lookup"><span data-stu-id="14953-153">When a user is authenticated, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="14953-154">Маркер содержит сведения о пользователе в форме [утверждений](/dotnet/framework/security/claims-based-identity-model) или маркера ссылки, указывающий приложение поддерживается в приложении состояние пользователя.</span><span class="sxs-lookup"><span data-stu-id="14953-154">The token contains user information in the form of [claims](/dotnet/framework/security/claims-based-identity-model) or a reference token that points the app to user state maintained in the app.</span></span> <span data-ttu-id="14953-155">Когда пользователь пытается получить доступ к ресурсу, требующей проверки подлинности, маркер отправляется в приложение с заголовком дополнительной авторизации в виде маркера носителя.</span><span class="sxs-lookup"><span data-stu-id="14953-155">When a user attempts to access a resource requiring authentication, the token is sent to the app with an additional authorization header in form of Bearer token.</span></span> <span data-ttu-id="14953-156">В результате приложения без отслеживания состояния.</span><span class="sxs-lookup"><span data-stu-id="14953-156">This makes the app stateless.</span></span> <span data-ttu-id="14953-157">В каждый последующий запрос этот маркер передается в запросе для проверки на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="14953-157">In each subsequent request, the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="14953-158">Этот токен не *зашифрованных*; он имеет *кодировке*.</span><span class="sxs-lookup"><span data-stu-id="14953-158">This token isn't *encrypted*; it's *encoded*.</span></span> <span data-ttu-id="14953-159">На сервере токен декодируется для доступа к его данным.</span><span class="sxs-lookup"><span data-stu-id="14953-159">On the server, the token is decoded to access its information.</span></span> <span data-ttu-id="14953-160">Чтобы отправить маркер при последующих запросах, сохранения токена в локальном хранилище браузера.</span><span class="sxs-lookup"><span data-stu-id="14953-160">To send the token on subsequent requests, store the token in the browser's local storage.</span></span> <span data-ttu-id="14953-161">Не стоит беспокоиться о CSRF уязвимости, если он хранится в локальном хранилище браузера.</span><span class="sxs-lookup"><span data-stu-id="14953-161">Don't be concerned about CSRF vulnerability if the token is stored in the browser's local storage.</span></span> <span data-ttu-id="14953-162">CSRF имеет значения, когда он хранится в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="14953-162">CSRF is a concern when the token is stored in a cookie.</span></span>

### <a name="multiple-apps-hosted-at-one-domain"></a><span data-ttu-id="14953-163">Несколько приложений, размещенных в одном домене</span><span class="sxs-lookup"><span data-stu-id="14953-163">Multiple apps hosted at one domain</span></span>

<span data-ttu-id="14953-164">Общими средами размещения уязвимы для захвата сеанса входа CSRF и других атак.</span><span class="sxs-lookup"><span data-stu-id="14953-164">Shared hosting environments are vulnerable to session hijacking, login CSRF, and other attacks.</span></span>

<span data-ttu-id="14953-165">Несмотря на то что `example1.contoso.net` и `example2.contoso.net` находятся на разных узлах, есть неявные доверительные отношения между узлами в группе `*.contoso.net` домена.</span><span class="sxs-lookup"><span data-stu-id="14953-165">Although `example1.contoso.net` and `example2.contoso.net` are different hosts, there's an implicit trust relationship between hosts under the `*.contoso.net` domain.</span></span> <span data-ttu-id="14953-166">Неявные доверительные отношения с этой позволяет потенциально небезопасных узлам влиять на друг друга файлы cookie (политики одного источника, определяющие запросов AJAX не иметь отношения к файлы cookie HTTP).</span><span class="sxs-lookup"><span data-stu-id="14953-166">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span>

<span data-ttu-id="14953-167">Благодаря использованию доменов можно предотвратить атак, использующих доверенных файлов cookie в приложениях, размещенных на том же домене.</span><span class="sxs-lookup"><span data-stu-id="14953-167">Attacks that exploit trusted cookies between apps hosted on the same domain can be prevented by not sharing domains.</span></span> <span data-ttu-id="14953-168">При каждой приложение размещено на своем собственном домене, отсутствует отношение доверия неявное куки-файл для использования.</span><span class="sxs-lookup"><span data-stu-id="14953-168">When each app is hosted on its own domain, there is no implicit cookie trust relationship to exploit.</span></span>

## <a name="aspnet-core-antiforgery-configuration"></a><span data-ttu-id="14953-169">Сложные конфигурации ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="14953-169">ASP.NET Core antiforgery configuration</span></span>

> [!WARNING]
> <span data-ttu-id="14953-170">ASP.NET Core реализует против подделки с помощью [защиты данных в ASP.NET Core](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="14953-170">ASP.NET Core implements antiforgery using [ASP.NET Core Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="14953-171">В стеке защиты данных должен быть настроен для работы в ферме серверов.</span><span class="sxs-lookup"><span data-stu-id="14953-171">The data protection stack must be configured to work in a server farm.</span></span> <span data-ttu-id="14953-172">См. в разделе [Настройка защиты данных](xref:security/data-protection/configuration/overview) Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="14953-172">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="14953-173">В ASP.NET Core 2.0 или более поздней версии [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) внедряет против подделки токенов в элементы HTML-формы.</span><span class="sxs-lookup"><span data-stu-id="14953-173">In ASP.NET Core 2.0 or later, the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span> <span data-ttu-id="14953-174">Следующая разметка в файле Razor автоматически создает против подделки токены:</span><span class="sxs-lookup"><span data-stu-id="14953-174">The following markup in a Razor file automatically generates antiforgery tokens:</span></span>

```cshtml
<form method="post">
    ...
</form>
```

<span data-ttu-id="14953-175">Аналогично, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) создает против подделки токенов по умолчанию, если метод формы не GET.</span><span class="sxs-lookup"><span data-stu-id="14953-175">Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generates antiforgery tokens by default if the form's method isn't GET.</span></span>

<span data-ttu-id="14953-176">Происходит автоматическое создание против подделки токенов для элементы HTML-формы при `<form>` тег содержит `method="post"` атрибут и одно из следующих условий:</span><span class="sxs-lookup"><span data-stu-id="14953-176">The automatic generation of antiforgery tokens for HTML form elements happens when the `<form>` tag contains the `method="post"` attribute and either of the following are true:</span></span>

  * <span data-ttu-id="14953-177">Атрибут действия пуст (`action=""`).</span><span class="sxs-lookup"><span data-stu-id="14953-177">The action attribute is empty (`action=""`).</span></span>
  * <span data-ttu-id="14953-178">Не указан атрибут действия (`<form method="post">`).</span><span class="sxs-lookup"><span data-stu-id="14953-178">The action attribute isn't supplied (`<form method="post">`).</span></span>

<span data-ttu-id="14953-179">Можно отключить автоматическое создание против подделки маркеры для элементы HTML-формы:</span><span class="sxs-lookup"><span data-stu-id="14953-179">Automatic generation of antiforgery tokens for HTML form elements can be disabled:</span></span>

* <span data-ttu-id="14953-180">Явно отключить против подделки маркеры с `asp-antiforgery` атрибут:</span><span class="sxs-lookup"><span data-stu-id="14953-180">Explicitly disable antiforgery tokens with the `asp-antiforgery` attribute:</span></span>

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* <span data-ttu-id="14953-181">Элемент формы является согласились горизонтального вспомогательных функций тегов с помощью вспомогательной функции тега [! символ отказаться](xref:mvc/views/tag-helpers/intro#opt-out):</span><span class="sxs-lookup"><span data-stu-id="14953-181">The form element is opted-out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out):</span></span>

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* <span data-ttu-id="14953-182">Удалить `FormTagHelper` из представления.</span><span class="sxs-lookup"><span data-stu-id="14953-182">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="14953-183">`FormTagHelper` Можно удалить из представления, добавив следующую директиву в представление Razor:</span><span class="sxs-lookup"><span data-stu-id="14953-183">The `FormTagHelper` can be removed from a view by adding the following directive to the Razor view:</span></span>

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="14953-184">[Razor Pages](xref:razor-pages/index) автоматически защищаются от XSRF/CSRF.</span><span class="sxs-lookup"><span data-stu-id="14953-184">[Razor Pages](xref:razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="14953-185">Дополнительные сведения см. в разделе [XSRF/CSRF и Razor Pages](xref:razor-pages/index#xsrf).</span><span class="sxs-lookup"><span data-stu-id="14953-185">For more information, see [XSRF/CSRF and Razor Pages](xref:razor-pages/index#xsrf).</span></span>

<span data-ttu-id="14953-186">Для защиты от атак CSRF наиболее распространенным подходом является использование *шаблона токена синхронизатор* (STP).</span><span class="sxs-lookup"><span data-stu-id="14953-186">The most common approach to defending against CSRF attacks is to use the *Synchronizer Token Pattern* (STP).</span></span> <span data-ttu-id="14953-187">STP используется в том случае, когда пользователь запрашивает страницу с данными формы:</span><span class="sxs-lookup"><span data-stu-id="14953-187">STP is used when the user requests a page with form data:</span></span>

1. <span data-ttu-id="14953-188">Сервер отправляет токен, связанный с удостоверением текущего пользователя на клиент.</span><span class="sxs-lookup"><span data-stu-id="14953-188">The server sends a token associated with the current user's identity to the client.</span></span>
1. <span data-ttu-id="14953-189">Клиент отправляет обратно токен на сервер для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="14953-189">The client sends back the token to the server for verification.</span></span>
1. <span data-ttu-id="14953-190">Если сервер получает маркер, который не соответствует удостоверение пользователя, прошедшего проверку подлинности, запрос отклоняется.</span><span class="sxs-lookup"><span data-stu-id="14953-190">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span>

<span data-ttu-id="14953-191">Токен должно быть уникальным и непредсказуемым.</span><span class="sxs-lookup"><span data-stu-id="14953-191">The token is unique and unpredictable.</span></span> <span data-ttu-id="14953-192">Маркер можно использовать также для обеспечения правильной последовательности из ряда запросов (например, обеспечьте последовательности запроса: страницы 1 &ndash; странице 2 &ndash; странице 3).</span><span class="sxs-lookup"><span data-stu-id="14953-192">The token can also be used to ensure proper sequencing of a series of requests (for example, ensuring the request sequence of: page 1 &ndash; page 2 &ndash; page 3).</span></span> <span data-ttu-id="14953-193">Все формы в ASP.NET Core MVC и Razor Pages шаблоны создают против подделки маркеры.</span><span class="sxs-lookup"><span data-stu-id="14953-193">All of the forms in ASP.NET Core MVC and Razor Pages templates generate antiforgery tokens.</span></span> <span data-ttu-id="14953-194">В следующей паре Просмотр примеров создания против подделки маркеров:</span><span class="sxs-lookup"><span data-stu-id="14953-194">The following pair of view examples generate antiforgery tokens:</span></span>

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

<span data-ttu-id="14953-195">Явным образом добавить токен против подделки для `<form>` элемент без использования вспомогательных функций тегов с помощью вспомогательного метода HTML [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span><span class="sxs-lookup"><span data-stu-id="14953-195">Explicitly add an antiforgery token to a `<form>` element without using Tag Helpers with the HTML helper [@Html.AntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span></span>

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="14953-196">В каждом из указанных случаев ASP.NET Core добавляет скрытое поле формы следующего вида:</span><span class="sxs-lookup"><span data-stu-id="14953-196">In each of the preceding cases, ASP.NET Core adds a hidden form field similar to the following:</span></span>

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

<span data-ttu-id="14953-197">ASP.NET Core включает в себя три [фильтры](xref:mvc/controllers/filters) для работы с против подделки токенов:</span><span class="sxs-lookup"><span data-stu-id="14953-197">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens:</span></span>

* [<span data-ttu-id="14953-198">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="14953-198">ValidateAntiForgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [<span data-ttu-id="14953-199">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="14953-199">AutoValidateAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [<span data-ttu-id="14953-200">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="14953-200">IgnoreAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a><span data-ttu-id="14953-201">Сложные параметры</span><span class="sxs-lookup"><span data-stu-id="14953-201">Antiforgery options</span></span>

<span data-ttu-id="14953-202">Настройка [против подделки параметры](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="14953-202">Customize [antiforgery options](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="14953-203">&dagger;Задайте защиты от подделки `Cookie` свойства с помощью свойства [CookieBuilder](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder) класса.</span><span class="sxs-lookup"><span data-stu-id="14953-203">&dagger;Set the antiforgery `Cookie` properties using the properties of the [CookieBuilder](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder) class.</span></span>

| <span data-ttu-id="14953-204">Параметр</span><span class="sxs-lookup"><span data-stu-id="14953-204">Option</span></span> | <span data-ttu-id="14953-205">Описание</span><span class="sxs-lookup"><span data-stu-id="14953-205">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="14953-206">Файл cookie</span><span class="sxs-lookup"><span data-stu-id="14953-206">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="14953-207">Определяет параметры, используемые для создания против подделки файлы cookie.</span><span class="sxs-lookup"><span data-stu-id="14953-207">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="14953-208">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="14953-208">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="14953-209">Имя скрытого поля формы используемые против подделки системой для подготовки к просмотру против подделки токенов в представлениях.</span><span class="sxs-lookup"><span data-stu-id="14953-209">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="14953-210">HeaderName</span><span class="sxs-lookup"><span data-stu-id="14953-210">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="14953-211">Имя заголовка, используемый системой против подделки.</span><span class="sxs-lookup"><span data-stu-id="14953-211">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="14953-212">Если `null`, то система рассматривает только данные формы.</span><span class="sxs-lookup"><span data-stu-id="14953-212">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="14953-213">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="14953-213">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="14953-214">Указывает, следует ли подавлять поколение `X-Frame-Options` заголовка.</span><span class="sxs-lookup"><span data-stu-id="14953-214">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="14953-215">По умолчанию заголовок создается со значением «SAMEORIGIN».</span><span class="sxs-lookup"><span data-stu-id="14953-215">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="14953-216">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="14953-216">Defaults to `false`.</span></span> |

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

| <span data-ttu-id="14953-217">Параметр</span><span class="sxs-lookup"><span data-stu-id="14953-217">Option</span></span> | <span data-ttu-id="14953-218">Описание</span><span class="sxs-lookup"><span data-stu-id="14953-218">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="14953-219">Файл cookie</span><span class="sxs-lookup"><span data-stu-id="14953-219">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="14953-220">Определяет параметры, используемые для создания против подделки файлы cookie.</span><span class="sxs-lookup"><span data-stu-id="14953-220">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="14953-221">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="14953-221">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | <span data-ttu-id="14953-222">Домен cookie.</span><span class="sxs-lookup"><span data-stu-id="14953-222">The domain of the cookie.</span></span> <span data-ttu-id="14953-223">По умолчанию — `null`.</span><span class="sxs-lookup"><span data-stu-id="14953-223">Defaults to `null`.</span></span> <span data-ttu-id="14953-224">Это свойство является устаревшим и будет удален в будущей версии.</span><span class="sxs-lookup"><span data-stu-id="14953-224">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="14953-225">Рекомендуемой альтернативой является Cookie.Domain.</span><span class="sxs-lookup"><span data-stu-id="14953-225">The recommended alternative is Cookie.Domain.</span></span> |
| [<span data-ttu-id="14953-226">CookieName</span><span class="sxs-lookup"><span data-stu-id="14953-226">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | <span data-ttu-id="14953-227">Имя файла cookie.</span><span class="sxs-lookup"><span data-stu-id="14953-227">The name of the cookie.</span></span> <span data-ttu-id="14953-228">Если не задано, система создает уникальное имя которого начинается с [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (». AspNetCore.Antiforgery.»).</span><span class="sxs-lookup"><span data-stu-id="14953-228">If not set, the system generates a unique name beginning with the [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (".AspNetCore.Antiforgery.").</span></span> <span data-ttu-id="14953-229">Это свойство является устаревшим и будет удален в будущей версии.</span><span class="sxs-lookup"><span data-stu-id="14953-229">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="14953-230">Рекомендуемой альтернативой является Cookie.Name.</span><span class="sxs-lookup"><span data-stu-id="14953-230">The recommended alternative is Cookie.Name.</span></span> |
| [<span data-ttu-id="14953-231">CookiePath</span><span class="sxs-lookup"><span data-stu-id="14953-231">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | <span data-ttu-id="14953-232">Путь в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="14953-232">The path set on the cookie.</span></span> <span data-ttu-id="14953-233">Это свойство является устаревшим и будет удален в будущей версии.</span><span class="sxs-lookup"><span data-stu-id="14953-233">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="14953-234">Рекомендуемой альтернативой является Cookie.Path.</span><span class="sxs-lookup"><span data-stu-id="14953-234">The recommended alternative is Cookie.Path.</span></span> |
| [<span data-ttu-id="14953-235">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="14953-235">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="14953-236">Имя скрытого поля формы используемые против подделки системой для подготовки к просмотру против подделки токенов в представлениях.</span><span class="sxs-lookup"><span data-stu-id="14953-236">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="14953-237">HeaderName</span><span class="sxs-lookup"><span data-stu-id="14953-237">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="14953-238">Имя заголовка, используемый системой против подделки.</span><span class="sxs-lookup"><span data-stu-id="14953-238">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="14953-239">Если `null`, то система рассматривает только данные формы.</span><span class="sxs-lookup"><span data-stu-id="14953-239">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="14953-240">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="14953-240">RequireSsl</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | <span data-ttu-id="14953-241">Указывает, требуется ли SSL против подделки системой.</span><span class="sxs-lookup"><span data-stu-id="14953-241">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="14953-242">Если `true`, происходит сбой запросов без SSL.</span><span class="sxs-lookup"><span data-stu-id="14953-242">If `true`, non-SSL requests fail.</span></span> <span data-ttu-id="14953-243">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="14953-243">Defaults to `false`.</span></span> <span data-ttu-id="14953-244">Это свойство является устаревшим и будет удален в будущей версии.</span><span class="sxs-lookup"><span data-stu-id="14953-244">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="14953-245">Рекомендуемой альтернативой является установка Cookie.SecurePolicy.</span><span class="sxs-lookup"><span data-stu-id="14953-245">The recommended alternative is to set Cookie.SecurePolicy.</span></span> |
| [<span data-ttu-id="14953-246">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="14953-246">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="14953-247">Указывает, следует ли подавлять поколение `X-Frame-Options` заголовка.</span><span class="sxs-lookup"><span data-stu-id="14953-247">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="14953-248">По умолчанию заголовок создается со значением «SAMEORIGIN».</span><span class="sxs-lookup"><span data-stu-id="14953-248">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="14953-249">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="14953-249">Defaults to `false`.</span></span> |

::: moniker-end

<span data-ttu-id="14953-250">Дополнительные сведения см. в разделе [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span><span class="sxs-lookup"><span data-stu-id="14953-250">For more information, see [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span></span>

## <a name="configure-antiforgery-features-with-iantiforgery"></a><span data-ttu-id="14953-251">Настройка против подделки функций работы с IAntiforgery</span><span class="sxs-lookup"><span data-stu-id="14953-251">Configure antiforgery features with IAntiforgery</span></span>

<span data-ttu-id="14953-252">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) предоставляет API для настройки функций против подделки.</span><span class="sxs-lookup"><span data-stu-id="14953-252">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) provides the API to configure antiforgery features.</span></span> <span data-ttu-id="14953-253">`IAntiforgery` можно запросить в `Configure` метод `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="14953-253">`IAntiforgery` can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="14953-254">В следующем примере используется по промежуточного слоя с домашней страницы приложения, чтобы создать токен против подделки и отправить его в ответ как файл cookie (с помощью именования по умолчанию Angular описано далее в этом разделе):</span><span class="sxs-lookup"><span data-stu-id="14953-254">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described later in this topic):</span></span>

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

### <a name="require-antiforgery-validation"></a><span data-ttu-id="14953-255">Требовать проверку против подделки</span><span class="sxs-lookup"><span data-stu-id="14953-255">Require antiforgery validation</span></span>

<span data-ttu-id="14953-256">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) является фильтром операции, которые могут применяться для отдельного действия, контроллера или глобально.</span><span class="sxs-lookup"><span data-stu-id="14953-256">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="14953-257">Запросы к действиям, которые имеют этот фильтр, применяемый будут заблокированы, если запрос содержит допустимый токен против подделки.</span><span class="sxs-lookup"><span data-stu-id="14953-257">Requests made to actions that have this filter applied are blocked unless the request includes a valid antiforgery token.</span></span>

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

<span data-ttu-id="14953-258">`ValidateAntiForgeryToken` Атрибут требует маркер для запросов к методам действий, он сопровождает, включая HTTP-запросы GET.</span><span class="sxs-lookup"><span data-stu-id="14953-258">The `ValidateAntiForgeryToken` attribute requires a token for requests to the action methods it decorates, including HTTP GET requests.</span></span> <span data-ttu-id="14953-259">Если `ValidateAntiForgeryToken` атрибут применяется на контроллерах приложения, его можно переопределить с помощью `IgnoreAntiforgeryToken` атрибута.</span><span class="sxs-lookup"><span data-stu-id="14953-259">If the `ValidateAntiForgeryToken` attribute is applied across the app's controllers, it can be overridden with the `IgnoreAntiforgeryToken` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="14953-260">ASP.NET Core не поддерживает добавление маркеры против подделки запросов GET автоматически.</span><span class="sxs-lookup"><span data-stu-id="14953-260">ASP.NET Core doesn't support adding antiforgery tokens to GET requests automatically.</span></span>

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a><span data-ttu-id="14953-261">Автоматически проверьте против подделки маркеры на предмет только небезопасных методов HTTP</span><span class="sxs-lookup"><span data-stu-id="14953-261">Automatically validate antiforgery tokens for unsafe HTTP methods only</span></span>

<span data-ttu-id="14953-262">Приложения ASP.NET Core не создают против подделки маркеры для безопасные методы HTTP (GET, HEAD, параметры и ТРАССИРОВКИ).</span><span class="sxs-lookup"><span data-stu-id="14953-262">ASP.NET Core apps don't generate antiforgery tokens for safe HTTP methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="14953-263">Вместо применения широко `ValidateAntiForgeryToken` атрибут и затем переопределение с помощью `IgnoreAntiforgeryToken` атрибуты, [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) атрибут может использоваться.</span><span class="sxs-lookup"><span data-stu-id="14953-263">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, the [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribute can be used.</span></span> <span data-ttu-id="14953-264">Этот атрибут работает идентично `ValidateAntiForgeryToken` атрибут, за исключением того, что он не требует токены для запросов, выполненных с помощью следующих методов HTTP:</span><span class="sxs-lookup"><span data-stu-id="14953-264">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="14953-265">GET</span><span class="sxs-lookup"><span data-stu-id="14953-265">GET</span></span>
* <span data-ttu-id="14953-266">HEAD,</span><span class="sxs-lookup"><span data-stu-id="14953-266">HEAD</span></span>
* <span data-ttu-id="14953-267">OPTIONS</span><span class="sxs-lookup"><span data-stu-id="14953-267">OPTIONS</span></span>
* <span data-ttu-id="14953-268">TRACE</span><span class="sxs-lookup"><span data-stu-id="14953-268">TRACE</span></span>

<span data-ttu-id="14953-269">Рекомендуется использовать `AutoValidateAntiforgeryToken` широко для сценариев, отличных от API.</span><span class="sxs-lookup"><span data-stu-id="14953-269">We recommend use of `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="14953-270">Это гарантирует, что действия после защищены по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="14953-270">This ensures POST actions are protected by default.</span></span> <span data-ttu-id="14953-271">Вместо этого должен игнорировать против подделки токенов по умолчанию, если не `ValidateAntiForgeryToken` применяется для отдельных методов действий.</span><span class="sxs-lookup"><span data-stu-id="14953-271">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to individual action methods.</span></span> <span data-ttu-id="14953-272">Он более вероятно в этом сценарии для метода POST действие должно быть оставлено без защиты по ошибке, оставляя приложение уязвимым к CSRF-атакам.</span><span class="sxs-lookup"><span data-stu-id="14953-272">It's more likely in this scenario for a POST action method to be left unprotected by mistake, leaving the app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="14953-273">Все сообщения, необходимо отправить маркер защиты от подделки.</span><span class="sxs-lookup"><span data-stu-id="14953-273">All POSTs should send the antiforgery token.</span></span>

<span data-ttu-id="14953-274">API-интерфейсы не имеют автоматический механизм для отправки не файл cookie частью маркера.</span><span class="sxs-lookup"><span data-stu-id="14953-274">APIs don't have an automatic mechanism for sending the non-cookie part of the token.</span></span> <span data-ttu-id="14953-275">Реализация, вероятно, зависит от реализации кода клиента.</span><span class="sxs-lookup"><span data-stu-id="14953-275">The implementation probably depends on the client code implementation.</span></span> <span data-ttu-id="14953-276">Ниже приведены некоторые примеры:</span><span class="sxs-lookup"><span data-stu-id="14953-276">Some examples are shown below:</span></span>

<span data-ttu-id="14953-277">Пример уровня класса.</span><span class="sxs-lookup"><span data-stu-id="14953-277">Class-level example:</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="14953-278">Пример глобального.</span><span class="sxs-lookup"><span data-stu-id="14953-278">Global example:</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a><span data-ttu-id="14953-279">Переопределение глобальных или против подделки атрибутов контроллера</span><span class="sxs-lookup"><span data-stu-id="14953-279">Override global or controller antiforgery attributes</span></span>

<span data-ttu-id="14953-280">[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) фильтр позволяет избежать необходимости токен от подделки, для заданного действия (или контроллера).</span><span class="sxs-lookup"><span data-stu-id="14953-280">The [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filter is used to eliminate the need for an antiforgery token for a given action (or controller).</span></span> <span data-ttu-id="14953-281">При применении этого фильтра переопределяет `ValidateAntiForgeryToken` и `AutoValidateAntiforgeryToken` фильтрам, указанным на более высоком уровне (глобально или на контроллере).</span><span class="sxs-lookup"><span data-stu-id="14953-281">When applied, this filter overrides `ValidateAntiForgeryToken` and `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

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

## <a name="refresh-tokens-after-authentication"></a><span data-ttu-id="14953-282">Маркеры обновления после проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="14953-282">Refresh tokens after authentication</span></span>

<span data-ttu-id="14953-283">Маркеры должны обновляться после проверки подлинности пользователя, перенаправляя пользователя к представлению или страницы Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="14953-283">Tokens should be refreshed after the user is authenticated by redirecting the user to a view or Razor Pages page.</span></span>

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="14953-284">JavaScript, AJAX и одностраничные приложения</span><span class="sxs-lookup"><span data-stu-id="14953-284">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="14953-285">В традиционных приложениях на базе HTML против подделки маркеры передаются на сервер с помощью скрытых полях формы.</span><span class="sxs-lookup"><span data-stu-id="14953-285">In traditional HTML-based apps, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="14953-286">В современных приложений на базе JavaScript и одностраничные приложения много запросов выполняются программно.</span><span class="sxs-lookup"><span data-stu-id="14953-286">In modern JavaScript-based apps and SPAs, many requests are made programmatically.</span></span> <span data-ttu-id="14953-287">Эти запросы AJAX могут использовать другие технологии (например, заголовки запросов или файлы cookie) для отправки маркера.</span><span class="sxs-lookup"><span data-stu-id="14953-287">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span>

<span data-ttu-id="14953-288">Если файлы cookie используются для хранения маркеров проверки подлинности и проверки подлинности запросов API, на сервере, CSRF может стать проблемой.</span><span class="sxs-lookup"><span data-stu-id="14953-288">If cookies are used to store authentication tokens and to authenticate API requests on the server, CSRF is a potential problem.</span></span> <span data-ttu-id="14953-289">Если локальное хранилище используется для сохранения токена, CSRF уязвимость может устранен, так как значения из локального хранилища не автоматически отправляются на сервер с каждым запросом.</span><span class="sxs-lookup"><span data-stu-id="14953-289">If local storage is used to store the token, CSRF vulnerability might be mitigated because values from local storage aren't sent automatically to the server with every request.</span></span> <span data-ttu-id="14953-290">Таким образом с помощью локального хранилища для хранения маркер защиты от подделки на клиенте, а также отправки маркера, как в заголовке запроса — это рекомендуемый подход.</span><span class="sxs-lookup"><span data-stu-id="14953-290">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="javascript"></a><span data-ttu-id="14953-291">JavaScript</span><span class="sxs-lookup"><span data-stu-id="14953-291">JavaScript</span></span>

<span data-ttu-id="14953-292">Использование JavaScript с представлениями, маркер могут создаваться с использованием службу в представлении.</span><span class="sxs-lookup"><span data-stu-id="14953-292">Using JavaScript with views, the token can be created using a service from within the view.</span></span> <span data-ttu-id="14953-293">Внедрить [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) службы в представления и вызовите [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span><span class="sxs-lookup"><span data-stu-id="14953-293">Inject the [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service into the view and call [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

<span data-ttu-id="14953-294">Такой подход избавляет от необходимости работать непосредственно с помощью параметра с сервера файлы cookie или их считывания из клиента.</span><span class="sxs-lookup"><span data-stu-id="14953-294">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="14953-295">В предыдущем примере JavaScript считывается значение скрытого поля для заголовка AJAX POST.</span><span class="sxs-lookup"><span data-stu-id="14953-295">The preceding example uses JavaScript to read the hidden field value for the AJAX POST header.</span></span>

<span data-ttu-id="14953-296">JavaScript можно также маркеров доступа в файлы cookie и использовать содержимое этого файла cookie для создания заголовка со значением маркера.</span><span class="sxs-lookup"><span data-stu-id="14953-296">JavaScript can also access tokens in cookies and use the cookie's contents to create a header with the token's value.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="14953-297">При условии, что скрипт запрашивает отправку маркера в заголовок, называемый `X-CSRF-TOKEN`, настроить службу против подделки искать `X-CSRF-TOKEN` заголовка:</span><span class="sxs-lookup"><span data-stu-id="14953-297">Assuming the script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="14953-298">В следующем примере используется JavaScript для выполнения запроса AJAX с соответствующий заголовок:</span><span class="sxs-lookup"><span data-stu-id="14953-298">The following example uses JavaScript to make an AJAX request with the appropriate header:</span></span>

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

### <a name="angularjs"></a><span data-ttu-id="14953-299">AngularJS</span><span class="sxs-lookup"><span data-stu-id="14953-299">AngularJS</span></span>

<span data-ttu-id="14953-300">AngularJS используется соглашение по адресу CSRF.</span><span class="sxs-lookup"><span data-stu-id="14953-300">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="14953-301">Если сервер отправляет файл cookie с именем `XSRF-TOKEN`, AngularJS `$http` служба добавляет значение файла cookie в заголовке, когда он отправляет запрос к серверу.</span><span class="sxs-lookup"><span data-stu-id="14953-301">If the server sends a cookie with the name `XSRF-TOKEN`, the AngularJS `$http` service adds the cookie value to a header when it sends a request to the server.</span></span> <span data-ttu-id="14953-302">Этот процесс выполняется автоматически.</span><span class="sxs-lookup"><span data-stu-id="14953-302">This process is automatic.</span></span> <span data-ttu-id="14953-303">Заголовок не требуется явным образом задать.</span><span class="sxs-lookup"><span data-stu-id="14953-303">The header doesn't need to be set explicitly.</span></span> <span data-ttu-id="14953-304">Имя заголовка — `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="14953-304">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="14953-305">Сервер должен обнаружить этот заголовок и проверить его содержимое.</span><span class="sxs-lookup"><span data-stu-id="14953-305">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="14953-306">Для ASP.NET Core API работы с этим соглашением:</span><span class="sxs-lookup"><span data-stu-id="14953-306">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="14953-307">Настройте приложение, чтобы предоставить маркер в файл cookie с именем `XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="14953-307">Configure your app to provide a token in a cookie called `XSRF-TOKEN`.</span></span>
* <span data-ttu-id="14953-308">Настроить службу против подделки искать заголовок с именем `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="14953-308">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`.</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="14953-309">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="14953-309">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="extend-antiforgery"></a><span data-ttu-id="14953-310">Расширение защиты от подделки</span><span class="sxs-lookup"><span data-stu-id="14953-310">Extend antiforgery</span></span>

<span data-ttu-id="14953-311">[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) типа позволяет разработчикам расширять поведение системы anti-CSRF, циклической обработки дополнительных данных в каждом маркере.</span><span class="sxs-lookup"><span data-stu-id="14953-311">The [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-CSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="14953-312">[GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) метод вызывается каждый раз создается маркер поля, и возвращаемое значение внедрены в созданный маркер.</span><span class="sxs-lookup"><span data-stu-id="14953-312">The [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="14953-313">Разработчик может возвращать метку времени, nonce или любое другое значение, а затем вызвать [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) проверить эти данные, когда маркер проверяется.</span><span class="sxs-lookup"><span data-stu-id="14953-313">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) to validate this data when the token is validated.</span></span> <span data-ttu-id="14953-314">Имя пользователя клиента внедренных в создаваемые маркеры, поэтому нет необходимости, чтобы включить эту информацию.</span><span class="sxs-lookup"><span data-stu-id="14953-314">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="14953-315">Если маркер содержит дополнительные данные, но не `IAntiForgeryAdditionalDataProvider` будет настроен, дополнительные данные не проверяются.</span><span class="sxs-lookup"><span data-stu-id="14953-315">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` is configured, the supplemental data isn't validated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="14953-316">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="14953-316">Additional resources</span></span>

* <span data-ttu-id="14953-317">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) на [откройте Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="14953-317">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
* <xref:host-and-deploy/web-farm>
