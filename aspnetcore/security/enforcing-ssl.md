---
title: Применять HTTPS в ASP.NET Core
author: rick-anderson
description: Показано, как требуется HTTPS/TLS в ASP.NET Core веб-приложения.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 0433ddb3bf1ef0074c683903ad4553cd6a0b4741
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/31/2018
ms.locfileid: "34687822"
---
# <a name="enforce-https-in-an-aspnet-core"></a><span data-ttu-id="62297-103">Применять HTTPS в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="62297-103">Enforce HTTPS in an ASP.NET Core</span></span>

<span data-ttu-id="62297-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="62297-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="62297-105">В этом документе показано, как:</span><span class="sxs-lookup"><span data-stu-id="62297-105">This document shows how to:</span></span>

- <span data-ttu-id="62297-106">Требуйте использования протокола HTTPS для всех запросов.</span><span class="sxs-lookup"><span data-stu-id="62297-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="62297-107">Перенаправление всех запросов HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="62297-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="62297-108">Сделать **не** использовать `RequireHttpsAttribute` на веб-API, получать конфиденциальные сведения.</span><span class="sxs-lookup"><span data-stu-id="62297-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="62297-109">`RequireHttpsAttribute` использует коды состояния HTTP для перенаправления обозревателей с HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="62297-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="62297-110">Клиенты API не может понять или подчиняются перенаправлений с HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="62297-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="62297-111">Такие клиенты могут отправлять сведения по протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="62297-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="62297-112">Веб-API должен:</span><span class="sxs-lookup"><span data-stu-id="62297-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="62297-113">Прослушивает HTTP.</span><span class="sxs-lookup"><span data-stu-id="62297-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="62297-114">Закрывает соединение с кодом состояния 400 (неправильный запрос) и обслуживает запрос.</span><span class="sxs-lookup"><span data-stu-id="62297-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="62297-115">Требовать использования протокола HTTPS</span><span class="sxs-lookup"><span data-stu-id="62297-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="62297-116">Корпорация Майкрософт рекомендует всем ASP.NET Core веб-приложений вызов `UseHttpsRedirection` для перенаправления всех запросов HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="62297-116">We recommend all ASP.NET Core web apps call `UseHttpsRedirection` to redirect all HTTP requests to HTTPS.</span></span> <span data-ttu-id="62297-117">Если `UseHsts` вызывается в приложении, он должен быть вызван перед `UseHttpsRedirection`.</span><span class="sxs-lookup"><span data-stu-id="62297-117">If `UseHsts` is called in the app, it must be called before `UseHttpsRedirection`.</span></span>

<span data-ttu-id="62297-118">Следующий код вызывает `UseHttpsRedirection` в `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="62297-118">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="62297-119">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="62297-119">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>


<span data-ttu-id="62297-120">В приведенном ниже коде</span><span class="sxs-lookup"><span data-stu-id="62297-120">The following code:</span></span>

<span data-ttu-id="62297-121">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="62297-121">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

* <span data-ttu-id="62297-122">Наборы `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="62297-122">Sets `RedirectStatusCode`.</span></span>
* <span data-ttu-id="62297-123">Задает 5001 HTTPS-порт.</span><span class="sxs-lookup"><span data-stu-id="62297-123">Sets the HTTPS port to 5001.</span></span>

::: moniker-end


