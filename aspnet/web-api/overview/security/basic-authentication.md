---
uid: web-api/overview/security/basic-authentication
title: Обычная проверка подлинности в веб-API ASP.NET | Документация Майкрософт
author: MikeWasson
description: Описывает использование обычной проверки подлинности в веб-API ASP.NET.
ms.author: aspnetcontent
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 035baec7c56c0bf6eaacd26ea5192faf2ed6e932
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829591"
---
<a name="basic-authentication-in-aspnet-web-api"></a><span data-ttu-id="f023a-103">Обычная проверка подлинности в веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f023a-103">Basic Authentication in ASP.NET Web API</span></span>
====================
<span data-ttu-id="f023a-104">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f023a-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f023a-105">Обычная проверка подлинности определена в [RFC 2617, проверка подлинности HTTP: Basic and Digest Authentication доступа](http://www.ietf.org/rfc/rfc2617.txt).</span><span class="sxs-lookup"><span data-stu-id="f023a-105">Basic authentication is defined in [RFC 2617, HTTP Authentication: Basic and Digest Access Authentication](http://www.ietf.org/rfc/rfc2617.txt).</span></span>

<span data-ttu-id="f023a-106">Недостатки</span><span class="sxs-lookup"><span data-stu-id="f023a-106">Disadvantages</span></span>

- <span data-ttu-id="f023a-107">Учетные данные отправляются в запросе.</span><span class="sxs-lookup"><span data-stu-id="f023a-107">User credentials are sent in the request.</span></span>
- <span data-ttu-id="f023a-108">Учетные данные отправляются в виде открытого текста.</span><span class="sxs-lookup"><span data-stu-id="f023a-108">Credentials are sent as plaintext.</span></span>
- <span data-ttu-id="f023a-109">Учетные данные отправляются с каждым запросом.</span><span class="sxs-lookup"><span data-stu-id="f023a-109">Credentials are sent with every request.</span></span>
- <span data-ttu-id="f023a-110">Невозможно выйти, за исключением путем завершения сеанса браузера.</span><span class="sxs-lookup"><span data-stu-id="f023a-110">No way to log out, except by ending the browser session.</span></span>
- <span data-ttu-id="f023a-111">Подвержена межсайтовых подделки запросов (CSRF); требуется anti-CSRF меры.</span><span class="sxs-lookup"><span data-stu-id="f023a-111">Vulnerable to cross-site request forgery (CSRF); requires anti-CSRF measures.</span></span>

<span data-ttu-id="f023a-112">Преимущества</span><span class="sxs-lookup"><span data-stu-id="f023a-112">Advantages</span></span>

- <span data-ttu-id="f023a-113">Стандарты Интернета.</span><span class="sxs-lookup"><span data-stu-id="f023a-113">Internet standard.</span></span>
- <span data-ttu-id="f023a-114">Поддерживается всеми основными браузерами.</span><span class="sxs-lookup"><span data-stu-id="f023a-114">Supported by all major browsers.</span></span>
- <span data-ttu-id="f023a-115">Довольно простой протокол.</span><span class="sxs-lookup"><span data-stu-id="f023a-115">Relatively simple protocol.</span></span>

<span data-ttu-id="f023a-116">Обычная проверка подлинности работает следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f023a-116">Basic authentication works as follows:</span></span>

1. <span data-ttu-id="f023a-117">Если запрос требует проверки подлинности, сервер возвращает 401 (не санкционировано).</span><span class="sxs-lookup"><span data-stu-id="f023a-117">If a request requires authentication, the server returns 401 (Unauthorized).</span></span> <span data-ttu-id="f023a-118">Ответ включает заголовок WWW-Authenticate, указывающий, что сервер поддерживает обычную проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="f023a-118">The response includes a WWW-Authenticate header, indicating the server supports Basic authentication.</span></span>
2. <span data-ttu-id="f023a-119">Клиент отправляет другой запрос, с помощью учетных данных клиента в заголовке авторизации.</span><span class="sxs-lookup"><span data-stu-id="f023a-119">The client sends another request, with the client credentials in the Authorization header.</span></span> <span data-ttu-id="f023a-120">Учетные данные форматируются в виде строки «имя: password», с кодировкой base64.</span><span class="sxs-lookup"><span data-stu-id="f023a-120">The credentials are formatted as the string "name:password", base64-encoded.</span></span> <span data-ttu-id="f023a-121">Учетные данные не шифруются.</span><span class="sxs-lookup"><span data-stu-id="f023a-121">The credentials are not encrypted.</span></span>

