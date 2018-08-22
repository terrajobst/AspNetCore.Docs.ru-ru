---
title: Предотвращение атак открытого перенаправления в ASP.NET Core
author: ardalis
description: Показано, как для предотвращения атак открытого перенаправления для приложения ASP.NET Core
ms.author: riande
ms.date: 07/07/2017
uid: security/preventing-open-redirects
ms.openlocfilehash: 0896189d2caaccb19647eb7c6d57f29dfc0290dd
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835568"
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a><span data-ttu-id="1f581-103">Предотвращение атак открытого перенаправления в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1f581-103">Prevent open redirect attacks in ASP.NET Core</span></span>

<span data-ttu-id="1f581-104">Веб-приложение, которое перенаправляет на URL-адрес, который задается с помощью запроса, такой как данные строки запроса или формы потенциально могут быть подделаны для перенаправления пользователей на внешний, вредоносный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="1f581-104">A web app that redirects to a URL that's specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="1f581-105">Это изменение данных называется атака открытое перенаправление.</span><span class="sxs-lookup"><span data-stu-id="1f581-105">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="1f581-106">Каждый раз, когда указанный URL-адрес перенаправляет логики приложения, необходимо убедиться, что URL-адрес перенаправления не было изменено.</span><span class="sxs-lookup"><span data-stu-id="1f581-106">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="1f581-107">ASP.NET Core содержит встроенные функциональные возможности, помогающие защитить приложения от открытой переадресацией (также известный как открытое перенаправление).</span><span class="sxs-lookup"><span data-stu-id="1f581-107">ASP.NET Core has built-in functionality to help protect apps from open redirect (also known as open redirection) attacks.</span></span>

## <a name="what-is-an-open-redirect-attack"></a><span data-ttu-id="1f581-108">Что такое атака открытого перенаправления</span><span class="sxs-lookup"><span data-stu-id="1f581-108">What is an open redirect attack?</span></span>

<span data-ttu-id="1f581-109">Веб-приложений часто перенаправлять пользователей на страницу входа при доступе к ресурсам, требующим проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="1f581-109">Web applications frequently redirect users to a login page when they access resources that require authentication.</span></span> <span data-ttu-id="1f581-110">Перенаправления обычно включает `returnUrl` параметр строки запроса, чтобы после успешного входа пользователя могут быть возвращены на первоначально запрошенный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="1f581-110">The redirection typically includes a `returnUrl` querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span> <span data-ttu-id="1f581-111">После проверки подлинности пользователя, он будет перенаправлен на URL-адрес, который запрошен изначально.</span><span class="sxs-lookup"><span data-stu-id="1f581-111">After the user authenticates, they're redirected to the URL they had originally requested.</span></span>

<span data-ttu-id="1f581-112">Поскольку конечный URL-адрес указан в строке запроса запроса, пользователь-злоумышленник может незаконно изменить строку запроса.</span><span class="sxs-lookup"><span data-stu-id="1f581-112">Because the destination URL is specified in the querystring of the request, a malicious user could tamper with the querystring.</span></span> <span data-ttu-id="1f581-113">Искаженные querystring делает возможным сайта для перенаправления пользователя на внешних, предоставленных вредоносный сайт.</span><span class="sxs-lookup"><span data-stu-id="1f581-113">A tampered querystring could allow the site to redirect the user to an external, malicious site.</span></span> <span data-ttu-id="1f581-114">Этот прием называется атака откройте перенаправления (или перенаправления).</span><span class="sxs-lookup"><span data-stu-id="1f581-114">This technique is called an open redirect (or redirection) attack.</span></span>

### <a name="an-example-attack"></a><span data-ttu-id="1f581-115">Пример атаки</span><span class="sxs-lookup"><span data-stu-id="1f581-115">An example attack</span></span>