::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="62297-124">[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) используется для обязательного использования протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="62297-124">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="62297-125">`[RequireHttpsAttribute]` можно дополнить контроллеров или методы или могут быть применены глобально.</span><span class="sxs-lookup"><span data-stu-id="62297-125">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="62297-126">Чтобы применить атрибут глобально, добавьте следующий код в `ConfigureServices` в `Startup`:</span><span class="sxs-lookup"><span data-stu-id="62297-126">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="62297-127">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="62297-127">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="62297-128">Предыдущий выделенный код требует использовать все запросы `HTTPS`; таким образом, HTTP-запросы учитываются.</span><span class="sxs-lookup"><span data-stu-id="62297-128">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="62297-129">Следующий выделенный код перенаправляет все запросы HTTP на HTTPS:</span><span class="sxs-lookup"><span data-stu-id="62297-129">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="62297-130">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="62297-130">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="62297-131">Дополнительные сведения см. в разделе [по промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="62297-131">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="62297-132">Требования HTTPS глобально (`options.Filters.Add(new RequireHttpsAttribute());`) является рекомендации по безопасности.</span><span class="sxs-lookup"><span data-stu-id="62297-132">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="62297-133">Применение `[RequireHttps]` атрибут ко всем страницам контроллеры и Razor не считается защищено настолько, насколько требования HTTPS глобально.</span><span class="sxs-lookup"><span data-stu-id="62297-133">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="62297-134">Не может гарантировать `[RequireHttps]` атрибут при добавлении новых контроллеров и страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="62297-134">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="62297-135">Безопасность Strict транспортный протокол HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="62297-135">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="62297-136">На [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [строгой безопасности транспорта (HSTS) HTTP](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) — расширение согласиться на использование безопасности, определяемый веб-приложения с помощью специальных заголовка.</span><span class="sxs-lookup"><span data-stu-id="62297-136">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="62297-137">Когда этот заголовок получает поддерживаемого браузера браузера позволит обмен данными с передачей по протоколу HTTP к указанному домену и вместо этого будет отправлять все связи по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="62297-137">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="62297-138">Он также препятствует щелкните HTTPS через запросы в браузерах.</span><span class="sxs-lookup"><span data-stu-id="62297-138">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="62297-139">ASP.NET Core 2.1 или более поздней реализует HSTS с `UseHsts` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="62297-139">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="62297-140">Следующий код вызывает `UseHsts` при это приложение не в [режим разработки](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="62297-140">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="62297-141">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="62297-141">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="62297-142">`UseHsts` не рекомендуется при разработке решений, так как заголовок HSTS высокой может быть кэширован в браузерах.</span><span class="sxs-lookup"><span data-stu-id="62297-142">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="62297-143">По умолчанию UseHsts исключает локальный петлевой адрес.</span><span class="sxs-lookup"><span data-stu-id="62297-143">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="62297-144">В приведенном ниже коде</span><span class="sxs-lookup"><span data-stu-id="62297-144">The following code:</span></span>

<span data-ttu-id="62297-145">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="62297-145">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="62297-146">Задает параметр предварительной загрузки заголовка безопасности Strict-транспорта.</span><span class="sxs-lookup"><span data-stu-id="62297-146">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="62297-147">Предварительная загрузка не частью [спецификации RFC HSTS](https://tools.ietf.org/html/rfc6797), но поддерживается для веб-браузера для предварительной загрузки HSTS узлов на установку.</span><span class="sxs-lookup"><span data-stu-id="62297-147">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="62297-148">Дополнительные сведения см. в разделе [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="62297-148">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="62297-149">Включает [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), который применяет политику HSTS дочерних узлов.</span><span class="sxs-lookup"><span data-stu-id="62297-149">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="62297-150">Явно задает для параметра max-age заголовка безопасности для транспорта Strict до 60 дней.</span><span class="sxs-lookup"><span data-stu-id="62297-150">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="62297-151">Если свойство не задано, по умолчанию используется значение 30 дней.</span><span class="sxs-lookup"><span data-stu-id="62297-151">If not set, defaults to 30 days.</span></span> <span data-ttu-id="62297-152">В разделе [директива max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="62297-152">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="62297-153">Добавляет `example.com` в список узлов для исключения.</span><span class="sxs-lookup"><span data-stu-id="62297-153">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="62297-154">`UseHsts` исключает замыкания на себя следующие узлы:</span><span class="sxs-lookup"><span data-stu-id="62297-154">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="62297-155">`localhost` : IPv4-адрес замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="62297-155">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="62297-156">`127.0.0.1` : IPv4-адрес замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="62297-156">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="62297-157">`[::1]` : Петлевой адрес IPv6.</span><span class="sxs-lookup"><span data-stu-id="62297-157">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="62297-158">Предыдущий пример демонстрирует добавление дополнительных узлов.</span><span class="sxs-lookup"><span data-stu-id="62297-158">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end


::: moniker range=">= aspnetcore-2.1"
<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="62297-159">Отказаться от HTTPS на создания проекта</span><span class="sxs-lookup"><span data-stu-id="62297-159">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="62297-160">Включить ASP.NET Core 2.1 и более поздних версий шаблонов веб-приложений (из Visual Studio или в командной строке dotnet) [перенаправления HTTPS](#require) и [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="62297-160">The ASP.NET Core 2.1 and later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="62297-161">Для развертываний, не требующие HTTPS вы можете отказаться от HTTPS.</span><span class="sxs-lookup"><span data-stu-id="62297-161">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="62297-162">Например некоторые серверных служб, где HTTPS обрабатывается извне на границе с помощью протокола HTTPS на каждом узле не требуется.</span><span class="sxs-lookup"><span data-stu-id="62297-162">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="62297-163">Чтобы отказаться от HTTPS:</span><span class="sxs-lookup"><span data-stu-id="62297-163">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="62297-164">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="62297-164">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="62297-165">Снимите флажок **настроить для использования протокола HTTPS** флажок.</span><span class="sxs-lookup"><span data-stu-id="62297-165">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Схема сущностей](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="62297-167">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="62297-167">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="62297-168">Использовать параметр `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="62297-168">Use the `--no-https` option.</span></span> <span data-ttu-id="62297-169">Пример</span><span class="sxs-lookup"><span data-stu-id="62297-169">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="62297-170">Как можно настроить сертификат разработчика для Docker</span><span class="sxs-lookup"><span data-stu-id="62297-170">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="62297-171">В разделе [этой проблемы GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="62297-171">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