<span data-ttu-id="f023a-122">Обычная проверка подлинности выполняется в контексте «область».</span><span class="sxs-lookup"><span data-stu-id="f023a-122">Basic authentication is performed within the context of a "realm."</span></span> <span data-ttu-id="f023a-123">Сервер включает имя области в заголовке WWW-Authenticate.</span><span class="sxs-lookup"><span data-stu-id="f023a-123">The server includes the name of the realm in the WWW-Authenticate header.</span></span> <span data-ttu-id="f023a-124">Учетные данные пользователя допустимы в пределах этой области.</span><span class="sxs-lookup"><span data-stu-id="f023a-124">The user's credentials are valid within that realm.</span></span> <span data-ttu-id="f023a-125">Точный объем область определяется сервером.</span><span class="sxs-lookup"><span data-stu-id="f023a-125">The exact scope of a realm is defined by the server.</span></span> <span data-ttu-id="f023a-126">Например можно определить несколько областей в порядке к ресурсам секции.</span><span class="sxs-lookup"><span data-stu-id="f023a-126">For example, you might define several realms in order to partition resources.</span></span>

![](basic-authentication/_static/image1.png)

<span data-ttu-id="f023a-127">Так как учетные данные отсылаются нешифрованными, обычная проверка подлинности защищен только по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f023a-127">Because the credentials are sent unencrypted, Basic authentication is only secure over HTTPS.</span></span> <span data-ttu-id="f023a-128">См. в разделе [работа с SSL в веб-API](working-with-ssl-in-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="f023a-128">See [Working with SSL in Web API](working-with-ssl-in-web-api.md).</span></span>

