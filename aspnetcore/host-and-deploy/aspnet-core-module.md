---
title: Модуль ASP.NET Core
author: guardrex
description: Сведения о настройке модуля ASP.NET Core для размещения приложений ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 504e5d35f11531a5752b3c8e23d96db3cbe40d1a
ms.sourcegitcommit: b8ed594ab9f47fa32510574f3e1b210cff000967
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2019
ms.locfileid: "66251437"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="6dfcc-103">Модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6dfcc-103">ASP.NET Core Module</span></span>

<span data-ttu-id="6dfcc-104">Авторы: [Том Дайкстра (Tom Dykstra)](https://github.com/tdykstra), [Рик Штраль (Rick Strahl)](https://github.com/RickStrahl), [Крис Росс (Chris Ross)](https://github.com/Tratcher), [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT), [Сураб Ширхатти (Sourabh Shirhatti)](https://twitter.com/sshirhatti), [ Джастин Коталик (Justin Kotalik)](https://github.com/jkotalik) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6dfcc-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6dfcc-105">Модуль ASP.NET Core имеет собственный модуль IIS, который подключается к конвейеру IIS для выполнения следующих задач:</span><span class="sxs-lookup"><span data-stu-id="6dfcc-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="6dfcc-106">Размещение приложения ASP.NET Core внутри рабочего процесса IIS (`w3wp.exe`). Это так называемая [модель внутрипроцессного размещения](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="6dfcc-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="6dfcc-107">Переадресация веб-запросов к серверной части приложения ASP.NET Core на [сервере Kestrel](xref:fundamentals/servers/kestrel). Это [модель размещения вне процесса](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="6dfcc-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="6dfcc-108">Поддерживаемые версии Windows:</span><span class="sxs-lookup"><span data-stu-id="6dfcc-108">Supported Windows versions:</span></span>

* <span data-ttu-id="6dfcc-109">Windows 7 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="6dfcc-109">Windows 7 or later</span></span>
* <span data-ttu-id="6dfcc-110">Windows Server 2008 R2 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="6dfcc-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="6dfcc-111">При размещении в процессе модуль использует реализацию внутрипроцессного сервера для IIS — HTTP-сервер IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="6dfcc-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="6dfcc-112">При размещении вне процесса модуль работает только с Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="6dfcc-113">Модуль несовместим с [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="6dfcc-113">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="6dfcc-114">Модели размещения</span><span class="sxs-lookup"><span data-stu-id="6dfcc-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="6dfcc-115">Модель внутрипроцессного размещения</span><span class="sxs-lookup"><span data-stu-id="6dfcc-115">In-process hosting model</span></span>

<span data-ttu-id="6dfcc-116">Чтобы настроить приложение для внутрипроцессного размещения, добавьте свойство `<AspNetCoreHostingModel>` к файлу проекта приложения со значением `InProcess` (размещение вне процесса имеет значение `OutOfProcess`):</span><span class="sxs-lookup"><span data-stu-id="6dfcc-116">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="6dfcc-117">Модель внутрипроцессного размещения не поддерживается для приложений ASP.NET Core, предназначенных для .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-117">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="6dfcc-118">Если свойство `<AspNetCoreHostingModel>` отсутствует в файле, значение по умолчанию — `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-118">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="6dfcc-119">При внутрипроцессном размещении применимы следующие характеристики:</span><span class="sxs-lookup"><span data-stu-id="6dfcc-119">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="6dfcc-120">Вместо сервера [Kestrel](xref:fundamentals/servers/kestrel) используется HTTP-сервер IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="6dfcc-120">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="6dfcc-121">Для внутрипроцессной обработки [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) вызывает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="6dfcc-121">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="6dfcc-122">Регистрация `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-122">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="6dfcc-123">Настройка порта и базового пути, которые будет прослушивать сервер при выполнении за модулем ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-123">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="6dfcc-124">Настройка перехвата ошибок запуска на узле.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-124">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="6dfcc-125">[Атрибут requestTimeout](#attributes-of-the-aspnetcore-element) не применяется к внутрипроцессному размещению.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-125">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="6dfcc-126">Совместное использование пула приложений среди приложений не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-126">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="6dfcc-127">Используйте один пул приложений для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-127">Use one app pool per app.</span></span>

* <span data-ttu-id="6dfcc-128">При использовании [веб-развертывания](/iis/publish/using-web-deploy/introduction-to-web-deploy) или размещении [файла app_offline.htm в развертывании](xref:host-and-deploy/iis/index#locked-deployment-files) вручную приложение может не завершить работу сразу при наличии открытого соединения.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-128">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="6dfcc-129">Например, подключение websocket может задерживать завершение работы приложения.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-129">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="6dfcc-130">Архитектура (разрядность) приложения и установленная среда выполнения (x64 или x86) должны соответствовать архитектуре пула приложений.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-130">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="6dfcc-131">Если вы настраиваете узел приложения вручную с помощью `WebHostBuilder` (не используя [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) и приложение запускается напрямую на сервере Kestrel (с локальным размещением), вызывайте `UseKestrel` перед вызовом `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-131">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="6dfcc-132">Если изменить порядок, узел не запустится.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-132">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="6dfcc-133">Обнаружены отключения клиентов.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-133">Client disconnects are detected.</span></span> <span data-ttu-id="6dfcc-134">При отключении клиента происходит отмена токена отмены [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*).</span><span class="sxs-lookup"><span data-stu-id="6dfcc-134">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="6dfcc-135">В ASP.NET Core 2.2.1 и более ранних версий <xref:System.IO.Directory.GetCurrentDirectory*> возвращает рабочий каталог процесса, запущенного службами IIS, а не каталог приложения (например, *C:\Windows\System32\inetsrv* для *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="6dfcc-135">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="6dfcc-136">Пример кода, который задает текущий каталог приложения, см. в разделе [Класс CurrentDirectoryHelpers](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="6dfcc-136">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="6dfcc-137">Вызовите метод `SetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-137">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="6dfcc-138">Последующие вызовы <xref:System.IO.Directory.GetCurrentDirectory*> возвращают каталог приложения.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-138">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="6dfcc-139">При размещении в процессе <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> не вызывается внутри для инициализации пользователя.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-139">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="6dfcc-140">Таким образом, реализация <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>, используемая для преобразования утверждений после каждой проверки подлинности, не активируется по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-140">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="6dfcc-141">При преобразовании утверждений с реализацией <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> вызовите <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> для добавления служб проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="6dfcc-141">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

  ```csharp
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddTransient<IClaimsTransformation, ClaimsTransformer>();
      services.AddAuthentication(IISServerDefaults.AuthenticationScheme);
  }

  public void Configure(IApplicationBuilder app)
  {
      app.UseAuthentication();
  }
  ```

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="6dfcc-142">Модель размещения вне процесса</span><span class="sxs-lookup"><span data-stu-id="6dfcc-142">Out-of-process hosting model</span></span>

<span data-ttu-id="6dfcc-143">Чтобы настроить приложение для размещения вне процесса, используйте один из следующих подходов в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-143">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="6dfcc-144">Не указывайте свойство `<AspNetCoreHostingModel>`.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-144">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="6dfcc-145">Если свойство `<AspNetCoreHostingModel>` отсутствует в файле, значение по умолчанию — `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-145">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="6dfcc-146">Установите для свойства `<AspNetCoreHostingModel>` значение `OutOfProcess` (внутрипроцессное размещение имеет значение `InProcess`):</span><span class="sxs-lookup"><span data-stu-id="6dfcc-146">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="6dfcc-147">Сервер [Kestrel](xref:fundamentals/servers/kestrel) используется вместо HTTP-сервера IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="6dfcc-147">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="6dfcc-148">Для внепроцессной обработки [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) вызывает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="6dfcc-148">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="6dfcc-149">Настройка порта и базового пути, которые будет прослушивать сервер при выполнении за модулем ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-149">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="6dfcc-150">Настройка перехвата ошибок запуска на узле.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-150">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="6dfcc-151">Изменения модели размещения</span><span class="sxs-lookup"><span data-stu-id="6dfcc-151">Hosting model changes</span></span>

<span data-ttu-id="6dfcc-152">Если параметр `hostingModel` изменяется в файле *web.config* (как описано в разделе [Конфигурация с помощью web.config](#configuration-with-webconfig)), модуль перезапускает рабочий процесс для служб IIS.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-152">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="6dfcc-153">Для IIS Express модуль не перезапускает рабочий процесс, а запускает нормальное завершение работы текущего процесса IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-153">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="6dfcc-154">Следующий запрос для приложения порождает новый процесс IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-154">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="6dfcc-155">Имя процесса</span><span class="sxs-lookup"><span data-stu-id="6dfcc-155">Process name</span></span>

<span data-ttu-id="6dfcc-156">`Process.GetCurrentProcess().ProcessName` сообщает `w3wp`/`iisexpress` (внутри процесса) или `dotnet` (вне процесса).</span><span class="sxs-lookup"><span data-stu-id="6dfcc-156">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="6dfcc-157">Модуль ASP.NET Core имеет собственный модуль IIS, который подключается к конвейеру IIS для переадресации веб-запросов в серверные приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-157">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="6dfcc-158">Поддерживаемые версии Windows:</span><span class="sxs-lookup"><span data-stu-id="6dfcc-158">Supported Windows versions:</span></span>

* <span data-ttu-id="6dfcc-159">Windows 7 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="6dfcc-159">Windows 7 or later</span></span>
* <span data-ttu-id="6dfcc-160">Windows Server 2008 R2 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="6dfcc-160">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="6dfcc-161">Модуль работает только с Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-161">The module only works with Kestrel.</span></span> <span data-ttu-id="6dfcc-162">Модуль несовместим с [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="6dfcc-162">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="6dfcc-163">Так как приложения ASP.NET Core выполняются в процессе, отделенном от рабочего процесса IIS, этот модуль также обрабатывает управление процессами.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-163">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="6dfcc-164">Модуль запускает процесс для приложения ASP.NET Core при поступлении первого запроса и перезапускает приложение при сбое.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-164">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="6dfcc-165">Это, по сути, совпадает с поведением приложений ASP.NET 4.x, выполняемых внутрипроцессно в IIS и управляемых [службой активации процессов Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="6dfcc-165">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="6dfcc-166">На следующей схеме показана связь между IIS, модулем ASP.NET Core и приложением:</span><span class="sxs-lookup"><span data-stu-id="6dfcc-166">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Модуль ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="6dfcc-168">Запросы поступают из Интернета в драйвер HTTP.sys в режиме ядра.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-168">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="6dfcc-169">Драйвер направляет запросы к службам IIS на настроенный порт веб-сайта — обычно 80 (HTTP) или 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="6dfcc-169">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="6dfcc-170">Модуль перенаправляет запросы Kestrel на случайный порт для приложения, отличающийся от порта 80 или 443.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-170">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="6dfcc-171">Модуль задает порт с помощью переменной среды во время запуска, а [ПО промежуточного слоя для интеграции IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) настраивает сервер для прослушивания `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-171">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="6dfcc-172">Выполняются дополнительные проверки, и запросы не из модуля отклоняются.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-172">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="6dfcc-173">Модуль не поддерживает переадресацию по HTTPS, поэтому запросы переадресовываются по протоколу HTTP, даже если были получены IIS по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-173">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="6dfcc-174">После того как Kestrel забирает запрос из модуля, запрос передается в конвейер ПО промежуточного слоя ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-174">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="6dfcc-175">Конвейер ПО промежуточного слоя обрабатывает запрос и передает его в качестве экземпляра `HttpContext` в логику приложения.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-175">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="6dfcc-176">ПО промежуточного слоя, добавленное интеграцией IIS, обновляет схему, удаленный IP-адрес и базовый путь для переадресации запроса в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-176">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="6dfcc-177">Отклик приложения передается обратно в службу IIS, которая отправляет его обратно в HTTP-клиент, инициировавший запрос.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-177">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="6dfcc-178">Многие собственные модули, такие как проверка подлинности Windows, остаются активными.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-178">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="6dfcc-179">Дополнительные сведения о модулях IIS, активных с модулем ASP.NET Core, см. в разделе <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-179">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="6dfcc-180">Дополнительные возможности модуля ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="6dfcc-180">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="6dfcc-181">Задание переменных среды для рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-181">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="6dfcc-182">Внесение в журнал выходных данных stdout для хранилища файлов с целью устранения неполадок при запуске.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-182">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="6dfcc-183">Переадресация токенов проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-183">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="6dfcc-184">Как установить и использовать модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6dfcc-184">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="6dfcc-185">Инструкции о том, как установить и использовать модуль ASP.NET Core, см. в разделе <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-185">For instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="6dfcc-186">Конфигурация с помощью файла web.config</span><span class="sxs-lookup"><span data-stu-id="6dfcc-186">Configuration with web.config</span></span>

<span data-ttu-id="6dfcc-187">Модуль ASP.NET Core настроен с помощью раздела `aspNetCore` узла `system.webServer` файла *web.config* на веб-сайте.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-187">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="6dfcc-188">Следующий файл *web.config* публикуется для [зависимого от платформы развертывания](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) и настраивает модуль ASP.NET Core для обработки запросов к веб-сайту.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-188">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="6dfcc-189">Следующий файл *web.config* опубликован для [автономного развертывания](/dotnet/articles/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="6dfcc-189">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="6dfcc-190">Значение `false` свойства <xref:System.Configuration.SectionInformation.InheritInChildApplications*> указывает, что параметры, заданные в элементе [\<расположение>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location), не наследуются приложениями, которые находятся во вложенном каталоге приложения.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-190">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

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

<span data-ttu-id="6dfcc-191">Когда приложение развернуто в [службе приложений Azure](https://azure.microsoft.com/services/app-service/), путь `stdoutLogFile` задан как `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-191">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="6dfcc-192">Путь сохраняет журналы stdout в папке *LogFiles*, расположение которой автоматически создается службой.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-192">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="6dfcc-193">Сведения о конфигурации дочерних приложений IIS см. здесь: <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-193">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="6dfcc-194">Атрибуты элемента aspNetCore</span><span class="sxs-lookup"><span data-stu-id="6dfcc-194">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="6dfcc-195">Атрибут</span><span class="sxs-lookup"><span data-stu-id="6dfcc-195">Attribute</span></span> | <span data-ttu-id="6dfcc-196">Описание</span><span class="sxs-lookup"><span data-stu-id="6dfcc-196">Description</span></span> | <span data-ttu-id="6dfcc-197">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="6dfcc-197">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="6dfcc-198">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-198">Optional string attribute.</span></span></p><p><span data-ttu-id="6dfcc-199">Аргументы для исполняемого файла, указанного в атрибуте **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-199">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="6dfcc-200">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-200">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6dfcc-201">Если значение равно true, страница **502.5 — ошибка процесса** подавляется и страница в файле *web.config* с кодом состояния 502 имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-201">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="6dfcc-202">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-202">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6dfcc-203">Если значение равно true, маркер безопасности отправляется дочернему процессу, прослушивающему порт %ASPNETCORE_PORT% как заголовок "MS-ASPNETCORE-WINAUTHTOKEN" каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-203">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="6dfcc-204">Этот процесс вызывает CloseHandle по этому маркеру безопасности каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-204">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="6dfcc-205">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-205">Optional string attribute.</span></span></p><p><span data-ttu-id="6dfcc-206">Указывает модель размещения — внутри процесса (`InProcess`) или вне процесса (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="6dfcc-206">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="6dfcc-207">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-207">Optional integer attribute.</span></span></p><p><span data-ttu-id="6dfcc-208">Указывает число экземпляров процесса, заданное в параметре **processPath**, которое может появиться для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-208">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="6dfcc-209">&dagger;Для внутрипроцессного размещения существует ограничение: `1`.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-209">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="6dfcc-210">Параметр `processesPerApplication` не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-210">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="6dfcc-211">Этот атрибут будет удален в будущем выпуске.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-211">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="6dfcc-212">По умолчанию: `1`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-212">Default: `1`</span></span><br><span data-ttu-id="6dfcc-213">Минимум: `1`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-213">Min: `1`</span></span><br><span data-ttu-id="6dfcc-214">Максимум: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="6dfcc-214">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="6dfcc-215">Обязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-215">Required string attribute.</span></span></p><p><span data-ttu-id="6dfcc-216">Путь к исполняемому файлу, который запускает процесс прослушивания HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-216">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="6dfcc-217">Поддерживаются относительные пути.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-217">Relative paths are supported.</span></span> <span data-ttu-id="6dfcc-218">Если путь начинается с `.`, то начало пути считается относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-218">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="6dfcc-219">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-219">Optional integer attribute.</span></span></p><p><span data-ttu-id="6dfcc-220">Указывает количество сбоев за минуту, которыми может завершиться процесс, указанный в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-220">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="6dfcc-221">Если этот предел превышен, модуль останавливает запуск процесса на оставшуюся часть минуты.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-221">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="6dfcc-222">Не поддерживается для внутрипроцессного размещения.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-222">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="6dfcc-223">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-223">Default: `10`</span></span><br><span data-ttu-id="6dfcc-224">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-224">Min: `0`</span></span><br><span data-ttu-id="6dfcc-225">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-225">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="6dfcc-226">Необязательный атрибут timespan.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-226">Optional timespan attribute.</span></span></p><p><span data-ttu-id="6dfcc-227">Указывает продолжительность, на протяжении которой модуль ASP.NET Core ожидает ответа от процесса, прослушивающего порт %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-227">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="6dfcc-228">В версиях модуля ASP.NET Core, поставляемых с выпуском ASP.NET Core 2.1 или новее, атрибут `requestTimeout` указывается в часах, минутах и секундах.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-228">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="6dfcc-229">Не применяется к внутрипроцессному размещению.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-229">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="6dfcc-230">Для внутрипроцессного размещения модуль ожидает, пока приложение не обработает запрос.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-230">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="6dfcc-231">Допустимые значения для сегментов минут и секунд в строках находятся в диапазоне 0–59.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-231">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="6dfcc-232">Значение **60** для минут и секунд приведет к ошибке *500 — внутренняя ошибка сервера*.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-232">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="6dfcc-233">По умолчанию: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-233">Default: `00:02:00`</span></span><br><span data-ttu-id="6dfcc-234">Минимум: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-234">Min: `00:00:00`</span></span><br><span data-ttu-id="6dfcc-235">Максимум: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-235">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="6dfcc-236">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-236">Optional integer attribute.</span></span></p><p><span data-ttu-id="6dfcc-237">Длительность ожидания модуля в секундах, пока произойдет правильное выключение исполняемого файла при обнаружении файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-237">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="6dfcc-238">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-238">Default: `10`</span></span><br><span data-ttu-id="6dfcc-239">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-239">Min: `0`</span></span><br><span data-ttu-id="6dfcc-240">Максимум: `600`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-240">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="6dfcc-241">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-241">Optional integer attribute.</span></span></p><p><span data-ttu-id="6dfcc-242">Время в секундах, которое модуль ожидает, пока запустится процесс прослушивания порта исполняемого файла.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-242">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="6dfcc-243">Если этот предел превышен, модуль завершает процесс.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-243">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="6dfcc-244">Модуль пытается перезапустить процесс при получении нового запроса и будет продолжать пытаться перезапустить процесс для последующих входящих запросов, если не удается запустить приложение определенное в атрибуте **rapidFailsPerMinute** количество раз за последнюю минуту.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-244">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="6dfcc-245">Значение 0 (ноль) **не** считается бесконечным временем ожидания.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-245">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="6dfcc-246">По умолчанию: `120`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-246">Default: `120`</span></span><br><span data-ttu-id="6dfcc-247">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-247">Min: `0`</span></span><br><span data-ttu-id="6dfcc-248">Максимум: `3600`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-248">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="6dfcc-249">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-249">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6dfcc-250">Если значение равно true, **stdout** и **stderr** для процесса, указанного в атрибуте **processPath**, перенаправляются к файлу, заданному в атрибуте **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-250">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="6dfcc-251">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-251">Optional string attribute.</span></span></p><p><span data-ttu-id="6dfcc-252">Указывает относительный или абсолютный путь к файлу, для которого регистрируются **stdout** и **stderr** из процесса, указанного в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-252">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="6dfcc-253">Относительные пути задаются относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-253">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="6dfcc-254">Любой путь, начинающийся с `.`, относится к корневому каталогу веб-сайта, а все остальные пути рассматриваются как абсолютные пути.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-254">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="6dfcc-255">Все папки, указанные в пути, создаются модулем при создании файла журнала.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-255">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="6dfcc-256">С помощью разделителей подчеркивания метка времени, идентификатор процесса и расширение файла ( *.log*) добавляются к последнему сегменту пути журнала **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-256">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="6dfcc-257">Если в качестве значения задано значение `.\logs\stdout`, например, журнал stdout сохраняется как *stdout_20180205194132_1934.log* в папке *журналов* с датой 5 февраля 2018 г. в 19:41:32 с идентификатором процесса 1934.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-257">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="6dfcc-258">Атрибут</span><span class="sxs-lookup"><span data-stu-id="6dfcc-258">Attribute</span></span> | <span data-ttu-id="6dfcc-259">Описание</span><span class="sxs-lookup"><span data-stu-id="6dfcc-259">Description</span></span> | <span data-ttu-id="6dfcc-260">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="6dfcc-260">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="6dfcc-261">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-261">Optional string attribute.</span></span></p><p><span data-ttu-id="6dfcc-262">Аргументы для исполняемого файла, указанного в атрибуте **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-262">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="6dfcc-263">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-263">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6dfcc-264">Если значение равно true, страница **502.5 — ошибка процесса** подавляется и страница в файле *web.config* с кодом состояния 502 имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-264">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="6dfcc-265">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-265">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6dfcc-266">Если значение равно true, маркер безопасности отправляется дочернему процессу, прослушивающему порт %ASPNETCORE_PORT% как заголовок "MS-ASPNETCORE-WINAUTHTOKEN" каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-266">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="6dfcc-267">Этот процесс вызывает CloseHandle по этому маркеру безопасности каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-267">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="6dfcc-268">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-268">Optional integer attribute.</span></span></p><p><span data-ttu-id="6dfcc-269">Указывает число экземпляров процесса, заданное в параметре **processPath**, которое может появиться для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-269">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="6dfcc-270">Параметр `processesPerApplication` не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-270">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="6dfcc-271">Этот атрибут будет удален в будущем выпуске.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-271">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="6dfcc-272">По умолчанию: `1`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-272">Default: `1`</span></span><br><span data-ttu-id="6dfcc-273">Минимум: `1`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-273">Min: `1`</span></span><br><span data-ttu-id="6dfcc-274">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-274">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="6dfcc-275">Обязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-275">Required string attribute.</span></span></p><p><span data-ttu-id="6dfcc-276">Путь к исполняемому файлу, который запускает процесс прослушивания HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-276">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="6dfcc-277">Поддерживаются относительные пути.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-277">Relative paths are supported.</span></span> <span data-ttu-id="6dfcc-278">Если путь начинается с `.`, то начало пути считается относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-278">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="6dfcc-279">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-279">Optional integer attribute.</span></span></p><p><span data-ttu-id="6dfcc-280">Указывает количество сбоев за минуту, которыми может завершиться процесс, указанный в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-280">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="6dfcc-281">Если этот предел превышен, модуль останавливает запуск процесса на оставшуюся часть минуты.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-281">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="6dfcc-282">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-282">Default: `10`</span></span><br><span data-ttu-id="6dfcc-283">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-283">Min: `0`</span></span><br><span data-ttu-id="6dfcc-284">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-284">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="6dfcc-285">Необязательный атрибут timespan.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-285">Optional timespan attribute.</span></span></p><p><span data-ttu-id="6dfcc-286">Указывает продолжительность, на протяжении которой модуль ASP.NET Core ожидает ответа от процесса, прослушивающего порт %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-286">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="6dfcc-287">В версиях модуля ASP.NET Core, поставляемых с выпуском ASP.NET Core 2.1 или новее, атрибут `requestTimeout` указывается в часах, минутах и секундах.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-287">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="6dfcc-288">По умолчанию: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-288">Default: `00:02:00`</span></span><br><span data-ttu-id="6dfcc-289">Минимум: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-289">Min: `00:00:00`</span></span><br><span data-ttu-id="6dfcc-290">Максимум: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-290">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="6dfcc-291">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-291">Optional integer attribute.</span></span></p><p><span data-ttu-id="6dfcc-292">Длительность ожидания модуля в секундах, пока произойдет правильное выключение исполняемого файла при обнаружении файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-292">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="6dfcc-293">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-293">Default: `10`</span></span><br><span data-ttu-id="6dfcc-294">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-294">Min: `0`</span></span><br><span data-ttu-id="6dfcc-295">Максимум: `600`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-295">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="6dfcc-296">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-296">Optional integer attribute.</span></span></p><p><span data-ttu-id="6dfcc-297">Время в секундах, которое модуль ожидает, пока запустится процесс прослушивания порта исполняемого файла.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-297">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="6dfcc-298">Если этот предел превышен, модуль завершает процесс.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-298">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="6dfcc-299">Модуль пытается перезапустить процесс при получении нового запроса и будет продолжать пытаться перезапустить процесс для последующих входящих запросов, если не удается запустить приложение определенное в атрибуте **rapidFailsPerMinute** количество раз за последнюю минуту.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-299">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="6dfcc-300">Значение 0 (ноль) **не** считается бесконечным временем ожидания.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-300">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="6dfcc-301">По умолчанию: `120`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-301">Default: `120`</span></span><br><span data-ttu-id="6dfcc-302">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-302">Min: `0`</span></span><br><span data-ttu-id="6dfcc-303">Максимум: `3600`</span><span class="sxs-lookup"><span data-stu-id="6dfcc-303">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="6dfcc-304">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-304">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6dfcc-305">Если значение равно true, **stdout** и **stderr** для процесса, указанного в атрибуте **processPath**, перенаправляются к файлу, заданному в атрибуте **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-305">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="6dfcc-306">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-306">Optional string attribute.</span></span></p><p><span data-ttu-id="6dfcc-307">Указывает относительный или абсолютный путь к файлу, для которого регистрируются **stdout** и **stderr** из процесса, указанного в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-307">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="6dfcc-308">Относительные пути задаются относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-308">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="6dfcc-309">Любой путь, начинающийся с `.`, относится к корневому каталогу веб-сайта, а все остальные пути рассматриваются как абсолютные пути.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-309">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="6dfcc-310">Все папки, указанные в пути, должны существовать, чтобы модуль мог создать файл журнала.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-310">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="6dfcc-311">С помощью разделителей подчеркивания метка времени, идентификатор процесса и расширение файла ( *.log*) добавляются к последнему сегменту пути журнала **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-311">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="6dfcc-312">Если в качестве значения задано значение `.\logs\stdout`, например, журнал stdout сохраняется как *stdout_20180205194132_1934.log* в папке *журналов* с датой 5 февраля 2018 г. в 19:41:32 с идентификатором процесса 1934.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-312">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="6dfcc-313">Настройка переменных среды</span><span class="sxs-lookup"><span data-stu-id="6dfcc-313">Setting environment variables</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6dfcc-314">Переменные среды для процесса можно указать в атрибуте `processPath`.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-314">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="6dfcc-315">Укажите переменную среды с дочерним элементом `<environmentVariable>` элемента коллекции `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-315">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="6dfcc-316">Переменные среды, установленные в этом разделе, имеют приоритет над переменными системной среды.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-316">Environment variables set in this section take precedence over system environment variables.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="6dfcc-317">Переменные среды для процесса можно указать в атрибуте `processPath`.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-317">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="6dfcc-318">Укажите переменную среды с дочерним элементом `<environmentVariable>` элемента коллекции `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-318">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="6dfcc-319">Переменные среды, заданные в этом разделе, конфликтуют с переменными системной среды с такими же именами.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-319">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="6dfcc-320">Если переменная среды задана и в файле *web.config*, и на уровне системы в Windows, значение из файла *web.config* добавляется к значению переменной системной среды (например, `ASPNETCORE_ENVIRONMENT: Development;Development`), которая препятствует запуску приложения.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-320">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

::: moniker-end

<span data-ttu-id="6dfcc-321">В следующем примере устанавливаются две переменные среды.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-321">The following example sets two environment variables.</span></span> <span data-ttu-id="6dfcc-322">`ASPNETCORE_ENVIRONMENT` настраивает среду приложения для `Development`.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-322">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="6dfcc-323">Разработчик может временно задать это значение в файле *web.config*, чтобы принудительно загрузить [Страницу исключений для разработчиков](xref:fundamentals/error-handling) при отладке исключения приложения.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-323">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="6dfcc-324">`CONFIG_DIR` — пример пользовательской переменной среды, где разработчик написал код, который считывает значение при запуске, чтобы сформировать путь для загрузки файла конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-324">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="6dfcc-325">Вместо задания среды напрямую в *web.config* включите свойство `<EnvironmentName>` в профиль публикации ( *.pubxml*) или файл проекта.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-325">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="6dfcc-326">При этом подходе во время публикации проекта среда задается в файле *web.config*:</span><span class="sxs-lookup"><span data-stu-id="6dfcc-326">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="6dfcc-327">Установите только переменную среды `ASPNETCORE_ENVIRONMENT` для `Development` на серверах промежуточных процессов и тестирования, которые недоступны для ненадежных сетей, таких как Интернет.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-327">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="6dfcc-328">App_offline.htm</span><span class="sxs-lookup"><span data-stu-id="6dfcc-328">app_offline.htm</span></span>

<span data-ttu-id="6dfcc-329">Если в корневом каталоге приложения обнаружен файл с именем *app_offline.htm*, модуль ASP.NET Core пытается корректно закрыть приложение и прекратить обработку входящих запросов.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-329">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="6dfcc-330">Если приложение по-прежнему выполняется через количество секунд, определенное атрибутом `shutdownTimeLimit`, модуль ASP.NET Core завершает выполняющийся процесс.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-330">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="6dfcc-331">Хотя файл *app_offline.htm* присутствует, модуль ASP.NET Core отвечает на запросы, отправляя назад содержимое файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-331">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="6dfcc-332">Когда *app_offline.htm* файл удаляется, следующий запрос запускает приложение.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-332">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6dfcc-333">При использовании модели размещения вне процесса приложение может не завершать работу немедленно при наличии открытого соединения.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-333">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="6dfcc-334">Например, подключение websocket может задерживать завершение работы приложения.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-334">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="6dfcc-335">Страница ошибок запуска</span><span class="sxs-lookup"><span data-stu-id="6dfcc-335">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6dfcc-336">Если при внутри- или внепроцессном размещении происходит сбой запуска приложения, открываются страницы пользовательских сообщений об ошибках.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-336">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="6dfcc-337">Если модулю ASP.NET Core не удается найти внутри- или внепроцессный обработчик запросов, откроется страница кода состояния *500.0 — ошибка загрузки внутри- или внепроцессного обработчика запросов*.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-337">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="6dfcc-338">Если в модели размещения внутри процесса модулю ASP.NET Core не удается запустить приложение, откроется страница кода состояния *500.30 — ошибка запуска*.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-338">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="6dfcc-339">Если в модели размещения вне процесса модулю ASP.NET Core не удается запустить серверный процесс или начинается серверный процесс, но ему не удается прослушать настроенный порт, появится страница кода состояния *502.5 — ошибка процесса*.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-339">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="6dfcc-340">Чтобы подавить отображение этой странице и вернуться к странице IIS кода состояния 5xx по умолчанию, используйте атрибут `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-340">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="6dfcc-341">Дополнительные сведения о настройке пользовательских сообщений об ошибках см. в разделе [Ошибки HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="6dfcc-341">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="6dfcc-342">Если модулю ASP.NET Core не удается запустить серверный процесс или начинается серверный процесс, но ему не удается прослушать настроенный порт, появится страница кода состояния *502.5 — ошибка процесса*.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-342">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="6dfcc-343">Чтобы подавить эту страницу и вернуться к странице IIS кода состояния 502 по умолчанию, используйте атрибут `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-343">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="6dfcc-344">Дополнительные сведения о настройке пользовательских сообщений об ошибках см. в разделе [Ошибки HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="6dfcc-344">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Страница кода состояния "502.5 — ошибка процесса"](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="6dfcc-346">Создание и перенаправление журнала</span><span class="sxs-lookup"><span data-stu-id="6dfcc-346">Log creation and redirection</span></span>

<span data-ttu-id="6dfcc-347">Модуль ASP.NET Core перенаправляет выходные потоки консоли stdout и stderr на диск, если заданы атрибуты `stdoutLogEnabled` и `stdoutLogFile` элемента `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-347">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="6dfcc-348">Все папки, указанные в пути `stdoutLogFile`, создаются модулем при создании файла журнала.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-348">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="6dfcc-349">Пул приложений должен иметь доступ на запись в папку, где записываются журналы (используйте атрибут `IIS AppPool\<app_pool_name>` для предоставления разрешения на запись).</span><span class="sxs-lookup"><span data-stu-id="6dfcc-349">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="6dfcc-350">Журналы не выполняют циклический сдвиг, пока не произойдет процесс перезапуска или перезагрузки.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-350">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="6dfcc-351">Администратор несет ответственность за ограничение дискового пространства, которое потребляют журналы.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-351">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="6dfcc-352">Использование журнала stdout рекомендуется только для устранения неполадок при запуске приложений.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-352">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="6dfcc-353">Не используйте журнал stdout для ведения общего журнала приложений.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-353">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="6dfcc-354">Для обычного входа в приложение ASP.NET Core используйте библиотеку ведения журналов, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-354">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="6dfcc-355">Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="6dfcc-355">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="6dfcc-356">При создании файла журнала автоматически добавляются отметка времени и расширение файла.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-356">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="6dfcc-357">Имя файла журнала составляется путем добавления метки времени, идентификатора процесса и расширения файла ( *.log*) к последнему сегменту атрибута пути `stdoutLogFile` (обычно *stdout*) с символами подчеркивания в качестве разделителей.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-357">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="6dfcc-358">Если атрибут пути `stdoutLogFile` заканчивается элементом *stdout*, журнал приложения с идентификатором 1934, созданный 5 февраля 2018 г. в 19:42:32, будет иметь имя *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-358">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6dfcc-359">Если `stdoutLogEnabled` имеет значение false, возникающие при запуске приложения ошибки записываются и передаются в журнал событий (макс. 30 КБ).</span><span class="sxs-lookup"><span data-stu-id="6dfcc-359">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="6dfcc-360">После запуска все дополнительные журналы удаляются.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-360">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="6dfcc-361">В следующем примере элемент `aspNetCore` настраивает ведение журнала stdout для приложения, размещенного в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-361">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="6dfcc-362">Для локального ведения журнала допустим локальный или общий сетевой путь.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-362">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="6dfcc-363">Убедитесь, что идентификатор пользователя AppPool имеет разрешение на запись по указанному пути.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-363">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="6dfcc-364">Расширенные журналы диагностики</span><span class="sxs-lookup"><span data-stu-id="6dfcc-364">Enhanced diagnostic logs</span></span>

<span data-ttu-id="6dfcc-365">Модуль ASP.NET Core можно настроить. Он позволяет работать с расширенными журналами диагностики.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-365">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="6dfcc-366">Добавьте элемент `<handlerSettings>` в элемент `<aspNetCore>` в файле *web.config*. Задайте параметру `debugLevel` значение `TRACE`, чтобы обеспечить высокую точность диагностических сведений:</span><span class="sxs-lookup"><span data-stu-id="6dfcc-366">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="6dfcc-367">Значения уровня отладки (`debugLevel`) могут включать уровень и расположение.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-367">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="6dfcc-368">Уровни (в порядке возрастания степени детализации):</span><span class="sxs-lookup"><span data-stu-id="6dfcc-368">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="6dfcc-369">ОШИБКА</span><span class="sxs-lookup"><span data-stu-id="6dfcc-369">ERROR</span></span>
* <span data-ttu-id="6dfcc-370">ПРЕДУПРЕЖДЕНИЕ</span><span class="sxs-lookup"><span data-stu-id="6dfcc-370">WARNING</span></span>
* <span data-ttu-id="6dfcc-371">ИНФОРМАЦИЯ</span><span class="sxs-lookup"><span data-stu-id="6dfcc-371">INFO</span></span>
* <span data-ttu-id="6dfcc-372">TRACE</span><span class="sxs-lookup"><span data-stu-id="6dfcc-372">TRACE</span></span>

<span data-ttu-id="6dfcc-373">Расположения (допускаются несколько расположений):</span><span class="sxs-lookup"><span data-stu-id="6dfcc-373">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="6dfcc-374">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="6dfcc-374">CONSOLE</span></span>
* <span data-ttu-id="6dfcc-375">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="6dfcc-375">EVENTLOG</span></span>
* <span data-ttu-id="6dfcc-376">FILE</span><span class="sxs-lookup"><span data-stu-id="6dfcc-376">FILE</span></span>

<span data-ttu-id="6dfcc-377">Параметры обработчика могут быть указаны с помощью переменных среды:</span><span class="sxs-lookup"><span data-stu-id="6dfcc-377">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="6dfcc-378">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Путь к файлу журнала отладки.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-378">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="6dfcc-379">(По умолчанию: *aspnetcore debug.log*)</span><span class="sxs-lookup"><span data-stu-id="6dfcc-379">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="6dfcc-380">`ASPNETCORE_MODULE_DEBUG` &ndash; Параметр уровня отладки.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-380">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="6dfcc-381">**Не** оставляйте ведение журнала отладки включенным в развертывании дольше того времени, которое требуется для устранения проблемы.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-381">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="6dfcc-382">Размер журнала не ограничен.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-382">The size of the log isn't limited.</span></span> <span data-ttu-id="6dfcc-383">Если оставить журнал отладки включенным, он может исчерпать все доступное место на диске и привести к сбою сервера или службы приложений.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-383">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="6dfcc-384">См. пример элемента `aspNetCore` в файле *web.config* в разделе [Конфигурация с помощью файла web.config](#configuration-with-webconfig).</span><span class="sxs-lookup"><span data-stu-id="6dfcc-384">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="6dfcc-385">Конфигурация прокси-сервера использует протокол HTTP и токен связывания</span><span class="sxs-lookup"><span data-stu-id="6dfcc-385">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6dfcc-386">*Применяется только к размещению вне процесса.*</span><span class="sxs-lookup"><span data-stu-id="6dfcc-386">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="6dfcc-387">Прокси-сервер, созданный между модулем ASP.NET Core и Kestrel, использует протокол HTTP.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-387">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="6dfcc-388">Использование HTTP позволяет оптимизировать производительность, так как трафик между модулем и Kestrel передается на петлевой адрес в сетевом интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-388">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="6dfcc-389">Отсутствует риск перехвата трафика между модулем и Kestrel из расположения на сервере.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-389">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="6dfcc-390">Токен связывания гарантирует, что полученные Kestrel запросы были переданы службами IIS, а не из какого-либо другого источника.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-390">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="6dfcc-391">Этот токен создается и задается модулем в переменную среды (`ASPNETCORE_TOKEN`).</span><span class="sxs-lookup"><span data-stu-id="6dfcc-391">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="6dfcc-392">Он также задается в заголовке (`MS-ASPNETCORE-TOKEN`) каждого запроса, переданного через прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-392">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="6dfcc-393">ПО промежуточного слоя IIS проверяет каждый получаемый запрос, чтобы убедиться, что заголовок с токеном связывания соответствует значению переменной среды.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-393">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="6dfcc-394">Если значения токена не совпадают, запрос заносится в журнал и отклоняется.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-394">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="6dfcc-395">Переменная среды с токеном связывания и трафик между модулем и Kestrel недоступны из расположения на сервере.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-395">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="6dfcc-396">Не зная значение токена связывания, злоумышленник не может отправлять запросы, обходящие проверку в ПО промежуточного слоя IIS.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-396">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="6dfcc-397">Модуль ASP.NET Core с общей конфигурацией IIS</span><span class="sxs-lookup"><span data-stu-id="6dfcc-397">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="6dfcc-398">Программа установки модуля ASP.NET Core запускается с правами учетной записи **TrustedInstaller**.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-398">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="6dfcc-399">Поскольку учетная запись локальной системы не имеет разрешения на изменение пути к общей папке, используемого общей конфигурацией IIS, установщик получает ошибку отказа в доступе при попытке настроить параметры модуля в файле *applicationHost.config* общей папки.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-399">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6dfcc-400">При использовании общей конфигурации IIS на том же компьютере, где установлены службы IIS, запустите установщик пакета размещения ASP.NET Core с параметром `OPT_NO_SHARED_CONFIG_CHECK` со значением `1`:</span><span class="sxs-lookup"><span data-stu-id="6dfcc-400">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="6dfcc-401">Если путь к общей конфигурации не на том же компьютере, где установлены службы IIS, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-401">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="6dfcc-402">Отключите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-402">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="6dfcc-403">Запустите установщик.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-403">Run the installer.</span></span>
1. <span data-ttu-id="6dfcc-404">Экспортируйте обновленный файл *applicationHost.config* в общую папку.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-404">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="6dfcc-405">Повторно включите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-405">Re-enable the IIS Shared Configuration.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="6dfcc-406">При использовании общей конфигурации IIS выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-406">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="6dfcc-407">Отключите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-407">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="6dfcc-408">Запустите установщик.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-408">Run the installer.</span></span>
1. <span data-ttu-id="6dfcc-409">Экспортируйте обновленный файл *applicationHost.config* в общую папку.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-409">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="6dfcc-410">Повторно включите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-410">Re-enable the IIS Shared Configuration.</span></span>

::: moniker-end

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="6dfcc-411">Версия модуля и журналы установщика хостинга Bundle</span><span class="sxs-lookup"><span data-stu-id="6dfcc-411">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="6dfcc-412">Чтобы определить версию установщика модуля ASP.NET Core, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-412">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="6dfcc-413">В системе размещения перейдите к папке *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-413">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="6dfcc-414">Найдите файл *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-414">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="6dfcc-415">Щелкните правой кнопкой мыши файл и выберите **Свойства** из контекстного меню.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-415">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="6dfcc-416">Выберите вкладку **Сведения**. **Версия файла** и **Версия продукта** дают представление об установленной версии модуля.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-416">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="6dfcc-417">Журналы установщика хостинга Bundle для модуля находятся в папке *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. Этот файл имеет имя *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-417">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="6dfcc-418">Модуль, схемы и расположение файлов конфигурации</span><span class="sxs-lookup"><span data-stu-id="6dfcc-418">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="6dfcc-419">Module</span><span class="sxs-lookup"><span data-stu-id="6dfcc-419">Module</span></span>

<span data-ttu-id="6dfcc-420">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="6dfcc-420">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="6dfcc-421">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6dfcc-421">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="6dfcc-422">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6dfcc-422">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="6dfcc-423">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6dfcc-423">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="6dfcc-424">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6dfcc-424">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="6dfcc-425">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="6dfcc-425">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="6dfcc-426">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6dfcc-426">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="6dfcc-427">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6dfcc-427">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="6dfcc-428">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6dfcc-428">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="6dfcc-429">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6dfcc-429">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="6dfcc-430">Схема</span><span class="sxs-lookup"><span data-stu-id="6dfcc-430">Schema</span></span>

<span data-ttu-id="6dfcc-431">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="6dfcc-431">**IIS**</span></span>

* <span data-ttu-id="6dfcc-432">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="6dfcc-432">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="6dfcc-433">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="6dfcc-433">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

<span data-ttu-id="6dfcc-434">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="6dfcc-434">**IIS Express**</span></span>

* <span data-ttu-id="6dfcc-435">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="6dfcc-435">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="6dfcc-436">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="6dfcc-436">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="6dfcc-437">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="6dfcc-437">Configuration</span></span>

<span data-ttu-id="6dfcc-438">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="6dfcc-438">**IIS**</span></span>

* <span data-ttu-id="6dfcc-439">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="6dfcc-439">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="6dfcc-440">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="6dfcc-440">**IIS Express**</span></span>

* <span data-ttu-id="6dfcc-441">Visual Studio: {КОРЕНЬ ПРИЛОЖЕНИЯ}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="6dfcc-441">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="6dfcc-442">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="6dfcc-442">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="6dfcc-443">Файлы можно найти путем поиска *aspnetcore* в файле *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="6dfcc-443">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6dfcc-444">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6dfcc-444">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="6dfcc-445">Репозиторий GitHub для модуля ASP.NET Core (справочные материалы)</span><span class="sxs-lookup"><span data-stu-id="6dfcc-445">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
