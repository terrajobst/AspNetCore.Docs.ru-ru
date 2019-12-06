---
title: Предотвращение атак с подделкой межсайтовых запросов (XSRF/CSRF) в ASP.NET Core
author: steve-smith
description: Узнайте, как предотвратить атаки на веб-приложения, в которых вредоносный веб-сайт может повлиять на взаимодействие между браузером клиента и приложением.
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: security/anti-request-forgery
ms.openlocfilehash: 54e153af55f28d9a89bbf16bce1c17f876567b59
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880809"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="a9bdf-103">Предотвращение атак с подделкой межсайтовых запросов (XSRF/CSRF) в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a9bdf-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="a9bdf-104">[Рик Андерсон (](https://twitter.com/RickAndMSFT), [Фийаз Хасан](https://twitter.com/FiyazBinHasan)и [Виктор Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="a9bdf-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a9bdf-105">Подделка межсайтовых запросов (также известная как XSRF или CSRF) — это атака на веб-приложения, с помощью которой вредоносное веб-приложение может повлиять на взаимодействие между браузером клиента и веб-приложением, которое доверяет этому браузеру.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-105">Cross-site request forgery (also known as XSRF or CSRF) is an attack against web-hosted apps whereby a malicious web app can influence the interaction between a client browser and a web app that trusts that browser.</span></span> <span data-ttu-id="a9bdf-106">Эти атаки возможны, поскольку веб-браузеры автоматически отправляют некоторые типы токенов проверки подлинности при каждом запросе на веб-сайт.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-106">These attacks are possible because web browsers send some types of authentication tokens automatically with every request to a website.</span></span> <span data-ttu-id="a9bdf-107">Такая форма атаки также называется *атакой одним щелчком* или *обкрытием сеанса* , поскольку атака использует преимущества ранее проверенного сеанса пользователя.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-107">This form of exploit is also known as a *one-click attack* or *session riding* because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="a9bdf-108">Пример атаки CSRF:</span><span class="sxs-lookup"><span data-stu-id="a9bdf-108">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="a9bdf-109">Пользователь входит в `www.good-banking-site.com` с использованием проверки подлинности с помощью форм.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-109">A user signs into `www.good-banking-site.com` using forms authentication.</span></span> <span data-ttu-id="a9bdf-110">Сервер проверяет подлинность пользователя и выдает ответ, включающий файл cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-110">The server authenticates the user and issues a response that includes an authentication cookie.</span></span> <span data-ttu-id="a9bdf-111">Сайт уязвим к атакам, так как он доверяет любому полученному в результате запроса с действительным файлом cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-111">The site is vulnerable to attack because it trusts any request that it receives with a valid authentication cookie.</span></span>
1. <span data-ttu-id="a9bdf-112">Пользователь посещает вредоносный сайт `www.bad-crook-site.com`.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-112">The user visits a malicious site, `www.bad-crook-site.com`.</span></span>

   <span data-ttu-id="a9bdf-113">Вредоносный сайт, `www.bad-crook-site.com`, содержит HTML-форму, аналогичную следующей:</span><span class="sxs-lookup"><span data-stu-id="a9bdf-113">The malicious site, `www.bad-crook-site.com`, contains an HTML form similar to the following:</span></span>

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   <span data-ttu-id="a9bdf-114">Обратите внимание, что форма `action` отправляется на уязвимый сайт, а не на вредоносный сайт.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-114">Notice that the form's `action` posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="a9bdf-115">Это часть CSRF, связанная с "межсайт".</span><span class="sxs-lookup"><span data-stu-id="a9bdf-115">This is the "cross-site" part of CSRF.</span></span>

1. <span data-ttu-id="a9bdf-116">Пользователь нажимает кнопку Submit (отправить).</span><span class="sxs-lookup"><span data-stu-id="a9bdf-116">The user selects the submit button.</span></span> <span data-ttu-id="a9bdf-117">Браузер выполняет запрос и автоматически включает файл cookie проверки подлинности для запрошенного домена `www.good-banking-site.com`.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-117">The browser makes the request and automatically includes the authentication cookie for the requested domain, `www.good-banking-site.com`.</span></span>
1. <span data-ttu-id="a9bdf-118">Запрос выполняется на `www.good-banking-site.com` сервере с контекстом проверки подлинности пользователя и может выполнять любое действие, которое прошедший проверку подлинности пользователь разрешает.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-118">The request runs on the `www.good-banking-site.com` server with the user's authentication context and can perform any action that an authenticated user is allowed to perform.</span></span>

<span data-ttu-id="a9bdf-119">В дополнение к сценарию, в котором пользователь нажимает кнопку для отправки формы, вредоносный сайт может:</span><span class="sxs-lookup"><span data-stu-id="a9bdf-119">In addition to the scenario where the user selects the button to submit the form, the malicious site could:</span></span>

* <span data-ttu-id="a9bdf-120">Запуск скрипта, который автоматически отправляет форму.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-120">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="a9bdf-121">Отправка отправки формы в виде запроса AJAX.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-121">Send the form submission as an AJAX request.</span></span>
* <span data-ttu-id="a9bdf-122">Скрытие формы с помощью CSS.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-122">Hide the form using CSS.</span></span>

<span data-ttu-id="a9bdf-123">В этих альтернативных сценариях не требуется никаких действий или входных данных пользователя, кроме первоначального посещения вредоносного сайта.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-123">These alternative scenarios don't require any action or input from the user other than initially visiting the malicious site.</span></span>

<span data-ttu-id="a9bdf-124">Использование протокола HTTPS не мешает CSRF атаке.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-124">Using HTTPS doesn't prevent a CSRF attack.</span></span> <span data-ttu-id="a9bdf-125">Вредоносный сайт может отправить запрос `https://www.good-banking-site.com/` так же просто, как он может отправить небезопасный запрос.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-125">The malicious site can send an `https://www.good-banking-site.com/` request just as easily as it can send an insecure request.</span></span>

