---
title: Модуль ASP.NET Core
author: guardrex
description: Сведения о настройке модуля ASP.NET Core для размещения приложений ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 192e4bf8e970083cc05babcd7fb3cf52985e35bf
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396328"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="2633e-103">Модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2633e-103">ASP.NET Core Module</span></span>

<span data-ttu-id="2633e-104">Авторы: [Том Дайкстра (Tom Dykstra)](https://github.com/tdykstra), [Рик Штраль (Rick Strahl)](https://github.com/RickStrahl), [Крис Росс (Chris Ross)](https://github.com/Tratcher), [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT), [Сураб Ширхатти (Sourabh Shirhatti)](https://twitter.com/sshirhatti), [ Джастин Коталик (Justin Kotalik)](https://github.com/jkotalik) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2633e-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2633e-105">Модуль ASP.NET Core имеет собственный модуль IIS, который подключается к конвейеру IIS для выполнения следующих задач:</span><span class="sxs-lookup"><span data-stu-id="2633e-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="2633e-106">Размещение приложения ASP.NET Core внутри рабочего процесса IIS (`w3wp.exe`). Это так называемая [модель внутрипроцессного размещения](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="2633e-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="2633e-107">Переадресация веб-запросов к серверной части приложения ASP.NET Core на [сервере Kestrel](xref:fundamentals/servers/kestrel). Это [модель размещения вне процесса](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="2633e-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="2633e-108">Поддерживаемые версии Windows:</span><span class="sxs-lookup"><span data-stu-id="2633e-108">Supported Windows versions:</span></span>

* <span data-ttu-id="2633e-109">Windows 7 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="2633e-109">Windows 7 or later</span></span>
* <span data-ttu-id="2633e-110">Windows Server 2008 R2 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="2633e-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="2633e-111">При размещении в процессе модуль использует реализацию внутрипроцессного сервера для IIS — HTTP-сервер IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="2633e-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="2633e-112">При размещении вне процесса модуль работает только с Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2633e-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="2633e-113">Модуль несовместим с [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="2633e-113">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="2633e-114">Модели размещения</span><span class="sxs-lookup"><span data-stu-id="2633e-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="2633e-115">Модель внутрипроцессного размещения</span><span class="sxs-lookup"><span data-stu-id="2633e-115">In-process hosting model</span></span>

<span data-ttu-id="2633e-116">Чтобы настроить приложение для внутрипроцессного размещения, добавьте свойство `<AspNetCoreHostingModel>` к файлу проекта приложения со значением `InProcess` (размещение вне процесса имеет значение `OutOfProcess`):</span><span class="sxs-lookup"><span data-stu-id="2633e-116">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="2633e-117">Модель внутрипроцессного размещения не поддерживается для приложений ASP.NET Core, предназначенных для .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2633e-117">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="2633e-118">Если свойство `<AspNetCoreHostingModel>` отсутствует в файле, значение по умолчанию — `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="2633e-118">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="2633e-119">При внутрипроцессном размещении применимы следующие характеристики:</span><span class="sxs-lookup"><span data-stu-id="2633e-119">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="2633e-120">Вместо сервера [Kestrel](xref:fundamentals/servers/kestrel) используется HTTP-сервер IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="2633e-120">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

* <span data-ttu-id="2633e-121">[Атрибут requestTimeout](#attributes-of-the-aspnetcore-element) не применяется к внутрипроцессному размещению.</span><span class="sxs-lookup"><span data-stu-id="2633e-121">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="2633e-122">Совместное использование пула приложений среди приложений не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="2633e-122">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="2633e-123">Используйте один пул приложений для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="2633e-123">Use one app pool per app.</span></span>

* <span data-ttu-id="2633e-124">При использовании [веб-развертывания](/iis/publish/using-web-deploy/introduction-to-web-deploy) или размещении [файла app_offline.htm в развертывании](xref:host-and-deploy/iis/index#locked-deployment-files) вручную приложение может не завершить работу сразу при наличии открытого соединения.</span><span class="sxs-lookup"><span data-stu-id="2633e-124">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="2633e-125">Например, подключение websocket может задерживать завершение работы приложения.</span><span class="sxs-lookup"><span data-stu-id="2633e-125">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="2633e-126">Архитектура (разрядность) приложения и установленная среда выполнения (x64 или x86) должны соответствовать архитектуре пула приложений.</span><span class="sxs-lookup"><span data-stu-id="2633e-126">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="2633e-127">Если вы настраиваете узел приложения вручную с помощью `WebHostBuilder` (не используя [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) и приложение запускается напрямую на сервере Kestrel (с локальным размещением), вызывайте `UseKestrel` перед вызовом `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="2633e-127">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="2633e-128">Если изменить порядок, узел не запустится.</span><span class="sxs-lookup"><span data-stu-id="2633e-128">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="2633e-129">Обнаружены отключения клиентов.</span><span class="sxs-lookup"><span data-stu-id="2633e-129">Client disconnects are detected.</span></span> <span data-ttu-id="2633e-130">При отключении клиента происходит отмена токена отмены [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*).</span><span class="sxs-lookup"><span data-stu-id="2633e-130">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="2633e-131"><xref:System.IO.Directory.GetCurrentDirectory*> возвращает рабочий каталог процесса, запущенного службами IIS, а не каталог приложения (например, *C:\Windows\System32\inetsrv* для *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="2633e-131"><xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="2633e-132">Пример кода, который задает текущий каталог приложения, см. в разделе [Класс CurrentDirectoryHelpers](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="2633e-132">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="2633e-133">Вызовите метод `SetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="2633e-133">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="2633e-134">Последующие вызовы <xref:System.IO.Directory.GetCurrentDirectory*> возвращают каталог приложения.</span><span class="sxs-lookup"><span data-stu-id="2633e-134">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="2633e-135">Модель размещения вне процесса</span><span class="sxs-lookup"><span data-stu-id="2633e-135">Out-of-process hosting model</span></span>

<span data-ttu-id="2633e-136">Чтобы настроить приложение для размещения вне процесса, используйте один из следующих подходов в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="2633e-136">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="2633e-137">Не указывайте свойство `<AspNetCoreHostingModel>`.</span><span class="sxs-lookup"><span data-stu-id="2633e-137">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="2633e-138">Если свойство `<AspNetCoreHostingModel>` отсутствует в файле, значение по умолчанию — `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="2633e-138">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="2633e-139">Установите для свойства `<AspNetCoreHostingModel>` значение `OutOfProcess` (внутрипроцессное размещение имеет значение `InProcess`):</span><span class="sxs-lookup"><span data-stu-id="2633e-139">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="2633e-140">Сервер [Kestrel](xref:fundamentals/servers/kestrel) используется вместо HTTP-сервера IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="2633e-140">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="2633e-141">Изменения модели размещения</span><span class="sxs-lookup"><span data-stu-id="2633e-141">Hosting model changes</span></span>

<span data-ttu-id="2633e-142">Если параметр `hostingModel` изменяется в файле *web.config* (как описано в разделе [Конфигурация с помощью web.config](#configuration-with-webconfig)), модуль перезапускает рабочий процесс для служб IIS.</span><span class="sxs-lookup"><span data-stu-id="2633e-142">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="2633e-143">Для IIS Express модуль не перезапускает рабочий процесс, а запускает нормальное завершение работы текущего процесса IIS Express.</span><span class="sxs-lookup"><span data-stu-id="2633e-143">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="2633e-144">Следующий запрос для приложения порождает новый процесс IIS Express.</span><span class="sxs-lookup"><span data-stu-id="2633e-144">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="2633e-145">Имя процесса</span><span class="sxs-lookup"><span data-stu-id="2633e-145">Process name</span></span>

<span data-ttu-id="2633e-146">`Process.GetCurrentProcess().ProcessName` сообщает `w3wp`/`iisexpress` (внутри процесса) или `dotnet` (вне процесса).</span><span class="sxs-lookup"><span data-stu-id="2633e-146">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="2633e-147">Модуль ASP.NET Core имеет собственный модуль IIS, который подключается к конвейеру IIS для переадресации веб-запросов в серверные приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2633e-147">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="2633e-148">Поддерживаемые версии Windows:</span><span class="sxs-lookup"><span data-stu-id="2633e-148">Supported Windows versions:</span></span>

* <span data-ttu-id="2633e-149">Windows 7 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="2633e-149">Windows 7 or later</span></span>
* <span data-ttu-id="2633e-150">Windows Server 2008 R2 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="2633e-150">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="2633e-151">Модуль работает только с Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2633e-151">The module only works with Kestrel.</span></span> <span data-ttu-id="2633e-152">Модуль несовместим с [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="2633e-152">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="2633e-153">Так как приложения ASP.NET Core выполняются в процессе, отделенном от рабочего процесса IIS, этот модуль также обрабатывает управление процессами.</span><span class="sxs-lookup"><span data-stu-id="2633e-153">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="2633e-154">Модуль запускает процесс для приложения ASP.NET Core при поступлении первого запроса и перезапускает приложение при сбое.</span><span class="sxs-lookup"><span data-stu-id="2633e-154">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="2633e-155">Это, по сути, совпадает с поведением приложений ASP.NET 4.x, выполняемых внутрипроцессно в IIS и управляемых [службой активации процессов Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="2633e-155">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="2633e-156">На следующей схеме показана связь между IIS, модулем ASP.NET Core и приложением:</span><span class="sxs-lookup"><span data-stu-id="2633e-156">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Модуль ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="2633e-158">Запросы поступают из Интернета в драйвер HTTP.sys в режиме ядра.</span><span class="sxs-lookup"><span data-stu-id="2633e-158">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="2633e-159">Драйвер направляет запросы к службам IIS на настроенный порт веб-сайта — обычно 80 (HTTP) или 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="2633e-159">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="2633e-160">Модуль перенаправляет запросы Kestrel на случайный порт для приложения, отличающийся от порта 80 или 443.</span><span class="sxs-lookup"><span data-stu-id="2633e-160">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="2633e-161">Модуль задает порт с помощью переменной среды во время запуска, а ПО промежуточного слоя для интеграции IIS настраивает сервер для прослушивания `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="2633e-161">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="2633e-162">Выполняются дополнительные проверки, и запросы не из модуля отклоняются.</span><span class="sxs-lookup"><span data-stu-id="2633e-162">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="2633e-163">Модуль не поддерживает переадресацию по HTTPS, поэтому запросы переадресовываются по протоколу HTTP, даже если были получены IIS по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2633e-163">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="2633e-164">После того как Kestrel забирает запрос из модуля, запрос передается в конвейер ПО промежуточного слоя ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2633e-164">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="2633e-165">Конвейер ПО промежуточного слоя обрабатывает запрос и передает его в качестве экземпляра `HttpContext` в логику приложения.</span><span class="sxs-lookup"><span data-stu-id="2633e-165">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="2633e-166">ПО промежуточного слоя, добавленное интеграцией IIS, обновляет схему, удаленный IP-адрес и базовый путь для переадресации запроса в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2633e-166">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="2633e-167">Отклик приложения передается обратно в службу IIS, которая отправляет его обратно в HTTP-клиент, инициировавший запрос.</span><span class="sxs-lookup"><span data-stu-id="2633e-167">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="2633e-168">Многие собственные модули, такие как проверка подлинности Windows, остаются активными.</span><span class="sxs-lookup"><span data-stu-id="2633e-168">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="2633e-169">Дополнительные сведения о модулях IIS, активных с модулем ASP.NET Core, см. в разделе <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="2633e-169">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="2633e-170">Дополнительные возможности модуля ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="2633e-170">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="2633e-171">Задание переменных среды для рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="2633e-171">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="2633e-172">Внесение в журнал выходных данных stdout для хранилища файлов с целью устранения неполадок при запуске.</span><span class="sxs-lookup"><span data-stu-id="2633e-172">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="2633e-173">Переадресация токенов проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="2633e-173">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="2633e-174">Как установить и использовать модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2633e-174">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="2633e-175">Инструкции о том, как установить и использовать модуль ASP.NET Core, см. в разделе <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="2633e-175">For instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="2633e-176">Конфигурация с помощью файла web.config</span><span class="sxs-lookup"><span data-stu-id="2633e-176">Configuration with web.config</span></span>

<span data-ttu-id="2633e-177">Модуль ASP.NET Core настроен с помощью раздела `aspNetCore` узла `system.webServer` файла *web.config* на веб-сайте.</span><span class="sxs-lookup"><span data-stu-id="2633e-177">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="2633e-178">Следующий файл *web.config* публикуется для [зависимого от платформы развертывания](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) и настраивает модуль ASP.NET Core для обработки запросов к веб-сайту.</span><span class="sxs-lookup"><span data-stu-id="2633e-178">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath="dotnet" 
                  arguments=".\MyApp.dll" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="2633e-179">Следующий файл *web.config* опубликован для [автономного развертывания](/dotnet/articles/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="2633e-179">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath=".\MyApp.exe" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="2633e-180">Значение `false` свойства <xref:System.Configuration.SectionInformation.InheritInChildApplications*> указывает, что параметры, заданные в элементе [\<расположение>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location), не наследуются приложениями, которые находятся во вложенном каталоге приложения.</span><span class="sxs-lookup"><span data-stu-id="2633e-180">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="2633e-181">Когда приложение развернуто в [службе приложений Azure](https://azure.microsoft.com/services/app-service/), путь `stdoutLogFile` задан как `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="2633e-181">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="2633e-182">Путь сохраняет журналы stdout в папке *LogFiles*, расположение которой автоматически создается службой.</span><span class="sxs-lookup"><span data-stu-id="2633e-182">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="2633e-183">Сведения о конфигурации дочерних приложений IIS см. здесь: <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="2633e-183">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="2633e-184">Атрибуты элемента aspNetCore</span><span class="sxs-lookup"><span data-stu-id="2633e-184">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="2633e-185">Атрибут</span><span class="sxs-lookup"><span data-stu-id="2633e-185">Attribute</span></span> | <span data-ttu-id="2633e-186">Описание</span><span class="sxs-lookup"><span data-stu-id="2633e-186">Description</span></span> | <span data-ttu-id="2633e-187">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="2633e-187">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="2633e-188">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-188">Optional string attribute.</span></span></p><p><span data-ttu-id="2633e-189">Аргументы для исполняемого файла, указанного в атрибуте **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2633e-189">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="2633e-190">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-190">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2633e-191">Если значение равно true, страница **502.5 — ошибка процесса** подавляется и страница в файле *web.config* с кодом состояния 502 имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="2633e-191">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="2633e-192">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-192">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2633e-193">Если значение равно true, маркер безопасности отправляется дочернему процессу, прослушивающему порт %ASPNETCORE_PORT% как заголовок "MS-ASPNETCORE-WINAUTHTOKEN" каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="2633e-193">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="2633e-194">Этот процесс вызывает CloseHandle по этому маркеру безопасности каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="2633e-194">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="2633e-195">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-195">Optional string attribute.</span></span></p><p><span data-ttu-id="2633e-196">Указывает модель размещения — внутри процесса (`InProcess`) или вне процесса (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="2633e-196">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="2633e-197">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-197">Optional integer attribute.</span></span></p><p><span data-ttu-id="2633e-198">Указывает число экземпляров процесса, заданное в параметре **processPath**, которое может появиться для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="2633e-198">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="2633e-199">&dagger;Для внутрипроцессного размещения существует ограничение: `1`.</span><span class="sxs-lookup"><span data-stu-id="2633e-199">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="2633e-200">По умолчанию: `1`</span><span class="sxs-lookup"><span data-stu-id="2633e-200">Default: `1`</span></span><br><span data-ttu-id="2633e-201">Минимум: `1`</span><span class="sxs-lookup"><span data-stu-id="2633e-201">Min: `1`</span></span><br><span data-ttu-id="2633e-202">Максимум: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="2633e-202">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="2633e-203">Обязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-203">Required string attribute.</span></span></p><p><span data-ttu-id="2633e-204">Путь к исполняемому файлу, который запускает процесс прослушивания HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="2633e-204">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="2633e-205">Поддерживаются относительные пути.</span><span class="sxs-lookup"><span data-stu-id="2633e-205">Relative paths are supported.</span></span> <span data-ttu-id="2633e-206">Если путь начинается с `.`, то начало пути считается относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="2633e-206">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="2633e-207">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-207">Optional integer attribute.</span></span></p><p><span data-ttu-id="2633e-208">Указывает количество сбоев за минуту, которыми может завершиться процесс, указанный в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2633e-208">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="2633e-209">Если этот предел превышен, модуль останавливает запуск процесса на оставшуюся часть минуты.</span><span class="sxs-lookup"><span data-stu-id="2633e-209">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="2633e-210">Не поддерживается для внутрипроцессного размещения.</span><span class="sxs-lookup"><span data-stu-id="2633e-210">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="2633e-211">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="2633e-211">Default: `10`</span></span><br><span data-ttu-id="2633e-212">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="2633e-212">Min: `0`</span></span><br><span data-ttu-id="2633e-213">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="2633e-213">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="2633e-214">Необязательный атрибут timespan.</span><span class="sxs-lookup"><span data-stu-id="2633e-214">Optional timespan attribute.</span></span></p><p><span data-ttu-id="2633e-215">Указывает продолжительность, на протяжении которой модуль ASP.NET Core ожидает ответа от процесса, прослушивающего порт %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="2633e-215">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="2633e-216">В версиях модуля ASP.NET Core, поставляемых с выпуском ASP.NET Core 2.1 или новее, атрибут `requestTimeout` указывается в часах, минутах и секундах.</span><span class="sxs-lookup"><span data-stu-id="2633e-216">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="2633e-217">Не применяется к внутрипроцессному размещению.</span><span class="sxs-lookup"><span data-stu-id="2633e-217">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="2633e-218">Для внутрипроцессного размещения модуль ожидает, пока приложение не обработает запрос.</span><span class="sxs-lookup"><span data-stu-id="2633e-218">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="2633e-219">По умолчанию: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="2633e-219">Default: `00:02:00`</span></span><br><span data-ttu-id="2633e-220">Минимум: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="2633e-220">Min: `00:00:00`</span></span><br><span data-ttu-id="2633e-221">Максимум: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="2633e-221">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="2633e-222">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-222">Optional integer attribute.</span></span></p><p><span data-ttu-id="2633e-223">Длительность ожидания модуля в секундах, пока произойдет правильное выключение исполняемого файла при обнаружении файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="2633e-223">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="2633e-224">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="2633e-224">Default: `10`</span></span><br><span data-ttu-id="2633e-225">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="2633e-225">Min: `0`</span></span><br><span data-ttu-id="2633e-226">Максимум: `600`</span><span class="sxs-lookup"><span data-stu-id="2633e-226">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="2633e-227">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-227">Optional integer attribute.</span></span></p><p><span data-ttu-id="2633e-228">Время в секундах, которое модуль ожидает, пока запустится процесс прослушивания порта исполняемого файла.</span><span class="sxs-lookup"><span data-stu-id="2633e-228">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="2633e-229">Если этот предел превышен, модуль завершает процесс.</span><span class="sxs-lookup"><span data-stu-id="2633e-229">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="2633e-230">Модуль пытается перезапустить процесс при получении нового запроса и будет продолжать пытаться перезапустить процесс для последующих входящих запросов, если не удается запустить приложение определенное в атрибуте **rapidFailsPerMinute** количество раз за последнюю минуту.</span><span class="sxs-lookup"><span data-stu-id="2633e-230">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="2633e-231">Значение 0 (ноль) **не** считается бесконечным временем ожидания.</span><span class="sxs-lookup"><span data-stu-id="2633e-231">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="2633e-232">По умолчанию: `120`</span><span class="sxs-lookup"><span data-stu-id="2633e-232">Default: `120`</span></span><br><span data-ttu-id="2633e-233">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="2633e-233">Min: `0`</span></span><br><span data-ttu-id="2633e-234">Максимум: `3600`</span><span class="sxs-lookup"><span data-stu-id="2633e-234">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="2633e-235">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-235">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2633e-236">Если значение равно true, **stdout** и **stderr** для процесса, указанного в атрибуте **processPath**, перенаправляются к файлу, заданному в атрибуте **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="2633e-236">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="2633e-237">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-237">Optional string attribute.</span></span></p><p><span data-ttu-id="2633e-238">Указывает относительный или абсолютный путь к файлу, для которого регистрируются **stdout** и **stderr** из процесса, указанного в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2633e-238">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="2633e-239">Относительные пути задаются относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="2633e-239">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="2633e-240">Любой путь, начинающийся с `.`, относится к корневому каталогу веб-сайта, а все остальные пути рассматриваются как абсолютные пути.</span><span class="sxs-lookup"><span data-stu-id="2633e-240">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="2633e-241">Все папки, указанные в пути, создаются модулем при создании файла журнала.</span><span class="sxs-lookup"><span data-stu-id="2633e-241">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="2633e-242">С помощью разделителей подчеркивания метка времени, идентификатор процесса и расширение файла (*.log*) добавляются к последнему сегменту пути журнала **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="2633e-242">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="2633e-243">Если в качестве значения задано значение `.\logs\stdout`, например, журнал stdout сохраняется как *stdout_20180205194132_1934.log* в папке *журналов* с датой 5 февраля 2018 г. в 19:41:32 с идентификатором процесса 1934.</span><span class="sxs-lookup"><span data-stu-id="2633e-243">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="2633e-244">Атрибут</span><span class="sxs-lookup"><span data-stu-id="2633e-244">Attribute</span></span> | <span data-ttu-id="2633e-245">Описание</span><span class="sxs-lookup"><span data-stu-id="2633e-245">Description</span></span> | <span data-ttu-id="2633e-246">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="2633e-246">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="2633e-247">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-247">Optional string attribute.</span></span></p><p><span data-ttu-id="2633e-248">Аргументы для исполняемого файла, указанного в атрибуте **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2633e-248">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="2633e-249">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-249">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2633e-250">Если значение равно true, страница **502.5 — ошибка процесса** подавляется и страница в файле *web.config* с кодом состояния 502 имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="2633e-250">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="2633e-251">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-251">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2633e-252">Если значение равно true, маркер безопасности отправляется дочернему процессу, прослушивающему порт %ASPNETCORE_PORT% как заголовок "MS-ASPNETCORE-WINAUTHTOKEN" каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="2633e-252">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="2633e-253">Этот процесс вызывает CloseHandle по этому маркеру безопасности каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="2633e-253">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="2633e-254">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-254">Optional integer attribute.</span></span></p><p><span data-ttu-id="2633e-255">Указывает число экземпляров процесса, заданное в параметре **processPath**, которое может появиться для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="2633e-255">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="2633e-256">По умолчанию: `1`</span><span class="sxs-lookup"><span data-stu-id="2633e-256">Default: `1`</span></span><br><span data-ttu-id="2633e-257">Минимум: `1`</span><span class="sxs-lookup"><span data-stu-id="2633e-257">Min: `1`</span></span><br><span data-ttu-id="2633e-258">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="2633e-258">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="2633e-259">Обязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-259">Required string attribute.</span></span></p><p><span data-ttu-id="2633e-260">Путь к исполняемому файлу, который запускает процесс прослушивания HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="2633e-260">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="2633e-261">Поддерживаются относительные пути.</span><span class="sxs-lookup"><span data-stu-id="2633e-261">Relative paths are supported.</span></span> <span data-ttu-id="2633e-262">Если путь начинается с `.`, то начало пути считается относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="2633e-262">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="2633e-263">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-263">Optional integer attribute.</span></span></p><p><span data-ttu-id="2633e-264">Указывает количество сбоев за минуту, которыми может завершиться процесс, указанный в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2633e-264">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="2633e-265">Если этот предел превышен, модуль останавливает запуск процесса на оставшуюся часть минуты.</span><span class="sxs-lookup"><span data-stu-id="2633e-265">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="2633e-266">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="2633e-266">Default: `10`</span></span><br><span data-ttu-id="2633e-267">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="2633e-267">Min: `0`</span></span><br><span data-ttu-id="2633e-268">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="2633e-268">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="2633e-269">Необязательный атрибут timespan.</span><span class="sxs-lookup"><span data-stu-id="2633e-269">Optional timespan attribute.</span></span></p><p><span data-ttu-id="2633e-270">Указывает продолжительность, на протяжении которой модуль ASP.NET Core ожидает ответа от процесса, прослушивающего порт %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="2633e-270">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="2633e-271">В версиях модуля ASP.NET Core, поставляемых с выпуском ASP.NET Core 2.1 или новее, атрибут `requestTimeout` указывается в часах, минутах и секундах.</span><span class="sxs-lookup"><span data-stu-id="2633e-271">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="2633e-272">По умолчанию: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="2633e-272">Default: `00:02:00`</span></span><br><span data-ttu-id="2633e-273">Минимум: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="2633e-273">Min: `00:00:00`</span></span><br><span data-ttu-id="2633e-274">Максимум: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="2633e-274">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="2633e-275">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-275">Optional integer attribute.</span></span></p><p><span data-ttu-id="2633e-276">Длительность ожидания модуля в секундах, пока произойдет правильное выключение исполняемого файла при обнаружении файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="2633e-276">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="2633e-277">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="2633e-277">Default: `10`</span></span><br><span data-ttu-id="2633e-278">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="2633e-278">Min: `0`</span></span><br><span data-ttu-id="2633e-279">Максимум: `600`</span><span class="sxs-lookup"><span data-stu-id="2633e-279">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="2633e-280">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-280">Optional integer attribute.</span></span></p><p><span data-ttu-id="2633e-281">Время в секундах, которое модуль ожидает, пока запустится процесс прослушивания порта исполняемого файла.</span><span class="sxs-lookup"><span data-stu-id="2633e-281">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="2633e-282">Если этот предел превышен, модуль завершает процесс.</span><span class="sxs-lookup"><span data-stu-id="2633e-282">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="2633e-283">Модуль пытается перезапустить процесс при получении нового запроса и будет продолжать пытаться перезапустить процесс для последующих входящих запросов, если не удается запустить приложение определенное в атрибуте **rapidFailsPerMinute** количество раз за последнюю минуту.</span><span class="sxs-lookup"><span data-stu-id="2633e-283">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="2633e-284">Значение 0 (ноль) **не** считается бесконечным временем ожидания.</span><span class="sxs-lookup"><span data-stu-id="2633e-284">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="2633e-285">По умолчанию: `120`</span><span class="sxs-lookup"><span data-stu-id="2633e-285">Default: `120`</span></span><br><span data-ttu-id="2633e-286">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="2633e-286">Min: `0`</span></span><br><span data-ttu-id="2633e-287">Максимум: `3600`</span><span class="sxs-lookup"><span data-stu-id="2633e-287">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="2633e-288">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-288">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2633e-289">Если значение равно true, **stdout** и **stderr** для процесса, указанного в атрибуте **processPath**, перенаправляются к файлу, заданному в атрибуте **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="2633e-289">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="2633e-290">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-290">Optional string attribute.</span></span></p><p><span data-ttu-id="2633e-291">Указывает относительный или абсолютный путь к файлу, для которого регистрируются **stdout** и **stderr** из процесса, указанного в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2633e-291">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="2633e-292">Относительные пути задаются относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="2633e-292">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="2633e-293">Любой путь, начинающийся с `.`, относится к корневому каталогу веб-сайта, а все остальные пути рассматриваются как абсолютные пути.</span><span class="sxs-lookup"><span data-stu-id="2633e-293">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="2633e-294">Все папки, указанные в пути, должны существовать, чтобы модуль мог создать файл журнала.</span><span class="sxs-lookup"><span data-stu-id="2633e-294">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="2633e-295">С помощью разделителей подчеркивания метка времени, идентификатор процесса и расширение файла (*.log*) добавляются к последнему сегменту пути журнала **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="2633e-295">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="2633e-296">Если в качестве значения задано значение `.\logs\stdout`, например, журнал stdout сохраняется как *stdout_20180205194132_1934.log* в папке *журналов* с датой 5 февраля 2018 г. в 19:41:32 с идентификатором процесса 1934.</span><span class="sxs-lookup"><span data-stu-id="2633e-296">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="2633e-297">Атрибут</span><span class="sxs-lookup"><span data-stu-id="2633e-297">Attribute</span></span> | <span data-ttu-id="2633e-298">Описание</span><span class="sxs-lookup"><span data-stu-id="2633e-298">Description</span></span> | <span data-ttu-id="2633e-299">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="2633e-299">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="2633e-300">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-300">Optional string attribute.</span></span></p><p><span data-ttu-id="2633e-301">Аргументы для исполняемого файла, указанного в атрибуте **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2633e-301">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="2633e-302">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-302">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2633e-303">Если значение равно true, страница **502.5 — ошибка процесса** подавляется и страница в файле *web.config* с кодом состояния 502 имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="2633e-303">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="2633e-304">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-304">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2633e-305">Если значение равно true, маркер безопасности отправляется дочернему процессу, прослушивающему порт %ASPNETCORE_PORT% как заголовок "MS-ASPNETCORE-WINAUTHTOKEN" каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="2633e-305">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="2633e-306">Этот процесс вызывает CloseHandle по этому маркеру безопасности каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="2633e-306">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="2633e-307">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-307">Optional integer attribute.</span></span></p><p><span data-ttu-id="2633e-308">Указывает число экземпляров процесса, заданное в параметре **processPath**, которое может появиться для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="2633e-308">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="2633e-309">По умолчанию: `1`</span><span class="sxs-lookup"><span data-stu-id="2633e-309">Default: `1`</span></span><br><span data-ttu-id="2633e-310">Минимум: `1`</span><span class="sxs-lookup"><span data-stu-id="2633e-310">Min: `1`</span></span><br><span data-ttu-id="2633e-311">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="2633e-311">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="2633e-312">Обязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-312">Required string attribute.</span></span></p><p><span data-ttu-id="2633e-313">Путь к исполняемому файлу, который запускает процесс прослушивания HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="2633e-313">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="2633e-314">Поддерживаются относительные пути.</span><span class="sxs-lookup"><span data-stu-id="2633e-314">Relative paths are supported.</span></span> <span data-ttu-id="2633e-315">Если путь начинается с `.`, то начало пути считается относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="2633e-315">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="2633e-316">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-316">Optional integer attribute.</span></span></p><p><span data-ttu-id="2633e-317">Указывает количество сбоев за минуту, которыми может завершиться процесс, указанный в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2633e-317">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="2633e-318">Если этот предел превышен, модуль останавливает запуск процесса на оставшуюся часть минуты.</span><span class="sxs-lookup"><span data-stu-id="2633e-318">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="2633e-319">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="2633e-319">Default: `10`</span></span><br><span data-ttu-id="2633e-320">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="2633e-320">Min: `0`</span></span><br><span data-ttu-id="2633e-321">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="2633e-321">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="2633e-322">Необязательный атрибут timespan.</span><span class="sxs-lookup"><span data-stu-id="2633e-322">Optional timespan attribute.</span></span></p><p><span data-ttu-id="2633e-323">Указывает продолжительность, на протяжении которой модуль ASP.NET Core ожидает ответа от процесса, прослушивающего порт %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="2633e-323">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="2633e-324">В версиях модуля ASP.NET Core, поставляемых вместе с выпуском ASP.NET Core 2.0 или более ранними версиями, атрибут `requestTimeout` должен был быть задан в целых минутах (в противном случае для него по умолчанию задано значение 2 минуты).</span><span class="sxs-lookup"><span data-stu-id="2633e-324">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="2633e-325">По умолчанию: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="2633e-325">Default: `00:02:00`</span></span><br><span data-ttu-id="2633e-326">Минимум: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="2633e-326">Min: `00:00:00`</span></span><br><span data-ttu-id="2633e-327">Максимум: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="2633e-327">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="2633e-328">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-328">Optional integer attribute.</span></span></p><p><span data-ttu-id="2633e-329">Длительность ожидания модуля в секундах, пока произойдет правильное выключение исполняемого файла при обнаружении файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="2633e-329">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="2633e-330">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="2633e-330">Default: `10`</span></span><br><span data-ttu-id="2633e-331">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="2633e-331">Min: `0`</span></span><br><span data-ttu-id="2633e-332">Максимум: `600`</span><span class="sxs-lookup"><span data-stu-id="2633e-332">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="2633e-333">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-333">Optional integer attribute.</span></span></p><p><span data-ttu-id="2633e-334">Время в секундах, которое модуль ожидает, пока запустится процесс прослушивания порта исполняемого файла.</span><span class="sxs-lookup"><span data-stu-id="2633e-334">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="2633e-335">Если этот предел превышен, модуль завершает процесс.</span><span class="sxs-lookup"><span data-stu-id="2633e-335">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="2633e-336">Модуль пытается перезапустить процесс при получении нового запроса и будет продолжать пытаться перезапустить процесс для последующих входящих запросов, если не удается запустить приложение определенное в атрибуте **rapidFailsPerMinute** количество раз за последнюю минуту.</span><span class="sxs-lookup"><span data-stu-id="2633e-336">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="2633e-337">По умолчанию: `120`</span><span class="sxs-lookup"><span data-stu-id="2633e-337">Default: `120`</span></span><br><span data-ttu-id="2633e-338">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="2633e-338">Min: `0`</span></span><br><span data-ttu-id="2633e-339">Максимум: `3600`</span><span class="sxs-lookup"><span data-stu-id="2633e-339">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="2633e-340">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-340">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2633e-341">Если значение равно true, **stdout** и **stderr** для процесса, указанного в атрибуте **processPath**, перенаправляются к файлу, заданному в атрибуте **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="2633e-341">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="2633e-342">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="2633e-342">Optional string attribute.</span></span></p><p><span data-ttu-id="2633e-343">Указывает относительный или абсолютный путь к файлу, для которого регистрируются **stdout** и **stderr** из процесса, указанного в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2633e-343">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="2633e-344">Относительные пути задаются относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="2633e-344">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="2633e-345">Любой путь, начинающийся с `.`, относится к корневому каталогу веб-сайта, а все остальные пути рассматриваются как абсолютные пути.</span><span class="sxs-lookup"><span data-stu-id="2633e-345">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="2633e-346">Все папки, указанные в пути, должны существовать, чтобы модуль мог создать файл журнала.</span><span class="sxs-lookup"><span data-stu-id="2633e-346">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="2633e-347">С помощью разделителей подчеркивания метка времени, идентификатор процесса и расширение файла (*.log*) добавляются к последнему сегменту пути журнала **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="2633e-347">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="2633e-348">Если в качестве значения задано значение `.\logs\stdout`, например, журнал stdout сохраняется как *stdout_20180205194132_1934.log* в папке *журналов* с датой 5 февраля 2018 г. в 19:41:32 с идентификатором процесса 1934.</span><span class="sxs-lookup"><span data-stu-id="2633e-348">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="2633e-349">Настройка переменных среды</span><span class="sxs-lookup"><span data-stu-id="2633e-349">Setting environment variables</span></span>

<span data-ttu-id="2633e-350">Переменные среды для процесса можно указать в атрибуте `processPath`.</span><span class="sxs-lookup"><span data-stu-id="2633e-350">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="2633e-351">Укажите переменную среды с дочерним элементом `environmentVariable` элемента коллекции `environmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="2633e-351">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="2633e-352">Переменные среды, установленные в этом разделе, имеют приоритет над переменными системной среды.</span><span class="sxs-lookup"><span data-stu-id="2633e-352">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="2633e-353">В следующем примере устанавливаются две переменные среды.</span><span class="sxs-lookup"><span data-stu-id="2633e-353">The following example sets two environment variables.</span></span> <span data-ttu-id="2633e-354">`ASPNETCORE_ENVIRONMENT` настраивает среду приложения для `Development`.</span><span class="sxs-lookup"><span data-stu-id="2633e-354">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="2633e-355">Разработчик может временно задать это значение в файле *web.config*, чтобы принудительно загрузить [Страницу исключений для разработчиков](xref:fundamentals/error-handling) при отладке исключения приложения.</span><span class="sxs-lookup"><span data-stu-id="2633e-355">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="2633e-356">`CONFIG_DIR` — пример пользовательской переменной среды, где разработчик написал код, который считывает значение при запуске, чтобы сформировать путь для загрузки файла конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="2633e-356">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="2633e-357">Установите только переменную среды `ASPNETCORE_ENVIRONMENT` для `Development` на серверах промежуточных процессов и тестирования, которые недоступны для ненадежных сетей, таких как Интернет.</span><span class="sxs-lookup"><span data-stu-id="2633e-357">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="2633e-358">App_offline.htm</span><span class="sxs-lookup"><span data-stu-id="2633e-358">app_offline.htm</span></span>

<span data-ttu-id="2633e-359">Если в корневом каталоге приложения обнаружен файл с именем *app_offline.htm*, модуль ASP.NET Core пытается корректно закрыть приложение и прекратить обработку входящих запросов.</span><span class="sxs-lookup"><span data-stu-id="2633e-359">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="2633e-360">Если приложение по-прежнему выполняется через количество секунд, определенное атрибутом `shutdownTimeLimit`, модуль ASP.NET Core завершает выполняющийся процесс.</span><span class="sxs-lookup"><span data-stu-id="2633e-360">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="2633e-361">Хотя файл *app_offline.htm* присутствует, модуль ASP.NET Core отвечает на запросы, отправляя назад содержимое файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="2633e-361">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="2633e-362">Когда *app_offline.htm* файл удаляется, следующий запрос запускает приложение.</span><span class="sxs-lookup"><span data-stu-id="2633e-362">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2633e-363">При использовании модели размещения вне процесса приложение может не завершать работу немедленно при наличии открытого соединения.</span><span class="sxs-lookup"><span data-stu-id="2633e-363">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="2633e-364">Например, подключение websocket может задерживать завершение работы приложения.</span><span class="sxs-lookup"><span data-stu-id="2633e-364">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="2633e-365">Страница ошибок запуска</span><span class="sxs-lookup"><span data-stu-id="2633e-365">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2633e-366">Если при внутри- или внепроцессном размещении происходит сбой запуска приложения, открываются страницы пользовательских сообщений об ошибках.</span><span class="sxs-lookup"><span data-stu-id="2633e-366">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="2633e-367">Если модулю ASP.NET Core не удается найти внутри- или внепроцессный обработчик запросов, откроется страница кода состояния *500.0 — ошибка загрузки внутри- или внепроцессного обработчика запросов*.</span><span class="sxs-lookup"><span data-stu-id="2633e-367">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="2633e-368">Если в модели размещения внутри процесса модулю ASP.NET Core не удается запустить приложение, откроется страница кода состояния *500.30 — ошибка запуска*.</span><span class="sxs-lookup"><span data-stu-id="2633e-368">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="2633e-369">Если в модели размещения вне процесса модулю ASP.NET Core не удается запустить серверный процесс или начинается серверный процесс, но ему не удается прослушать настроенный порт, появится страница кода состояния *502.5 — ошибка процесса*.</span><span class="sxs-lookup"><span data-stu-id="2633e-369">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="2633e-370">Чтобы подавить отображение этой странице и вернуться к странице IIS кода состояния 5xx по умолчанию, используйте атрибут `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="2633e-370">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="2633e-371">Дополнительные сведения о настройке пользовательских сообщений об ошибках см. в разделе [Ошибки HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="2633e-371">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="2633e-372">Если модулю ASP.NET Core не удается запустить серверный процесс или начинается серверный процесс, но ему не удается прослушать настроенный порт, появится страница кода состояния *502.5 — ошибка процесса*.</span><span class="sxs-lookup"><span data-stu-id="2633e-372">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="2633e-373">Чтобы подавить эту страницу и вернуться к странице IIS кода состояния 502 по умолчанию, используйте атрибут `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="2633e-373">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="2633e-374">Дополнительные сведения о настройке пользовательских сообщений об ошибках см. в разделе [Ошибки HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="2633e-374">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Страница кода состояния "502.5 — ошибка процесса"](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="2633e-376">Создание и перенаправление журнала</span><span class="sxs-lookup"><span data-stu-id="2633e-376">Log creation and redirection</span></span>

<span data-ttu-id="2633e-377">Модуль ASP.NET Core перенаправляет выходные потоки консоли stdout и stderr на диск, если заданы атрибуты `stdoutLogEnabled` и `stdoutLogFile` элемента `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="2633e-377">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="2633e-378">Все папки, указанные в пути `stdoutLogFile`, создаются модулем при создании файла журнала.</span><span class="sxs-lookup"><span data-stu-id="2633e-378">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="2633e-379">Пул приложений должен иметь доступ на запись в папку, где записываются журналы (используйте атрибут `IIS AppPool\<app_pool_name>` для предоставления разрешения на запись).</span><span class="sxs-lookup"><span data-stu-id="2633e-379">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="2633e-380">Журналы не выполняют циклический сдвиг, пока не произойдет процесс перезапуска или перезагрузки.</span><span class="sxs-lookup"><span data-stu-id="2633e-380">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="2633e-381">Администратор несет ответственность за ограничение дискового пространства, которое потребляют журналы.</span><span class="sxs-lookup"><span data-stu-id="2633e-381">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="2633e-382">Использование журнала stdout рекомендуется только для устранения неполадок при запуске приложений.</span><span class="sxs-lookup"><span data-stu-id="2633e-382">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="2633e-383">Не используйте журнал stdout для ведения общего журнала приложений.</span><span class="sxs-lookup"><span data-stu-id="2633e-383">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="2633e-384">Для обычного входа в приложение ASP.NET Core используйте библиотеку ведения журналов, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов.</span><span class="sxs-lookup"><span data-stu-id="2633e-384">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="2633e-385">Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="2633e-385">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="2633e-386">При создании файла журнала автоматически добавляются отметка времени и расширение файла.</span><span class="sxs-lookup"><span data-stu-id="2633e-386">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="2633e-387">Имя файла журнала составляется путем добавления метки времени, идентификатора процесса и расширения файла (*.log*) к последнему сегменту атрибута пути `stdoutLogFile` (обычно *stdout*) с символами подчеркивания в качестве разделителей.</span><span class="sxs-lookup"><span data-stu-id="2633e-387">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="2633e-388">Если атрибут пути `stdoutLogFile` заканчивается элементом *stdout*, журнал приложения с идентификатором 1934, созданный 5 февраля 2018 г. в 19:42:32, будет иметь имя *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="2633e-388">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2633e-389">Если `stdoutLogEnabled` имеет значение false, возникающие при запуске приложения ошибки записываются и передаются в журнал событий (макс. 30 КБ).</span><span class="sxs-lookup"><span data-stu-id="2633e-389">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="2633e-390">После запуска все дополнительные журналы удаляются.</span><span class="sxs-lookup"><span data-stu-id="2633e-390">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="2633e-391">В следующем примере элемент `aspNetCore` настраивает ведение журнала stdout для приложения, размещенного в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="2633e-391">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="2633e-392">Для локального ведения журнала допустим локальный или общий сетевой путь.</span><span class="sxs-lookup"><span data-stu-id="2633e-392">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="2633e-393">Убедитесь, что идентификатор пользователя AppPool имеет разрешение на запись по указанному пути.</span><span class="sxs-lookup"><span data-stu-id="2633e-393">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="2633e-394">Расширенные журналы диагностики</span><span class="sxs-lookup"><span data-stu-id="2633e-394">Enhanced diagnostic logs</span></span>

<span data-ttu-id="2633e-395">Модуль ASP.NET Core предоставляет возможности настройки, позволяющие работать с расширенными журналами диагностики.</span><span class="sxs-lookup"><span data-stu-id="2633e-395">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="2633e-396">Добавьте элемент `<handlerSettings>` в элемент `<aspNetCore>` в файле *web.config*. Задайте параметру `debugLevel` значение `TRACE`, чтобы обеспечить высокую точность диагностических сведений:</span><span class="sxs-lookup"><span data-stu-id="2633e-396">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="debugFile" value="aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="2633e-397">Значения уровня отладки (`debugLevel`) могут включать уровень и расположение.</span><span class="sxs-lookup"><span data-stu-id="2633e-397">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="2633e-398">Уровни (в порядке возрастания степени детализации):</span><span class="sxs-lookup"><span data-stu-id="2633e-398">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="2633e-399">ОШИБКА</span><span class="sxs-lookup"><span data-stu-id="2633e-399">ERROR</span></span>
* <span data-ttu-id="2633e-400">ПРЕДУПРЕЖДЕНИЕ</span><span class="sxs-lookup"><span data-stu-id="2633e-400">WARNING</span></span>
* <span data-ttu-id="2633e-401">ИНФОРМАЦИЯ</span><span class="sxs-lookup"><span data-stu-id="2633e-401">INFO</span></span>
* <span data-ttu-id="2633e-402">TRACE</span><span class="sxs-lookup"><span data-stu-id="2633e-402">TRACE</span></span>

<span data-ttu-id="2633e-403">Расположения (допускаются несколько расположений):</span><span class="sxs-lookup"><span data-stu-id="2633e-403">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="2633e-404">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="2633e-404">CONSOLE</span></span>
* <span data-ttu-id="2633e-405">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="2633e-405">EVENTLOG</span></span>
* <span data-ttu-id="2633e-406">ФАЙЛ</span><span class="sxs-lookup"><span data-stu-id="2633e-406">FILE</span></span>

<span data-ttu-id="2633e-407">Параметры обработчика могут быть указаны с помощью переменных среды:</span><span class="sxs-lookup"><span data-stu-id="2633e-407">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="2633e-408">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Путь к файлу журнала отладки.</span><span class="sxs-lookup"><span data-stu-id="2633e-408">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="2633e-409">(По умолчанию: *aspnetcore debug.log*)</span><span class="sxs-lookup"><span data-stu-id="2633e-409">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="2633e-410">`ASPNETCORE_MODULE_DEBUG` &ndash; Параметр уровня отладки.</span><span class="sxs-lookup"><span data-stu-id="2633e-410">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="2633e-411">**Не** оставляйте ведение журнала отладки включенным в развертывании дольше того времени, которое требуется для устранения проблемы.</span><span class="sxs-lookup"><span data-stu-id="2633e-411">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="2633e-412">Размер журнала не ограничен.</span><span class="sxs-lookup"><span data-stu-id="2633e-412">The size of the log isn't limited.</span></span> <span data-ttu-id="2633e-413">Если оставить журнал отладки включенным, он может исчерпать все доступное место на диске и привести к сбою сервера или службы приложений.</span><span class="sxs-lookup"><span data-stu-id="2633e-413">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="2633e-414">См. пример элемента `aspNetCore` в файле *web.config* в разделе [Конфигурация с помощью файла web.config](#configuration-with-webconfig).</span><span class="sxs-lookup"><span data-stu-id="2633e-414">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="2633e-415">Конфигурация прокси-сервера использует протокол HTTP и токен связывания</span><span class="sxs-lookup"><span data-stu-id="2633e-415">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2633e-416">*Применяется только к размещению вне процесса.*</span><span class="sxs-lookup"><span data-stu-id="2633e-416">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="2633e-417">Прокси-сервер, созданный между модулем ASP.NET Core и Kestrel, использует протокол HTTP.</span><span class="sxs-lookup"><span data-stu-id="2633e-417">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="2633e-418">Использование HTTP позволяет оптимизировать производительность, так как трафик между модулем и Kestrel передается на петлевой адрес в сетевом интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="2633e-418">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="2633e-419">Отсутствует риск перехвата трафика между модулем и Kestrel из расположения на сервере.</span><span class="sxs-lookup"><span data-stu-id="2633e-419">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="2633e-420">Токен связывания гарантирует, что полученные Kestrel запросы были переданы службами IIS, а не из какого-либо другого источника.</span><span class="sxs-lookup"><span data-stu-id="2633e-420">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="2633e-421">Этот токен создается и задается модулем в переменную среды (`ASPNETCORE_TOKEN`).</span><span class="sxs-lookup"><span data-stu-id="2633e-421">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="2633e-422">Он также задается в заголовке (`MS-ASPNETCORE-TOKEN`) каждого запроса, переданного через прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="2633e-422">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="2633e-423">ПО промежуточного слоя IIS проверяет каждый получаемый запрос, чтобы убедиться, что заголовок с токеном связывания соответствует значению переменной среды.</span><span class="sxs-lookup"><span data-stu-id="2633e-423">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="2633e-424">Если значения токена не совпадают, запрос заносится в журнал и отклоняется.</span><span class="sxs-lookup"><span data-stu-id="2633e-424">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="2633e-425">Переменная среды с токеном связывания и трафик между модулем и Kestrel недоступны из расположения на сервере.</span><span class="sxs-lookup"><span data-stu-id="2633e-425">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="2633e-426">Не зная значение токена связывания, злоумышленник не может отправлять запросы, обходящие проверку в ПО промежуточного слоя IIS.</span><span class="sxs-lookup"><span data-stu-id="2633e-426">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="2633e-427">Модуль ASP.NET Core с общей конфигурацией IIS</span><span class="sxs-lookup"><span data-stu-id="2633e-427">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="2633e-428">Программа установки модуля ASP.NET Core запускается с правами **системной** учетной записи.</span><span class="sxs-lookup"><span data-stu-id="2633e-428">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="2633e-429">Поскольку учетная запись локальной системы не имеет разрешения на изменение пути к общей папке, используемого общей конфигурацией IIS, установщик получает ошибку отказа в доступе при попытке настроить параметры модуля в файле *applicationHost.config* общей папки.</span><span class="sxs-lookup"><span data-stu-id="2633e-429">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="2633e-430">При использовании общей конфигурации IIS выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="2633e-430">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="2633e-431">Отключите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="2633e-431">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="2633e-432">Запустите установщик.</span><span class="sxs-lookup"><span data-stu-id="2633e-432">Run the installer.</span></span>
1. <span data-ttu-id="2633e-433">Экспортируйте обновленный файл *applicationHost.config* в общую папку.</span><span class="sxs-lookup"><span data-stu-id="2633e-433">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="2633e-434">Повторно включите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="2633e-434">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="2633e-435">Версия модуля и журналы установщика хостинга Bundle</span><span class="sxs-lookup"><span data-stu-id="2633e-435">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="2633e-436">Чтобы определить версию установщика модуля ASP.NET Core, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="2633e-436">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="2633e-437">В системе размещения перейдите к папке *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="2633e-437">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="2633e-438">Найдите файл *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="2633e-438">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="2633e-439">Щелкните правой кнопкой мыши файл и выберите **Свойства** из контекстного меню.</span><span class="sxs-lookup"><span data-stu-id="2633e-439">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="2633e-440">Выберите вкладку **Сведения**. **Версия файла** и **Версия продукта** дают представление об установленной версии модуля.</span><span class="sxs-lookup"><span data-stu-id="2633e-440">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="2633e-441">Журналы установщика хостинга Bundle для модуля находятся в папке *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. Этот файл имеет имя *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="2633e-441">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="2633e-442">Модуль, схемы и расположение файлов конфигурации</span><span class="sxs-lookup"><span data-stu-id="2633e-442">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="2633e-443">Module</span><span class="sxs-lookup"><span data-stu-id="2633e-443">Module</span></span>

<span data-ttu-id="2633e-444">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="2633e-444">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="2633e-445">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="2633e-445">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="2633e-446">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="2633e-446">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="2633e-447">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="2633e-447">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="2633e-448">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="2633e-448">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="2633e-449">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="2633e-449">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="2633e-450">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="2633e-450">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="2633e-451">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="2633e-451">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="2633e-452">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="2633e-452">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="2633e-453">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="2633e-453">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="2633e-454">Схема</span><span class="sxs-lookup"><span data-stu-id="2633e-454">Schema</span></span>

<span data-ttu-id="2633e-455">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="2633e-455">**IIS**</span></span>

   * <span data-ttu-id="2633e-456">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="2633e-456">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="2633e-457">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="2633e-457">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="2633e-458">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="2633e-458">**IIS Express**</span></span>

   * <span data-ttu-id="2633e-459">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="2633e-459">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="2633e-460">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="2633e-460">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="2633e-461">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="2633e-461">Configuration</span></span>

<span data-ttu-id="2633e-462">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="2633e-462">**IIS**</span></span>

   * <span data-ttu-id="2633e-463">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="2633e-463">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="2633e-464">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="2633e-464">**IIS Express**</span></span>

   * <span data-ttu-id="2633e-465">Visual Studio: {КОРЕНЬ ПРИЛОЖЕНИЯ}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="2633e-465">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>
   
   * <span data-ttu-id="2633e-466">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="2633e-466">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="2633e-467">Файлы можно найти путем поиска *aspnetcore* в файле *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="2633e-467">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2633e-468">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="2633e-468">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="2633e-469">Репозиторий GitHub для модуля ASP.NET Core (справочные материалы)</span><span class="sxs-lookup"><span data-stu-id="2633e-469">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
