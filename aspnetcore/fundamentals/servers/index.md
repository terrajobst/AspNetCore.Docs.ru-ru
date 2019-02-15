---
title: Реализации веб-сервера в ASP.NET Core
author: guardrex
description: Откройте возможности веб-серверов Kestrel и HTTP.sys для ASP.NET Core. Рекомендации по выбору сервера и сведения о сценариях использования обратного прокси-сервера.
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/13/2019
uid: fundamentals/servers/index
ms.openlocfilehash: 672fe2ce6fd0adae09c380fe508344a254f1a9fe
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248138"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="415dd-104">Реализации веб-сервера в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="415dd-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="415dd-105">Авторы: [Том Дисктра](https://github.com/tdykstra) (Tom Dykstra), [Стив Смит](https://ardalis.com/) (Steve Smith), [Стивен Хальтер](https://twitter.com/halter73) (Stephen Halter) и [Крис Росс](https://github.com/Tratcher) (Chris Ross)</span><span class="sxs-lookup"><span data-stu-id="415dd-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="415dd-106">Приложение ASP.NET Core выполняется вместе с внутрипроцессной реализацией HTTP-сервера.</span><span class="sxs-lookup"><span data-stu-id="415dd-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="415dd-107">Реализация сервера прослушивает HTTP-запросы и передает их в приложение как набор [функций запросов](xref:fundamentals/request-features), объединенных в <xref:Microsoft.AspNetCore.Http.HttpContext>.</span><span class="sxs-lookup"><span data-stu-id="415dd-107">The server implementation listens for HTTP requests and surfaces them to the app as a set of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="415dd-108">Windows</span><span class="sxs-lookup"><span data-stu-id="415dd-108">Windows</span></span>](#tab/windows)

<span data-ttu-id="415dd-109">В состав ASP.NET Core входит следующее:</span><span class="sxs-lookup"><span data-stu-id="415dd-109">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="415dd-110">Сервер [Kestrel](xref:fundamentals/servers/kestrel) — это реализация кроссплатформенного HTTP-сервера по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="415dd-110">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server implementation.</span></span>
* <span data-ttu-id="415dd-111">HTTP-сервер IIS — это [внутрипроцессный сервер для службы IIS](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="415dd-111">IIS HTTP Server is an [in-process server](#in-process-hosting-model) for IIS.</span></span>
* <span data-ttu-id="415dd-112">[Сервер HTTP.sys](xref:fundamentals/servers/httpsys) — это HTTP-сервер, предназначенный только для Windows и основанный на [драйвере ядра HTTP.sys и API HTTP-сервера](/windows/desktop/Http/http-api-start-page).</span><span class="sxs-lookup"><span data-stu-id="415dd-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](/windows/desktop/Http/http-api-start-page).</span></span>

<span data-ttu-id="415dd-113">При использовании [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) или [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) приложение запускается одним из следующих способов:</span><span class="sxs-lookup"><span data-stu-id="415dd-113">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app either runs:</span></span>

* <span data-ttu-id="415dd-114">В том же процессе, что и рабочий процесс IIS ([модель внутрипроцессного размещения](#in-process-hosting-model)), с использованием [HTTP-сервера IIS](#iis-http-server).</span><span class="sxs-lookup"><span data-stu-id="415dd-114">In the same process as the IIS worker process (the [in-process hosting model](#in-process-hosting-model)) with the [IIS HTTP Server](#iis-http-server).</span></span> <span data-ttu-id="415dd-115">*Внутрипроцессное размещение* является рекомендуемой конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="415dd-115">*In-process* is the recommended configuration.</span></span>
* <span data-ttu-id="415dd-116">В процессе отдельно от рабочего процесса IIS ([модель внепроцессного размещения](#out-of-process-hosting-model)) с использованием [сервера Kestrel](#kestrel).</span><span class="sxs-lookup"><span data-stu-id="415dd-116">In a process separate from the IIS worker process (the [out-of-process hosting model](#out-of-process-hosting-model)) with the [Kestrel server](#kestrel).</span></span>

<span data-ttu-id="415dd-117">[Модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module) — это собственный модуль IIS, который обрабатывает собственные запросы IIS между службами IIS и HTTP-сервером IIS (внутрипроцессно) или сервером Kestrel.</span><span class="sxs-lookup"><span data-stu-id="415dd-117">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is a native IIS module that handles native IIS requests between IIS and the in-process IIS HTTP Server or Kestrel.</span></span> <span data-ttu-id="415dd-118">Для получения дополнительной информации см. <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="415dd-118">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="hosting-models"></a><span data-ttu-id="415dd-119">Модели размещения</span><span class="sxs-lookup"><span data-stu-id="415dd-119">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="415dd-120">Модель внутрипроцессного размещения</span><span class="sxs-lookup"><span data-stu-id="415dd-120">In-process hosting model</span></span>

<span data-ttu-id="415dd-121">При внутрипроцессном размещении приложение ASP.NET Core выполняется в том же процессе, что и рабочий процесс IIS.</span><span class="sxs-lookup"><span data-stu-id="415dd-121">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="415dd-122">При этом повышается производительность по сравнению с внепроцессным размещением, так как запросы не передаются через адаптер замыкания на себя (сетевой интерфейс, который возвращает исходящий сетевой трафик на тот же компьютер).</span><span class="sxs-lookup"><span data-stu-id="415dd-122">In-process hosting provides improved performance over out-of-process hosting because requests aren't proxied over the loopback adapter, a network interface that returns outgoing network traffic back to the same machine.</span></span> <span data-ttu-id="415dd-123">IIS обрабатывает управление процессом с помощью [службы активации процессов Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="415dd-123">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="415dd-124">Модуль ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="415dd-124">The ASP.NET Core Module:</span></span>

* <span data-ttu-id="415dd-125">Инициализирует приложение.</span><span class="sxs-lookup"><span data-stu-id="415dd-125">Performs app initialization.</span></span>
  * <span data-ttu-id="415dd-126">Загружает [CoreCLR](/dotnet/standard/glossary#coreclr).</span><span class="sxs-lookup"><span data-stu-id="415dd-126">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="415dd-127">Вызывает `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="415dd-127">Calls `Program.Main`.</span></span>
* <span data-ttu-id="415dd-128">Управляет жизненным циклом собственного запроса IIS.</span><span class="sxs-lookup"><span data-stu-id="415dd-128">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="415dd-129">Модель внутрипроцессного размещения не поддерживается для приложений ASP.NET Core, предназначенных для .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="415dd-129">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="415dd-130">На следующей схеме показана связь между IIS, модулем ASP.NET Core и приложением, размещенным внутри процесса.</span><span class="sxs-lookup"><span data-stu-id="415dd-130">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![Модуль ASP.NET Core](_static/ancm-inprocess.png)

<span data-ttu-id="415dd-132">Запрос поступает из Интернета в драйвер HTTP.sys в режиме ядра.</span><span class="sxs-lookup"><span data-stu-id="415dd-132">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="415dd-133">Драйвер направляет собственный запрос к IIS на настроенный порт веб-сайта — обычно 80 (HTTP) или 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="415dd-133">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="415dd-134">Модуль получает собственный запрос и передает его на HTTP-сервер IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="415dd-134">The module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="415dd-135">HTTP-сервер IIS — это реализация внутрипроцессного сервера для IIS, в которой запрос преобразовывается из собственной формы в управляемую.</span><span class="sxs-lookup"><span data-stu-id="415dd-135">IIS HTTP Server is an in-process server implementation for IIS that converts the request from native to managed.</span></span>

<span data-ttu-id="415dd-136">После того как HTTP-сервер IIS обрабатывает запрос, он передается в конвейер ПО промежуточного слоя ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="415dd-136">After the IIS HTTP Server processes the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="415dd-137">Конвейер ПО промежуточного слоя обрабатывает запрос и передает его в качестве экземпляра `HttpContext` в логику приложения.</span><span class="sxs-lookup"><span data-stu-id="415dd-137">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="415dd-138">Отклик приложения передается обратно в службу IIS, которая отправляет его обратно в клиент, инициировавший запрос.</span><span class="sxs-lookup"><span data-stu-id="415dd-138">The app's response is passed back to IIS, which pushes it back out to the client that initiated the request.</span></span>

<span data-ttu-id="415dd-139">Внутрипроцессное размещение необходимо явно выбирать в существующих приложениях, но в шаблонах [dotnet new](/dotnet/core/tools/dotnet-new) оно включено по умолчанию для всех сценариев IIS и IIS Express.</span><span class="sxs-lookup"><span data-stu-id="415dd-139">In-process hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="415dd-140">Модель размещения вне процесса</span><span class="sxs-lookup"><span data-stu-id="415dd-140">Out-of-process hosting model</span></span>

<span data-ttu-id="415dd-141">Так как приложения ASP.NET Core выполняются в процессе, отделенном от рабочего процесса IIS, этот модуль обрабатывает управление процессами.</span><span class="sxs-lookup"><span data-stu-id="415dd-141">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="415dd-142">Модуль запускает процесс для приложения ASP.NET Core при поступлении первого запроса и перезапускает приложение при сбое или завершении работы.</span><span class="sxs-lookup"><span data-stu-id="415dd-142">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="415dd-143">Это, по сути, совпадает с поведением приложений, выполняемых внутрипроцессно и управляемых [службой активации процессов Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="415dd-143">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="415dd-144">На следующей схеме показана связь между IIS, модулем ASP.NET Core и приложением, размещенным вне процесса.</span><span class="sxs-lookup"><span data-stu-id="415dd-144">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![Модуль ASP.NET Core](_static/ancm-outofprocess.png)

<span data-ttu-id="415dd-146">Запросы поступают из Интернета в драйвер HTTP.sys в режиме ядра.</span><span class="sxs-lookup"><span data-stu-id="415dd-146">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="415dd-147">Драйвер направляет запросы к службам IIS на настроенный порт веб-сайта — обычно 80 (HTTP) или 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="415dd-147">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="415dd-148">Модуль перенаправляет запросы Kestrel на случайный порт для приложения, отличающийся от порта 80 или 443.</span><span class="sxs-lookup"><span data-stu-id="415dd-148">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="415dd-149">Модуль задает порт с помощью переменной среды во время запуска, а ПО промежуточного слоя для интеграции IIS настраивает сервер для прослушивания `http://localhost:{PORT}`.</span><span class="sxs-lookup"><span data-stu-id="415dd-149">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{PORT}`.</span></span> <span data-ttu-id="415dd-150">Выполняются дополнительные проверки, и запросы не из модуля отклоняются.</span><span class="sxs-lookup"><span data-stu-id="415dd-150">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="415dd-151">Модуль не поддерживает переадресацию по HTTPS, поэтому запросы переадресовываются по протоколу HTTP, даже если были получены IIS по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="415dd-151">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="415dd-152">После того как Kestrel забирает запрос из модуля, запрос передается в конвейер ПО промежуточного слоя ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="415dd-152">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="415dd-153">Конвейер ПО промежуточного слоя обрабатывает запрос и передает его в качестве экземпляра `HttpContext` в логику приложения.</span><span class="sxs-lookup"><span data-stu-id="415dd-153">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="415dd-154">ПО промежуточного слоя, добавленное интеграцией IIS, обновляет схему, удаленный IP-адрес и базовый путь для переадресации запроса в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="415dd-154">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="415dd-155">Отклик приложения передается обратно в службу IIS, которая отправляет его обратно в HTTP-клиент, инициировавший запрос.</span><span class="sxs-lookup"><span data-stu-id="415dd-155">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="415dd-156">Инструкции по настройке модуля ASP.NET Core и IIS см. в следующих статьях:</span><span class="sxs-lookup"><span data-stu-id="415dd-156">For IIS and ASP.NET Core Module configuration guidance, see the following topics:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[<span data-ttu-id="415dd-157">macOS</span><span class="sxs-lookup"><span data-stu-id="415dd-157">macOS</span></span>](#tab/macos)

<span data-ttu-id="415dd-158">ASP.NET Core поставляется с [сервером Kestrel](xref:fundamentals/servers/kestrel), который является кроссплатформенным HTTP-сервером по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="415dd-158">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="415dd-159">Linux</span><span class="sxs-lookup"><span data-stu-id="415dd-159">Linux</span></span>](#tab/linux)

<span data-ttu-id="415dd-160">ASP.NET Core поставляется с [сервером Kestrel](xref:fundamentals/servers/kestrel), который является кроссплатформенным HTTP-сервером по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="415dd-160">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="415dd-161">Windows</span><span class="sxs-lookup"><span data-stu-id="415dd-161">Windows</span></span>](#tab/windows)

<span data-ttu-id="415dd-162">В состав ASP.NET Core входит следующее:</span><span class="sxs-lookup"><span data-stu-id="415dd-162">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="415dd-163">Сервер [Kestrel](xref:fundamentals/servers/kestrel) — кроссплатформенный HTTP-сервер по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="415dd-163">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="415dd-164">[Сервер HTTP.sys](xref:fundamentals/servers/httpsys) — это HTTP-сервер, предназначенный только для Windows и основанный на [драйвере ядра HTTP.sys и API HTTP-сервера](/windows/desktop/Http/http-api-start-page).</span><span class="sxs-lookup"><span data-stu-id="415dd-164">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](/windows/desktop/Http/http-api-start-page).</span></span>

<span data-ttu-id="415dd-165">При использовании [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) или [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) приложение запускается отдельно от рабочего процесса IIS (*внепроцессно*) с [сервером Kestrel](#kestrel).</span><span class="sxs-lookup"><span data-stu-id="415dd-165">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app runs in a process separate from the IIS worker process (*out-of-process*) with the [Kestrel server](#kestrel).</span></span>

<span data-ttu-id="415dd-166">Так как приложения ASP.NET Core выполняются в процессе, отделенном от рабочего процесса IIS, этот модуль обрабатывает управление процессами.</span><span class="sxs-lookup"><span data-stu-id="415dd-166">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="415dd-167">Модуль запускает процесс для приложения ASP.NET Core при поступлении первого запроса и перезапускает приложение при сбое или завершении работы.</span><span class="sxs-lookup"><span data-stu-id="415dd-167">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="415dd-168">Это, по сути, совпадает с поведением приложений, выполняемых внутрипроцессно и управляемых [службой активации процессов Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="415dd-168">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="415dd-169">На следующей схеме показана связь между IIS, модулем ASP.NET Core и приложением, размещенным вне процесса.</span><span class="sxs-lookup"><span data-stu-id="415dd-169">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![Модуль ASP.NET Core](_static/ancm-outofprocess.png)

<span data-ttu-id="415dd-171">Запросы поступают из Интернета в драйвер HTTP.sys в режиме ядра.</span><span class="sxs-lookup"><span data-stu-id="415dd-171">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="415dd-172">Драйвер направляет запросы к службам IIS на настроенный порт веб-сайта — обычно 80 (HTTP) или 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="415dd-172">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="415dd-173">Модуль перенаправляет запросы Kestrel на случайный порт для приложения, отличающийся от порта 80 или 443.</span><span class="sxs-lookup"><span data-stu-id="415dd-173">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="415dd-174">Модуль задает порт с помощью переменной среды во время запуска, а ПО промежуточного слоя для интеграции IIS настраивает сервер для прослушивания `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="415dd-174">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="415dd-175">Выполняются дополнительные проверки, и запросы не из модуля отклоняются.</span><span class="sxs-lookup"><span data-stu-id="415dd-175">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="415dd-176">Модуль не поддерживает переадресацию по HTTPS, поэтому запросы переадресовываются по протоколу HTTP, даже если были получены IIS по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="415dd-176">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="415dd-177">После того как Kestrel забирает запрос из модуля, запрос передается в конвейер ПО промежуточного слоя ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="415dd-177">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="415dd-178">Конвейер ПО промежуточного слоя обрабатывает запрос и передает его в качестве экземпляра `HttpContext` в логику приложения.</span><span class="sxs-lookup"><span data-stu-id="415dd-178">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="415dd-179">ПО промежуточного слоя, добавленное интеграцией IIS, обновляет схему, удаленный IP-адрес и базовый путь для переадресации запроса в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="415dd-179">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="415dd-180">Отклик приложения передается обратно в службу IIS, которая отправляет его обратно в HTTP-клиент, инициировавший запрос.</span><span class="sxs-lookup"><span data-stu-id="415dd-180">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="415dd-181">Инструкции по настройке модуля ASP.NET Core и IIS см. в следующих статьях:</span><span class="sxs-lookup"><span data-stu-id="415dd-181">For IIS and ASP.NET Core Module configuration guidance, see the following topics:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[<span data-ttu-id="415dd-182">macOS</span><span class="sxs-lookup"><span data-stu-id="415dd-182">macOS</span></span>](#tab/macos)

<span data-ttu-id="415dd-183">ASP.NET Core поставляется с [сервером Kestrel](xref:fundamentals/servers/kestrel), который является кроссплатформенным HTTP-сервером по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="415dd-183">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="415dd-184">Linux</span><span class="sxs-lookup"><span data-stu-id="415dd-184">Linux</span></span>](#tab/linux)

<span data-ttu-id="415dd-185">ASP.NET Core поставляется с [сервером Kestrel](xref:fundamentals/servers/kestrel), который является кроссплатформенным HTTP-сервером по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="415dd-185">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="415dd-186">Kestrel</span><span class="sxs-lookup"><span data-stu-id="415dd-186">Kestrel</span></span>

<span data-ttu-id="415dd-187">Веб-сервер Kestrel по умолчанию включается в шаблоны проектов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="415dd-187">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="415dd-188">Kestrel можно использовать:</span><span class="sxs-lookup"><span data-stu-id="415dd-188">Kestrel can be used:</span></span>

* <span data-ttu-id="415dd-189">Сам по себе как пограничный сервер обработки запросов непосредственно из сети, включая Интернет.</span><span class="sxs-lookup"><span data-stu-id="415dd-189">By itself as an edge server processing requests directly from a network, including the Internet.</span></span>

  ![Kestrel взаимодействует с Интернетом напрямую, без обратного прокси-сервера](kestrel/_static/kestrel-to-internet2.png)

* <span data-ttu-id="415dd-191">С *обратным прокси-сервером*, таким как [службы IIS](https://www.iis.net/), [Nginx](http://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="415dd-191">With a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="415dd-192">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="415dd-192">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

  ![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="415dd-194">Любая из этих конфигураций размещения&mdash;с обратным прокси-сервером и без него&mdash; поддерживается для приложений ASP.NET Core версии 2.1 и выше.</span><span class="sxs-lookup"><span data-stu-id="415dd-194">Either hosting configuration&mdash;with or without a reverse proxy server&mdash;is supported for ASP.NET Core 2.1 or later apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="415dd-195">Если приложение принимает запросы только от внутренней сети, можно использовать Kestrel сам по себе.</span><span class="sxs-lookup"><span data-stu-id="415dd-195">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![Kestrel взаимодействует с внутренней сетью напрямую](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="415dd-197">Если приложение имеет доступ к Интернету, Kestrel должен использовать *обратный прокси-сервер*, такой как [службы IIS](https://www.iis.net/), [Nginx](http://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="415dd-197">If the app is exposed to the Internet, Kestrel must use a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="415dd-198">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="415dd-198">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="415dd-200">Основная причина использования обратного прокси-сервера для развертываний пограничных серверов, открытых для общего доступа, — это безопасность.</span><span class="sxs-lookup"><span data-stu-id="415dd-200">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="415dd-201">Версии Kestrel 1.x не обладают важными функциями безопасности для защиты от атак из Интернета.</span><span class="sxs-lookup"><span data-stu-id="415dd-201">The 1.x versions of Kestrel don't include important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="415dd-202">Сюда входят, например, соответствующее время ожидания, предельные размеры запроса и максимальное количество одновременных подключений.</span><span class="sxs-lookup"><span data-stu-id="415dd-202">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

::: moniker-end

<span data-ttu-id="415dd-203">Инструкции по настройке Kestrel и сведения о том, когда следует использовать Kestrel в конфигурации обратного прокси-сервера, см. в статье <xref:fundamentals/servers/kestrel>.</span><span class="sxs-lookup"><span data-stu-id="415dd-203">For Kestrel configuration guidance and information on when to use Kestrel in a reverse proxy configuration, see <xref:fundamentals/servers/kestrel>.</span></span>

### <a name="nginx-with-kestrel"></a><span data-ttu-id="415dd-204">Nginx с Kestrel</span><span class="sxs-lookup"><span data-stu-id="415dd-204">Nginx with Kestrel</span></span>

<span data-ttu-id="415dd-205">Сведения о том, как использовать Nginx в Linux в качестве обратного прокси-сервера для Kestrel, см. в <xref:host-and-deploy/linux-nginx>.</span><span class="sxs-lookup"><span data-stu-id="415dd-205">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-nginx>.</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="415dd-206">Apache с Kestrel</span><span class="sxs-lookup"><span data-stu-id="415dd-206">Apache with Kestrel</span></span>

<span data-ttu-id="415dd-207">Сведения о том, как использовать Apache в Linux в качестве обратного прокси-сервера для Kestrel, см. в <xref:host-and-deploy/linux-apache>.</span><span class="sxs-lookup"><span data-stu-id="415dd-207">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-apache>.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="iis-http-server"></a><span data-ttu-id="415dd-208">HTTP-сервер IIS</span><span class="sxs-lookup"><span data-stu-id="415dd-208">IIS HTTP Server</span></span>

<span data-ttu-id="415dd-209">HTTP-сервер IIS — это [внутрипроцессный сервер](#in-process-hosting-model) для служб IIS, который требуется для внутрипроцессных развертываний.</span><span class="sxs-lookup"><span data-stu-id="415dd-209">IIS HTTP Server is an [in-process server](#in-process-hosting-model) for IIS and required for in-process deployments.</span></span> <span data-ttu-id="415dd-210">[Модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module) обрабатывает собственные запросы IIS между HTTP-сервером IIS и службами IIS.</span><span class="sxs-lookup"><span data-stu-id="415dd-210">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) handles native IIS requests between IIS and IIS HTTP Server.</span></span> <span data-ttu-id="415dd-211">Для получения дополнительной информации см. <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="415dd-211">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

## <a name="httpsys"></a><span data-ttu-id="415dd-212">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="415dd-212">HTTP.sys</span></span>

<span data-ttu-id="415dd-213">Если приложение ASP.NET Core запускается в Windows, вместо Kestrel можно использовать HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="415dd-213">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="415dd-214">Как правило, для оптимальной производительности рекомендуется Kestrel.</span><span class="sxs-lookup"><span data-stu-id="415dd-214">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="415dd-215">HTTP.sys можно использовать в сценариях, где приложение имеет доступ к Интернету и необходимые возможности поддерживаются HTTP.sys, но не Kestrel.</span><span class="sxs-lookup"><span data-stu-id="415dd-215">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="415dd-216">Для получения дополнительной информации см. <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="415dd-216">For more information, see <xref:fundamentals/servers/httpsys>.</span></span>

![HTTP.sys взаимодействует с Интернетом напрямую](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="415dd-218">HTTP.sys можно также использовать для приложений, которые имеют доступ только к внутренней сети.</span><span class="sxs-lookup"><span data-stu-id="415dd-218">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![HTTP.sys взаимодействует с внутренней сетью напрямую](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="415dd-220">Инструкции по настройке HTTP.sys см. в статье <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="415dd-220">For HTTP.sys configuration guidance, see <xref:fundamentals/servers/httpsys>.</span></span>

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="415dd-221">Инфраструктура сервера ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="415dd-221">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="415dd-222"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>, доступный в методе `Startup.Configure`, предоставляет свойство <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ServerFeatures> типа <xref:Microsoft.AspNetCore.Http.Features.IFeatureCollection>.</span><span class="sxs-lookup"><span data-stu-id="415dd-222">The <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> available in the `Startup.Configure` method exposes the <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ServerFeatures> property of type <xref:Microsoft.AspNetCore.Http.Features.IFeatureCollection>.</span></span> <span data-ttu-id="415dd-223">Kestrel и HTTP.sys предоставляют только один компонент, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>, но разные реализации сервера могут предоставлять дополнительные возможности.</span><span class="sxs-lookup"><span data-stu-id="415dd-223">Kestrel and HTTP.sys only expose a single feature each, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>, but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="415dd-224">`IServerAddressesFeature` можно использовать для того, чтобы узнать, какой порт в реализации сервера привязан к среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="415dd-224">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="415dd-225">Пользовательские серверы</span><span class="sxs-lookup"><span data-stu-id="415dd-225">Custom servers</span></span>

<span data-ttu-id="415dd-226">Если встроенные серверы не отвечают требованиям приложения, можно создать реализацию пользовательского сервера.</span><span class="sxs-lookup"><span data-stu-id="415dd-226">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="415dd-227">В [руководстве по открытому веб-интерфейсу .NET (OWIN)](xref:fundamentals/owin) демонстрируется запись реализации <xref:Microsoft.AspNetCore.Hosting.Server.IServer> на основе [Nowin](https://github.com/Bobris/Nowin).</span><span class="sxs-lookup"><span data-stu-id="415dd-227">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation.</span></span> <span data-ttu-id="415dd-228">Требуют реализации только интерфейсы компонентов, используемых приложением, но как минимум должны поддерживаться <xref:Microsoft.AspNetCore.Http.Features.IHttpRequestFeature> и <xref:Microsoft.AspNetCore.Http.Features.IHttpResponseFeature>.</span><span class="sxs-lookup"><span data-stu-id="415dd-228">Only the feature interfaces that the app uses require implementation, though at a minimum <xref:Microsoft.AspNetCore.Http.Features.IHttpRequestFeature> and <xref:Microsoft.AspNetCore.Http.Features.IHttpResponseFeature> must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="415dd-229">Запуск сервера</span><span class="sxs-lookup"><span data-stu-id="415dd-229">Server startup</span></span>

<span data-ttu-id="415dd-230">Сервер запускается, когда интегрированная среда разработки (IDE) или редактор запускает приложение:</span><span class="sxs-lookup"><span data-stu-id="415dd-230">The server is launched when the Integrated Development Environment (IDE) or editor starts the app:</span></span>

* <span data-ttu-id="415dd-231">[Visual Studio](https://www.visualstudio.com/vs/) — профили запуска можно использовать для запуска приложения и сервера с помощью [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module) или консоли.</span><span class="sxs-lookup"><span data-stu-id="415dd-231">[Visual Studio](https://www.visualstudio.com/vs/) &ndash; Launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) or the console.</span></span>
* <span data-ttu-id="415dd-232">[Visual Studio Code](https://code.visualstudio.com/) — приложение и сервер запускаются решением [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), которое активирует отладчик CoreCLR.</span><span class="sxs-lookup"><span data-stu-id="415dd-232">[Visual Studio Code](https://code.visualstudio.com/) &ndash; The app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span>
* <span data-ttu-id="415dd-233">[Visual Studio для Mac](https://www.visualstudio.com/vs/mac/) — приложение и сервер запускаются отладчиком [Mono Soft Debugger](https://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span><span class="sxs-lookup"><span data-stu-id="415dd-233">[Visual Studio for Mac](https://www.visualstudio.com/vs/mac/) &ndash; The app and server are started by the [Mono Soft-Mode Debugger](https://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="415dd-234">При запуске приложения из командной строки в папке проекта [dotnet run](/dotnet/core/tools/dotnet-run) запускает приложение и сервер (только Kestrel и HTTP.sys).</span><span class="sxs-lookup"><span data-stu-id="415dd-234">When launching the app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="415dd-235">Конфигурация определяется параметром `-c|--configuration`, который может принимать значение `Debug` (по умолчанию) или `Release`.</span><span class="sxs-lookup"><span data-stu-id="415dd-235">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="415dd-236">Если профили запуска указаны в файле *launchSettings.json*, используйте параметр `--launch-profile <NAME>`, чтобы настроить профиль запуска (например, `Development` или `Production`).</span><span class="sxs-lookup"><span data-stu-id="415dd-236">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="415dd-237">Дополнительные сведения см. в статьях [dotnet run](/dotnet/core/tools/dotnet-run) и [Упаковка дистрибутивов .NET Core](/dotnet/core/build/distribution-packaging).</span><span class="sxs-lookup"><span data-stu-id="415dd-237">For more information, see [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging).</span></span>

## <a name="http2-support"></a><span data-ttu-id="415dd-238">Поддержка HTTP/2</span><span class="sxs-lookup"><span data-stu-id="415dd-238">HTTP/2 support</span></span>

<span data-ttu-id="415dd-239">[HTTP/2](https://httpwg.org/specs/rfc7540.html) поддерживается в ASP.NET Core для следующих сценариев развертывания:</span><span class="sxs-lookup"><span data-stu-id="415dd-239">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="415dd-240">Kestrel</span><span class="sxs-lookup"><span data-stu-id="415dd-240">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="415dd-241">Операционная система</span><span class="sxs-lookup"><span data-stu-id="415dd-241">Operating system</span></span>
    * <span data-ttu-id="415dd-242">Windows Server 2016 / Windows 10 или более поздних версий&dagger;</span><span class="sxs-lookup"><span data-stu-id="415dd-242">Windows Server 2016/Windows 10 or later&dagger;</span></span>
    * <span data-ttu-id="415dd-243">Linux с OpenSSL 1.0.2 или более поздней версии (например, Ubuntu 16.04 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="415dd-243">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="415dd-244">HTTP/2 будет поддерживаться для macOS в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="415dd-244">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="415dd-245">Требуемая версия .NET Framework: .NET Core версии 2.2 или более поздней</span><span class="sxs-lookup"><span data-stu-id="415dd-245">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="415dd-246">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="415dd-246">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="415dd-247">Windows Server 2016 / Windows 10 или более поздних версий</span><span class="sxs-lookup"><span data-stu-id="415dd-247">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="415dd-248">Целевая платформа: неприменимо к развертываниям HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="415dd-248">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="415dd-249">IIS (внутрипроцессный)</span><span class="sxs-lookup"><span data-stu-id="415dd-249">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="415dd-250">Windows Server 2016 / Windows 10 или более поздних версий; IIS 10 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="415dd-250">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="415dd-251">Требуемая версия .NET Framework: .NET Core версии 2.2 или более поздней</span><span class="sxs-lookup"><span data-stu-id="415dd-251">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="415dd-252">IIS (внепроцессный)</span><span class="sxs-lookup"><span data-stu-id="415dd-252">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="415dd-253">Windows Server 2016 / Windows 10 или более поздних версий; IIS 10 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="415dd-253">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="415dd-254">Подключения к пограничным серверам, открытых для общего доступа, выполняются по протоколу HTTP/2, а подключения к серверу Kestrel через обратный прокси-сервер — по протоколу HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="415dd-254">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="415dd-255">Целевая платформа: неприменимо к внепроцессным развертываниям IIS.</span><span class="sxs-lookup"><span data-stu-id="415dd-255">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

<span data-ttu-id="415dd-256">&dagger;Для Kestrel предусмотрена ограниченная поддержка HTTP/2 в Windows Server 2012 R2 и Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="415dd-256">&dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="415dd-257">Поддержка ограничена из-за небольшого числа поддерживаемых комплектов шифров TLS, доступных для этих операционных систем.</span><span class="sxs-lookup"><span data-stu-id="415dd-257">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="415dd-258">Для обеспечения безопасности TLS-подключений может потребоваться сертификат, созданный с использованием алгоритма ECDSA.</span><span class="sxs-lookup"><span data-stu-id="415dd-258">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="415dd-259">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="415dd-259">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="415dd-260">Windows Server 2016 / Windows 10 или более поздних версий</span><span class="sxs-lookup"><span data-stu-id="415dd-260">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="415dd-261">Целевая платформа: неприменимо к развертываниям HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="415dd-261">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="415dd-262">IIS (внепроцессный)</span><span class="sxs-lookup"><span data-stu-id="415dd-262">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="415dd-263">Windows Server 2016 / Windows 10 или более поздних версий; IIS 10 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="415dd-263">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="415dd-264">Подключения к пограничным серверам, открытых для общего доступа, выполняются по протоколу HTTP/2, а подключения к серверу Kestrel через обратный прокси-сервер — по протоколу HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="415dd-264">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="415dd-265">Целевая платформа: неприменимо к внепроцессным развертываниям IIS.</span><span class="sxs-lookup"><span data-stu-id="415dd-265">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="415dd-266">Подключение HTTP/2 должно использовать [согласование протокола уровня приложений (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) и TLS 1.2 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="415dd-266">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="415dd-267">Дополнительные сведения см. в разделах, о конкретных сценариях развертывания сервера.</span><span class="sxs-lookup"><span data-stu-id="415dd-267">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="415dd-268">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="415dd-268">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:fundamentals/servers/httpsys>