<span data-ttu-id="a9bdf-126">Некоторые атаки направлены на конечные точки, отвечающие на запросы GET. в этом случае для выполнения действия можно использовать тег Image.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-126">Some attacks target endpoints that respond to GET requests, in which case an image tag can be used to perform the action.</span></span> <span data-ttu-id="a9bdf-127">Такая форма атаки является распространенной на сайтах форумов, которые допускают образы, но блокируют JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-127">This form of attack is common on forum sites that permit images but block JavaScript.</span></span> <span data-ttu-id="a9bdf-128">Приложения, изменяющие состояние запросов GET, которые изменяются при изменении переменных или ресурсов, уязвимы для атак злоумышленников.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-128">Apps that change state on GET requests, where variables or resources are altered, are vulnerable to malicious attacks.</span></span> <span data-ttu-id="a9bdf-129">**Запросы на получение, которые изменили состояние, являются небезопасными. Рекомендуется никогда не изменять состояние запроса GET.**</span><span class="sxs-lookup"><span data-stu-id="a9bdf-129">**GET requests that change state are insecure. A best practice is to never change state on a GET request.**</span></span>

<span data-ttu-id="a9bdf-130">CSRF атаки могут использоваться для веб-приложений, использующих файлы cookie для проверки подлинности по следующим причинам.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-130">CSRF attacks are possible against web apps that use cookies for authentication because:</span></span>

* <span data-ttu-id="a9bdf-131">Браузеры хранят файлы cookie, выданные веб-приложением.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-131">Browsers store cookies issued by a web app.</span></span>
* <span data-ttu-id="a9bdf-132">Сохраненные файлы cookie содержат файлы cookie сеанса для пользователей, прошедших проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-132">Stored cookies include session cookies for authenticated users.</span></span>
* <span data-ttu-id="a9bdf-133">Браузеры отправляют все файлы cookie, связанные с доменом, в веб-приложение каждый запрос, независимо от того, как запрос к приложению был создан в браузере.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-133">Browsers send all of the cookies associated with a domain to the web app every request regardless of how the request to app was generated within the browser.</span></span>

<span data-ttu-id="a9bdf-134">Однако атаки CSRF не ограничиваются использованием файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-134">However, CSRF attacks aren't limited to exploiting cookies.</span></span> <span data-ttu-id="a9bdf-135">Например, также уязвимы обычная и краткая проверка подлинности.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-135">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="a9bdf-136">После того как пользователь войдет в систему с базовой или дайджест-проверкой подлинности, браузер автоматически отправит учетные данные, пока сеанс&dagger; не завершится.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-136">After a user signs in with Basic or Digest authentication, the browser automatically sends the credentials until the session&dagger; ends.</span></span>

<span data-ttu-id="a9bdf-137">&dagger;в этом контексте *сеанс* относится к сеансу на стороне клиента, в течение которого пользователь прошел проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-137">&dagger;In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="a9bdf-138">Он не связан с сеансами на стороне сервера или по [промежуточного слоя сеанса ASP.NET Core](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="a9bdf-138">It's unrelated to server-side sessions or [ASP.NET Core Session Middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="a9bdf-139">Пользователи могут защищаться от CSRF уязвимостей, предпринимая меры предосторожности.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-139">Users can guard against CSRF vulnerabilities by taking precautions:</span></span>

* <span data-ttu-id="a9bdf-140">Выйдите из веб-приложений по завершении их использования.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-140">Sign off of web apps when finished using them.</span></span>
* <span data-ttu-id="a9bdf-141">Периодически очищать файлы cookie браузера.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-141">Clear browser cookies periodically.</span></span>

<span data-ttu-id="a9bdf-142">Однако уязвимости CSRF являются фундаментальной проблемой с веб-приложением, а не конечным пользователем.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-142">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="authentication-fundamentals"></a><span data-ttu-id="a9bdf-143">Основы проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="a9bdf-143">Authentication fundamentals</span></span>

<span data-ttu-id="a9bdf-144">Аутентификация на основе файлов cookie — это популярная форма проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-144">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="a9bdf-145">Системы проверки подлинности на основе маркеров растут по популярности, особенно для одностраничных приложений (одностраничные приложения).</span><span class="sxs-lookup"><span data-stu-id="a9bdf-145">Token-based authentication systems are growing in popularity, especially for Single Page Applications (SPAs).</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="a9bdf-146">Проверка подлинности на основе файлов cookie</span><span class="sxs-lookup"><span data-stu-id="a9bdf-146">Cookie-based authentication</span></span>

