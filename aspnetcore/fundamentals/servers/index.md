---
title: Реализации веб-сервера в ASP.NET Core
author: guardrex
description: Откройте возможности веб-серверов Kestrel и HTTP.sys для ASP.NET Core. Рекомендации по выбору сервера и сведения о сценариях использования обратного прокси-сервера.
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/14/2019
uid: fundamentals/servers/index
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="28b5c-104">Реализации веб-сервера в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="28b5c-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="28b5c-105">Авторы: [Том Дисктра](https://github.com/tdykstra) (Tom Dykstra), [Стив Смит](https://ardalis.com/) (Steve Smith), [Стивен Хальтер](https://twitter.com/halter73) (Stephen Halter) и [Крис Росс](https://github.com/Tratcher) (Chris Ross)</span><span class="sxs-lookup"><span data-stu-id="28b5c-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="28b5c-106">Приложение ASP.NET Core выполняется вместе с внутрипроцессной реализацией HTTP-сервера.</span><span class="sxs-lookup"><span data-stu-id="28b5c-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="28b5c-107">Реализация сервера прослушивает HTTP-запросы и передает их в приложение как набор [функций запросов](xref:fundamentals/request-features), объединенных в <xref:Microsoft.AspNetCore.Http.HttpContext>.</span><span class="sxs-lookup"><span data-stu-id="28b5c-107">The server implementation listens for HTTP requests and surfaces them to the app as a set of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="28b5c-108">Windows</span><span class="sxs-lookup"><span data-stu-id="28b5c-108">Windows</span></span>](#tab/windows)

