---
title: "По промежуточного слоя в ASP.NET Core видоизменения URL-адресов"
author: guardrex
description: "Дополнительные сведения о URL-адрес, перезаписи и перенаправления с по промежуточного слоя перезаписи URL-адрес в приложениях ASP.NET Core."
keywords: "ASP.NET Core переопределения URL-адресов переписывать, URL-адрес, URL-адрес перенаправления URL-адрес перенаправления, по промежуточного слоя, apache_mod"
ms.author: riande
manager: wpickett
ms.date: 08/17/2017
ms.topic: article
ms.assetid: e6130638-c410-4161-9921-b658ce988bd1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/url-rewriting
ms.openlocfilehash: dde0b5673c9885db2fecbb24b384752e5ddf70eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="e1dc4-104">По промежуточного слоя в ASP.NET Core видоизменения URL-адресов</span><span class="sxs-lookup"><span data-stu-id="e1dc4-104">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="e1dc4-105">По [Latham Люк](https://github.com/guardrex) и [Mengistu Микаэль](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="e1dc4-105">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="e1dc4-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e1dc4-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e1dc4-107">Перезапись URL-адрес является изменение запроса URL-адреса на основании одного или нескольких стандартных правил.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-107">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="e1dc4-108">Переписывание URL-адресов создает абстракции между расположений ресурсов и их адресов, позволяя местоположения и адреса не связаны тесно.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-108">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="e1dc4-109">Существует несколько сценариев, где полезен перезаписи URL-адресов:</span><span class="sxs-lookup"><span data-stu-id="e1dc4-109">There are several scenarios where URL rewriting is valuable:</span></span>
* <span data-ttu-id="e1dc4-110">Перемещение или замещение ресурсы сервера временно или навсегда при сохранении стабильный указатели для этих ресурсов</span><span class="sxs-lookup"><span data-stu-id="e1dc4-110">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources</span></span>
* <span data-ttu-id="e1dc4-111">Разделение запросов обработки в различных приложениях или в области одного приложения</span><span class="sxs-lookup"><span data-stu-id="e1dc4-111">Splitting request processing across different apps or across areas of one app</span></span>
* <span data-ttu-id="e1dc4-112">Удаление, добавление или реорганизации сегменты URL-адреса входящих запросов</span><span class="sxs-lookup"><span data-stu-id="e1dc4-112">Removing, adding, or reorganizing URL segments on incoming requests</span></span>
* <span data-ttu-id="e1dc4-113">Оптимизация общедоступные URL-адреса для оптимизации поиска подсистемы (SEO)</span><span class="sxs-lookup"><span data-stu-id="e1dc4-113">Optimizing public URLs for Search Engine Optimization (SEO)</span></span>
* <span data-ttu-id="e1dc4-114">Разрешить использование общих поисковых помогает пользователям прогнозирования содержимое, которое они найдут по ссылке</span><span class="sxs-lookup"><span data-stu-id="e1dc4-114">Permitting the use of friendly public URLs to help people predict the content they will find by following a link</span></span>
* <span data-ttu-id="e1dc4-115">Перенаправление небезопасного запросов для защиты конечных точек</span><span class="sxs-lookup"><span data-stu-id="e1dc4-115">Redirecting insecure requests to secure endpoints</span></span>
* <span data-ttu-id="e1dc4-116">Предотвращение hotlinking изображения</span><span class="sxs-lookup"><span data-stu-id="e1dc4-116">Preventing image hotlinking</span></span>

<span data-ttu-id="e1dc4-117">Можно определить правила для изменения URL-адрес несколькими способами, включая регулярное выражение, модуль правил Apache mod_rewrite, IIS модуль переопределения правил и с помощью настраиваемого правила логики.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-117">You can define rules for changing the URL in several ways, including regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="e1dc4-118">В этом документе представлены перезаписи URL-адресов с инструкциями о способах использования по промежуточного слоя перезаписи URL-адрес в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-118">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="e1dc4-119">Переписывание URL-адресов может привести к снижению производительности приложения.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-119">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="e1dc4-120">Если возможно, следует ограничить количество и сложность правил.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-120">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="e1dc4-121">Перепишите перенаправления URL-адреса и URL-адрес</span><span class="sxs-lookup"><span data-stu-id="e1dc4-121">URL redirect and URL rewrite</span></span>
<span data-ttu-id="e1dc4-122">Разница в текст между *URL-адрес перенаправления* и *перезапись URL-адреса* может показаться слабая на первый, но содержится важные последствия для предоставления ресурсов клиентам.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-122">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="e1dc4-123">По промежуточного слоя ASP.NET Core URL-адрес перезаписи способен удовлетворяет потребность в обоих.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-123">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="e1dc4-124">Объект *URL-адрес перенаправления* является операции на стороне клиента, где указано клиента для доступа к ресурсу по другому адресу.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-124">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="e1dc4-125">Это требует дополнительного обращения к серверу, и URL-адрес перенаправления, возвращаемый клиенту появляется в адресной строке браузера, когда клиент отправляет новый запрос для ресурса.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-125">This requires a round-trip to the server, and the redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> <span data-ttu-id="e1dc4-126">Если `/resource` — *перенаправлено* для `/different-resource`, клиентские запросы `/resource`, и сервер отвечает, что клиент должен получить ресурс по адресу `/different-resource` код состояния, указывающую, что является перенаправления быть временным или постоянным.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`, and the server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="e1dc4-127">Клиент выполняет новый запрос для ресурса на URL-адрес перенаправления.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-127">The client executes a new request for the resource at the redirect URL.</span></span>

![Конечная точка службы WebAPI временно изменен с версии 1 (v1) до версии 2 (v2) на сервере.](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="e1dc4-133">Когда перенаправления запросов на другой URL-адрес, можно указать ли перенаправления постоянных и временных.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-133">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="e1dc4-134">301 (перемещен постоянно) код состояния позволяет где ресурс содержит URL-адрес нового, постоянный и хотите указать клиента, что все последующие запросы для ресурса следует использовать новый URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-134">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="e1dc4-135">*Клиент может кэшировать ответ при получении код 301 состояния.*</span><span class="sxs-lookup"><span data-stu-id="e1dc4-135">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="e1dc4-136">Код состояния 302 (найдено) используется где перенаправление временные или обычно субъекта для изменения таким образом, что клиент не должен хранить и повторно использовать URL-адрес перенаправления, в будущем.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-136">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="e1dc4-137">Дополнительные сведения см. в разделе [RFC 2616: определения кодов состояния](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="e1dc4-137">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="e1dc4-138">Объект *перезапись URL-адреса* является операции на стороне сервера для предоставления ресурса, с адресом другого ресурса.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-138">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="e1dc4-139">Перезапись URL-адрес не требует дополнительного обращения к серверу.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-139">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="e1dc4-140">URL-адрес перезаписанного не возвращаются клиенту и не будет отображаться в адресной строке браузера.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-140">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="e1dc4-141">При `/resource` — *переписать* для `/different-resource`, клиентские запросы `/resource`и сервер *внутренне* получает ресурс на `/different-resource`.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-141">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="e1dc4-142">Несмотря на то, что клиент может иметь возможность получить ресурс по перезаписанного URL-АДРЕСУ, клиент не быть в курсе, что ресурс существует перезаписанного URL-адрес, если она его запрашивает и получает ответ.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-142">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![Конечная точка службы WebAPI был изменен с версии 1 (v1) до версии 2 (v2) на сервере.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="e1dc4-147">Пример приложения URL-адрес для перезаписи</span><span class="sxs-lookup"><span data-stu-id="e1dc4-147">URL rewriting sample app</span></span>
<span data-ttu-id="e1dc4-148">Можно ознакомиться с функциями URL-адрес перезаписи по промежуточного слоя с [пример приложения URL-адрес для перезаписи](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span><span class="sxs-lookup"><span data-stu-id="e1dc4-148">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span></span> <span data-ttu-id="e1dc4-149">Перепишите применяется приложение и перенаправления правила и показывает перезаписанного или перенаправленный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-149">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="e1dc4-150">Когда следует использовать URL-адрес перезаписи по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="e1dc4-150">When to use URL Rewriting Middleware</span></span>
<span data-ttu-id="e1dc4-151">По промежуточного слоя перезаписи URL-адрес используется, если не удается использовать [модуль переопределения URL-адрес](https://www.iis.net/downloads/microsoft/url-rewrite) со службами IIS в Windows Server [модуля Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/) на сервере Apache [перезаписи URL-адресов на Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), или приложения, размещенного на [HTTP.sys сервера](xref:fundamentals/servers/httpsys) (ранее называвшиеся [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="e1dc4-151">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="e1dc4-152">Основных причин для использования серверных URL-адрес перезаписи технологии в IIS, Apache или Nginx, что по промежуточного слоя не поддерживает все возможности этих модулей и производительности по промежуточного слоя, вероятно, могут не соответствовать, модулей.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-152">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="e1dc4-153">Однако существуют некоторые возможности серверных модулей, которые не работают с проекты ASP.NET Core, таких как `IsFile` и `IsDirectory` ограничения модуль переопределения IIS.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-153">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="e1dc4-154">В этих сценариях вместо этого используйте по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-154">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="e1dc4-155">Пакет</span><span class="sxs-lookup"><span data-stu-id="e1dc4-155">Package</span></span>
<span data-ttu-id="e1dc4-156">Чтобы включить по промежуточного слоя в проекте, добавьте ссылку на [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) пакета.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-156">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="e1dc4-157">Эта функция предназначена для приложений, предназначенных для ASP.NET Core 1.1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-157">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="e1dc4-158">Расширение и параметры</span><span class="sxs-lookup"><span data-stu-id="e1dc4-158">Extension and options</span></span>
<span data-ttu-id="e1dc4-159">Установить на перезапись URL-адреса и перенаправить правила, создав экземпляр `RewriteOptions` класса с помощью методов расширения для каждого правила.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-159">Establish your URL rewrite and redirect rules by creating an instance of the `RewriteOptions` class with extension methods for each of your rules.</span></span> <span data-ttu-id="e1dc4-160">Привязать несколько правил в порядке, что вы хотите их обработать.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-160">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="e1dc4-161">`RewriteOptions` Передаются в по промежуточного слоя перезаписи URL-адрес, как оно добавляется в конвейер обработки запросов с `app.UseRewriter(options);`.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-161">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e1dc4-162">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e1dc4-162">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e1dc4-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e1dc4-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1)]

---

### <a name="url-redirect"></a><span data-ttu-id="e1dc4-164">URL-адрес перенаправления</span><span class="sxs-lookup"><span data-stu-id="e1dc4-164">URL redirect</span></span>
<span data-ttu-id="e1dc4-165">Используйте `AddRedirect` для перенаправления запросов.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-165">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="e1dc4-166">Первый параметр содержит регулярное выражение для сопоставления по пути входящего URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-166">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="e1dc4-167">Второй параметр является строка замены.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-167">The second parameter is the replacement string.</span></span> <span data-ttu-id="e1dc4-168">Третий параметр, если он присутствует, указывает код состояния.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-168">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="e1dc4-169">Если не указать код состояния, по умолчанию 302 (найдено), который указывает, что ресурс временно переместить или заменить.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-169">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e1dc4-170">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e1dc4-170">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e1dc4-171">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e1dc4-171">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=2)]

---

<span data-ttu-id="e1dc4-172">В обозревателе со средствами разработчика включена, сделать запрос в пример приложения с путем `/redirect-rule/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-172">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="e1dc4-173">Регулярное выражение соответствует пути запроса на `redirect-rule/(.*)`, и путь должен быть заменен параметром `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-173">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="e1dc4-174">Перенаправление URL-адрес отправляется обратно клиенту с кодом состояния 302 (найдено).</span><span class="sxs-lookup"><span data-stu-id="e1dc4-174">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="e1dc4-175">Браузер отправляет новый запрос на URL-адрес перенаправления, который отображается в адресной строке браузера.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-175">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="e1dc4-176">Поскольку соответствие не найдено в пример приложения на URL-адрес перенаправления, второй запрос получает ответ 200 (ОК) из приложения и текст ответа показан URL-адрес перенаправления.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-176">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="e1dc4-177">Обмен данными сделаны на сервере, когда требуется URL-адрес *перенаправлено*.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-177">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="e1dc4-178">Будьте осторожны при установке правила перенаправления.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-178">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="e1dc4-179">Правила перенаправления вычисляются при каждом запросе данного приложения, включая после перенаправления.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-179">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="e1dc4-180">Можно легко случайно создать перенаправлений бесконечный цикл.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-180">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="e1dc4-181">Исходный запрос:`/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="e1dc4-181">Original Request: `/redirect-rule/1234/5678`</span></span>

![Окно браузера со средствами разработчика отслеживания запросов и ответов](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="e1dc4-183">Часть выражения в скобках называется *группа захвата*.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-183">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="e1dc4-184">Точка (`.`) выражения означает *соответствует любому символу*.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-184">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="e1dc4-185">Звездочка (`*`) указывает *соответствует предыдущему символу ноль или более раз*.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-185">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="e1dc4-186">Таким образом, два последних сегментов URL-адреса, `1234/5678`, захваченные по группа захвата `(.*)`.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-186">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="e1dc4-187">Любое значение, указываемые в URL-АДРЕСЕ запроса после `redirect-rule/` охваченного этой группы один символ.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-187">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="e1dc4-188">Захватываемые группы в строке замены, вводится в строку с знак доллара (`$`) следуют порядковый номер записи.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-188">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="e1dc4-189">Первое значение группы захвата получен с `$1`, другой — с `$2`, и они по-прежнему в последовательности для групп захвата в регулярное выражение.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-189">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="e1dc4-190">Установлен только один захватываемой группы в регулярное выражение правила перенаправления в пример приложения, то есть только одна группа, введенный в строке замены, который является `$1`.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-190">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="e1dc4-191">Если применяется правило, URL-адрес становится `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-191">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

<a name="url-redirect-to-secure-endpoint"></a>
### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="e1dc4-192">URL-адрес перенаправления для защищенной конечной точки</span><span class="sxs-lookup"><span data-stu-id="e1dc4-192">URL redirect to a secure endpoint</span></span>
<span data-ttu-id="e1dc4-193">Используйте `AddRedirectToHttps` для перенаправления HTTP-запросов на один и тот же узел и путь, с помощью протокола HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="e1dc4-193">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="e1dc4-194">Если код состояния не указано, по промежуточного слоя по умолчанию 302 (найдено).</span><span class="sxs-lookup"><span data-stu-id="e1dc4-194">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="e1dc4-195">Если порт не указан, по умолчанию используется по промежуточного слоя `null`, означающее протокол примет `https://` и клиент обращается к ресурсу через порт 443.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-195">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="e1dc4-196">В примере показано установить код состояния для 301 (перемещен постоянно) и изменить номер порта 5001.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-196">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>
```csharp
var options = new RewriteOptions()
    .AddRedirectToHttps(301, 5001);

app.UseRewriter(options);
```
<span data-ttu-id="e1dc4-197">Используйте `AddRedirectToHttpsPermanent` для перенаправления небезопасных запросов на один и тот же узел и путь с безопасным протоколом HTTPS (`https://` через порт 443).</span><span class="sxs-lookup"><span data-stu-id="e1dc4-197">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="e1dc4-198">По промежуточного слоя задает код состояния для 301 (перемещен постоянно).</span><span class="sxs-lookup"><span data-stu-id="e1dc4-198">The middleware sets the status code to 301 (Moved Permanently).</span></span>

<span data-ttu-id="e1dc4-199">Пример приложения способен демонстрирующие использование `AddRedirectToHttps` или `AddRedirectToHttpsPermanent`.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-199">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="e1dc4-200">Добавьте метод расширения `RewriteOptions`.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-200">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="e1dc4-201">Выполните небезопасных запрос к приложению в любой URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-201">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="e1dc4-202">Закройте предупреждение, что самозаверяющий сертификат не является доверенным безопасности браузера.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-202">Dismiss the browser security warning that the self-signed certificate is untrusted.</span></span>

<span data-ttu-id="e1dc4-203">С помощью исходного запроса `AddRedirectToHttps(301, 5001)`:`/secure`</span><span class="sxs-lookup"><span data-stu-id="e1dc4-203">Original Request using `AddRedirectToHttps(301, 5001)`: `/secure`</span></span>

![Окно браузера со средствами разработчика отслеживания запросов и ответов](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="e1dc4-205">С помощью исходного запроса `AddRedirectToHttpsPermanent`:`/secure`</span><span class="sxs-lookup"><span data-stu-id="e1dc4-205">Original Request using `AddRedirectToHttpsPermanent`: `/secure`</span></span>

![Окно браузера со средствами разработчика отслеживания запросов и ответов](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="e1dc4-207">Перезапись URL-адреса</span><span class="sxs-lookup"><span data-stu-id="e1dc4-207">URL rewrite</span></span>
<span data-ttu-id="e1dc4-208">Используйте `AddRewrite` для создания правил для перезаписи URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-208">Use `AddRewrite` to create a rules for rewriting URLs.</span></span> <span data-ttu-id="e1dc4-209">Первый параметр содержит регулярное выражение для сопоставления на входящий URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-209">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="e1dc4-210">Второй параметр является строка замены.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-210">The second parameter is the replacement string.</span></span> <span data-ttu-id="e1dc4-211">Третий параметр `skipRemainingRules: {true|false}`, указывает по промежуточного слоя ли пропустить правила дополнительных подстановки, если применяется текущего правила.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-211">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e1dc4-212">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e1dc4-212">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=6)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e1dc4-213">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e1dc4-213">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=3)]