<span data-ttu-id="1f581-116">Пользователь-злоумышленник можно разрабатывать атаку, позволяет предоставлять пользователь-злоумышленник доступ к учетным данным пользователя или конфиденциальной информации.</span><span class="sxs-lookup"><span data-stu-id="1f581-116">A malicious user can develop an attack intended to allow the malicious user access to a user's credentials or sensitive information.</span></span> <span data-ttu-id="1f581-117">Чтобы начать атаку, пользователь-злоумышленник побуждает пользователя щелкнуть ссылку на страницу входа веб-узла с `returnUrl` значение строки запроса, добавляемое к URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="1f581-117">To begin the attack, the malicious user convinces the user to click a link to your site's login page with a `returnUrl` querystring value added to the URL.</span></span> <span data-ttu-id="1f581-118">Например, рассмотрим приложение, в `contoso.com` , включает страницу входа в `http://contoso.com/Account/LogOn?returnUrl=/Home/About`.</span><span class="sxs-lookup"><span data-stu-id="1f581-118">For example, consider an app at `contoso.com` that includes a login page at `http://contoso.com/Account/LogOn?returnUrl=/Home/About`.</span></span> <span data-ttu-id="1f581-119">Атака включает следующие шаги.</span><span class="sxs-lookup"><span data-stu-id="1f581-119">The attack follows these steps:</span></span>

1. <span data-ttu-id="1f581-120">Пользователь нажмет на `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` (второй URL-адрес является «contoso**1**.com», не «contoso.com»).</span><span class="sxs-lookup"><span data-stu-id="1f581-120">The user clicks a malicious link to `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` (the second URL is "contoso**1**.com", not "contoso.com").</span></span>
2. <span data-ttu-id="1f581-121">Пользователь успешно вошел.</span><span class="sxs-lookup"><span data-stu-id="1f581-121">The user logs in successfully.</span></span>
3. <span data-ttu-id="1f581-122">Пользователь перенаправляется (узлом) `http://contoso1.com/Account/LogOn` (создать узел, выглядит в точности как настоящий узел).</span><span class="sxs-lookup"><span data-stu-id="1f581-122">The user is redirected (by the site) to `http://contoso1.com/Account/LogOn` (a malicious site that looks exactly like real site).</span></span>
4. <span data-ttu-id="1f581-123">Попытку входа пользователя (предоставляя вредоносных сайтов свои учетные данные) и перенаправляется в подлинному узлу.</span><span class="sxs-lookup"><span data-stu-id="1f581-123">The user logs in again (giving malicious site their credentials) and is redirected back to the real site.</span></span>

<span data-ttu-id="1f581-124">Пользователь, скорее всего полагает, что не удалось выполнить их первой попытки входа и что их вторая попытка окажется успешной.</span><span class="sxs-lookup"><span data-stu-id="1f581-124">The user likely believes that their first attempt to log in failed and that their second attempt is successful.</span></span> <span data-ttu-id="1f581-125">Скорее, остается неосведомленной, компрометация учетных данных пользователя.</span><span class="sxs-lookup"><span data-stu-id="1f581-125">The user most likely remains unaware that their credentials are compromised.</span></span>

![Открытое перенаправление атак процесса](preventing-open-redirects/_static/open-redirection-attack-process.png)

<span data-ttu-id="1f581-127">Помимо страницы для входа некоторые сайты предоставляют перенаправление страницы или конечных точек.</span><span class="sxs-lookup"><span data-stu-id="1f581-127">In addition to login pages, some sites provide redirect pages or endpoints.</span></span> <span data-ttu-id="1f581-128">Представьте себе приложение имеет страницу с помощью открытого перенаправления, `/Home/Redirect`.</span><span class="sxs-lookup"><span data-stu-id="1f581-128">Imagine your app has a page with an open redirect, `/Home/Redirect`.</span></span> <span data-ttu-id="1f581-129">Злоумышленник может создать, например, ссылку в сообщение электронной почты, которая ведет на `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`.</span><span class="sxs-lookup"><span data-stu-id="1f581-129">An attacker could create, for example, a link in an email that goes to `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`.</span></span> <span data-ttu-id="1f581-130">Обычного пользователя будет проверять URL-адрес и увидеть, что он начинается с имени веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="1f581-130">A typical user will look at the URL and see it begins with your site name.</span></span> <span data-ttu-id="1f581-131">Предоставление доверия, будет по ссылке.</span><span class="sxs-lookup"><span data-stu-id="1f581-131">Trusting that, they will click the link.</span></span> <span data-ttu-id="1f581-132">Открытого перенаправления затем следует отправить пользователю сайта фишинга, который выглядит так же к вам, и пользователь, скорее является имя входа для уверенности в веб-узла.</span><span class="sxs-lookup"><span data-stu-id="1f581-132">The open redirect would then send the user to the phishing site, which looks identical to yours, and the user would likely login to what they believe is your site.</span></span>

