---
title: Предотвращение атак открытого перенаправления в ASP.NET Core
author: ardalis
description: Показывает, как предотвратить атаки открытого перенаправления в приложении ASP.NET Core
ms.author: riande
ms.date: 07/07/2017
uid: security/preventing-open-redirects
ms.openlocfilehash: 9d8cac8708fe9aeadba5af1287362a20df7f6bfe
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815491"
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a><span data-ttu-id="a21fa-103">Предотвращение атак открытого перенаправления в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a21fa-103">Prevent open redirect attacks in ASP.NET Core</span></span>

<span data-ttu-id="a21fa-104">Если веб-приложение перенаправляет пользователя на URL-адрес, указанный по запросу, например в строке запроса или через данные формы, этот URL-адрес может быть подделан и вести на внешний вредоносный сайт.</span><span class="sxs-lookup"><span data-stu-id="a21fa-104">A web app that redirects to a URL that's specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="a21fa-105">Такое изменение данных называется атакой открытого перенаправления.</span><span class="sxs-lookup"><span data-stu-id="a21fa-105">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="a21fa-106">Каждый раз, когда логика вашего приложения выполняет перенаправление на определенный URL-адрес, нужно убедиться, что этот адрес не был изменен.</span><span class="sxs-lookup"><span data-stu-id="a21fa-106">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="a21fa-107">ASP.NET Core содержит встроенные функциональные возможности, помогающие защитить приложения от атак методом открытого перенаправления/переадресации.</span><span class="sxs-lookup"><span data-stu-id="a21fa-107">ASP.NET Core has built-in functionality to help protect apps from open redirect (also known as open redirection) attacks.</span></span>

## <a name="what-is-an-open-redirect-attack"></a><span data-ttu-id="a21fa-108">Что такое атака открытого перенаправления</span><span class="sxs-lookup"><span data-stu-id="a21fa-108">What is an open redirect attack?</span></span>

<span data-ttu-id="a21fa-109">Веб-приложения часто перенаправляют пользователей на страницу входа при доступе к ресурсам, требующим проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="a21fa-109">Web applications frequently redirect users to a login page when they access resources that require authentication.</span></span> <span data-ttu-id="a21fa-110">Перенаправление обычно включает параметр строки запроса `returnUrl`, чтобы после успешной проверки подлинности пользователь был перенаправлен на первоначально запрошенный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="a21fa-110">The redirection typically includes a `returnUrl` querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span> <span data-ttu-id="a21fa-111">После проверки подлинности пользователя, он будет перенаправлен на URL-адрес, который запрошен изначально.</span><span class="sxs-lookup"><span data-stu-id="a21fa-111">After the user authenticates, they're redirected to the URL they had originally requested.</span></span>

<span data-ttu-id="a21fa-112">Поскольку конечный URL-адрес указан в строке запроса, злоумышленник может незаконно изменить ее.</span><span class="sxs-lookup"><span data-stu-id="a21fa-112">Because the destination URL is specified in the querystring of the request, a malicious user could tamper with the querystring.</span></span> <span data-ttu-id="a21fa-113">Измененный запрос позволит сайту перенаправить пользователя на внешний вредоносный сайт.</span><span class="sxs-lookup"><span data-stu-id="a21fa-113">A tampered querystring could allow the site to redirect the user to an external, malicious site.</span></span> <span data-ttu-id="a21fa-114">Этот прием называется атакой открытого перенаправления.</span><span class="sxs-lookup"><span data-stu-id="a21fa-114">This technique is called an open redirect (or redirection) attack.</span></span>

### <a name="an-example-attack"></a><span data-ttu-id="a21fa-115">Пример атаки</span><span class="sxs-lookup"><span data-stu-id="a21fa-115">An example attack</span></span>