<span data-ttu-id="a9bdf-147">Когда пользователь проходит проверку подлинности с использованием имени пользователя и пароля, он выдает маркер, содержащий билет проверки подлинности, который можно использовать для проверки подлинности и авторизации.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-147">When a user authenticates using their username and password, they're issued a token, containing an authentication ticket that can be used for authentication and authorization.</span></span> <span data-ttu-id="a9bdf-148">Маркер хранится в виде файла cookie, сопровождающего каждый запрос, создаваемый клиентом.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-148">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="a9bdf-149">Создание и проверка этого файла cookie выполняется по промежуточного слоя проверки подлинности cookie.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-149">Generating and validating this cookie is performed by the Cookie Authentication Middleware.</span></span> <span data-ttu-id="a9bdf-150">По [промежуточного слоя](xref:fundamentals/middleware/index) пользователь сериализует субъект-пользователя в зашифрованный файл cookie.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-150">The [middleware](xref:fundamentals/middleware/index) serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="a9bdf-151">При последующих запросах по промежуточного слоя проверяет файл cookie, повторно создает участника и назначает участника свойству [пользователя](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="a9bdf-151">On subsequent requests, the middleware validates the cookie, recreates the principal, and assigns the principal to the [User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) property of [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span>

### <a name="token-based-authentication"></a><span data-ttu-id="a9bdf-152">Проверка подлинности на основе токенов</span><span class="sxs-lookup"><span data-stu-id="a9bdf-152">Token-based authentication</span></span>

<span data-ttu-id="a9bdf-153">Когда пользователь проходит проверку подлинности, ему выдается маркер (а не маркер подделки).</span><span class="sxs-lookup"><span data-stu-id="a9bdf-153">When a user is authenticated, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="a9bdf-154">Маркер содержит сведения о пользователе в виде [утверждений](/dotnet/framework/security/claims-based-identity-model) или маркер ссылки, который указывает приложению на обслуживание пользовательского состояния в приложении.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-154">The token contains user information in the form of [claims](/dotnet/framework/security/claims-based-identity-model) or a reference token that points the app to user state maintained in the app.</span></span> <span data-ttu-id="a9bdf-155">Когда пользователь пытается получить доступ к ресурсу, который требует проверки подлинности, маркер отправляется в приложение с дополнительным заголовком авторизации в виде токена носителя.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-155">When a user attempts to access a resource requiring authentication, the token is sent to the app with an additional authorization header in form of Bearer token.</span></span> <span data-ttu-id="a9bdf-156">Это делает приложение без отслеживания состояния.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-156">This makes the app stateless.</span></span> <span data-ttu-id="a9bdf-157">В каждом последующем запросе маркер передается в запросе на проверку на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-157">In each subsequent request, the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="a9bdf-158">Этот токен не *зашифрован*; Он *кодируется*.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-158">This token isn't *encrypted*; it's *encoded*.</span></span> <span data-ttu-id="a9bdf-159">На сервере маркер декодирован для доступа к его данным.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-159">On the server, the token is decoded to access its information.</span></span> <span data-ttu-id="a9bdf-160">Чтобы отправить маркер при последующих запросах, сохраните маркер в локальном хранилище браузера.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-160">To send the token on subsequent requests, store the token in the browser's local storage.</span></span> <span data-ttu-id="a9bdf-161">Не стоит беспокоиться об уязвимости CSRF, если маркер хранится в локальном хранилище браузера.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-161">Don't be concerned about CSRF vulnerability if the token is stored in the browser's local storage.</span></span> <span data-ttu-id="a9bdf-162">CSRF является проблемой, когда маркер хранится в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-162">CSRF is a concern when the token is stored in a cookie.</span></span> <span data-ttu-id="a9bdf-163">Дополнительные сведения см. в [примере кода "проблемное соглашение GitHub" добавляет два cookie](https://github.com/aspnet/AspNetCore.Docs/issues/13369).</span><span class="sxs-lookup"><span data-stu-id="a9bdf-163">For more information, see the GitHub issue [SPA code sample adds two cookies](https://github.com/aspnet/AspNetCore.Docs/issues/13369).</span></span>

### <a name="multiple-apps-hosted-at-one-domain"></a><span data-ttu-id="a9bdf-164">Несколько приложений, размещенных в одном домене</span><span class="sxs-lookup"><span data-stu-id="a9bdf-164">Multiple apps hosted at one domain</span></span>

<span data-ttu-id="a9bdf-165">Общие среды размещения уязвимы для перехвата сеансов, входа CSRF и других атак.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-165">Shared hosting environments are vulnerable to session hijacking, login CSRF, and other attacks.</span></span>

<span data-ttu-id="a9bdf-166">Хотя `example1.contoso.net` и `example2.contoso.net` являются разными узлами, между узлами в домене `*.contoso.net` существует неявное отношение доверия.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-166">Although `example1.contoso.net` and `example2.contoso.net` are different hosts, there's an implicit trust relationship between hosts under the `*.contoso.net` domain.</span></span> <span data-ttu-id="a9bdf-167">Это неявное отношение доверия позволяет потенциально недоверенным узлам повлиять на файлы cookie друг друга (те же политики, которые управляют запросами AJAX, не обязательно применяются к файлам cookie HTTP).</span><span class="sxs-lookup"><span data-stu-id="a9bdf-167">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span>

<span data-ttu-id="a9bdf-168">Атаки, которые используют доверенные файлы cookie между приложениями, размещенными в одном домене, могут быть предотвращены за счет отсутствия общего доступа к доменам.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-168">Attacks that exploit trusted cookies between apps hosted on the same domain can be prevented by not sharing domains.</span></span> <span data-ttu-id="a9bdf-169">Если каждое приложение размещается в собственном домене, для эксплойта не существует неявного отношения доверия с файлом cookie.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-169">When each app is hosted on its own domain, there is no implicit cookie trust relationship to exploit.</span></span>

## <a name="aspnet-core-antiforgery-configuration"></a><span data-ttu-id="a9bdf-170">Настройка защиты от подделки ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a9bdf-170">ASP.NET Core antiforgery configuration</span></span>

> [!WARNING]
> <span data-ttu-id="a9bdf-171">ASP.NET Core реализует подделку с помощью [ASP.NET Core защиты данных](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="a9bdf-171">ASP.NET Core implements antiforgery using [ASP.NET Core Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="a9bdf-172">Стек защиты данных должен быть настроен для работы в ферме серверов.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-172">The data protection stack must be configured to work in a server farm.</span></span> <span data-ttu-id="a9bdf-173">Дополнительные сведения см. в разделе [Настройка защиты данных](xref:security/data-protection/configuration/overview) .</span><span class="sxs-lookup"><span data-stu-id="a9bdf-173">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a9bdf-174">По промежуточного слоя для подделки добавляется в контейнер [внедрения зависимостей](xref:fundamentals/dependency-injection) , когда в `Startup.ConfigureServices`вызывается один из следующих интерфейсов API:</span><span class="sxs-lookup"><span data-stu-id="a9bdf-174">Antiforgery middleware is added to the [Dependency injection](xref:fundamentals/dependency-injection) container when one of the following APIs is called in `Startup.ConfigureServices`:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>
* <xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapRazorPages*>
* <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>
* <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub*>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a9bdf-175">По промежуточного слоя для подделки добавляется в контейнер [внедрения зависимостей](xref:fundamentals/dependency-injection) при вызове <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> в `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="a9bdf-175">Antiforgery middleware is added to the [Dependency injection](xref:fundamentals/dependency-injection) container when <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> is called in `Startup.ConfigureServices`</span></span>

::: moniker-end

<span data-ttu-id="a9bdf-176">В ASP.NET Core 2,0 или более поздней версии [формтагхелпер](xref:mvc/views/working-with-forms#the-form-tag-helper) вставляет маркеры для защиты от подделки в элементы HTML-форм.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-176">In ASP.NET Core 2.0 or later, the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span> <span data-ttu-id="a9bdf-177">Следующая разметка в файле Razor автоматически создает маркеры для защиты от подделки:</span><span class="sxs-lookup"><span data-stu-id="a9bdf-177">The following markup in a Razor file automatically generates antiforgery tokens:</span></span>

```cshtml
<form method="post">
    ...
</form>
```

<span data-ttu-id="a9bdf-178">Аналогичным образом [ихтмлхелпер. бегинформ](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) по умолчанию создает маркеры подделки, если метод формы не получает значение.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-178">Similarly, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generates antiforgery tokens by default if the form's method isn't GET.</span></span>

<span data-ttu-id="a9bdf-179">Автоматическое создание маркеров защиты от подделки для HTML-элементов форм происходит, когда тег `<form>` содержит атрибут `method="post"` и выполняется одно из следующих условий.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-179">The automatic generation of antiforgery tokens for HTML form elements happens when the `<form>` tag contains the `method="post"` attribute and either of the following are true:</span></span>

* <span data-ttu-id="a9bdf-180">Атрибут action пуст (`action=""`).</span><span class="sxs-lookup"><span data-stu-id="a9bdf-180">The action attribute is empty (`action=""`).</span></span>
* <span data-ttu-id="a9bdf-181">Атрибут Action не указан (`<form method="post">`).</span><span class="sxs-lookup"><span data-stu-id="a9bdf-181">The action attribute isn't supplied (`<form method="post">`).</span></span>

<span data-ttu-id="a9bdf-182">Автоматическое создание маркеров подделки для элементов HTML-форм можно отключить:</span><span class="sxs-lookup"><span data-stu-id="a9bdf-182">Automatic generation of antiforgery tokens for HTML form elements can be disabled:</span></span>

* <span data-ttu-id="a9bdf-183">Явно отключите токены защиты от подделки с помощью атрибута `asp-antiforgery`:</span><span class="sxs-lookup"><span data-stu-id="a9bdf-183">Explicitly disable antiforgery tokens with the `asp-antiforgery` attribute:</span></span>

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* <span data-ttu-id="a9bdf-184">Элемент form не участвует в вспомогательных функциях тегов с помощью [символа Helper! символ согласия](xref:mvc/views/tag-helpers/intro#opt-out):</span><span class="sxs-lookup"><span data-stu-id="a9bdf-184">The form element is opted-out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out):</span></span>

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* <span data-ttu-id="a9bdf-185">Удалите `FormTagHelper` из представления.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-185">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="a9bdf-186">`FormTagHelper` можно удалить из представления, добавив следующую директиву в представление Razor:</span><span class="sxs-lookup"><span data-stu-id="a9bdf-186">The `FormTagHelper` can be removed from a view by adding the following directive to the Razor view:</span></span>

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="a9bdf-187">[Razor Pages](xref:razor-pages/index) автоматически защищаются от XSRF/CSRF.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-187">[Razor Pages](xref:razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="a9bdf-188">Дополнительные сведения см. в разделе [XSRF/CSRF and Razor Pages](xref:razor-pages/index#xsrf).</span><span class="sxs-lookup"><span data-stu-id="a9bdf-188">For more information, see [XSRF/CSRF and Razor Pages](xref:razor-pages/index#xsrf).</span></span>

<span data-ttu-id="a9bdf-189">Наиболее распространенным подходом к защите от атак CSRF является использование *шаблона токена синхронизатора* (STP).</span><span class="sxs-lookup"><span data-stu-id="a9bdf-189">The most common approach to defending against CSRF attacks is to use the *Synchronizer Token Pattern* (STP).</span></span> <span data-ttu-id="a9bdf-190">STP используется, когда пользователь запрашивает страницу с данными формы:</span><span class="sxs-lookup"><span data-stu-id="a9bdf-190">STP is used when the user requests a page with form data:</span></span>

1. <span data-ttu-id="a9bdf-191">Сервер отправляет клиенту маркер, связанный с удостоверением текущего пользователя.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-191">The server sends a token associated with the current user's identity to the client.</span></span>
1. <span data-ttu-id="a9bdf-192">Клиент отправляет на сервер токен, чтобы выполнить проверку.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-192">The client sends back the token to the server for verification.</span></span>
1. <span data-ttu-id="a9bdf-193">Если сервер получает маркер, который не соответствует удостоверению аутентифицированного пользователя, запрос отклоняется.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-193">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span>

<span data-ttu-id="a9bdf-194">Маркер является уникальным и непредсказуемым.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-194">The token is unique and unpredictable.</span></span> <span data-ttu-id="a9bdf-195">Маркер также можно использовать для обеспечения надлежащей последовательности последовательности запросов (например, для обеспечения последовательности запроса: страница 1 &ndash; стр. 2 &ndash; стр. 3).</span><span class="sxs-lookup"><span data-stu-id="a9bdf-195">The token can also be used to ensure proper sequencing of a series of requests (for example, ensuring the request sequence of: page 1 &ndash; page 2 &ndash; page 3).</span></span> <span data-ttu-id="a9bdf-196">Все формы в ASP.NET Core шаблонах MVC и Razor Pages создают маркеры защиты от подделки.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-196">All of the forms in ASP.NET Core MVC and Razor Pages templates generate antiforgery tokens.</span></span> <span data-ttu-id="a9bdf-197">В следующей паре примеров представления создаются маркеры защиты от подделки:</span><span class="sxs-lookup"><span data-stu-id="a9bdf-197">The following pair of view examples generate antiforgery tokens:</span></span>

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

<span data-ttu-id="a9bdf-198">Явно добавьте токен защиты от подделки к элементу `<form>` без использования вспомогательных функций тегов при помощи [`@Html.AntiForgeryToken`](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken)поддержки HTML:</span><span class="sxs-lookup"><span data-stu-id="a9bdf-198">Explicitly add an antiforgery token to a `<form>` element without using Tag Helpers with the HTML helper [`@Html.AntiForgeryToken`](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span></span>

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="a9bdf-199">В каждом из предыдущих случаев ASP.NET Core добавляет скрытое поле формы, аналогичное следующему:</span><span class="sxs-lookup"><span data-stu-id="a9bdf-199">In each of the preceding cases, ASP.NET Core adds a hidden form field similar to the following:</span></span>

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

<span data-ttu-id="a9bdf-200">ASP.NET Core включает три [фильтра](xref:mvc/controllers/filters) для работы с маркерами подделки:</span><span class="sxs-lookup"><span data-stu-id="a9bdf-200">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens:</span></span>

* [<span data-ttu-id="a9bdf-201">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="a9bdf-201">ValidateAntiForgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [<span data-ttu-id="a9bdf-202">аутовалидатеантифоржеритокен</span><span class="sxs-lookup"><span data-stu-id="a9bdf-202">AutoValidateAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [<span data-ttu-id="a9bdf-203">игнореантифоржеритокен</span><span class="sxs-lookup"><span data-stu-id="a9bdf-203">IgnoreAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a><span data-ttu-id="a9bdf-204">Параметры защиты от подделки</span><span class="sxs-lookup"><span data-stu-id="a9bdf-204">Antiforgery options</span></span>

<span data-ttu-id="a9bdf-205">Настройка [параметров защиты от подделки](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a9bdf-205">Customize [antiforgery options](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="a9bdf-206">&dagger;настроить подделку `Cookie` свойства, используя свойства класса [кукиебуилдер](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder) .</span><span class="sxs-lookup"><span data-stu-id="a9bdf-206">&dagger;Set the antiforgery `Cookie` properties using the properties of the [CookieBuilder](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder) class.</span></span>

| <span data-ttu-id="a9bdf-207">Параметр</span><span class="sxs-lookup"><span data-stu-id="a9bdf-207">Option</span></span> | <span data-ttu-id="a9bdf-208">Описание</span><span class="sxs-lookup"><span data-stu-id="a9bdf-208">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="a9bdf-209">Файл cookie</span><span class="sxs-lookup"><span data-stu-id="a9bdf-209">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="a9bdf-210">Определяет параметры, используемые для создания файлов cookie подделки.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-210">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="a9bdf-211">формфиелднаме</span><span class="sxs-lookup"><span data-stu-id="a9bdf-211">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="a9bdf-212">Имя скрытого поля формы, используемое системой защиты от подделки для отображения маркеров подделки в представлениях.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-212">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="a9bdf-213">хеадернаме</span><span class="sxs-lookup"><span data-stu-id="a9bdf-213">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="a9bdf-214">Имя заголовка, используемого системой защиты от подделки.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-214">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="a9bdf-215">Если `null`, система рассматривает только данные формы.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-215">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="a9bdf-216">суппрессксфрамеоптионшеадер</span><span class="sxs-lookup"><span data-stu-id="a9bdf-216">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="a9bdf-217">Указывает, следует ли подавлять Создание заголовка `X-Frame-Options`.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-217">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="a9bdf-218">По умолчанию заголовок создается со значением «САМЕОРИГИН».</span><span class="sxs-lookup"><span data-stu-id="a9bdf-218">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="a9bdf-219">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-219">Defaults to `false`.</span></span> |

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

| <span data-ttu-id="a9bdf-220">Параметр</span><span class="sxs-lookup"><span data-stu-id="a9bdf-220">Option</span></span> | <span data-ttu-id="a9bdf-221">Описание</span><span class="sxs-lookup"><span data-stu-id="a9bdf-221">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="a9bdf-222">Файл cookie</span><span class="sxs-lookup"><span data-stu-id="a9bdf-222">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="a9bdf-223">Определяет параметры, используемые для создания файлов cookie подделки.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-223">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="a9bdf-224">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="a9bdf-224">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | <span data-ttu-id="a9bdf-225">Домен cookie.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-225">The domain of the cookie.</span></span> <span data-ttu-id="a9bdf-226">По умолчанию — `null`.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-226">Defaults to `null`.</span></span> <span data-ttu-id="a9bdf-227">Это свойство устарело и будет удалено в следующей версии.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-227">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="a9bdf-228">Взамен рекомендуется использовать файл cookie. domain.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-228">The recommended alternative is Cookie.Domain.</span></span> |
| [<span data-ttu-id="a9bdf-229">CookieName</span><span class="sxs-lookup"><span data-stu-id="a9bdf-229">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | <span data-ttu-id="a9bdf-230">Имя cookie.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-230">The name of the cookie.</span></span> <span data-ttu-id="a9bdf-231">Если значение не задано, система создает уникальное имя, начинающееся с [дефаулткукиепрефикс](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (". AspNetCore. подделка. ").</span><span class="sxs-lookup"><span data-stu-id="a9bdf-231">If not set, the system generates a unique name beginning with the [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (".AspNetCore.Antiforgery.").</span></span> <span data-ttu-id="a9bdf-232">Это свойство устарело и будет удалено в следующей версии.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-232">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="a9bdf-233">Рекомендуемой альтернативой является Cookie.Name.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-233">The recommended alternative is Cookie.Name.</span></span> |
| [<span data-ttu-id="a9bdf-234">CookiePath</span><span class="sxs-lookup"><span data-stu-id="a9bdf-234">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | <span data-ttu-id="a9bdf-235">Путь, заданный для файла cookie.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-235">The path set on the cookie.</span></span> <span data-ttu-id="a9bdf-236">Это свойство устарело и будет удалено в следующей версии.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-236">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="a9bdf-237">Взамен рекомендуется использовать файл cookie. Path.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-237">The recommended alternative is Cookie.Path.</span></span> |
| [<span data-ttu-id="a9bdf-238">формфиелднаме</span><span class="sxs-lookup"><span data-stu-id="a9bdf-238">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="a9bdf-239">Имя скрытого поля формы, используемое системой защиты от подделки для отображения маркеров подделки в представлениях.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-239">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="a9bdf-240">хеадернаме</span><span class="sxs-lookup"><span data-stu-id="a9bdf-240">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="a9bdf-241">Имя заголовка, используемого системой защиты от подделки.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-241">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="a9bdf-242">Если `null`, система рассматривает только данные формы.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-242">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="a9bdf-243">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="a9bdf-243">RequireSsl</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | <span data-ttu-id="a9bdf-244">Указывает, требуется ли протокол HTTPS в системе защиты от подделки.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-244">Specifies whether HTTPS is required by the antiforgery system.</span></span> <span data-ttu-id="a9bdf-245">Если `true`, то запросы, не относящиеся к HTTPS, завершаются сбоем.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-245">If `true`, non-HTTPS requests fail.</span></span> <span data-ttu-id="a9bdf-246">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-246">Defaults to `false`.</span></span> <span data-ttu-id="a9bdf-247">Это свойство устарело и будет удалено в следующей версии.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-247">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="a9bdf-248">Рекомендуемым альтернативным вариантом является установка файла cookie. Секуреполици.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-248">The recommended alternative is to set Cookie.SecurePolicy.</span></span> |
| [<span data-ttu-id="a9bdf-249">суппрессксфрамеоптионшеадер</span><span class="sxs-lookup"><span data-stu-id="a9bdf-249">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="a9bdf-250">Указывает, следует ли подавлять Создание заголовка `X-Frame-Options`.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-250">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="a9bdf-251">По умолчанию заголовок создается со значением «САМЕОРИГИН».</span><span class="sxs-lookup"><span data-stu-id="a9bdf-251">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="a9bdf-252">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-252">Defaults to `false`.</span></span> |

::: moniker-end

<span data-ttu-id="a9bdf-253">Дополнительные сведения см. в разделе [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span><span class="sxs-lookup"><span data-stu-id="a9bdf-253">For more information, see [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span></span>

## <a name="configure-antiforgery-features-with-iantiforgery"></a><span data-ttu-id="a9bdf-254">Настройка функций защиты от подделки с помощью Иантифоржери</span><span class="sxs-lookup"><span data-stu-id="a9bdf-254">Configure antiforgery features with IAntiforgery</span></span>

<span data-ttu-id="a9bdf-255">[Иантифоржери](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) предоставляет API для настройки функций защиты от подделки.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-255">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) provides the API to configure antiforgery features.</span></span> <span data-ttu-id="a9bdf-256">`IAntiforgery` можно запросить в методе `Configure` класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-256">`IAntiforgery` can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="a9bdf-257">В следующем примере по промежуточного слоя с домашней страницы приложения используется для создания токена защиты от подделки и отправки его в ответ в виде файла cookie (с использованием стандартного соглашения об именовании, описанного далее в этом разделе):</span><span class="sxs-lookup"><span data-stu-id="a9bdf-257">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described later in this topic):</span></span>

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

### <a name="require-antiforgery-validation"></a><span data-ttu-id="a9bdf-258">Требовать проверку защиты от подделки</span><span class="sxs-lookup"><span data-stu-id="a9bdf-258">Require antiforgery validation</span></span>

<span data-ttu-id="a9bdf-259">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) — это фильтр действий, который можно применить к отдельному действию, контроллеру или глобально.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-259">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="a9bdf-260">Запросы к действиям, к которым применен этот фильтр, блокируются, если запрос не содержит допустимый токен защиты от подделки.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-260">Requests made to actions that have this filter applied are blocked unless the request includes a valid antiforgery token.</span></span>

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

<span data-ttu-id="a9bdf-261">Атрибут `ValidateAntiForgeryToken` требует наличия маркера для запросов к методам действий, которые он помечает, включая HTTP-запросы GET.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-261">The `ValidateAntiForgeryToken` attribute requires a token for requests to the action methods it marks, including HTTP GET requests.</span></span> <span data-ttu-id="a9bdf-262">Если атрибут `ValidateAntiForgeryToken` применяется на контроллерах приложения, его можно переопределить с помощью атрибута `IgnoreAntiforgeryToken`.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-262">If the `ValidateAntiForgeryToken` attribute is applied across the app's controllers, it can be overridden with the `IgnoreAntiforgeryToken` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="a9bdf-263">ASP.NET Core не поддерживает добавление маркеров подделки для автоматического получения запросов.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-263">ASP.NET Core doesn't support adding antiforgery tokens to GET requests automatically.</span></span>

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a><span data-ttu-id="a9bdf-264">Автоматически проверять маркеры подделки для ненадежных методов HTTP</span><span class="sxs-lookup"><span data-stu-id="a9bdf-264">Automatically validate antiforgery tokens for unsafe HTTP methods only</span></span>

<span data-ttu-id="a9bdf-265">ASP.NET Core приложения не создают маркеры для защиты от подделки для безопасного метода HTTP (получение, HEAD, параметры и ТРАССИРОВКа).</span><span class="sxs-lookup"><span data-stu-id="a9bdf-265">ASP.NET Core apps don't generate antiforgery tokens for safe HTTP methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="a9bdf-266">Вместо широкого применения атрибута `ValidateAntiForgeryToken` и последующего его переопределения с `IgnoreAntiforgeryToken` атрибутами можно использовать атрибут [аутовалидатеантифоржеритокен](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) .</span><span class="sxs-lookup"><span data-stu-id="a9bdf-266">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, the [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribute can be used.</span></span> <span data-ttu-id="a9bdf-267">Этот атрибут работает идентично атрибуту `ValidateAntiForgeryToken`, за исключением того, что для запросов, выполняемых с помощью следующих методов HTTP, не требуются маркеры:</span><span class="sxs-lookup"><span data-stu-id="a9bdf-267">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="a9bdf-268">GET</span><span class="sxs-lookup"><span data-stu-id="a9bdf-268">GET</span></span>
* <span data-ttu-id="a9bdf-269">HEAD,</span><span class="sxs-lookup"><span data-stu-id="a9bdf-269">HEAD</span></span>
* <span data-ttu-id="a9bdf-270">OPTIONS</span><span class="sxs-lookup"><span data-stu-id="a9bdf-270">OPTIONS</span></span>
* <span data-ttu-id="a9bdf-271">TRACE</span><span class="sxs-lookup"><span data-stu-id="a9bdf-271">TRACE</span></span>

<span data-ttu-id="a9bdf-272">Рекомендуется использовать `AutoValidateAntiforgeryToken` широко для сценариев, не относящихся к API.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-272">We recommend use of `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="a9bdf-273">Это гарантирует, что действия POST по умолчанию защищаются.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-273">This ensures POST actions are protected by default.</span></span> <span data-ttu-id="a9bdf-274">Альтернативой является игнорирование маркеров подделки по умолчанию, если только `ValidateAntiForgeryToken` не применяются к отдельным методам действий.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-274">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to individual action methods.</span></span> <span data-ttu-id="a9bdf-275">Более вероятно, что в этом случае метод действия POST остается незащищенным по ошибке, что оставляет приложение уязвимым для атак CSRF.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-275">It's more likely in this scenario for a POST action method to be left unprotected by mistake, leaving the app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="a9bdf-276">Все записи должны отправить токен защиты от подделки.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-276">All POSTs should send the antiforgery token.</span></span>

<span data-ttu-id="a9bdf-277">Интерфейсы API не имеют автоматического механизма отправки части маркера, отличной от файла cookie.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-277">APIs don't have an automatic mechanism for sending the non-cookie part of the token.</span></span> <span data-ttu-id="a9bdf-278">Реализация, вероятно, зависит от реализации клиентского кода.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-278">The implementation probably depends on the client code implementation.</span></span> <span data-ttu-id="a9bdf-279">Ниже приведены некоторые примеры.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-279">Some examples are shown below:</span></span>

<span data-ttu-id="a9bdf-280">Пример уровня класса:</span><span class="sxs-lookup"><span data-stu-id="a9bdf-280">Class-level example:</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="a9bdf-281">Глобальный пример:</span><span class="sxs-lookup"><span data-stu-id="a9bdf-281">Global example:</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a><span data-ttu-id="a9bdf-282">Переопределение атрибутов или подделки глобальных или контроллеров</span><span class="sxs-lookup"><span data-stu-id="a9bdf-282">Override global or controller antiforgery attributes</span></span>

<span data-ttu-id="a9bdf-283">Фильтр [игнореантифоржеритокен](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) используется для устранения необходимости в токене подделки для данного действия (или контроллера).</span><span class="sxs-lookup"><span data-stu-id="a9bdf-283">The [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filter is used to eliminate the need for an antiforgery token for a given action (or controller).</span></span> <span data-ttu-id="a9bdf-284">При применении этот фильтр переопределяет `ValidateAntiForgeryToken` и `AutoValidateAntiforgeryToken` фильтры, указанные на более высоком уровне (глобально или на контроллере).</span><span class="sxs-lookup"><span data-stu-id="a9bdf-284">When applied, this filter overrides `ValidateAntiForgeryToken` and `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

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

## <a name="refresh-tokens-after-authentication"></a><span data-ttu-id="a9bdf-285">Обновить маркеры после проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="a9bdf-285">Refresh tokens after authentication</span></span>

<span data-ttu-id="a9bdf-286">Токены следует обновлять после проверки подлинности пользователя путем перенаправления пользователя на представление или на Razor Pages страницу.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-286">Tokens should be refreshed after the user is authenticated by redirecting the user to a view or Razor Pages page.</span></span>

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="a9bdf-287">JavaScript, AJAX и одностраничные приложения</span><span class="sxs-lookup"><span data-stu-id="a9bdf-287">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="a9bdf-288">В традиционных приложениях на основе HTML маркеры защиты от подделки передаются на сервер с помощью скрытых полей формы.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-288">In traditional HTML-based apps, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="a9bdf-289">В современных приложениях на основе JavaScript и одностраничные приложения многие запросы выполняются программным образом.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-289">In modern JavaScript-based apps and SPAs, many requests are made programmatically.</span></span> <span data-ttu-id="a9bdf-290">Эти запросы AJAX могут использовать другие методы (например, заголовки запросов или файлы cookie) для отправки маркера.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-290">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span>

<span data-ttu-id="a9bdf-291">Если файлы cookie используются для хранения маркеров проверки подлинности и для проверки подлинности запросов API на сервере, CSRF является потенциальной проблемой.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-291">If cookies are used to store authentication tokens and to authenticate API requests on the server, CSRF is a potential problem.</span></span> <span data-ttu-id="a9bdf-292">Если для хранения маркера используется локальное хранилище, уязвимость CSRF может быть устранена, так как значения из локального хранилища не отправляются на сервер автоматически при каждом запросе.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-292">If local storage is used to store the token, CSRF vulnerability might be mitigated because values from local storage aren't sent automatically to the server with every request.</span></span> <span data-ttu-id="a9bdf-293">Поэтому рекомендуемым подходом является использование локального хранилища для хранения токена защиты от подделки на клиенте и отправка маркера в качестве заголовка запроса.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-293">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="javascript"></a><span data-ttu-id="a9bdf-294">JavaScript</span><span class="sxs-lookup"><span data-stu-id="a9bdf-294">JavaScript</span></span>

<span data-ttu-id="a9bdf-295">Используя JavaScript с представлениями, маркер можно создать с помощью службы в представлении.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-295">Using JavaScript with views, the token can be created using a service from within the view.</span></span> <span data-ttu-id="a9bdf-296">Вставьте службу [Microsoft. AspNetCore. подделка. иантифоржери](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) в представление и вызовите [жетандсторетокенс](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span><span class="sxs-lookup"><span data-stu-id="a9bdf-296">Inject the [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service into the view and call [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

<span data-ttu-id="a9bdf-297">Такой подход устраняет необходимость непосредственно в настройке файлов cookie с сервера или считывании их из клиента.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-297">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="a9bdf-298">В предыдущем примере для чтения значения скрытого поля заголовка AJAX POST используется JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-298">The preceding example uses JavaScript to read the hidden field value for the AJAX POST header.</span></span>

<span data-ttu-id="a9bdf-299">JavaScript также может получать доступ к маркерам в файлах cookie и использовать содержимое файла cookie для создания заголовка со значением маркера.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-299">JavaScript can also access tokens in cookies and use the cookie's contents to create a header with the token's value.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="a9bdf-300">Предполагая, что сценарий отправляет маркер в заголовок с именем `X-CSRF-TOKEN`, настройте службу защиты от подделки для поиска заголовка `X-CSRF-TOKEN`:</span><span class="sxs-lookup"><span data-stu-id="a9bdf-300">Assuming the script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="a9bdf-301">В следующем примере используется JavaScript для создания запроса AJAX с соответствующим заголовком:</span><span class="sxs-lookup"><span data-stu-id="a9bdf-301">The following example uses JavaScript to make an AJAX request with the appropriate header:</span></span>

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

### <a name="angularjs"></a><span data-ttu-id="a9bdf-302">AngularJS</span><span class="sxs-lookup"><span data-stu-id="a9bdf-302">AngularJS</span></span>

<span data-ttu-id="a9bdf-303">В AngularJS используется соглашение для адресации CSRF.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-303">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="a9bdf-304">Если сервер отправляет файл cookie с именем `XSRF-TOKEN`, служба AngularJS `$http` добавляет значение файла cookie в заголовок при отправке запроса на сервер.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-304">If the server sends a cookie with the name `XSRF-TOKEN`, the AngularJS `$http` service adds the cookie value to a header when it sends a request to the server.</span></span> <span data-ttu-id="a9bdf-305">Этот процесс выполняется автоматически.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-305">This process is automatic.</span></span> <span data-ttu-id="a9bdf-306">Заголовок не обязательно задавать в клиенте явным образом.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-306">The header doesn't need to be set in the client explicitly.</span></span> <span data-ttu-id="a9bdf-307">Имя заголовка — `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-307">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="a9bdf-308">Сервер должен обнаружить этот заголовок и проверить его содержимое.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-308">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="a9bdf-309">Для ASP.NET Core API для работы с этим соглашением при запуске приложения:</span><span class="sxs-lookup"><span data-stu-id="a9bdf-309">For ASP.NET Core API to work with this convention in your application startup:</span></span>

* <span data-ttu-id="a9bdf-310">Настройте приложение, чтобы предоставить маркер в файле cookie с именем `XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-310">Configure your app to provide a token in a cookie called `XSRF-TOKEN`.</span></span>
* <span data-ttu-id="a9bdf-311">Настройте службу защиты от подделки для поиска заголовка с именем `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-311">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`.</span></span>

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

public void ConfigureServices(IServiceCollection services)
{
    // Angular's default header name for sending the XSRF token.
    services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
}
```

<span data-ttu-id="a9bdf-312">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a9bdf-312">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="extend-antiforgery"></a><span data-ttu-id="a9bdf-313">Расширение защиты от подделки</span><span class="sxs-lookup"><span data-stu-id="a9bdf-313">Extend antiforgery</span></span>

<span data-ttu-id="a9bdf-314">Тип [иантифоржеряддитионалдатапровидер](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) позволяет разработчикам расширять поведение системы защиты от CSRF, округляя дополнительные данные в каждом маркере.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-314">The [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-CSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="a9bdf-315">Метод [жетаддитионалдата](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) вызывается каждый раз при создании маркера поля, а возвращаемое значение внедряется в созданный токен.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-315">The [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="a9bdf-316">Разработчик может вернуть метку времени, nonce или любое другое значение, а затем вызвать [валидатеаддитионалдата](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) для проверки этих данных при проверке маркера.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-316">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) to validate this data when the token is validated.</span></span> <span data-ttu-id="a9bdf-317">Имя пользователя клиента уже внедрено в созданные токены, поэтому нет необходимости включать эти сведения.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-317">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="a9bdf-318">Если маркер включает дополнительные данные, но `IAntiForgeryAdditionalDataProvider` не настроены, дополнительные данные не проверяются.</span><span class="sxs-lookup"><span data-stu-id="a9bdf-318">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` is configured, the supplemental data isn't validated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a9bdf-319">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a9bdf-319">Additional resources</span></span>

* <span data-ttu-id="a9bdf-320">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) в [открытом проекте безопасности веб-приложений](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="a9bdf-320">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
* <xref:host-and-deploy/web-farm>
