---
title: Реализация веб-сервера HTTP.sys в ASP.NET Core
author: guardrex
description: Общие сведения о веб-сервере HTTP.sys для ASP.NET Core в Windows. Веб-сервер HTTP.sys на основе работающего в режиме ядра драйвера Http.Sys — это альтернатива Kestrel, которую можно использовать для прямого подключения к Интернету без служб IIS.
monikerRange: '>= aspnetcore-2.0'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/27/2019
uid: fundamentals/servers/httpsys
ms.openlocfilehash: d91a0f7b83fb073147978319edfbed0bda9a1c44
ms.sourcegitcommit: 06c4f2910dd54ded25e1b8750e09c66578748bc9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/30/2019
ms.locfileid: "66395955"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="fd223-104">Реализация веб-сервера HTTP.sys в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fd223-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="fd223-105">Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra), [Крис Росс](https://github.com/Tratcher) (Chris Ross) и [Люк Латам](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="fd223-105">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="fd223-106">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) — это [веб-сервер для ASP.NET Core](xref:fundamentals/servers/index), который запускается только в Windows.</span><span class="sxs-lookup"><span data-stu-id="fd223-106">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) is a [web server for ASP.NET Core](xref:fundamentals/servers/index) that only runs on Windows.</span></span> <span data-ttu-id="fd223-107">HTTP.sys является альтернативой серверу [Kestrel](xref:fundamentals/servers/kestrel), предлагая некоторые функции, отсутствующие в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd223-107">HTTP.sys is an alternative to [Kestrel](xref:fundamentals/servers/kestrel) server and offers some features that Kestrel doesn't provide.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fd223-108">HTTP.sys не подходит для использования с IIS или IIS Express из-за несовместимости с [модулем ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="fd223-108">HTTP.sys isn't compatible with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and can't be used with IIS or IIS Express.</span></span>

<span data-ttu-id="fd223-109">HTTP.sys поддерживает следующие функции:</span><span class="sxs-lookup"><span data-stu-id="fd223-109">HTTP.sys supports the following features:</span></span>

* [<span data-ttu-id="fd223-110">Аутентификация Windows</span><span class="sxs-lookup"><span data-stu-id="fd223-110">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
* <span data-ttu-id="fd223-111">Совместное использование портов</span><span class="sxs-lookup"><span data-stu-id="fd223-111">Port sharing</span></span>
* <span data-ttu-id="fd223-112">Использование HTTPS с SNI</span><span class="sxs-lookup"><span data-stu-id="fd223-112">HTTPS with SNI</span></span>
* <span data-ttu-id="fd223-113">Использование HTTP/2 через TLS (Windows 10 и более поздние версии)</span><span class="sxs-lookup"><span data-stu-id="fd223-113">HTTP/2 over TLS (Windows 10 or later)</span></span>
* <span data-ttu-id="fd223-114">Прямая передача файлов</span><span class="sxs-lookup"><span data-stu-id="fd223-114">Direct file transmission</span></span>
* <span data-ttu-id="fd223-115">Кэширование откликов</span><span class="sxs-lookup"><span data-stu-id="fd223-115">Response caching</span></span>
* <span data-ttu-id="fd223-116">Использование WebSockets (Windows 8 и более поздние версии)</span><span class="sxs-lookup"><span data-stu-id="fd223-116">WebSockets (Windows 8 or later)</span></span>

<span data-ttu-id="fd223-117">Поддерживаемые версии Windows:</span><span class="sxs-lookup"><span data-stu-id="fd223-117">Supported Windows versions:</span></span>

* <span data-ttu-id="fd223-118">Windows 7 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="fd223-118">Windows 7 or later</span></span>
* <span data-ttu-id="fd223-119">Windows Server 2008 R2 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="fd223-119">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="fd223-120">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fd223-120">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="fd223-121">Условия для применения HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="fd223-121">When to use HTTP.sys</span></span>

<span data-ttu-id="fd223-122">HTTP.sys удобно использовать с развертываниями в таких случаях:</span><span class="sxs-lookup"><span data-stu-id="fd223-122">HTTP.sys is useful for deployments where:</span></span>

* <span data-ttu-id="fd223-123">когда нужно подключить сервер к Интернету напрямую без использования служб IIS;</span><span class="sxs-lookup"><span data-stu-id="fd223-123">There's a need to expose the server directly to the Internet without using IIS.</span></span>

  ![HTTP.sys взаимодействует с Интернетом напрямую](httpsys/_static/httpsys-to-internet.png)

* <span data-ttu-id="fd223-125">когда для внутренних развертываний нужна функция, отсутствующая в Kestrel, например [аутентификация Windows](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="fd223-125">An internal deployment requires a feature not available in Kestrel, such as [Windows Authentication](xref:security/authentication/windowsauth).</span></span>

  ![HTTP.sys взаимодействует с внутренней сетью напрямую](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="fd223-127">HTTP.sys — это проверенная технология, которая защищает от многих типов атак, а также обеспечивает надежность, безопасность и масштабируемость полнофункционального веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="fd223-127">HTTP.sys is mature technology that protects against many types of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="fd223-128">Сами службы IIS выполняются в качестве HTTP-прослушивателя поверх HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="fd223-128">IIS itself runs as an HTTP listener on top of HTTP.sys.</span></span>

## <a name="http2-support"></a><span data-ttu-id="fd223-129">Поддержка HTTP/2</span><span class="sxs-lookup"><span data-stu-id="fd223-129">HTTP/2 support</span></span>

<span data-ttu-id="fd223-130">Протокол [HTTP/2](https://httpwg.org/specs/rfc7540.html) включен для приложений ASP.NET Core, если выполнены следующие базовые требования:</span><span class="sxs-lookup"><span data-stu-id="fd223-130">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is enabled for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="fd223-131">установлена ОС Windows Server 2016 либо Windows 10 или более поздних версий;</span><span class="sxs-lookup"><span data-stu-id="fd223-131">Windows Server 2016/Windows 10 or later</span></span>
* <span data-ttu-id="fd223-132">установлено подключение с поддержкой [согласования протокола уровня приложений (ALPN)](https://tools.ietf.org/html/rfc7301#section-3);</span><span class="sxs-lookup"><span data-stu-id="fd223-132">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="fd223-133">установлено подключение TLS 1.2 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="fd223-133">TLS 1.2 or later connection</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="fd223-134">Если установлено подключение HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) возвращает `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="fd223-134">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="fd223-135">Если установлено подключение HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) возвращает `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="fd223-135">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="fd223-136">Протокол HTTP/2 включен по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fd223-136">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="fd223-137">Если не удается установить подключение HTTP/2, применяется резервный вариант HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="fd223-137">If an HTTP/2 connection isn't established, the connection falls back to HTTP/1.1.</span></span> <span data-ttu-id="fd223-138">В будущих версиях Windows будут доступны флаги конфигурации HTTP/2, в том числе возможность отключения HTTP/2 с использованием HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="fd223-138">In a future release of Windows, HTTP/2 configuration flags will be available, including the ability to disable HTTP/2 with HTTP.sys.</span></span>

## <a name="kernel-mode-authentication-with-kerberos"></a><span data-ttu-id="fd223-139">Проверка подлинности в режиме ядра с помощью Kerberos</span><span class="sxs-lookup"><span data-stu-id="fd223-139">Kernel mode authentication with Kerberos</span></span>

<span data-ttu-id="fd223-140">HTTP.sys делегирует задачи в проверку подлинности в режиме ядра с помощью протокола проверки подлинности Kerberos.</span><span class="sxs-lookup"><span data-stu-id="fd223-140">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="fd223-141">Проверка подлинности в режиме пользователя не поддерживается с Kerberos и HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="fd223-141">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="fd223-142">Необходимо использовать учетную запись компьютера для расшифровки маркера/билета Kerberos, полученного из Active Directory и переадресованного клиентом на сервер для проверки подлинности пользователя.</span><span class="sxs-lookup"><span data-stu-id="fd223-142">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="fd223-143">Зарегистрируйте имя субъекта-службы (SPN) для узла, а не пользователя приложения.</span><span class="sxs-lookup"><span data-stu-id="fd223-143">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

## <a name="how-to-use-httpsys"></a><span data-ttu-id="fd223-144">Способы применения HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="fd223-144">How to use HTTP.sys</span></span>

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a><span data-ttu-id="fd223-145">Настройка приложения ASP.NET Core для использования HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="fd223-145">Configure the ASP.NET Core app to use HTTP.sys</span></span>

1. <span data-ttu-id="fd223-146">Ссылка на пакет в файле проекта не требуется при использовании [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (ASP.NET Core 2.1 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="fd223-146">A package reference in the project file isn't required when using the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="fd223-147">Если метапакет `Microsoft.AspNetCore.App` не используется, добавьте ссылку на пакет в файл [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).</span><span class="sxs-lookup"><span data-stu-id="fd223-147">When not using the `Microsoft.AspNetCore.App` metapackage, add a package reference to [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).</span></span>

2. <span data-ttu-id="fd223-148">Вызовите метод расширения <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> при создании веб-узла, указав все необходимые параметры <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>:</span><span class="sxs-lookup"><span data-stu-id="fd223-148">Call the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> extension method when building Web Host, specifying any required <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>:</span></span>

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   <span data-ttu-id="fd223-149">Дополнительная настройка HTTP.sys выполняется с помощью [параметров реестра](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).</span><span class="sxs-lookup"><span data-stu-id="fd223-149">Additional HTTP.sys configuration is handled through [registry settings](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).</span></span>

   <span data-ttu-id="fd223-150">**Параметры HTTP.sys**</span><span class="sxs-lookup"><span data-stu-id="fd223-150">**HTTP.sys options**</span></span>

   | <span data-ttu-id="fd223-151">Свойство</span><span class="sxs-lookup"><span data-stu-id="fd223-151">Property</span></span> | <span data-ttu-id="fd223-152">Описание</span><span class="sxs-lookup"><span data-stu-id="fd223-152">Description</span></span> | <span data-ttu-id="fd223-153">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="fd223-153">Default</span></span> |
   | -------- | ----------- | :-----: |
   | [<span data-ttu-id="fd223-154">AllowSynchronousIO</span><span class="sxs-lookup"><span data-stu-id="fd223-154">AllowSynchronousIO</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | <span data-ttu-id="fd223-155">Указывает, разрешен ли синхронные операции ввода-вывода для `HttpContext.Request.Body` и `HttpContext.Response.Body`.</span><span class="sxs-lookup"><span data-stu-id="fd223-155">Control whether synchronous input/output is allowed for the `HttpContext.Request.Body` and `HttpContext.Response.Body`.</span></span> | `true` |
   | [<span data-ttu-id="fd223-156">Authentication.AllowAnonymous</span><span class="sxs-lookup"><span data-stu-id="fd223-156">Authentication.AllowAnonymous</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | <span data-ttu-id="fd223-157">Разрешает анонимные запросы.</span><span class="sxs-lookup"><span data-stu-id="fd223-157">Allow anonymous requests.</span></span> | `true` |
   | [<span data-ttu-id="fd223-158">Authentication.Schemes</span><span class="sxs-lookup"><span data-stu-id="fd223-158">Authentication.Schemes</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | <span data-ttu-id="fd223-159">Указывает разрешенные схемы аутентификации.</span><span class="sxs-lookup"><span data-stu-id="fd223-159">Specify the allowed authentication schemes.</span></span> <span data-ttu-id="fd223-160">Может быть изменен в любое время до удаления прослушивателя.</span><span class="sxs-lookup"><span data-stu-id="fd223-160">May be modified at any time prior to disposing the listener.</span></span> <span data-ttu-id="fd223-161">Предоставляет значения, полученные при [перечислении AuthenticationSchemes](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None` и `NTLM`.</span><span class="sxs-lookup"><span data-stu-id="fd223-161">Values are provided by the [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None`, and `NTLM`.</span></span> | `None` |
   | [<span data-ttu-id="fd223-162">EnableResponseCaching</span><span class="sxs-lookup"><span data-stu-id="fd223-162">EnableResponseCaching</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | <span data-ttu-id="fd223-163">Выполняет попытку кэшировать [режим ядра](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) для ответов с допустимыми заголовками.</span><span class="sxs-lookup"><span data-stu-id="fd223-163">Attempt [kernel-mode](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) caching for responses with eligible headers.</span></span> <span data-ttu-id="fd223-164">Ответ не может включать заголовки `Set-Cookie`, `Vary` или `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="fd223-164">The response may not include `Set-Cookie`, `Vary`, or `Pragma` headers.</span></span> <span data-ttu-id="fd223-165">Он должен включать заголовок `Cache-Control` со значением `public`, а также значение `shared-max-age` или `max-age` заголовок `Expires`.</span><span class="sxs-lookup"><span data-stu-id="fd223-165">It must include a `Cache-Control` header that's `public` and either a `shared-max-age` or `max-age` value, or an `Expires` header.</span></span> | `true` |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | <span data-ttu-id="fd223-166">Максимальное число одновременных попыток.</span><span class="sxs-lookup"><span data-stu-id="fd223-166">The maximum number of concurrent accepts.</span></span> | <span data-ttu-id="fd223-167">5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span><span class="sxs-lookup"><span data-stu-id="fd223-167">5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | <span data-ttu-id="fd223-168">Максимальное число попыток установить одновременное подключение.</span><span class="sxs-lookup"><span data-stu-id="fd223-168">The maximum number of concurrent connections to accept.</span></span> <span data-ttu-id="fd223-169">Использует `-1` для бесконечных циклов.</span><span class="sxs-lookup"><span data-stu-id="fd223-169">Use `-1` for infinite.</span></span> <span data-ttu-id="fd223-170">Использует `null` для работы с параметром реестра на уровне компьютера.</span><span class="sxs-lookup"><span data-stu-id="fd223-170">Use `null` to use the registry's machine-wide setting.</span></span> | `null`<br><span data-ttu-id="fd223-171">Без ограничений.</span><span class="sxs-lookup"><span data-stu-id="fd223-171">(unlimited)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | <span data-ttu-id="fd223-172">См. раздел <a href="#maxrequestbodysize">MaxRequestBodySize</a>.</span><span class="sxs-lookup"><span data-stu-id="fd223-172">See the <a href="#maxrequestbodysize">MaxRequestBodySize</a> section.</span></span> | <span data-ttu-id="fd223-173">30 000 000 байт.</span><span class="sxs-lookup"><span data-stu-id="fd223-173">30000000 bytes</span></span><br><span data-ttu-id="fd223-174">(~28,6 МБ).</span><span class="sxs-lookup"><span data-stu-id="fd223-174">(~28.6 MB)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | <span data-ttu-id="fd223-175">Максимально допустимое число запросов в очереди.</span><span class="sxs-lookup"><span data-stu-id="fd223-175">The maximum number of requests that can be queued.</span></span> | <span data-ttu-id="fd223-176">1000.</span><span class="sxs-lookup"><span data-stu-id="fd223-176">1000</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | <span data-ttu-id="fd223-177">Указывает, следует ли вызывать исключение или завершать работу нормально, когда запись текста ответа завершается ошибкой из-за отключения клиента.</span><span class="sxs-lookup"><span data-stu-id="fd223-177">Indicate if response body writes that fail due to client disconnects should throw exceptions or complete normally.</span></span> | `false`<br><span data-ttu-id="fd223-178">Нормальное завершение.</span><span class="sxs-lookup"><span data-stu-id="fd223-178">(complete normally)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | <span data-ttu-id="fd223-179">Предоставляет конфигурацию <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> HTTP.sys, которую также можно настроить в реестре.</span><span class="sxs-lookup"><span data-stu-id="fd223-179">Expose the HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> configuration, which may also be configured in the registry.</span></span> <span data-ttu-id="fd223-180">Дополнительные сведения о каждом параметре, включая значения по умолчанию, см. здесь:</span><span class="sxs-lookup"><span data-stu-id="fd223-180">Follow the API links to learn more about each setting, including default values:</span></span><ul><li><span data-ttu-id="fd223-181">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; время, выделенное для API сервера HTTP для очистки текста сущности при активном подключении.</span><span class="sxs-lookup"><span data-stu-id="fd223-181">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; Time allowed for the HTTP Server API to drain the entity body on a Keep-Alive connection.</span></span></li><li><span data-ttu-id="fd223-182">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; время, выделенное на получение текста сущности запроса.</span><span class="sxs-lookup"><span data-stu-id="fd223-182">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; Time allowed for the request entity body to arrive.</span></span></li><li><span data-ttu-id="fd223-183">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; время, выделенное для API сервера HTTP для выполнения синтаксического анализа заголовка запроса.</span><span class="sxs-lookup"><span data-stu-id="fd223-183">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; Time allowed for the HTTP Server API to parse the request header.</span></span></li><li><span data-ttu-id="fd223-184">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; время, выделенное для неактивного подключения.</span><span class="sxs-lookup"><span data-stu-id="fd223-184">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; Time allowed for an idle connection.</span></span></li><li><span data-ttu-id="fd223-185">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; минимальная скорость отправки ответа.</span><span class="sxs-lookup"><span data-stu-id="fd223-185">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; The minimum send rate for the response.</span></span></li><li><span data-ttu-id="fd223-186">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; время, выделенное для пребывания запроса в очереди до его получения приложением.</span><span class="sxs-lookup"><span data-stu-id="fd223-186">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; Time allowed for the request to remain in the request queue before the app picks it up.</span></span></li></ul> |  |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | <span data-ttu-id="fd223-187">Указывает <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> для регистрации с использованием HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="fd223-187">Specify the <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> to register with HTTP.sys.</span></span> <span data-ttu-id="fd223-188">Удобнее всего использовать параметр [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), который добавляет префикс к коллекции.</span><span class="sxs-lookup"><span data-stu-id="fd223-188">The most useful is [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), which is used to add a prefix to the collection.</span></span> <span data-ttu-id="fd223-189">Могут быть изменены в любое время до удаления прослушивателя.</span><span class="sxs-lookup"><span data-stu-id="fd223-189">These may be modified at any time prior to disposing the listener.</span></span> |  |

   <a name="maxrequestbodysize"></a>

   <span data-ttu-id="fd223-190">**MaxRequestBodySize**</span><span class="sxs-lookup"><span data-stu-id="fd223-190">**MaxRequestBodySize**</span></span>

   <span data-ttu-id="fd223-191">Максимально допустимый размер текста запроса в байтах.</span><span class="sxs-lookup"><span data-stu-id="fd223-191">The maximum allowed size of any request body in bytes.</span></span> <span data-ttu-id="fd223-192">Если задано значение `null`, размер максимального запроса не ограничен.</span><span class="sxs-lookup"><span data-stu-id="fd223-192">When set to `null`, the maximum request body size is unlimited.</span></span> <span data-ttu-id="fd223-193">Это ограничение не оказывает влияния на обновленные подключения, которые не имеют ограничений.</span><span class="sxs-lookup"><span data-stu-id="fd223-193">This limit has no effect on upgraded connections, which are always unlimited.</span></span>

   <span data-ttu-id="fd223-194">Чтобы переопределить это ограничение в приложении ASP.NET Core MVC для `IActionResult`, рекомендуется использовать атрибут <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> в методе действия:</span><span class="sxs-lookup"><span data-stu-id="fd223-194">The recommended method to override the limit in an ASP.NET Core MVC app for a single `IActionResult` is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   <span data-ttu-id="fd223-195">При попытке приложения настроить ограничение для запроса после того, как приложение начало считывать запрос, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="fd223-195">An exception is thrown if the app attempts to configure the limit on a request after the app has started reading the request.</span></span> <span data-ttu-id="fd223-196">Свойство `IsReadOnly` указывает на то, что свойство `MaxRequestBodySize` находится в состоянии только для чтения и настраивать ограничение слишком поздно.</span><span class="sxs-lookup"><span data-stu-id="fd223-196">An `IsReadOnly` property can be used to indicate if the `MaxRequestBodySize` property is in a read-only state, meaning it's too late to configure the limit.</span></span>

   <span data-ttu-id="fd223-197">Если приложение должно переопределять <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> по запросу, используйте <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>:</span><span class="sxs-lookup"><span data-stu-id="fd223-197">If the app should override <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> per-request, use the <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>:</span></span>

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

3. <span data-ttu-id="fd223-198">При использовании Visual Studio убедитесь, что приложение не настроено для запуска IIS или IIS Express.</span><span class="sxs-lookup"><span data-stu-id="fd223-198">If using Visual Studio, make sure the app isn't configured to run IIS or IIS Express.</span></span>

   <span data-ttu-id="fd223-199">В Visual Studio профиль запуска по умолчанию использует IIS Express.</span><span class="sxs-lookup"><span data-stu-id="fd223-199">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="fd223-200">Чтобы запустить проект как консольное приложение, измените выбранный профиль вручную, как показано на следующем снимке экрана.</span><span class="sxs-lookup"><span data-stu-id="fd223-200">To run the project as a console app, manually change the selected profile, as shown in the following screen shot:</span></span>

   ![Выбор профиля консольного приложения](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a><span data-ttu-id="fd223-202">Настройка Windows Server</span><span class="sxs-lookup"><span data-stu-id="fd223-202">Configure Windows Server</span></span>

1. <span data-ttu-id="fd223-203">Определите, какие порты нужно открыть для приложения, и используйте [брандмауэр Windows](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) или командлет PowerShell [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule), чтобы открыть порты брандмауэра для доступа трафика к HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="fd223-203">Determine the ports to open for the app and use [Windows Firewall](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) or the [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) PowerShell cmdlet to open firewall ports to allow traffic to reach HTTP.sys.</span></span> <span data-ttu-id="fd223-204">В следующих командах и конфигурации приложения используется порт 443.</span><span class="sxs-lookup"><span data-stu-id="fd223-204">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="fd223-205">При развертывании на виртуальных машинах Azure откройте эти порты в [группе безопасности сети](/azure/virtual-machines/windows/nsg-quickstart-portal).</span><span class="sxs-lookup"><span data-stu-id="fd223-205">When deploying to an Azure VM, open the ports in the [Network Security Group](/azure/virtual-machines/windows/nsg-quickstart-portal).</span></span> <span data-ttu-id="fd223-206">В следующих командах и конфигурации приложения используется порт 443.</span><span class="sxs-lookup"><span data-stu-id="fd223-206">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="fd223-207">При необходимости получите и установите сертификаты X.509.</span><span class="sxs-lookup"><span data-stu-id="fd223-207">Obtain and install X.509 certificates, if required.</span></span>

   <span data-ttu-id="fd223-208">В Windows создайте самозаверяющие сертификаты с помощью [командлета PowerShell New-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="fd223-208">On Windows, create self-signed certificates using the [New-SelfSignedCertificate PowerShell cmdlet](/powershell/module/pkiclient/new-selfsignedcertificate).</span></span> <span data-ttu-id="fd223-209">Примеры, которые не поддерживаются, см. в разделе [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).</span><span class="sxs-lookup"><span data-stu-id="fd223-209">For an unsupported example, see [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).</span></span>

   <span data-ttu-id="fd223-210">Установите самозаверяющие или подписанные центром сертификации сертификаты в хранилище сервера, выбрав **Локальный компьютер** > **Личный**.</span><span class="sxs-lookup"><span data-stu-id="fd223-210">Install either self-signed or CA-signed certificates in the server's **Local Machine** > **Personal** store.</span></span>

1. <span data-ttu-id="fd223-211">Если приложение является [развертыванием, не зависящим от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd), установите NET Core или .NET Framework (или обе платформы, если это приложение .NET Core, предназначенное для .NET Framework).</span><span class="sxs-lookup"><span data-stu-id="fd223-211">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd), install .NET Core, .NET Framework, or both (if the app is a .NET Core app targeting the .NET Framework).</span></span>

   * <span data-ttu-id="fd223-212">**.NET Core** — если приложению требуется .NET Core, скачайте установщик **среды выполнения .NET Core** на странице [скачивания .NET Core](https://dotnet.microsoft.com/download) и запустите его.</span><span class="sxs-lookup"><span data-stu-id="fd223-212">**.NET Core** &ndash; If the app requires .NET Core, obtain and run the **.NET Core Runtime** installer from [.NET Core Downloads](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="fd223-213">Не устанавливайте полный пакет SDK на сервере.</span><span class="sxs-lookup"><span data-stu-id="fd223-213">Don't install the full SDK on the server.</span></span>
   * <span data-ttu-id="fd223-214">**.NET Framework** — если приложению требуется .NET Framework, см. [руководство по установке](/dotnet/framework/install/).</span><span class="sxs-lookup"><span data-stu-id="fd223-214">**.NET Framework** &ndash; If the app requires .NET Framework, see the [.NET Framework installation guide](/dotnet/framework/install/).</span></span> <span data-ttu-id="fd223-215">Установите требуемую платформу .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="fd223-215">Install the required .NET Framework.</span></span> <span data-ttu-id="fd223-216">Установщик последней версии .NET Framework доступен на странице [скачивания .NET](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="fd223-216">The installer for the latest .NET Framework is available from the [.NET Core Downloads](https://dotnet.microsoft.com/download) page.</span></span>

   <span data-ttu-id="fd223-217">Если приложение [развертывается автономно](/dotnet/core/deploying/#framework-dependent-deployments-scd), в его развертывание включена среда выполнения.</span><span class="sxs-lookup"><span data-stu-id="fd223-217">If the app is a [self-contained deployment](/dotnet/core/deploying/#framework-dependent-deployments-scd), the app includes the runtime in its deployment.</span></span> <span data-ttu-id="fd223-218">Устанавливать .NET Framework на сервере не нужно.</span><span class="sxs-lookup"><span data-stu-id="fd223-218">No framework installation is required on the server.</span></span>

1. <span data-ttu-id="fd223-219">Настройте URL-адреса и порты в приложении.</span><span class="sxs-lookup"><span data-stu-id="fd223-219">Configure URLs and ports in the app.</span></span>

   <span data-ttu-id="fd223-220">По умолчанию платформа ASP.NET Core привязана к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="fd223-220">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="fd223-221">Чтобы настроить префиксы URL-адресов и порты, используйте следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="fd223-221">To configure URL prefixes and ports, options include:</span></span>

   * <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
   * <span data-ttu-id="fd223-222">Аргументы командной строки `urls`.</span><span class="sxs-lookup"><span data-stu-id="fd223-222">`urls` command-line argument</span></span>
   * <span data-ttu-id="fd223-223">Переменная среды `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="fd223-223">`ASPNETCORE_URLS` environment variable</span></span>
   * <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes>

   <span data-ttu-id="fd223-224">В следующем примере кода показано, как использовать <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> с локальным IP-адресом сервера `10.0.0.4` через порт 443.</span><span class="sxs-lookup"><span data-stu-id="fd223-224">The following code example shows how to use <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> with the server's local IP address `10.0.0.4` on port 443:</span></span>

   [!code-csharp[](httpsys/sample_snapshot/Program.cs?name=snippet1&highlight=11)]

   <span data-ttu-id="fd223-225">Преимущество `UrlPrefixes` заключается в том, что при неправильном формате префиксов сразу же создается сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="fd223-225">An advantage of `UrlPrefixes` is that an error message is generated immediately for improperly formatted prefixes.</span></span>

   <span data-ttu-id="fd223-226">Этот параметр в `UrlPrefixes` переопределяет параметры `UseUrls`/`urls`/`ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="fd223-226">The settings in `UrlPrefixes` override `UseUrls`/`urls`/`ASPNETCORE_URLS` settings.</span></span> <span data-ttu-id="fd223-227">Таким образом, преимущество переменных среды`UseUrls`, `urls`и `ASPNETCORE_URLS` заключается в возможности быстрого переключения между Kestrel и HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="fd223-227">Therefore, an advantage of `UseUrls`, `urls`, and the `ASPNETCORE_URLS` environment variable is that it's easier to switch between Kestrel and HTTP.sys.</span></span> <span data-ttu-id="fd223-228">Для получения дополнительной информации см. <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="fd223-228">For more information, see <xref:fundamentals/host/web-host>.</span></span>

   <span data-ttu-id="fd223-229">HTTP.sys использует [форматы строк UrlPrefix API сервера HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="fd223-229">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

   > [!WARNING]
   > <span data-ttu-id="fd223-230">**Не используйте** привязки с подстановочными знаками (`http://*:80/` и `http://+:80`) на верхнем уровне.</span><span class="sxs-lookup"><span data-stu-id="fd223-230">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="fd223-231">Они создают уязвимости и ставят под угрозу безопасность приложения.</span><span class="sxs-lookup"><span data-stu-id="fd223-231">Top-level wildcard bindings create app security vulnerabilities.</span></span> <span data-ttu-id="fd223-232">Сюда относятся и строгие, и нестрогие подстановочные знаки.</span><span class="sxs-lookup"><span data-stu-id="fd223-232">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="fd223-233">Вместо подстановочных знаков используйте имена узлов или IP-адреса в явном виде.</span><span class="sxs-lookup"><span data-stu-id="fd223-233">Use explicit host names or IP addresses rather than wildcards.</span></span> <span data-ttu-id="fd223-234">Привязки с подстановочными знаками на уровне дочерних доменов (например, `*.mysub.com`) не создают таких угроз безопасности, если вы полностью контролируете родительский домен (в отличие от варианта `*.com`, создающего уязвимость).</span><span class="sxs-lookup"><span data-stu-id="fd223-234">Subdomain wildcard binding (for example, `*.mysub.com`) isn't a security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="fd223-235">Дополнительные сведения см. в стандарте [RFC 7230, раздел 5.4, Host](https://tools.ietf.org/html/rfc7230#section-5.4).</span><span class="sxs-lookup"><span data-stu-id="fd223-235">For more information, see [RFC 7230: Section 5.4: Host](https://tools.ietf.org/html/rfc7230#section-5.4).</span></span>

1. <span data-ttu-id="fd223-236">Предварительно зарегистрируйте префиксы URL-адресов на сервере.</span><span class="sxs-lookup"><span data-stu-id="fd223-236">Preregister URL prefixes on the server.</span></span>

   <span data-ttu-id="fd223-237">Встроенным средством для настройки сервера HTTP.sys является *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="fd223-237">The built-in tool for configuring HTTP.sys is *netsh.exe*.</span></span> <span data-ttu-id="fd223-238">С помощью*netsh.exe* можно зарезервировать префиксы URL-адресов и назначить сертификаты X.509.</span><span class="sxs-lookup"><span data-stu-id="fd223-238">*netsh.exe* is used to reserve URL prefixes and assign X.509 certificates.</span></span> <span data-ttu-id="fd223-239">Для использования этого средства требуются права администратора.</span><span class="sxs-lookup"><span data-stu-id="fd223-239">The tool requires administrator privileges.</span></span>

   <span data-ttu-id="fd223-240">Используйте средство *netsh.exe* для регистрации URL-адреса приложения.</span><span class="sxs-lookup"><span data-stu-id="fd223-240">Use the *netsh.exe* tool to register URLs for the app:</span></span>

   ```console
   netsh http add urlacl url=<URL> user=<USER>
   ```

   * <span data-ttu-id="fd223-241">`<URL>` — полное имя URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="fd223-241">`<URL>` &ndash; The fully qualified Uniform Resource Locator (URL).</span></span> <span data-ttu-id="fd223-242">Не используйте привязки с подстановочными знаками.</span><span class="sxs-lookup"><span data-stu-id="fd223-242">Don't use a wildcard binding.</span></span> <span data-ttu-id="fd223-243">Используйте допустимое имя узла или локальный IP-адрес.</span><span class="sxs-lookup"><span data-stu-id="fd223-243">Use a valid hostname or local IP address.</span></span> <span data-ttu-id="fd223-244">*URL-адрес должен включать косую черту в конце.*</span><span class="sxs-lookup"><span data-stu-id="fd223-244">*The URL must include a trailing slash.*</span></span>
   * <span data-ttu-id="fd223-245">`<USER>` — определяет имя пользователя или группы пользователей.</span><span class="sxs-lookup"><span data-stu-id="fd223-245">`<USER>` &ndash; Specifies the user or user-group name.</span></span>

   <span data-ttu-id="fd223-246">В следующем примере сервер имеет локальный IP-адрес `10.0.0.4`.</span><span class="sxs-lookup"><span data-stu-id="fd223-246">In the following example, the local IP address of the server is `10.0.0.4`:</span></span>

   ```console
   netsh http add urlacl url=https://10.0.0.4:443/ user=Users
   ```

   <span data-ttu-id="fd223-247">При регистрации URL-адреса средство возвращает ответ `URL reservation successfully added`.</span><span class="sxs-lookup"><span data-stu-id="fd223-247">When a URL is registered, the tool responds with `URL reservation successfully added`.</span></span>

   <span data-ttu-id="fd223-248">Чтобы удалить зарегистрированный URL-адрес, используйте команду `delete urlacl`.</span><span class="sxs-lookup"><span data-stu-id="fd223-248">To delete a registered URL, use the `delete urlacl` command:</span></span>

   ```console
   netsh http delete urlacl url=<URL>
   ```

1. <span data-ttu-id="fd223-249">Зарегистрируйте сертификаты X.509 на сервере.</span><span class="sxs-lookup"><span data-stu-id="fd223-249">Register X.509 certificates on the server.</span></span>

   <span data-ttu-id="fd223-250">Используйте средство *netsh.exe* для регистрации сертификатов приложения.</span><span class="sxs-lookup"><span data-stu-id="fd223-250">Use the *netsh.exe* tool to register certificates for the app:</span></span>

   ```console
   netsh http add sslcert ipport=<IP>:<PORT> certhash=<THUMBPRINT> appid="{<GUID>}"
   ```

   * <span data-ttu-id="fd223-251">`<IP>` — задает локальный IP-адрес для привязки.</span><span class="sxs-lookup"><span data-stu-id="fd223-251">`<IP>` &ndash; Specifies the local IP address for the binding.</span></span> <span data-ttu-id="fd223-252">Не используйте привязки с подстановочными знаками.</span><span class="sxs-lookup"><span data-stu-id="fd223-252">Don't use a wildcard binding.</span></span> <span data-ttu-id="fd223-253">Используйте допустимый IP-адрес.</span><span class="sxs-lookup"><span data-stu-id="fd223-253">Use a valid IP address.</span></span>
   * <span data-ttu-id="fd223-254">`<PORT>` — указывает порт для прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="fd223-254">`<PORT>` &ndash; Specifies the port for the binding.</span></span>
   * <span data-ttu-id="fd223-255">`<THUMBPRINT>` — отпечаток сертификата X.509.</span><span class="sxs-lookup"><span data-stu-id="fd223-255">`<THUMBPRINT>` &ndash; The X.509 certificate thumbprint.</span></span>
   * <span data-ttu-id="fd223-256">`<GUID>` — глобальный уникальный идентификатор приложения, задаваемый разработчиком в информационных целях.</span><span class="sxs-lookup"><span data-stu-id="fd223-256">`<GUID>` &ndash; A developer-generated GUID to represent the app for informational purposes.</span></span>

   <span data-ttu-id="fd223-257">В справочных целях храните GUID в приложении в виде тега пакета.</span><span class="sxs-lookup"><span data-stu-id="fd223-257">For reference purposes, store the GUID in the app as a package tag:</span></span>

   * <span data-ttu-id="fd223-258">В Visual Studio сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="fd223-258">In Visual Studio:</span></span>
     * <span data-ttu-id="fd223-259">Откройте свойства проекта приложения, щелкнув приложение правой кнопкой мыши в **обозревателе решений** и выбрав **Properties** (Свойства).</span><span class="sxs-lookup"><span data-stu-id="fd223-259">Open the app's project properties by right-clicking on the app in **Solution Explorer** and selecting **Properties**.</span></span>
     * <span data-ttu-id="fd223-260">Перейдите на вкладку **Package** (Пакет).</span><span class="sxs-lookup"><span data-stu-id="fd223-260">Select the **Package** tab.</span></span>
     * <span data-ttu-id="fd223-261">Введите GUID, который вы указали в поле **Tags** (Теги).</span><span class="sxs-lookup"><span data-stu-id="fd223-261">Enter the GUID that you created in the **Tags** field.</span></span>
   * <span data-ttu-id="fd223-262">Если Visual Studio не используется:</span><span class="sxs-lookup"><span data-stu-id="fd223-262">When not using Visual Studio:</span></span>
     * <span data-ttu-id="fd223-263">Откройте файл проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="fd223-263">Open the app's project file.</span></span>
     * <span data-ttu-id="fd223-264">Добавьте свойство `<PackageTags>` в новую или существующую группу `<PropertyGroup>` с GUID, который вы создали.</span><span class="sxs-lookup"><span data-stu-id="fd223-264">Add a `<PackageTags>` property to a new or existing `<PropertyGroup>` with the GUID that you created:</span></span>

       ```xml
       <PropertyGroup>
         <PackageTags>9412ee86-c21b-4eb8-bd89-f650fbf44931</PackageTags>
       </PropertyGroup>
       ```

   <span data-ttu-id="fd223-265">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="fd223-265">In the following example:</span></span>

   * <span data-ttu-id="fd223-266">Локальный IP-адрес сервера — `10.0.0.4`.</span><span class="sxs-lookup"><span data-stu-id="fd223-266">The local IP address of the server is `10.0.0.4`.</span></span>
   * <span data-ttu-id="fd223-267">Сетевой генератор случайных GUID задает значение `appid`.</span><span class="sxs-lookup"><span data-stu-id="fd223-267">An online random GUID generator provides the `appid` value.</span></span>

   ```console
   netsh http add sslcert 
       ipport=10.0.0.4:443 
       certhash=b66ee04419d4ee37464ab8785ff02449980eae10 
       appid="{9412ee86-c21b-4eb8-bd89-f650fbf44931}"
   ```

   <span data-ttu-id="fd223-268">При регистрации сертификата средство возвращает ответ `SSL Certificate successfully added`.</span><span class="sxs-lookup"><span data-stu-id="fd223-268">When a certificate is registered, the tool responds with `SSL Certificate successfully added`.</span></span>

   <span data-ttu-id="fd223-269">Чтобы удалить регистрацию сертификата, используйте команду `delete sslcert`.</span><span class="sxs-lookup"><span data-stu-id="fd223-269">To delete a certificate registration, use the `delete sslcert` command:</span></span>

   ```console
   netsh http delete sslcert ipport=<IP>:<PORT>
   ```

   <span data-ttu-id="fd223-270">Дополнительные сведения см. в справочной документации по *netsh.exe*:</span><span class="sxs-lookup"><span data-stu-id="fd223-270">Reference documentation for *netsh.exe*:</span></span>

   * [<span data-ttu-id="fd223-271">Команды netsh для протокола HTTP</span><span class="sxs-lookup"><span data-stu-id="fd223-271">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
   * [<span data-ttu-id="fd223-272">Строки UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="fd223-272">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

1. <span data-ttu-id="fd223-273">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="fd223-273">Run the app.</span></span>

   <span data-ttu-id="fd223-274">Если выполнена привязка к localhost через HTTP (не HTTPS) с номером порта больше 1024, для запуска приложения права администратора не требуются.</span><span class="sxs-lookup"><span data-stu-id="fd223-274">Administrator privileges aren't required to run the app when binding to localhost using HTTP (not HTTPS) with a port number greater than 1024.</span></span> <span data-ttu-id="fd223-275">При других конфигурациях (например, при использовании локального IP-адреса или привязки к порту 443) для запуска приложения требуются права администратора.</span><span class="sxs-lookup"><span data-stu-id="fd223-275">For other configurations (for example, using a local IP address or binding to port 443), run the app with administrator privileges.</span></span>

   <span data-ttu-id="fd223-276">Приложение отвечает по общедоступному IP-адресу сервера.</span><span class="sxs-lookup"><span data-stu-id="fd223-276">The app responds at the server's public IP address.</span></span> <span data-ttu-id="fd223-277">В этом примере подключение к серверу происходит через Интернет по общедоступному IP-адресу `104.214.79.47` сервера.</span><span class="sxs-lookup"><span data-stu-id="fd223-277">In this example, the server is reached from the Internet at its public IP address of `104.214.79.47`.</span></span>

   <span data-ttu-id="fd223-278">В этом примере используется сертификат разработки.</span><span class="sxs-lookup"><span data-stu-id="fd223-278">A development certificate is used in this example.</span></span> <span data-ttu-id="fd223-279">После обхода предупреждения о ненадежном сертификате браузера происходит безопасная загрузка страницы.</span><span class="sxs-lookup"><span data-stu-id="fd223-279">The page loads securely after bypassing the browser's untrusted certificate warning.</span></span>

   ![Окно браузера с отображаемой страницей индекса приложения](httpsys/_static/browser.png)

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="fd223-281">Сценарии использования прокси-сервера и подсистемы балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="fd223-281">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="fd223-282">Для приложений, размещенных с помощью файла HTTP.sys, которые взаимодействуют с запросами из Интернета или корпоративной сети, может потребоваться дополнительная настройка при размещении за прокси-серверами и подсистемами балансировки нагрузки.</span><span class="sxs-lookup"><span data-stu-id="fd223-282">For apps hosted by HTTP.sys that interact with requests from the Internet or a corporate network, additional configuration might be required when hosting behind proxy servers and load balancers.</span></span> <span data-ttu-id="fd223-283">Дополнительные сведения см. в разделе [Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="fd223-283">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fd223-284">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="fd223-284">Additional resources</span></span>

* [<span data-ttu-id="fd223-285">Включить проверку подлинности Windows с использованием HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="fd223-285">Enable Windows Authentication with HTTP.sys</span></span>](xref:security/authentication/windowsauth#httpsys)
* [<span data-ttu-id="fd223-286">API сервера HTTP</span><span class="sxs-lookup"><span data-stu-id="fd223-286">HTTP Server API</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [<span data-ttu-id="fd223-287">Репозиторий GitHub ASPNET/HttpSysServer (исходный код)</span><span class="sxs-lookup"><span data-stu-id="fd223-287">aspnet/HttpSysServer GitHub repository (source code)</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="fd223-288">Узел</span><span class="sxs-lookup"><span data-stu-id="fd223-288">The host</span></span>](xref:fundamentals/index#host)
* <xref:test/troubleshoot>