## <a name="protecting-against-open-redirect-attacks"></a><span data-ttu-id="1f581-133">Защита от открытой переадресацией</span><span class="sxs-lookup"><span data-stu-id="1f581-133">Protecting against open redirect attacks</span></span>

<span data-ttu-id="1f581-134">При разработке веб-приложений, следует считайте все предоставленные пользователем данные не заслуживающих доверия.</span><span class="sxs-lookup"><span data-stu-id="1f581-134">When developing web applications, treat all user-provided data as untrustworthy.</span></span> <span data-ttu-id="1f581-135">Если приложение имеет функциональность, которая перенаправляет пользователя на основе содержимого URL-адреса, убедитесь, что такое перенаправление осуществляется только локально в приложении (или на известные URL-адрес, не любой URL-адрес, который может быть введен в строке запроса).</span><span class="sxs-lookup"><span data-stu-id="1f581-135">If your application has functionality that redirects the user based on the contents of the URL,  ensure that such redirects are only done locally within your app (or to a known URL, not any URL that may be supplied in the querystring).</span></span>

### <a name="localredirect"></a><span data-ttu-id="1f581-136">LocalRedirect</span><span class="sxs-lookup"><span data-stu-id="1f581-136">LocalRedirect</span></span>

<span data-ttu-id="1f581-137">Используйте `LocalRedirect` вспомогательный метод из базового `Controller` класса:</span><span class="sxs-lookup"><span data-stu-id="1f581-137">Use the `LocalRedirect` helper method from the base `Controller` class:</span></span>

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

<span data-ttu-id="1f581-138">`LocalRedirect` Если указан URL-адрес, не являющейся локальной для вызовет исключение.</span><span class="sxs-lookup"><span data-stu-id="1f581-138">`LocalRedirect` will throw an exception if a non-local URL is specified.</span></span> <span data-ttu-id="1f581-139">В противном случае он ведет себя так же, как `Redirect` метод.</span><span class="sxs-lookup"><span data-stu-id="1f581-139">Otherwise, it behaves just like the `Redirect` method.</span></span>

### <a name="islocalurl"></a><span data-ttu-id="1f581-140">IsLocalUrl</span><span class="sxs-lookup"><span data-stu-id="1f581-140">IsLocalUrl</span></span>

<span data-ttu-id="1f581-141">Используйте [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) метод для проверки перед перенаправлением URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="1f581-141">Use the [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) method to test URLs before redirecting:</span></span>

<span data-ttu-id="1f581-142">Приведенный ниже показано, как проверить, является ли URL-адрес локальным перед перенаправлением.</span><span class="sxs-lookup"><span data-stu-id="1f581-142">The following example shows how to check whether a URL is local before redirecting.</span></span>

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

<span data-ttu-id="1f581-143">`IsLocalUrl` Метод защищает пользователей от случайно перенаправляемой на вредоносный сайт.</span><span class="sxs-lookup"><span data-stu-id="1f581-143">The `IsLocalUrl` method protects users from being inadvertently redirected to a malicious site.</span></span> <span data-ttu-id="1f581-144">Можно записать подробности URL-адрес, предоставленный при URL-адрес, не являющейся локальной в ситуации, где ожидается локальный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="1f581-144">You can log the details of the URL that was provided when a non-local URL is supplied in a situation where you expected a local URL.</span></span> <span data-ttu-id="1f581-145">Ведение журнала перенаправления URL-адреса может помочь в диагностике атак путем перенаправления.</span><span class="sxs-lookup"><span data-stu-id="1f581-145">Logging redirect URLs may help in diagnosing redirection attacks.</span></span>