<span data-ttu-id="28b5c-109">В состав ASP.NET Core входит следующее:</span><span class="sxs-lookup"><span data-stu-id="28b5c-109">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="28b5c-110">Сервер [Kestrel](xref:fundamentals/servers/kestrel) — это реализация кроссплатформенного HTTP-сервера по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="28b5c-110">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server implementation.</span></span>
* <span data-ttu-id="28b5c-111">HTTP-сервер IIS — это [внутрипроцессный сервер для службы IIS](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="28b5c-111">IIS HTTP Server is an [in-process server](#in-process-hosting-model) for IIS.</span></span>
* <span data-ttu-id="28b5c-112">[Сервер HTTP.sys](xref:fundamentals/servers/httpsys) — это HTTP-сервер, предназначенный только для Windows и основанный на [драйвере ядра HTTP.sys и API HTTP-сервера](/windows/desktop/Http/http-api-start-page).</span><span class="sxs-lookup"><span data-stu-id="28b5c-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](/windows/desktop/Http/http-api-start-page).</span></span>

<span data-ttu-id="28b5c-113">При использовании [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) или [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) приложение запускается одним из следующих способов:</span><span class="sxs-lookup"><span data-stu-id="28b5c-113">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app either runs:</span></span>

* <span data-ttu-id="28b5c-114">В том же процессе, что и рабочий процесс IIS ([модель внутрипроцессного размещения](#in-process-hosting-model)), с использованием [HTTP-сервера IIS](#iis-http-server).</span><span class="sxs-lookup"><span data-stu-id="28b5c-114">In the same process as the IIS worker process (the [in-process hosting model](#in-process-hosting-model)) with the [IIS HTTP Server](#iis-http-server).</span></span> <span data-ttu-id="28b5c-115">*Внутрипроцессное размещение* является рекомендуемой конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="28b5c-115">*In-process* is the recommended configuration.</span></span>
* <span data-ttu-id="28b5c-116">В процессе отдельно от рабочего процесса IIS ([модель внепроцессного размещения](#out-of-process-hosting-model)) с использованием [сервера Kestrel](#kestrel).</span><span class="sxs-lookup"><span data-stu-id="28b5c-116">In a process separate from the IIS worker process (the [out-of-process hosting model](#out-of-process-hosting-model)) with the [Kestrel server](#kestrel).</span></span>

<span data-ttu-id="28b5c-117">[Модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module) — это собственный модуль IIS, который обрабатывает собственные запросы IIS между службами IIS и HTTP-сервером IIS (внутрипроцессно) или сервером Kestrel.</span><span class="sxs-lookup"><span data-stu-id="28b5c-117">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is a native IIS module that handles native IIS requests between IIS and the in-process IIS HTTP Server or Kestrel.</span></span> <span data-ttu-id="28b5c-118">Для получения дополнительной информации см. <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="28b5c-118">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="hosting-models"></a><span data-ttu-id="28b5c-119">Модели размещения</span><span class="sxs-lookup"><span data-stu-id="28b5c-119">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="28b5c-120">Модель внутрипроцессного размещения</span><span class="sxs-lookup"><span data-stu-id="28b5c-120">In-process hosting model</span></span>

<span data-ttu-id="28b5c-121">При внутрипроцессном размещении приложение ASP.NET Core выполняется в том же процессе, что и рабочий процесс IIS.</span><span class="sxs-lookup"><span data-stu-id="28b5c-121">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="28b5c-122">При этом повышается производительность по сравнению с внепроцессным размещением, так как запросы не передаются через адаптер замыкания на себя (сетевой интерфейс, который возвращает исходящий сетевой трафик на тот же компьютер).</span><span class="sxs-lookup"><span data-stu-id="28b5c-122">In-process hosting provides improved performance over out-of-process hosting because requests aren't proxied over the loopback adapter, a network interface that returns outgoing network traffic back to the same machine.</span></span> <span data-ttu-id="28b5c-123">IIS обрабатывает управление процессом с помощью [службы активации процессов Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="28b5c-123">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="28b5c-124">Модуль ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="28b5c-124">The ASP.NET Core Module:</span></span>

* <span data-ttu-id="28b5c-125">Инициализирует приложение.</span><span class="sxs-lookup"><span data-stu-id="28b5c-125">Performs app initialization.</span></span>
  * <span data-ttu-id="28b5c-126">Загружает [CoreCLR](/dotnet/standard/glossary#coreclr).</span><span class="sxs-lookup"><span data-stu-id="28b5c-126">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="28b5c-127">Вызывает `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="28b5c-127">Calls `Program.Main`.</span></span>
* <span data-ttu-id="28b5c-128">Управляет жизненным циклом собственного запроса IIS.</span><span class="sxs-lookup"><span data-stu-id="28b5c-128">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="28b5c-129">Модель внутрипроцессного размещения не поддерживается для приложений ASP.NET Core, предназначенных для .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="28b5c-129">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="28b5c-130">На следующей схеме показана связь между IIS, модулем ASP.NET Core и приложением, размещенным внутри процесса.</span><span class="sxs-lookup"><span data-stu-id="28b5c-130">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![Модуль ASP.NET Core](_static/ancm-inprocess.png)

<span data-ttu-id="28b5c-132">Запрос поступает из Интернета в драйвер HTTP.sys в режиме ядра.</span><span class="sxs-lookup"><span data-stu-id="28b5c-132">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="28b5c-133">Драйвер направляет собственный запрос к IIS на настроенный порт веб-сайта — обычно 80 (HTTP) или 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="28b5c-133">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="28b5c-134">Модуль получает собственный запрос и передает его на HTTP-сервер IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="28b5c-134">The module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="28b5c-135">HTTP-сервер IIS — это реализация внутрипроцессного сервера для IIS, в которой запрос преобразовывается из собственной формы в управляемую.</span><span class="sxs-lookup"><span data-stu-id="28b5c-135">IIS HTTP Server is an in-process server implementation for IIS that converts the request from native to managed.</span></span>

<span data-ttu-id="28b5c-136">После того как HTTP-сервер IIS обрабатывает запрос, он передается в конвейер ПО промежуточного слоя ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="28b5c-136">After the IIS HTTP Server processes the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="28b5c-137">Конвейер ПО промежуточного слоя обрабатывает запрос и передает его в качестве экземпляра `HttpContext` в логику приложения.</span><span class="sxs-lookup"><span data-stu-id="28b5c-137">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="28b5c-138">Ответ приложения передается обратно в службы IIS через HTTP-сервер IIS.</span><span class="sxs-lookup"><span data-stu-id="28b5c-138">The app's response is passed back to IIS through IIS HTTP Server.</span></span> <span data-ttu-id="28b5c-139">IIS отправляет ответ клиенту, который инициировал запрос.</span><span class="sxs-lookup"><span data-stu-id="28b5c-139">IIS sends the response to the client that initiated the request.</span></span>

<span data-ttu-id="28b5c-140">Внутрипроцессное размещение необходимо явно выбирать в существующих приложениях, но в шаблонах [dotnet new](/dotnet/core/tools/dotnet-new) оно включено по умолчанию для всех сценариев IIS и IIS Express.</span><span class="sxs-lookup"><span data-stu-id="28b5c-140">In-process hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="28b5c-141">Модель размещения вне процесса</span><span class="sxs-lookup"><span data-stu-id="28b5c-141">Out-of-process hosting model</span></span>

<span data-ttu-id="28b5c-142">Так как приложения ASP.NET Core выполняются в процессе, отделенном от рабочего процесса IIS, этот модуль обрабатывает управление процессами.</span><span class="sxs-lookup"><span data-stu-id="28b5c-142">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="28b5c-143">Модуль запускает процесс для приложения ASP.NET Core при поступлении первого запроса и перезапускает приложение при сбое или завершении работы.</span><span class="sxs-lookup"><span data-stu-id="28b5c-143">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="28b5c-144">Это, по сути, совпадает с поведением приложений, выполняемых внутрипроцессно и управляемых [службой активации процессов Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="28b5c-144">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="28b5c-145">На следующей схеме показана связь между IIS, модулем ASP.NET Core и приложением, размещенным вне процесса.</span><span class="sxs-lookup"><span data-stu-id="28b5c-145">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![Модуль ASP.NET Core](_static/ancm-outofprocess.png)

<span data-ttu-id="28b5c-147">Запросы поступают из Интернета в драйвер HTTP.sys в режиме ядра.</span><span class="sxs-lookup"><span data-stu-id="28b5c-147">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="28b5c-148">Драйвер направляет запросы к службам IIS на настроенный порт веб-сайта — обычно 80 (HTTP) или 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="28b5c-148">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="28b5c-149">Модуль перенаправляет запросы Kestrel на случайный порт для приложения, отличающийся от порта 80 или 443.</span><span class="sxs-lookup"><span data-stu-id="28b5c-149">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="28b5c-150">Модуль задает порт с помощью переменной среды во время запуска, а ПО промежуточного слоя для интеграции IIS настраивает сервер для прослушивания `http://localhost:{PORT}`.</span><span class="sxs-lookup"><span data-stu-id="28b5c-150">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{PORT}`.</span></span> <span data-ttu-id="28b5c-151">Выполняются дополнительные проверки, и запросы не из модуля отклоняются.</span><span class="sxs-lookup"><span data-stu-id="28b5c-151">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="28b5c-152">Модуль не поддерживает переадресацию по HTTPS, поэтому запросы переадресовываются по протоколу HTTP, даже если были получены IIS по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="28b5c-152">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="28b5c-153">После того как Kestrel забирает запрос из модуля, запрос передается в конвейер ПО промежуточного слоя ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="28b5c-153">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="28b5c-154">Конвейер ПО промежуточного слоя обрабатывает запрос и передает его в качестве экземпляра `HttpContext` в логику приложения.</span><span class="sxs-lookup"><span data-stu-id="28b5c-154">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="28b5c-155">ПО промежуточного слоя, добавленное интеграцией IIS, обновляет схему, удаленный IP-адрес и базовый путь для переадресации запроса в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="28b5c-155">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="28b5c-156">Отклик приложения передается обратно в службу IIS, которая отправляет его обратно в HTTP-клиент, инициировавший запрос.</span><span class="sxs-lookup"><span data-stu-id="28b5c-156">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="28b5c-157">Инструкции по настройке модуля ASP.NET Core и IIS см. в следующих статьях:</span><span class="sxs-lookup"><span data-stu-id="28b5c-157">For IIS and ASP.NET Core Module configuration guidance, see the following topics:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[<span data-ttu-id="28b5c-158">macOS</span><span class="sxs-lookup"><span data-stu-id="28b5c-158">macOS</span></span>](#tab/macos)

<span data-ttu-id="28b5c-159">ASP.NET Core поставляется с [сервером Kestrel](xref:fundamentals/servers/kestrel), который является кроссплатформенным HTTP-сервером по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="28b5c-159">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="28b5c-160">Linux</span><span class="sxs-lookup"><span data-stu-id="28b5c-160">Linux</span></span>](#tab/linux)

<span data-ttu-id="28b5c-161">ASP.NET Core поставляется с [сервером Kestrel](xref:fundamentals/servers/kestrel), который является кроссплатформенным HTTP-сервером по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="28b5c-161">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="28b5c-162">Windows</span><span class="sxs-lookup"><span data-stu-id="28b5c-162">Windows</span></span>](#tab/windows)

<span data-ttu-id="28b5c-163">В состав ASP.NET Core входит следующее:</span><span class="sxs-lookup"><span data-stu-id="28b5c-163">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="28b5c-164">Сервер [Kestrel](xref:fundamentals/servers/kestrel) — кроссплатформенный HTTP-сервер по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="28b5c-164">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="28b5c-165">[Сервер HTTP.sys](xref:fundamentals/servers/httpsys) — это HTTP-сервер, предназначенный только для Windows и основанный на [драйвере ядра HTTP.sys и API HTTP-сервера](/windows/desktop/Http/http-api-start-page).</span><span class="sxs-lookup"><span data-stu-id="28b5c-165">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](/windows/desktop/Http/http-api-start-page).</span></span>

<span data-ttu-id="28b5c-166">При использовании [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) или [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) приложение запускается отдельно от рабочего процесса IIS (*внепроцессно*) с [сервером Kestrel](#kestrel).</span><span class="sxs-lookup"><span data-stu-id="28b5c-166">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app runs in a process separate from the IIS worker process (*out-of-process*) with the [Kestrel server](#kestrel).</span></span>

<span data-ttu-id="28b5c-167">Так как приложения ASP.NET Core выполняются в процессе, отделенном от рабочего процесса IIS, этот модуль обрабатывает управление процессами.</span><span class="sxs-lookup"><span data-stu-id="28b5c-167">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="28b5c-168">Модуль запускает процесс для приложения ASP.NET Core при поступлении первого запроса и перезапускает приложение при сбое или завершении работы.</span><span class="sxs-lookup"><span data-stu-id="28b5c-168">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="28b5c-169">Это, по сути, совпадает с поведением приложений, выполняемых внутрипроцессно и управляемых [службой активации процессов Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="28b5c-169">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="28b5c-170">На следующей схеме показана связь между IIS, модулем ASP.NET Core и приложением, размещенным вне процесса.</span><span class="sxs-lookup"><span data-stu-id="28b5c-170">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![Модуль ASP.NET Core](_static/ancm-outofprocess.png)

<span data-ttu-id="28b5c-172">Запросы поступают из Интернета в драйвер HTTP.sys в режиме ядра.</span><span class="sxs-lookup"><span data-stu-id="28b5c-172">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="28b5c-173">Драйвер направляет запросы к службам IIS на настроенный порт веб-сайта — обычно 80 (HTTP) или 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="28b5c-173">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="28b5c-174">Модуль перенаправляет запросы Kestrel на случайный порт для приложения, отличающийся от порта 80 или 443.</span><span class="sxs-lookup"><span data-stu-id="28b5c-174">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="28b5c-175">Модуль задает порт с помощью переменной среды во время запуска, а ПО промежуточного слоя для интеграции IIS настраивает сервер для прослушивания `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="28b5c-175">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="28b5c-176">Выполняются дополнительные проверки, и запросы не из модуля отклоняются.</span><span class="sxs-lookup"><span data-stu-id="28b5c-176">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="28b5c-177">Модуль не поддерживает переадресацию по HTTPS, поэтому запросы переадресовываются по протоколу HTTP, даже если были получены IIS по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="28b5c-177">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="28b5c-178">После того как Kestrel забирает запрос из модуля, запрос передается в конвейер ПО промежуточного слоя ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="28b5c-178">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="28b5c-179">Конвейер ПО промежуточного слоя обрабатывает запрос и передает его в качестве экземпляра `HttpContext` в логику приложения.</span><span class="sxs-lookup"><span data-stu-id="28b5c-179">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="28b5c-180">ПО промежуточного слоя, добавленное интеграцией IIS, обновляет схему, удаленный IP-адрес и базовый путь для переадресации запроса в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="28b5c-180">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="28b5c-181">Отклик приложения передается обратно в службу IIS, которая отправляет его обратно в HTTP-клиент, инициировавший запрос.</span><span class="sxs-lookup"><span data-stu-id="28b5c-181">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="28b5c-182">Инструкции по настройке модуля ASP.NET Core и IIS см. в следующих статьях:</span><span class="sxs-lookup"><span data-stu-id="28b5c-182">For IIS and ASP.NET Core Module configuration guidance, see the following topics:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[<span data-ttu-id="28b5c-183">macOS</span><span class="sxs-lookup"><span data-stu-id="28b5c-183">macOS</span></span>](#tab/macos)

<span data-ttu-id="28b5c-184">ASP.NET Core поставляется с [сервером Kestrel](xref:fundamentals/servers/kestrel), который является кроссплатформенным HTTP-сервером по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="28b5c-184">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="28b5c-185">Linux</span><span class="sxs-lookup"><span data-stu-id="28b5c-185">Linux</span></span>](#tab/linux)

<span data-ttu-id="28b5c-186">ASP.NET Core поставляется с [сервером Kestrel](xref:fundamentals/servers/kestrel), который является кроссплатформенным HTTP-сервером по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="28b5c-186">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="28b5c-187">Kestrel</span><span class="sxs-lookup"><span data-stu-id="28b5c-187">Kestrel</span></span>

<span data-ttu-id="28b5c-188">Веб-сервер Kestrel по умолчанию включается в шаблоны проектов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="28b5c-188">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="28b5c-189">Kestrel можно использовать:</span><span class="sxs-lookup"><span data-stu-id="28b5c-189">Kestrel can be used:</span></span>

* <span data-ttu-id="28b5c-190">Сам по себе как пограничный сервер обработки запросов непосредственно из сети, включая Интернет.</span><span class="sxs-lookup"><span data-stu-id="28b5c-190">By itself as an edge server processing requests directly from a network, including the Internet.</span></span>

  ![Kestrel взаимодействует с Интернетом напрямую, без обратного прокси-сервера](kestrel/_static/kestrel-to-internet2.png)

* <span data-ttu-id="28b5c-192">С *обратным прокси-сервером*, таким как [службы IIS](https://www.iis.net/), [Nginx](http://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="28b5c-192">With a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="28b5c-193">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="28b5c-193">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

  ![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="28b5c-195">Любая из этих конфигураций размещения&mdash;с обратным прокси-сервером и без него&mdash; поддерживается для приложений ASP.NET Core версии 2.1 и выше.</span><span class="sxs-lookup"><span data-stu-id="28b5c-195">Either hosting configuration&mdash;with or without a reverse proxy server&mdash;is supported for ASP.NET Core 2.1 or later apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="28b5c-196">Если приложение принимает запросы только от внутренней сети, можно использовать Kestrel сам по себе.</span><span class="sxs-lookup"><span data-stu-id="28b5c-196">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![Kestrel взаимодействует с внутренней сетью напрямую](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="28b5c-198">Если приложение имеет доступ к Интернету, Kestrel должен использовать *обратный прокси-сервер*, такой как [службы IIS](https://www.iis.net/), [Nginx](http://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="28b5c-198">If the app is exposed to the Internet, Kestrel must use a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="28b5c-199">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="28b5c-199">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="28b5c-201">Основная причина использования обратного прокси-сервера для развертываний пограничных серверов, открытых для общего доступа, — это безопасность.</span><span class="sxs-lookup"><span data-stu-id="28b5c-201">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="28b5c-202">Версии Kestrel 1.x не обладают важными функциями безопасности для защиты от атак из Интернета.</span><span class="sxs-lookup"><span data-stu-id="28b5c-202">The 1.x versions of Kestrel don't include important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="28b5c-203">Сюда входят, например, соответствующее время ожидания, предельные размеры запроса и максимальное количество одновременных подключений.</span><span class="sxs-lookup"><span data-stu-id="28b5c-203">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

::: moniker-end

<span data-ttu-id="28b5c-204">Инструкции по настройке Kestrel и сведения о том, когда следует использовать Kestrel в конфигурации обратного прокси-сервера, см. в статье <xref:fundamentals/servers/kestrel>.</span><span class="sxs-lookup"><span data-stu-id="28b5c-204">For Kestrel configuration guidance and information on when to use Kestrel in a reverse proxy configuration, see <xref:fundamentals/servers/kestrel>.</span></span>

### <a name="nginx-with-kestrel"></a><span data-ttu-id="28b5c-205">Nginx с Kestrel</span><span class="sxs-lookup"><span data-stu-id="28b5c-205">Nginx with Kestrel</span></span>

<span data-ttu-id="28b5c-206">Сведения о том, как использовать Nginx в Linux в качестве обратного прокси-сервера для Kestrel, см. в <xref:host-and-deploy/linux-nginx>.</span><span class="sxs-lookup"><span data-stu-id="28b5c-206">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-nginx>.</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="28b5c-207">Apache с Kestrel</span><span class="sxs-lookup"><span data-stu-id="28b5c-207">Apache with Kestrel</span></span>

<span data-ttu-id="28b5c-208">Сведения о том, как использовать Apache в Linux в качестве обратного прокси-сервера для Kestrel, см. в <xref:host-and-deploy/linux-apache>.</span><span class="sxs-lookup"><span data-stu-id="28b5c-208">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-apache>.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="iis-http-server"></a><span data-ttu-id="28b5c-209">HTTP-сервер IIS</span><span class="sxs-lookup"><span data-stu-id="28b5c-209">IIS HTTP Server</span></span>

<span data-ttu-id="28b5c-210">HTTP-сервер IIS — это [внутрипроцессный сервер](#in-process-hosting-model) для служб IIS, который требуется для внутрипроцессных развертываний.</span><span class="sxs-lookup"><span data-stu-id="28b5c-210">IIS HTTP Server is an [in-process server](#in-process-hosting-model) for IIS and required for in-process deployments.</span></span> <span data-ttu-id="28b5c-211">[Модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module) обрабатывает собственные запросы IIS между HTTP-сервером IIS и службами IIS.</span><span class="sxs-lookup"><span data-stu-id="28b5c-211">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) handles native IIS requests between IIS and IIS HTTP Server.</span></span> <span data-ttu-id="28b5c-212">Для получения дополнительной информации см. <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="28b5c-212">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

## <a name="httpsys"></a><span data-ttu-id="28b5c-213">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="28b5c-213">HTTP.sys</span></span>

<span data-ttu-id="28b5c-214">Если приложение ASP.NET Core запускается в Windows, вместо Kestrel можно использовать HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="28b5c-214">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="28b5c-215">Как правило, для оптимальной производительности рекомендуется Kestrel.</span><span class="sxs-lookup"><span data-stu-id="28b5c-215">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="28b5c-216">HTTP.sys можно использовать в сценариях, где приложение имеет доступ к Интернету и необходимые возможности поддерживаются HTTP.sys, но не Kestrel.</span><span class="sxs-lookup"><span data-stu-id="28b5c-216">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="28b5c-217">Для получения дополнительной информации см. <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="28b5c-217">For more information, see <xref:fundamentals/servers/httpsys>.</span></span>

![HTTP.sys взаимодействует с Интернетом напрямую](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="28b5c-219">HTTP.sys можно также использовать для приложений, которые имеют доступ только к внутренней сети.</span><span class="sxs-lookup"><span data-stu-id="28b5c-219">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![HTTP.sys взаимодействует с внутренней сетью напрямую](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="28b5c-221">Инструкции по настройке HTTP.sys см. в статье <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="28b5c-221">For HTTP.sys configuration guidance, see <xref:fundamentals/servers/httpsys>.</span></span>

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="28b5c-222">Инфраструктура сервера ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="28b5c-222">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="28b5c-223"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>, доступный в методе `Startup.Configure`, предоставляет свойство <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ServerFeatures> типа <xref:Microsoft.AspNetCore.Http.Features.IFeatureCollection>.</span><span class="sxs-lookup"><span data-stu-id="28b5c-223">The <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> available in the `Startup.Configure` method exposes the <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ServerFeatures> property of type <xref:Microsoft.AspNetCore.Http.Features.IFeatureCollection>.</span></span> <span data-ttu-id="28b5c-224">Kestrel и HTTP.sys предоставляют только один компонент, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>, но разные реализации сервера могут предоставлять дополнительные возможности.</span><span class="sxs-lookup"><span data-stu-id="28b5c-224">Kestrel and HTTP.sys only expose a single feature each, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>, but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="28b5c-225">`IServerAddressesFeature` можно использовать для того, чтобы узнать, какой порт в реализации сервера привязан к среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="28b5c-225">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="28b5c-226">Пользовательские серверы</span><span class="sxs-lookup"><span data-stu-id="28b5c-226">Custom servers</span></span>

<span data-ttu-id="28b5c-227">Если встроенные серверы не отвечают требованиям приложения, можно создать реализацию пользовательского сервера.</span><span class="sxs-lookup"><span data-stu-id="28b5c-227">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="28b5c-228">В [руководстве по открытому веб-интерфейсу .NET (OWIN)](xref:fundamentals/owin) демонстрируется запись реализации <xref:Microsoft.AspNetCore.Hosting.Server.IServer> на основе [Nowin](https://github.com/Bobris/Nowin).</span><span class="sxs-lookup"><span data-stu-id="28b5c-228">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation.</span></span> <span data-ttu-id="28b5c-229">Требуют реализации только интерфейсы компонентов, используемых приложением, но как минимум должны поддерживаться <xref:Microsoft.AspNetCore.Http.Features.IHttpRequestFeature> и <xref:Microsoft.AspNetCore.Http.Features.IHttpResponseFeature>.</span><span class="sxs-lookup"><span data-stu-id="28b5c-229">Only the feature interfaces that the app uses require implementation, though at a minimum <xref:Microsoft.AspNetCore.Http.Features.IHttpRequestFeature> and <xref:Microsoft.AspNetCore.Http.Features.IHttpResponseFeature> must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="28b5c-230">Запуск сервера</span><span class="sxs-lookup"><span data-stu-id="28b5c-230">Server startup</span></span>

<span data-ttu-id="28b5c-231">Сервер запускается, когда интегрированная среда разработки (IDE) или редактор запускает приложение:</span><span class="sxs-lookup"><span data-stu-id="28b5c-231">The server is launched when the Integrated Development Environment (IDE) or editor starts the app:</span></span>

* <span data-ttu-id="28b5c-232">[Visual Studio](https://www.visualstudio.com/vs/) — профили запуска можно использовать для запуска приложения и сервера с помощью [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module) или консоли.</span><span class="sxs-lookup"><span data-stu-id="28b5c-232">[Visual Studio](https://www.visualstudio.com/vs/) &ndash; Launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) or the console.</span></span>
* <span data-ttu-id="28b5c-233">[Visual Studio Code](https://code.visualstudio.com/) — приложение и сервер запускаются решением [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), которое активирует отладчик CoreCLR.</span><span class="sxs-lookup"><span data-stu-id="28b5c-233">[Visual Studio Code](https://code.visualstudio.com/) &ndash; The app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span>
* <span data-ttu-id="28b5c-234">[Visual Studio для Mac](https://www.visualstudio.com/vs/mac/) — приложение и сервер запускаются отладчиком [Mono Soft Debugger](https://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span><span class="sxs-lookup"><span data-stu-id="28b5c-234">[Visual Studio for Mac](https://www.visualstudio.com/vs/mac/) &ndash; The app and server are started by the [Mono Soft-Mode Debugger](https://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="28b5c-235">При запуске приложения из командной строки в папке проекта [dotnet run](/dotnet/core/tools/dotnet-run) запускает приложение и сервер (только Kestrel и HTTP.sys).</span><span class="sxs-lookup"><span data-stu-id="28b5c-235">When launching the app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="28b5c-236">Конфигурация определяется параметром `-c|--configuration`, который может принимать значение `Debug` (по умолчанию) или `Release`.</span><span class="sxs-lookup"><span data-stu-id="28b5c-236">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="28b5c-237">Если профили запуска указаны в файле *launchSettings.json*, используйте параметр `--launch-profile <NAME>`, чтобы настроить профиль запуска (например, `Development` или `Production`).</span><span class="sxs-lookup"><span data-stu-id="28b5c-237">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="28b5c-238">Дополнительные сведения см. в статьях [dotnet run](/dotnet/core/tools/dotnet-run) и [Упаковка дистрибутивов .NET Core](/dotnet/core/build/distribution-packaging).</span><span class="sxs-lookup"><span data-stu-id="28b5c-238">For more information, see [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging).</span></span>

## <a name="http2-support"></a><span data-ttu-id="28b5c-239">Поддержка HTTP/2</span><span class="sxs-lookup"><span data-stu-id="28b5c-239">HTTP/2 support</span></span>

<span data-ttu-id="28b5c-240">[HTTP/2](https://httpwg.org/specs/rfc7540.html) поддерживается в ASP.NET Core для следующих сценариев развертывания:</span><span class="sxs-lookup"><span data-stu-id="28b5c-240">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="28b5c-241">Kestrel</span><span class="sxs-lookup"><span data-stu-id="28b5c-241">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="28b5c-242">Операционная система</span><span class="sxs-lookup"><span data-stu-id="28b5c-242">Operating system</span></span>
    * <span data-ttu-id="28b5c-243">Windows Server 2016 / Windows 10 или более поздних версий&dagger;</span><span class="sxs-lookup"><span data-stu-id="28b5c-243">Windows Server 2016/Windows 10 or later&dagger;</span></span>
    * <span data-ttu-id="28b5c-244">Linux с OpenSSL 1.0.2 или более поздней версии (например, Ubuntu 16.04 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="28b5c-244">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="28b5c-245">HTTP/2 будет поддерживаться для macOS в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="28b5c-245">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="28b5c-246">Требуемая версия .NET Framework: .NET Core версии 2.2 или более поздней</span><span class="sxs-lookup"><span data-stu-id="28b5c-246">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="28b5c-247">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="28b5c-247">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="28b5c-248">Windows Server 2016 / Windows 10 или более поздних версий</span><span class="sxs-lookup"><span data-stu-id="28b5c-248">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="28b5c-249">Целевая платформа: неприменимо к развертываниям HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="28b5c-249">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="28b5c-250">IIS (внутрипроцессный)</span><span class="sxs-lookup"><span data-stu-id="28b5c-250">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="28b5c-251">Windows Server 2016 / Windows 10 или более поздних версий; IIS 10 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="28b5c-251">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="28b5c-252">Требуемая версия .NET Framework: .NET Core версии 2.2 или более поздней</span><span class="sxs-lookup"><span data-stu-id="28b5c-252">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="28b5c-253">IIS (внепроцессный)</span><span class="sxs-lookup"><span data-stu-id="28b5c-253">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="28b5c-254">Windows Server 2016 / Windows 10 или более поздних версий; IIS 10 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="28b5c-254">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="28b5c-255">Подключения к пограничным серверам, открытых для общего доступа, выполняются по протоколу HTTP/2, а подключения к серверу Kestrel через обратный прокси-сервер — по протоколу HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="28b5c-255">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="28b5c-256">Целевая платформа: неприменимо к внепроцессным развертываниям IIS.</span><span class="sxs-lookup"><span data-stu-id="28b5c-256">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

<span data-ttu-id="28b5c-257">&dagger;Для Kestrel предусмотрена ограниченная поддержка HTTP/2 в Windows Server 2012 R2 и Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="28b5c-257">&dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="28b5c-258">Поддержка ограничена из-за небольшого числа поддерживаемых комплектов шифров TLS, доступных для этих операционных систем.</span><span class="sxs-lookup"><span data-stu-id="28b5c-258">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="28b5c-259">Для обеспечения безопасности TLS-подключений может потребоваться сертификат, созданный с использованием алгоритма ECDSA.</span><span class="sxs-lookup"><span data-stu-id="28b5c-259">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="28b5c-260">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="28b5c-260">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="28b5c-261">Windows Server 2016 / Windows 10 или более поздних версий</span><span class="sxs-lookup"><span data-stu-id="28b5c-261">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="28b5c-262">Целевая платформа: неприменимо к развертываниям HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="28b5c-262">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="28b5c-263">IIS (внепроцессный)</span><span class="sxs-lookup"><span data-stu-id="28b5c-263">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="28b5c-264">Windows Server 2016 / Windows 10 или более поздних версий; IIS 10 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="28b5c-264">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="28b5c-265">Подключения к пограничным серверам, открытых для общего доступа, выполняются по протоколу HTTP/2, а подключения к серверу Kestrel через обратный прокси-сервер — по протоколу HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="28b5c-265">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="28b5c-266">Целевая платформа: неприменимо к внепроцессным развертываниям IIS.</span><span class="sxs-lookup"><span data-stu-id="28b5c-266">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="28b5c-267">Подключение HTTP/2 должно использовать [согласование протокола уровня приложений (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) и TLS 1.2 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="28b5c-267">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="28b5c-268">Дополнительные сведения см. в разделах, о конкретных сценариях развертывания сервера.</span><span class="sxs-lookup"><span data-stu-id="28b5c-268">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="28b5c-269">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="28b5c-269">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:fundamentals/servers/httpsys>