---

<span data-ttu-id="e1dc4-214">Исходный запрос:`/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="e1dc4-214">Original Request: `/rewrite-rule/1234/5678`</span></span>

![Окно браузера со средствами разработчика отслеживания запросов и ответов](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="e1dc4-216">Первое, что в регулярное выражение — начальный (`^`) в начале выражения.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-216">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="e1dc4-217">Это означает, что сопоставление начинается в начале URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-217">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="e1dc4-218">В предыдущем примере с помощью правила перенаправления `redirect-rule/(.*)`, отсутствует в начале регулярных выражений не начальный; таким образом, может предшествовать символы `redirect-rule/` в пути, для успешного сопоставления.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-218">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="e1dc4-219">Путь</span><span class="sxs-lookup"><span data-stu-id="e1dc4-219">Path</span></span>                               | <span data-ttu-id="e1dc4-220">Соответствие</span><span class="sxs-lookup"><span data-stu-id="e1dc4-220">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="e1dc4-221">Да</span><span class="sxs-lookup"><span data-stu-id="e1dc4-221">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="e1dc4-222">Да</span><span class="sxs-lookup"><span data-stu-id="e1dc4-222">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="e1dc4-223">Да</span><span class="sxs-lookup"><span data-stu-id="e1dc4-223">Yes</span></span>   |

<span data-ttu-id="e1dc4-224">Правило подстановки, `^rewrite-rule/(\d+)/(\d+)`, соответствует пути, только если они начинаются с `rewrite-rule/`.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-224">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="e1dc4-225">Обратите внимание на разницу в сопоставлении ниже правила подстановки и выше правила перенаправления.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-225">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="e1dc4-226">Путь</span><span class="sxs-lookup"><span data-stu-id="e1dc4-226">Path</span></span>                              | <span data-ttu-id="e1dc4-227">Соответствие</span><span class="sxs-lookup"><span data-stu-id="e1dc4-227">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="e1dc4-228">Да</span><span class="sxs-lookup"><span data-stu-id="e1dc4-228">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="e1dc4-229">Нет</span><span class="sxs-lookup"><span data-stu-id="e1dc4-229">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="e1dc4-230">Нет</span><span class="sxs-lookup"><span data-stu-id="e1dc4-230">No</span></span>    |

<span data-ttu-id="e1dc4-231">После `^rewrite-rule/` часть выражения, имеются две группы захвата, `(\d+)/(\d+)`.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-231">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="e1dc4-232">`\d` Означает *сопоставления цифр (number)*.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-232">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="e1dc4-233">Знак «плюс» (`+`) означает *сопоставление одного или нескольких предшествующий символ*.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-233">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="e1dc4-234">Таким образом URL-адрес должен содержать числом следуют прямой косой черты следуют другой номер.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-234">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="e1dc4-235">Эти записи группы вводится в перезаписанного URL-адрес как `$1` и `$2`.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-235">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="e1dc4-236">Строка замены правило перезаписи помещает записанной группы в строке запроса.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-236">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="e1dc4-237">Запрошенный путь `/rewrite-rule/1234/5678` переписать так, чтобы получить ресурс по адресу `/rewritten?var1=1234&var2=5678`.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-237">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="e1dc4-238">Если строку запроса присутствует в исходном запросе, он сохраняется при переписать URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-238">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="e1dc4-239">Без обращения к серверу, чтобы получить ресурс не существует.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-239">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="e1dc4-240">Если ресурс существует, она имеет выборке и возвращаются клиенту с кодом состояния 200 (ОК).</span><span class="sxs-lookup"><span data-stu-id="e1dc4-240">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="e1dc4-241">Поскольку клиент не перенаправлен, URL-адрес в адресной строке браузера не изменяется.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-241">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="e1dc4-242">Как клиент отвечает, произошло никогда не операции перезаписи URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-242">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="e1dc4-243">Используйте `skipRemainingRules: true` по возможности, поскольку правила сопоставления ресурсоемким процессом и сократить время отклика приложений.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-243">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="e1dc4-244">Самый быстрый ответ приложения:</span><span class="sxs-lookup"><span data-stu-id="e1dc4-244">For the fastest app response:</span></span>
> * <span data-ttu-id="e1dc4-245">Порядок правила подстановки из наиболее часто соответствующие правила реже всего соответствующие правила.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-245">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="e1dc4-246">Пропустите обработку остальных правил при совпадении и обработка дополнительное правило не является обязательным.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-246">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="e1dc4-247">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="e1dc4-247">Apache mod_rewrite</span></span>
<span data-ttu-id="e1dc4-248">Применение правил Apache mod_rewrite с `AddApacheModRewrite`.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-248">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="e1dc4-249">Убедитесь, что файл правил развертывается с приложением.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-249">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="e1dc4-250">Дополнительные сведения и примеры правил mod_rewrite см. в разделе [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span><span class="sxs-lookup"><span data-stu-id="e1dc4-250">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e1dc4-251">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e1dc4-251">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e1dc4-252">Объект `StreamReader` используется для чтения правил из *ApacheModRewrite.txt* файлу правил.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-252">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=1,7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e1dc4-253">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e1dc4-253">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e1dc4-254">Первый параметр принимает `IFileProvider`, которая обеспечивается за счет [внедрения зависимостей](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="e1dc4-254">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="e1dc4-255">`IHostingEnvironment` Встраивается для предоставления `ContentRootFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-255">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="e1dc4-256">Второй параметр — путь к файлу правил, являющийся *ApacheModRewrite.txt* в пример приложения.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-256">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=4)]

