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
ms.openlocfilehash: 0509bebe430c6ba213031a2cb7cb91bb7a39566d
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2018
---
# <a name="enforce-https-in-an-aspnet-core"></a><span data-ttu-id="b7275-103">Применять HTTPS в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b7275-103">Enforce HTTPS in an ASP.NET Core</span></span>

<span data-ttu-id="b7275-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="b7275-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b7275-105">В этом документе показано, как:</span><span class="sxs-lookup"><span data-stu-id="b7275-105">This document shows how to:</span></span>

- <span data-ttu-id="b7275-106">Требуйте использования протокола HTTPS для всех запросов.</span><span class="sxs-lookup"><span data-stu-id="b7275-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="b7275-107">Перенаправление всех запросов HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b7275-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="b7275-108">Сделать **не** использовать `RequireHttpsAttribute` на веб-API, получать конфиденциальные сведения.</span><span class="sxs-lookup"><span data-stu-id="b7275-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="b7275-109">`RequireHttpsAttribute` использует коды состояния HTTP для перенаправления обозревателей с HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b7275-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="b7275-110">Клиенты API не может понять или подчиняются перенаправлений с HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b7275-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="b7275-111">Такие клиенты могут отправлять сведения по протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="b7275-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="b7275-112">Веб-API должен:</span><span class="sxs-lookup"><span data-stu-id="b7275-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="b7275-113">Прослушивает HTTP.</span><span class="sxs-lookup"><span data-stu-id="b7275-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="b7275-114">Закрывает соединение с кодом состояния 400 (неправильный запрос) и обслуживает запрос.</span><span class="sxs-lookup"><span data-stu-id="b7275-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="b7275-115">Требовать использования протокола HTTPS</span><span class="sxs-lookup"><span data-stu-id="b7275-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE[](~/includes/2.1.md)]

<span data-ttu-id="b7275-117">Корпорация Майкрософт рекомендует всем ASP.NET Core веб-приложений вызов `UseHttpsRedirection` для перенаправления всех запросов HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b7275-117">We recommend all ASP.NET Core web apps call `UseHttpsRedirection` to redirect all HTTP requests to HTTPS.</span></span> <span data-ttu-id="b7275-118">Если `UseHsts` вызывается в приложении, он должен быть вызван перед `UseHttpsRedirection`.</span><span class="sxs-lookup"><span data-stu-id="b7275-118">If `UseHsts` is called in the app, it must be called before `UseHttpsRedirection`.</span></span>

<span data-ttu-id="b7275-119">Следующий код вызывает `UseHttpsRedirection` в `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="b7275-119">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="b7275-120">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="b7275-120">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>


<span data-ttu-id="b7275-121">В приведенном ниже коде</span><span class="sxs-lookup"><span data-stu-id="b7275-121">The following code:</span></span>

<span data-ttu-id="b7275-122">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="b7275-122">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

* <span data-ttu-id="b7275-123">Наборы `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="b7275-123">Sets `RedirectStatusCode`.</span></span>
* <span data-ttu-id="b7275-124">Задает 5001 HTTPS-порт.</span><span class="sxs-lookup"><span data-stu-id="b7275-124">Sets the HTTPS port to 5001.</span></span>

::: moniker-end


