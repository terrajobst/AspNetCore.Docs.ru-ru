---
title: "Предотвращение атак перенаправления открыть в приложении ASP.NET Core | Документы Microsoft"
author: ardalis
description: "Показано, как предотвратить атаки с открытым перенаправления приложения ASP.NET Core"
keywords: "Атака ASP.NET Core, безопасности, откройте перенаправления"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: article
ms.assetid: 4604e563-e91a-4ecd-b7ed-00b3f1eee2b5
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/preventing-open-redirects
ms.openlocfilehash: 4083845a77eb19d9ba9beb389a92ceb5c14edbde
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="preventing-open-redirect-attacks-in-an-aspnet-core-app"></a><span data-ttu-id="41994-104">Предотвращение атак перенаправления открыть в приложении ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="41994-104">Preventing Open Redirect Attacks in an ASP.NET Core app</span></span>

<span data-ttu-id="41994-105">Веб-приложения, который перенаправляет на URL-адрес, указанный через запрос, например, данные строки запроса или формы потенциально могут быть подменены для перенаправления пользователей на внешний, вредоносные URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="41994-105">A web app that redirects to a URL that is specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="41994-106">Это изменение данных называется атака откройте перенаправления.</span><span class="sxs-lookup"><span data-stu-id="41994-106">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="41994-107">Каждый раз, когда указанный URL-адрес перенаправляет логики приложения, необходимо убедиться, что URL-адрес перенаправления не было изменено.</span><span class="sxs-lookup"><span data-stu-id="41994-107">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="41994-108">ASP.NET Core имеет встроенные функциональные возможности для защиты приложения от атак откройте перенаправления (также известный как открытые перенаправление).</span><span class="sxs-lookup"><span data-stu-id="41994-108">ASP.NET Core has built-in functionality to help protect apps from open redirect (also known as open redirection) attacks.</span></span>

## <a name="what-is-an-open-redirect-attack"></a><span data-ttu-id="41994-109">Что такое атака откройте перенаправления</span><span class="sxs-lookup"><span data-stu-id="41994-109">What is an open redirect attack?</span></span>

<span data-ttu-id="41994-110">Веб-приложений часто перенаправлять пользователей на страницу входа при доступе к ресурсам, требующим проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="41994-110">Web applications frequently redirect users to a login page when they access resources that require authentication.</span></span> <span data-ttu-id="41994-111">Включает перенаправление typlically `returnUrl` параметр строки запроса, чтобы пользователь могут быть возвращены первоначально запрошенному URL-адресу после их успешного входа.</span><span class="sxs-lookup"><span data-stu-id="41994-111">The redirection typlically includes a `returnUrl` querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span> <span data-ttu-id="41994-112">После проверки подлинности пользователя, он перенаправляется на первоначально запрошенных ими URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="41994-112">After the user authenticates, they are redirected to the URL they had originally requested.</span></span>

<span data-ttu-id="41994-113">Поскольку в строке запроса запроса указан URL-адрес назначения, злонамеренный пользователь может незаконно изменить строку запроса.</span><span class="sxs-lookup"><span data-stu-id="41994-113">Because the destination URL is specified in the querystring of the request, a malicious user could tamper with the querystring.</span></span> <span data-ttu-id="41994-114">Измененные querystring делает возможным перенаправлять пользователя на узле внешних, вредоносный сайт.</span><span class="sxs-lookup"><span data-stu-id="41994-114">A tampered querystring could allow the site to redirect the user to an external, malicious site.</span></span> <span data-ttu-id="41994-115">Этот прием называется атака откройте перенаправления (или перенаправления).</span><span class="sxs-lookup"><span data-stu-id="41994-115">This technique is called an open redirect (or redirection) attack.</span></span>

### <a name="an-example-attack"></a><span data-ttu-id="41994-116">Пример атаки</span><span class="sxs-lookup"><span data-stu-id="41994-116">An example attack</span></span>