---

<span data-ttu-id="e1dc4-257">Пример приложения перенаправляет запросы с `/apache-mod-rules-redirect/(.\*)` для `/redirected?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-257">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="e1dc4-258">Код состояния ответа: 302 (найдено).</span><span class="sxs-lookup"><span data-stu-id="e1dc4-258">The response status code is 302 (Found).</span></span>

[!code[Main](url-rewriting/samples/2.x/ApacheModRewrite.txt)]

<span data-ttu-id="e1dc4-259">Исходный запрос:`/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="e1dc4-259">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![Окно браузера со средствами разработчика отслеживания запросов и ответов](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a><span data-ttu-id="e1dc4-261">Поддерживаемые серверные переменные</span><span class="sxs-lookup"><span data-stu-id="e1dc4-261">Supported server variables</span></span>
<span data-ttu-id="e1dc4-262">По промежуточного слоя поддерживает следующие переменные сервера Apache mod_rewrite:</span><span class="sxs-lookup"><span data-stu-id="e1dc4-262">The middleware supports the following Apache mod_rewrite server variables:</span></span>
* <span data-ttu-id="e1dc4-263">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="e1dc4-263">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="e1dc4-264">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="e1dc4-264">HTTP_ACCEPT</span></span>
* <span data-ttu-id="e1dc4-265">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="e1dc4-265">HTTP_CONNECTION</span></span>
* <span data-ttu-id="e1dc4-266">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="e1dc4-266">HTTP_COOKIE</span></span>
* <span data-ttu-id="e1dc4-267">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="e1dc4-267">HTTP_FORWARDED</span></span>
* <span data-ttu-id="e1dc4-268">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="e1dc4-268">HTTP_HOST</span></span>
* <span data-ttu-id="e1dc4-269">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="e1dc4-269">HTTP_REFERER</span></span>
* <span data-ttu-id="e1dc4-270">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="e1dc4-270">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="e1dc4-271">HTTPS</span><span class="sxs-lookup"><span data-stu-id="e1dc4-271">HTTPS</span></span>
* <span data-ttu-id="e1dc4-272">IPV6</span><span class="sxs-lookup"><span data-stu-id="e1dc4-272">IPV6</span></span>
* <span data-ttu-id="e1dc4-273">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="e1dc4-273">QUERY_STRING</span></span>
* <span data-ttu-id="e1dc4-274">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="e1dc4-274">REMOTE_ADDR</span></span>
* <span data-ttu-id="e1dc4-275">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="e1dc4-275">REMOTE_PORT</span></span>
* <span data-ttu-id="e1dc4-276">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="e1dc4-276">REQUEST_FILENAME</span></span>
* <span data-ttu-id="e1dc4-277">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="e1dc4-277">REQUEST_METHOD</span></span>
* <span data-ttu-id="e1dc4-278">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="e1dc4-278">REQUEST_SCHEME</span></span>
* <span data-ttu-id="e1dc4-279">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="e1dc4-279">REQUEST_URI</span></span>
* <span data-ttu-id="e1dc4-280">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="e1dc4-280">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="e1dc4-281">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="e1dc4-281">SERVER_ADDR</span></span>
* <span data-ttu-id="e1dc4-282">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="e1dc4-282">SERVER_PORT</span></span>
* <span data-ttu-id="e1dc4-283">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="e1dc4-283">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="e1dc4-284">ВРЕМЯ</span><span class="sxs-lookup"><span data-stu-id="e1dc4-284">TIME</span></span>
* <span data-ttu-id="e1dc4-285">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="e1dc4-285">TIME_DAY</span></span>
* <span data-ttu-id="e1dc4-286">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="e1dc4-286">TIME_HOUR</span></span>
* <span data-ttu-id="e1dc4-287">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="e1dc4-287">TIME_MIN</span></span>
* <span data-ttu-id="e1dc4-288">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="e1dc4-288">TIME_MON</span></span>
* <span data-ttu-id="e1dc4-289">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="e1dc4-289">TIME_SEC</span></span>
* <span data-ttu-id="e1dc4-290">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="e1dc4-290">TIME_WDAY</span></span>
* <span data-ttu-id="e1dc4-291">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="e1dc4-291">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="e1dc4-292">Правила модуль переопределения URL-адреса IIS</span><span class="sxs-lookup"><span data-stu-id="e1dc4-292">IIS URL Rewrite Module rules</span></span>
<span data-ttu-id="e1dc4-293">Чтобы использовать правила, применяемые к модуль переопределения URL-адреса IIS, используйте `AddIISUrlRewrite`.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-293">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="e1dc4-294">Убедитесь, что файл правил развертывается с приложением.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-294">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="e1dc4-295">Не направлять по промежуточного слоя для использования вашей *web.config* файлов при работе в Windows Server IIS.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-295">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="e1dc4-296">В службах IIS, эти правила должны храниться вне вашего *web.config* для предотвращения конфликтов с модуль переопределения IIS.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-296">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="e1dc4-297">Дополнительные сведения и примеры правил модуль переопределения URL-адрес служб IIS см. в разделе [2.0 с помощью модуля перепишите URL-адрес](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) и [URL-адрес ссылки перепишите конфигурации модуль](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span><span class="sxs-lookup"><span data-stu-id="e1dc4-297">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e1dc4-298">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e1dc4-298">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e1dc4-299">Объект `StreamReader` используется для чтения правил из *IISUrlRewrite.xml* файлу правил.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-299">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=2,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e1dc4-300">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e1dc4-300">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e1dc4-301">Первый параметр принимает `IFileProvider`, а второй параметр — это путь к XML-файл правил, который является *IISUrlRewrite.xml* в пример приложения.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-301">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=5)]