::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="b7275-125">[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) используется для обязательного использования протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b7275-125">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="b7275-126">`[RequireHttpsAttribute]` можно дополнить контроллеров или методы или могут быть применены глобально.</span><span class="sxs-lookup"><span data-stu-id="b7275-126">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="b7275-127">Чтобы применить атрибут глобально, добавьте следующий код в `ConfigureServices` в `Startup`:</span><span class="sxs-lookup"><span data-stu-id="b7275-127">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="b7275-128">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="b7275-128">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="b7275-129">Предыдущий выделенный код требует использовать все запросы `HTTPS`; таким образом, HTTP-запросы учитываются.</span><span class="sxs-lookup"><span data-stu-id="b7275-129">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="b7275-130">Следующий выделенный код перенаправляет все запросы HTTP на HTTPS:</span><span class="sxs-lookup"><span data-stu-id="b7275-130">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="b7275-131">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="b7275-131">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="b7275-132">Дополнительные сведения см. в разделе [по промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="b7275-132">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="b7275-133">Требования HTTPS глобально (`options.Filters.Add(new RequireHttpsAttribute());`) является рекомендации по безопасности.</span><span class="sxs-lookup"><span data-stu-id="b7275-133">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="b7275-134">Применение `[RequireHttps]` атрибут ко всем страницам контроллеры и Razor не считается защищено настолько, насколько требования HTTPS глобально.</span><span class="sxs-lookup"><span data-stu-id="b7275-134">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="b7275-135">Не может гарантировать `[RequireHttps]` атрибут при добавлении новых контроллеров и страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="b7275-135">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="b7275-136">Безопасность Strict транспортный протокол HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="b7275-136">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="b7275-137">На [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [строгой безопасности транспорта (HSTS) HTTP](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) — расширение согласиться на использование безопасности, определяемый веб-приложения с помощью специальных заголовка.</span><span class="sxs-lookup"><span data-stu-id="b7275-137">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="b7275-138">Когда этот заголовок получает поддерживаемого браузера браузера позволит обмен данными с передачей по протоколу HTTP к указанному домену и вместо этого будет отправлять все связи по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b7275-138">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="b7275-139">Он также препятствует щелкните HTTPS через запросы в браузерах.</span><span class="sxs-lookup"><span data-stu-id="b7275-139">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="b7275-140">ASP.NET Core Предварительная версия 1 инструментов 2.1 или более поздней версии реализует HSTS с `UseHsts` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="b7275-140">ASP.NET Core 2.1 preview1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="b7275-141">Следующий код вызывает `UseHsts` при это приложение не в [режим разработки](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="b7275-141">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="b7275-142">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="b7275-142">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="b7275-143">`UseHsts` не рекомендуется при разработке решений, так как заголовок HSTS высокой может быть кэширован в браузерах.</span><span class="sxs-lookup"><span data-stu-id="b7275-143">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="b7275-144">По умолчанию UseHsts исключает локальный петлевой адрес.</span><span class="sxs-lookup"><span data-stu-id="b7275-144">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="b7275-145">В приведенном ниже коде</span><span class="sxs-lookup"><span data-stu-id="b7275-145">The following code:</span></span>

<span data-ttu-id="b7275-146">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="b7275-146">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="b7275-147">Задает параметр предварительной загрузки заголовка безопасности Strict-транспорта.</span><span class="sxs-lookup"><span data-stu-id="b7275-147">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="b7275-148">Предварительная загрузка не частью [спецификации RFC HSTS](https://tools.ietf.org/html/rfc6797), но поддерживается для веб-браузера для предварительной загрузки HSTS узлов на установку.</span><span class="sxs-lookup"><span data-stu-id="b7275-148">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="b7275-149">Дополнительные сведения см. в разделе [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="b7275-149">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="b7275-150">Включает [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), который применяет политику HSTS дочерних узлов.</span><span class="sxs-lookup"><span data-stu-id="b7275-150">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="b7275-151">Явно задает для параметра max-age заголовка безопасности для транспорта Strict до 60 дней.</span><span class="sxs-lookup"><span data-stu-id="b7275-151">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="b7275-152">Если свойство не задано, по умолчанию используется значение 30 дней.</span><span class="sxs-lookup"><span data-stu-id="b7275-152">If not set, defaults to 30 days.</span></span> <span data-ttu-id="b7275-153">В разделе [директива max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="b7275-153">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="b7275-154">Добавляет `example.com` в список узлов для исключения.</span><span class="sxs-lookup"><span data-stu-id="b7275-154">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="b7275-155">`UseHsts` исключает замыкания на себя следующие узлы:</span><span class="sxs-lookup"><span data-stu-id="b7275-155">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="b7275-156">`localhost` : IPv4-адрес замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="b7275-156">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="b7275-157">`127.0.0.1` : IPv4-адрес замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="b7275-157">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="b7275-158">`[::1]` : Петлевой адрес IPv6.</span><span class="sxs-lookup"><span data-stu-id="b7275-158">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="b7275-159">Предыдущий пример демонстрирует добавление дополнительных узлов.</span><span class="sxs-lookup"><span data-stu-id="b7275-159">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end


::: moniker range=">= aspnetcore-2.1"
<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="b7275-160">Отказаться от HTTPS на создания проекта</span><span class="sxs-lookup"><span data-stu-id="b7275-160">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="b7275-161">Включить ASP.NET Core 2.1 и более поздних версий шаблонов веб-приложений (из Visual Studio или в командной строке dotnet) [перенаправления HTTPS](#require) и [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="b7275-161">The ASP.NET Core 2.1 and later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="b7275-162">Для развертываний, не требующие HTTPS вы можете отказаться от HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b7275-162">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="b7275-163">Например некоторые серверных служб, где HTTPS обрабатывается извне на границе с помощью протокола HTTPS на каждом узле не требуется.</span><span class="sxs-lookup"><span data-stu-id="b7275-163">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="b7275-164">Чтобы отказаться от HTTPS:</span><span class="sxs-lookup"><span data-stu-id="b7275-164">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b7275-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b7275-165">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="b7275-166">Снимите флажок **настроить для использования протокола HTTPS** флажок.</span><span class="sxs-lookup"><span data-stu-id="b7275-166">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Схема сущностей](enforcing-ssl/_static/out.png)


#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b7275-168">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="b7275-168">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="b7275-169">Использовать параметр `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="b7275-169">Use the `--no-https` option.</span></span> <span data-ttu-id="b7275-170">Пример</span><span class="sxs-lookup"><span data-stu-id="b7275-170">For example</span></span>

```cli
dotnet new razor --no-https
```

------

::: moniker-end