---
title: Модуль ASP.NET Core
author: guardrex
description: Сведения о настройке модуля ASP.NET Core для размещения приложений ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: a33606bce6c78a19e3d380f7440e5892778806c3
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64889399"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="c1650-103">Модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c1650-103">ASP.NET Core Module</span></span>

<span data-ttu-id="c1650-104">Авторы: [Том Дайкстра (Tom Dykstra)](https://github.com/tdykstra), [Рик Штраль (Rick Strahl)](https://github.com/RickStrahl), [Крис Росс (Chris Ross)](https://github.com/Tratcher), [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT), [Сураб Ширхатти (Sourabh Shirhatti)](https://twitter.com/sshirhatti), [ Джастин Коталик (Justin Kotalik)](https://github.com/jkotalik) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c1650-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="c1650-105">Модуль ASP.NET Core имеет собственный модуль IIS, который подключается к конвейеру IIS для выполнения следующих задач:</span><span class="sxs-lookup"><span data-stu-id="c1650-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="c1650-106">Размещение приложения ASP.NET Core внутри рабочего процесса IIS (`w3wp.exe`). Это так называемая [модель внутрипроцессного размещения](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="c1650-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="c1650-107">Переадресация веб-запросов к серверной части приложения ASP.NET Core на [сервере Kestrel](xref:fundamentals/servers/kestrel). Это [модель размещения вне процесса](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="c1650-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="c1650-108">Поддерживаемые версии Windows:</span><span class="sxs-lookup"><span data-stu-id="c1650-108">Supported Windows versions:</span></span>

* <span data-ttu-id="c1650-109">Windows 7 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="c1650-109">Windows 7 or later</span></span>
* <span data-ttu-id="c1650-110">Windows Server 2008 R2 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="c1650-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="c1650-111">При размещении в процессе модуль использует реализацию внутрипроцессного сервера для IIS — HTTP-сервер IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="c1650-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="c1650-112">При размещении вне процесса модуль работает только с Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c1650-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="c1650-113">Модуль несовместим с [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="c1650-113">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="c1650-114">Модели размещения</span><span class="sxs-lookup"><span data-stu-id="c1650-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="c1650-115">Модель внутрипроцессного размещения</span><span class="sxs-lookup"><span data-stu-id="c1650-115">In-process hosting model</span></span>

<span data-ttu-id="c1650-116">Чтобы настроить приложение для внутрипроцессного размещения, добавьте свойство `<AspNetCoreHostingModel>` к файлу проекта приложения со значением `InProcess` (размещение вне процесса имеет значение `OutOfProcess`):</span><span class="sxs-lookup"><span data-stu-id="c1650-116">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="c1650-117">Модель внутрипроцессного размещения не поддерживается для приложений ASP.NET Core, предназначенных для .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="c1650-117">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="c1650-118">Если свойство `<AspNetCoreHostingModel>` отсутствует в файле, значение по умолчанию — `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="c1650-118">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="c1650-119">При внутрипроцессном размещении применимы следующие характеристики:</span><span class="sxs-lookup"><span data-stu-id="c1650-119">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="c1650-120">Вместо сервера [Kestrel](xref:fundamentals/servers/kestrel) используется HTTP-сервер IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="c1650-120">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="c1650-121">Для внутрипроцессной обработки [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) вызывает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="c1650-121">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="c1650-122">Регистрация `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="c1650-122">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="c1650-123">Настройка порта и базового пути, которые будет прослушивать сервер при выполнении за модулем ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c1650-123">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="c1650-124">Настройка перехвата ошибок запуска на узле.</span><span class="sxs-lookup"><span data-stu-id="c1650-124">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="c1650-125">[Атрибут requestTimeout](#attributes-of-the-aspnetcore-element) не применяется к внутрипроцессному размещению.</span><span class="sxs-lookup"><span data-stu-id="c1650-125">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="c1650-126">Совместное использование пула приложений среди приложений не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="c1650-126">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="c1650-127">Используйте один пул приложений для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="c1650-127">Use one app pool per app.</span></span>

* <span data-ttu-id="c1650-128">При использовании [веб-развертывания](/iis/publish/using-web-deploy/introduction-to-web-deploy) или размещении [файла app_offline.htm в развертывании](xref:host-and-deploy/iis/index#locked-deployment-files) вручную приложение может не завершить работу сразу при наличии открытого соединения.</span><span class="sxs-lookup"><span data-stu-id="c1650-128">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="c1650-129">Например, подключение websocket может задерживать завершение работы приложения.</span><span class="sxs-lookup"><span data-stu-id="c1650-129">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="c1650-130">Архитектура (разрядность) приложения и установленная среда выполнения (x64 или x86) должны соответствовать архитектуре пула приложений.</span><span class="sxs-lookup"><span data-stu-id="c1650-130">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="c1650-131">Если вы настраиваете узел приложения вручную с помощью `WebHostBuilder` (не используя [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) и приложение запускается напрямую на сервере Kestrel (с локальным размещением), вызывайте `UseKestrel` перед вызовом `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="c1650-131">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="c1650-132">Если изменить порядок, узел не запустится.</span><span class="sxs-lookup"><span data-stu-id="c1650-132">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="c1650-133">Обнаружены отключения клиентов.</span><span class="sxs-lookup"><span data-stu-id="c1650-133">Client disconnects are detected.</span></span> <span data-ttu-id="c1650-134">При отключении клиента происходит отмена токена отмены [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*).</span><span class="sxs-lookup"><span data-stu-id="c1650-134">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="c1650-135">В ASP.NET Core 2.2.1 и более ранних версий <xref:System.IO.Directory.GetCurrentDirectory*> возвращает рабочий каталог процесса, запущенного службами IIS, а не каталог приложения (например, *C:\Windows\System32\inetsrv* для *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="c1650-135">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="c1650-136">Пример кода, который задает текущий каталог приложения, см. в разделе [Класс CurrentDirectoryHelpers](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="c1650-136">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="c1650-137">Вызовите метод `SetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="c1650-137">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="c1650-138">Последующие вызовы <xref:System.IO.Directory.GetCurrentDirectory*> возвращают каталог приложения.</span><span class="sxs-lookup"><span data-stu-id="c1650-138">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="c1650-139">При размещении в процессе <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> не вызывается внутри для инициализации пользователя.</span><span class="sxs-lookup"><span data-stu-id="c1650-139">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="c1650-140">Таким образом, реализация <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>, используемая для преобразования утверждений после каждой проверки подлинности, не активируется по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="c1650-140">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="c1650-141">При преобразовании утверждений с реализацией <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> вызовите <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> для добавления служб проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="c1650-141">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="c1650-142">Модель размещения вне процесса</span><span class="sxs-lookup"><span data-stu-id="c1650-142">Out-of-process hosting model</span></span>

<span data-ttu-id="c1650-143">Чтобы настроить приложение для размещения вне процесса, используйте один из следующих подходов в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="c1650-143">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="c1650-144">Не указывайте свойство `<AspNetCoreHostingModel>`.</span><span class="sxs-lookup"><span data-stu-id="c1650-144">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="c1650-145">Если свойство `<AspNetCoreHostingModel>` отсутствует в файле, значение по умолчанию — `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="c1650-145">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="c1650-146">Установите для свойства `<AspNetCoreHostingModel>` значение `OutOfProcess` (внутрипроцессное размещение имеет значение `InProcess`):</span><span class="sxs-lookup"><span data-stu-id="c1650-146">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="c1650-147">Сервер [Kestrel](xref:fundamentals/servers/kestrel) используется вместо HTTP-сервера IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="c1650-147">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="c1650-148">Для внепроцессной обработки [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) вызывает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="c1650-148">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="c1650-149">Настройка порта и базового пути, которые будет прослушивать сервер при выполнении за модулем ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c1650-149">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="c1650-150">Настройка перехвата ошибок запуска на узле.</span><span class="sxs-lookup"><span data-stu-id="c1650-150">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="c1650-151">Изменения модели размещения</span><span class="sxs-lookup"><span data-stu-id="c1650-151">Hosting model changes</span></span>

<span data-ttu-id="c1650-152">Если параметр `hostingModel` изменяется в файле *web.config* (как описано в разделе [Конфигурация с помощью web.config](#configuration-with-webconfig)), модуль перезапускает рабочий процесс для служб IIS.</span><span class="sxs-lookup"><span data-stu-id="c1650-152">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="c1650-153">Для IIS Express модуль не перезапускает рабочий процесс, а запускает нормальное завершение работы текущего процесса IIS Express.</span><span class="sxs-lookup"><span data-stu-id="c1650-153">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="c1650-154">Следующий запрос для приложения порождает новый процесс IIS Express.</span><span class="sxs-lookup"><span data-stu-id="c1650-154">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="c1650-155">Имя процесса</span><span class="sxs-lookup"><span data-stu-id="c1650-155">Process name</span></span>

<span data-ttu-id="c1650-156">`Process.GetCurrentProcess().ProcessName` сообщает `w3wp`/`iisexpress` (внутри процесса) или `dotnet` (вне процесса).</span><span class="sxs-lookup"><span data-stu-id="c1650-156">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="c1650-157">Модуль ASP.NET Core имеет собственный модуль IIS, который подключается к конвейеру IIS для переадресации веб-запросов в серверные приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c1650-157">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="c1650-158">Поддерживаемые версии Windows:</span><span class="sxs-lookup"><span data-stu-id="c1650-158">Supported Windows versions:</span></span>

* <span data-ttu-id="c1650-159">Windows 7 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="c1650-159">Windows 7 or later</span></span>
* <span data-ttu-id="c1650-160">Windows Server 2008 R2 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="c1650-160">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="c1650-161">Модуль работает только с Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c1650-161">The module only works with Kestrel.</span></span> <span data-ttu-id="c1650-162">Модуль несовместим с [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="c1650-162">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="c1650-163">Так как приложения ASP.NET Core выполняются в процессе, отделенном от рабочего процесса IIS, этот модуль также обрабатывает управление процессами.</span><span class="sxs-lookup"><span data-stu-id="c1650-163">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="c1650-164">Модуль запускает процесс для приложения ASP.NET Core при поступлении первого запроса и перезапускает приложение при сбое.</span><span class="sxs-lookup"><span data-stu-id="c1650-164">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="c1650-165">Это, по сути, совпадает с поведением приложений ASP.NET 4.x, выполняемых внутрипроцессно в IIS и управляемых [службой активации процессов Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="c1650-165">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="c1650-166">На следующей схеме показана связь между IIS, модулем ASP.NET Core и приложением:</span><span class="sxs-lookup"><span data-stu-id="c1650-166">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Модуль ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="c1650-168">Запросы поступают из Интернета в драйвер HTTP.sys в режиме ядра.</span><span class="sxs-lookup"><span data-stu-id="c1650-168">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="c1650-169">Драйвер направляет запросы к службам IIS на настроенный порт веб-сайта — обычно 80 (HTTP) или 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="c1650-169">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="c1650-170">Модуль перенаправляет запросы Kestrel на случайный порт для приложения, отличающийся от порта 80 или 443.</span><span class="sxs-lookup"><span data-stu-id="c1650-170">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="c1650-171">Модуль задает порт с помощью переменной среды во время запуска, а ПО промежуточного слоя для интеграции IIS настраивает сервер для прослушивания `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="c1650-171">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="c1650-172">Выполняются дополнительные проверки, и запросы не из модуля отклоняются.</span><span class="sxs-lookup"><span data-stu-id="c1650-172">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="c1650-173">Модуль не поддерживает переадресацию по HTTPS, поэтому запросы переадресовываются по протоколу HTTP, даже если были получены IIS по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c1650-173">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="c1650-174">После того как Kestrel забирает запрос из модуля, запрос передается в конвейер ПО промежуточного слоя ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c1650-174">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="c1650-175">Конвейер ПО промежуточного слоя обрабатывает запрос и передает его в качестве экземпляра `HttpContext` в логику приложения.</span><span class="sxs-lookup"><span data-stu-id="c1650-175">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="c1650-176">ПО промежуточного слоя, добавленное интеграцией IIS, обновляет схему, удаленный IP-адрес и базовый путь для переадресации запроса в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c1650-176">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="c1650-177">Отклик приложения передается обратно в службу IIS, которая отправляет его обратно в HTTP-клиент, инициировавший запрос.</span><span class="sxs-lookup"><span data-stu-id="c1650-177">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="c1650-178">Многие собственные модули, такие как проверка подлинности Windows, остаются активными.</span><span class="sxs-lookup"><span data-stu-id="c1650-178">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="c1650-179">Дополнительные сведения о модулях IIS, активных с модулем ASP.NET Core, см. в разделе <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="c1650-179">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="c1650-180">Дополнительные возможности модуля ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="c1650-180">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="c1650-181">Задание переменных среды для рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="c1650-181">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="c1650-182">Внесение в журнал выходных данных stdout для хранилища файлов с целью устранения неполадок при запуске.</span><span class="sxs-lookup"><span data-stu-id="c1650-182">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="c1650-183">Переадресация токенов проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="c1650-183">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="c1650-184">Как установить и использовать модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c1650-184">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="c1650-185">Инструкции о том, как установить и использовать модуль ASP.NET Core, см. в разделе <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="c1650-185">For instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="c1650-186">Конфигурация с помощью файла web.config</span><span class="sxs-lookup"><span data-stu-id="c1650-186">Configuration with web.config</span></span>

<span data-ttu-id="c1650-187">Модуль ASP.NET Core настроен с помощью раздела `aspNetCore` узла `system.webServer` файла *web.config* на веб-сайте.</span><span class="sxs-lookup"><span data-stu-id="c1650-187">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="c1650-188">Следующий файл *web.config* публикуется для [зависимого от платформы развертывания](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) и настраивает модуль ASP.NET Core для обработки запросов к веб-сайту.</span><span class="sxs-lookup"><span data-stu-id="c1650-188">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="c1650-189">Следующий файл *web.config* опубликован для [автономного развертывания](/dotnet/articles/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="c1650-189">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="c1650-190">Значение `false` свойства <xref:System.Configuration.SectionInformation.InheritInChildApplications*> указывает, что параметры, заданные в элементе [\<расположение>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location), не наследуются приложениями, которые находятся во вложенном каталоге приложения.</span><span class="sxs-lookup"><span data-stu-id="c1650-190">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

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

<span data-ttu-id="c1650-191">Когда приложение развернуто в [службе приложений Azure](https://azure.microsoft.com/services/app-service/), путь `stdoutLogFile` задан как `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="c1650-191">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="c1650-192">Путь сохраняет журналы stdout в папке *LogFiles*, расположение которой автоматически создается службой.</span><span class="sxs-lookup"><span data-stu-id="c1650-192">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="c1650-193">Сведения о конфигурации дочерних приложений IIS см. здесь: <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="c1650-193">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="c1650-194">Атрибуты элемента aspNetCore</span><span class="sxs-lookup"><span data-stu-id="c1650-194">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="c1650-195">Атрибут</span><span class="sxs-lookup"><span data-stu-id="c1650-195">Attribute</span></span> | <span data-ttu-id="c1650-196">Описание</span><span class="sxs-lookup"><span data-stu-id="c1650-196">Description</span></span> | <span data-ttu-id="c1650-197">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="c1650-197">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="c1650-198">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-198">Optional string attribute.</span></span></p><p><span data-ttu-id="c1650-199">Аргументы для исполняемого файла, указанного в атрибуте **processPath**.</span><span class="sxs-lookup"><span data-stu-id="c1650-199">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="c1650-200">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-200">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="c1650-201">Если значение равно true, страница **502.5 — ошибка процесса** подавляется и страница в файле *web.config* с кодом состояния 502 имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="c1650-201">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="c1650-202">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-202">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="c1650-203">Если значение равно true, маркер безопасности отправляется дочернему процессу, прослушивающему порт %ASPNETCORE_PORT% как заголовок "MS-ASPNETCORE-WINAUTHTOKEN" каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="c1650-203">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="c1650-204">Этот процесс вызывает CloseHandle по этому маркеру безопасности каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="c1650-204">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="c1650-205">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-205">Optional string attribute.</span></span></p><p><span data-ttu-id="c1650-206">Указывает модель размещения — внутри процесса (`InProcess`) или вне процесса (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="c1650-206">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="c1650-207">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-207">Optional integer attribute.</span></span></p><p><span data-ttu-id="c1650-208">Указывает число экземпляров процесса, заданное в параметре **processPath**, которое может появиться для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="c1650-208">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="c1650-209">&dagger;Для внутрипроцессного размещения существует ограничение: `1`.</span><span class="sxs-lookup"><span data-stu-id="c1650-209">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="c1650-210">Параметр `processesPerApplication` не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="c1650-210">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="c1650-211">Этот атрибут будет удален в будущем выпуске.</span><span class="sxs-lookup"><span data-stu-id="c1650-211">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="c1650-212">По умолчанию: `1`</span><span class="sxs-lookup"><span data-stu-id="c1650-212">Default: `1`</span></span><br><span data-ttu-id="c1650-213">Минимум: `1`</span><span class="sxs-lookup"><span data-stu-id="c1650-213">Min: `1`</span></span><br><span data-ttu-id="c1650-214">Максимум: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="c1650-214">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="c1650-215">Обязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-215">Required string attribute.</span></span></p><p><span data-ttu-id="c1650-216">Путь к исполняемому файлу, который запускает процесс прослушивания HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="c1650-216">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="c1650-217">Поддерживаются относительные пути.</span><span class="sxs-lookup"><span data-stu-id="c1650-217">Relative paths are supported.</span></span> <span data-ttu-id="c1650-218">Если путь начинается с `.`, то начало пути считается относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="c1650-218">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="c1650-219">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-219">Optional integer attribute.</span></span></p><p><span data-ttu-id="c1650-220">Указывает количество сбоев за минуту, которыми может завершиться процесс, указанный в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="c1650-220">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="c1650-221">Если этот предел превышен, модуль останавливает запуск процесса на оставшуюся часть минуты.</span><span class="sxs-lookup"><span data-stu-id="c1650-221">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="c1650-222">Не поддерживается для внутрипроцессного размещения.</span><span class="sxs-lookup"><span data-stu-id="c1650-222">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="c1650-223">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="c1650-223">Default: `10`</span></span><br><span data-ttu-id="c1650-224">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="c1650-224">Min: `0`</span></span><br><span data-ttu-id="c1650-225">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="c1650-225">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="c1650-226">Необязательный атрибут timespan.</span><span class="sxs-lookup"><span data-stu-id="c1650-226">Optional timespan attribute.</span></span></p><p><span data-ttu-id="c1650-227">Указывает продолжительность, на протяжении которой модуль ASP.NET Core ожидает ответа от процесса, прослушивающего порт %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="c1650-227">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="c1650-228">В версиях модуля ASP.NET Core, поставляемых с выпуском ASP.NET Core 2.1 или новее, атрибут `requestTimeout` указывается в часах, минутах и секундах.</span><span class="sxs-lookup"><span data-stu-id="c1650-228">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="c1650-229">Не применяется к внутрипроцессному размещению.</span><span class="sxs-lookup"><span data-stu-id="c1650-229">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="c1650-230">Для внутрипроцессного размещения модуль ожидает, пока приложение не обработает запрос.</span><span class="sxs-lookup"><span data-stu-id="c1650-230">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="c1650-231">По умолчанию: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="c1650-231">Default: `00:02:00`</span></span><br><span data-ttu-id="c1650-232">Минимум: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="c1650-232">Min: `00:00:00`</span></span><br><span data-ttu-id="c1650-233">Максимум: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="c1650-233">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="c1650-234">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-234">Optional integer attribute.</span></span></p><p><span data-ttu-id="c1650-235">Длительность ожидания модуля в секундах, пока произойдет правильное выключение исполняемого файла при обнаружении файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="c1650-235">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="c1650-236">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="c1650-236">Default: `10`</span></span><br><span data-ttu-id="c1650-237">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="c1650-237">Min: `0`</span></span><br><span data-ttu-id="c1650-238">Максимум: `600`</span><span class="sxs-lookup"><span data-stu-id="c1650-238">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="c1650-239">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-239">Optional integer attribute.</span></span></p><p><span data-ttu-id="c1650-240">Время в секундах, которое модуль ожидает, пока запустится процесс прослушивания порта исполняемого файла.</span><span class="sxs-lookup"><span data-stu-id="c1650-240">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="c1650-241">Если этот предел превышен, модуль завершает процесс.</span><span class="sxs-lookup"><span data-stu-id="c1650-241">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="c1650-242">Модуль пытается перезапустить процесс при получении нового запроса и будет продолжать пытаться перезапустить процесс для последующих входящих запросов, если не удается запустить приложение определенное в атрибуте **rapidFailsPerMinute** количество раз за последнюю минуту.</span><span class="sxs-lookup"><span data-stu-id="c1650-242">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="c1650-243">Значение 0 (ноль) **не** считается бесконечным временем ожидания.</span><span class="sxs-lookup"><span data-stu-id="c1650-243">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="c1650-244">По умолчанию: `120`</span><span class="sxs-lookup"><span data-stu-id="c1650-244">Default: `120`</span></span><br><span data-ttu-id="c1650-245">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="c1650-245">Min: `0`</span></span><br><span data-ttu-id="c1650-246">Максимум: `3600`</span><span class="sxs-lookup"><span data-stu-id="c1650-246">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="c1650-247">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-247">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="c1650-248">Если значение равно true, **stdout** и **stderr** для процесса, указанного в атрибуте **processPath**, перенаправляются к файлу, заданному в атрибуте **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="c1650-248">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="c1650-249">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-249">Optional string attribute.</span></span></p><p><span data-ttu-id="c1650-250">Указывает относительный или абсолютный путь к файлу, для которого регистрируются **stdout** и **stderr** из процесса, указанного в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="c1650-250">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="c1650-251">Относительные пути задаются относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="c1650-251">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="c1650-252">Любой путь, начинающийся с `.`, относится к корневому каталогу веб-сайта, а все остальные пути рассматриваются как абсолютные пути.</span><span class="sxs-lookup"><span data-stu-id="c1650-252">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="c1650-253">Все папки, указанные в пути, создаются модулем при создании файла журнала.</span><span class="sxs-lookup"><span data-stu-id="c1650-253">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="c1650-254">С помощью разделителей подчеркивания метка времени, идентификатор процесса и расширение файла (*.log*) добавляются к последнему сегменту пути журнала **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="c1650-254">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="c1650-255">Если в качестве значения задано значение `.\logs\stdout`, например, журнал stdout сохраняется как *stdout_20180205194132_1934.log* в папке *журналов* с датой 5 февраля 2018 г. в 19:41:32 с идентификатором процесса 1934.</span><span class="sxs-lookup"><span data-stu-id="c1650-255">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="c1650-256">Атрибут</span><span class="sxs-lookup"><span data-stu-id="c1650-256">Attribute</span></span> | <span data-ttu-id="c1650-257">Описание</span><span class="sxs-lookup"><span data-stu-id="c1650-257">Description</span></span> | <span data-ttu-id="c1650-258">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="c1650-258">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="c1650-259">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-259">Optional string attribute.</span></span></p><p><span data-ttu-id="c1650-260">Аргументы для исполняемого файла, указанного в атрибуте **processPath**.</span><span class="sxs-lookup"><span data-stu-id="c1650-260">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="c1650-261">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-261">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="c1650-262">Если значение равно true, страница **502.5 — ошибка процесса** подавляется и страница в файле *web.config* с кодом состояния 502 имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="c1650-262">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="c1650-263">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-263">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="c1650-264">Если значение равно true, маркер безопасности отправляется дочернему процессу, прослушивающему порт %ASPNETCORE_PORT% как заголовок "MS-ASPNETCORE-WINAUTHTOKEN" каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="c1650-264">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="c1650-265">Этот процесс вызывает CloseHandle по этому маркеру безопасности каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="c1650-265">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="c1650-266">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-266">Optional integer attribute.</span></span></p><p><span data-ttu-id="c1650-267">Указывает число экземпляров процесса, заданное в параметре **processPath**, которое может появиться для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="c1650-267">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="c1650-268">Параметр `processesPerApplication` не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="c1650-268">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="c1650-269">Этот атрибут будет удален в будущем выпуске.</span><span class="sxs-lookup"><span data-stu-id="c1650-269">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="c1650-270">По умолчанию: `1`</span><span class="sxs-lookup"><span data-stu-id="c1650-270">Default: `1`</span></span><br><span data-ttu-id="c1650-271">Минимум: `1`</span><span class="sxs-lookup"><span data-stu-id="c1650-271">Min: `1`</span></span><br><span data-ttu-id="c1650-272">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="c1650-272">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="c1650-273">Обязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-273">Required string attribute.</span></span></p><p><span data-ttu-id="c1650-274">Путь к исполняемому файлу, который запускает процесс прослушивания HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="c1650-274">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="c1650-275">Поддерживаются относительные пути.</span><span class="sxs-lookup"><span data-stu-id="c1650-275">Relative paths are supported.</span></span> <span data-ttu-id="c1650-276">Если путь начинается с `.`, то начало пути считается относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="c1650-276">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="c1650-277">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-277">Optional integer attribute.</span></span></p><p><span data-ttu-id="c1650-278">Указывает количество сбоев за минуту, которыми может завершиться процесс, указанный в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="c1650-278">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="c1650-279">Если этот предел превышен, модуль останавливает запуск процесса на оставшуюся часть минуты.</span><span class="sxs-lookup"><span data-stu-id="c1650-279">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="c1650-280">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="c1650-280">Default: `10`</span></span><br><span data-ttu-id="c1650-281">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="c1650-281">Min: `0`</span></span><br><span data-ttu-id="c1650-282">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="c1650-282">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="c1650-283">Необязательный атрибут timespan.</span><span class="sxs-lookup"><span data-stu-id="c1650-283">Optional timespan attribute.</span></span></p><p><span data-ttu-id="c1650-284">Указывает продолжительность, на протяжении которой модуль ASP.NET Core ожидает ответа от процесса, прослушивающего порт %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="c1650-284">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="c1650-285">В версиях модуля ASP.NET Core, поставляемых с выпуском ASP.NET Core 2.1 или новее, атрибут `requestTimeout` указывается в часах, минутах и секундах.</span><span class="sxs-lookup"><span data-stu-id="c1650-285">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="c1650-286">По умолчанию: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="c1650-286">Default: `00:02:00`</span></span><br><span data-ttu-id="c1650-287">Минимум: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="c1650-287">Min: `00:00:00`</span></span><br><span data-ttu-id="c1650-288">Максимум: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="c1650-288">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="c1650-289">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-289">Optional integer attribute.</span></span></p><p><span data-ttu-id="c1650-290">Длительность ожидания модуля в секундах, пока произойдет правильное выключение исполняемого файла при обнаружении файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="c1650-290">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="c1650-291">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="c1650-291">Default: `10`</span></span><br><span data-ttu-id="c1650-292">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="c1650-292">Min: `0`</span></span><br><span data-ttu-id="c1650-293">Максимум: `600`</span><span class="sxs-lookup"><span data-stu-id="c1650-293">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="c1650-294">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-294">Optional integer attribute.</span></span></p><p><span data-ttu-id="c1650-295">Время в секундах, которое модуль ожидает, пока запустится процесс прослушивания порта исполняемого файла.</span><span class="sxs-lookup"><span data-stu-id="c1650-295">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="c1650-296">Если этот предел превышен, модуль завершает процесс.</span><span class="sxs-lookup"><span data-stu-id="c1650-296">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="c1650-297">Модуль пытается перезапустить процесс при получении нового запроса и будет продолжать пытаться перезапустить процесс для последующих входящих запросов, если не удается запустить приложение определенное в атрибуте **rapidFailsPerMinute** количество раз за последнюю минуту.</span><span class="sxs-lookup"><span data-stu-id="c1650-297">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="c1650-298">Значение 0 (ноль) **не** считается бесконечным временем ожидания.</span><span class="sxs-lookup"><span data-stu-id="c1650-298">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="c1650-299">По умолчанию: `120`</span><span class="sxs-lookup"><span data-stu-id="c1650-299">Default: `120`</span></span><br><span data-ttu-id="c1650-300">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="c1650-300">Min: `0`</span></span><br><span data-ttu-id="c1650-301">Максимум: `3600`</span><span class="sxs-lookup"><span data-stu-id="c1650-301">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="c1650-302">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-302">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="c1650-303">Если значение равно true, **stdout** и **stderr** для процесса, указанного в атрибуте **processPath**, перенаправляются к файлу, заданному в атрибуте **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="c1650-303">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="c1650-304">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-304">Optional string attribute.</span></span></p><p><span data-ttu-id="c1650-305">Указывает относительный или абсолютный путь к файлу, для которого регистрируются **stdout** и **stderr** из процесса, указанного в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="c1650-305">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="c1650-306">Относительные пути задаются относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="c1650-306">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="c1650-307">Любой путь, начинающийся с `.`, относится к корневому каталогу веб-сайта, а все остальные пути рассматриваются как абсолютные пути.</span><span class="sxs-lookup"><span data-stu-id="c1650-307">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="c1650-308">Все папки, указанные в пути, должны существовать, чтобы модуль мог создать файл журнала.</span><span class="sxs-lookup"><span data-stu-id="c1650-308">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="c1650-309">С помощью разделителей подчеркивания метка времени, идентификатор процесса и расширение файла (*.log*) добавляются к последнему сегменту пути журнала **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="c1650-309">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="c1650-310">Если в качестве значения задано значение `.\logs\stdout`, например, журнал stdout сохраняется как *stdout_20180205194132_1934.log* в папке *журналов* с датой 5 февраля 2018 г. в 19:41:32 с идентификатором процесса 1934.</span><span class="sxs-lookup"><span data-stu-id="c1650-310">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="c1650-311">Атрибут</span><span class="sxs-lookup"><span data-stu-id="c1650-311">Attribute</span></span> | <span data-ttu-id="c1650-312">Описание</span><span class="sxs-lookup"><span data-stu-id="c1650-312">Description</span></span> | <span data-ttu-id="c1650-313">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="c1650-313">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="c1650-314">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-314">Optional string attribute.</span></span></p><p><span data-ttu-id="c1650-315">Аргументы для исполняемого файла, указанного в атрибуте **processPath**.</span><span class="sxs-lookup"><span data-stu-id="c1650-315">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="c1650-316">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-316">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="c1650-317">Если значение равно true, страница **502.5 — ошибка процесса** подавляется и страница в файле *web.config* с кодом состояния 502 имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="c1650-317">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="c1650-318">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-318">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="c1650-319">Если значение равно true, маркер безопасности отправляется дочернему процессу, прослушивающему порт %ASPNETCORE_PORT% как заголовок "MS-ASPNETCORE-WINAUTHTOKEN" каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="c1650-319">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="c1650-320">Этот процесс вызывает CloseHandle по этому маркеру безопасности каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="c1650-320">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="c1650-321">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-321">Optional integer attribute.</span></span></p><p><span data-ttu-id="c1650-322">Указывает число экземпляров процесса, заданное в параметре **processPath**, которое может появиться для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="c1650-322">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="c1650-323">Параметр `processesPerApplication` не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="c1650-323">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="c1650-324">Этот атрибут будет удален в будущем выпуске.</span><span class="sxs-lookup"><span data-stu-id="c1650-324">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="c1650-325">По умолчанию: `1`</span><span class="sxs-lookup"><span data-stu-id="c1650-325">Default: `1`</span></span><br><span data-ttu-id="c1650-326">Минимум: `1`</span><span class="sxs-lookup"><span data-stu-id="c1650-326">Min: `1`</span></span><br><span data-ttu-id="c1650-327">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="c1650-327">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="c1650-328">Обязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-328">Required string attribute.</span></span></p><p><span data-ttu-id="c1650-329">Путь к исполняемому файлу, который запускает процесс прослушивания HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="c1650-329">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="c1650-330">Поддерживаются относительные пути.</span><span class="sxs-lookup"><span data-stu-id="c1650-330">Relative paths are supported.</span></span> <span data-ttu-id="c1650-331">Если путь начинается с `.`, то начало пути считается относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="c1650-331">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="c1650-332">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-332">Optional integer attribute.</span></span></p><p><span data-ttu-id="c1650-333">Указывает количество сбоев за минуту, которыми может завершиться процесс, указанный в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="c1650-333">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="c1650-334">Если этот предел превышен, модуль останавливает запуск процесса на оставшуюся часть минуты.</span><span class="sxs-lookup"><span data-stu-id="c1650-334">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="c1650-335">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="c1650-335">Default: `10`</span></span><br><span data-ttu-id="c1650-336">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="c1650-336">Min: `0`</span></span><br><span data-ttu-id="c1650-337">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="c1650-337">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="c1650-338">Необязательный атрибут timespan.</span><span class="sxs-lookup"><span data-stu-id="c1650-338">Optional timespan attribute.</span></span></p><p><span data-ttu-id="c1650-339">Указывает продолжительность, на протяжении которой модуль ASP.NET Core ожидает ответа от процесса, прослушивающего порт %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="c1650-339">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="c1650-340">В версиях модуля ASP.NET Core, поставляемых вместе с выпуском ASP.NET Core 2.0 или более ранними версиями, атрибут `requestTimeout` должен был быть задан в целых минутах (в противном случае для него по умолчанию задано значение 2 минуты).</span><span class="sxs-lookup"><span data-stu-id="c1650-340">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="c1650-341">По умолчанию: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="c1650-341">Default: `00:02:00`</span></span><br><span data-ttu-id="c1650-342">Минимум: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="c1650-342">Min: `00:00:00`</span></span><br><span data-ttu-id="c1650-343">Максимум: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="c1650-343">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="c1650-344">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-344">Optional integer attribute.</span></span></p><p><span data-ttu-id="c1650-345">Длительность ожидания модуля в секундах, пока произойдет правильное выключение исполняемого файла при обнаружении файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="c1650-345">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="c1650-346">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="c1650-346">Default: `10`</span></span><br><span data-ttu-id="c1650-347">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="c1650-347">Min: `0`</span></span><br><span data-ttu-id="c1650-348">Максимум: `600`</span><span class="sxs-lookup"><span data-stu-id="c1650-348">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="c1650-349">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-349">Optional integer attribute.</span></span></p><p><span data-ttu-id="c1650-350">Время в секундах, которое модуль ожидает, пока запустится процесс прослушивания порта исполняемого файла.</span><span class="sxs-lookup"><span data-stu-id="c1650-350">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="c1650-351">Если этот предел превышен, модуль завершает процесс.</span><span class="sxs-lookup"><span data-stu-id="c1650-351">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="c1650-352">Модуль пытается перезапустить процесс при получении нового запроса и будет продолжать пытаться перезапустить процесс для последующих входящих запросов, если не удается запустить приложение определенное в атрибуте **rapidFailsPerMinute** количество раз за последнюю минуту.</span><span class="sxs-lookup"><span data-stu-id="c1650-352">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="c1650-353">По умолчанию: `120`</span><span class="sxs-lookup"><span data-stu-id="c1650-353">Default: `120`</span></span><br><span data-ttu-id="c1650-354">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="c1650-354">Min: `0`</span></span><br><span data-ttu-id="c1650-355">Максимум: `3600`</span><span class="sxs-lookup"><span data-stu-id="c1650-355">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="c1650-356">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-356">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="c1650-357">Если значение равно true, **stdout** и **stderr** для процесса, указанного в атрибуте **processPath**, перенаправляются к файлу, заданному в атрибуте **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="c1650-357">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="c1650-358">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1650-358">Optional string attribute.</span></span></p><p><span data-ttu-id="c1650-359">Указывает относительный или абсолютный путь к файлу, для которого регистрируются **stdout** и **stderr** из процесса, указанного в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="c1650-359">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="c1650-360">Относительные пути задаются относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="c1650-360">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="c1650-361">Любой путь, начинающийся с `.`, относится к корневому каталогу веб-сайта, а все остальные пути рассматриваются как абсолютные пути.</span><span class="sxs-lookup"><span data-stu-id="c1650-361">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="c1650-362">Все папки, указанные в пути, должны существовать, чтобы модуль мог создать файл журнала.</span><span class="sxs-lookup"><span data-stu-id="c1650-362">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="c1650-363">С помощью разделителей подчеркивания метка времени, идентификатор процесса и расширение файла (*.log*) добавляются к последнему сегменту пути журнала **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="c1650-363">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="c1650-364">Если в качестве значения задано значение `.\logs\stdout`, например, журнал stdout сохраняется как *stdout_20180205194132_1934.log* в папке *журналов* с датой 5 февраля 2018 г. в 19:41:32 с идентификатором процесса 1934.</span><span class="sxs-lookup"><span data-stu-id="c1650-364">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="c1650-365">Настройка переменных среды</span><span class="sxs-lookup"><span data-stu-id="c1650-365">Setting environment variables</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c1650-366">Переменные среды для процесса можно указать в атрибуте `processPath`.</span><span class="sxs-lookup"><span data-stu-id="c1650-366">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="c1650-367">Укажите переменную среды с дочерним элементом `<environmentVariable>` элемента коллекции `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="c1650-367">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="c1650-368">Переменные среды, установленные в этом разделе, имеют приоритет над переменными системной среды.</span><span class="sxs-lookup"><span data-stu-id="c1650-368">Environment variables set in this section take precedence over system environment variables.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c1650-369">Переменные среды для процесса можно указать в атрибуте `processPath`.</span><span class="sxs-lookup"><span data-stu-id="c1650-369">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="c1650-370">Укажите переменную среды с дочерним элементом `<environmentVariable>` элемента коллекции `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="c1650-370">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="c1650-371">Переменные среды, заданные в этом разделе, конфликтуют с переменными системной среды с такими же именами.</span><span class="sxs-lookup"><span data-stu-id="c1650-371">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="c1650-372">Если переменная среды задана и в файле *web.config*, и на уровне системы в Windows, значение из файла *web.config* добавляется к значению переменной системной среды (например, `ASPNETCORE_ENVIRONMENT: Development;Development`), которая препятствует запуску приложения.</span><span class="sxs-lookup"><span data-stu-id="c1650-372">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

::: moniker-end

<span data-ttu-id="c1650-373">В следующем примере устанавливаются две переменные среды.</span><span class="sxs-lookup"><span data-stu-id="c1650-373">The following example sets two environment variables.</span></span> <span data-ttu-id="c1650-374">`ASPNETCORE_ENVIRONMENT` настраивает среду приложения для `Development`.</span><span class="sxs-lookup"><span data-stu-id="c1650-374">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="c1650-375">Разработчик может временно задать это значение в файле *web.config*, чтобы принудительно загрузить [Страницу исключений для разработчиков](xref:fundamentals/error-handling) при отладке исключения приложения.</span><span class="sxs-lookup"><span data-stu-id="c1650-375">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="c1650-376">`CONFIG_DIR` — пример пользовательской переменной среды, где разработчик написал код, который считывает значение при запуске, чтобы сформировать путь для загрузки файла конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="c1650-376">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="c1650-377">Вместо задания среды напрямую в *web.config* включите свойство `<EnvironmentName>` в профиль публикации (*.pubxml*) или файл проекта.</span><span class="sxs-lookup"><span data-stu-id="c1650-377">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="c1650-378">При этом подходе во время публикации проекта среда задается в файле *web.config*:</span><span class="sxs-lookup"><span data-stu-id="c1650-378">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="c1650-379">Установите только переменную среды `ASPNETCORE_ENVIRONMENT` для `Development` на серверах промежуточных процессов и тестирования, которые недоступны для ненадежных сетей, таких как Интернет.</span><span class="sxs-lookup"><span data-stu-id="c1650-379">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="c1650-380">App_offline.htm</span><span class="sxs-lookup"><span data-stu-id="c1650-380">app_offline.htm</span></span>

<span data-ttu-id="c1650-381">Если в корневом каталоге приложения обнаружен файл с именем *app_offline.htm*, модуль ASP.NET Core пытается корректно закрыть приложение и прекратить обработку входящих запросов.</span><span class="sxs-lookup"><span data-stu-id="c1650-381">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="c1650-382">Если приложение по-прежнему выполняется через количество секунд, определенное атрибутом `shutdownTimeLimit`, модуль ASP.NET Core завершает выполняющийся процесс.</span><span class="sxs-lookup"><span data-stu-id="c1650-382">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="c1650-383">Хотя файл *app_offline.htm* присутствует, модуль ASP.NET Core отвечает на запросы, отправляя назад содержимое файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="c1650-383">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="c1650-384">Когда *app_offline.htm* файл удаляется, следующий запрос запускает приложение.</span><span class="sxs-lookup"><span data-stu-id="c1650-384">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="c1650-385">При использовании модели размещения вне процесса приложение может не завершать работу немедленно при наличии открытого соединения.</span><span class="sxs-lookup"><span data-stu-id="c1650-385">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="c1650-386">Например, подключение websocket может задерживать завершение работы приложения.</span><span class="sxs-lookup"><span data-stu-id="c1650-386">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="c1650-387">Страница ошибок запуска</span><span class="sxs-lookup"><span data-stu-id="c1650-387">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="c1650-388">Если при внутри- или внепроцессном размещении происходит сбой запуска приложения, открываются страницы пользовательских сообщений об ошибках.</span><span class="sxs-lookup"><span data-stu-id="c1650-388">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="c1650-389">Если модулю ASP.NET Core не удается найти внутри- или внепроцессный обработчик запросов, откроется страница кода состояния *500.0 — ошибка загрузки внутри- или внепроцессного обработчика запросов*.</span><span class="sxs-lookup"><span data-stu-id="c1650-389">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="c1650-390">Если в модели размещения внутри процесса модулю ASP.NET Core не удается запустить приложение, откроется страница кода состояния *500.30 — ошибка запуска*.</span><span class="sxs-lookup"><span data-stu-id="c1650-390">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="c1650-391">Если в модели размещения вне процесса модулю ASP.NET Core не удается запустить серверный процесс или начинается серверный процесс, но ему не удается прослушать настроенный порт, появится страница кода состояния *502.5 — ошибка процесса*.</span><span class="sxs-lookup"><span data-stu-id="c1650-391">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="c1650-392">Чтобы подавить отображение этой странице и вернуться к странице IIS кода состояния 5xx по умолчанию, используйте атрибут `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="c1650-392">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="c1650-393">Дополнительные сведения о настройке пользовательских сообщений об ошибках см. в разделе [Ошибки HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="c1650-393">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="c1650-394">Если модулю ASP.NET Core не удается запустить серверный процесс или начинается серверный процесс, но ему не удается прослушать настроенный порт, появится страница кода состояния *502.5 — ошибка процесса*.</span><span class="sxs-lookup"><span data-stu-id="c1650-394">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="c1650-395">Чтобы подавить эту страницу и вернуться к странице IIS кода состояния 502 по умолчанию, используйте атрибут `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="c1650-395">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="c1650-396">Дополнительные сведения о настройке пользовательских сообщений об ошибках см. в разделе [Ошибки HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="c1650-396">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Страница кода состояния "502.5 — ошибка процесса"](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="c1650-398">Создание и перенаправление журнала</span><span class="sxs-lookup"><span data-stu-id="c1650-398">Log creation and redirection</span></span>

<span data-ttu-id="c1650-399">Модуль ASP.NET Core перенаправляет выходные потоки консоли stdout и stderr на диск, если заданы атрибуты `stdoutLogEnabled` и `stdoutLogFile` элемента `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="c1650-399">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="c1650-400">Все папки, указанные в пути `stdoutLogFile`, создаются модулем при создании файла журнала.</span><span class="sxs-lookup"><span data-stu-id="c1650-400">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="c1650-401">Пул приложений должен иметь доступ на запись в папку, где записываются журналы (используйте атрибут `IIS AppPool\<app_pool_name>` для предоставления разрешения на запись).</span><span class="sxs-lookup"><span data-stu-id="c1650-401">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="c1650-402">Журналы не выполняют циклический сдвиг, пока не произойдет процесс перезапуска или перезагрузки.</span><span class="sxs-lookup"><span data-stu-id="c1650-402">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="c1650-403">Администратор несет ответственность за ограничение дискового пространства, которое потребляют журналы.</span><span class="sxs-lookup"><span data-stu-id="c1650-403">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="c1650-404">Использование журнала stdout рекомендуется только для устранения неполадок при запуске приложений.</span><span class="sxs-lookup"><span data-stu-id="c1650-404">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="c1650-405">Не используйте журнал stdout для ведения общего журнала приложений.</span><span class="sxs-lookup"><span data-stu-id="c1650-405">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="c1650-406">Для обычного входа в приложение ASP.NET Core используйте библиотеку ведения журналов, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов.</span><span class="sxs-lookup"><span data-stu-id="c1650-406">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="c1650-407">Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="c1650-407">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="c1650-408">При создании файла журнала автоматически добавляются отметка времени и расширение файла.</span><span class="sxs-lookup"><span data-stu-id="c1650-408">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="c1650-409">Имя файла журнала составляется путем добавления метки времени, идентификатора процесса и расширения файла (*.log*) к последнему сегменту атрибута пути `stdoutLogFile` (обычно *stdout*) с символами подчеркивания в качестве разделителей.</span><span class="sxs-lookup"><span data-stu-id="c1650-409">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="c1650-410">Если атрибут пути `stdoutLogFile` заканчивается элементом *stdout*, журнал приложения с идентификатором 1934, созданный 5 февраля 2018 г. в 19:42:32, будет иметь имя *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="c1650-410">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="c1650-411">Если `stdoutLogEnabled` имеет значение false, возникающие при запуске приложения ошибки записываются и передаются в журнал событий (макс. 30 КБ).</span><span class="sxs-lookup"><span data-stu-id="c1650-411">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="c1650-412">После запуска все дополнительные журналы удаляются.</span><span class="sxs-lookup"><span data-stu-id="c1650-412">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="c1650-413">В следующем примере элемент `aspNetCore` настраивает ведение журнала stdout для приложения, размещенного в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="c1650-413">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="c1650-414">Для локального ведения журнала допустим локальный или общий сетевой путь.</span><span class="sxs-lookup"><span data-stu-id="c1650-414">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="c1650-415">Убедитесь, что идентификатор пользователя AppPool имеет разрешение на запись по указанному пути.</span><span class="sxs-lookup"><span data-stu-id="c1650-415">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="c1650-416">Расширенные журналы диагностики</span><span class="sxs-lookup"><span data-stu-id="c1650-416">Enhanced diagnostic logs</span></span>

<span data-ttu-id="c1650-417">Модуль ASP.NET Core можно настроить. Он позволяет работать с расширенными журналами диагностики.</span><span class="sxs-lookup"><span data-stu-id="c1650-417">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="c1650-418">Добавьте элемент `<handlerSettings>` в элемент `<aspNetCore>` в файле *web.config*. Задайте параметру `debugLevel` значение `TRACE`, чтобы обеспечить высокую точность диагностических сведений:</span><span class="sxs-lookup"><span data-stu-id="c1650-418">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="c1650-419">Значения уровня отладки (`debugLevel`) могут включать уровень и расположение.</span><span class="sxs-lookup"><span data-stu-id="c1650-419">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="c1650-420">Уровни (в порядке возрастания степени детализации):</span><span class="sxs-lookup"><span data-stu-id="c1650-420">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="c1650-421">ОШИБКА</span><span class="sxs-lookup"><span data-stu-id="c1650-421">ERROR</span></span>
* <span data-ttu-id="c1650-422">ПРЕДУПРЕЖДЕНИЕ</span><span class="sxs-lookup"><span data-stu-id="c1650-422">WARNING</span></span>
* <span data-ttu-id="c1650-423">ИНФОРМАЦИЯ</span><span class="sxs-lookup"><span data-stu-id="c1650-423">INFO</span></span>
* <span data-ttu-id="c1650-424">TRACE</span><span class="sxs-lookup"><span data-stu-id="c1650-424">TRACE</span></span>

<span data-ttu-id="c1650-425">Расположения (допускаются несколько расположений):</span><span class="sxs-lookup"><span data-stu-id="c1650-425">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="c1650-426">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="c1650-426">CONSOLE</span></span>
* <span data-ttu-id="c1650-427">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="c1650-427">EVENTLOG</span></span>
* <span data-ttu-id="c1650-428">FILE</span><span class="sxs-lookup"><span data-stu-id="c1650-428">FILE</span></span>

<span data-ttu-id="c1650-429">Параметры обработчика могут быть указаны с помощью переменных среды:</span><span class="sxs-lookup"><span data-stu-id="c1650-429">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="c1650-430">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Путь к файлу журнала отладки.</span><span class="sxs-lookup"><span data-stu-id="c1650-430">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="c1650-431">(По умолчанию: *aspnetcore debug.log*)</span><span class="sxs-lookup"><span data-stu-id="c1650-431">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="c1650-432">`ASPNETCORE_MODULE_DEBUG` &ndash; Параметр уровня отладки.</span><span class="sxs-lookup"><span data-stu-id="c1650-432">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="c1650-433">**Не** оставляйте ведение журнала отладки включенным в развертывании дольше того времени, которое требуется для устранения проблемы.</span><span class="sxs-lookup"><span data-stu-id="c1650-433">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="c1650-434">Размер журнала не ограничен.</span><span class="sxs-lookup"><span data-stu-id="c1650-434">The size of the log isn't limited.</span></span> <span data-ttu-id="c1650-435">Если оставить журнал отладки включенным, он может исчерпать все доступное место на диске и привести к сбою сервера или службы приложений.</span><span class="sxs-lookup"><span data-stu-id="c1650-435">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="c1650-436">См. пример элемента `aspNetCore` в файле *web.config* в разделе [Конфигурация с помощью файла web.config](#configuration-with-webconfig).</span><span class="sxs-lookup"><span data-stu-id="c1650-436">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="c1650-437">Конфигурация прокси-сервера использует протокол HTTP и токен связывания</span><span class="sxs-lookup"><span data-stu-id="c1650-437">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="c1650-438">*Применяется только к размещению вне процесса.*</span><span class="sxs-lookup"><span data-stu-id="c1650-438">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="c1650-439">Прокси-сервер, созданный между модулем ASP.NET Core и Kestrel, использует протокол HTTP.</span><span class="sxs-lookup"><span data-stu-id="c1650-439">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="c1650-440">Использование HTTP позволяет оптимизировать производительность, так как трафик между модулем и Kestrel передается на петлевой адрес в сетевом интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="c1650-440">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="c1650-441">Отсутствует риск перехвата трафика между модулем и Kestrel из расположения на сервере.</span><span class="sxs-lookup"><span data-stu-id="c1650-441">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="c1650-442">Токен связывания гарантирует, что полученные Kestrel запросы были переданы службами IIS, а не из какого-либо другого источника.</span><span class="sxs-lookup"><span data-stu-id="c1650-442">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="c1650-443">Этот токен создается и задается модулем в переменную среды (`ASPNETCORE_TOKEN`).</span><span class="sxs-lookup"><span data-stu-id="c1650-443">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="c1650-444">Он также задается в заголовке (`MS-ASPNETCORE-TOKEN`) каждого запроса, переданного через прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="c1650-444">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="c1650-445">ПО промежуточного слоя IIS проверяет каждый получаемый запрос, чтобы убедиться, что заголовок с токеном связывания соответствует значению переменной среды.</span><span class="sxs-lookup"><span data-stu-id="c1650-445">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="c1650-446">Если значения токена не совпадают, запрос заносится в журнал и отклоняется.</span><span class="sxs-lookup"><span data-stu-id="c1650-446">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="c1650-447">Переменная среды с токеном связывания и трафик между модулем и Kestrel недоступны из расположения на сервере.</span><span class="sxs-lookup"><span data-stu-id="c1650-447">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="c1650-448">Не зная значение токена связывания, злоумышленник не может отправлять запросы, обходящие проверку в ПО промежуточного слоя IIS.</span><span class="sxs-lookup"><span data-stu-id="c1650-448">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="c1650-449">Модуль ASP.NET Core с общей конфигурацией IIS</span><span class="sxs-lookup"><span data-stu-id="c1650-449">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="c1650-450">Программа установки модуля ASP.NET Core запускается с правами учетной записи **TrustedInstaller**.</span><span class="sxs-lookup"><span data-stu-id="c1650-450">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="c1650-451">Поскольку учетная запись локальной системы не имеет разрешения на изменение пути к общей папке, используемого общей конфигурацией IIS, установщик получает ошибку отказа в доступе при попытке настроить параметры модуля в файле *applicationHost.config* общей папки.</span><span class="sxs-lookup"><span data-stu-id="c1650-451">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="c1650-452">При использовании общей конфигурации IIS на том же компьютере, где установлены службы IIS, запустите установщик пакета размещения ASP.NET Core с параметром `OPT_NO_SHARED_CONFIG_CHECK` со значением `1`:</span><span class="sxs-lookup"><span data-stu-id="c1650-452">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="c1650-453">Если путь к общей конфигурации не на том же компьютере, где установлены службы IIS, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="c1650-453">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="c1650-454">Отключите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="c1650-454">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="c1650-455">Запустите установщик.</span><span class="sxs-lookup"><span data-stu-id="c1650-455">Run the installer.</span></span>
1. <span data-ttu-id="c1650-456">Экспортируйте обновленный файл *applicationHost.config* в общую папку.</span><span class="sxs-lookup"><span data-stu-id="c1650-456">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="c1650-457">Повторно включите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="c1650-457">Re-enable the IIS Shared Configuration.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="c1650-458">При использовании общей конфигурации IIS выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="c1650-458">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="c1650-459">Отключите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="c1650-459">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="c1650-460">Запустите установщик.</span><span class="sxs-lookup"><span data-stu-id="c1650-460">Run the installer.</span></span>
1. <span data-ttu-id="c1650-461">Экспортируйте обновленный файл *applicationHost.config* в общую папку.</span><span class="sxs-lookup"><span data-stu-id="c1650-461">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="c1650-462">Повторно включите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="c1650-462">Re-enable the IIS Shared Configuration.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="application-initialization"></a><span data-ttu-id="c1650-463">Инициализация приложений</span><span class="sxs-lookup"><span data-stu-id="c1650-463">Application Initialization</span></span>

<span data-ttu-id="c1650-464">Функция [инициализации приложений](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) в IIS отправляет в приложение HTTP-запрос при запуске или перезапуске пула приложений.</span><span class="sxs-lookup"><span data-stu-id="c1650-464">[IIS Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) is an IIS feature that sends an HTTP request to the app when the app pool starts or is recycled.</span></span> <span data-ttu-id="c1650-465">Этот запрос инициирует запуск приложения.</span><span class="sxs-lookup"><span data-stu-id="c1650-465">The request triggers the app to start.</span></span> <span data-ttu-id="c1650-466">Инициализация приложений может использоваться в [модели внутрипроцессного размещения](xref:fundamentals/servers/index#in-process-hosting-model) и [модели внепроцессного размещения](xref:fundamentals/servers/index#out-of-process-hosting-model) с модулем ASP.NET Core версии 2.</span><span class="sxs-lookup"><span data-stu-id="c1650-466">Application Initialization can be used by both the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model) and [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model) with the ASP.NET Core Module version 2.</span></span>

<span data-ttu-id="c1650-467">Чтобы включить инициализацию приложений, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="c1650-467">To enable Application Initialization:</span></span>

1. <span data-ttu-id="c1650-468">Убедитесь, что включена роль инициализации приложения IIS.</span><span class="sxs-lookup"><span data-stu-id="c1650-468">Confirm that the IIS Application Initialization role feature in enabled:</span></span>
   * <span data-ttu-id="c1650-469">На ОС Windows 7 и более поздних версий. Последовательно выберите **Панель управления** > **Программы** > **Программы и компоненты** > **Включение или отключение компонентов Windows** (в левой части экрана).</span><span class="sxs-lookup"><span data-stu-id="c1650-469">On Windows 7 or later: Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="c1650-470">Откройте **Службы IIS** > **Службы Интернета** > **Компоненты разработки приложений**.</span><span class="sxs-lookup"><span data-stu-id="c1650-470">Open **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="c1650-471">Установите флажок **Инициализация приложений**.</span><span class="sxs-lookup"><span data-stu-id="c1650-471">Select the check box for **Application Initialization**.</span></span>
   * <span data-ttu-id="c1650-472">В Windows Server 2008 R2 или более поздней версии откройте **мастер добавления ролей и компонентов**.</span><span class="sxs-lookup"><span data-stu-id="c1650-472">On Windows Server 2008 R2 or later, open the **Add Roles and Features Wizard**.</span></span> <span data-ttu-id="c1650-473">Добравшись до панели **Выбор служб ролей**, откройте узел **Разработка приложений** и установите флажок **Инициализация приложений**.</span><span class="sxs-lookup"><span data-stu-id="c1650-473">When you reach the **Select role services** panel, open the **Application Development** node and select the **Application Initialization** check box.</span></span>
1. <span data-ttu-id="c1650-474">В диспетчере IIS выберите **Пулы приложений** на панели **Подключения**.</span><span class="sxs-lookup"><span data-stu-id="c1650-474">In IIS Manager, select **Application Pools** in the **Connections** panel.</span></span>
1. <span data-ttu-id="c1650-475">В списке выберите пул приложений для приложения.</span><span class="sxs-lookup"><span data-stu-id="c1650-475">Select the app's app pool in the list.</span></span>
1. <span data-ttu-id="c1650-476">Выберите **Дополнительные параметры** в разделе **Изменение пула приложений** панели **Действия**.</span><span class="sxs-lookup"><span data-stu-id="c1650-476">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="c1650-477">Для параметра **Режим запуска** выберите вариант **AlwaysRunning**.</span><span class="sxs-lookup"><span data-stu-id="c1650-477">Set **Start Mode** to **AlwaysRunning**.</span></span>
1. <span data-ttu-id="c1650-478">Откройте узел **Сайты** на панели **Подключения**.</span><span class="sxs-lookup"><span data-stu-id="c1650-478">Open the **Sites** node in the **Connections** panel.</span></span>
1. <span data-ttu-id="c1650-479">Выберите нужное приложение.</span><span class="sxs-lookup"><span data-stu-id="c1650-479">Select the app.</span></span>
1. <span data-ttu-id="c1650-480">Выберите **Дополнительные параметры** в разделе **Управление веб-сайтом** на панели **Действия**.</span><span class="sxs-lookup"><span data-stu-id="c1650-480">Select **Advanced Settings** under **Manage Website** in the **Actions** panel.</span></span>
1. <span data-ttu-id="c1650-481">Для параметра **Предварительная загрузка включена** выберите значение **True**.</span><span class="sxs-lookup"><span data-stu-id="c1650-481">Set **Preload Enabled** to **True**.</span></span>

<span data-ttu-id="c1650-482">Дополнительные сведения см. в статье [об инициализации приложений в IIS 8.0](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization).</span><span class="sxs-lookup"><span data-stu-id="c1650-482">For more information, see [IIS 8.0 Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization).</span></span>

<span data-ttu-id="c1650-483">Приложения, которые используют [модель внепроцессного размещения](xref:fundamentals/servers/index#out-of-process-hosting-model), должны периодически проверять связь с приложением с помощью внешней службы, чтобы поддерживать его в рабочем состоянии.</span><span class="sxs-lookup"><span data-stu-id="c1650-483">Apps that use the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model) must use an external service to periodically ping the app in order to keep it running.</span></span>

::: moniker-end

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="c1650-484">Версия модуля и журналы установщика хостинга Bundle</span><span class="sxs-lookup"><span data-stu-id="c1650-484">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="c1650-485">Чтобы определить версию установщика модуля ASP.NET Core, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="c1650-485">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="c1650-486">В системе размещения перейдите к папке *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="c1650-486">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="c1650-487">Найдите файл *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="c1650-487">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="c1650-488">Щелкните правой кнопкой мыши файл и выберите **Свойства** из контекстного меню.</span><span class="sxs-lookup"><span data-stu-id="c1650-488">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="c1650-489">Выберите вкладку **Сведения**. **Версия файла** и **Версия продукта** дают представление об установленной версии модуля.</span><span class="sxs-lookup"><span data-stu-id="c1650-489">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="c1650-490">Журналы установщика хостинга Bundle для модуля находятся в папке *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. Этот файл имеет имя *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="c1650-490">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="c1650-491">Модуль, схемы и расположение файлов конфигурации</span><span class="sxs-lookup"><span data-stu-id="c1650-491">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="c1650-492">Module</span><span class="sxs-lookup"><span data-stu-id="c1650-492">Module</span></span>

<span data-ttu-id="c1650-493">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="c1650-493">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="c1650-494">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="c1650-494">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="c1650-495">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="c1650-495">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="c1650-496">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="c1650-496">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="c1650-497">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="c1650-497">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="c1650-498">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="c1650-498">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="c1650-499">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="c1650-499">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="c1650-500">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="c1650-500">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="c1650-501">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="c1650-501">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="c1650-502">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="c1650-502">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="c1650-503">Схема</span><span class="sxs-lookup"><span data-stu-id="c1650-503">Schema</span></span>

<span data-ttu-id="c1650-504">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="c1650-504">**IIS**</span></span>

* <span data-ttu-id="c1650-505">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="c1650-505">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="c1650-506">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="c1650-506">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

<span data-ttu-id="c1650-507">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="c1650-507">**IIS Express**</span></span>

* <span data-ttu-id="c1650-508">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="c1650-508">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="c1650-509">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="c1650-509">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="c1650-510">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="c1650-510">Configuration</span></span>

<span data-ttu-id="c1650-511">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="c1650-511">**IIS**</span></span>

* <span data-ttu-id="c1650-512">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="c1650-512">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="c1650-513">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="c1650-513">**IIS Express**</span></span>

* <span data-ttu-id="c1650-514">Visual Studio: {КОРЕНЬ ПРИЛОЖЕНИЯ}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="c1650-514">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="c1650-515">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="c1650-515">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="c1650-516">Файлы можно найти путем поиска *aspnetcore* в файле *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="c1650-516">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c1650-517">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c1650-517">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="c1650-518">Репозиторий GitHub для модуля ASP.NET Core (справочные материалы)</span><span class="sxs-lookup"><span data-stu-id="c1650-518">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