<span data-ttu-id="a21fa-116">Злоумышленник может разработать атаку, которая позволит ему получить доступ к учетным или конфиденциальным данным пользователя.</span><span class="sxs-lookup"><span data-stu-id="a21fa-116">A malicious user can develop an attack intended to allow the malicious user access to a user's credentials or sensitive information.</span></span> <span data-ttu-id="a21fa-117">Чтобы начать атаку, злоумышленник убеждает пользователя щелкнуть ссылку на страницу авторизации вашего сайта, добавляя в URL-адрес значение строки запроса `returnUrl`.</span><span class="sxs-lookup"><span data-stu-id="a21fa-117">To begin the attack, the malicious user convinces the user to click a link to your site's login page with a `returnUrl` querystring value added to the URL.</span></span> <span data-ttu-id="a21fa-118">Например, рассмотрим приложение на сайте `contoso.com` со страницей входа `http://contoso.com/Account/LogOn?returnUrl=/Home/About`.</span><span class="sxs-lookup"><span data-stu-id="a21fa-118">For example, consider an app at `contoso.com` that includes a login page at `http://contoso.com/Account/LogOn?returnUrl=/Home/About`.</span></span> <span data-ttu-id="a21fa-119">Атака включает следующие шаги:</span><span class="sxs-lookup"><span data-stu-id="a21fa-119">The attack follows these steps:</span></span>

1. <span data-ttu-id="a21fa-120">Пользователь нажмет на `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` (второй URL-адрес является «contoso**1**.com», не «contoso.com»).</span><span class="sxs-lookup"><span data-stu-id="a21fa-120">The user clicks a malicious link to `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` (the second URL is "contoso**1**.com", not "contoso.com").</span></span>
2. <span data-ttu-id="a21fa-121">Пользователь успешно вошел.</span><span class="sxs-lookup"><span data-stu-id="a21fa-121">The user logs in successfully.</span></span>
3. <span data-ttu-id="a21fa-122">Сайт перенаправляет пользователя на `http://contoso1.com/Account/LogOn` (вредоносный сайт, который выглядит как настоящий).</span><span class="sxs-lookup"><span data-stu-id="a21fa-122">The user is redirected (by the site) to `http://contoso1.com/Account/LogOn` (a malicious site that looks exactly like real site).</span></span>
4. <span data-ttu-id="a21fa-123">Пользователь пытается войти еще раз (предоставляя вредоносному сайту свои учетные данные) и перенаправляется на настоящий сайт.</span><span class="sxs-lookup"><span data-stu-id="a21fa-123">The user logs in again (giving malicious site their credentials) and is redirected back to the real site.</span></span>

<span data-ttu-id="a21fa-124">Пользователь скорее всего думает, что ему просто не удалось войти с первой попытки, и не подозревает,</span><span class="sxs-lookup"><span data-stu-id="a21fa-124">The user likely believes that their first attempt to log in failed and that their second attempt is successful.</span></span> <span data-ttu-id="a21fa-125">что его учетные данные скомпрометированы.</span><span class="sxs-lookup"><span data-stu-id="a21fa-125">The user most likely remains unaware that their credentials are compromised.</span></span>

![Процесс атаки открытого перенаправления](preventing-open-redirects/_static/open-redirection-attack-process.png)

<span data-ttu-id="a21fa-127">Помимо страниц для входа, некоторые сайты предоставляют страницы перенаправления или конечные точки.</span><span class="sxs-lookup"><span data-stu-id="a21fa-127">In addition to login pages, some sites provide redirect pages or endpoints.</span></span> <span data-ttu-id="a21fa-128">Предположим, у вашего приложения есть страница с открытым перенаправлением `/Home/Redirect`.</span><span class="sxs-lookup"><span data-stu-id="a21fa-128">Imagine your app has a page with an open redirect, `/Home/Redirect`.</span></span> <span data-ttu-id="a21fa-129">Злоумышленник может создать, например, ссылку в электронном письме, которая ведет на `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`.</span><span class="sxs-lookup"><span data-stu-id="a21fa-129">An attacker could create, for example, a link in an email that goes to `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`.</span></span> <span data-ttu-id="a21fa-130">Обычного пользователя будет проверять URL-адрес и увидеть, что он начинается с имени веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="a21fa-130">A typical user will look at the URL and see it begins with your site name.</span></span> <span data-ttu-id="a21fa-131">Увидев, что этот URL-адрес начинается с названия вашего веб-сайта, обычный пользователь, ни о чем не подозревая, щелкнет ссылку.</span><span class="sxs-lookup"><span data-stu-id="a21fa-131">Trusting that, they will click the link.</span></span> <span data-ttu-id="a21fa-132">Открытое перенаправление затем переведет его на фишинговый сайт, который выглядит в точности как ваш, и пользователь, скорее всего, введет на нем свои учетные данные.</span><span class="sxs-lookup"><span data-stu-id="a21fa-132">The open redirect would then send the user to the phishing site, which looks identical to yours, and the user would likely login to what they believe is your site.</span></span>

