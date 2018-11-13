---
title: Модуль ASP.NET Core
author: guardrex
description: Сведения о том, как модуль ASP.NET Core позволяет веб-серверу Kestrel использовать IIS или IIS Express в качестве обратного прокси-сервера.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 39c1b364f9dab635c79e00561d212c858c0c4395
ms.sourcegitcommit: 09affee3d234cb27ea6fe33bc113b79e68900d22
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191260"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="a885d-103">Модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a885d-103">ASP.NET Core Module</span></span>

<span data-ttu-id="a885d-104">Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra), [Рик Штраль](https://github.com/RickStrahl) (Rick Strahl) и [Крис Росс](https://github.com/Tratcher) (Chris Ross)</span><span class="sxs-lookup"><span data-stu-id="a885d-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a885d-105">Модуль Core ASP.NET позволяет приложениям ASP.NET Core выполняться в рабочем процессе IIS (*внутри процесса*) или за IIS при использовании обратного прокси-сервера (*вне процесса*).</span><span class="sxs-lookup"><span data-stu-id="a885d-105">The ASP.NET Core Module allows ASP.NET Core apps to run in an IIS worker process (*in-process*) or behind IIS in a reverse proxy configuration (*out-of-process*).</span></span> <span data-ttu-id="a885d-106">Служба IIS предоставляет расширенные функции безопасности и управляемости веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="a885d-106">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a885d-107">Модуль Core ASP.NET позволяет приложениям ASP.NET Core выполняться за IIS при использовании обратного прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="a885d-107">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="a885d-108">Служба IIS предоставляет расширенные функции безопасности и управляемости веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="a885d-108">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

<span data-ttu-id="a885d-109">Поддерживаемые версии Windows:</span><span class="sxs-lookup"><span data-stu-id="a885d-109">Supported Windows versions:</span></span>

* <span data-ttu-id="a885d-110">Windows 7 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="a885d-110">Windows 7 or later</span></span>
* <span data-ttu-id="a885d-111">Windows Server 2008 R2 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="a885d-111">Windows Server 2008 R2 or later</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a885d-112">При размещении внутри процесса у модуля есть своя реализация сервера — `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="a885d-112">When hosting in-process, the module has its own server implementation, `IISHttpServer`.</span></span>

<span data-ttu-id="a885d-113">При размещении вне процесса модуль работает только с Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a885d-113">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="a885d-114">Модуль несовместим с [HTTP.sys](xref:fundamentals/servers/httpsys) (ранее — [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="a885d-114">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a885d-115">Модуль работает только с Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a885d-115">The module only works with Kestrel.</span></span> <span data-ttu-id="a885d-116">Модуль несовместим с [HTTP.sys](xref:fundamentals/servers/httpsys) (ранее — [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="a885d-116">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

## <a name="aspnet-core-module-description"></a><span data-ttu-id="a885d-117">Описание модуля ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a885d-117">ASP.NET Core Module description</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a885d-118">Модуль ASP.NET Core имеет собственный модуль IIS, который подключается к конвейеру IIS для выполнения следующих задач:</span><span class="sxs-lookup"><span data-stu-id="a885d-118">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="a885d-119">Размещение приложения ASP.NET Core внутри рабочего процесса IIS (`w3wp.exe`). Это так называемая [модель внутрипроцессного размещения](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="a885d-119">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>

* <span data-ttu-id="a885d-120">Переадресация веб-запросов к серверной части приложения ASP.NET Core на [сервере Kestrel](xref:fundamentals/servers/kestrel). Это [модель размещения вне процесса](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="a885d-120">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="a885d-121">Модель внутрипроцессного размещения</span><span class="sxs-lookup"><span data-stu-id="a885d-121">In-process hosting model</span></span>

<span data-ttu-id="a885d-122">При внутрипроцессном размещении приложение ASP.NET Core выполняется в том же процессе, что и рабочий процесс IIS.</span><span class="sxs-lookup"><span data-stu-id="a885d-122">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="a885d-123">При этом не приходится тратить ресурсы на проксирование запросов через адаптер замыкания на себя, как при использовании модели размещения вне процесса.</span><span class="sxs-lookup"><span data-stu-id="a885d-123">This removes the performance penalty of proxying requests over the loopback adapter when using the out-of-process hosting model.</span></span> <span data-ttu-id="a885d-124">IIS обрабатывает управление процессом с помощью [службы активации процессов Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="a885d-124">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="a885d-125">Модуль ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="a885d-125">The ASP.NET Core Module:</span></span>

* <span data-ttu-id="a885d-126">Инициализирует приложение.</span><span class="sxs-lookup"><span data-stu-id="a885d-126">Performs app initialization.</span></span>
  * <span data-ttu-id="a885d-127">Загружает [CoreCLR](/dotnet/standard/glossary#coreclr).</span><span class="sxs-lookup"><span data-stu-id="a885d-127">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="a885d-128">Вызывает `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="a885d-128">Calls `Program.Main`.</span></span>
* <span data-ttu-id="a885d-129">Управляет жизненным циклом собственного запроса IIS.</span><span class="sxs-lookup"><span data-stu-id="a885d-129">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="a885d-130">На следующей схеме показана связь между IIS, модулем ASP.NET Core и приложением, размещенным внутри процесса.</span><span class="sxs-lookup"><span data-stu-id="a885d-130">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![Модуль ASP.NET Core](aspnet-core-module/_static/ancm-inprocess.png)

<span data-ttu-id="a885d-132">Запрос поступает из Интернета в драйвер HTTP.sys в режиме ядра.</span><span class="sxs-lookup"><span data-stu-id="a885d-132">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="a885d-133">Драйвер направляет собственный запрос к IIS на настроенный порт веб-сайта — обычно 80 (HTTP) или 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="a885d-133">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="a885d-134">Модуль получает собственный запрос и передает управление `IISHttpServer`, который преобразует запрос из собственного в управляемый.</span><span class="sxs-lookup"><span data-stu-id="a885d-134">The module receives the native request and passes control to `IISHttpServer`, which is what converts the request from native to managed.</span></span>

<span data-ttu-id="a885d-135">После того как `IISHttpServer` забирает запрос, он передается в конвейер ПО промежуточного слоя ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a885d-135">After `IISHttpServer` picks up the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="a885d-136">Конвейер ПО промежуточного слоя обрабатывает запрос и передает его в качестве экземпляра `HttpContext` в логику приложения.</span><span class="sxs-lookup"><span data-stu-id="a885d-136">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="a885d-137">Отклик приложения передается обратно в службу IIS, которая отправляет его обратно в HTTP-клиент, инициировавший запрос.</span><span class="sxs-lookup"><span data-stu-id="a885d-137">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="a885d-138">Модель размещения вне процесса</span><span class="sxs-lookup"><span data-stu-id="a885d-138">Out-of-process hosting model</span></span>

<span data-ttu-id="a885d-139">Так как приложения ASP.NET Core выполняются в процессе, отделенном от рабочего процесса IIS, этот модуль обрабатывает управление процессами.</span><span class="sxs-lookup"><span data-stu-id="a885d-139">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="a885d-140">Модуль запускает процесс для приложения ASP.NET Core при поступлении первого запроса и перезапускает приложение при сбое или завершении работы.</span><span class="sxs-lookup"><span data-stu-id="a885d-140">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="a885d-141">Это, по сути, совпадает с поведением приложений, выполняемых внутрипроцессно и управляемых [службой активации процессов Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="a885d-141">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="a885d-142">На следующей схеме показана связь между IIS, модулем ASP.NET Core и приложением, размещенным вне процесса.</span><span class="sxs-lookup"><span data-stu-id="a885d-142">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![Модуль ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="a885d-144">Запросы поступают из Интернета в драйвер HTTP.sys в режиме ядра.</span><span class="sxs-lookup"><span data-stu-id="a885d-144">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="a885d-145">Драйвер направляет запросы к службам IIS на настроенный порт веб-сайта — обычно 80 (HTTP) или 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="a885d-145">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="a885d-146">Модуль перенаправляет запросы Kestrel на случайный порт для приложения, который не является портом 80 или 443.</span><span class="sxs-lookup"><span data-stu-id="a885d-146">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="a885d-147">Модуль задает порт с помощью переменной среды во время запуска, а ПО промежуточного слоя для интеграции IIS настраивает сервер для прослушивания `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="a885d-147">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="a885d-148">Выполняются дополнительные проверки, и запросы не из модуля отклоняются.</span><span class="sxs-lookup"><span data-stu-id="a885d-148">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="a885d-149">Модуль не поддерживает переадресацию по HTTPS, поэтому запросы переадресовываются по протоколу HTTP, даже если были получены IIS по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a885d-149">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="a885d-150">После того как Kestrel забирает запрос из модуля, запрос передается в конвейер ПО промежуточного слоя ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a885d-150">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="a885d-151">Конвейер ПО промежуточного слоя обрабатывает запрос и передает его в качестве экземпляра `HttpContext` в логику приложения.</span><span class="sxs-lookup"><span data-stu-id="a885d-151">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="a885d-152">ПО промежуточного слоя, добавленное интеграцией IIS, обновляет схему, удаленный IP-адрес и базовый путь для переадресации запроса в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a885d-152">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="a885d-153">Отклик приложения передается обратно в службу IIS, которая отправляет его обратно в HTTP-клиент, инициировавший запрос.</span><span class="sxs-lookup"><span data-stu-id="a885d-153">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a885d-154">Модуль ASP.NET Core имеет собственный модуль IIS, который подключается к конвейеру IIS для переадресации веб-запросов в серверные приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a885d-154">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="a885d-155">Так как приложения ASP.NET Core выполняются в процессе, отделенном от рабочего процесса IIS, этот модуль также обрабатывает управление процессами.</span><span class="sxs-lookup"><span data-stu-id="a885d-155">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="a885d-156">Модуль запускает процесс для приложения ASP.NET Core при поступлении первого запроса и перезапускает приложение при сбое.</span><span class="sxs-lookup"><span data-stu-id="a885d-156">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="a885d-157">Это, по сути, совпадает с поведением приложений ASP.NET 4.x, выполняемых внутрипроцессно в IIS и управляемых [службой активации процессов Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="a885d-157">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="a885d-158">На следующей схеме показана связь между IIS, модулем ASP.NET Core и приложением:</span><span class="sxs-lookup"><span data-stu-id="a885d-158">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Модуль ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="a885d-160">Запросы поступают из Интернета в драйвер HTTP.sys в режиме ядра.</span><span class="sxs-lookup"><span data-stu-id="a885d-160">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="a885d-161">Драйвер направляет запросы к службам IIS на настроенный порт веб-сайта — обычно 80 (HTTP) или 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="a885d-161">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="a885d-162">Модуль перенаправляет запросы Kestrel на случайный порт для приложения, который не является портом 80 или 443.</span><span class="sxs-lookup"><span data-stu-id="a885d-162">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="a885d-163">Модуль задает порт с помощью переменной среды во время запуска, а ПО промежуточного слоя для интеграции IIS настраивает сервер для прослушивания `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="a885d-163">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="a885d-164">Выполняются дополнительные проверки, и запросы не из модуля отклоняются.</span><span class="sxs-lookup"><span data-stu-id="a885d-164">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="a885d-165">Модуль не поддерживает переадресацию по HTTPS, поэтому запросы переадресовываются по протоколу HTTP, даже если были получены IIS по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a885d-165">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="a885d-166">После того как Kestrel забирает запрос из модуля, запрос передается в конвейер ПО промежуточного слоя ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a885d-166">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="a885d-167">Конвейер ПО промежуточного слоя обрабатывает запрос и передает его в качестве экземпляра `HttpContext` в логику приложения.</span><span class="sxs-lookup"><span data-stu-id="a885d-167">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="a885d-168">ПО промежуточного слоя, добавленное интеграцией IIS, обновляет схему, удаленный IP-адрес и базовый путь для переадресации запроса в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a885d-168">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="a885d-169">Отклик приложения передается обратно в службу IIS, которая отправляет его обратно в HTTP-клиент, инициировавший запрос.</span><span class="sxs-lookup"><span data-stu-id="a885d-169">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="a885d-170">Многие собственные модули, такие как проверка подлинности Windows, остаются активными.</span><span class="sxs-lookup"><span data-stu-id="a885d-170">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="a885d-171">Дополнительные сведения о модулях IIS, активных в этом модуле, см. в разделе <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="a885d-171">To learn more about IIS modules active with the module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="a885d-172">Модуль ASP.NET Core имеет несколько других функций.</span><span class="sxs-lookup"><span data-stu-id="a885d-172">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="a885d-173">Функции модуля:</span><span class="sxs-lookup"><span data-stu-id="a885d-173">The module can:</span></span>

* <span data-ttu-id="a885d-174">Задание переменных среды для рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="a885d-174">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="a885d-175">Внесение в журнал выходных данных stdout для хранилища файлов с целью устранения неполадок при запуске.</span><span class="sxs-lookup"><span data-stu-id="a885d-175">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="a885d-176">Переадресация токенов проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="a885d-176">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="a885d-177">Как установить и использовать модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a885d-177">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="a885d-178">Подробные инструкции о том, как установить и использовать модуль ASP.NET Core, см. в разделе <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="a885d-178">For detailed instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span> <span data-ttu-id="a885d-179">Сведения о настройке модуля см. в разделе <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="a885d-179">For information on configuring the module, see the <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a885d-180">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a885d-180">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* [<span data-ttu-id="a885d-181">Репозиторий GitHub для модуля ASP.NET Core (исходный код)</span><span class="sxs-lookup"><span data-stu-id="a885d-181">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
