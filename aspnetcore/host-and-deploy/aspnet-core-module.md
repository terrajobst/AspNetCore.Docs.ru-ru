---
title: Модуль ASP.NET Core
author: guardrex
description: Сведения о настройке модуля ASP.NET Core для размещения приложений ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/08/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: c1c34f368cb3f7767bf0f229ff70c5ab53c6005f
ms.sourcegitcommit: fcdf9aaa6c45c1a926bd870ed8f893bdb4935152
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72165327"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="fb6b7-103">Модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fb6b7-103">ASP.NET Core Module</span></span>

<span data-ttu-id="fb6b7-104">Авторы: [Том Дайкстра (Tom Dykstra)](https://github.com/tdykstra), [Рик Штраль (Rick Strahl)](https://github.com/RickStrahl), [Крис Росс (Chris Ross)](https://github.com/Tratcher), [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT), [Сураб Ширхатти (Sourabh Shirhatti)](https://twitter.com/sshirhatti), [ Джастин Коталик (Justin Kotalik)](https://github.com/jkotalik) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fb6b7-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fb6b7-105">Модуль ASP.NET Core имеет собственный модуль IIS, который подключается к конвейеру IIS для выполнения следующих задач:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="fb6b7-106">Размещение приложения ASP.NET Core внутри рабочего процесса IIS (`w3wp.exe`). Это так называемая [модель внутрипроцессного размещения](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="fb6b7-107">Переадресация веб-запросов к серверной части приложения ASP.NET Core на [сервере Kestrel](xref:fundamentals/servers/kestrel). Это [модель размещения вне процесса](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="fb6b7-108">Поддерживаемые версии Windows:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-108">Supported Windows versions:</span></span>

* <span data-ttu-id="fb6b7-109">Windows 7 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="fb6b7-109">Windows 7 or later</span></span>
* <span data-ttu-id="fb6b7-110">Windows Server 2008 R2 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="fb6b7-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="fb6b7-111">При размещении в процессе модуль использует реализацию внутрипроцессного сервера для IIS — HTTP-сервер IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="fb6b7-112">При размещении вне процесса модуль работает только с Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="fb6b7-113">Модуль не работает с [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-113">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="fb6b7-114">Модели размещения</span><span class="sxs-lookup"><span data-stu-id="fb6b7-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="fb6b7-115">Модель внутрипроцессного размещения</span><span class="sxs-lookup"><span data-stu-id="fb6b7-115">In-process hosting model</span></span>

<span data-ttu-id="fb6b7-116">По умолчанию приложения ASP.NET Core используют модель внутрипроцессного размещения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-116">ASP.NET Core apps default to the in-process hosting model.</span></span>

<span data-ttu-id="fb6b7-117">При внутрипроцессном размещении применимы следующие характеристики:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-117">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="fb6b7-118">Вместо сервера [Kestrel](xref:fundamentals/servers/kestrel) используется HTTP-сервер IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-118">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="fb6b7-119">Для внутрипроцессной обработки [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) вызывает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-119">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="fb6b7-120">Регистрация `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-120">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="fb6b7-121">Настройка порта и базового пути, которые будет прослушивать сервер при выполнении за модулем ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-121">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="fb6b7-122">Настройка перехвата ошибок запуска на узле.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-122">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="fb6b7-123">[Атрибут requestTimeout](#attributes-of-the-aspnetcore-element) не применяется к внутрипроцессному размещению.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-123">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="fb6b7-124">Совместное использование пула приложений среди приложений не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-124">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="fb6b7-125">Используйте один пул приложений для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-125">Use one app pool per app.</span></span>

* <span data-ttu-id="fb6b7-126">При использовании [веб-развертывания](/iis/publish/using-web-deploy/introduction-to-web-deploy) или размещении [файла app_offline.htm в развертывании](xref:host-and-deploy/iis/index#locked-deployment-files) вручную приложение может не завершить работу сразу при наличии открытого соединения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-126">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="fb6b7-127">Например, подключение websocket может задерживать завершение работы приложения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-127">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="fb6b7-128">Архитектура (разрядность) приложения и установленная среда выполнения (x64 или x86) должны соответствовать архитектуре пула приложений.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-128">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="fb6b7-129">Обнаружены отключения клиентов.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-129">Client disconnects are detected.</span></span> <span data-ttu-id="fb6b7-130">При отключении клиента происходит отмена токена отмены [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-130">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="fb6b7-131">В ASP.NET Core 2.2.1 и более ранних версий <xref:System.IO.Directory.GetCurrentDirectory*> возвращает рабочий каталог процесса, запущенного службами IIS, а не каталог приложения (например, *C:\Windows\System32\inetsrv* для *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-131">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="fb6b7-132">Пример кода, который задает текущий каталог приложения, см. в разделе [Класс CurrentDirectoryHelpers](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-132">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="fb6b7-133">Вызовите метод `SetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-133">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="fb6b7-134">Последующие вызовы <xref:System.IO.Directory.GetCurrentDirectory*> возвращают каталог приложения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-134">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="fb6b7-135">При размещении в процессе <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> не вызывается внутри для инициализации пользователя.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-135">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="fb6b7-136">Таким образом, реализация <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>, используемая для преобразования утверждений после каждой проверки подлинности, не активируется по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-136">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="fb6b7-137">При преобразовании утверждений с реализацией <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> вызовите <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> для добавления служб проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-137">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="fb6b7-138">Модель размещения вне процесса</span><span class="sxs-lookup"><span data-stu-id="fb6b7-138">Out-of-process hosting model</span></span>

<span data-ttu-id="fb6b7-139">Чтобы настроить приложение для внепроцессного размещения, задайте для свойства `<AspNetCoreHostingModel>` значение `OutOfProcess` в файле проекта (*CSPROJ*):</span><span class="sxs-lookup"><span data-stu-id="fb6b7-139">To configure an app for out-of-process hosting, set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` in the project file (*.csproj*):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="fb6b7-140">Для внутрипроцессного размещения указано значение `InProcess`, которое является значением по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-140">In-process hosting is set with `InProcess`, which is the default value.</span></span>

<span data-ttu-id="fb6b7-141">Сервер [Kestrel](xref:fundamentals/servers/kestrel) используется вместо HTTP-сервера IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-141">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="fb6b7-142">Для внепроцессной обработки [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) вызывает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-142">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="fb6b7-143">Настройка порта и базового пути, которые будет прослушивать сервер при выполнении за модулем ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-143">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="fb6b7-144">Настройка перехвата ошибок запуска на узле.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-144">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="fb6b7-145">Изменения модели размещения</span><span class="sxs-lookup"><span data-stu-id="fb6b7-145">Hosting model changes</span></span>

<span data-ttu-id="fb6b7-146">Если параметр `hostingModel` изменяется в файле *web.config* (как описано в разделе [Конфигурация с помощью web.config](#configuration-with-webconfig)), модуль перезапускает рабочий процесс для служб IIS.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-146">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="fb6b7-147">Для IIS Express модуль не перезапускает рабочий процесс, а запускает нормальное завершение работы текущего процесса IIS Express.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-147">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="fb6b7-148">Следующий запрос для приложения порождает новый процесс IIS Express.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-148">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="fb6b7-149">Имя процесса</span><span class="sxs-lookup"><span data-stu-id="fb6b7-149">Process name</span></span>

<span data-ttu-id="fb6b7-150">`Process.GetCurrentProcess().ProcessName` сообщает `w3wp`/`iisexpress` (внутри процесса) или `dotnet` (вне процесса).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-150">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="fb6b7-151">Многие собственные модули, такие как проверка подлинности Windows, остаются активными.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-151">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="fb6b7-152">Дополнительные сведения о модулях IIS, активных с модулем ASP.NET Core, см. в разделе <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-152">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="fb6b7-153">Дополнительные возможности модуля ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-153">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="fb6b7-154">Задание переменных среды для рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-154">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="fb6b7-155">Внесение в журнал выходных данных stdout для хранилища файлов с целью устранения неполадок при запуске.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-155">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="fb6b7-156">Переадресация токенов проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-156">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="fb6b7-157">Как установить и использовать модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fb6b7-157">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="fb6b7-158">Инструкции о том, как установить модуль ASP.NET Core, см. в разделе [Установка пакета размещения .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-158">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="fb6b7-159">Конфигурация с помощью файла web.config</span><span class="sxs-lookup"><span data-stu-id="fb6b7-159">Configuration with web.config</span></span>

<span data-ttu-id="fb6b7-160">Модуль ASP.NET Core настроен с помощью раздела `aspNetCore` узла `system.webServer` файла *web.config* на веб-сайте.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-160">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="fb6b7-161">Следующий файл *web.config* публикуется для [зависимого от платформы развертывания](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) и настраивает модуль ASP.NET Core для обработки запросов к веб-сайту.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-161">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="fb6b7-162">Следующий файл *web.config* опубликован для [автономного развертывания](/dotnet/articles/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-162">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="fb6b7-163">Значение `false` свойства <xref:System.Configuration.SectionInformation.InheritInChildApplications*> указывает, что параметры, заданные в элементе [\<расположение>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location), не наследуются приложениями, которые находятся во вложенном каталоге приложения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-163">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="fb6b7-164">Когда приложение развернуто в [службе приложений Azure](https://azure.microsoft.com/services/app-service/), путь `stdoutLogFile` задан как `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-164">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="fb6b7-165">Путь сохраняет журналы stdout в папке *LogFiles*, расположение которой автоматически создается службой.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-165">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="fb6b7-166">Сведения о конфигурации дочерних приложений IIS см. здесь: <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-166">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="fb6b7-167">Атрибуты элемента aspNetCore</span><span class="sxs-lookup"><span data-stu-id="fb6b7-167">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="fb6b7-168">Атрибут</span><span class="sxs-lookup"><span data-stu-id="fb6b7-168">Attribute</span></span> | <span data-ttu-id="fb6b7-169">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="fb6b7-169">Description</span></span> | <span data-ttu-id="fb6b7-170">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="fb6b7-170">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="fb6b7-171">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-171">Optional string attribute.</span></span></p><p><span data-ttu-id="fb6b7-172">Аргументы для исполняемого файла, указанного в атрибуте **processPath**.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-172">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="fb6b7-173">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-173">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="fb6b7-174">Если значение равно true, страница **502.5 — ошибка процесса** подавляется и страница в файле *web.config* с кодом состояния 502 имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-174">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="fb6b7-175">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-175">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="fb6b7-176">Если значение равно true, маркер безопасности отправляется дочернему процессу, прослушивающему порт %ASPNETCORE_PORT% как заголовок "MS-ASPNETCORE-WINAUTHTOKEN" каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-176">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="fb6b7-177">Этот процесс вызывает CloseHandle по этому маркеру безопасности каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-177">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="fb6b7-178">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-178">Optional string attribute.</span></span></p><p><span data-ttu-id="fb6b7-179">Указывает модель размещения — внутри процесса (`InProcess`) или вне процесса (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-179">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `InProcess` |
| `processesPerApplication` | <p><span data-ttu-id="fb6b7-180">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-180">Optional integer attribute.</span></span></p><p><span data-ttu-id="fb6b7-181">Указывает число экземпляров процесса, заданное в параметре **processPath**, которое может появиться для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-181">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="fb6b7-182">&dagger;Для внутрипроцессного размещения существует ограничение: `1`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-182">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="fb6b7-183">Параметр `processesPerApplication` не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-183">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="fb6b7-184">Этот атрибут будет удален в будущем выпуске.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-184">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="fb6b7-185">По умолчанию: `1`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-185">Default: `1`</span></span><br><span data-ttu-id="fb6b7-186">Минимум: `1`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-186">Min: `1`</span></span><br><span data-ttu-id="fb6b7-187">Максимум: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="fb6b7-187">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="fb6b7-188">Обязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-188">Required string attribute.</span></span></p><p><span data-ttu-id="fb6b7-189">Путь к исполняемому файлу, который запускает процесс прослушивания HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-189">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="fb6b7-190">Поддерживаются относительные пути.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-190">Relative paths are supported.</span></span> <span data-ttu-id="fb6b7-191">Если путь начинается с `.`, то начало пути считается относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-191">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="fb6b7-192">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-192">Optional integer attribute.</span></span></p><p><span data-ttu-id="fb6b7-193">Указывает количество сбоев за минуту, которыми может завершиться процесс, указанный в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-193">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="fb6b7-194">Если этот предел превышен, модуль останавливает запуск процесса на оставшуюся часть минуты.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-194">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="fb6b7-195">Не поддерживается для внутрипроцессного размещения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-195">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="fb6b7-196">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-196">Default: `10`</span></span><br><span data-ttu-id="fb6b7-197">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-197">Min: `0`</span></span><br><span data-ttu-id="fb6b7-198">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-198">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="fb6b7-199">Необязательный атрибут timespan.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-199">Optional timespan attribute.</span></span></p><p><span data-ttu-id="fb6b7-200">Указывает продолжительность, на протяжении которой модуль ASP.NET Core ожидает ответа от процесса, прослушивающего порт %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-200">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="fb6b7-201">В версиях модуля ASP.NET Core, поставляемых с выпуском ASP.NET Core 2.1 или новее, атрибут `requestTimeout` указывается в часах, минутах и секундах.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-201">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="fb6b7-202">Не применяется к внутрипроцессному размещению.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-202">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="fb6b7-203">Для внутрипроцессного размещения модуль ожидает, пока приложение не обработает запрос.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-203">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="fb6b7-204">Допустимые значения для сегментов минут и секунд в строках находятся в диапазоне 0–59.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-204">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="fb6b7-205">Значение **60** для минут и секунд приведет к ошибке *500 — внутренняя ошибка сервера*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-205">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="fb6b7-206">По умолчанию: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-206">Default: `00:02:00`</span></span><br><span data-ttu-id="fb6b7-207">Минимум: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-207">Min: `00:00:00`</span></span><br><span data-ttu-id="fb6b7-208">Максимум: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-208">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="fb6b7-209">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-209">Optional integer attribute.</span></span></p><p><span data-ttu-id="fb6b7-210">Длительность ожидания модуля в секундах, пока произойдет правильное выключение исполняемого файла при обнаружении файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-210">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="fb6b7-211">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-211">Default: `10`</span></span><br><span data-ttu-id="fb6b7-212">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-212">Min: `0`</span></span><br><span data-ttu-id="fb6b7-213">Максимум: `600`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-213">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="fb6b7-214">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-214">Optional integer attribute.</span></span></p><p><span data-ttu-id="fb6b7-215">Время в секундах, которое модуль ожидает, пока запустится процесс прослушивания порта исполняемого файла.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-215">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="fb6b7-216">Если этот предел превышен, модуль завершает процесс.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-216">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="fb6b7-217">Модуль пытается перезапустить процесс при получении нового запроса и будет продолжать пытаться перезапустить процесс для последующих входящих запросов, если не удается запустить приложение определенное в атрибуте **rapidFailsPerMinute** количество раз за последнюю минуту.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-217">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="fb6b7-218">Значение 0 (ноль) **не** считается бесконечным временем ожидания.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-218">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="fb6b7-219">По умолчанию: `120`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-219">Default: `120`</span></span><br><span data-ttu-id="fb6b7-220">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-220">Min: `0`</span></span><br><span data-ttu-id="fb6b7-221">Максимум: `3600`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-221">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="fb6b7-222">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-222">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="fb6b7-223">Если значение равно true, **stdout** и **stderr** для процесса, указанного в атрибуте **processPath**, перенаправляются к файлу, заданному в атрибуте **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-223">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="fb6b7-224">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-224">Optional string attribute.</span></span></p><p><span data-ttu-id="fb6b7-225">Указывает относительный или абсолютный путь к файлу, для которого регистрируются **stdout** и **stderr** из процесса, указанного в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-225">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="fb6b7-226">Относительные пути задаются относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-226">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="fb6b7-227">Любой путь, начинающийся с `.`, относится к корневому каталогу веб-сайта, а все остальные пути рассматриваются как абсолютные пути.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-227">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="fb6b7-228">Все папки, указанные в пути, создаются модулем при создании файла журнала.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-228">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="fb6b7-229">С помощью разделителей подчеркивания метка времени, идентификатор процесса и расширение файла ( *.log*) добавляются к последнему сегменту пути журнала **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-229">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="fb6b7-230">Если в качестве значения задано значение `.\logs\stdout`, например, журнал stdout сохраняется как *stdout_20180205194132_1934.log* в папке *журналов* с датой 5 февраля 2018 г. в 19:41:32 с идентификатором процесса 1934.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-230">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="set-environment-variables"></a><span data-ttu-id="fb6b7-231">Настройка переменных среды</span><span class="sxs-lookup"><span data-stu-id="fb6b7-231">Set environment variables</span></span>

<span data-ttu-id="fb6b7-232">Переменные среды для процесса можно указать в атрибуте `processPath`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-232">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="fb6b7-233">Укажите переменную среды с дочерним элементом `<environmentVariable>` элемента коллекции `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-233">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="fb6b7-234">Переменные среды, установленные в этом разделе, имеют приоритет над переменными системной среды.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-234">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="fb6b7-235">В следующем примере устанавливаются две переменные среды в *web.config*. `ASPNETCORE_ENVIRONMENT` настраивает в качестве среды приложения `Development`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-235">The following example sets two environment variables in *web.config*. `ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="fb6b7-236">Разработчик может временно задать это значение в файле *web.config*, чтобы принудительно загрузить [Страницу исключений для разработчиков](xref:fundamentals/error-handling) при отладке исключения приложения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-236">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="fb6b7-237">`CONFIG_DIR` — пример пользовательской переменной среды, где разработчик написал код, который считывает значение при запуске, чтобы сформировать путь для загрузки файла конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-237">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="fb6b7-238">Вместо задания среды напрямую в *web.config* включите свойство `<EnvironmentName>` в профиль публикации ( *.pubxml*) или файл проекта.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-238">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="fb6b7-239">При этом подходе во время публикации проекта среда задается в файле *web.config*:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-239">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="fb6b7-240">Установите только переменную среды `ASPNETCORE_ENVIRONMENT` для `Development` на серверах промежуточных процессов и тестирования, которые недоступны для ненадежных сетей, таких как Интернет.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-240">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="fb6b7-241">App_offline.htm</span><span class="sxs-lookup"><span data-stu-id="fb6b7-241">app_offline.htm</span></span>

<span data-ttu-id="fb6b7-242">Если в корневом каталоге приложения обнаружен файл с именем *app_offline.htm*, модуль ASP.NET Core пытается корректно закрыть приложение и прекратить обработку входящих запросов.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-242">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="fb6b7-243">Если приложение по-прежнему выполняется через количество секунд, определенное атрибутом `shutdownTimeLimit`, модуль ASP.NET Core завершает выполняющийся процесс.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-243">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="fb6b7-244">Хотя файл *app_offline.htm* присутствует, модуль ASP.NET Core отвечает на запросы, отправляя назад содержимое файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-244">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="fb6b7-245">Когда *app_offline.htm* файл удаляется, следующий запрос запускает приложение.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-245">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="fb6b7-246">При использовании модели размещения вне процесса приложение может не завершать работу немедленно при наличии открытого соединения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-246">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="fb6b7-247">Например, подключение websocket может задерживать завершение работы приложения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-247">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="fb6b7-248">Страница ошибок запуска</span><span class="sxs-lookup"><span data-stu-id="fb6b7-248">Start-up error page</span></span>

<span data-ttu-id="fb6b7-249">Если при внутри- или внепроцессном размещении происходит сбой запуска приложения, открываются страницы пользовательских сообщений об ошибках.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-249">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="fb6b7-250">Если модулю ASP.NET Core не удается найти внутри- или внепроцессный обработчик запросов, откроется страница кода состояния *500.0 — ошибка загрузки внутри- или внепроцессного обработчика запросов*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-250">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="fb6b7-251">Если в модели размещения внутри процесса модулю ASP.NET Core не удается запустить приложение, откроется страница кода состояния *500.30 — ошибка запуска*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-251">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="fb6b7-252">Если в модели размещения вне процесса модулю ASP.NET Core не удается запустить серверный процесс или начинается серверный процесс, но ему не удается прослушать настроенный порт, появится страница кода состояния *502.5 — ошибка процесса*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-252">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="fb6b7-253">Чтобы подавить отображение этой странице и вернуться к странице IIS кода состояния 5xx по умолчанию, используйте атрибут `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-253">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="fb6b7-254">Дополнительные сведения о настройке пользовательских сообщений об ошибках см. в разделе [Ошибки HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-254">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="fb6b7-255">Создание и перенаправление журнала</span><span class="sxs-lookup"><span data-stu-id="fb6b7-255">Log creation and redirection</span></span>

<span data-ttu-id="fb6b7-256">Модуль ASP.NET Core перенаправляет выходные потоки консоли stdout и stderr на диск, если заданы атрибуты `stdoutLogEnabled` и `stdoutLogFile` элемента `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-256">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="fb6b7-257">Все папки, указанные в пути `stdoutLogFile`, создаются модулем при создании файла журнала.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-257">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="fb6b7-258">Пул приложений должен иметь доступ на запись в папку, где записываются журналы (используйте атрибут `IIS AppPool\<app_pool_name>` для предоставления разрешения на запись).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-258">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="fb6b7-259">Журналы не выполняют циклический сдвиг, пока не произойдет процесс перезапуска или перезагрузки.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-259">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="fb6b7-260">Администратор несет ответственность за ограничение дискового пространства, которое потребляют журналы.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-260">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="fb6b7-261">Использование журнала stdout рекомендуется только для устранения неполадок при запуске приложений.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-261">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="fb6b7-262">Не используйте журнал stdout для ведения общего журнала приложений.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-262">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="fb6b7-263">Для обычного входа в приложение ASP.NET Core используйте библиотеку ведения журналов, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-263">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="fb6b7-264">Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-264">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="fb6b7-265">При создании файла журнала автоматически добавляются отметка времени и расширение файла.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-265">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="fb6b7-266">Имя файла журнала составляется путем добавления метки времени, идентификатора процесса и расширения файла ( *.log*) к последнему сегменту атрибута пути `stdoutLogFile` (обычно *stdout*) с символами подчеркивания в качестве разделителей.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-266">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="fb6b7-267">Если атрибут пути `stdoutLogFile` заканчивается элементом *stdout*, журнал приложения с идентификатором 1934, созданный 5 февраля 2018 г. в 19:42:32, будет иметь имя *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-267">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="fb6b7-268">Если `stdoutLogEnabled` имеет значение false, возникающие при запуске приложения ошибки записываются и передаются в журнал событий (макс. 30 КБ).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-268">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="fb6b7-269">После запуска все дополнительные журналы удаляются.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-269">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="fb6b7-270">В следующем примере элемент `aspNetCore` в файле *web.config* настраивает ведение журнала stdout для приложения, размещенного в Службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-270">The following sample `aspNetCore` element in a *web.config* file configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="fb6b7-271">Для локального ведения журнала допустим локальный или общий сетевой путь.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-271">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="fb6b7-272">Убедитесь, что идентификатор пользователя AppPool имеет разрешение на запись по указанному пути.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-272">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="fb6b7-273">Расширенные журналы диагностики</span><span class="sxs-lookup"><span data-stu-id="fb6b7-273">Enhanced diagnostic logs</span></span>

<span data-ttu-id="fb6b7-274">Модуль ASP.NET Core можно настроить. Он позволяет работать с расширенными журналами диагностики.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-274">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="fb6b7-275">Добавьте элемент `<handlerSettings>` в элемент `<aspNetCore>` в файле *web.config*. Задайте параметру `debugLevel` значение `TRACE`, чтобы обеспечить высокую точность диагностических сведений:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-275">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="fb6b7-276">Все папки, указанные в пути (*logs* в приведенном выше примере), создаются модулем при создании файла журнала.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-276">Any folders in the path (*logs* in the preceding example) are created by the module when the log file is created.</span></span> <span data-ttu-id="fb6b7-277">Пул приложений должен иметь доступ на запись в папку, где записываются журналы (используйте атрибут `IIS AppPool\<app_pool_name>` для предоставления разрешения на запись).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-277">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="fb6b7-278">Значения уровня отладки (`debugLevel`) могут включать уровень и расположение.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-278">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="fb6b7-279">Уровни (в порядке возрастания степени детализации):</span><span class="sxs-lookup"><span data-stu-id="fb6b7-279">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="fb6b7-280">ОШИБКА</span><span class="sxs-lookup"><span data-stu-id="fb6b7-280">ERROR</span></span>
* <span data-ttu-id="fb6b7-281">ПРЕДУПРЕЖДЕНИЕ</span><span class="sxs-lookup"><span data-stu-id="fb6b7-281">WARNING</span></span>
* <span data-ttu-id="fb6b7-282">ИНФОРМАЦИЯ</span><span class="sxs-lookup"><span data-stu-id="fb6b7-282">INFO</span></span>
* <span data-ttu-id="fb6b7-283">TRACE</span><span class="sxs-lookup"><span data-stu-id="fb6b7-283">TRACE</span></span>

<span data-ttu-id="fb6b7-284">Расположения (допускаются несколько расположений):</span><span class="sxs-lookup"><span data-stu-id="fb6b7-284">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="fb6b7-285">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="fb6b7-285">CONSOLE</span></span>
* <span data-ttu-id="fb6b7-286">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="fb6b7-286">EVENTLOG</span></span>
* <span data-ttu-id="fb6b7-287">FILE</span><span class="sxs-lookup"><span data-stu-id="fb6b7-287">FILE</span></span>

<span data-ttu-id="fb6b7-288">Параметры обработчика могут быть указаны с помощью переменных среды:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-288">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="fb6b7-289">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Путь к файлу журнала отладки.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-289">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="fb6b7-290">(По умолчанию: *aspnetcore debug.log*)</span><span class="sxs-lookup"><span data-stu-id="fb6b7-290">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="fb6b7-291">`ASPNETCORE_MODULE_DEBUG` &ndash; Параметр уровня отладки.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-291">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="fb6b7-292">**Не** оставляйте ведение журнала отладки включенным в развертывании дольше того времени, которое требуется для устранения проблемы.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-292">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="fb6b7-293">Размер журнала не ограничен.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-293">The size of the log isn't limited.</span></span> <span data-ttu-id="fb6b7-294">Если оставить журнал отладки включенным, он может исчерпать все доступное место на диске и привести к сбою сервера или службы приложений.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-294">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="fb6b7-295">См. пример элемента `aspNetCore` в файле *web.config* в разделе [Конфигурация с помощью файла web.config](#configuration-with-webconfig).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-295">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="modify-the-stack-size"></a><span data-ttu-id="fb6b7-296">Изменение размера стека</span><span class="sxs-lookup"><span data-stu-id="fb6b7-296">Modify the stack size</span></span>

<span data-ttu-id="fb6b7-297">*Применяется только при использовании модели внутрипроцессного размещения.*</span><span class="sxs-lookup"><span data-stu-id="fb6b7-297">*Only applies when using the in-process hosting model.*</span></span>

<span data-ttu-id="fb6b7-298">Настройте размер управляемого стека, задав для параметра `stackSize` значение в байтах в файле *web.config*. Размер по умолчанию — `1048576` байт (1 МБ).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-298">Configure the managed stack size using the `stackSize` setting in bytes in *web.config*. The default size is `1048576` bytes (1 MB).</span></span>

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

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="fb6b7-299">Конфигурация прокси-сервера использует протокол HTTP и токен связывания</span><span class="sxs-lookup"><span data-stu-id="fb6b7-299">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="fb6b7-300">*Применяется только к размещению вне процесса.*</span><span class="sxs-lookup"><span data-stu-id="fb6b7-300">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="fb6b7-301">Прокси-сервер, созданный между модулем ASP.NET Core и Kestrel, использует протокол HTTP.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-301">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="fb6b7-302">Отсутствует риск перехвата трафика между модулем и Kestrel из расположения на сервере.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-302">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="fb6b7-303">Токен связывания гарантирует, что полученные Kestrel запросы были переданы службами IIS, а не из какого-либо другого источника.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-303">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="fb6b7-304">Этот токен создается и задается модулем в переменную среды (`ASPNETCORE_TOKEN`).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-304">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="fb6b7-305">Он также задается в заголовке (`MS-ASPNETCORE-TOKEN`) каждого запроса, переданного через прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-305">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="fb6b7-306">ПО промежуточного слоя IIS проверяет каждый получаемый запрос, чтобы убедиться, что заголовок с токеном связывания соответствует значению переменной среды.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-306">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="fb6b7-307">Если значения токена не совпадают, запрос заносится в журнал и отклоняется.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-307">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="fb6b7-308">Переменная среды с токеном связывания и трафик между модулем и Kestrel недоступны из расположения на сервере.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-308">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="fb6b7-309">Не зная значение токена связывания, злоумышленник не может отправлять запросы, обходящие проверку в ПО промежуточного слоя IIS.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-309">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="fb6b7-310">Модуль ASP.NET Core с общей конфигурацией IIS</span><span class="sxs-lookup"><span data-stu-id="fb6b7-310">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="fb6b7-311">Программа установки модуля ASP.NET Core запускается с правами учетной записи **TrustedInstaller**.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-311">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="fb6b7-312">Поскольку учетная запись локальной системы не имеет разрешения на изменение пути к общей папке, используемого общей конфигурацией IIS, установщик получает ошибку отказа в доступе при попытке настроить параметры модуля в файле *applicationHost.config* общей папки.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-312">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="fb6b7-313">При использовании общей конфигурации IIS на том же компьютере, где установлены службы IIS, запустите установщик пакета размещения ASP.NET Core с параметром `OPT_NO_SHARED_CONFIG_CHECK` со значением `1`:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-313">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="fb6b7-314">Если путь к общей конфигурации не на том же компьютере, где установлены службы IIS, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-314">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="fb6b7-315">Отключите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-315">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="fb6b7-316">Запустите установщик.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-316">Run the installer.</span></span>
1. <span data-ttu-id="fb6b7-317">Экспортируйте обновленный файл *applicationHost.config* в общую папку.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-317">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="fb6b7-318">Повторно включите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-318">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="fb6b7-319">Версия модуля и журналы установщика хостинга Bundle</span><span class="sxs-lookup"><span data-stu-id="fb6b7-319">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="fb6b7-320">Чтобы определить версию установщика модуля ASP.NET Core, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-320">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="fb6b7-321">В системе размещения перейдите к папке *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-321">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="fb6b7-322">Найдите файл *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-322">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="fb6b7-323">Щелкните правой кнопкой мыши файл и выберите **Свойства** из контекстного меню.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-323">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="fb6b7-324">Выберите вкладку **Сведения**. **Версия файла** и **Версия продукта** дают представление об установленной версии модуля.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-324">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="fb6b7-325">Журналы установщика хостинга Bundle для модуля находятся в папке *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. Этот файл имеет имя *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-325">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="fb6b7-326">Модуль, схемы и расположение файлов конфигурации</span><span class="sxs-lookup"><span data-stu-id="fb6b7-326">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="fb6b7-327">Module</span><span class="sxs-lookup"><span data-stu-id="fb6b7-327">Module</span></span>

<span data-ttu-id="fb6b7-328">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="fb6b7-328">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="fb6b7-329">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fb6b7-329">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="fb6b7-330">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fb6b7-330">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="fb6b7-331">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="fb6b7-331">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="fb6b7-332">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="fb6b7-332">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="fb6b7-333">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="fb6b7-333">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="fb6b7-334">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fb6b7-334">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="fb6b7-335">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fb6b7-335">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="fb6b7-336">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="fb6b7-336">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="fb6b7-337">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="fb6b7-337">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="fb6b7-338">Схема</span><span class="sxs-lookup"><span data-stu-id="fb6b7-338">Schema</span></span>

<span data-ttu-id="fb6b7-339">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="fb6b7-339">**IIS**</span></span>

* <span data-ttu-id="fb6b7-340">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="fb6b7-340">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="fb6b7-341">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="fb6b7-341">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="fb6b7-342">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="fb6b7-342">**IIS Express**</span></span>

* <span data-ttu-id="fb6b7-343">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="fb6b7-343">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="fb6b7-344">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="fb6b7-344">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="fb6b7-345">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="fb6b7-345">Configuration</span></span>

<span data-ttu-id="fb6b7-346">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="fb6b7-346">**IIS**</span></span>

* <span data-ttu-id="fb6b7-347">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="fb6b7-347">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="fb6b7-348">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="fb6b7-348">**IIS Express**</span></span>

* <span data-ttu-id="fb6b7-349">Visual Studio: {КОРЕНЬ ПРИЛОЖЕНИЯ}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="fb6b7-349">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="fb6b7-350">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="fb6b7-350">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="fb6b7-351">Файлы можно найти путем поиска *aspnetcore* в файле *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-351">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="fb6b7-352">Модуль ASP.NET Core имеет собственный модуль IIS, который подключается к конвейеру IIS для выполнения следующих задач:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-352">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="fb6b7-353">Размещение приложения ASP.NET Core внутри рабочего процесса IIS (`w3wp.exe`). Это так называемая [модель внутрипроцессного размещения](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-353">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="fb6b7-354">Переадресация веб-запросов к серверной части приложения ASP.NET Core на [сервере Kestrel](xref:fundamentals/servers/kestrel). Это [модель размещения вне процесса](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-354">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="fb6b7-355">Поддерживаемые версии Windows:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-355">Supported Windows versions:</span></span>

* <span data-ttu-id="fb6b7-356">Windows 7 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="fb6b7-356">Windows 7 or later</span></span>
* <span data-ttu-id="fb6b7-357">Windows Server 2008 R2 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="fb6b7-357">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="fb6b7-358">При размещении в процессе модуль использует реализацию внутрипроцессного сервера для IIS — HTTP-сервер IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-358">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="fb6b7-359">При размещении вне процесса модуль работает только с Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-359">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="fb6b7-360">Модуль не работает с [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-360">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="fb6b7-361">Модели размещения</span><span class="sxs-lookup"><span data-stu-id="fb6b7-361">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="fb6b7-362">Модель внутрипроцессного размещения</span><span class="sxs-lookup"><span data-stu-id="fb6b7-362">In-process hosting model</span></span>

<span data-ttu-id="fb6b7-363">Чтобы настроить приложение для внутрипроцессного размещения, добавьте свойство `<AspNetCoreHostingModel>` к файлу проекта приложения со значением `InProcess` (размещение вне процесса имеет значение `OutOfProcess`):</span><span class="sxs-lookup"><span data-stu-id="fb6b7-363">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="fb6b7-364">Модель внутрипроцессного размещения не поддерживается для приложений ASP.NET Core, предназначенных для .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-364">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="fb6b7-365">Если свойство `<AspNetCoreHostingModel>` отсутствует в файле, значение по умолчанию — `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-365">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="fb6b7-366">При внутрипроцессном размещении применимы следующие характеристики:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-366">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="fb6b7-367">Вместо сервера [Kestrel](xref:fundamentals/servers/kestrel) используется HTTP-сервер IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-367">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="fb6b7-368">Для внутрипроцессной обработки [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) вызывает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-368">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="fb6b7-369">Регистрация `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-369">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="fb6b7-370">Настройка порта и базового пути, которые будет прослушивать сервер при выполнении за модулем ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-370">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="fb6b7-371">Настройка перехвата ошибок запуска на узле.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-371">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="fb6b7-372">[Атрибут requestTimeout](#attributes-of-the-aspnetcore-element) не применяется к внутрипроцессному размещению.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-372">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="fb6b7-373">Совместное использование пула приложений среди приложений не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-373">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="fb6b7-374">Используйте один пул приложений для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-374">Use one app pool per app.</span></span>

* <span data-ttu-id="fb6b7-375">При использовании [веб-развертывания](/iis/publish/using-web-deploy/introduction-to-web-deploy) или размещении [файла app_offline.htm в развертывании](xref:host-and-deploy/iis/index#locked-deployment-files) вручную приложение может не завершить работу сразу при наличии открытого соединения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-375">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="fb6b7-376">Например, подключение websocket может задерживать завершение работы приложения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-376">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="fb6b7-377">Архитектура (разрядность) приложения и установленная среда выполнения (x64 или x86) должны соответствовать архитектуре пула приложений.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-377">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="fb6b7-378">Обнаружены отключения клиентов.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-378">Client disconnects are detected.</span></span> <span data-ttu-id="fb6b7-379">При отключении клиента происходит отмена токена отмены [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-379">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="fb6b7-380">В ASP.NET Core 2.2.1 и более ранних версий <xref:System.IO.Directory.GetCurrentDirectory*> возвращает рабочий каталог процесса, запущенного службами IIS, а не каталог приложения (например, *C:\Windows\System32\inetsrv* для *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-380">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="fb6b7-381">Пример кода, который задает текущий каталог приложения, см. в разделе [Класс CurrentDirectoryHelpers](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-381">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="fb6b7-382">Вызовите метод `SetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-382">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="fb6b7-383">Последующие вызовы <xref:System.IO.Directory.GetCurrentDirectory*> возвращают каталог приложения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-383">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="fb6b7-384">При размещении в процессе <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> не вызывается внутри для инициализации пользователя.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-384">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="fb6b7-385">Таким образом, реализация <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>, используемая для преобразования утверждений после каждой проверки подлинности, не активируется по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-385">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="fb6b7-386">При преобразовании утверждений с реализацией <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> вызовите <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> для добавления служб проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-386">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="fb6b7-387">Модель размещения вне процесса</span><span class="sxs-lookup"><span data-stu-id="fb6b7-387">Out-of-process hosting model</span></span>

<span data-ttu-id="fb6b7-388">Чтобы настроить приложение для размещения вне процесса, используйте один из следующих подходов в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-388">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="fb6b7-389">Не указывайте свойство `<AspNetCoreHostingModel>`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-389">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="fb6b7-390">Если свойство `<AspNetCoreHostingModel>` отсутствует в файле, значение по умолчанию — `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-390">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="fb6b7-391">Установите для свойства `<AspNetCoreHostingModel>` значение `OutOfProcess` (внутрипроцессное размещение имеет значение `InProcess`):</span><span class="sxs-lookup"><span data-stu-id="fb6b7-391">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="fb6b7-392">Сервер [Kestrel](xref:fundamentals/servers/kestrel) используется вместо HTTP-сервера IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-392">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="fb6b7-393">Для внепроцессной обработки [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) вызывает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-393">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="fb6b7-394">Настройка порта и базового пути, которые будет прослушивать сервер при выполнении за модулем ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-394">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="fb6b7-395">Настройка перехвата ошибок запуска на узле.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-395">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="fb6b7-396">Изменения модели размещения</span><span class="sxs-lookup"><span data-stu-id="fb6b7-396">Hosting model changes</span></span>

<span data-ttu-id="fb6b7-397">Если параметр `hostingModel` изменяется в файле *web.config* (как описано в разделе [Конфигурация с помощью web.config](#configuration-with-webconfig)), модуль перезапускает рабочий процесс для служб IIS.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-397">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="fb6b7-398">Для IIS Express модуль не перезапускает рабочий процесс, а запускает нормальное завершение работы текущего процесса IIS Express.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-398">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="fb6b7-399">Следующий запрос для приложения порождает новый процесс IIS Express.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-399">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="fb6b7-400">Имя процесса</span><span class="sxs-lookup"><span data-stu-id="fb6b7-400">Process name</span></span>

<span data-ttu-id="fb6b7-401">`Process.GetCurrentProcess().ProcessName` сообщает `w3wp`/`iisexpress` (внутри процесса) или `dotnet` (вне процесса).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-401">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="fb6b7-402">Многие собственные модули, такие как проверка подлинности Windows, остаются активными.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-402">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="fb6b7-403">Дополнительные сведения о модулях IIS, активных с модулем ASP.NET Core, см. в разделе <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-403">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="fb6b7-404">Дополнительные возможности модуля ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-404">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="fb6b7-405">Задание переменных среды для рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-405">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="fb6b7-406">Внесение в журнал выходных данных stdout для хранилища файлов с целью устранения неполадок при запуске.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-406">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="fb6b7-407">Переадресация токенов проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-407">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="fb6b7-408">Как установить и использовать модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fb6b7-408">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="fb6b7-409">Инструкции о том, как установить модуль ASP.NET Core, см. в разделе [Установка пакета размещения .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-409">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="fb6b7-410">Конфигурация с помощью файла web.config</span><span class="sxs-lookup"><span data-stu-id="fb6b7-410">Configuration with web.config</span></span>

<span data-ttu-id="fb6b7-411">Модуль ASP.NET Core настроен с помощью раздела `aspNetCore` узла `system.webServer` файла *web.config* на веб-сайте.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-411">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="fb6b7-412">Следующий файл *web.config* публикуется для [зависимого от платформы развертывания](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) и настраивает модуль ASP.NET Core для обработки запросов к веб-сайту.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-412">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="fb6b7-413">Следующий файл *web.config* опубликован для [автономного развертывания](/dotnet/articles/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-413">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="fb6b7-414">Значение `false` свойства <xref:System.Configuration.SectionInformation.InheritInChildApplications*> указывает, что параметры, заданные в элементе [\<расположение>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location), не наследуются приложениями, которые находятся во вложенном каталоге приложения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-414">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="fb6b7-415">Когда приложение развернуто в [службе приложений Azure](https://azure.microsoft.com/services/app-service/), путь `stdoutLogFile` задан как `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-415">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="fb6b7-416">Путь сохраняет журналы stdout в папке *LogFiles*, расположение которой автоматически создается службой.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-416">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="fb6b7-417">Сведения о конфигурации дочерних приложений IIS см. здесь: <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-417">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="fb6b7-418">Атрибуты элемента aspNetCore</span><span class="sxs-lookup"><span data-stu-id="fb6b7-418">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="fb6b7-419">Атрибут</span><span class="sxs-lookup"><span data-stu-id="fb6b7-419">Attribute</span></span> | <span data-ttu-id="fb6b7-420">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="fb6b7-420">Description</span></span> | <span data-ttu-id="fb6b7-421">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="fb6b7-421">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="fb6b7-422">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-422">Optional string attribute.</span></span></p><p><span data-ttu-id="fb6b7-423">Аргументы для исполняемого файла, указанного в атрибуте **processPath**.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-423">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="fb6b7-424">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-424">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="fb6b7-425">Если значение равно true, страница **502.5 — ошибка процесса** подавляется и страница в файле *web.config* с кодом состояния 502 имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-425">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="fb6b7-426">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-426">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="fb6b7-427">Если значение равно true, маркер безопасности отправляется дочернему процессу, прослушивающему порт %ASPNETCORE_PORT% как заголовок "MS-ASPNETCORE-WINAUTHTOKEN" каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-427">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="fb6b7-428">Этот процесс вызывает CloseHandle по этому маркеру безопасности каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-428">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="fb6b7-429">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-429">Optional string attribute.</span></span></p><p><span data-ttu-id="fb6b7-430">Указывает модель размещения — внутри процесса (`InProcess`) или вне процесса (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-430">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="fb6b7-431">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-431">Optional integer attribute.</span></span></p><p><span data-ttu-id="fb6b7-432">Указывает число экземпляров процесса, заданное в параметре **processPath**, которое может появиться для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-432">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="fb6b7-433">&dagger;Для внутрипроцессного размещения существует ограничение: `1`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-433">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="fb6b7-434">Параметр `processesPerApplication` не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-434">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="fb6b7-435">Этот атрибут будет удален в будущем выпуске.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-435">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="fb6b7-436">По умолчанию: `1`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-436">Default: `1`</span></span><br><span data-ttu-id="fb6b7-437">Минимум: `1`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-437">Min: `1`</span></span><br><span data-ttu-id="fb6b7-438">Максимум: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="fb6b7-438">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="fb6b7-439">Обязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-439">Required string attribute.</span></span></p><p><span data-ttu-id="fb6b7-440">Путь к исполняемому файлу, который запускает процесс прослушивания HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-440">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="fb6b7-441">Поддерживаются относительные пути.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-441">Relative paths are supported.</span></span> <span data-ttu-id="fb6b7-442">Если путь начинается с `.`, то начало пути считается относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-442">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="fb6b7-443">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-443">Optional integer attribute.</span></span></p><p><span data-ttu-id="fb6b7-444">Указывает количество сбоев за минуту, которыми может завершиться процесс, указанный в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-444">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="fb6b7-445">Если этот предел превышен, модуль останавливает запуск процесса на оставшуюся часть минуты.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-445">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="fb6b7-446">Не поддерживается для внутрипроцессного размещения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-446">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="fb6b7-447">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-447">Default: `10`</span></span><br><span data-ttu-id="fb6b7-448">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-448">Min: `0`</span></span><br><span data-ttu-id="fb6b7-449">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-449">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="fb6b7-450">Необязательный атрибут timespan.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-450">Optional timespan attribute.</span></span></p><p><span data-ttu-id="fb6b7-451">Указывает продолжительность, на протяжении которой модуль ASP.NET Core ожидает ответа от процесса, прослушивающего порт %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-451">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="fb6b7-452">В версиях модуля ASP.NET Core, поставляемых с выпуском ASP.NET Core 2.1 или новее, атрибут `requestTimeout` указывается в часах, минутах и секундах.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-452">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="fb6b7-453">Не применяется к внутрипроцессному размещению.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-453">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="fb6b7-454">Для внутрипроцессного размещения модуль ожидает, пока приложение не обработает запрос.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-454">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="fb6b7-455">Допустимые значения для сегментов минут и секунд в строках находятся в диапазоне 0–59.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-455">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="fb6b7-456">Значение **60** для минут и секунд приведет к ошибке *500 — внутренняя ошибка сервера*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-456">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="fb6b7-457">По умолчанию: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-457">Default: `00:02:00`</span></span><br><span data-ttu-id="fb6b7-458">Минимум: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-458">Min: `00:00:00`</span></span><br><span data-ttu-id="fb6b7-459">Максимум: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-459">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="fb6b7-460">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-460">Optional integer attribute.</span></span></p><p><span data-ttu-id="fb6b7-461">Длительность ожидания модуля в секундах, пока произойдет правильное выключение исполняемого файла при обнаружении файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-461">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="fb6b7-462">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-462">Default: `10`</span></span><br><span data-ttu-id="fb6b7-463">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-463">Min: `0`</span></span><br><span data-ttu-id="fb6b7-464">Максимум: `600`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-464">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="fb6b7-465">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-465">Optional integer attribute.</span></span></p><p><span data-ttu-id="fb6b7-466">Время в секундах, которое модуль ожидает, пока запустится процесс прослушивания порта исполняемого файла.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-466">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="fb6b7-467">Если этот предел превышен, модуль завершает процесс.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-467">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="fb6b7-468">Модуль пытается перезапустить процесс при получении нового запроса и будет продолжать пытаться перезапустить процесс для последующих входящих запросов, если не удается запустить приложение определенное в атрибуте **rapidFailsPerMinute** количество раз за последнюю минуту.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-468">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="fb6b7-469">Значение 0 (ноль) **не** считается бесконечным временем ожидания.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-469">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="fb6b7-470">По умолчанию: `120`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-470">Default: `120`</span></span><br><span data-ttu-id="fb6b7-471">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-471">Min: `0`</span></span><br><span data-ttu-id="fb6b7-472">Максимум: `3600`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-472">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="fb6b7-473">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-473">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="fb6b7-474">Если значение равно true, **stdout** и **stderr** для процесса, указанного в атрибуте **processPath**, перенаправляются к файлу, заданному в атрибуте **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-474">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="fb6b7-475">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-475">Optional string attribute.</span></span></p><p><span data-ttu-id="fb6b7-476">Указывает относительный или абсолютный путь к файлу, для которого регистрируются **stdout** и **stderr** из процесса, указанного в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-476">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="fb6b7-477">Относительные пути задаются относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-477">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="fb6b7-478">Любой путь, начинающийся с `.`, относится к корневому каталогу веб-сайта, а все остальные пути рассматриваются как абсолютные пути.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-478">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="fb6b7-479">Все папки, указанные в пути, создаются модулем при создании файла журнала.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-479">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="fb6b7-480">С помощью разделителей подчеркивания метка времени, идентификатор процесса и расширение файла ( *.log*) добавляются к последнему сегменту пути журнала **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-480">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="fb6b7-481">Если в качестве значения задано значение `.\logs\stdout`, например, журнал stdout сохраняется как *stdout_20180205194132_1934.log* в папке *журналов* с датой 5 февраля 2018 г. в 19:41:32 с идентификатором процесса 1934.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-481">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="fb6b7-482">Настройка переменных среды</span><span class="sxs-lookup"><span data-stu-id="fb6b7-482">Setting environment variables</span></span>

<span data-ttu-id="fb6b7-483">Переменные среды для процесса можно указать в атрибуте `processPath`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-483">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="fb6b7-484">Укажите переменную среды с дочерним элементом `<environmentVariable>` элемента коллекции `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-484">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="fb6b7-485">Переменные среды, установленные в этом разделе, имеют приоритет над переменными системной среды.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-485">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="fb6b7-486">В следующем примере устанавливаются две переменные среды.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-486">The following example sets two environment variables.</span></span> <span data-ttu-id="fb6b7-487">`ASPNETCORE_ENVIRONMENT` настраивает среду приложения для `Development`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-487">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="fb6b7-488">Разработчик может временно задать это значение в файле *web.config*, чтобы принудительно загрузить [Страницу исключений для разработчиков](xref:fundamentals/error-handling) при отладке исключения приложения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-488">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="fb6b7-489">`CONFIG_DIR` — пример пользовательской переменной среды, где разработчик написал код, который считывает значение при запуске, чтобы сформировать путь для загрузки файла конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-489">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="fb6b7-490">Вместо задания среды напрямую в *web.config* включите свойство `<EnvironmentName>` в профиль публикации ( *.pubxml*) или файл проекта.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-490">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="fb6b7-491">При этом подходе во время публикации проекта среда задается в файле *web.config*:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-491">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="fb6b7-492">Установите только переменную среды `ASPNETCORE_ENVIRONMENT` для `Development` на серверах промежуточных процессов и тестирования, которые недоступны для ненадежных сетей, таких как Интернет.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-492">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="fb6b7-493">App_offline.htm</span><span class="sxs-lookup"><span data-stu-id="fb6b7-493">app_offline.htm</span></span>

<span data-ttu-id="fb6b7-494">Если в корневом каталоге приложения обнаружен файл с именем *app_offline.htm*, модуль ASP.NET Core пытается корректно закрыть приложение и прекратить обработку входящих запросов.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-494">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="fb6b7-495">Если приложение по-прежнему выполняется через количество секунд, определенное атрибутом `shutdownTimeLimit`, модуль ASP.NET Core завершает выполняющийся процесс.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-495">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="fb6b7-496">Хотя файл *app_offline.htm* присутствует, модуль ASP.NET Core отвечает на запросы, отправляя назад содержимое файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-496">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="fb6b7-497">Когда *app_offline.htm* файл удаляется, следующий запрос запускает приложение.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-497">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="fb6b7-498">При использовании модели размещения вне процесса приложение может не завершать работу немедленно при наличии открытого соединения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-498">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="fb6b7-499">Например, подключение websocket может задерживать завершение работы приложения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-499">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="fb6b7-500">Страница ошибок запуска</span><span class="sxs-lookup"><span data-stu-id="fb6b7-500">Start-up error page</span></span>

<span data-ttu-id="fb6b7-501">Если при внутри- или внепроцессном размещении происходит сбой запуска приложения, открываются страницы пользовательских сообщений об ошибках.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-501">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="fb6b7-502">Если модулю ASP.NET Core не удается найти внутри- или внепроцессный обработчик запросов, откроется страница кода состояния *500.0 — ошибка загрузки внутри- или внепроцессного обработчика запросов*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-502">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="fb6b7-503">Если в модели размещения внутри процесса модулю ASP.NET Core не удается запустить приложение, откроется страница кода состояния *500.30 — ошибка запуска*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-503">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="fb6b7-504">Если в модели размещения вне процесса модулю ASP.NET Core не удается запустить серверный процесс или начинается серверный процесс, но ему не удается прослушать настроенный порт, появится страница кода состояния *502.5 — ошибка процесса*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-504">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="fb6b7-505">Чтобы подавить отображение этой странице и вернуться к странице IIS кода состояния 5xx по умолчанию, используйте атрибут `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-505">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="fb6b7-506">Дополнительные сведения о настройке пользовательских сообщений об ошибках см. в разделе [Ошибки HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-506">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="fb6b7-507">Создание и перенаправление журнала</span><span class="sxs-lookup"><span data-stu-id="fb6b7-507">Log creation and redirection</span></span>

<span data-ttu-id="fb6b7-508">Модуль ASP.NET Core перенаправляет выходные потоки консоли stdout и stderr на диск, если заданы атрибуты `stdoutLogEnabled` и `stdoutLogFile` элемента `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-508">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="fb6b7-509">Все папки, указанные в пути `stdoutLogFile`, создаются модулем при создании файла журнала.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-509">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="fb6b7-510">Пул приложений должен иметь доступ на запись в папку, где записываются журналы (используйте атрибут `IIS AppPool\<app_pool_name>` для предоставления разрешения на запись).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-510">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="fb6b7-511">Журналы не выполняют циклический сдвиг, пока не произойдет процесс перезапуска или перезагрузки.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-511">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="fb6b7-512">Администратор несет ответственность за ограничение дискового пространства, которое потребляют журналы.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-512">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="fb6b7-513">Использование журнала stdout рекомендуется только для устранения неполадок при запуске приложений.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-513">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="fb6b7-514">Не используйте журнал stdout для ведения общего журнала приложений.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-514">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="fb6b7-515">Для обычного входа в приложение ASP.NET Core используйте библиотеку ведения журналов, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-515">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="fb6b7-516">Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-516">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="fb6b7-517">При создании файла журнала автоматически добавляются отметка времени и расширение файла.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-517">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="fb6b7-518">Имя файла журнала составляется путем добавления метки времени, идентификатора процесса и расширения файла ( *.log*) к последнему сегменту атрибута пути `stdoutLogFile` (обычно *stdout*) с символами подчеркивания в качестве разделителей.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-518">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="fb6b7-519">Если атрибут пути `stdoutLogFile` заканчивается элементом *stdout*, журнал приложения с идентификатором 1934, созданный 5 февраля 2018 г. в 19:42:32, будет иметь имя *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-519">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="fb6b7-520">Если `stdoutLogEnabled` имеет значение false, возникающие при запуске приложения ошибки записываются и передаются в журнал событий (макс. 30 КБ).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-520">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="fb6b7-521">После запуска все дополнительные журналы удаляются.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-521">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="fb6b7-522">В следующем примере элемент `aspNetCore` настраивает ведение журнала stdout для приложения, размещенного в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-522">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="fb6b7-523">Для локального ведения журнала допустим локальный или общий сетевой путь.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-523">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="fb6b7-524">Убедитесь, что идентификатор пользователя AppPool имеет разрешение на запись по указанному пути.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-524">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="fb6b7-525">Расширенные журналы диагностики</span><span class="sxs-lookup"><span data-stu-id="fb6b7-525">Enhanced diagnostic logs</span></span>

<span data-ttu-id="fb6b7-526">Модуль ASP.NET Core можно настроить. Он позволяет работать с расширенными журналами диагностики.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-526">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="fb6b7-527">Добавьте элемент `<handlerSettings>` в элемент `<aspNetCore>` в файле *web.config*. Задайте параметру `debugLevel` значение `TRACE`, чтобы обеспечить высокую точность диагностических сведений:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-527">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="fb6b7-528">Папки, указанные в пути к значению `<handlerSetting>` (*logs* в приведенном выше примере), не создаются модулем автоматически и должны заранее существовать в развертывании.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-528">Folders in the path provided to the `<handlerSetting>` value (*logs* in the preceding example) aren't created by the module automatically and should pre-exist in the deployment.</span></span> <span data-ttu-id="fb6b7-529">Пул приложений должен иметь доступ на запись в папку, где записываются журналы (используйте атрибут `IIS AppPool\<app_pool_name>` для предоставления разрешения на запись).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-529">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="fb6b7-530">Значения уровня отладки (`debugLevel`) могут включать уровень и расположение.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-530">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="fb6b7-531">Уровни (в порядке возрастания степени детализации):</span><span class="sxs-lookup"><span data-stu-id="fb6b7-531">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="fb6b7-532">ОШИБКА</span><span class="sxs-lookup"><span data-stu-id="fb6b7-532">ERROR</span></span>
* <span data-ttu-id="fb6b7-533">ПРЕДУПРЕЖДЕНИЕ</span><span class="sxs-lookup"><span data-stu-id="fb6b7-533">WARNING</span></span>
* <span data-ttu-id="fb6b7-534">ИНФОРМАЦИЯ</span><span class="sxs-lookup"><span data-stu-id="fb6b7-534">INFO</span></span>
* <span data-ttu-id="fb6b7-535">TRACE</span><span class="sxs-lookup"><span data-stu-id="fb6b7-535">TRACE</span></span>

<span data-ttu-id="fb6b7-536">Расположения (допускаются несколько расположений):</span><span class="sxs-lookup"><span data-stu-id="fb6b7-536">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="fb6b7-537">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="fb6b7-537">CONSOLE</span></span>
* <span data-ttu-id="fb6b7-538">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="fb6b7-538">EVENTLOG</span></span>
* <span data-ttu-id="fb6b7-539">FILE</span><span class="sxs-lookup"><span data-stu-id="fb6b7-539">FILE</span></span>

<span data-ttu-id="fb6b7-540">Параметры обработчика могут быть указаны с помощью переменных среды:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-540">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="fb6b7-541">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Путь к файлу журнала отладки.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-541">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="fb6b7-542">(По умолчанию: *aspnetcore debug.log*)</span><span class="sxs-lookup"><span data-stu-id="fb6b7-542">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="fb6b7-543">`ASPNETCORE_MODULE_DEBUG` &ndash; Параметр уровня отладки.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-543">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="fb6b7-544">**Не** оставляйте ведение журнала отладки включенным в развертывании дольше того времени, которое требуется для устранения проблемы.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-544">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="fb6b7-545">Размер журнала не ограничен.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-545">The size of the log isn't limited.</span></span> <span data-ttu-id="fb6b7-546">Если оставить журнал отладки включенным, он может исчерпать все доступное место на диске и привести к сбою сервера или службы приложений.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-546">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="fb6b7-547">См. пример элемента `aspNetCore` в файле *web.config* в разделе [Конфигурация с помощью файла web.config](#configuration-with-webconfig).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-547">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="fb6b7-548">Конфигурация прокси-сервера использует протокол HTTP и токен связывания</span><span class="sxs-lookup"><span data-stu-id="fb6b7-548">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="fb6b7-549">*Применяется только к размещению вне процесса.*</span><span class="sxs-lookup"><span data-stu-id="fb6b7-549">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="fb6b7-550">Прокси-сервер, созданный между модулем ASP.NET Core и Kestrel, использует протокол HTTP.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-550">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="fb6b7-551">Отсутствует риск перехвата трафика между модулем и Kestrel из расположения на сервере.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-551">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="fb6b7-552">Токен связывания гарантирует, что полученные Kestrel запросы были переданы службами IIS, а не из какого-либо другого источника.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-552">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="fb6b7-553">Этот токен создается и задается модулем в переменную среды (`ASPNETCORE_TOKEN`).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-553">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="fb6b7-554">Он также задается в заголовке (`MS-ASPNETCORE-TOKEN`) каждого запроса, переданного через прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-554">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="fb6b7-555">ПО промежуточного слоя IIS проверяет каждый получаемый запрос, чтобы убедиться, что заголовок с токеном связывания соответствует значению переменной среды.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-555">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="fb6b7-556">Если значения токена не совпадают, запрос заносится в журнал и отклоняется.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-556">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="fb6b7-557">Переменная среды с токеном связывания и трафик между модулем и Kestrel недоступны из расположения на сервере.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-557">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="fb6b7-558">Не зная значение токена связывания, злоумышленник не может отправлять запросы, обходящие проверку в ПО промежуточного слоя IIS.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-558">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="fb6b7-559">Модуль ASP.NET Core с общей конфигурацией IIS</span><span class="sxs-lookup"><span data-stu-id="fb6b7-559">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="fb6b7-560">Программа установки модуля ASP.NET Core запускается с правами учетной записи **TrustedInstaller**.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-560">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="fb6b7-561">Поскольку учетная запись локальной системы не имеет разрешения на изменение пути к общей папке, используемого общей конфигурацией IIS, установщик получает ошибку отказа в доступе при попытке настроить параметры модуля в файле *applicationHost.config* общей папки.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-561">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="fb6b7-562">При использовании общей конфигурации IIS на том же компьютере, где установлены службы IIS, запустите установщик пакета размещения ASP.NET Core с параметром `OPT_NO_SHARED_CONFIG_CHECK` со значением `1`:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-562">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="fb6b7-563">Если путь к общей конфигурации не на том же компьютере, где установлены службы IIS, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-563">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="fb6b7-564">Отключите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-564">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="fb6b7-565">Запустите установщик.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-565">Run the installer.</span></span>
1. <span data-ttu-id="fb6b7-566">Экспортируйте обновленный файл *applicationHost.config* в общую папку.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-566">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="fb6b7-567">Повторно включите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-567">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="fb6b7-568">Версия модуля и журналы установщика хостинга Bundle</span><span class="sxs-lookup"><span data-stu-id="fb6b7-568">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="fb6b7-569">Чтобы определить версию установщика модуля ASP.NET Core, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-569">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="fb6b7-570">В системе размещения перейдите к папке *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-570">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="fb6b7-571">Найдите файл *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-571">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="fb6b7-572">Щелкните правой кнопкой мыши файл и выберите **Свойства** из контекстного меню.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-572">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="fb6b7-573">Выберите вкладку **Сведения**. **Версия файла** и **Версия продукта** дают представление об установленной версии модуля.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-573">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="fb6b7-574">Журналы установщика хостинга Bundle для модуля находятся в папке *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. Этот файл имеет имя *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-574">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="fb6b7-575">Модуль, схемы и расположение файлов конфигурации</span><span class="sxs-lookup"><span data-stu-id="fb6b7-575">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="fb6b7-576">Module</span><span class="sxs-lookup"><span data-stu-id="fb6b7-576">Module</span></span>

<span data-ttu-id="fb6b7-577">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="fb6b7-577">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="fb6b7-578">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fb6b7-578">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="fb6b7-579">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fb6b7-579">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="fb6b7-580">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="fb6b7-580">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="fb6b7-581">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="fb6b7-581">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="fb6b7-582">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="fb6b7-582">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="fb6b7-583">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fb6b7-583">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="fb6b7-584">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fb6b7-584">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="fb6b7-585">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="fb6b7-585">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="fb6b7-586">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="fb6b7-586">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="fb6b7-587">Схема</span><span class="sxs-lookup"><span data-stu-id="fb6b7-587">Schema</span></span>

<span data-ttu-id="fb6b7-588">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="fb6b7-588">**IIS**</span></span>

* <span data-ttu-id="fb6b7-589">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="fb6b7-589">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="fb6b7-590">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="fb6b7-590">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="fb6b7-591">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="fb6b7-591">**IIS Express**</span></span>

* <span data-ttu-id="fb6b7-592">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="fb6b7-592">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="fb6b7-593">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="fb6b7-593">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="fb6b7-594">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="fb6b7-594">Configuration</span></span>

<span data-ttu-id="fb6b7-595">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="fb6b7-595">**IIS**</span></span>

* <span data-ttu-id="fb6b7-596">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="fb6b7-596">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="fb6b7-597">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="fb6b7-597">**IIS Express**</span></span>

* <span data-ttu-id="fb6b7-598">Visual Studio: {КОРЕНЬ ПРИЛОЖЕНИЯ}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="fb6b7-598">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="fb6b7-599">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="fb6b7-599">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="fb6b7-600">Файлы можно найти путем поиска *aspnetcore* в файле *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-600">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="fb6b7-601">Модуль ASP.NET Core имеет собственный модуль IIS, который подключается к конвейеру IIS для переадресации веб-запросов в серверные приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-601">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="fb6b7-602">Поддерживаемые версии Windows:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-602">Supported Windows versions:</span></span>

* <span data-ttu-id="fb6b7-603">Windows 7 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="fb6b7-603">Windows 7 or later</span></span>
* <span data-ttu-id="fb6b7-604">Windows Server 2008 R2 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="fb6b7-604">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="fb6b7-605">Модуль работает только с Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-605">The module only works with Kestrel.</span></span> <span data-ttu-id="fb6b7-606">Модуль несовместим с [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-606">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="fb6b7-607">Так как приложения ASP.NET Core выполняются в процессе, отделенном от рабочего процесса IIS, этот модуль также обрабатывает управление процессами.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-607">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="fb6b7-608">Модуль запускает процесс для приложения ASP.NET Core при поступлении первого запроса и перезапускает приложение при сбое.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-608">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="fb6b7-609">Это, по сути, совпадает с поведением приложений ASP.NET 4.x, выполняемых внутрипроцессно в IIS и управляемых [службой активации процессов Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-609">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="fb6b7-610">На следующей схеме показана связь между IIS, модулем ASP.NET Core и приложением:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-610">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Модуль ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="fb6b7-612">Запросы поступают из Интернета в драйвер HTTP.sys в режиме ядра.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-612">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="fb6b7-613">Драйвер направляет запросы к службам IIS на настроенный порт веб-сайта — обычно 80 (HTTP) или 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-613">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="fb6b7-614">Модуль перенаправляет запросы Kestrel на случайный порт для приложения, отличающийся от порта 80 или 443.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-614">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="fb6b7-615">Модуль задает порт с помощью переменной среды во время запуска, а [ПО промежуточного слоя для интеграции IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) настраивает сервер для прослушивания `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-615">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="fb6b7-616">Выполняются дополнительные проверки, и запросы не из модуля отклоняются.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-616">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="fb6b7-617">Модуль не поддерживает переадресацию по HTTPS, поэтому запросы переадресовываются по протоколу HTTP, даже если были получены IIS по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-617">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="fb6b7-618">После того как Kestrel забирает запрос из модуля, запрос передается в конвейер ПО промежуточного слоя ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-618">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="fb6b7-619">Конвейер ПО промежуточного слоя обрабатывает запрос и передает его в качестве экземпляра `HttpContext` в логику приложения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-619">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="fb6b7-620">ПО промежуточного слоя, добавленное интеграцией IIS, обновляет схему, удаленный IP-адрес и базовый путь для переадресации запроса в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-620">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="fb6b7-621">Отклик приложения передается обратно в службу IIS, которая отправляет его обратно в HTTP-клиент, инициировавший запрос.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-621">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="fb6b7-622">Многие собственные модули, такие как проверка подлинности Windows, остаются активными.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-622">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="fb6b7-623">Дополнительные сведения о модулях IIS, активных с модулем ASP.NET Core, см. в разделе <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-623">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="fb6b7-624">Дополнительные возможности модуля ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="fb6b7-624">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="fb6b7-625">Задание переменных среды для рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-625">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="fb6b7-626">Внесение в журнал выходных данных stdout для хранилища файлов с целью устранения неполадок при запуске.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-626">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="fb6b7-627">Переадресация токенов проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-627">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="fb6b7-628">Как установить и использовать модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fb6b7-628">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="fb6b7-629">Инструкции о том, как установить модуль ASP.NET Core, см. в разделе [Установка пакета размещения .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-629">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="fb6b7-630">Конфигурация с помощью файла web.config</span><span class="sxs-lookup"><span data-stu-id="fb6b7-630">Configuration with web.config</span></span>

<span data-ttu-id="fb6b7-631">Модуль ASP.NET Core настроен с помощью раздела `aspNetCore` узла `system.webServer` файла *web.config* на веб-сайте.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-631">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="fb6b7-632">Следующий файл *web.config* публикуется для [зависимого от платформы развертывания](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) и настраивает модуль ASP.NET Core для обработки запросов к веб-сайту.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-632">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="fb6b7-633">Следующий файл *web.config* опубликован для [автономного развертывания](/dotnet/articles/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-633">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="fb6b7-634">Когда приложение развернуто в [службе приложений Azure](https://azure.microsoft.com/services/app-service/), путь `stdoutLogFile` задан как `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-634">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="fb6b7-635">Путь сохраняет журналы stdout в папке *LogFiles*, расположение которой автоматически создается службой.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-635">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="fb6b7-636">Сведения о конфигурации дочерних приложений IIS см. здесь: <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-636">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="fb6b7-637">Атрибуты элемента aspNetCore</span><span class="sxs-lookup"><span data-stu-id="fb6b7-637">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="fb6b7-638">Атрибут</span><span class="sxs-lookup"><span data-stu-id="fb6b7-638">Attribute</span></span> | <span data-ttu-id="fb6b7-639">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="fb6b7-639">Description</span></span> | <span data-ttu-id="fb6b7-640">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="fb6b7-640">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="fb6b7-641">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-641">Optional string attribute.</span></span></p><p><span data-ttu-id="fb6b7-642">Аргументы для исполняемого файла, указанного в атрибуте **processPath**.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-642">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="fb6b7-643">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-643">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="fb6b7-644">Если значение равно true, страница **502.5 — ошибка процесса** подавляется и страница в файле *web.config* с кодом состояния 502 имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-644">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="fb6b7-645">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-645">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="fb6b7-646">Если значение равно true, маркер безопасности отправляется дочернему процессу, прослушивающему порт %ASPNETCORE_PORT% как заголовок "MS-ASPNETCORE-WINAUTHTOKEN" каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-646">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="fb6b7-647">Этот процесс вызывает CloseHandle по этому маркеру безопасности каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-647">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="fb6b7-648">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-648">Optional integer attribute.</span></span></p><p><span data-ttu-id="fb6b7-649">Указывает число экземпляров процесса, заданное в параметре **processPath**, которое может появиться для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-649">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="fb6b7-650">Параметр `processesPerApplication` не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-650">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="fb6b7-651">Этот атрибут будет удален в будущем выпуске.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-651">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="fb6b7-652">По умолчанию: `1`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-652">Default: `1`</span></span><br><span data-ttu-id="fb6b7-653">Минимум: `1`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-653">Min: `1`</span></span><br><span data-ttu-id="fb6b7-654">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-654">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="fb6b7-655">Обязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-655">Required string attribute.</span></span></p><p><span data-ttu-id="fb6b7-656">Путь к исполняемому файлу, который запускает процесс прослушивания HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-656">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="fb6b7-657">Поддерживаются относительные пути.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-657">Relative paths are supported.</span></span> <span data-ttu-id="fb6b7-658">Если путь начинается с `.`, то начало пути считается относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-658">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="fb6b7-659">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-659">Optional integer attribute.</span></span></p><p><span data-ttu-id="fb6b7-660">Указывает количество сбоев за минуту, которыми может завершиться процесс, указанный в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-660">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="fb6b7-661">Если этот предел превышен, модуль останавливает запуск процесса на оставшуюся часть минуты.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-661">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="fb6b7-662">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-662">Default: `10`</span></span><br><span data-ttu-id="fb6b7-663">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-663">Min: `0`</span></span><br><span data-ttu-id="fb6b7-664">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-664">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="fb6b7-665">Необязательный атрибут timespan.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-665">Optional timespan attribute.</span></span></p><p><span data-ttu-id="fb6b7-666">Указывает продолжительность, на протяжении которой модуль ASP.NET Core ожидает ответа от процесса, прослушивающего порт %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-666">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="fb6b7-667">В версиях модуля ASP.NET Core, поставляемых с выпуском ASP.NET Core 2.1 или новее, атрибут `requestTimeout` указывается в часах, минутах и секундах.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-667">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="fb6b7-668">По умолчанию: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-668">Default: `00:02:00`</span></span><br><span data-ttu-id="fb6b7-669">Минимум: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-669">Min: `00:00:00`</span></span><br><span data-ttu-id="fb6b7-670">Максимум: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-670">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="fb6b7-671">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-671">Optional integer attribute.</span></span></p><p><span data-ttu-id="fb6b7-672">Длительность ожидания модуля в секундах, пока произойдет правильное выключение исполняемого файла при обнаружении файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-672">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="fb6b7-673">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-673">Default: `10`</span></span><br><span data-ttu-id="fb6b7-674">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-674">Min: `0`</span></span><br><span data-ttu-id="fb6b7-675">Максимум: `600`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-675">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="fb6b7-676">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-676">Optional integer attribute.</span></span></p><p><span data-ttu-id="fb6b7-677">Время в секундах, которое модуль ожидает, пока запустится процесс прослушивания порта исполняемого файла.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-677">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="fb6b7-678">Если этот предел превышен, модуль завершает процесс.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-678">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="fb6b7-679">Модуль пытается перезапустить процесс при получении нового запроса и будет продолжать пытаться перезапустить процесс для последующих входящих запросов, если не удается запустить приложение определенное в атрибуте **rapidFailsPerMinute** количество раз за последнюю минуту.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-679">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="fb6b7-680">Значение 0 (ноль) **не** считается бесконечным временем ожидания.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-680">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="fb6b7-681">По умолчанию: `120`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-681">Default: `120`</span></span><br><span data-ttu-id="fb6b7-682">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-682">Min: `0`</span></span><br><span data-ttu-id="fb6b7-683">Максимум: `3600`</span><span class="sxs-lookup"><span data-stu-id="fb6b7-683">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="fb6b7-684">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-684">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="fb6b7-685">Если значение равно true, **stdout** и **stderr** для процесса, указанного в атрибуте **processPath**, перенаправляются к файлу, заданному в атрибуте **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-685">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="fb6b7-686">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-686">Optional string attribute.</span></span></p><p><span data-ttu-id="fb6b7-687">Указывает относительный или абсолютный путь к файлу, для которого регистрируются **stdout** и **stderr** из процесса, указанного в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-687">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="fb6b7-688">Относительные пути задаются относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-688">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="fb6b7-689">Любой путь, начинающийся с `.`, относится к корневому каталогу веб-сайта, а все остальные пути рассматриваются как абсолютные пути.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-689">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="fb6b7-690">Все папки, указанные в пути, должны существовать, чтобы модуль мог создать файл журнала.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-690">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="fb6b7-691">С помощью разделителей подчеркивания метка времени, идентификатор процесса и расширение файла ( *.log*) добавляются к последнему сегменту пути журнала **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-691">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="fb6b7-692">Если в качестве значения задано значение `.\logs\stdout`, например, журнал stdout сохраняется как *stdout_20180205194132_1934.log* в папке *журналов* с датой 5 февраля 2018 г. в 19:41:32 с идентификатором процесса 1934.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-692">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="fb6b7-693">Настройка переменных среды</span><span class="sxs-lookup"><span data-stu-id="fb6b7-693">Setting environment variables</span></span>

<span data-ttu-id="fb6b7-694">Переменные среды для процесса можно указать в атрибуте `processPath`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-694">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="fb6b7-695">Укажите переменную среды с дочерним элементом `<environmentVariable>` элемента коллекции `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-695">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="fb6b7-696">Переменные среды, заданные в этом разделе, конфликтуют с переменными системной среды с такими же именами.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-696">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="fb6b7-697">Если переменная среды задана и в файле *web.config*, и на уровне системы в Windows, значение из файла *web.config* добавляется к значению переменной системной среды (например, `ASPNETCORE_ENVIRONMENT: Development;Development`), которая препятствует запуску приложения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-697">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

<span data-ttu-id="fb6b7-698">В следующем примере устанавливаются две переменные среды.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-698">The following example sets two environment variables.</span></span> <span data-ttu-id="fb6b7-699">`ASPNETCORE_ENVIRONMENT` настраивает среду приложения для `Development`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-699">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="fb6b7-700">Разработчик может временно задать это значение в файле *web.config*, чтобы принудительно загрузить [Страницу исключений для разработчиков](xref:fundamentals/error-handling) при отладке исключения приложения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-700">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="fb6b7-701">`CONFIG_DIR` — пример пользовательской переменной среды, где разработчик написал код, который считывает значение при запуске, чтобы сформировать путь для загрузки файла конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-701">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="fb6b7-702">Установите только переменную среды `ASPNETCORE_ENVIRONMENT` для `Development` на серверах промежуточных процессов и тестирования, которые недоступны для ненадежных сетей, таких как Интернет.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-702">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="fb6b7-703">App_offline.htm</span><span class="sxs-lookup"><span data-stu-id="fb6b7-703">app_offline.htm</span></span>

<span data-ttu-id="fb6b7-704">Если в корневом каталоге приложения обнаружен файл с именем *app_offline.htm*, модуль ASP.NET Core пытается корректно закрыть приложение и прекратить обработку входящих запросов.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-704">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="fb6b7-705">Если приложение по-прежнему выполняется через количество секунд, определенное атрибутом `shutdownTimeLimit`, модуль ASP.NET Core завершает выполняющийся процесс.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-705">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="fb6b7-706">Хотя файл *app_offline.htm* присутствует, модуль ASP.NET Core отвечает на запросы, отправляя назад содержимое файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-706">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="fb6b7-707">Когда *app_offline.htm* файл удаляется, следующий запрос запускает приложение.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-707">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="fb6b7-708">Страница ошибок запуска</span><span class="sxs-lookup"><span data-stu-id="fb6b7-708">Start-up error page</span></span>

<span data-ttu-id="fb6b7-709">Если модулю ASP.NET Core не удается запустить серверный процесс или начинается серверный процесс, но ему не удается прослушать настроенный порт, появится страница кода состояния *502.5 — ошибка процесса*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-709">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="fb6b7-710">Чтобы подавить эту страницу и вернуться к странице IIS кода состояния 502 по умолчанию, используйте атрибут `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-710">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="fb6b7-711">Дополнительные сведения о настройке пользовательских сообщений об ошибках см. в разделе [Ошибки HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-711">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Страница кода состояния "502.5 — ошибка процесса"](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="fb6b7-713">Создание и перенаправление журнала</span><span class="sxs-lookup"><span data-stu-id="fb6b7-713">Log creation and redirection</span></span>

<span data-ttu-id="fb6b7-714">Модуль ASP.NET Core перенаправляет выходные потоки консоли stdout и stderr на диск, если заданы атрибуты `stdoutLogEnabled` и `stdoutLogFile` элемента `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-714">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="fb6b7-715">Все папки, указанные в пути `stdoutLogFile`, создаются модулем при создании файла журнала.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-715">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="fb6b7-716">Пул приложений должен иметь доступ на запись в папку, где записываются журналы (используйте атрибут `IIS AppPool\<app_pool_name>` для предоставления разрешения на запись).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-716">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="fb6b7-717">Журналы не выполняют циклический сдвиг, пока не произойдет процесс перезапуска или перезагрузки.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-717">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="fb6b7-718">Администратор несет ответственность за ограничение дискового пространства, которое потребляют журналы.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-718">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="fb6b7-719">Использование журнала stdout рекомендуется только для устранения неполадок при запуске приложений.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-719">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="fb6b7-720">Не используйте журнал stdout для ведения общего журнала приложений.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-720">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="fb6b7-721">Для обычного входа в приложение ASP.NET Core используйте библиотеку ведения журналов, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-721">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="fb6b7-722">Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-722">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="fb6b7-723">При создании файла журнала автоматически добавляются отметка времени и расширение файла.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-723">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="fb6b7-724">Имя файла журнала составляется путем добавления метки времени, идентификатора процесса и расширения файла ( *.log*) к последнему сегменту атрибута пути `stdoutLogFile` (обычно *stdout*) с символами подчеркивания в качестве разделителей.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-724">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="fb6b7-725">Если атрибут пути `stdoutLogFile` заканчивается элементом *stdout*, журнал приложения с идентификатором 1934, созданный 5 февраля 2018 г. в 19:42:32, будет иметь имя *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-725">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="fb6b7-726">В следующем примере элемент `aspNetCore` настраивает ведение журнала stdout для приложения, размещенного в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-726">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="fb6b7-727">Для локального ведения журнала допустим локальный или общий сетевой путь.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-727">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="fb6b7-728">Убедитесь, что идентификатор пользователя AppPool имеет разрешение на запись по указанному пути.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-728">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

<span data-ttu-id="fb6b7-729">Папки, указанные в пути к значению `<handlerSetting>` (*logs* в приведенном выше примере), не создаются модулем автоматически и должны заранее существовать в развертывании.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-729">Folders in the path provided to the `<handlerSetting>` value (*logs* in the preceding example) aren't created by the module automatically and should pre-exist in the deployment.</span></span> <span data-ttu-id="fb6b7-730">Пул приложений должен иметь доступ на запись в папку, где записываются журналы (используйте атрибут `IIS AppPool\<app_pool_name>` для предоставления разрешения на запись).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-730">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="fb6b7-731">См. пример элемента `aspNetCore` в файле *web.config* в разделе [Конфигурация с помощью файла web.config](#configuration-with-webconfig).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-731">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="fb6b7-732">Конфигурация прокси-сервера использует протокол HTTP и токен связывания</span><span class="sxs-lookup"><span data-stu-id="fb6b7-732">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="fb6b7-733">Прокси-сервер, созданный между модулем ASP.NET Core и Kestrel, использует протокол HTTP.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-733">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="fb6b7-734">Отсутствует риск перехвата трафика между модулем и Kestrel из расположения на сервере.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-734">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="fb6b7-735">Токен связывания гарантирует, что полученные Kestrel запросы были переданы службами IIS, а не из какого-либо другого источника.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-735">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="fb6b7-736">Этот токен создается и задается модулем в переменную среды (`ASPNETCORE_TOKEN`).</span><span class="sxs-lookup"><span data-stu-id="fb6b7-736">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="fb6b7-737">Он также задается в заголовке (`MS-ASPNETCORE-TOKEN`) каждого запроса, переданного через прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-737">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="fb6b7-738">ПО промежуточного слоя IIS проверяет каждый получаемый запрос, чтобы убедиться, что заголовок с токеном связывания соответствует значению переменной среды.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-738">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="fb6b7-739">Если значения токена не совпадают, запрос заносится в журнал и отклоняется.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-739">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="fb6b7-740">Переменная среды с токеном связывания и трафик между модулем и Kestrel недоступны из расположения на сервере.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-740">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="fb6b7-741">Не зная значение токена связывания, злоумышленник не может отправлять запросы, обходящие проверку в ПО промежуточного слоя IIS.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-741">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="fb6b7-742">Модуль ASP.NET Core с общей конфигурацией IIS</span><span class="sxs-lookup"><span data-stu-id="fb6b7-742">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="fb6b7-743">Программа установки модуля ASP.NET Core запускается с правами учетной записи **TrustedInstaller**.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-743">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="fb6b7-744">Поскольку учетная запись локальной системы не имеет разрешения на изменение пути к общей папке, используемого общей конфигурацией IIS, установщик получает ошибку отказа в доступе при попытке настроить параметры модуля в файле *applicationHost.config* общей папки.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-744">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="fb6b7-745">При использовании общей конфигурации IIS выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-745">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="fb6b7-746">Отключите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-746">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="fb6b7-747">Запустите установщик.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-747">Run the installer.</span></span>
1. <span data-ttu-id="fb6b7-748">Экспортируйте обновленный файл *applicationHost.config* в общую папку.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-748">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="fb6b7-749">Повторно включите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-749">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="fb6b7-750">Версия модуля и журналы установщика хостинга Bundle</span><span class="sxs-lookup"><span data-stu-id="fb6b7-750">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="fb6b7-751">Чтобы определить версию установщика модуля ASP.NET Core, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-751">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="fb6b7-752">В системе размещения перейдите к папке *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-752">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="fb6b7-753">Найдите файл *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-753">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="fb6b7-754">Щелкните правой кнопкой мыши файл и выберите **Свойства** из контекстного меню.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-754">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="fb6b7-755">Выберите вкладку **Сведения**. **Версия файла** и **Версия продукта** дают представление об установленной версии модуля.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-755">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="fb6b7-756">Журналы установщика хостинга Bundle для модуля находятся в папке *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. Этот файл имеет имя *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-756">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="fb6b7-757">Модуль, схемы и расположение файлов конфигурации</span><span class="sxs-lookup"><span data-stu-id="fb6b7-757">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="fb6b7-758">Module</span><span class="sxs-lookup"><span data-stu-id="fb6b7-758">Module</span></span>

<span data-ttu-id="fb6b7-759">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="fb6b7-759">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="fb6b7-760">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fb6b7-760">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="fb6b7-761">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fb6b7-761">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="fb6b7-762">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="fb6b7-762">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="fb6b7-763">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fb6b7-763">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="fb6b7-764">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fb6b7-764">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="fb6b7-765">Схема</span><span class="sxs-lookup"><span data-stu-id="fb6b7-765">Schema</span></span>

<span data-ttu-id="fb6b7-766">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="fb6b7-766">**IIS**</span></span>

* <span data-ttu-id="fb6b7-767">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="fb6b7-767">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="fb6b7-768">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="fb6b7-768">**IIS Express**</span></span>

* <span data-ttu-id="fb6b7-769">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="fb6b7-769">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="fb6b7-770">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="fb6b7-770">Configuration</span></span>

<span data-ttu-id="fb6b7-771">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="fb6b7-771">**IIS**</span></span>

* <span data-ttu-id="fb6b7-772">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="fb6b7-772">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="fb6b7-773">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="fb6b7-773">**IIS Express**</span></span>

* <span data-ttu-id="fb6b7-774">Visual Studio: {КОРЕНЬ ПРИЛОЖЕНИЯ}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="fb6b7-774">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="fb6b7-775">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="fb6b7-775">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="fb6b7-776">Файлы можно найти путем поиска *aspnetcore* в файле *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="fb6b7-776">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="fb6b7-777">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="fb6b7-777">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="fb6b7-778">Репозиторий GitHub для модуля ASP.NET Core (справочные материалы)</span><span class="sxs-lookup"><span data-stu-id="fb6b7-778">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
