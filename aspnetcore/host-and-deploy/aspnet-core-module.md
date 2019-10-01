---
title: Модуль ASP.NET Core
author: guardrex
description: Сведения о настройке модуля ASP.NET Core для размещения приложений ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/24/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 811aafce6686b446440b146efd7449b598ed1722
ms.sourcegitcommit: e54672f5c493258dc449fac5b98faf47eb123b28
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2019
ms.locfileid: "71248343"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="4a35d-103">Модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4a35d-103">ASP.NET Core Module</span></span>

<span data-ttu-id="4a35d-104">Авторы: [Том Дайкстра (Tom Dykstra)](https://github.com/tdykstra), [Рик Штраль (Rick Strahl)](https://github.com/RickStrahl), [Крис Росс (Chris Ross)](https://github.com/Tratcher), [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT), [Сураб Ширхатти (Sourabh Shirhatti)](https://twitter.com/sshirhatti), [ Джастин Коталик (Justin Kotalik)](https://github.com/jkotalik) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4a35d-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4a35d-105">Модуль ASP.NET Core имеет собственный модуль IIS, который подключается к конвейеру IIS для выполнения следующих задач:</span><span class="sxs-lookup"><span data-stu-id="4a35d-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="4a35d-106">Размещение приложения ASP.NET Core внутри рабочего процесса IIS (`w3wp.exe`). Это так называемая [модель внутрипроцессного размещения](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="4a35d-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="4a35d-107">Переадресация веб-запросов к серверной части приложения ASP.NET Core на [сервере Kestrel](xref:fundamentals/servers/kestrel). Это [модель размещения вне процесса](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="4a35d-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="4a35d-108">Поддерживаемые версии Windows:</span><span class="sxs-lookup"><span data-stu-id="4a35d-108">Supported Windows versions:</span></span>

* <span data-ttu-id="4a35d-109">Windows 7 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="4a35d-109">Windows 7 or later</span></span>
* <span data-ttu-id="4a35d-110">Windows Server 2008 R2 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="4a35d-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="4a35d-111">При размещении в процессе модуль использует реализацию внутрипроцессного сервера для IIS — HTTP-сервер IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="4a35d-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="4a35d-112">При размещении вне процесса модуль работает только с Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4a35d-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="4a35d-113">Модуль не работает с [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="4a35d-113">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="4a35d-114">Модели размещения</span><span class="sxs-lookup"><span data-stu-id="4a35d-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="4a35d-115">Модель внутрипроцессного размещения</span><span class="sxs-lookup"><span data-stu-id="4a35d-115">In-process hosting model</span></span>

<span data-ttu-id="4a35d-116">По умолчанию приложения ASP.NET Core используют модель внутрипроцессного размещения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-116">ASP.NET Core apps default to the in-process hosting model.</span></span>

<span data-ttu-id="4a35d-117">При внутрипроцессном размещении применимы следующие характеристики:</span><span class="sxs-lookup"><span data-stu-id="4a35d-117">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="4a35d-118">Вместо сервера [Kestrel](xref:fundamentals/servers/kestrel) используется HTTP-сервер IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="4a35d-118">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="4a35d-119">Для внутрипроцессной обработки [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) вызывает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="4a35d-119">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="4a35d-120">Регистрация `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-120">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="4a35d-121">Настройка порта и базового пути, которые будет прослушивать сервер при выполнении за модулем ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4a35d-121">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="4a35d-122">Настройка перехвата ошибок запуска на узле.</span><span class="sxs-lookup"><span data-stu-id="4a35d-122">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="4a35d-123">[Атрибут requestTimeout](#attributes-of-the-aspnetcore-element) не применяется к внутрипроцессному размещению.</span><span class="sxs-lookup"><span data-stu-id="4a35d-123">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="4a35d-124">Совместное использование пула приложений среди приложений не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="4a35d-124">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="4a35d-125">Используйте один пул приложений для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-125">Use one app pool per app.</span></span>

* <span data-ttu-id="4a35d-126">При использовании [веб-развертывания](/iis/publish/using-web-deploy/introduction-to-web-deploy) или размещении [файла app_offline.htm в развертывании](xref:host-and-deploy/iis/index#locked-deployment-files) вручную приложение может не завершить работу сразу при наличии открытого соединения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-126">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="4a35d-127">Например, подключение websocket может задерживать завершение работы приложения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-127">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="4a35d-128">Архитектура (разрядность) приложения и установленная среда выполнения (x64 или x86) должны соответствовать архитектуре пула приложений.</span><span class="sxs-lookup"><span data-stu-id="4a35d-128">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="4a35d-129">Обнаружены отключения клиентов.</span><span class="sxs-lookup"><span data-stu-id="4a35d-129">Client disconnects are detected.</span></span> <span data-ttu-id="4a35d-130">При отключении клиента происходит отмена токена отмены [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*).</span><span class="sxs-lookup"><span data-stu-id="4a35d-130">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="4a35d-131">В ASP.NET Core 2.2.1 и более ранних версий <xref:System.IO.Directory.GetCurrentDirectory*> возвращает рабочий каталог процесса, запущенного службами IIS, а не каталог приложения (например, *C:\Windows\System32\inetsrv* для *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="4a35d-131">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="4a35d-132">Пример кода, который задает текущий каталог приложения, см. в разделе [Класс CurrentDirectoryHelpers](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="4a35d-132">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="4a35d-133">Вызовите метод `SetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-133">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="4a35d-134">Последующие вызовы <xref:System.IO.Directory.GetCurrentDirectory*> возвращают каталог приложения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-134">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="4a35d-135">При размещении в процессе <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> не вызывается внутри для инициализации пользователя.</span><span class="sxs-lookup"><span data-stu-id="4a35d-135">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="4a35d-136">Таким образом, реализация <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>, используемая для преобразования утверждений после каждой проверки подлинности, не активируется по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="4a35d-136">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="4a35d-137">При преобразовании утверждений с реализацией <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> вызовите <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> для добавления служб проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="4a35d-137">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="4a35d-138">Модель размещения вне процесса</span><span class="sxs-lookup"><span data-stu-id="4a35d-138">Out-of-process hosting model</span></span>

<span data-ttu-id="4a35d-139">Чтобы настроить приложение для внепроцессного размещения, задайте для свойства `<AspNetCoreHostingModel>` значение `OutOfProcess` (для внутрипроцессного размещения указано значение `InProcess`, которое является значением по умолчанию):</span><span class="sxs-lookup"><span data-stu-id="4a35d-139">To configure an app for out-of-process hosting, set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`, which is the default value):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="4a35d-140">Сервер [Kestrel](xref:fundamentals/servers/kestrel) используется вместо HTTP-сервера IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="4a35d-140">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="4a35d-141">Для внепроцессной обработки [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) вызывает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="4a35d-141">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="4a35d-142">Настройка порта и базового пути, которые будет прослушивать сервер при выполнении за модулем ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4a35d-142">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="4a35d-143">Настройка перехвата ошибок запуска на узле.</span><span class="sxs-lookup"><span data-stu-id="4a35d-143">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="4a35d-144">Изменения модели размещения</span><span class="sxs-lookup"><span data-stu-id="4a35d-144">Hosting model changes</span></span>

<span data-ttu-id="4a35d-145">Если параметр `hostingModel` изменяется в файле *web.config* (как описано в разделе [Конфигурация с помощью web.config](#configuration-with-webconfig)), модуль перезапускает рабочий процесс для служб IIS.</span><span class="sxs-lookup"><span data-stu-id="4a35d-145">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="4a35d-146">Для IIS Express модуль не перезапускает рабочий процесс, а запускает нормальное завершение работы текущего процесса IIS Express.</span><span class="sxs-lookup"><span data-stu-id="4a35d-146">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="4a35d-147">Следующий запрос для приложения порождает новый процесс IIS Express.</span><span class="sxs-lookup"><span data-stu-id="4a35d-147">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="4a35d-148">Имя процесса</span><span class="sxs-lookup"><span data-stu-id="4a35d-148">Process name</span></span>

<span data-ttu-id="4a35d-149">`Process.GetCurrentProcess().ProcessName` сообщает `w3wp`/`iisexpress` (внутри процесса) или `dotnet` (вне процесса).</span><span class="sxs-lookup"><span data-stu-id="4a35d-149">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="4a35d-150">Многие собственные модули, такие как проверка подлинности Windows, остаются активными.</span><span class="sxs-lookup"><span data-stu-id="4a35d-150">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="4a35d-151">Дополнительные сведения о модулях IIS, активных с модулем ASP.NET Core, см. в разделе <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="4a35d-151">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="4a35d-152">Дополнительные возможности модуля ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="4a35d-152">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="4a35d-153">Задание переменных среды для рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="4a35d-153">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="4a35d-154">Внесение в журнал выходных данных stdout для хранилища файлов с целью устранения неполадок при запуске.</span><span class="sxs-lookup"><span data-stu-id="4a35d-154">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="4a35d-155">Переадресация токенов проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="4a35d-155">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="4a35d-156">Как установить и использовать модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4a35d-156">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="4a35d-157">Инструкции о том, как установить модуль ASP.NET Core, см. в разделе [Установка пакета размещения .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="4a35d-157">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="4a35d-158">Конфигурация с помощью файла web.config</span><span class="sxs-lookup"><span data-stu-id="4a35d-158">Configuration with web.config</span></span>

<span data-ttu-id="4a35d-159">Модуль ASP.NET Core настроен с помощью раздела `aspNetCore` узла `system.webServer` файла *web.config* на веб-сайте.</span><span class="sxs-lookup"><span data-stu-id="4a35d-159">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="4a35d-160">Следующий файл *web.config* публикуется для [зависимого от платформы развертывания](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) и настраивает модуль ASP.NET Core для обработки запросов к веб-сайту.</span><span class="sxs-lookup"><span data-stu-id="4a35d-160">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="4a35d-161">Следующий файл *web.config* опубликован для [автономного развертывания](/dotnet/articles/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="4a35d-161">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="4a35d-162">Значение `false` свойства <xref:System.Configuration.SectionInformation.InheritInChildApplications*> указывает, что параметры, заданные в элементе [\<расположение>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location), не наследуются приложениями, которые находятся во вложенном каталоге приложения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-162">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="4a35d-163">Когда приложение развернуто в [службе приложений Azure](https://azure.microsoft.com/services/app-service/), путь `stdoutLogFile` задан как `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-163">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="4a35d-164">Путь сохраняет журналы stdout в папке *LogFiles*, расположение которой автоматически создается службой.</span><span class="sxs-lookup"><span data-stu-id="4a35d-164">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="4a35d-165">Сведения о конфигурации дочерних приложений IIS см. здесь: <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="4a35d-165">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="4a35d-166">Атрибуты элемента aspNetCore</span><span class="sxs-lookup"><span data-stu-id="4a35d-166">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="4a35d-167">Атрибут</span><span class="sxs-lookup"><span data-stu-id="4a35d-167">Attribute</span></span> | <span data-ttu-id="4a35d-168">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="4a35d-168">Description</span></span> | <span data-ttu-id="4a35d-169">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="4a35d-169">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="4a35d-170">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-170">Optional string attribute.</span></span></p><p><span data-ttu-id="4a35d-171">Аргументы для исполняемого файла, указанного в атрибуте **processPath**.</span><span class="sxs-lookup"><span data-stu-id="4a35d-171">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="4a35d-172">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-172">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="4a35d-173">Если значение равно true, страница **502.5 — ошибка процесса** подавляется и страница в файле *web.config* с кодом состояния 502 имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="4a35d-173">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="4a35d-174">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-174">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="4a35d-175">Если значение равно true, маркер безопасности отправляется дочернему процессу, прослушивающему порт %ASPNETCORE_PORT% как заголовок "MS-ASPNETCORE-WINAUTHTOKEN" каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="4a35d-175">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="4a35d-176">Этот процесс вызывает CloseHandle по этому маркеру безопасности каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="4a35d-176">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="4a35d-177">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-177">Optional string attribute.</span></span></p><p><span data-ttu-id="4a35d-178">Указывает модель размещения — внутри процесса (`InProcess`) или вне процесса (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="4a35d-178">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `InProcess` |
| `processesPerApplication` | <p><span data-ttu-id="4a35d-179">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-179">Optional integer attribute.</span></span></p><p><span data-ttu-id="4a35d-180">Указывает число экземпляров процесса, заданное в параметре **processPath**, которое может появиться для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-180">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="4a35d-181">&dagger;Для внутрипроцессного размещения существует ограничение: `1`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-181">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="4a35d-182">Параметр `processesPerApplication` не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="4a35d-182">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="4a35d-183">Этот атрибут будет удален в будущем выпуске.</span><span class="sxs-lookup"><span data-stu-id="4a35d-183">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="4a35d-184">По умолчанию: `1`</span><span class="sxs-lookup"><span data-stu-id="4a35d-184">Default: `1`</span></span><br><span data-ttu-id="4a35d-185">Минимум: `1`</span><span class="sxs-lookup"><span data-stu-id="4a35d-185">Min: `1`</span></span><br><span data-ttu-id="4a35d-186">Максимум: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="4a35d-186">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="4a35d-187">Обязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-187">Required string attribute.</span></span></p><p><span data-ttu-id="4a35d-188">Путь к исполняемому файлу, который запускает процесс прослушивания HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="4a35d-188">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="4a35d-189">Поддерживаются относительные пути.</span><span class="sxs-lookup"><span data-stu-id="4a35d-189">Relative paths are supported.</span></span> <span data-ttu-id="4a35d-190">Если путь начинается с `.`, то начало пути считается относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="4a35d-190">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="4a35d-191">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-191">Optional integer attribute.</span></span></p><p><span data-ttu-id="4a35d-192">Указывает количество сбоев за минуту, которыми может завершиться процесс, указанный в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="4a35d-192">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="4a35d-193">Если этот предел превышен, модуль останавливает запуск процесса на оставшуюся часть минуты.</span><span class="sxs-lookup"><span data-stu-id="4a35d-193">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="4a35d-194">Не поддерживается для внутрипроцессного размещения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-194">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="4a35d-195">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="4a35d-195">Default: `10`</span></span><br><span data-ttu-id="4a35d-196">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="4a35d-196">Min: `0`</span></span><br><span data-ttu-id="4a35d-197">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="4a35d-197">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="4a35d-198">Необязательный атрибут timespan.</span><span class="sxs-lookup"><span data-stu-id="4a35d-198">Optional timespan attribute.</span></span></p><p><span data-ttu-id="4a35d-199">Указывает продолжительность, на протяжении которой модуль ASP.NET Core ожидает ответа от процесса, прослушивающего порт %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="4a35d-199">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="4a35d-200">В версиях модуля ASP.NET Core, поставляемых с выпуском ASP.NET Core 2.1 или новее, атрибут `requestTimeout` указывается в часах, минутах и секундах.</span><span class="sxs-lookup"><span data-stu-id="4a35d-200">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="4a35d-201">Не применяется к внутрипроцессному размещению.</span><span class="sxs-lookup"><span data-stu-id="4a35d-201">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="4a35d-202">Для внутрипроцессного размещения модуль ожидает, пока приложение не обработает запрос.</span><span class="sxs-lookup"><span data-stu-id="4a35d-202">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="4a35d-203">Допустимые значения для сегментов минут и секунд в строках находятся в диапазоне 0–59.</span><span class="sxs-lookup"><span data-stu-id="4a35d-203">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="4a35d-204">Значение **60** для минут и секунд приведет к ошибке *500 — внутренняя ошибка сервера*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-204">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="4a35d-205">По умолчанию: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="4a35d-205">Default: `00:02:00`</span></span><br><span data-ttu-id="4a35d-206">Минимум: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="4a35d-206">Min: `00:00:00`</span></span><br><span data-ttu-id="4a35d-207">Максимум: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="4a35d-207">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="4a35d-208">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-208">Optional integer attribute.</span></span></p><p><span data-ttu-id="4a35d-209">Длительность ожидания модуля в секундах, пока произойдет правильное выключение исполняемого файла при обнаружении файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-209">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="4a35d-210">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="4a35d-210">Default: `10`</span></span><br><span data-ttu-id="4a35d-211">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="4a35d-211">Min: `0`</span></span><br><span data-ttu-id="4a35d-212">Максимум: `600`</span><span class="sxs-lookup"><span data-stu-id="4a35d-212">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="4a35d-213">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-213">Optional integer attribute.</span></span></p><p><span data-ttu-id="4a35d-214">Время в секундах, которое модуль ожидает, пока запустится процесс прослушивания порта исполняемого файла.</span><span class="sxs-lookup"><span data-stu-id="4a35d-214">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="4a35d-215">Если этот предел превышен, модуль завершает процесс.</span><span class="sxs-lookup"><span data-stu-id="4a35d-215">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="4a35d-216">Модуль пытается перезапустить процесс при получении нового запроса и будет продолжать пытаться перезапустить процесс для последующих входящих запросов, если не удается запустить приложение определенное в атрибуте **rapidFailsPerMinute** количество раз за последнюю минуту.</span><span class="sxs-lookup"><span data-stu-id="4a35d-216">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="4a35d-217">Значение 0 (ноль) **не** считается бесконечным временем ожидания.</span><span class="sxs-lookup"><span data-stu-id="4a35d-217">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="4a35d-218">По умолчанию: `120`</span><span class="sxs-lookup"><span data-stu-id="4a35d-218">Default: `120`</span></span><br><span data-ttu-id="4a35d-219">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="4a35d-219">Min: `0`</span></span><br><span data-ttu-id="4a35d-220">Максимум: `3600`</span><span class="sxs-lookup"><span data-stu-id="4a35d-220">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="4a35d-221">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-221">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="4a35d-222">Если значение равно true, **stdout** и **stderr** для процесса, указанного в атрибуте **processPath**, перенаправляются к файлу, заданному в атрибуте **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="4a35d-222">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="4a35d-223">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-223">Optional string attribute.</span></span></p><p><span data-ttu-id="4a35d-224">Указывает относительный или абсолютный путь к файлу, для которого регистрируются **stdout** и **stderr** из процесса, указанного в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="4a35d-224">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="4a35d-225">Относительные пути задаются относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="4a35d-225">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="4a35d-226">Любой путь, начинающийся с `.`, относится к корневому каталогу веб-сайта, а все остальные пути рассматриваются как абсолютные пути.</span><span class="sxs-lookup"><span data-stu-id="4a35d-226">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="4a35d-227">Все папки, указанные в пути, создаются модулем при создании файла журнала.</span><span class="sxs-lookup"><span data-stu-id="4a35d-227">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="4a35d-228">С помощью разделителей подчеркивания метка времени, идентификатор процесса и расширение файла ( *.log*) добавляются к последнему сегменту пути журнала **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="4a35d-228">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="4a35d-229">Если в качестве значения задано значение `.\logs\stdout`, например, журнал stdout сохраняется как *stdout_20180205194132_1934.log* в папке *журналов* с датой 5 февраля 2018 г. в 19:41:32 с идентификатором процесса 1934.</span><span class="sxs-lookup"><span data-stu-id="4a35d-229">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="4a35d-230">Настройка переменных среды</span><span class="sxs-lookup"><span data-stu-id="4a35d-230">Setting environment variables</span></span>

<span data-ttu-id="4a35d-231">Переменные среды для процесса можно указать в атрибуте `processPath`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-231">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="4a35d-232">Укажите переменную среды с дочерним элементом `<environmentVariable>` элемента коллекции `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-232">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="4a35d-233">Переменные среды, установленные в этом разделе, имеют приоритет над переменными системной среды.</span><span class="sxs-lookup"><span data-stu-id="4a35d-233">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="4a35d-234">В следующем примере устанавливаются две переменные среды.</span><span class="sxs-lookup"><span data-stu-id="4a35d-234">The following example sets two environment variables.</span></span> <span data-ttu-id="4a35d-235">`ASPNETCORE_ENVIRONMENT` настраивает среду приложения для `Development`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-235">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="4a35d-236">Разработчик может временно задать это значение в файле *web.config*, чтобы принудительно загрузить [Страницу исключений для разработчиков](xref:fundamentals/error-handling) при отладке исключения приложения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-236">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="4a35d-237">`CONFIG_DIR` — пример пользовательской переменной среды, где разработчик написал код, который считывает значение при запуске, чтобы сформировать путь для загрузки файла конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-237">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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

> [!NOTE]
> <span data-ttu-id="4a35d-238">Вместо задания среды напрямую в *web.config* включите свойство `<EnvironmentName>` в профиль публикации ( *.pubxml*) или файл проекта.</span><span class="sxs-lookup"><span data-stu-id="4a35d-238">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="4a35d-239">При этом подходе во время публикации проекта среда задается в файле *web.config*:</span><span class="sxs-lookup"><span data-stu-id="4a35d-239">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="4a35d-240">Установите только переменную среды `ASPNETCORE_ENVIRONMENT` для `Development` на серверах промежуточных процессов и тестирования, которые недоступны для ненадежных сетей, таких как Интернет.</span><span class="sxs-lookup"><span data-stu-id="4a35d-240">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="4a35d-241">App_offline.htm</span><span class="sxs-lookup"><span data-stu-id="4a35d-241">app_offline.htm</span></span>

<span data-ttu-id="4a35d-242">Если в корневом каталоге приложения обнаружен файл с именем *app_offline.htm*, модуль ASP.NET Core пытается корректно закрыть приложение и прекратить обработку входящих запросов.</span><span class="sxs-lookup"><span data-stu-id="4a35d-242">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="4a35d-243">Если приложение по-прежнему выполняется через количество секунд, определенное атрибутом `shutdownTimeLimit`, модуль ASP.NET Core завершает выполняющийся процесс.</span><span class="sxs-lookup"><span data-stu-id="4a35d-243">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="4a35d-244">Хотя файл *app_offline.htm* присутствует, модуль ASP.NET Core отвечает на запросы, отправляя назад содержимое файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-244">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="4a35d-245">Когда *app_offline.htm* файл удаляется, следующий запрос запускает приложение.</span><span class="sxs-lookup"><span data-stu-id="4a35d-245">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="4a35d-246">При использовании модели размещения вне процесса приложение может не завершать работу немедленно при наличии открытого соединения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-246">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="4a35d-247">Например, подключение websocket может задерживать завершение работы приложения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-247">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="4a35d-248">Страница ошибок запуска</span><span class="sxs-lookup"><span data-stu-id="4a35d-248">Start-up error page</span></span>

<span data-ttu-id="4a35d-249">Если при внутри- или внепроцессном размещении происходит сбой запуска приложения, открываются страницы пользовательских сообщений об ошибках.</span><span class="sxs-lookup"><span data-stu-id="4a35d-249">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="4a35d-250">Если модулю ASP.NET Core не удается найти внутри- или внепроцессный обработчик запросов, откроется страница кода состояния *500.0 — ошибка загрузки внутри- или внепроцессного обработчика запросов*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-250">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="4a35d-251">Если в модели размещения внутри процесса модулю ASP.NET Core не удается запустить приложение, откроется страница кода состояния *500.30 — ошибка запуска*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-251">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="4a35d-252">Если в модели размещения вне процесса модулю ASP.NET Core не удается запустить серверный процесс или начинается серверный процесс, но ему не удается прослушать настроенный порт, появится страница кода состояния *502.5 — ошибка процесса*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-252">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="4a35d-253">Чтобы подавить отображение этой странице и вернуться к странице IIS кода состояния 5xx по умолчанию, используйте атрибут `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-253">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="4a35d-254">Дополнительные сведения о настройке пользовательских сообщений об ошибках см. в разделе [Ошибки HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="4a35d-254">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="4a35d-255">Создание и перенаправление журнала</span><span class="sxs-lookup"><span data-stu-id="4a35d-255">Log creation and redirection</span></span>

<span data-ttu-id="4a35d-256">Модуль ASP.NET Core перенаправляет выходные потоки консоли stdout и stderr на диск, если заданы атрибуты `stdoutLogEnabled` и `stdoutLogFile` элемента `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-256">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="4a35d-257">Все папки, указанные в пути `stdoutLogFile`, создаются модулем при создании файла журнала.</span><span class="sxs-lookup"><span data-stu-id="4a35d-257">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="4a35d-258">Пул приложений должен иметь доступ на запись в папку, где записываются журналы (используйте атрибут `IIS AppPool\<app_pool_name>` для предоставления разрешения на запись).</span><span class="sxs-lookup"><span data-stu-id="4a35d-258">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="4a35d-259">Журналы не выполняют циклический сдвиг, пока не произойдет процесс перезапуска или перезагрузки.</span><span class="sxs-lookup"><span data-stu-id="4a35d-259">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="4a35d-260">Администратор несет ответственность за ограничение дискового пространства, которое потребляют журналы.</span><span class="sxs-lookup"><span data-stu-id="4a35d-260">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="4a35d-261">Использование журнала stdout рекомендуется только для устранения неполадок при запуске приложений.</span><span class="sxs-lookup"><span data-stu-id="4a35d-261">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="4a35d-262">Не используйте журнал stdout для ведения общего журнала приложений.</span><span class="sxs-lookup"><span data-stu-id="4a35d-262">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="4a35d-263">Для обычного входа в приложение ASP.NET Core используйте библиотеку ведения журналов, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов.</span><span class="sxs-lookup"><span data-stu-id="4a35d-263">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="4a35d-264">Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="4a35d-264">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="4a35d-265">При создании файла журнала автоматически добавляются отметка времени и расширение файла.</span><span class="sxs-lookup"><span data-stu-id="4a35d-265">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="4a35d-266">Имя файла журнала составляется путем добавления метки времени, идентификатора процесса и расширения файла ( *.log*) к последнему сегменту атрибута пути `stdoutLogFile` (обычно *stdout*) с символами подчеркивания в качестве разделителей.</span><span class="sxs-lookup"><span data-stu-id="4a35d-266">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="4a35d-267">Если атрибут пути `stdoutLogFile` заканчивается элементом *stdout*, журнал приложения с идентификатором 1934, созданный 5 февраля 2018 г. в 19:42:32, будет иметь имя *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-267">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="4a35d-268">Если `stdoutLogEnabled` имеет значение false, возникающие при запуске приложения ошибки записываются и передаются в журнал событий (макс. 30 КБ).</span><span class="sxs-lookup"><span data-stu-id="4a35d-268">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="4a35d-269">После запуска все дополнительные журналы удаляются.</span><span class="sxs-lookup"><span data-stu-id="4a35d-269">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="4a35d-270">В следующем примере элемент `aspNetCore` настраивает ведение журнала stdout для приложения, размещенного в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="4a35d-270">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="4a35d-271">Для локального ведения журнала допустим локальный или общий сетевой путь.</span><span class="sxs-lookup"><span data-stu-id="4a35d-271">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="4a35d-272">Убедитесь, что идентификатор пользователя AppPool имеет разрешение на запись по указанному пути.</span><span class="sxs-lookup"><span data-stu-id="4a35d-272">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="4a35d-273">Расширенные журналы диагностики</span><span class="sxs-lookup"><span data-stu-id="4a35d-273">Enhanced diagnostic logs</span></span>

<span data-ttu-id="4a35d-274">Модуль ASP.NET Core можно настроить. Он позволяет работать с расширенными журналами диагностики.</span><span class="sxs-lookup"><span data-stu-id="4a35d-274">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="4a35d-275">Добавьте элемент `<handlerSettings>` в элемент `<aspNetCore>` в файле *web.config*. Задайте параметру `debugLevel` значение `TRACE`, чтобы обеспечить высокую точность диагностических сведений:</span><span class="sxs-lookup"><span data-stu-id="4a35d-275">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="debugFile" value=".\logs\aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="4a35d-276">Все папки, указанные в пути (*logs* в приведенном выше примере), создаются модулем при создании файла журнала.</span><span class="sxs-lookup"><span data-stu-id="4a35d-276">Any folders in the path (*logs* in the preceding example) are created by the module when the log file is created.</span></span> <span data-ttu-id="4a35d-277">Пул приложений должен иметь доступ на запись в папку, где записываются журналы (используйте атрибут `IIS AppPool\<app_pool_name>` для предоставления разрешения на запись).</span><span class="sxs-lookup"><span data-stu-id="4a35d-277">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="4a35d-278">Значения уровня отладки (`debugLevel`) могут включать уровень и расположение.</span><span class="sxs-lookup"><span data-stu-id="4a35d-278">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="4a35d-279">Уровни (в порядке возрастания степени детализации):</span><span class="sxs-lookup"><span data-stu-id="4a35d-279">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="4a35d-280">ОШИБКА</span><span class="sxs-lookup"><span data-stu-id="4a35d-280">ERROR</span></span>
* <span data-ttu-id="4a35d-281">ПРЕДУПРЕЖДЕНИЕ</span><span class="sxs-lookup"><span data-stu-id="4a35d-281">WARNING</span></span>
* <span data-ttu-id="4a35d-282">ИНФОРМАЦИЯ</span><span class="sxs-lookup"><span data-stu-id="4a35d-282">INFO</span></span>
* <span data-ttu-id="4a35d-283">TRACE</span><span class="sxs-lookup"><span data-stu-id="4a35d-283">TRACE</span></span>

<span data-ttu-id="4a35d-284">Расположения (допускаются несколько расположений):</span><span class="sxs-lookup"><span data-stu-id="4a35d-284">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="4a35d-285">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="4a35d-285">CONSOLE</span></span>
* <span data-ttu-id="4a35d-286">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="4a35d-286">EVENTLOG</span></span>
* <span data-ttu-id="4a35d-287">FILE</span><span class="sxs-lookup"><span data-stu-id="4a35d-287">FILE</span></span>

<span data-ttu-id="4a35d-288">Параметры обработчика могут быть указаны с помощью переменных среды:</span><span class="sxs-lookup"><span data-stu-id="4a35d-288">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="4a35d-289">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Путь к файлу журнала отладки.</span><span class="sxs-lookup"><span data-stu-id="4a35d-289">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="4a35d-290">(По умолчанию: *aspnetcore debug.log*)</span><span class="sxs-lookup"><span data-stu-id="4a35d-290">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="4a35d-291">`ASPNETCORE_MODULE_DEBUG` &ndash; Параметр уровня отладки.</span><span class="sxs-lookup"><span data-stu-id="4a35d-291">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="4a35d-292">**Не** оставляйте ведение журнала отладки включенным в развертывании дольше того времени, которое требуется для устранения проблемы.</span><span class="sxs-lookup"><span data-stu-id="4a35d-292">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="4a35d-293">Размер журнала не ограничен.</span><span class="sxs-lookup"><span data-stu-id="4a35d-293">The size of the log isn't limited.</span></span> <span data-ttu-id="4a35d-294">Если оставить журнал отладки включенным, он может исчерпать все доступное место на диске и привести к сбою сервера или службы приложений.</span><span class="sxs-lookup"><span data-stu-id="4a35d-294">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="4a35d-295">См. пример элемента `aspNetCore` в файле *web.config* в разделе [Конфигурация с помощью файла web.config](#configuration-with-webconfig).</span><span class="sxs-lookup"><span data-stu-id="4a35d-295">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="modify-the-stack-size"></a><span data-ttu-id="4a35d-296">Изменение размера стека</span><span class="sxs-lookup"><span data-stu-id="4a35d-296">Modify the stack size</span></span>

<span data-ttu-id="4a35d-297">*Применяется только при использовании модели внутрипроцессного размещения.*</span><span class="sxs-lookup"><span data-stu-id="4a35d-297">*Only applies when using the in-process hosting model.*</span></span>

<span data-ttu-id="4a35d-298">Настройте размер управляемого стека, задав для параметра `stackSize` значение в байтах.</span><span class="sxs-lookup"><span data-stu-id="4a35d-298">Configure the managed stack size using the `stackSize` setting in bytes.</span></span> <span data-ttu-id="4a35d-299">Размер по умолчанию — `1048576` байт (1 МБ).</span><span class="sxs-lookup"><span data-stu-id="4a35d-299">The default size is `1048576` bytes (1 MB).</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="stackSize" value="2097152" />
  </handlerSettings>
</aspNetCore>
```

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="4a35d-300">Конфигурация прокси-сервера использует протокол HTTP и токен связывания</span><span class="sxs-lookup"><span data-stu-id="4a35d-300">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="4a35d-301">*Применяется только к размещению вне процесса.*</span><span class="sxs-lookup"><span data-stu-id="4a35d-301">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="4a35d-302">Прокси-сервер, созданный между модулем ASP.NET Core и Kestrel, использует протокол HTTP.</span><span class="sxs-lookup"><span data-stu-id="4a35d-302">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="4a35d-303">Отсутствует риск перехвата трафика между модулем и Kestrel из расположения на сервере.</span><span class="sxs-lookup"><span data-stu-id="4a35d-303">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="4a35d-304">Токен связывания гарантирует, что полученные Kestrel запросы были переданы службами IIS, а не из какого-либо другого источника.</span><span class="sxs-lookup"><span data-stu-id="4a35d-304">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="4a35d-305">Этот токен создается и задается модулем в переменную среды (`ASPNETCORE_TOKEN`).</span><span class="sxs-lookup"><span data-stu-id="4a35d-305">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="4a35d-306">Он также задается в заголовке (`MS-ASPNETCORE-TOKEN`) каждого запроса, переданного через прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="4a35d-306">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="4a35d-307">ПО промежуточного слоя IIS проверяет каждый получаемый запрос, чтобы убедиться, что заголовок с токеном связывания соответствует значению переменной среды.</span><span class="sxs-lookup"><span data-stu-id="4a35d-307">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="4a35d-308">Если значения токена не совпадают, запрос заносится в журнал и отклоняется.</span><span class="sxs-lookup"><span data-stu-id="4a35d-308">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="4a35d-309">Переменная среды с токеном связывания и трафик между модулем и Kestrel недоступны из расположения на сервере.</span><span class="sxs-lookup"><span data-stu-id="4a35d-309">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="4a35d-310">Не зная значение токена связывания, злоумышленник не может отправлять запросы, обходящие проверку в ПО промежуточного слоя IIS.</span><span class="sxs-lookup"><span data-stu-id="4a35d-310">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="4a35d-311">Модуль ASP.NET Core с общей конфигурацией IIS</span><span class="sxs-lookup"><span data-stu-id="4a35d-311">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="4a35d-312">Программа установки модуля ASP.NET Core запускается с правами учетной записи **TrustedInstaller**.</span><span class="sxs-lookup"><span data-stu-id="4a35d-312">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="4a35d-313">Поскольку учетная запись локальной системы не имеет разрешения на изменение пути к общей папке, используемого общей конфигурацией IIS, установщик получает ошибку отказа в доступе при попытке настроить параметры модуля в файле *applicationHost.config* общей папки.</span><span class="sxs-lookup"><span data-stu-id="4a35d-313">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="4a35d-314">При использовании общей конфигурации IIS на том же компьютере, где установлены службы IIS, запустите установщик пакета размещения ASP.NET Core с параметром `OPT_NO_SHARED_CONFIG_CHECK` со значением `1`:</span><span class="sxs-lookup"><span data-stu-id="4a35d-314">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="4a35d-315">Если путь к общей конфигурации не на том же компьютере, где установлены службы IIS, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="4a35d-315">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="4a35d-316">Отключите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="4a35d-316">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="4a35d-317">Запустите установщик.</span><span class="sxs-lookup"><span data-stu-id="4a35d-317">Run the installer.</span></span>
1. <span data-ttu-id="4a35d-318">Экспортируйте обновленный файл *applicationHost.config* в общую папку.</span><span class="sxs-lookup"><span data-stu-id="4a35d-318">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="4a35d-319">Повторно включите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="4a35d-319">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="4a35d-320">Версия модуля и журналы установщика хостинга Bundle</span><span class="sxs-lookup"><span data-stu-id="4a35d-320">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="4a35d-321">Чтобы определить версию установщика модуля ASP.NET Core, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="4a35d-321">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="4a35d-322">В системе размещения перейдите к папке *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-322">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="4a35d-323">Найдите файл *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-323">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="4a35d-324">Щелкните правой кнопкой мыши файл и выберите **Свойства** из контекстного меню.</span><span class="sxs-lookup"><span data-stu-id="4a35d-324">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="4a35d-325">Выберите вкладку **Сведения**. **Версия файла** и **Версия продукта** дают представление об установленной версии модуля.</span><span class="sxs-lookup"><span data-stu-id="4a35d-325">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="4a35d-326">Журналы установщика хостинга Bundle для модуля находятся в папке *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. Этот файл имеет имя *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-326">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="4a35d-327">Модуль, схемы и расположение файлов конфигурации</span><span class="sxs-lookup"><span data-stu-id="4a35d-327">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="4a35d-328">Module</span><span class="sxs-lookup"><span data-stu-id="4a35d-328">Module</span></span>

<span data-ttu-id="4a35d-329">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="4a35d-329">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="4a35d-330">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4a35d-330">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="4a35d-331">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4a35d-331">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="4a35d-332">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="4a35d-332">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="4a35d-333">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="4a35d-333">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="4a35d-334">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="4a35d-334">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="4a35d-335">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4a35d-335">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="4a35d-336">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4a35d-336">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="4a35d-337">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="4a35d-337">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="4a35d-338">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="4a35d-338">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="4a35d-339">Схема</span><span class="sxs-lookup"><span data-stu-id="4a35d-339">Schema</span></span>

<span data-ttu-id="4a35d-340">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="4a35d-340">**IIS**</span></span>

* <span data-ttu-id="4a35d-341">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="4a35d-341">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="4a35d-342">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="4a35d-342">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="4a35d-343">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="4a35d-343">**IIS Express**</span></span>

* <span data-ttu-id="4a35d-344">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="4a35d-344">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="4a35d-345">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="4a35d-345">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="4a35d-346">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="4a35d-346">Configuration</span></span>

<span data-ttu-id="4a35d-347">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="4a35d-347">**IIS**</span></span>

* <span data-ttu-id="4a35d-348">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="4a35d-348">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="4a35d-349">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="4a35d-349">**IIS Express**</span></span>

* <span data-ttu-id="4a35d-350">Visual Studio: {КОРЕНЬ ПРИЛОЖЕНИЯ}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="4a35d-350">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="4a35d-351">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="4a35d-351">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="4a35d-352">Файлы можно найти путем поиска *aspnetcore* в файле *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-352">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="4a35d-353">Модуль ASP.NET Core имеет собственный модуль IIS, который подключается к конвейеру IIS для выполнения следующих задач:</span><span class="sxs-lookup"><span data-stu-id="4a35d-353">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="4a35d-354">Размещение приложения ASP.NET Core внутри рабочего процесса IIS (`w3wp.exe`). Это так называемая [модель внутрипроцессного размещения](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="4a35d-354">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="4a35d-355">Переадресация веб-запросов к серверной части приложения ASP.NET Core на [сервере Kestrel](xref:fundamentals/servers/kestrel). Это [модель размещения вне процесса](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="4a35d-355">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="4a35d-356">Поддерживаемые версии Windows:</span><span class="sxs-lookup"><span data-stu-id="4a35d-356">Supported Windows versions:</span></span>

* <span data-ttu-id="4a35d-357">Windows 7 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="4a35d-357">Windows 7 or later</span></span>
* <span data-ttu-id="4a35d-358">Windows Server 2008 R2 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="4a35d-358">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="4a35d-359">При размещении в процессе модуль использует реализацию внутрипроцессного сервера для IIS — HTTP-сервер IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="4a35d-359">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="4a35d-360">При размещении вне процесса модуль работает только с Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4a35d-360">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="4a35d-361">Модуль не работает с [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="4a35d-361">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="4a35d-362">Модели размещения</span><span class="sxs-lookup"><span data-stu-id="4a35d-362">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="4a35d-363">Модель внутрипроцессного размещения</span><span class="sxs-lookup"><span data-stu-id="4a35d-363">In-process hosting model</span></span>

<span data-ttu-id="4a35d-364">Чтобы настроить приложение для внутрипроцессного размещения, добавьте свойство `<AspNetCoreHostingModel>` к файлу проекта приложения со значением `InProcess` (размещение вне процесса имеет значение `OutOfProcess`):</span><span class="sxs-lookup"><span data-stu-id="4a35d-364">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="4a35d-365">Модель внутрипроцессного размещения не поддерживается для приложений ASP.NET Core, предназначенных для .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4a35d-365">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="4a35d-366">Если свойство `<AspNetCoreHostingModel>` отсутствует в файле, значение по умолчанию — `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-366">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="4a35d-367">При внутрипроцессном размещении применимы следующие характеристики:</span><span class="sxs-lookup"><span data-stu-id="4a35d-367">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="4a35d-368">Вместо сервера [Kestrel](xref:fundamentals/servers/kestrel) используется HTTP-сервер IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="4a35d-368">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="4a35d-369">Для внутрипроцессной обработки [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) вызывает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="4a35d-369">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="4a35d-370">Регистрация `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-370">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="4a35d-371">Настройка порта и базового пути, которые будет прослушивать сервер при выполнении за модулем ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4a35d-371">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="4a35d-372">Настройка перехвата ошибок запуска на узле.</span><span class="sxs-lookup"><span data-stu-id="4a35d-372">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="4a35d-373">[Атрибут requestTimeout](#attributes-of-the-aspnetcore-element) не применяется к внутрипроцессному размещению.</span><span class="sxs-lookup"><span data-stu-id="4a35d-373">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="4a35d-374">Совместное использование пула приложений среди приложений не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="4a35d-374">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="4a35d-375">Используйте один пул приложений для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-375">Use one app pool per app.</span></span>

* <span data-ttu-id="4a35d-376">При использовании [веб-развертывания](/iis/publish/using-web-deploy/introduction-to-web-deploy) или размещении [файла app_offline.htm в развертывании](xref:host-and-deploy/iis/index#locked-deployment-files) вручную приложение может не завершить работу сразу при наличии открытого соединения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-376">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="4a35d-377">Например, подключение websocket может задерживать завершение работы приложения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-377">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="4a35d-378">Архитектура (разрядность) приложения и установленная среда выполнения (x64 или x86) должны соответствовать архитектуре пула приложений.</span><span class="sxs-lookup"><span data-stu-id="4a35d-378">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="4a35d-379">Обнаружены отключения клиентов.</span><span class="sxs-lookup"><span data-stu-id="4a35d-379">Client disconnects are detected.</span></span> <span data-ttu-id="4a35d-380">При отключении клиента происходит отмена токена отмены [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*).</span><span class="sxs-lookup"><span data-stu-id="4a35d-380">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="4a35d-381">В ASP.NET Core 2.2.1 и более ранних версий <xref:System.IO.Directory.GetCurrentDirectory*> возвращает рабочий каталог процесса, запущенного службами IIS, а не каталог приложения (например, *C:\Windows\System32\inetsrv* для *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="4a35d-381">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="4a35d-382">Пример кода, который задает текущий каталог приложения, см. в разделе [Класс CurrentDirectoryHelpers](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="4a35d-382">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="4a35d-383">Вызовите метод `SetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-383">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="4a35d-384">Последующие вызовы <xref:System.IO.Directory.GetCurrentDirectory*> возвращают каталог приложения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-384">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="4a35d-385">При размещении в процессе <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> не вызывается внутри для инициализации пользователя.</span><span class="sxs-lookup"><span data-stu-id="4a35d-385">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="4a35d-386">Таким образом, реализация <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>, используемая для преобразования утверждений после каждой проверки подлинности, не активируется по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="4a35d-386">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="4a35d-387">При преобразовании утверждений с реализацией <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> вызовите <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> для добавления служб проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="4a35d-387">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="4a35d-388">Модель размещения вне процесса</span><span class="sxs-lookup"><span data-stu-id="4a35d-388">Out-of-process hosting model</span></span>

<span data-ttu-id="4a35d-389">Чтобы настроить приложение для размещения вне процесса, используйте один из следующих подходов в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="4a35d-389">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="4a35d-390">Не указывайте свойство `<AspNetCoreHostingModel>`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-390">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="4a35d-391">Если свойство `<AspNetCoreHostingModel>` отсутствует в файле, значение по умолчанию — `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-391">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="4a35d-392">Установите для свойства `<AspNetCoreHostingModel>` значение `OutOfProcess` (внутрипроцессное размещение имеет значение `InProcess`):</span><span class="sxs-lookup"><span data-stu-id="4a35d-392">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="4a35d-393">Сервер [Kestrel](xref:fundamentals/servers/kestrel) используется вместо HTTP-сервера IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="4a35d-393">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="4a35d-394">Для внепроцессной обработки [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) вызывает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="4a35d-394">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="4a35d-395">Настройка порта и базового пути, которые будет прослушивать сервер при выполнении за модулем ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4a35d-395">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="4a35d-396">Настройка перехвата ошибок запуска на узле.</span><span class="sxs-lookup"><span data-stu-id="4a35d-396">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="4a35d-397">Изменения модели размещения</span><span class="sxs-lookup"><span data-stu-id="4a35d-397">Hosting model changes</span></span>

<span data-ttu-id="4a35d-398">Если параметр `hostingModel` изменяется в файле *web.config* (как описано в разделе [Конфигурация с помощью web.config](#configuration-with-webconfig)), модуль перезапускает рабочий процесс для служб IIS.</span><span class="sxs-lookup"><span data-stu-id="4a35d-398">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="4a35d-399">Для IIS Express модуль не перезапускает рабочий процесс, а запускает нормальное завершение работы текущего процесса IIS Express.</span><span class="sxs-lookup"><span data-stu-id="4a35d-399">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="4a35d-400">Следующий запрос для приложения порождает новый процесс IIS Express.</span><span class="sxs-lookup"><span data-stu-id="4a35d-400">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="4a35d-401">Имя процесса</span><span class="sxs-lookup"><span data-stu-id="4a35d-401">Process name</span></span>

<span data-ttu-id="4a35d-402">`Process.GetCurrentProcess().ProcessName` сообщает `w3wp`/`iisexpress` (внутри процесса) или `dotnet` (вне процесса).</span><span class="sxs-lookup"><span data-stu-id="4a35d-402">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="4a35d-403">Многие собственные модули, такие как проверка подлинности Windows, остаются активными.</span><span class="sxs-lookup"><span data-stu-id="4a35d-403">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="4a35d-404">Дополнительные сведения о модулях IIS, активных с модулем ASP.NET Core, см. в разделе <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="4a35d-404">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="4a35d-405">Дополнительные возможности модуля ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="4a35d-405">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="4a35d-406">Задание переменных среды для рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="4a35d-406">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="4a35d-407">Внесение в журнал выходных данных stdout для хранилища файлов с целью устранения неполадок при запуске.</span><span class="sxs-lookup"><span data-stu-id="4a35d-407">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="4a35d-408">Переадресация токенов проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="4a35d-408">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="4a35d-409">Как установить и использовать модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4a35d-409">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="4a35d-410">Инструкции о том, как установить модуль ASP.NET Core, см. в разделе [Установка пакета размещения .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="4a35d-410">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="4a35d-411">Конфигурация с помощью файла web.config</span><span class="sxs-lookup"><span data-stu-id="4a35d-411">Configuration with web.config</span></span>

<span data-ttu-id="4a35d-412">Модуль ASP.NET Core настроен с помощью раздела `aspNetCore` узла `system.webServer` файла *web.config* на веб-сайте.</span><span class="sxs-lookup"><span data-stu-id="4a35d-412">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="4a35d-413">Следующий файл *web.config* публикуется для [зависимого от платформы развертывания](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) и настраивает модуль ASP.NET Core для обработки запросов к веб-сайту.</span><span class="sxs-lookup"><span data-stu-id="4a35d-413">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="4a35d-414">Следующий файл *web.config* опубликован для [автономного развертывания](/dotnet/articles/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="4a35d-414">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="4a35d-415">Значение `false` свойства <xref:System.Configuration.SectionInformation.InheritInChildApplications*> указывает, что параметры, заданные в элементе [\<расположение>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location), не наследуются приложениями, которые находятся во вложенном каталоге приложения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-415">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="4a35d-416">Когда приложение развернуто в [службе приложений Azure](https://azure.microsoft.com/services/app-service/), путь `stdoutLogFile` задан как `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-416">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="4a35d-417">Путь сохраняет журналы stdout в папке *LogFiles*, расположение которой автоматически создается службой.</span><span class="sxs-lookup"><span data-stu-id="4a35d-417">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="4a35d-418">Сведения о конфигурации дочерних приложений IIS см. здесь: <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="4a35d-418">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="4a35d-419">Атрибуты элемента aspNetCore</span><span class="sxs-lookup"><span data-stu-id="4a35d-419">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="4a35d-420">Атрибут</span><span class="sxs-lookup"><span data-stu-id="4a35d-420">Attribute</span></span> | <span data-ttu-id="4a35d-421">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="4a35d-421">Description</span></span> | <span data-ttu-id="4a35d-422">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="4a35d-422">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="4a35d-423">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-423">Optional string attribute.</span></span></p><p><span data-ttu-id="4a35d-424">Аргументы для исполняемого файла, указанного в атрибуте **processPath**.</span><span class="sxs-lookup"><span data-stu-id="4a35d-424">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="4a35d-425">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-425">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="4a35d-426">Если значение равно true, страница **502.5 — ошибка процесса** подавляется и страница в файле *web.config* с кодом состояния 502 имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="4a35d-426">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="4a35d-427">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-427">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="4a35d-428">Если значение равно true, маркер безопасности отправляется дочернему процессу, прослушивающему порт %ASPNETCORE_PORT% как заголовок "MS-ASPNETCORE-WINAUTHTOKEN" каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="4a35d-428">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="4a35d-429">Этот процесс вызывает CloseHandle по этому маркеру безопасности каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="4a35d-429">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="4a35d-430">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-430">Optional string attribute.</span></span></p><p><span data-ttu-id="4a35d-431">Указывает модель размещения — внутри процесса (`InProcess`) или вне процесса (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="4a35d-431">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="4a35d-432">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-432">Optional integer attribute.</span></span></p><p><span data-ttu-id="4a35d-433">Указывает число экземпляров процесса, заданное в параметре **processPath**, которое может появиться для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-433">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="4a35d-434">&dagger;Для внутрипроцессного размещения существует ограничение: `1`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-434">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="4a35d-435">Параметр `processesPerApplication` не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="4a35d-435">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="4a35d-436">Этот атрибут будет удален в будущем выпуске.</span><span class="sxs-lookup"><span data-stu-id="4a35d-436">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="4a35d-437">По умолчанию: `1`</span><span class="sxs-lookup"><span data-stu-id="4a35d-437">Default: `1`</span></span><br><span data-ttu-id="4a35d-438">Минимум: `1`</span><span class="sxs-lookup"><span data-stu-id="4a35d-438">Min: `1`</span></span><br><span data-ttu-id="4a35d-439">Максимум: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="4a35d-439">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="4a35d-440">Обязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-440">Required string attribute.</span></span></p><p><span data-ttu-id="4a35d-441">Путь к исполняемому файлу, который запускает процесс прослушивания HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="4a35d-441">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="4a35d-442">Поддерживаются относительные пути.</span><span class="sxs-lookup"><span data-stu-id="4a35d-442">Relative paths are supported.</span></span> <span data-ttu-id="4a35d-443">Если путь начинается с `.`, то начало пути считается относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="4a35d-443">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="4a35d-444">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-444">Optional integer attribute.</span></span></p><p><span data-ttu-id="4a35d-445">Указывает количество сбоев за минуту, которыми может завершиться процесс, указанный в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="4a35d-445">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="4a35d-446">Если этот предел превышен, модуль останавливает запуск процесса на оставшуюся часть минуты.</span><span class="sxs-lookup"><span data-stu-id="4a35d-446">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="4a35d-447">Не поддерживается для внутрипроцессного размещения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-447">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="4a35d-448">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="4a35d-448">Default: `10`</span></span><br><span data-ttu-id="4a35d-449">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="4a35d-449">Min: `0`</span></span><br><span data-ttu-id="4a35d-450">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="4a35d-450">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="4a35d-451">Необязательный атрибут timespan.</span><span class="sxs-lookup"><span data-stu-id="4a35d-451">Optional timespan attribute.</span></span></p><p><span data-ttu-id="4a35d-452">Указывает продолжительность, на протяжении которой модуль ASP.NET Core ожидает ответа от процесса, прослушивающего порт %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="4a35d-452">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="4a35d-453">В версиях модуля ASP.NET Core, поставляемых с выпуском ASP.NET Core 2.1 или новее, атрибут `requestTimeout` указывается в часах, минутах и секундах.</span><span class="sxs-lookup"><span data-stu-id="4a35d-453">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="4a35d-454">Не применяется к внутрипроцессному размещению.</span><span class="sxs-lookup"><span data-stu-id="4a35d-454">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="4a35d-455">Для внутрипроцессного размещения модуль ожидает, пока приложение не обработает запрос.</span><span class="sxs-lookup"><span data-stu-id="4a35d-455">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="4a35d-456">Допустимые значения для сегментов минут и секунд в строках находятся в диапазоне 0–59.</span><span class="sxs-lookup"><span data-stu-id="4a35d-456">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="4a35d-457">Значение **60** для минут и секунд приведет к ошибке *500 — внутренняя ошибка сервера*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-457">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="4a35d-458">По умолчанию: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="4a35d-458">Default: `00:02:00`</span></span><br><span data-ttu-id="4a35d-459">Минимум: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="4a35d-459">Min: `00:00:00`</span></span><br><span data-ttu-id="4a35d-460">Максимум: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="4a35d-460">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="4a35d-461">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-461">Optional integer attribute.</span></span></p><p><span data-ttu-id="4a35d-462">Длительность ожидания модуля в секундах, пока произойдет правильное выключение исполняемого файла при обнаружении файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-462">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="4a35d-463">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="4a35d-463">Default: `10`</span></span><br><span data-ttu-id="4a35d-464">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="4a35d-464">Min: `0`</span></span><br><span data-ttu-id="4a35d-465">Максимум: `600`</span><span class="sxs-lookup"><span data-stu-id="4a35d-465">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="4a35d-466">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-466">Optional integer attribute.</span></span></p><p><span data-ttu-id="4a35d-467">Время в секундах, которое модуль ожидает, пока запустится процесс прослушивания порта исполняемого файла.</span><span class="sxs-lookup"><span data-stu-id="4a35d-467">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="4a35d-468">Если этот предел превышен, модуль завершает процесс.</span><span class="sxs-lookup"><span data-stu-id="4a35d-468">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="4a35d-469">Модуль пытается перезапустить процесс при получении нового запроса и будет продолжать пытаться перезапустить процесс для последующих входящих запросов, если не удается запустить приложение определенное в атрибуте **rapidFailsPerMinute** количество раз за последнюю минуту.</span><span class="sxs-lookup"><span data-stu-id="4a35d-469">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="4a35d-470">Значение 0 (ноль) **не** считается бесконечным временем ожидания.</span><span class="sxs-lookup"><span data-stu-id="4a35d-470">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="4a35d-471">По умолчанию: `120`</span><span class="sxs-lookup"><span data-stu-id="4a35d-471">Default: `120`</span></span><br><span data-ttu-id="4a35d-472">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="4a35d-472">Min: `0`</span></span><br><span data-ttu-id="4a35d-473">Максимум: `3600`</span><span class="sxs-lookup"><span data-stu-id="4a35d-473">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="4a35d-474">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-474">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="4a35d-475">Если значение равно true, **stdout** и **stderr** для процесса, указанного в атрибуте **processPath**, перенаправляются к файлу, заданному в атрибуте **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="4a35d-475">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="4a35d-476">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-476">Optional string attribute.</span></span></p><p><span data-ttu-id="4a35d-477">Указывает относительный или абсолютный путь к файлу, для которого регистрируются **stdout** и **stderr** из процесса, указанного в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="4a35d-477">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="4a35d-478">Относительные пути задаются относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="4a35d-478">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="4a35d-479">Любой путь, начинающийся с `.`, относится к корневому каталогу веб-сайта, а все остальные пути рассматриваются как абсолютные пути.</span><span class="sxs-lookup"><span data-stu-id="4a35d-479">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="4a35d-480">Все папки, указанные в пути, создаются модулем при создании файла журнала.</span><span class="sxs-lookup"><span data-stu-id="4a35d-480">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="4a35d-481">С помощью разделителей подчеркивания метка времени, идентификатор процесса и расширение файла ( *.log*) добавляются к последнему сегменту пути журнала **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="4a35d-481">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="4a35d-482">Если в качестве значения задано значение `.\logs\stdout`, например, журнал stdout сохраняется как *stdout_20180205194132_1934.log* в папке *журналов* с датой 5 февраля 2018 г. в 19:41:32 с идентификатором процесса 1934.</span><span class="sxs-lookup"><span data-stu-id="4a35d-482">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="4a35d-483">Настройка переменных среды</span><span class="sxs-lookup"><span data-stu-id="4a35d-483">Setting environment variables</span></span>

<span data-ttu-id="4a35d-484">Переменные среды для процесса можно указать в атрибуте `processPath`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-484">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="4a35d-485">Укажите переменную среды с дочерним элементом `<environmentVariable>` элемента коллекции `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-485">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="4a35d-486">Переменные среды, установленные в этом разделе, имеют приоритет над переменными системной среды.</span><span class="sxs-lookup"><span data-stu-id="4a35d-486">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="4a35d-487">В следующем примере устанавливаются две переменные среды.</span><span class="sxs-lookup"><span data-stu-id="4a35d-487">The following example sets two environment variables.</span></span> <span data-ttu-id="4a35d-488">`ASPNETCORE_ENVIRONMENT` настраивает среду приложения для `Development`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-488">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="4a35d-489">Разработчик может временно задать это значение в файле *web.config*, чтобы принудительно загрузить [Страницу исключений для разработчиков](xref:fundamentals/error-handling) при отладке исключения приложения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-489">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="4a35d-490">`CONFIG_DIR` — пример пользовательской переменной среды, где разработчик написал код, который считывает значение при запуске, чтобы сформировать путь для загрузки файла конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-490">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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

> [!NOTE]
> <span data-ttu-id="4a35d-491">Вместо задания среды напрямую в *web.config* включите свойство `<EnvironmentName>` в профиль публикации ( *.pubxml*) или файл проекта.</span><span class="sxs-lookup"><span data-stu-id="4a35d-491">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="4a35d-492">При этом подходе во время публикации проекта среда задается в файле *web.config*:</span><span class="sxs-lookup"><span data-stu-id="4a35d-492">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="4a35d-493">Установите только переменную среды `ASPNETCORE_ENVIRONMENT` для `Development` на серверах промежуточных процессов и тестирования, которые недоступны для ненадежных сетей, таких как Интернет.</span><span class="sxs-lookup"><span data-stu-id="4a35d-493">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="4a35d-494">App_offline.htm</span><span class="sxs-lookup"><span data-stu-id="4a35d-494">app_offline.htm</span></span>

<span data-ttu-id="4a35d-495">Если в корневом каталоге приложения обнаружен файл с именем *app_offline.htm*, модуль ASP.NET Core пытается корректно закрыть приложение и прекратить обработку входящих запросов.</span><span class="sxs-lookup"><span data-stu-id="4a35d-495">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="4a35d-496">Если приложение по-прежнему выполняется через количество секунд, определенное атрибутом `shutdownTimeLimit`, модуль ASP.NET Core завершает выполняющийся процесс.</span><span class="sxs-lookup"><span data-stu-id="4a35d-496">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="4a35d-497">Хотя файл *app_offline.htm* присутствует, модуль ASP.NET Core отвечает на запросы, отправляя назад содержимое файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-497">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="4a35d-498">Когда *app_offline.htm* файл удаляется, следующий запрос запускает приложение.</span><span class="sxs-lookup"><span data-stu-id="4a35d-498">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="4a35d-499">При использовании модели размещения вне процесса приложение может не завершать работу немедленно при наличии открытого соединения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-499">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="4a35d-500">Например, подключение websocket может задерживать завершение работы приложения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-500">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="4a35d-501">Страница ошибок запуска</span><span class="sxs-lookup"><span data-stu-id="4a35d-501">Start-up error page</span></span>

<span data-ttu-id="4a35d-502">Если при внутри- или внепроцессном размещении происходит сбой запуска приложения, открываются страницы пользовательских сообщений об ошибках.</span><span class="sxs-lookup"><span data-stu-id="4a35d-502">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="4a35d-503">Если модулю ASP.NET Core не удается найти внутри- или внепроцессный обработчик запросов, откроется страница кода состояния *500.0 — ошибка загрузки внутри- или внепроцессного обработчика запросов*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-503">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="4a35d-504">Если в модели размещения внутри процесса модулю ASP.NET Core не удается запустить приложение, откроется страница кода состояния *500.30 — ошибка запуска*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-504">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="4a35d-505">Если в модели размещения вне процесса модулю ASP.NET Core не удается запустить серверный процесс или начинается серверный процесс, но ему не удается прослушать настроенный порт, появится страница кода состояния *502.5 — ошибка процесса*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-505">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="4a35d-506">Чтобы подавить отображение этой странице и вернуться к странице IIS кода состояния 5xx по умолчанию, используйте атрибут `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-506">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="4a35d-507">Дополнительные сведения о настройке пользовательских сообщений об ошибках см. в разделе [Ошибки HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="4a35d-507">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="4a35d-508">Создание и перенаправление журнала</span><span class="sxs-lookup"><span data-stu-id="4a35d-508">Log creation and redirection</span></span>

<span data-ttu-id="4a35d-509">Модуль ASP.NET Core перенаправляет выходные потоки консоли stdout и stderr на диск, если заданы атрибуты `stdoutLogEnabled` и `stdoutLogFile` элемента `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-509">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="4a35d-510">Все папки, указанные в пути `stdoutLogFile`, создаются модулем при создании файла журнала.</span><span class="sxs-lookup"><span data-stu-id="4a35d-510">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="4a35d-511">Пул приложений должен иметь доступ на запись в папку, где записываются журналы (используйте атрибут `IIS AppPool\<app_pool_name>` для предоставления разрешения на запись).</span><span class="sxs-lookup"><span data-stu-id="4a35d-511">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="4a35d-512">Журналы не выполняют циклический сдвиг, пока не произойдет процесс перезапуска или перезагрузки.</span><span class="sxs-lookup"><span data-stu-id="4a35d-512">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="4a35d-513">Администратор несет ответственность за ограничение дискового пространства, которое потребляют журналы.</span><span class="sxs-lookup"><span data-stu-id="4a35d-513">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="4a35d-514">Использование журнала stdout рекомендуется только для устранения неполадок при запуске приложений.</span><span class="sxs-lookup"><span data-stu-id="4a35d-514">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="4a35d-515">Не используйте журнал stdout для ведения общего журнала приложений.</span><span class="sxs-lookup"><span data-stu-id="4a35d-515">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="4a35d-516">Для обычного входа в приложение ASP.NET Core используйте библиотеку ведения журналов, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов.</span><span class="sxs-lookup"><span data-stu-id="4a35d-516">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="4a35d-517">Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="4a35d-517">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="4a35d-518">При создании файла журнала автоматически добавляются отметка времени и расширение файла.</span><span class="sxs-lookup"><span data-stu-id="4a35d-518">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="4a35d-519">Имя файла журнала составляется путем добавления метки времени, идентификатора процесса и расширения файла ( *.log*) к последнему сегменту атрибута пути `stdoutLogFile` (обычно *stdout*) с символами подчеркивания в качестве разделителей.</span><span class="sxs-lookup"><span data-stu-id="4a35d-519">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="4a35d-520">Если атрибут пути `stdoutLogFile` заканчивается элементом *stdout*, журнал приложения с идентификатором 1934, созданный 5 февраля 2018 г. в 19:42:32, будет иметь имя *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-520">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="4a35d-521">Если `stdoutLogEnabled` имеет значение false, возникающие при запуске приложения ошибки записываются и передаются в журнал событий (макс. 30 КБ).</span><span class="sxs-lookup"><span data-stu-id="4a35d-521">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="4a35d-522">После запуска все дополнительные журналы удаляются.</span><span class="sxs-lookup"><span data-stu-id="4a35d-522">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="4a35d-523">В следующем примере элемент `aspNetCore` настраивает ведение журнала stdout для приложения, размещенного в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="4a35d-523">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="4a35d-524">Для локального ведения журнала допустим локальный или общий сетевой путь.</span><span class="sxs-lookup"><span data-stu-id="4a35d-524">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="4a35d-525">Убедитесь, что идентификатор пользователя AppPool имеет разрешение на запись по указанному пути.</span><span class="sxs-lookup"><span data-stu-id="4a35d-525">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="4a35d-526">Расширенные журналы диагностики</span><span class="sxs-lookup"><span data-stu-id="4a35d-526">Enhanced diagnostic logs</span></span>

<span data-ttu-id="4a35d-527">Модуль ASP.NET Core можно настроить. Он позволяет работать с расширенными журналами диагностики.</span><span class="sxs-lookup"><span data-stu-id="4a35d-527">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="4a35d-528">Добавьте элемент `<handlerSettings>` в элемент `<aspNetCore>` в файле *web.config*. Задайте параметру `debugLevel` значение `TRACE`, чтобы обеспечить высокую точность диагностических сведений:</span><span class="sxs-lookup"><span data-stu-id="4a35d-528">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="debugFile" value=".\logs\aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="4a35d-529">Папки, указанные в пути к значению `<handlerSetting>` (*logs* в приведенном выше примере), не создаются модулем автоматически и должны заранее существовать в развертывании.</span><span class="sxs-lookup"><span data-stu-id="4a35d-529">Folders in the path provided to the `<handlerSetting>` value (*logs* in the preceding example) aren't created by the module automatically and should pre-exist in the deployment.</span></span> <span data-ttu-id="4a35d-530">Пул приложений должен иметь доступ на запись в папку, где записываются журналы (используйте атрибут `IIS AppPool\<app_pool_name>` для предоставления разрешения на запись).</span><span class="sxs-lookup"><span data-stu-id="4a35d-530">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="4a35d-531">Значения уровня отладки (`debugLevel`) могут включать уровень и расположение.</span><span class="sxs-lookup"><span data-stu-id="4a35d-531">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="4a35d-532">Уровни (в порядке возрастания степени детализации):</span><span class="sxs-lookup"><span data-stu-id="4a35d-532">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="4a35d-533">ОШИБКА</span><span class="sxs-lookup"><span data-stu-id="4a35d-533">ERROR</span></span>
* <span data-ttu-id="4a35d-534">ПРЕДУПРЕЖДЕНИЕ</span><span class="sxs-lookup"><span data-stu-id="4a35d-534">WARNING</span></span>
* <span data-ttu-id="4a35d-535">ИНФОРМАЦИЯ</span><span class="sxs-lookup"><span data-stu-id="4a35d-535">INFO</span></span>
* <span data-ttu-id="4a35d-536">TRACE</span><span class="sxs-lookup"><span data-stu-id="4a35d-536">TRACE</span></span>

<span data-ttu-id="4a35d-537">Расположения (допускаются несколько расположений):</span><span class="sxs-lookup"><span data-stu-id="4a35d-537">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="4a35d-538">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="4a35d-538">CONSOLE</span></span>
* <span data-ttu-id="4a35d-539">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="4a35d-539">EVENTLOG</span></span>
* <span data-ttu-id="4a35d-540">FILE</span><span class="sxs-lookup"><span data-stu-id="4a35d-540">FILE</span></span>

<span data-ttu-id="4a35d-541">Параметры обработчика могут быть указаны с помощью переменных среды:</span><span class="sxs-lookup"><span data-stu-id="4a35d-541">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="4a35d-542">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Путь к файлу журнала отладки.</span><span class="sxs-lookup"><span data-stu-id="4a35d-542">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="4a35d-543">(По умолчанию: *aspnetcore debug.log*)</span><span class="sxs-lookup"><span data-stu-id="4a35d-543">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="4a35d-544">`ASPNETCORE_MODULE_DEBUG` &ndash; Параметр уровня отладки.</span><span class="sxs-lookup"><span data-stu-id="4a35d-544">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="4a35d-545">**Не** оставляйте ведение журнала отладки включенным в развертывании дольше того времени, которое требуется для устранения проблемы.</span><span class="sxs-lookup"><span data-stu-id="4a35d-545">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="4a35d-546">Размер журнала не ограничен.</span><span class="sxs-lookup"><span data-stu-id="4a35d-546">The size of the log isn't limited.</span></span> <span data-ttu-id="4a35d-547">Если оставить журнал отладки включенным, он может исчерпать все доступное место на диске и привести к сбою сервера или службы приложений.</span><span class="sxs-lookup"><span data-stu-id="4a35d-547">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="4a35d-548">См. пример элемента `aspNetCore` в файле *web.config* в разделе [Конфигурация с помощью файла web.config](#configuration-with-webconfig).</span><span class="sxs-lookup"><span data-stu-id="4a35d-548">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="4a35d-549">Конфигурация прокси-сервера использует протокол HTTP и токен связывания</span><span class="sxs-lookup"><span data-stu-id="4a35d-549">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="4a35d-550">*Применяется только к размещению вне процесса.*</span><span class="sxs-lookup"><span data-stu-id="4a35d-550">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="4a35d-551">Прокси-сервер, созданный между модулем ASP.NET Core и Kestrel, использует протокол HTTP.</span><span class="sxs-lookup"><span data-stu-id="4a35d-551">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="4a35d-552">Отсутствует риск перехвата трафика между модулем и Kestrel из расположения на сервере.</span><span class="sxs-lookup"><span data-stu-id="4a35d-552">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="4a35d-553">Токен связывания гарантирует, что полученные Kestrel запросы были переданы службами IIS, а не из какого-либо другого источника.</span><span class="sxs-lookup"><span data-stu-id="4a35d-553">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="4a35d-554">Этот токен создается и задается модулем в переменную среды (`ASPNETCORE_TOKEN`).</span><span class="sxs-lookup"><span data-stu-id="4a35d-554">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="4a35d-555">Он также задается в заголовке (`MS-ASPNETCORE-TOKEN`) каждого запроса, переданного через прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="4a35d-555">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="4a35d-556">ПО промежуточного слоя IIS проверяет каждый получаемый запрос, чтобы убедиться, что заголовок с токеном связывания соответствует значению переменной среды.</span><span class="sxs-lookup"><span data-stu-id="4a35d-556">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="4a35d-557">Если значения токена не совпадают, запрос заносится в журнал и отклоняется.</span><span class="sxs-lookup"><span data-stu-id="4a35d-557">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="4a35d-558">Переменная среды с токеном связывания и трафик между модулем и Kestrel недоступны из расположения на сервере.</span><span class="sxs-lookup"><span data-stu-id="4a35d-558">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="4a35d-559">Не зная значение токена связывания, злоумышленник не может отправлять запросы, обходящие проверку в ПО промежуточного слоя IIS.</span><span class="sxs-lookup"><span data-stu-id="4a35d-559">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="4a35d-560">Модуль ASP.NET Core с общей конфигурацией IIS</span><span class="sxs-lookup"><span data-stu-id="4a35d-560">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="4a35d-561">Программа установки модуля ASP.NET Core запускается с правами учетной записи **TrustedInstaller**.</span><span class="sxs-lookup"><span data-stu-id="4a35d-561">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="4a35d-562">Поскольку учетная запись локальной системы не имеет разрешения на изменение пути к общей папке, используемого общей конфигурацией IIS, установщик получает ошибку отказа в доступе при попытке настроить параметры модуля в файле *applicationHost.config* общей папки.</span><span class="sxs-lookup"><span data-stu-id="4a35d-562">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="4a35d-563">При использовании общей конфигурации IIS на том же компьютере, где установлены службы IIS, запустите установщик пакета размещения ASP.NET Core с параметром `OPT_NO_SHARED_CONFIG_CHECK` со значением `1`:</span><span class="sxs-lookup"><span data-stu-id="4a35d-563">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="4a35d-564">Если путь к общей конфигурации не на том же компьютере, где установлены службы IIS, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="4a35d-564">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="4a35d-565">Отключите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="4a35d-565">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="4a35d-566">Запустите установщик.</span><span class="sxs-lookup"><span data-stu-id="4a35d-566">Run the installer.</span></span>
1. <span data-ttu-id="4a35d-567">Экспортируйте обновленный файл *applicationHost.config* в общую папку.</span><span class="sxs-lookup"><span data-stu-id="4a35d-567">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="4a35d-568">Повторно включите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="4a35d-568">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="4a35d-569">Версия модуля и журналы установщика хостинга Bundle</span><span class="sxs-lookup"><span data-stu-id="4a35d-569">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="4a35d-570">Чтобы определить версию установщика модуля ASP.NET Core, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="4a35d-570">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="4a35d-571">В системе размещения перейдите к папке *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-571">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="4a35d-572">Найдите файл *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-572">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="4a35d-573">Щелкните правой кнопкой мыши файл и выберите **Свойства** из контекстного меню.</span><span class="sxs-lookup"><span data-stu-id="4a35d-573">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="4a35d-574">Выберите вкладку **Сведения**. **Версия файла** и **Версия продукта** дают представление об установленной версии модуля.</span><span class="sxs-lookup"><span data-stu-id="4a35d-574">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="4a35d-575">Журналы установщика хостинга Bundle для модуля находятся в папке *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. Этот файл имеет имя *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-575">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="4a35d-576">Модуль, схемы и расположение файлов конфигурации</span><span class="sxs-lookup"><span data-stu-id="4a35d-576">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="4a35d-577">Module</span><span class="sxs-lookup"><span data-stu-id="4a35d-577">Module</span></span>

<span data-ttu-id="4a35d-578">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="4a35d-578">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="4a35d-579">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4a35d-579">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="4a35d-580">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4a35d-580">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="4a35d-581">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="4a35d-581">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="4a35d-582">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="4a35d-582">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="4a35d-583">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="4a35d-583">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="4a35d-584">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4a35d-584">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="4a35d-585">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4a35d-585">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="4a35d-586">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="4a35d-586">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="4a35d-587">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="4a35d-587">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="4a35d-588">Схема</span><span class="sxs-lookup"><span data-stu-id="4a35d-588">Schema</span></span>

<span data-ttu-id="4a35d-589">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="4a35d-589">**IIS**</span></span>

* <span data-ttu-id="4a35d-590">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="4a35d-590">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="4a35d-591">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="4a35d-591">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="4a35d-592">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="4a35d-592">**IIS Express**</span></span>

* <span data-ttu-id="4a35d-593">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="4a35d-593">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="4a35d-594">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="4a35d-594">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="4a35d-595">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="4a35d-595">Configuration</span></span>

<span data-ttu-id="4a35d-596">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="4a35d-596">**IIS**</span></span>

* <span data-ttu-id="4a35d-597">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="4a35d-597">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="4a35d-598">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="4a35d-598">**IIS Express**</span></span>

* <span data-ttu-id="4a35d-599">Visual Studio: {КОРЕНЬ ПРИЛОЖЕНИЯ}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="4a35d-599">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="4a35d-600">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="4a35d-600">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="4a35d-601">Файлы можно найти путем поиска *aspnetcore* в файле *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-601">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="4a35d-602">Модуль ASP.NET Core имеет собственный модуль IIS, который подключается к конвейеру IIS для переадресации веб-запросов в серверные приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4a35d-602">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="4a35d-603">Поддерживаемые версии Windows:</span><span class="sxs-lookup"><span data-stu-id="4a35d-603">Supported Windows versions:</span></span>

* <span data-ttu-id="4a35d-604">Windows 7 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="4a35d-604">Windows 7 or later</span></span>
* <span data-ttu-id="4a35d-605">Windows Server 2008 R2 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="4a35d-605">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="4a35d-606">Модуль работает только с Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4a35d-606">The module only works with Kestrel.</span></span> <span data-ttu-id="4a35d-607">Модуль несовместим с [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="4a35d-607">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="4a35d-608">Так как приложения ASP.NET Core выполняются в процессе, отделенном от рабочего процесса IIS, этот модуль также обрабатывает управление процессами.</span><span class="sxs-lookup"><span data-stu-id="4a35d-608">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="4a35d-609">Модуль запускает процесс для приложения ASP.NET Core при поступлении первого запроса и перезапускает приложение при сбое.</span><span class="sxs-lookup"><span data-stu-id="4a35d-609">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="4a35d-610">Это, по сути, совпадает с поведением приложений ASP.NET 4.x, выполняемых внутрипроцессно в IIS и управляемых [службой активации процессов Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="4a35d-610">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="4a35d-611">На следующей схеме показана связь между IIS, модулем ASP.NET Core и приложением:</span><span class="sxs-lookup"><span data-stu-id="4a35d-611">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Модуль ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="4a35d-613">Запросы поступают из Интернета в драйвер HTTP.sys в режиме ядра.</span><span class="sxs-lookup"><span data-stu-id="4a35d-613">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="4a35d-614">Драйвер направляет запросы к службам IIS на настроенный порт веб-сайта — обычно 80 (HTTP) или 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="4a35d-614">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="4a35d-615">Модуль перенаправляет запросы Kestrel на случайный порт для приложения, отличающийся от порта 80 или 443.</span><span class="sxs-lookup"><span data-stu-id="4a35d-615">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="4a35d-616">Модуль задает порт с помощью переменной среды во время запуска, а [ПО промежуточного слоя для интеграции IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) настраивает сервер для прослушивания `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-616">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="4a35d-617">Выполняются дополнительные проверки, и запросы не из модуля отклоняются.</span><span class="sxs-lookup"><span data-stu-id="4a35d-617">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="4a35d-618">Модуль не поддерживает переадресацию по HTTPS, поэтому запросы переадресовываются по протоколу HTTP, даже если были получены IIS по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4a35d-618">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="4a35d-619">После того как Kestrel забирает запрос из модуля, запрос передается в конвейер ПО промежуточного слоя ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4a35d-619">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="4a35d-620">Конвейер ПО промежуточного слоя обрабатывает запрос и передает его в качестве экземпляра `HttpContext` в логику приложения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-620">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="4a35d-621">ПО промежуточного слоя, добавленное интеграцией IIS, обновляет схему, удаленный IP-адрес и базовый путь для переадресации запроса в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4a35d-621">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="4a35d-622">Отклик приложения передается обратно в службу IIS, которая отправляет его обратно в HTTP-клиент, инициировавший запрос.</span><span class="sxs-lookup"><span data-stu-id="4a35d-622">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="4a35d-623">Многие собственные модули, такие как проверка подлинности Windows, остаются активными.</span><span class="sxs-lookup"><span data-stu-id="4a35d-623">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="4a35d-624">Дополнительные сведения о модулях IIS, активных с модулем ASP.NET Core, см. в разделе <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="4a35d-624">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="4a35d-625">Дополнительные возможности модуля ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="4a35d-625">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="4a35d-626">Задание переменных среды для рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="4a35d-626">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="4a35d-627">Внесение в журнал выходных данных stdout для хранилища файлов с целью устранения неполадок при запуске.</span><span class="sxs-lookup"><span data-stu-id="4a35d-627">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="4a35d-628">Переадресация токенов проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="4a35d-628">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="4a35d-629">Как установить и использовать модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4a35d-629">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="4a35d-630">Инструкции о том, как установить модуль ASP.NET Core, см. в разделе [Установка пакета размещения .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="4a35d-630">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="4a35d-631">Конфигурация с помощью файла web.config</span><span class="sxs-lookup"><span data-stu-id="4a35d-631">Configuration with web.config</span></span>

<span data-ttu-id="4a35d-632">Модуль ASP.NET Core настроен с помощью раздела `aspNetCore` узла `system.webServer` файла *web.config* на веб-сайте.</span><span class="sxs-lookup"><span data-stu-id="4a35d-632">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="4a35d-633">Следующий файл *web.config* публикуется для [зависимого от платформы развертывания](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) и настраивает модуль ASP.NET Core для обработки запросов к веб-сайту.</span><span class="sxs-lookup"><span data-stu-id="4a35d-633">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="4a35d-634">Следующий файл *web.config* опубликован для [автономного развертывания](/dotnet/articles/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="4a35d-634">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="4a35d-635">Когда приложение развернуто в [службе приложений Azure](https://azure.microsoft.com/services/app-service/), путь `stdoutLogFile` задан как `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-635">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="4a35d-636">Путь сохраняет журналы stdout в папке *LogFiles*, расположение которой автоматически создается службой.</span><span class="sxs-lookup"><span data-stu-id="4a35d-636">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="4a35d-637">Сведения о конфигурации дочерних приложений IIS см. здесь: <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="4a35d-637">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="4a35d-638">Атрибуты элемента aspNetCore</span><span class="sxs-lookup"><span data-stu-id="4a35d-638">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="4a35d-639">Атрибут</span><span class="sxs-lookup"><span data-stu-id="4a35d-639">Attribute</span></span> | <span data-ttu-id="4a35d-640">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="4a35d-640">Description</span></span> | <span data-ttu-id="4a35d-641">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="4a35d-641">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="4a35d-642">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-642">Optional string attribute.</span></span></p><p><span data-ttu-id="4a35d-643">Аргументы для исполняемого файла, указанного в атрибуте **processPath**.</span><span class="sxs-lookup"><span data-stu-id="4a35d-643">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="4a35d-644">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-644">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="4a35d-645">Если значение равно true, страница **502.5 — ошибка процесса** подавляется и страница в файле *web.config* с кодом состояния 502 имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="4a35d-645">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="4a35d-646">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-646">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="4a35d-647">Если значение равно true, маркер безопасности отправляется дочернему процессу, прослушивающему порт %ASPNETCORE_PORT% как заголовок "MS-ASPNETCORE-WINAUTHTOKEN" каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="4a35d-647">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="4a35d-648">Этот процесс вызывает CloseHandle по этому маркеру безопасности каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="4a35d-648">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="4a35d-649">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-649">Optional integer attribute.</span></span></p><p><span data-ttu-id="4a35d-650">Указывает число экземпляров процесса, заданное в параметре **processPath**, которое может появиться для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-650">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="4a35d-651">Параметр `processesPerApplication` не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="4a35d-651">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="4a35d-652">Этот атрибут будет удален в будущем выпуске.</span><span class="sxs-lookup"><span data-stu-id="4a35d-652">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="4a35d-653">По умолчанию: `1`</span><span class="sxs-lookup"><span data-stu-id="4a35d-653">Default: `1`</span></span><br><span data-ttu-id="4a35d-654">Минимум: `1`</span><span class="sxs-lookup"><span data-stu-id="4a35d-654">Min: `1`</span></span><br><span data-ttu-id="4a35d-655">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="4a35d-655">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="4a35d-656">Обязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-656">Required string attribute.</span></span></p><p><span data-ttu-id="4a35d-657">Путь к исполняемому файлу, который запускает процесс прослушивания HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="4a35d-657">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="4a35d-658">Поддерживаются относительные пути.</span><span class="sxs-lookup"><span data-stu-id="4a35d-658">Relative paths are supported.</span></span> <span data-ttu-id="4a35d-659">Если путь начинается с `.`, то начало пути считается относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="4a35d-659">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="4a35d-660">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-660">Optional integer attribute.</span></span></p><p><span data-ttu-id="4a35d-661">Указывает количество сбоев за минуту, которыми может завершиться процесс, указанный в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="4a35d-661">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="4a35d-662">Если этот предел превышен, модуль останавливает запуск процесса на оставшуюся часть минуты.</span><span class="sxs-lookup"><span data-stu-id="4a35d-662">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="4a35d-663">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="4a35d-663">Default: `10`</span></span><br><span data-ttu-id="4a35d-664">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="4a35d-664">Min: `0`</span></span><br><span data-ttu-id="4a35d-665">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="4a35d-665">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="4a35d-666">Необязательный атрибут timespan.</span><span class="sxs-lookup"><span data-stu-id="4a35d-666">Optional timespan attribute.</span></span></p><p><span data-ttu-id="4a35d-667">Указывает продолжительность, на протяжении которой модуль ASP.NET Core ожидает ответа от процесса, прослушивающего порт %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="4a35d-667">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="4a35d-668">В версиях модуля ASP.NET Core, поставляемых с выпуском ASP.NET Core 2.1 или новее, атрибут `requestTimeout` указывается в часах, минутах и секундах.</span><span class="sxs-lookup"><span data-stu-id="4a35d-668">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="4a35d-669">По умолчанию: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="4a35d-669">Default: `00:02:00`</span></span><br><span data-ttu-id="4a35d-670">Минимум: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="4a35d-670">Min: `00:00:00`</span></span><br><span data-ttu-id="4a35d-671">Максимум: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="4a35d-671">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="4a35d-672">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-672">Optional integer attribute.</span></span></p><p><span data-ttu-id="4a35d-673">Длительность ожидания модуля в секундах, пока произойдет правильное выключение исполняемого файла при обнаружении файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-673">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="4a35d-674">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="4a35d-674">Default: `10`</span></span><br><span data-ttu-id="4a35d-675">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="4a35d-675">Min: `0`</span></span><br><span data-ttu-id="4a35d-676">Максимум: `600`</span><span class="sxs-lookup"><span data-stu-id="4a35d-676">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="4a35d-677">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-677">Optional integer attribute.</span></span></p><p><span data-ttu-id="4a35d-678">Время в секундах, которое модуль ожидает, пока запустится процесс прослушивания порта исполняемого файла.</span><span class="sxs-lookup"><span data-stu-id="4a35d-678">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="4a35d-679">Если этот предел превышен, модуль завершает процесс.</span><span class="sxs-lookup"><span data-stu-id="4a35d-679">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="4a35d-680">Модуль пытается перезапустить процесс при получении нового запроса и будет продолжать пытаться перезапустить процесс для последующих входящих запросов, если не удается запустить приложение определенное в атрибуте **rapidFailsPerMinute** количество раз за последнюю минуту.</span><span class="sxs-lookup"><span data-stu-id="4a35d-680">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="4a35d-681">Значение 0 (ноль) **не** считается бесконечным временем ожидания.</span><span class="sxs-lookup"><span data-stu-id="4a35d-681">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="4a35d-682">По умолчанию: `120`</span><span class="sxs-lookup"><span data-stu-id="4a35d-682">Default: `120`</span></span><br><span data-ttu-id="4a35d-683">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="4a35d-683">Min: `0`</span></span><br><span data-ttu-id="4a35d-684">Максимум: `3600`</span><span class="sxs-lookup"><span data-stu-id="4a35d-684">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="4a35d-685">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-685">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="4a35d-686">Если значение равно true, **stdout** и **stderr** для процесса, указанного в атрибуте **processPath**, перенаправляются к файлу, заданному в атрибуте **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="4a35d-686">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="4a35d-687">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="4a35d-687">Optional string attribute.</span></span></p><p><span data-ttu-id="4a35d-688">Указывает относительный или абсолютный путь к файлу, для которого регистрируются **stdout** и **stderr** из процесса, указанного в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="4a35d-688">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="4a35d-689">Относительные пути задаются относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="4a35d-689">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="4a35d-690">Любой путь, начинающийся с `.`, относится к корневому каталогу веб-сайта, а все остальные пути рассматриваются как абсолютные пути.</span><span class="sxs-lookup"><span data-stu-id="4a35d-690">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="4a35d-691">Все папки, указанные в пути, должны существовать, чтобы модуль мог создать файл журнала.</span><span class="sxs-lookup"><span data-stu-id="4a35d-691">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="4a35d-692">С помощью разделителей подчеркивания метка времени, идентификатор процесса и расширение файла ( *.log*) добавляются к последнему сегменту пути журнала **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="4a35d-692">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="4a35d-693">Если в качестве значения задано значение `.\logs\stdout`, например, журнал stdout сохраняется как *stdout_20180205194132_1934.log* в папке *журналов* с датой 5 февраля 2018 г. в 19:41:32 с идентификатором процесса 1934.</span><span class="sxs-lookup"><span data-stu-id="4a35d-693">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="4a35d-694">Настройка переменных среды</span><span class="sxs-lookup"><span data-stu-id="4a35d-694">Setting environment variables</span></span>

<span data-ttu-id="4a35d-695">Переменные среды для процесса можно указать в атрибуте `processPath`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-695">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="4a35d-696">Укажите переменную среды с дочерним элементом `<environmentVariable>` элемента коллекции `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-696">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="4a35d-697">Переменные среды, заданные в этом разделе, конфликтуют с переменными системной среды с такими же именами.</span><span class="sxs-lookup"><span data-stu-id="4a35d-697">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="4a35d-698">Если переменная среды задана и в файле *web.config*, и на уровне системы в Windows, значение из файла *web.config* добавляется к значению переменной системной среды (например, `ASPNETCORE_ENVIRONMENT: Development;Development`), которая препятствует запуску приложения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-698">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

<span data-ttu-id="4a35d-699">В следующем примере устанавливаются две переменные среды.</span><span class="sxs-lookup"><span data-stu-id="4a35d-699">The following example sets two environment variables.</span></span> <span data-ttu-id="4a35d-700">`ASPNETCORE_ENVIRONMENT` настраивает среду приложения для `Development`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-700">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="4a35d-701">Разработчик может временно задать это значение в файле *web.config*, чтобы принудительно загрузить [Страницу исключений для разработчиков](xref:fundamentals/error-handling) при отладке исключения приложения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-701">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="4a35d-702">`CONFIG_DIR` — пример пользовательской переменной среды, где разработчик написал код, который считывает значение при запуске, чтобы сформировать путь для загрузки файла конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="4a35d-702">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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

> [!WARNING]
> <span data-ttu-id="4a35d-703">Установите только переменную среды `ASPNETCORE_ENVIRONMENT` для `Development` на серверах промежуточных процессов и тестирования, которые недоступны для ненадежных сетей, таких как Интернет.</span><span class="sxs-lookup"><span data-stu-id="4a35d-703">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="4a35d-704">App_offline.htm</span><span class="sxs-lookup"><span data-stu-id="4a35d-704">app_offline.htm</span></span>

<span data-ttu-id="4a35d-705">Если в корневом каталоге приложения обнаружен файл с именем *app_offline.htm*, модуль ASP.NET Core пытается корректно закрыть приложение и прекратить обработку входящих запросов.</span><span class="sxs-lookup"><span data-stu-id="4a35d-705">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="4a35d-706">Если приложение по-прежнему выполняется через количество секунд, определенное атрибутом `shutdownTimeLimit`, модуль ASP.NET Core завершает выполняющийся процесс.</span><span class="sxs-lookup"><span data-stu-id="4a35d-706">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="4a35d-707">Хотя файл *app_offline.htm* присутствует, модуль ASP.NET Core отвечает на запросы, отправляя назад содержимое файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-707">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="4a35d-708">Когда *app_offline.htm* файл удаляется, следующий запрос запускает приложение.</span><span class="sxs-lookup"><span data-stu-id="4a35d-708">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="4a35d-709">Страница ошибок запуска</span><span class="sxs-lookup"><span data-stu-id="4a35d-709">Start-up error page</span></span>

<span data-ttu-id="4a35d-710">Если модулю ASP.NET Core не удается запустить серверный процесс или начинается серверный процесс, но ему не удается прослушать настроенный порт, появится страница кода состояния *502.5 — ошибка процесса*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-710">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="4a35d-711">Чтобы подавить эту страницу и вернуться к странице IIS кода состояния 502 по умолчанию, используйте атрибут `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-711">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="4a35d-712">Дополнительные сведения о настройке пользовательских сообщений об ошибках см. в разделе [Ошибки HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="4a35d-712">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Страница кода состояния "502.5 — ошибка процесса"](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="4a35d-714">Создание и перенаправление журнала</span><span class="sxs-lookup"><span data-stu-id="4a35d-714">Log creation and redirection</span></span>

<span data-ttu-id="4a35d-715">Модуль ASP.NET Core перенаправляет выходные потоки консоли stdout и stderr на диск, если заданы атрибуты `stdoutLogEnabled` и `stdoutLogFile` элемента `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="4a35d-715">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="4a35d-716">Все папки, указанные в пути `stdoutLogFile`, создаются модулем при создании файла журнала.</span><span class="sxs-lookup"><span data-stu-id="4a35d-716">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="4a35d-717">Пул приложений должен иметь доступ на запись в папку, где записываются журналы (используйте атрибут `IIS AppPool\<app_pool_name>` для предоставления разрешения на запись).</span><span class="sxs-lookup"><span data-stu-id="4a35d-717">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="4a35d-718">Журналы не выполняют циклический сдвиг, пока не произойдет процесс перезапуска или перезагрузки.</span><span class="sxs-lookup"><span data-stu-id="4a35d-718">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="4a35d-719">Администратор несет ответственность за ограничение дискового пространства, которое потребляют журналы.</span><span class="sxs-lookup"><span data-stu-id="4a35d-719">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="4a35d-720">Использование журнала stdout рекомендуется только для устранения неполадок при запуске приложений.</span><span class="sxs-lookup"><span data-stu-id="4a35d-720">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="4a35d-721">Не используйте журнал stdout для ведения общего журнала приложений.</span><span class="sxs-lookup"><span data-stu-id="4a35d-721">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="4a35d-722">Для обычного входа в приложение ASP.NET Core используйте библиотеку ведения журналов, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов.</span><span class="sxs-lookup"><span data-stu-id="4a35d-722">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="4a35d-723">Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="4a35d-723">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="4a35d-724">При создании файла журнала автоматически добавляются отметка времени и расширение файла.</span><span class="sxs-lookup"><span data-stu-id="4a35d-724">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="4a35d-725">Имя файла журнала составляется путем добавления метки времени, идентификатора процесса и расширения файла ( *.log*) к последнему сегменту атрибута пути `stdoutLogFile` (обычно *stdout*) с символами подчеркивания в качестве разделителей.</span><span class="sxs-lookup"><span data-stu-id="4a35d-725">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="4a35d-726">Если атрибут пути `stdoutLogFile` заканчивается элементом *stdout*, журнал приложения с идентификатором 1934, созданный 5 февраля 2018 г. в 19:42:32, будет иметь имя *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-726">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="4a35d-727">В следующем примере элемент `aspNetCore` настраивает ведение журнала stdout для приложения, размещенного в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="4a35d-727">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="4a35d-728">Для локального ведения журнала допустим локальный или общий сетевой путь.</span><span class="sxs-lookup"><span data-stu-id="4a35d-728">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="4a35d-729">Убедитесь, что идентификатор пользователя AppPool имеет разрешение на запись по указанному пути.</span><span class="sxs-lookup"><span data-stu-id="4a35d-729">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

<span data-ttu-id="4a35d-730">Папки, указанные в пути к значению `<handlerSetting>` (*logs* в приведенном выше примере), не создаются модулем автоматически и должны заранее существовать в развертывании.</span><span class="sxs-lookup"><span data-stu-id="4a35d-730">Folders in the path provided to the `<handlerSetting>` value (*logs* in the preceding example) aren't created by the module automatically and should pre-exist in the deployment.</span></span> <span data-ttu-id="4a35d-731">Пул приложений должен иметь доступ на запись в папку, где записываются журналы (используйте атрибут `IIS AppPool\<app_pool_name>` для предоставления разрешения на запись).</span><span class="sxs-lookup"><span data-stu-id="4a35d-731">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="4a35d-732">См. пример элемента `aspNetCore` в файле *web.config* в разделе [Конфигурация с помощью файла web.config](#configuration-with-webconfig).</span><span class="sxs-lookup"><span data-stu-id="4a35d-732">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="4a35d-733">Конфигурация прокси-сервера использует протокол HTTP и токен связывания</span><span class="sxs-lookup"><span data-stu-id="4a35d-733">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="4a35d-734">Прокси-сервер, созданный между модулем ASP.NET Core и Kestrel, использует протокол HTTP.</span><span class="sxs-lookup"><span data-stu-id="4a35d-734">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="4a35d-735">Отсутствует риск перехвата трафика между модулем и Kestrel из расположения на сервере.</span><span class="sxs-lookup"><span data-stu-id="4a35d-735">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="4a35d-736">Токен связывания гарантирует, что полученные Kestrel запросы были переданы службами IIS, а не из какого-либо другого источника.</span><span class="sxs-lookup"><span data-stu-id="4a35d-736">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="4a35d-737">Этот токен создается и задается модулем в переменную среды (`ASPNETCORE_TOKEN`).</span><span class="sxs-lookup"><span data-stu-id="4a35d-737">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="4a35d-738">Он также задается в заголовке (`MS-ASPNETCORE-TOKEN`) каждого запроса, переданного через прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="4a35d-738">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="4a35d-739">ПО промежуточного слоя IIS проверяет каждый получаемый запрос, чтобы убедиться, что заголовок с токеном связывания соответствует значению переменной среды.</span><span class="sxs-lookup"><span data-stu-id="4a35d-739">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="4a35d-740">Если значения токена не совпадают, запрос заносится в журнал и отклоняется.</span><span class="sxs-lookup"><span data-stu-id="4a35d-740">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="4a35d-741">Переменная среды с токеном связывания и трафик между модулем и Kestrel недоступны из расположения на сервере.</span><span class="sxs-lookup"><span data-stu-id="4a35d-741">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="4a35d-742">Не зная значение токена связывания, злоумышленник не может отправлять запросы, обходящие проверку в ПО промежуточного слоя IIS.</span><span class="sxs-lookup"><span data-stu-id="4a35d-742">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="4a35d-743">Модуль ASP.NET Core с общей конфигурацией IIS</span><span class="sxs-lookup"><span data-stu-id="4a35d-743">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="4a35d-744">Программа установки модуля ASP.NET Core запускается с правами учетной записи **TrustedInstaller**.</span><span class="sxs-lookup"><span data-stu-id="4a35d-744">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="4a35d-745">Поскольку учетная запись локальной системы не имеет разрешения на изменение пути к общей папке, используемого общей конфигурацией IIS, установщик получает ошибку отказа в доступе при попытке настроить параметры модуля в файле *applicationHost.config* общей папки.</span><span class="sxs-lookup"><span data-stu-id="4a35d-745">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="4a35d-746">При использовании общей конфигурации IIS выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="4a35d-746">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="4a35d-747">Отключите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="4a35d-747">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="4a35d-748">Запустите установщик.</span><span class="sxs-lookup"><span data-stu-id="4a35d-748">Run the installer.</span></span>
1. <span data-ttu-id="4a35d-749">Экспортируйте обновленный файл *applicationHost.config* в общую папку.</span><span class="sxs-lookup"><span data-stu-id="4a35d-749">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="4a35d-750">Повторно включите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="4a35d-750">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="4a35d-751">Версия модуля и журналы установщика хостинга Bundle</span><span class="sxs-lookup"><span data-stu-id="4a35d-751">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="4a35d-752">Чтобы определить версию установщика модуля ASP.NET Core, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="4a35d-752">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="4a35d-753">В системе размещения перейдите к папке *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-753">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="4a35d-754">Найдите файл *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-754">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="4a35d-755">Щелкните правой кнопкой мыши файл и выберите **Свойства** из контекстного меню.</span><span class="sxs-lookup"><span data-stu-id="4a35d-755">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="4a35d-756">Выберите вкладку **Сведения**. **Версия файла** и **Версия продукта** дают представление об установленной версии модуля.</span><span class="sxs-lookup"><span data-stu-id="4a35d-756">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="4a35d-757">Журналы установщика хостинга Bundle для модуля находятся в папке *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. Этот файл имеет имя *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-757">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="4a35d-758">Модуль, схемы и расположение файлов конфигурации</span><span class="sxs-lookup"><span data-stu-id="4a35d-758">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="4a35d-759">Module</span><span class="sxs-lookup"><span data-stu-id="4a35d-759">Module</span></span>

<span data-ttu-id="4a35d-760">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="4a35d-760">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="4a35d-761">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4a35d-761">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="4a35d-762">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4a35d-762">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="4a35d-763">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="4a35d-763">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="4a35d-764">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4a35d-764">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="4a35d-765">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4a35d-765">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="4a35d-766">Схема</span><span class="sxs-lookup"><span data-stu-id="4a35d-766">Schema</span></span>

<span data-ttu-id="4a35d-767">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="4a35d-767">**IIS**</span></span>

* <span data-ttu-id="4a35d-768">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="4a35d-768">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="4a35d-769">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="4a35d-769">**IIS Express**</span></span>

* <span data-ttu-id="4a35d-770">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="4a35d-770">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="4a35d-771">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="4a35d-771">Configuration</span></span>

<span data-ttu-id="4a35d-772">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="4a35d-772">**IIS**</span></span>

* <span data-ttu-id="4a35d-773">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="4a35d-773">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="4a35d-774">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="4a35d-774">**IIS Express**</span></span>

* <span data-ttu-id="4a35d-775">Visual Studio: {КОРЕНЬ ПРИЛОЖЕНИЯ}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="4a35d-775">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="4a35d-776">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="4a35d-776">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="4a35d-777">Файлы можно найти путем поиска *aspnetcore* в файле *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="4a35d-777">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="4a35d-778">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="4a35d-778">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="4a35d-779">Репозиторий GitHub для модуля ASP.NET Core (справочные материалы)</span><span class="sxs-lookup"><span data-stu-id="4a35d-779">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