<span data-ttu-id="41994-117">Пользователь-злоумышленник может разработать атака предусмотрено разрешение злоумышленнику доступ к конфиденциальной информации в приложении или учетные данные пользователя.</span><span class="sxs-lookup"><span data-stu-id="41994-117">A malicious user could develop an attack intended to allow the malicious user access to a user's credentials or sensitive information on your app.</span></span> <span data-ttu-id="41994-118">Чтобы начать атаку, они убедить пользователя со ссылкой на страницу входа веб-узла, с `returnUrl` значение строки запроса, добавляемое к URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="41994-118">To begin the attack, they convince the user to click a link to your site's login page, with a `returnUrl` querystring value added to the URL.</span></span> <span data-ttu-id="41994-119">Например [NerdDinner.com](http://nerddinner.com) пример приложения (написаны для ASP.NET MVC) включает в себя такие страницы входа здесь: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``.</span><span class="sxs-lookup"><span data-stu-id="41994-119">For example, the [NerdDinner.com](http://nerddinner.com) sample application (written for ASP.NET MVC) includes such a login page here: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``.</span></span> <span data-ttu-id="41994-120">Атака затем включает следующие шаги.</span><span class="sxs-lookup"><span data-stu-id="41994-120">The attack then follows these steps:</span></span>

1. <span data-ttu-id="41994-121">Пользователь щелкает ссылку на ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (Обратите внимание, что второй URL-адрес — nerddi**n**er, не nerddi**nn**er).</span><span class="sxs-lookup"><span data-stu-id="41994-121">User clicks a link to ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (note, second URL is nerddi**n**er, not nerddi**nn**er).</span></span>
2. <span data-ttu-id="41994-122">Пользователь успешно вошел.</span><span class="sxs-lookup"><span data-stu-id="41994-122">The user logs in successfully.</span></span>
3. <span data-ttu-id="41994-123">Пользователь перенаправляется (узлом) ``http://nerddiner.com/Account/LogOn`` (вредоносный сайт, выглядит как настоящий узел).</span><span class="sxs-lookup"><span data-stu-id="41994-123">The user is redirected (by the site) to ``http://nerddiner.com/Account/LogOn`` (malicious site that looks like real site).</span></span>
4. <span data-ttu-id="41994-124">Пользователь снова войдет систему (предоставление вредоносных сайтов свои учетные данные), а перенаправляется обратно на сайт real.</span><span class="sxs-lookup"><span data-stu-id="41994-124">The user logs in again (giving malicious site their credentials) and is redirected back to the real site.</span></span>

<span data-ttu-id="41994-125">Пользователь будет скорее всего считать их первой попытки входа не удалось, и их второй прошла успешно.</span><span class="sxs-lookup"><span data-stu-id="41994-125">The user will likely believe their first attempt to log in failed, and their second one was successful.</span></span> <span data-ttu-id="41994-126">Они скорее сохранится и знает компрометации учетных данных.</span><span class="sxs-lookup"><span data-stu-id="41994-126">They'll most likely remain unaware their credentials have been compromised.</span></span>

![Процесс атаки откройте перенаправления](preventing-open-redirects/_static/open-redirection-attack-process.png)

<span data-ttu-id="41994-128">Помимо страницы входа некоторые веб-сайты предоставляют перенаправление страницы или конечных точек.</span><span class="sxs-lookup"><span data-stu-id="41994-128">In addition to login pages, some sites provide redirect pages or endpoints.</span></span> <span data-ttu-id="41994-129">Предположим, приложение страницы с помощью открытые перенаправления ``/Home/Redirect``.</span><span class="sxs-lookup"><span data-stu-id="41994-129">Imagine your app has a page with an open redirect, ``/Home/Redirect``.</span></span> <span data-ttu-id="41994-130">Злоумышленник может создать, например, ссылки в сообщении электронной почты, который проходит на ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``.</span><span class="sxs-lookup"><span data-stu-id="41994-130">An attacker could create, for example, a link in an email that goes to ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``.</span></span> <span data-ttu-id="41994-131">Обычного пользователя будет взгляните на URL-адрес и узнать, когда он начинается с имени веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="41994-131">A typical user will look at the URL and see it begins with your site name.</span></span> <span data-ttu-id="41994-132">Предоставление доверия, они будут по ссылке.</span><span class="sxs-lookup"><span data-stu-id="41994-132">Trusting that, they will click the link.</span></span> <span data-ttu-id="41994-133">Откройте перенаправления затем будет направлять пользователя фишинг сайта, который выглядит идентичные к вам страну, и пользователь скорее всего будет имя входа, они предполагать является веб-узла.</span><span class="sxs-lookup"><span data-stu-id="41994-133">The open redirect would then send the user to the phishing site, which looks identical to yours, and the user would likely login to what they believe is your site.</span></span>

## <a name="protecting-against-open-redirect-attacks"></a><span data-ttu-id="41994-134">Защита от атак перенаправления open</span><span class="sxs-lookup"><span data-stu-id="41994-134">Protecting against open redirect attacks</span></span>

<span data-ttu-id="41994-135">При разработке веб-приложений, считать все данные, предоставленные пользователем не заслуживающих доверия.</span><span class="sxs-lookup"><span data-stu-id="41994-135">When developing web applications, treat all user-provided data as untrustworthy.</span></span> <span data-ttu-id="41994-136">Если приложение имеет функции, который перенаправляет пользователя на основе содержимого URL-адрес, убедитесь, что такие перенаправления осуществляется только локально внутри приложения (или на известный URL-адрес, не может быть указано в строке запроса URL-адрес).</span><span class="sxs-lookup"><span data-stu-id="41994-136">If your application has functionality that redirects the user based on the contents of the URL,  ensure that such redirects are only done locally within your app (or to a known URL, not any URL that may be supplied in the querystring).</span></span>

### <a name="localredirect"></a><span data-ttu-id="41994-137">LocalRedirect</span><span class="sxs-lookup"><span data-stu-id="41994-137">LocalRedirect</span></span>

<span data-ttu-id="41994-138">Используйте ``LocalRedirect`` вспомогательный метод из базового `Controller` класса:</span><span class="sxs-lookup"><span data-stu-id="41994-138">Use the ``LocalRedirect`` helper method from the base `Controller` class:</span></span>

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

<span data-ttu-id="41994-139">``LocalRedirect``Если указан URL-адрес нелокальных вызовет исключение.</span><span class="sxs-lookup"><span data-stu-id="41994-139">``LocalRedirect`` will throw an exception if a non-local URL is specified.</span></span> <span data-ttu-id="41994-140">В противном случае он ведет себя так же, как ``Redirect`` метод.</span><span class="sxs-lookup"><span data-stu-id="41994-140">Otherwise, it behaves just like the ``Redirect`` method.</span></span>

### <a name="islocalurl"></a><span data-ttu-id="41994-141">IsLocalUrl</span><span class="sxs-lookup"><span data-stu-id="41994-141">IsLocalUrl</span></span>

<span data-ttu-id="41994-142">Используйте [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) метод для проверки перед перенаправлением URL-адресов:</span><span class="sxs-lookup"><span data-stu-id="41994-142">Use the [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) method to test URLs before redirecting:</span></span>

<span data-ttu-id="41994-143">Приведенный ниже показано, как проверить, является ли URL-адрес локального перед перенаправлением.</span><span class="sxs-lookup"><span data-stu-id="41994-143">The following example shows how to check whether a URL is local before redirecting.</span></span>

```csharp
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

<span data-ttu-id="41994-144">`IsLocalUrl` Метод помогает защитить пользователей от случайно перенаправлением вредоносный сайт.</span><span class="sxs-lookup"><span data-stu-id="41994-144">The `IsLocalUrl` method protects users from being inadvertently redirected to a malicious site.</span></span> <span data-ttu-id="41994-145">Можно записывать в журнал сведения о URL-адрес, предоставленный при внешней URL-адрес указан в ситуации, где ожидается локальный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="41994-145">You can log the details of the URL that was provided when a non-local URL is supplied in a situation where you expected a local URL.</span></span> <span data-ttu-id="41994-146">Ведение журнала перенаправления URL-адреса может помочь при диагностике атак путем перенаправления.</span><span class="sxs-lookup"><span data-stu-id="41994-146">Logging redirect URLs may help in diagnosing redirection attacks.</span></span>
