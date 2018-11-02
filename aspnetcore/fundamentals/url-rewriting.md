---
title: ПО промежуточного слоя для переопределения URL-адресов в ASP.NET Core
author: guardrex
description: Сведения о переопределении и перенаправлении URL-адресов с помощью ПО промежуточного слоя для переопределения URL-адресов в приложениях ASP.NET Core.
ms.author: riande
ms.date: 08/17/2017
uid: fundamentals/url-rewriting
ms.openlocfilehash: 5a1891c838436467fb49ff6288587fab08201179
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207190"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="ae8b8-103">ПО промежуточного слоя для переопределения URL-адресов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ae8b8-103">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="ae8b8-104">Авторы: [Люк Лэтем](https://github.com/guardrex) (Luke Latham) и [Микаэль Менгисту](https://github.com/mikaelm12) (Mikael Mengistu)</span><span class="sxs-lookup"><span data-stu-id="ae8b8-104">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="ae8b8-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ae8b8-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ae8b8-106">Переопределение URL-адресов представляет собой изменение URL-адресов запросов на основе одного или нескольких предопределенных правил.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-106">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="ae8b8-107">Переопределение URL-адресов создает абстракцию между расположениями ресурсов и их адресами, позволяя убрать тесную связь между ними.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-107">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="ae8b8-108">Существует несколько сценариев, где может пригодиться переопределение URL-адресов:</span><span class="sxs-lookup"><span data-stu-id="ae8b8-108">There are several scenarios where URL rewriting is valuable:</span></span>

* <span data-ttu-id="ae8b8-109">Временное или постоянное перемещение или замещение ресурсов сервера с сохранением стабильных указателей для этих ресурсов.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-109">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources.</span></span>
* <span data-ttu-id="ae8b8-110">Разделение обработки запросов между разными приложениями или разными областями одного приложения.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-110">Splitting request processing across different apps or across areas of one app.</span></span>
* <span data-ttu-id="ae8b8-111">Удаление, добавление или переупорядочение сегментов URL-адресов для входящих запросов.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-111">Removing, adding, or reorganizing URL segments on incoming requests.</span></span>
* <span data-ttu-id="ae8b8-112">Оптимизация общедоступных URL-адресов в целях оптимизации для поисковых систем (SEO).</span><span class="sxs-lookup"><span data-stu-id="ae8b8-112">Optimizing public URLs for Search Engine Optimization (SEO).</span></span>
* <span data-ttu-id="ae8b8-113">Разрешение использования понятных общедоступных URL-адресов, чтобы пользователи могли прогнозировать, какое именно содержимое они найдут, перейдя по ссылке.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-113">Permitting the use of friendly public URLs to help people predict the content they will find by following a link.</span></span>
* <span data-ttu-id="ae8b8-114">Перенаправление небезопасных запросов на защищенные конечные точки.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-114">Redirecting insecure requests to secure endpoints.</span></span>
* <span data-ttu-id="ae8b8-115">Предотвращение использования активных ссылок на изображения.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-115">Preventing image hotlinking.</span></span>

<span data-ttu-id="ae8b8-116">Определить правила для изменения URL-адреса можно несколькими способами, включая регулярное выражение, правила модуля mod_rewrite Apache, правила модуля переопределения для IIS и логику настраиваемых правил.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-116">You can define rules for changing the URL in several ways, including Regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="ae8b8-117">Этот документ содержит вводные сведения о переопределении URL-адресов и инструкции по использованию ПО промежуточного слоя для переопределения URL-адресов в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-117">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="ae8b8-118">Переопределение URL-адресов может снижать производительность приложения.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-118">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="ae8b8-119">Следует по возможности ограничить число и сложность правил.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-119">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="ae8b8-120">Перенаправление и переопределение URL-адресов</span><span class="sxs-lookup"><span data-stu-id="ae8b8-120">URL redirect and URL rewrite</span></span>

<span data-ttu-id="ae8b8-121">На первый взгляд разница между *перенаправлением* и *переопределением* URL-адресов может показаться незначительной, но она оказывает значительное влияние на предоставление ресурсов клиентам.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-121">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="ae8b8-122">ПО промежуточного слоя для переопределения URL-адресов в ASP.NET Core удовлетворяет потребность в обеих этих функциях.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-122">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="ae8b8-123">*Перенаправление URL-адресов* является операцией на стороне клиента, которая предписывает клиенту обратиться к ресурсу по другому адресу.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-123">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="ae8b8-124">Для этого используется круговое обращение к серверу.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-124">This requires a round-trip to the server.</span></span> <span data-ttu-id="ae8b8-125">URL-адрес перенаправления, возвращаемый клиенту, отображается в адресной строке браузера, когда клиент отправляет новый запрос для данного ресурса.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-125">The redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> 

<span data-ttu-id="ae8b8-126">Если `/resource` *перенаправляется* к `/different-resource`, клиент запрашивает `/resource`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`.</span></span> <span data-ttu-id="ae8b8-127">Сервер отвечает, что клиенту следует получить ресурс в `/different-resource` с кодом состояния, указывающим, что такое перенаправление является временным или постоянным.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-127">The server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="ae8b8-128">Клиент выполняет новый запрос для ресурса по URL-адресу перенаправления.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-128">The client executes a new request for the resource at the redirect URL.</span></span>

![Конечная точка службы WebAPI временно изменена с версии 1 (v1) на версию 2 (v2) на сервере.](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="ae8b8-134">При перенаправлении запросов на другой URL-адрес можно указать, является ли оно постоянным или временным.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-134">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="ae8b8-135">Код состояния "301 (перемещен окончательно)" используется, когда ресурс имеет новый постоянный URL-адрес и вы хотите сообщить клиенту, что все последующие запросы для этого ресурса должны использовать новый URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-135">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="ae8b8-136">*Клиент может кэшировать отклик при получении кода состояния 301.*</span><span class="sxs-lookup"><span data-stu-id="ae8b8-136">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="ae8b8-137">Код состояния "302 (найдено)" используется, когда перенаправление является временным или обычно подвержено изменениям, из-за чего клиенту не требуется хранить и повторно использовать этот URL-адрес перенаправления в будущем.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-137">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="ae8b8-138">Дополнительные сведения см. в документе [RFC 2616: определения кодов состояния](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="ae8b8-138">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="ae8b8-139">*Переопределение URL-адреса* является операцией на стороне сервера для предоставления ресурса с другого адреса ресурса.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-139">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="ae8b8-140">Переопределение URL-адреса не требует кругового обращения к серверу.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-140">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="ae8b8-141">Переопределенный URL-адрес не возвращается клиенту и не отображается в адресной строке браузера.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-141">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="ae8b8-142">Когда `/resource` *переопределяется* на `/different-resource`, клиент запрашивает `/resource`, а сервер *внутренними средствами* получает этот ресурс в `/different-resource`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-142">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="ae8b8-143">Хотя клиент может быть в состоянии получить ресурс по переопределенному URL-адресу, когда клиент отправляет запрос и получает отклик, он не уведомляется о наличии ресурса по переопределенному URL-адресу.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-143">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![Конечная точка службы WebAPI была изменена с версии 1 (v1) на версию 2 (v2) на сервере.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="ae8b8-148">Пример приложения с переопределением URL-адресов</span><span class="sxs-lookup"><span data-stu-id="ae8b8-148">URL rewriting sample app</span></span>

<span data-ttu-id="ae8b8-149">Вы можете ознакомиться с функциями переопределения URL-адресов на [примере приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span><span class="sxs-lookup"><span data-stu-id="ae8b8-149">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span></span> <span data-ttu-id="ae8b8-150">Это приложение применяет правила переопределения и перенаправления и отображает переопределенный или перенаправленный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-150">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="ae8b8-151">Условия для использования ПО промежуточного слоя для переопределения URL-адресов</span><span class="sxs-lookup"><span data-stu-id="ae8b8-151">When to use URL Rewriting Middleware</span></span>

<span data-ttu-id="ae8b8-152">ПО промежуточного слоя для переопределения URL-адресов можно использовать, если не удается использовать [модуль переопределения URL-адресов](https://www.iis.net/downloads/microsoft/url-rewrite) для IIS в Windows Server, [модуль mod_rewrite Apache](https://httpd.apache.org/docs/2.4/rewrite/) на сервере Apache, [переопределение URL-адресов в Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/) либо если приложение размещено на [сервере HTTP.sys](xref:fundamentals/servers/httpsys) (предыдущее название [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="ae8b8-152">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="ae8b8-153">Основные причины для использования серверных технологий переопределения URL-адресов в IIS, Apache или Nginx заключаются в том, что ПО промежуточного слоя не поддерживает все функции этих модулей и, скорее всего, не соответствует их уровню производительности.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-153">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="ae8b8-154">Однако существуют некоторые функции серверных модулей, которые не работают с проектами ASP.NET Core, например ограничения `IsFile` и `IsDirectory` для модуля переопределения IIS.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-154">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="ae8b8-155">В этих случаях следует использовать ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-155">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="ae8b8-156">Пакет</span><span class="sxs-lookup"><span data-stu-id="ae8b8-156">Package</span></span>

<span data-ttu-id="ae8b8-157">Чтобы включить ПО промежуточного слоя в проект, добавьте ссылку на пакет [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/).</span><span class="sxs-lookup"><span data-stu-id="ae8b8-157">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="ae8b8-158">Эта функция доступна для приложений, предназначенных для ASP.NET Core 1.1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-158">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="ae8b8-159">Расширение и параметры</span><span class="sxs-lookup"><span data-stu-id="ae8b8-159">Extension and options</span></span>

<span data-ttu-id="ae8b8-160">Задайте правила переопределения и перенаправления URL-адресов, создав экземпляр класса [RewriteOptions](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptions) для каждого правила с помощью методов расширения.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-160">Establish your URL rewrite and redirect rules by creating an instance of the [RewriteOptions](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptions) class with extension methods for each of your rules.</span></span> <span data-ttu-id="ae8b8-161">Несколько правил можно объединить в цепочку в том порядке, в котором они должны обрабатываться.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-161">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="ae8b8-162">`RewriteOptions` передаются в ПО промежуточного слоя для переопределения URL-адресов по мере его добавления в конвейер запросов с помощью `app.UseRewriter(options);`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-162">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1")
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true)
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")
        .Add(RedirectXMLRequests)
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="redirect-non-www-to-www"></a><span data-ttu-id="ae8b8-163">Перенаправление не www на www</span><span class="sxs-lookup"><span data-stu-id="ae8b8-163">Redirect non-www to www</span></span>

<span data-ttu-id="ae8b8-164">Три параметра разрешают приложению перенаправлять запросы, отличные от `www`, на `www`:</span><span class="sxs-lookup"><span data-stu-id="ae8b8-164">Three options permit the app to redirect non-`www` requests to `www`:</span></span>

* <span data-ttu-id="ae8b8-165">[AddRedirectToWwwPermanent(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowwwpermanent) &ndash; окончательно перенаправляет запрос в поддомен `www`, если запрос не является `www`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-165">[AddRedirectToWwwPermanent(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowwwpermanent) &ndash; Permanently redirect the request to the `www` subdomain if the request is non-`www`.</span></span> <span data-ttu-id="ae8b8-166">Перенаправляет с кодом состояния [Status308PermanentRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status308permanentredirect).</span><span class="sxs-lookup"><span data-stu-id="ae8b8-166">Redirects with a [Status308PermanentRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status308permanentredirect) status code.</span></span>
* <span data-ttu-id="ae8b8-167">[AddRedirectToWww(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; перенаправляет запрос в поддомен `www`, если входящий запрос не является `www`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-167">[AddRedirectToWww(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; Redirect the request to the `www` subdomain if the incoming request is non-`www`.</span></span> <span data-ttu-id="ae8b8-168">Перенаправляет с кодом состояния [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect).</span><span class="sxs-lookup"><span data-stu-id="ae8b8-168">Redirects with a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) status code.</span></span>
* <span data-ttu-id="ae8b8-169">[AddRedirectToWww(RewriteOptions, Int32)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; перенаправляет запрос в поддомен `www`, если входящий запрос не является `www`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-169">[AddRedirectToWww(RewriteOptions, Int32)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; Redirect the request to the `www` subdomain if the incoming request is non-`www`.</span></span> <span data-ttu-id="ae8b8-170">Позволяет предоставить код состояния для ответа.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-170">Allows you to provide the status code for the response.</span></span> <span data-ttu-id="ae8b8-171">Используйте поля класса [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) для назначений `AddRedirectToWww`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-171">Use the fields of the [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) class for assignments to `AddRedirectToWww`.</span></span>

::: moniker-end

### <a name="url-redirect"></a><span data-ttu-id="ae8b8-172">Перенаправление URL-адресов</span><span class="sxs-lookup"><span data-stu-id="ae8b8-172">URL redirect</span></span>

<span data-ttu-id="ae8b8-173">Используйте `AddRedirect` для перенаправления запросов.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-173">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="ae8b8-174">Первый параметр содержит регулярное выражение, соответствующее пути входящего URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-174">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="ae8b8-175">Второй параметр является строкой замены.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-175">The second parameter is the replacement string.</span></span> <span data-ttu-id="ae8b8-176">Третий параметр (при его наличии) указывает код состояния.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-176">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="ae8b8-177">Если код состояния не задан, по умолчанию используется код "302 (найдено)", указывающий, что ресурс временно перемещен или заменен.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-177">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="ae8b8-178">В браузере с включенными средствами разработчика выполните запрос для примера приложения по пути `/redirect-rule/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-178">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="ae8b8-179">Регулярное выражение соответствует пути запроса в `redirect-rule/(.*)`, и этот путь заменяется на `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-179">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="ae8b8-180">URL-адрес перенаправления отправляется обратно клиенту с кодом состояния "302 (найдено)".</span><span class="sxs-lookup"><span data-stu-id="ae8b8-180">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="ae8b8-181">Браузер выполняет новый запрос на URL-адрес перенаправления, отображаемый в адресной строке.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-181">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="ae8b8-182">Так как ни одно из правил в примере приложения не соответствует URL-адресу перенаправления, второй запрос получает от приложения отклик "200 (ОК)", в тексте которого указан этот URL-адрес перенаправления.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-182">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="ae8b8-183">При *перенаправлении* URL-адреса используется круговое обращение к серверу.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-183">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="ae8b8-184">Соблюдайте осторожность при задании правил перенаправления.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-184">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="ae8b8-185">Они оцениваются для каждого запроса, отправляемого в приложение, даже после перенаправления.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-185">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="ae8b8-186">Можно легко случайно создать бесконечный цикл перенаправлений.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-186">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="ae8b8-187">Исходный запрос: `/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="ae8b8-187">Original Request: `/redirect-rule/1234/5678`</span></span>

![Окно браузера, в котором средства разработчика отслеживают запросы и отклики](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="ae8b8-189">Часть выражения, заключенная в скобки, называется *группой записи*.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-189">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="ae8b8-190">Точка (`.`) в выражении означает *совпадение с любым символом*.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-190">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="ae8b8-191">Звездочка (`*`) означает *совпадение с предыдущим символом ноль или более раз*.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-191">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="ae8b8-192">Таким образом, два последних сегмента пути URL-адреса, `1234/5678`, перехватываются группой записи `(.*)`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-192">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="ae8b8-193">Любое значение, указанное в URL-адресе запроса после `redirect-rule/`, перехватывается этой группой записи.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-193">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="ae8b8-194">В строке замены группы записи внедряются с помощью знака доллара (`$`), за которым следует порядковый номер записи.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-194">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="ae8b8-195">Первое значение группы записи получается с помощью `$1`, второе — с помощью `$2`, после чего они продолжают по очереди использоваться для групп записи в регулярном выражении.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-195">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="ae8b8-196">В регулярном выражении правила перенаправления в примере приложения присутствует всего одна группа записи, поэтому в строку замены внедряется тоже одна строка — `$1`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-196">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="ae8b8-197">Когда применяется правило, URL-адрес становится `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-197">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="ae8b8-198">Перенаправление URL-адресов на защищенную конечную точку</span><span class="sxs-lookup"><span data-stu-id="ae8b8-198">URL redirect to a secure endpoint</span></span>

<span data-ttu-id="ae8b8-199">Используйте `AddRedirectToHttps`, чтобы перенаправлять HTTP-запросы на тот же узел и путь с использованием протокола HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="ae8b8-199">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="ae8b8-200">Если код состояния не указан, ПО промежуточного слоя по умолчанию использует значение "302 (найдено)".</span><span class="sxs-lookup"><span data-stu-id="ae8b8-200">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="ae8b8-201">Если порт не указан, ПО промежуточного слоя по умолчанию использует значение `null`, в результате чего протокол изменяется на `https://`, а клиент обращается к ресурсу через порт 443.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-201">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="ae8b8-202">Этот пример показывает, как задать код состояния "301 (перемещен окончательно)" и изменить порт на 5001.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-202">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

<span data-ttu-id="ae8b8-203">Используйте `AddRedirectToHttpsPermanent`, чтобы перенаправить небезопасные запросы на тот же узел и путь с использованием безопасного протокола HTTPS (`https://` через порт 443).</span><span class="sxs-lookup"><span data-stu-id="ae8b8-203">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="ae8b8-204">ПО промежуточного слоя задает код состояния "301 (перемещен окончательно)".</span><span class="sxs-lookup"><span data-stu-id="ae8b8-204">The middleware sets the status code to 301 (Moved Permanently).</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> <span data-ttu-id="ae8b8-205">При перенаправлении на HTTPS без дополнительных правил перенаправления рекомендуется использовать ПО промежуточного слоя перенаправления на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-205">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware.</span></span> <span data-ttu-id="ae8b8-206">Дополнительные сведения см. в разделе [Обязательное использование HTTPS](xref:security/enforcing-ssl#require-https).</span><span class="sxs-lookup"><span data-stu-id="ae8b8-206">For more information, see the [Enforce HTTPS](xref:security/enforcing-ssl#require-https) topic.</span></span>

<span data-ttu-id="ae8b8-207">Пример приложения позволяет продемонстрировать использование `AddRedirectToHttps` или `AddRedirectToHttpsPermanent`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-207">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="ae8b8-208">Добавьте этот метод расширения в `RewriteOptions`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-208">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="ae8b8-209">Выполните небезопасный запрос к приложению по любому URL-адресу.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-209">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="ae8b8-210">Закройте предостережение системы безопасности о том, что самозаверяющий сертификат не является доверенным, или создайте исключение, чтобы сделать сертификат доверенным.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-210">Dismiss the browser security warning that the self-signed certificate is untrusted or create an exception to trust the certificate.</span></span>

<span data-ttu-id="ae8b8-211">Исходный запрос с использованием `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="ae8b8-211">Original Request using `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span></span>

![Окно браузера, в котором средства разработчика отслеживают запросы и отклики](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="ae8b8-213">Исходный запрос с использованием `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="ae8b8-213">Original Request using `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span></span>

![Окно браузера, в котором средства разработчика отслеживают запросы и отклики](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="ae8b8-215">Переопределение URL-адресов</span><span class="sxs-lookup"><span data-stu-id="ae8b8-215">URL rewrite</span></span>

<span data-ttu-id="ae8b8-216">Используйте `AddRewrite`, чтобы создать правило для переопределения URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-216">Use `AddRewrite` to create a rule for rewriting URLs.</span></span> <span data-ttu-id="ae8b8-217">Первый параметр содержит регулярное выражение, соответствующее пути входящего URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-217">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="ae8b8-218">Второй параметр является строкой замены.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-218">The second parameter is the replacement string.</span></span> <span data-ttu-id="ae8b8-219">Третий параметр `skipRemainingRules: {true|false}` сообщает ПО промежуточного слоя, нужно ли пропустить дополнительные правила переопределения, если применяется текущее правило.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-219">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="ae8b8-220">Исходный запрос: `/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="ae8b8-220">Original Request: `/rewrite-rule/1234/5678`</span></span>

![Окно браузера, в котором средства разработчика отслеживают запрос и отклик](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="ae8b8-222">Наиболее заметным символом в регулярном выражении является крышка (`^`) в его начале.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-222">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="ae8b8-223">Это означает, что сопоставление начинается с начала URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-223">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="ae8b8-224">В приведенном до этого примере с правилом перенаправления `redirect-rule/(.*)` крышка в начале регулярного выражения отсутствует, поэтому для удачного совпадения перед `redirect-rule/` в пути могут стоять любые символы.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-224">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="ae8b8-225">Путь</span><span class="sxs-lookup"><span data-stu-id="ae8b8-225">Path</span></span>                               | <span data-ttu-id="ae8b8-226">Соответствие</span><span class="sxs-lookup"><span data-stu-id="ae8b8-226">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="ae8b8-227">Да</span><span class="sxs-lookup"><span data-stu-id="ae8b8-227">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="ae8b8-228">Да</span><span class="sxs-lookup"><span data-stu-id="ae8b8-228">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="ae8b8-229">Да</span><span class="sxs-lookup"><span data-stu-id="ae8b8-229">Yes</span></span>   |

<span data-ttu-id="ae8b8-230">Правило переопределения `^rewrite-rule/(\d+)/(\d+)` соответствует путям, только если они начинаются с `rewrite-rule/`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-230">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="ae8b8-231">Обратите внимание на разницу в сопоставлении для приведенного ниже правила переопределения и приведенного выше правила перенаправления.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-231">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="ae8b8-232">Путь</span><span class="sxs-lookup"><span data-stu-id="ae8b8-232">Path</span></span>                              | <span data-ttu-id="ae8b8-233">Соответствие</span><span class="sxs-lookup"><span data-stu-id="ae8b8-233">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="ae8b8-234">Да</span><span class="sxs-lookup"><span data-stu-id="ae8b8-234">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="ae8b8-235">Нет</span><span class="sxs-lookup"><span data-stu-id="ae8b8-235">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="ae8b8-236">Нет</span><span class="sxs-lookup"><span data-stu-id="ae8b8-236">No</span></span>    |

<span data-ttu-id="ae8b8-237">После части `^rewrite-rule/` выражения стоят две группы записи `(\d+)/(\d+)`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-237">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="ae8b8-238">`\d` означает *соответствие цифре (числу)*.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-238">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="ae8b8-239">Знак "плюс" (`+`) означает *соответствие одному или нескольким предшествующим символам*.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-239">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="ae8b8-240">Таким образом, URL-адрес должен содержать число, за которым идет прямая косая черта, за которой идет другое число.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-240">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="ae8b8-241">Эти группы записи внедряются в переопределенный URL-адрес как `$1` и `$2`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-241">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="ae8b8-242">Строка замены для правила переопределения помещает группы записи в строку запроса.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-242">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="ae8b8-243">Запрошенный путь `/rewrite-rule/1234/5678` переопределяется, чтобы ресурс можно было получить по адресу `/rewritten?var1=1234&var2=5678`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-243">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="ae8b8-244">Если в исходном запросе присутствует строка запроса, она сохраняется при переопределении URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-244">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="ae8b8-245">Круговое обращение к серверу для получения ресурса не выполняется.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-245">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="ae8b8-246">Если ресурс существует, он извлекается и возвращается клиенту с кодом состояния "200 (ОК)".</span><span class="sxs-lookup"><span data-stu-id="ae8b8-246">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="ae8b8-247">Так как клиент не перенаправляется, URL-адрес в адресной строке браузера не изменяется.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-247">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="ae8b8-248">Пока осуществляется работа с клиентом, операция переопределения URL-адреса не выполняется.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-248">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="ae8b8-249">Использовать `skipRemainingRules: true` невозможно, так как правила сопоставления потребляют много ресурсов и ухудшают время отклика приложения.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-249">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="ae8b8-250">Чтобы максимально ускорить отклик приложения:</span><span class="sxs-lookup"><span data-stu-id="ae8b8-250">For the fastest app response:</span></span>
> * <span data-ttu-id="ae8b8-251">расположите правила переопределения в порядке от наиболее к наименее часто используемому;</span><span class="sxs-lookup"><span data-stu-id="ae8b8-251">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="ae8b8-252">пропускайте обработку оставшихся правил, когда совпадение найдено и обработка дополнительных правил не требуется.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-252">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="ae8b8-253">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="ae8b8-253">Apache mod_rewrite</span></span>

<span data-ttu-id="ae8b8-254">Для применения правил mod_rewrite Apache можно использовать `AddApacheModRewrite`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-254">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="ae8b8-255">Убедитесь, что файл правил развертывается вместе с приложением.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-255">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="ae8b8-256">Дополнительные сведения и примеры правил mod_rewrite см. в статье о [правилах mod_rewrite Apache](https://httpd.apache.org/docs/2.4/rewrite/).</span><span class="sxs-lookup"><span data-stu-id="ae8b8-256">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ae8b8-257">`StreamReader` используется для чтения правил из файла *ApacheModRewrite.txt*.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-257">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ae8b8-258">Первый параметр принимает `IFileProvider`, предоставляемый посредством [внедрения зависимостей](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="ae8b8-258">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="ae8b8-259">`IHostingEnvironment` внедряется для предоставления `ContentRootFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-259">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="ae8b8-260">Второй параметр является путем к файлу правил (в данном примере приложения это *ApacheModRewrite.txt*).</span><span class="sxs-lookup"><span data-stu-id="ae8b8-260">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="ae8b8-261">Пример приложения перенаправляет запросы из `/apache-mod-rules-redirect/(.\*)` в `/redirected?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-261">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="ae8b8-262">Отклик имеет код состояния "302 (найдено)".</span><span class="sxs-lookup"><span data-stu-id="ae8b8-262">The response status code is 302 (Found).</span></span>

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

<span data-ttu-id="ae8b8-263">Исходный запрос: `/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="ae8b8-263">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![Окно браузера, в котором средства разработчика отслеживают запросы и отклики](url-rewriting/_static/add_apache_mod_redirect.png)

<span data-ttu-id="ae8b8-265">ПО промежуточного слоя поддерживает следующие переменные сервера в mod_rewrite Apache:</span><span class="sxs-lookup"><span data-stu-id="ae8b8-265">The middleware supports the following Apache mod_rewrite server variables:</span></span>

* <span data-ttu-id="ae8b8-266">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="ae8b8-266">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="ae8b8-267">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="ae8b8-267">HTTP_ACCEPT</span></span>
* <span data-ttu-id="ae8b8-268">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="ae8b8-268">HTTP_CONNECTION</span></span>
* <span data-ttu-id="ae8b8-269">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="ae8b8-269">HTTP_COOKIE</span></span>
* <span data-ttu-id="ae8b8-270">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="ae8b8-270">HTTP_FORWARDED</span></span>
* <span data-ttu-id="ae8b8-271">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="ae8b8-271">HTTP_HOST</span></span>
* <span data-ttu-id="ae8b8-272">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="ae8b8-272">HTTP_REFERER</span></span>
* <span data-ttu-id="ae8b8-273">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="ae8b8-273">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="ae8b8-274">HTTPS</span><span class="sxs-lookup"><span data-stu-id="ae8b8-274">HTTPS</span></span>
* <span data-ttu-id="ae8b8-275">IPV6</span><span class="sxs-lookup"><span data-stu-id="ae8b8-275">IPV6</span></span>
* <span data-ttu-id="ae8b8-276">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="ae8b8-276">QUERY_STRING</span></span>
* <span data-ttu-id="ae8b8-277">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="ae8b8-277">REMOTE_ADDR</span></span>
* <span data-ttu-id="ae8b8-278">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="ae8b8-278">REMOTE_PORT</span></span>
* <span data-ttu-id="ae8b8-279">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="ae8b8-279">REQUEST_FILENAME</span></span>
* <span data-ttu-id="ae8b8-280">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="ae8b8-280">REQUEST_METHOD</span></span>
* <span data-ttu-id="ae8b8-281">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="ae8b8-281">REQUEST_SCHEME</span></span>
* <span data-ttu-id="ae8b8-282">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="ae8b8-282">REQUEST_URI</span></span>
* <span data-ttu-id="ae8b8-283">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="ae8b8-283">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="ae8b8-284">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="ae8b8-284">SERVER_ADDR</span></span>
* <span data-ttu-id="ae8b8-285">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="ae8b8-285">SERVER_PORT</span></span>
* <span data-ttu-id="ae8b8-286">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="ae8b8-286">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="ae8b8-287">TIME</span><span class="sxs-lookup"><span data-stu-id="ae8b8-287">TIME</span></span>
* <span data-ttu-id="ae8b8-288">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="ae8b8-288">TIME_DAY</span></span>
* <span data-ttu-id="ae8b8-289">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="ae8b8-289">TIME_HOUR</span></span>
* <span data-ttu-id="ae8b8-290">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="ae8b8-290">TIME_MIN</span></span>
* <span data-ttu-id="ae8b8-291">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="ae8b8-291">TIME_MON</span></span>
* <span data-ttu-id="ae8b8-292">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="ae8b8-292">TIME_SEC</span></span>
* <span data-ttu-id="ae8b8-293">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="ae8b8-293">TIME_WDAY</span></span>
* <span data-ttu-id="ae8b8-294">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="ae8b8-294">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="ae8b8-295">Правила модуля переопределения URL-адресов для IIS</span><span class="sxs-lookup"><span data-stu-id="ae8b8-295">IIS URL Rewrite Module rules</span></span>

<span data-ttu-id="ae8b8-296">Чтобы использовать правила, применяемые к модулю переопределения URL-адресов для IIS, используйте `AddIISUrlRewrite`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-296">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="ae8b8-297">Убедитесь, что файл правил развертывается вместе с приложением.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-297">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="ae8b8-298">Не направляйте ПО промежуточного слоя для использования вашего файла *web.config* при работе в службах IIS Windows Server.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-298">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="ae8b8-299">В IIS эти правила нужно хранить за пределами *web.config*, чтобы предотвратить конфликты с модулем переопределения для IIS.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-299">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="ae8b8-300">Дополнительные сведения и примеры правил модуля переопределения URL-адресов для IIS см. в разделах [Использование модуля переопределения URL-адресов 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) и [Справочник по конфигурации модуля переопределения URL-адресов](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span><span class="sxs-lookup"><span data-stu-id="ae8b8-300">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ae8b8-301">`StreamReader` используется для чтения правил из файла *IISUrlRewrite.xml*.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-301">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ae8b8-302">Первый параметр принимает `IFileProvider`, а второй является путем к файлу правил XML (в данном примере приложения это *IISUrlRewrite.xml*).</span><span class="sxs-lookup"><span data-stu-id="ae8b8-302">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="ae8b8-303">Пример приложения переопределяет запросы с `/iis-rules-rewrite/(.*)` на `/rewritten?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-303">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="ae8b8-304">Клиенту отправляется отклик с кодом состояния "200 (ОК)".</span><span class="sxs-lookup"><span data-stu-id="ae8b8-304">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

<span data-ttu-id="ae8b8-305">Исходный запрос: `/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="ae8b8-305">Original Request: `/iis-rules-rewrite/1234`</span></span>

![Окно браузера, в котором средства разработчика отслеживают запрос и отклик](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="ae8b8-307">Если имеется активный модуль переопределения для IIS, где настроены правила брандмауэра уровня сервера, способные негативно повлиять на ваше приложение, можно отключить этот модуль для приложения.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-307">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="ae8b8-308">Дополнительные сведения см. в разделе [Отключение модулей IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="ae8b8-308">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="ae8b8-309">Неподдерживаемые функции</span><span class="sxs-lookup"><span data-stu-id="ae8b8-309">Unsupported features</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ae8b8-310">ПО промежуточного слоя, выпущенное вместе с ASP.NET Core 2.x, не поддерживает следующие функции в модуле переопределения URL-адресов для IIS:</span><span class="sxs-lookup"><span data-stu-id="ae8b8-310">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="ae8b8-311">Правила для исходящих подключений</span><span class="sxs-lookup"><span data-stu-id="ae8b8-311">Outbound Rules</span></span>
* <span data-ttu-id="ae8b8-312">Пользовательские переменные сервера</span><span class="sxs-lookup"><span data-stu-id="ae8b8-312">Custom Server Variables</span></span>
* <span data-ttu-id="ae8b8-313">Знаки подстановки</span><span class="sxs-lookup"><span data-stu-id="ae8b8-313">Wildcards</span></span>
* <span data-ttu-id="ae8b8-314">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="ae8b8-314">LogRewrittenUrl</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ae8b8-315">ПО промежуточного слоя, выпущенное вместе с ASP.NET Core 1.x, не поддерживает следующие функции в модуле переопределения URL-адресов для IIS:</span><span class="sxs-lookup"><span data-stu-id="ae8b8-315">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="ae8b8-316">Глобальные правила</span><span class="sxs-lookup"><span data-stu-id="ae8b8-316">Global Rules</span></span>
* <span data-ttu-id="ae8b8-317">Правила для исходящих подключений</span><span class="sxs-lookup"><span data-stu-id="ae8b8-317">Outbound Rules</span></span>
* <span data-ttu-id="ae8b8-318">Сопоставления переопределения</span><span class="sxs-lookup"><span data-stu-id="ae8b8-318">Rewrite Maps</span></span>
* <span data-ttu-id="ae8b8-319">Действие CustomResponse</span><span class="sxs-lookup"><span data-stu-id="ae8b8-319">CustomResponse Action</span></span>
* <span data-ttu-id="ae8b8-320">Пользовательские переменные сервера</span><span class="sxs-lookup"><span data-stu-id="ae8b8-320">Custom Server Variables</span></span>
* <span data-ttu-id="ae8b8-321">Знаки подстановки</span><span class="sxs-lookup"><span data-stu-id="ae8b8-321">Wildcards</span></span>
* <span data-ttu-id="ae8b8-322">Action:CustomResponse</span><span class="sxs-lookup"><span data-stu-id="ae8b8-322">Action:CustomResponse</span></span>
* <span data-ttu-id="ae8b8-323">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="ae8b8-323">LogRewrittenUrl</span></span>

::: moniker-end

#### <a name="supported-server-variables"></a><span data-ttu-id="ae8b8-324">Поддерживаемые переменные сервера</span><span class="sxs-lookup"><span data-stu-id="ae8b8-324">Supported server variables</span></span>

<span data-ttu-id="ae8b8-325">ПО промежуточного слоя поддерживает следующие переменные сервера в модуле переопределения URL-адресов для IIS:</span><span class="sxs-lookup"><span data-stu-id="ae8b8-325">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>

* <span data-ttu-id="ae8b8-326">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="ae8b8-326">CONTENT_LENGTH</span></span>
* <span data-ttu-id="ae8b8-327">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="ae8b8-327">CONTENT_TYPE</span></span>
* <span data-ttu-id="ae8b8-328">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="ae8b8-328">HTTP_ACCEPT</span></span>
* <span data-ttu-id="ae8b8-329">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="ae8b8-329">HTTP_CONNECTION</span></span>
* <span data-ttu-id="ae8b8-330">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="ae8b8-330">HTTP_COOKIE</span></span>
* <span data-ttu-id="ae8b8-331">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="ae8b8-331">HTTP_HOST</span></span>
* <span data-ttu-id="ae8b8-332">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="ae8b8-332">HTTP_REFERER</span></span>
* <span data-ttu-id="ae8b8-333">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="ae8b8-333">HTTP_URL</span></span>
* <span data-ttu-id="ae8b8-334">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="ae8b8-334">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="ae8b8-335">HTTPS</span><span class="sxs-lookup"><span data-stu-id="ae8b8-335">HTTPS</span></span>
* <span data-ttu-id="ae8b8-336">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="ae8b8-336">LOCAL_ADDR</span></span>
* <span data-ttu-id="ae8b8-337">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="ae8b8-337">QUERY_STRING</span></span>
* <span data-ttu-id="ae8b8-338">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="ae8b8-338">REMOTE_ADDR</span></span>
* <span data-ttu-id="ae8b8-339">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="ae8b8-339">REMOTE_PORT</span></span>
* <span data-ttu-id="ae8b8-340">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="ae8b8-340">REQUEST_FILENAME</span></span>
* <span data-ttu-id="ae8b8-341">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="ae8b8-341">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="ae8b8-342">Можно также получить `IFileProvider` через `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-342">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="ae8b8-343">Такой подход позволяет более гибко задавать расположение для файлов с правилами переопределения.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-343">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="ae8b8-344">Убедитесь, что эти файлы развертываются на сервере по указанному пути.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-344">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="ae8b8-345">Правило, основанное на методе</span><span class="sxs-lookup"><span data-stu-id="ae8b8-345">Method-based rule</span></span>

<span data-ttu-id="ae8b8-346">Используйте `Add(Action<RewriteContext> applyRule)`, чтобы реализовать собственную логику правил в методе.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-346">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="ae8b8-347">`RewriteContext` предоставляет `HttpContext` для использования в методе.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-347">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="ae8b8-348">`RewriteContext.Result` определяет, как осуществляется дополнительная обработка в конвейере.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-348">The `RewriteContext.Result` determines how additional pipeline processing is handled.</span></span>

| `RewriteContext.Result`              | <span data-ttu-id="ae8b8-349">Действие</span><span class="sxs-lookup"><span data-stu-id="ae8b8-349">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="ae8b8-350">`RuleResult.ContinueRules` (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="ae8b8-350">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="ae8b8-351">Продолжение применения правил</span><span class="sxs-lookup"><span data-stu-id="ae8b8-351">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="ae8b8-352">Остановка применения правил и отправка отклика</span><span class="sxs-lookup"><span data-stu-id="ae8b8-352">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="ae8b8-353">Остановка применения правил и отправка контекста в следующий компонент ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="ae8b8-353">Stop applying rules and send the context to the next middleware</span></span> |

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="ae8b8-354">Пример приложения демонстрирует метод, который перенаправляет запросы для путей, заканчивающихся на *.xml*.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-354">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="ae8b8-355">Если сделать запрос `/file.xml`, он перенаправляется в `/xmlfiles/file.xml`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-355">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="ae8b8-356">Для кода состояния задается значение "301 (перемещен окончательно)".</span><span class="sxs-lookup"><span data-stu-id="ae8b8-356">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="ae8b8-357">Для перенаправления нужно явно задать код состояния ответа, в противном случае возвращается код состояния "200 (ОК)" и перенаправление для клиента не выполняется.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-357">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

<span data-ttu-id="ae8b8-358">Исходный запрос: `/file.xml`</span><span class="sxs-lookup"><span data-stu-id="ae8b8-358">Original Request: `/file.xml`</span></span>

![Окно браузера, в котором средства разработчика отслеживают запросы и отклики для file.xml](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="ae8b8-360">Правило, основанное на IRule</span><span class="sxs-lookup"><span data-stu-id="ae8b8-360">IRule-based rule</span></span>

<span data-ttu-id="ae8b8-361">Используйте `Add(IRule)` для инкапсуляции собственной логики правил в класс, реализующий интерфейс `IRule`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-361">Use `Add(IRule)` to encapsulate your own rule logic in a class that implements the `IRule` interface.</span></span> <span data-ttu-id="ae8b8-362">Использование `IRule` позволяет более гибко применять правила, основанные на методах.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-362">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="ae8b8-363">Класс реализации может включать конструктор, куда можно передать параметры для метода `ApplyRule`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-363">Your implementaion class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="ae8b8-364">Значения параметров в примере приложения для `extension` и `newPath` проверяются на соответствие нескольким условиям.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-364">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="ae8b8-365">`extension` должен содержать одно из значений: *.png*, *.jpg* или *.gif*.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-365">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="ae8b8-366">Если `newPath` не является допустимым, возникает исключение `ArgumentException`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-366">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="ae8b8-367">Если сделать запрос *image.png*, он перенаправляется в `/png-images/image.png`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-367">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="ae8b8-368">Если сделать запрос *image.jpg*, он перенаправляется в `/jpg-images/image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-368">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="ae8b8-369">Для кода состояния устанавливается значение "301 (перемещен окончательно)", а также задается `context.Result`, чтобы остановить обработку и отправить отклик.</span><span class="sxs-lookup"><span data-stu-id="ae8b8-369">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

<span data-ttu-id="ae8b8-370">Исходный запрос: `/image.png`</span><span class="sxs-lookup"><span data-stu-id="ae8b8-370">Original Request: `/image.png`</span></span>

![Окно браузера, в котором средства разработчика отслеживают запросы и отклики для image.png](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="ae8b8-372">Исходный запрос: `/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="ae8b8-372">Original Request: `/image.jpg`</span></span>

![Окно браузера, в котором средства разработчика отслеживают запросы и отклики для image.jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="ae8b8-374">Примеры регулярных выражений</span><span class="sxs-lookup"><span data-stu-id="ae8b8-374">Regex examples</span></span>

| <span data-ttu-id="ae8b8-375">Goal</span><span class="sxs-lookup"><span data-stu-id="ae8b8-375">Goal</span></span> | <span data-ttu-id="ae8b8-376">Пример строки регулярного выражения</span><span class="sxs-lookup"><span data-stu-id="ae8b8-376">Regex String &</span></span><br><span data-ttu-id="ae8b8-377">и совпадения</span><span class="sxs-lookup"><span data-stu-id="ae8b8-377">Match Example</span></span> | <span data-ttu-id="ae8b8-378">Пример строки замены и</span><span class="sxs-lookup"><span data-stu-id="ae8b8-378">Replacement String &</span></span><br><span data-ttu-id="ae8b8-379">выходных данных</span><span class="sxs-lookup"><span data-stu-id="ae8b8-379">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="ae8b8-380">Переопределение пути в строке запроса</span><span class="sxs-lookup"><span data-stu-id="ae8b8-380">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="ae8b8-381">Удаление косой черты в конце</span><span class="sxs-lookup"><span data-stu-id="ae8b8-381">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="ae8b8-382">Добавление косой черты в конце</span><span class="sxs-lookup"><span data-stu-id="ae8b8-382">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="ae8b8-383">Запрет переопределения отдельных запросов</span><span class="sxs-lookup"><span data-stu-id="ae8b8-383">Avoid rewriting specific requests</span></span> | <span data-ttu-id="ae8b8-384">`^(.*)(?<!\.axd)$` или `^(?!.*\.axd$)(.*)$`</span><span class="sxs-lookup"><span data-stu-id="ae8b8-384">`^(.*)(?<!\.axd)$` or `^(?!.*\.axd$)(.*)$`</span></span><br><span data-ttu-id="ae8b8-385">Да: `/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="ae8b8-385">Yes: `/resource.htm`</span></span><br><span data-ttu-id="ae8b8-386">Нет: `/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="ae8b8-386">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="ae8b8-387">Переупорядочение сегментов URL-адреса</span><span class="sxs-lookup"><span data-stu-id="ae8b8-387">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="ae8b8-388">Замена сегмента URL-адреса</span><span class="sxs-lookup"><span data-stu-id="ae8b8-388">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="ae8b8-389">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ae8b8-389">Additional resources</span></span>

* [<span data-ttu-id="ae8b8-390">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="ae8b8-390">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="ae8b8-391">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="ae8b8-391">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="ae8b8-392">Регулярные выражения в .NET</span><span class="sxs-lookup"><span data-stu-id="ae8b8-392">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="ae8b8-393">Элементы языка регулярных выражений — краткий справочник</span><span class="sxs-lookup"><span data-stu-id="ae8b8-393">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="ae8b8-394">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="ae8b8-394">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="ae8b8-395">Использование модуля переопределения URL-адресов 2.0 (для IIS)</span><span class="sxs-lookup"><span data-stu-id="ae8b8-395">Using Url Rewrite Module 2.0 (for IIS)</span></span>](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [<span data-ttu-id="ae8b8-396">Справочник по конфигурации модуля переопределения URL-адресов</span><span class="sxs-lookup"><span data-stu-id="ae8b8-396">URL Rewrite Module Configuration Reference</span></span>](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [<span data-ttu-id="ae8b8-397">Форум по модулю переопределения URL-адресов для IIS</span><span class="sxs-lookup"><span data-stu-id="ae8b8-397">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* [<span data-ttu-id="ae8b8-398">Сохранение простой структуры URL-адресов</span><span class="sxs-lookup"><span data-stu-id="ae8b8-398">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* [<span data-ttu-id="ae8b8-399">10 советов по переопределению URL-адресов</span><span class="sxs-lookup"><span data-stu-id="ae8b8-399">10 URL Rewriting Tips and Tricks</span></span>](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [<span data-ttu-id="ae8b8-400">Аспекты использования косой черты</span><span class="sxs-lookup"><span data-stu-id="ae8b8-400">To slash or not to slash</span></span>](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