---

<span data-ttu-id="e1dc4-302">Пример приложения переписывает запросы от `/iis-rules-rewrite/(.*)` для `/rewritten?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-302">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="e1dc4-303">Ответ отправляется клиенту с кодом состояния 200 (ОК).</span><span class="sxs-lookup"><span data-stu-id="e1dc4-303">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[Main](url-rewriting/samples/2.x/IISUrlRewrite.xml)]

<span data-ttu-id="e1dc4-304">Исходный запрос:`/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="e1dc4-304">Original Request: `/iis-rules-rewrite/1234`</span></span>

![Окно браузера со средствами разработчика отслеживания запросов и ответов](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="e1dc4-306">Если имеется активный модуль IIS переписать с настроены правила брандмауэра уровня сервера, которые повлияло бы на ваше приложение нежелательных способами, можно отключить модуль переопределения IIS для приложения.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-306">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="e1dc4-307">Дополнительные сведения см. в разделе [модули IIS Отключение](xref:hosting/iis-modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="e1dc4-307">For more information, see [Disabling IIS modules](xref:hosting/iis-modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="e1dc4-308">Неподдерживаемые функции</span><span class="sxs-lookup"><span data-stu-id="e1dc4-308">Unsupported features</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e1dc4-309">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e1dc4-309">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e1dc4-310">По промежуточного слоя выпущен с ASP.NET Core 2.x не поддерживает следующие функции модуль переопределения URL-адреса IIS:</span><span class="sxs-lookup"><span data-stu-id="e1dc4-310">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="e1dc4-311">Правила для исходящих подключений</span><span class="sxs-lookup"><span data-stu-id="e1dc4-311">Outbound Rules</span></span>
* <span data-ttu-id="e1dc4-312">Переменные пользовательского сервера</span><span class="sxs-lookup"><span data-stu-id="e1dc4-312">Custom Server Variables</span></span>
* <span data-ttu-id="e1dc4-313">Знаки подстановки</span><span class="sxs-lookup"><span data-stu-id="e1dc4-313">Wildcards</span></span>
* <span data-ttu-id="e1dc4-314">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="e1dc4-314">LogRewrittenUrl</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e1dc4-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e1dc4-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e1dc4-316">По промежуточного слоя выпущен с ASP.NET Core 1.x не поддерживает следующие функции модуль переопределения URL-адреса IIS:</span><span class="sxs-lookup"><span data-stu-id="e1dc4-316">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="e1dc4-317">Глобальные правила</span><span class="sxs-lookup"><span data-stu-id="e1dc4-317">Global Rules</span></span>
* <span data-ttu-id="e1dc4-318">Правила для исходящих подключений</span><span class="sxs-lookup"><span data-stu-id="e1dc4-318">Outbound Rules</span></span>
* <span data-ttu-id="e1dc4-319">Перепишите карты</span><span class="sxs-lookup"><span data-stu-id="e1dc4-319">Rewrite Maps</span></span>
* <span data-ttu-id="e1dc4-320">Действие CustomResponse</span><span class="sxs-lookup"><span data-stu-id="e1dc4-320">CustomResponse Action</span></span>
* <span data-ttu-id="e1dc4-321">Переменные пользовательского сервера</span><span class="sxs-lookup"><span data-stu-id="e1dc4-321">Custom Server Variables</span></span>
* <span data-ttu-id="e1dc4-322">Знаки подстановки</span><span class="sxs-lookup"><span data-stu-id="e1dc4-322">Wildcards</span></span>
* <span data-ttu-id="e1dc4-323">Действие: CustomResponse</span><span class="sxs-lookup"><span data-stu-id="e1dc4-323">Action:CustomResponse</span></span>
* <span data-ttu-id="e1dc4-324">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="e1dc4-324">LogRewrittenUrl</span></span>

---

#### <a name="supported-server-variables"></a><span data-ttu-id="e1dc4-325">Поддерживаемые серверные переменные</span><span class="sxs-lookup"><span data-stu-id="e1dc4-325">Supported server variables</span></span>
<span data-ttu-id="e1dc4-326">По промежуточного слоя поддерживает следующие переменные сервера модуль переопределения URL-адреса IIS.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-326">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>
* <span data-ttu-id="e1dc4-327">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="e1dc4-327">CONTENT_LENGTH</span></span>
* <span data-ttu-id="e1dc4-328">УКАЗЫВАЕТСЯ</span><span class="sxs-lookup"><span data-stu-id="e1dc4-328">CONTENT_TYPE</span></span>
* <span data-ttu-id="e1dc4-329">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="e1dc4-329">HTTP_ACCEPT</span></span>
* <span data-ttu-id="e1dc4-330">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="e1dc4-330">HTTP_CONNECTION</span></span>
* <span data-ttu-id="e1dc4-331">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="e1dc4-331">HTTP_COOKIE</span></span>
* <span data-ttu-id="e1dc4-332">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="e1dc4-332">HTTP_HOST</span></span>
* <span data-ttu-id="e1dc4-333">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="e1dc4-333">HTTP_REFERER</span></span>
* <span data-ttu-id="e1dc4-334">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="e1dc4-334">HTTP_URL</span></span>
* <span data-ttu-id="e1dc4-335">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="e1dc4-335">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="e1dc4-336">HTTPS</span><span class="sxs-lookup"><span data-stu-id="e1dc4-336">HTTPS</span></span>
* <span data-ttu-id="e1dc4-337">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="e1dc4-337">LOCAL_ADDR</span></span>
* <span data-ttu-id="e1dc4-338">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="e1dc4-338">QUERY_STRING</span></span>
* <span data-ttu-id="e1dc4-339">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="e1dc4-339">REMOTE_ADDR</span></span>
* <span data-ttu-id="e1dc4-340">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="e1dc4-340">REMOTE_PORT</span></span>
* <span data-ttu-id="e1dc4-341">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="e1dc4-341">REQUEST_FILENAME</span></span>
* <span data-ttu-id="e1dc4-342">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="e1dc4-342">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="e1dc4-343">Можно также получить `IFileProvider` через `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-343">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="e1dc4-344">Этот подход может обеспечивают большую гибкость для расположения вашей перезаписи файлов правил.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-344">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="e1dc4-345">Убедитесь, что правила перезаписи файлов развертываются в указании пути к серверу.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-345">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="e1dc4-346">Правила на основе метода</span><span class="sxs-lookup"><span data-stu-id="e1dc4-346">Method-based rule</span></span>
<span data-ttu-id="e1dc4-347">Используйте `Add(Action<RewriteContext> applyRule)` реализовать собственную логику правила в методе.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-347">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="e1dc4-348">`RewriteContext` Предоставляет `HttpContext` для использования в методе.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-348">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="e1dc4-349">`context.Result` Определяет, как увеличение конвейера осуществляется обработка.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-349">The `context.Result` determines how additional pipeline processing is handled.</span></span>

| <span data-ttu-id="e1dc4-350">контекст. Результат</span><span class="sxs-lookup"><span data-stu-id="e1dc4-350">context.Result</span></span>                       | <span data-ttu-id="e1dc4-351">Действие</span><span class="sxs-lookup"><span data-stu-id="e1dc4-351">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="e1dc4-352">`RuleResult.ContinueRules` (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="e1dc4-352">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="e1dc4-353">Продолжить применение правил</span><span class="sxs-lookup"><span data-stu-id="e1dc4-353">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="e1dc4-354">Остановить применение правил и отправки ответа</span><span class="sxs-lookup"><span data-stu-id="e1dc4-354">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="e1dc4-355">Остановить применение правил и отправить контекст следующее по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="e1dc4-355">Stop applying rules and send the context to the next middleware</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e1dc4-356">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e1dc4-356">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e1dc4-357">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e1dc4-357">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=6)]

---

<span data-ttu-id="e1dc4-358">Пример приложения показан метод, который перенаправляет запросы для путей, заканчиваться *.xml*.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-358">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="e1dc4-359">Если сделать запрос `/file.xml`, оно перенаправляется в `/xmlfiles/file.xml`.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-359">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="e1dc4-360">Код состояния равно 301 (перемещен постоянно).</span><span class="sxs-lookup"><span data-stu-id="e1dc4-360">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="e1dc4-361">Для перенаправления необходимо явно задать код состояния ответа; в противном случае возвращается код состояния 200 (ОК) и перенаправление не будет выполняться на клиенте.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-361">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e1dc4-362">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e1dc4-362">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e1dc4-363">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e1dc4-363">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet2)]

---

<span data-ttu-id="e1dc4-364">Исходный запрос:`/file.xml`</span><span class="sxs-lookup"><span data-stu-id="e1dc4-364">Original Request: `/file.xml`</span></span>

![Окно браузера со средствами разработчика отслеживания запросов и ответов для file.xml](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="e1dc4-366">Правила на основе IRule</span><span class="sxs-lookup"><span data-stu-id="e1dc4-366">IRule-based rule</span></span>
<span data-ttu-id="e1dc4-367">Используйте `Add(IRule)` реализовать собственную логику правила в класс, производный от `IRule`.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-367">Use `Add(IRule)` to implement your own rule logic in a class that derives from `IRule`.</span></span> <span data-ttu-id="e1dc4-368">С помощью `IRule` обеспечивает большую гибкость по сравнению с использованием подхода правила на основе методов.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-368">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="e1dc4-369">Производный класс может включать конструктор, где можно передать в параметры для `ApplyRule` метод.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-369">Your derived class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e1dc4-370">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e1dc4-370">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e1dc4-371">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e1dc4-371">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=7-8)]

---

<span data-ttu-id="e1dc4-372">Значения параметров в пример приложения для `extension` и `newPath` проверяются, чтобы удовлетворять нескольким условиям.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-372">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="e1dc4-373">`extension` Должен содержать значение, а значение должно быть *.png*, *.jpg*, или *.gif*.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-373">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="e1dc4-374">Если `newPath` является недопустимой, `ArgumentException` возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-374">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="e1dc4-375">Если сделать запрос *image.png*, оно перенаправляется в `/png-images/image.png`.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-375">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="e1dc4-376">Если сделать запрос *image.jpg*, оно перенаправляется в `/jpg-images/image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-376">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="e1dc4-377">Чтобы 301 (перемещен постоянно), задается код состояния и `context.Result` прекратить обработку правил и отправки ответа.</span><span class="sxs-lookup"><span data-stu-id="e1dc4-377">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e1dc4-378">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e1dc4-378">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e1dc4-379">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e1dc4-379">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/RewriteRule.cs?name=snippet1)]

---

<span data-ttu-id="e1dc4-380">Исходный запрос:`/image.png`</span><span class="sxs-lookup"><span data-stu-id="e1dc4-380">Original Request: `/image.png`</span></span>

![Окно браузера со средствами разработчика отслеживания запросов и ответов для image.png](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="e1dc4-382">Исходный запрос:`/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="e1dc4-382">Original Request: `/image.jpg`</span></span>

![Окно браузера со средствами разработчика отслеживания запросов и ответов для image.jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="e1dc4-384">Примеры регулярных выражений</span><span class="sxs-lookup"><span data-stu-id="e1dc4-384">Regex examples</span></span>

| <span data-ttu-id="e1dc4-385">Goal</span><span class="sxs-lookup"><span data-stu-id="e1dc4-385">Goal</span></span> | <span data-ttu-id="e1dc4-386">Строка Regex &</span><span class="sxs-lookup"><span data-stu-id="e1dc4-386">Regex String &</span></span><br><span data-ttu-id="e1dc4-387">Пример соответствия</span><span class="sxs-lookup"><span data-stu-id="e1dc4-387">Match Example</span></span> | <span data-ttu-id="e1dc4-388">Строка замены &</span><span class="sxs-lookup"><span data-stu-id="e1dc4-388">Replacement String &</span></span><br><span data-ttu-id="e1dc4-389">Пример выходных данных</span><span class="sxs-lookup"><span data-stu-id="e1dc4-389">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="e1dc4-390">Путь перезаписи в строки запроса</span><span class="sxs-lookup"><span data-stu-id="e1dc4-390">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="e1dc4-391">Полоса косой чертой</span><span class="sxs-lookup"><span data-stu-id="e1dc4-391">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="e1dc4-392">Применять косой чертой</span><span class="sxs-lookup"><span data-stu-id="e1dc4-392">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="e1dc4-393">Избежать перезаписи определенные запросы</span><span class="sxs-lookup"><span data-stu-id="e1dc4-393">Avoid rewriting specific requests</span></span> | `(.*[^(\.axd)])$`<br><span data-ttu-id="e1dc4-394">Да:`/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="e1dc4-394">Yes: `/resource.htm`</span></span><br><span data-ttu-id="e1dc4-395">Нет:`/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="e1dc4-395">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="e1dc4-396">Изменение порядка сегменты URL-адреса</span><span class="sxs-lookup"><span data-stu-id="e1dc4-396">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="e1dc4-397">Замените сегмент URL-адреса</span><span class="sxs-lookup"><span data-stu-id="e1dc4-397">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="e1dc4-398">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e1dc4-398">Additional resources</span></span>
* [<span data-ttu-id="e1dc4-399">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="e1dc4-399">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="e1dc4-400">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="e1dc4-400">Middleware</span></span>](middleware.md)
* [<span data-ttu-id="e1dc4-401">Регулярные выражения в .NET</span><span class="sxs-lookup"><span data-stu-id="e1dc4-401">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="e1dc4-402">Элементы языка регулярных выражений — краткий справочник</span><span class="sxs-lookup"><span data-stu-id="e1dc4-402">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="e1dc4-403">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="e1dc4-403">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="e1dc4-404">Используя модуль переопределения URL-адрес 2.0 (для IIS)</span><span class="sxs-lookup"><span data-stu-id="e1dc4-404">Using Url Rewrite Module 2.0 (for IIS)</span></span>](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [<span data-ttu-id="e1dc4-405">Справочник по конфигурации модуля перезаписи URL-адрес</span><span class="sxs-lookup"><span data-stu-id="e1dc4-405">URL Rewrite Module Configuration Reference</span></span>](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [<span data-ttu-id="e1dc4-406">Перепишите модуля IIS URL-адрес форума</span><span class="sxs-lookup"><span data-stu-id="e1dc4-406">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* [<span data-ttu-id="e1dc4-407">Сохранять простую структуру с URL-адрес</span><span class="sxs-lookup"><span data-stu-id="e1dc4-407">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* [<span data-ttu-id="e1dc4-408">10 перезаписи URL-адресов, советы и рекомендации</span><span class="sxs-lookup"><span data-stu-id="e1dc4-408">10 URL Rewriting Tips and Tricks</span></span>](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [<span data-ttu-id="e1dc4-409">Косая черта или не косая черта</span><span class="sxs-lookup"><span data-stu-id="e1dc4-409">To slash or not to slash</span></span>](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