<span data-ttu-id="f023a-129">Обычная проверка подлинности также является уязвимым к CSRF-атакам.</span><span class="sxs-lookup"><span data-stu-id="f023a-129">Basic authentication is also vulnerable to CSRF attacks.</span></span> <span data-ttu-id="f023a-130">После того как пользователь введет учетные данные, браузер автоматически отправляет их на последующие запросы к тому же домену, в течение сеанса.</span><span class="sxs-lookup"><span data-stu-id="f023a-130">After the user enters credentials, the browser automatically sends them on subsequent requests to the same domain, for the duration of the session.</span></span> <span data-ttu-id="f023a-131">Сюда входят запросы AJAX.</span><span class="sxs-lookup"><span data-stu-id="f023a-131">This includes AJAX requests.</span></span> <span data-ttu-id="f023a-132">См. в разделе [атак с подделкой межсайтовых запросов](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="f023a-132">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

## <a name="basic-authentication-with-iis"></a><span data-ttu-id="f023a-133">Проверка подлинности IIS</span><span class="sxs-lookup"><span data-stu-id="f023a-133">Basic Authentication with IIS</span></span>

<span data-ttu-id="f023a-134">IIS поддерживает обычную проверку подлинности, но Будьте осторожны: пользователь прошел проверку учетных данных Windows.</span><span class="sxs-lookup"><span data-stu-id="f023a-134">IIS supports Basic authentication, but there is a caveat: The user is authenticated against their Windows credentials.</span></span> <span data-ttu-id="f023a-135">Это означает, что пользователь должен иметь учетную запись в домене сервера.</span><span class="sxs-lookup"><span data-stu-id="f023a-135">That means the user must have an account on the server's domain.</span></span> <span data-ttu-id="f023a-136">Общедоступный веб-сайта обычно требуется для проверки подлинности поставщик членства ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f023a-136">For a public-facing web site, you typically want to authenticate against an ASP.NET membership provider.</span></span>

<span data-ttu-id="f023a-137">Чтобы включить обычную проверку подлинности с помощью служб IIS, задайте режим проверки подлинности «Windows» в файле Web.config проекта ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="f023a-137">To enable Basic authentication using IIS, set the authentication mode to "Windows" in the Web.config of your ASP.NET project:</span></span>

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

<span data-ttu-id="f023a-138">В этом режиме IIS использует учетные данные Windows для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="f023a-138">In this mode, IIS uses Windows credentials to authenticate.</span></span> <span data-ttu-id="f023a-139">Кроме того необходимо включить обычную проверку подлинности в службах IIS.</span><span class="sxs-lookup"><span data-stu-id="f023a-139">In addition, you must enable Basic authentication in IIS.</span></span> <span data-ttu-id="f023a-140">В диспетчере служб IIS, перейдите к представлению функций, выберите проверку подлинности и включить обычную проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="f023a-140">In IIS Manager, go to Features View, select Authentication, and enable Basic authentication.</span></span>

![](basic-authentication/_static/image2.png)

<span data-ttu-id="f023a-141">В проекте веб-API добавьте `[Authorize]` атрибут для любого действия контроллера, требующих проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="f023a-141">In your Web API project, add the `[Authorize]` attribute for any controller actions that need authentication.</span></span>

<span data-ttu-id="f023a-142">Клиент проходит проверку подлинности, задав для заголовка авторизации в запросе.</span><span class="sxs-lookup"><span data-stu-id="f023a-142">A client authenticates itself by setting the Authorization header in the request.</span></span> <span data-ttu-id="f023a-143">Клиенты браузера выполнить этот шаг автоматически.</span><span class="sxs-lookup"><span data-stu-id="f023a-143">Browser clients perform this step automatically.</span></span> <span data-ttu-id="f023a-144">Клиенты nonbrowser потребуется задать заголовок.</span><span class="sxs-lookup"><span data-stu-id="f023a-144">Nonbrowser clients will need to set the header.</span></span>

## <a name="basic-authentication-with-custom-membership"></a><span data-ttu-id="f023a-145">Обычная проверка подлинности с помощью настраиваемого членства</span><span class="sxs-lookup"><span data-stu-id="f023a-145">Basic Authentication with Custom Membership</span></span>

<span data-ttu-id="f023a-146">Как уже упоминалось, обычная проверка подлинности, встроенные в IIS использует учетные данные Windows.</span><span class="sxs-lookup"><span data-stu-id="f023a-146">As mentioned, the Basic Authentication built into IIS uses Windows credentials.</span></span> <span data-ttu-id="f023a-147">Это означает, что необходимо для создания пользовательских учетных записей на сервере размещения.</span><span class="sxs-lookup"><span data-stu-id="f023a-147">That means you need to create accounts for your users on the hosting server.</span></span> <span data-ttu-id="f023a-148">Но для Интернет-приложения, учетные записи пользователей обычно хранятся во внешней базе данных.</span><span class="sxs-lookup"><span data-stu-id="f023a-148">But for an internet application, user accounts are typically stored in an external database.</span></span>

<span data-ttu-id="f023a-149">В следующем примере кода, как модуль HTTP, который выполняет обычную проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="f023a-149">The following code how an HTTP module that performs Basic Authentication.</span></span> <span data-ttu-id="f023a-150">Вы можете легко подключить поставщик членства ASP.NET, заменив `CheckPassword` метод, который является пустой метод в этом примере.</span><span class="sxs-lookup"><span data-stu-id="f023a-150">You can easily plug in an ASP.NET membership provider by replacing the `CheckPassword` method, which is a dummy method in this example.</span></span>

<span data-ttu-id="f023a-151">В веб-API 2, следует рассмотреть записи [фильтр проверки подлинности](authentication-filters.md) или [по промежуточного слоя OWIN](../../../aspnet/overview/owin-and-katana/index.md), а не HTTP-модуль.</span><span class="sxs-lookup"><span data-stu-id="f023a-151">In Web API 2, you should consider writing an [authentication filter](authentication-filters.md) or [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), instead of an HTTP module.</span></span>

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

<span data-ttu-id="f023a-152">Чтобы включить HTTP-модуль, добавьте следующее в файл web.config в **system.webServer** раздел:</span><span class="sxs-lookup"><span data-stu-id="f023a-152">To enable the HTTP module, add the following to your web.config file in the **system.webServer** section:</span></span>

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

<span data-ttu-id="f023a-153">Замените на имя сборки (не включая расширение «dll») «Имя_сборки».</span><span class="sxs-lookup"><span data-stu-id="f023a-153">Replace "YourAssemblyName" with the name of the assembly (not including the "dll" extension).</span></span>

<span data-ttu-id="f023a-154">Следует отключить другие схемы проверки подлинности, например проверки подлинности Forms или Windows</span><span class="sxs-lookup"><span data-stu-id="f023a-154">You should disable other authentication schemes, such as Forms or Windows auth.</span></span>