## <a name="protecting-against-open-redirect-attacks"></a><span data-ttu-id="a21fa-133">Защита от открытого перенаправления</span><span class="sxs-lookup"><span data-stu-id="a21fa-133">Protecting against open redirect attacks</span></span>

<span data-ttu-id="a21fa-134">При разработке веб-приложений следует считать все предоставляемые пользователями данные не заслуживающими доверия.</span><span class="sxs-lookup"><span data-stu-id="a21fa-134">When developing web applications, treat all user-provided data as untrustworthy.</span></span> <span data-ttu-id="a21fa-135">Если приложение содержит функции, перенаправляющие пользователя на основе содержимого URL-адреса, перенаправление должно осуществляться только локально внутри приложения (или строго на известный URL, а не на любой адрес в строке запроса).</span><span class="sxs-lookup"><span data-stu-id="a21fa-135">If your application has functionality that redirects the user based on the contents of the URL,  ensure that such redirects are only done locally within your app (or to a known URL, not any URL that may be supplied in the querystring).</span></span>

### <a name="localredirect"></a><span data-ttu-id="a21fa-136">LocalRedirect</span><span class="sxs-lookup"><span data-stu-id="a21fa-136">LocalRedirect</span></span>

<span data-ttu-id="a21fa-137">Используйте `LocalRedirect` вспомогательный метод из базового `Controller` класса:</span><span class="sxs-lookup"><span data-stu-id="a21fa-137">Use the `LocalRedirect` helper method from the base `Controller` class:</span></span>

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

<span data-ttu-id="a21fa-138">`LocalRedirect` вызовет исключение, если указан URL-адрес, не являющейся локальным.</span><span class="sxs-lookup"><span data-stu-id="a21fa-138">`LocalRedirect` will throw an exception if a non-local URL is specified.</span></span> <span data-ttu-id="a21fa-139">В противном случае он ведет себя так же, как метод `Redirect`.</span><span class="sxs-lookup"><span data-stu-id="a21fa-139">Otherwise, it behaves just like the `Redirect` method.</span></span>

### <a name="islocalurl"></a><span data-ttu-id="a21fa-140">IsLocalUrl</span><span class="sxs-lookup"><span data-stu-id="a21fa-140">IsLocalUrl</span></span>

<span data-ttu-id="a21fa-141">Используйте [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper.islocalurl#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) метод для проверки перед перенаправлением URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="a21fa-141">Use the [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper.islocalurl#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) method to test URLs before redirecting:</span></span>

<span data-ttu-id="a21fa-142">В следующем примере показано, как проверить, является ли URL-адрес локальным, перед перенаправлением.</span><span class="sxs-lookup"><span data-stu-id="a21fa-142">The following example shows how to check whether a URL is local before redirecting.</span></span>

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

<span data-ttu-id="a21fa-143">Метод `IsLocalUrl` защищает пользователей от случайного перенаправления на вредоносный сайт.</span><span class="sxs-lookup"><span data-stu-id="a21fa-143">The `IsLocalUrl` method protects users from being inadvertently redirected to a malicious site.</span></span> <span data-ttu-id="a21fa-144">Рекомендуем фиксировать сведения об URL-адресе в ситуациях, когда вместо ожидаемого локального URL предоставляется нелокальный.</span><span class="sxs-lookup"><span data-stu-id="a21fa-144">You can log the details of the URL that was provided when a non-local URL is supplied in a situation where you expected a local URL.</span></span> <span data-ttu-id="a21fa-145">Это поможет в диагностике использующих перенаправление атак.</span><span class="sxs-lookup"><span data-stu-id="a21fa-145">Logging redirect URLs may help in diagnosing redirection attacks.</span></span>
