---
title: Справочник по конфигурации модуля ASP.NET Core
author: guardrex
description: Сведения о настройке модуля ASP.NET Core для размещения приложений ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 0ad73d89ffa3a8a3625c6e248efaad821e1b4d0a
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121561"
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="1579f-103">Справочник по конфигурации модуля ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1579f-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="1579f-104">Авторы [Люк Латэм](https://github.com/guardrex) (Luke Latham), [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson), [Сураб Ширхатти](https://twitter.com/sshirhatti) (Sourabh Shirhatti) и [Джастин Коталик](https://github.com/jkotalik) (Justin Kotalik)</span><span class="sxs-lookup"><span data-stu-id="1579f-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="1579f-105">Этот документ содержит инструкции о том, как настроить модуль ASP.NET Core для размещения приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1579f-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="1579f-106">Для ознакомления с модулем ASP.NET Core и инструкциями по установке см. [Обзор модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1579f-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a><span data-ttu-id="1579f-107">Модель размещения</span><span class="sxs-lookup"><span data-stu-id="1579f-107">Hosting model</span></span>

<span data-ttu-id="1579f-108">Для приложений, выполняющихся на .NET Core 2.2 или более поздней версии, модуль поддерживает модель внутрипроцессного размещения для повышения производительности по сравнению с размещением на обратном прокси-сервере (вне процесса).</span><span class="sxs-lookup"><span data-stu-id="1579f-108">For apps running on .NET Core 2.2 or later, the module supports an in-process hosting model for improved performance compared to reverse-proxy (out-of-process) hosting.</span></span> <span data-ttu-id="1579f-109">Дополнительные сведения см. в разделе <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span><span class="sxs-lookup"><span data-stu-id="1579f-109">For more information, see <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span></span>

<span data-ttu-id="1579f-110">Внутрипроцессное размещение необходимо явно выбирать в существующих приложениях, но в шаблонах [dotnet new](/dotnet/core/tools/dotnet-new) оно включено по умолчанию для всех сценариев IIS и IIS Express.</span><span class="sxs-lookup"><span data-stu-id="1579f-110">In-procsess hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="1579f-111">Чтобы настроить приложение для внутрипроцессного размещения, добавьте свойство `<AspNetCoreHostingModel>` в файл проекта приложения (например, *MyApp.csproj*) со значением `InProcess` (размещение вне процесса имеет значение `outofprocess`):</span><span class="sxs-lookup"><span data-stu-id="1579f-111">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file (for example, *MyApp.csproj*) with a value of `InProcess` (out-of-process hosting is set with `outofprocess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="1579f-112">При внутрипроцессном размещении применимы следующие характеристики:</span><span class="sxs-lookup"><span data-stu-id="1579f-112">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="1579f-113">Вместо сервера [Kestrel](xref:fundamentals/servers/kestrel) используется HTTP-сервер IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="1579f-113">IIS HTTP Server (`IISHttpServer`) is used instead of the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="1579f-114">HTTP-сервер IIS (`IISHttpServer`) — это альтернативная реализация <xref:Microsoft.AspNetCore.Hosting.Server.IServer>, которая преобразует собственные запросы IIS в управляемые запросы ASP.NET Core для обработки приложением.</span><span class="sxs-lookup"><span data-stu-id="1579f-114">IIS HTTP Server (`IISHttpServer`) is another <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation that converts IIS native requests into ASP.NET Core managed requests for processing by the app.</span></span>

* <span data-ttu-id="1579f-115">[Атрибут requestTimeout](#attributes-of-the-aspnetcore-element) не применяется к внутрипроцессному размещению.</span><span class="sxs-lookup"><span data-stu-id="1579f-115">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="1579f-116">Совместное использование пула приложений среди приложений не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="1579f-116">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="1579f-117">Используйте один пул приложений для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="1579f-117">Use one app pool per app.</span></span>

* <span data-ttu-id="1579f-118">При использовании [веб-развертывания](/iis/publish/using-web-deploy/introduction-to-web-deploy) или размещении [файла app_offline.htm в развертывании](xref:host-and-deploy/iis/index#locked-deployment-files) вручную приложение может не завершить работу сразу при наличии открытого соединения.</span><span class="sxs-lookup"><span data-stu-id="1579f-118">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="1579f-119">Например, подключение websocket может задерживать завершение работы приложения.</span><span class="sxs-lookup"><span data-stu-id="1579f-119">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="1579f-120">Архитектура (разрядность) приложения и установленная среда выполнения (x64 или x86) должны соответствовать архитектуре пула приложений.</span><span class="sxs-lookup"><span data-stu-id="1579f-120">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="1579f-121">Если вы настраиваете узел приложения вручную с помощью `WebHostBuilder` (не используя [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) и приложение запускается напрямую на сервере Kestrel (с локальным размещением), вызывайте `UseKestrel` перед вызовом `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="1579f-121">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="1579f-122">Если изменить порядок, узел не запустится.</span><span class="sxs-lookup"><span data-stu-id="1579f-122">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="1579f-123">Обнаружены отключения клиентов.</span><span class="sxs-lookup"><span data-stu-id="1579f-123">Client disconnects are detected.</span></span> <span data-ttu-id="1579f-124">При отключении клиента происходит отмена токена отмены [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*).</span><span class="sxs-lookup"><span data-stu-id="1579f-124">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="1579f-125"><xref:System.IO.Directory.GetCurrentDirectory*> возвращает рабочий каталог процесса, запущенного службами IIS, а не каталог приложения (например, *C:\Windows\System32\inetsrv* для *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="1579f-125"><xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="1579f-126">Пример кода, который задает текущий каталог приложения, см. в разделе [Класс CurrentDirectoryHelpers](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="1579f-126">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="1579f-127">Вызовите метод `SetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="1579f-127">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="1579f-128">Последующие вызовы <xref:System.IO.Directory.GetCurrentDirectory*> возвращают каталог приложения.</span><span class="sxs-lookup"><span data-stu-id="1579f-128">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="1579f-129">Изменения модели размещения</span><span class="sxs-lookup"><span data-stu-id="1579f-129">Hosting model changes</span></span>

<span data-ttu-id="1579f-130">Если параметр `hostingModel` изменяется в файле *web.config* (как описано в разделе [Конфигурация с помощью web.config](#configuration-with-webconfig)), модуль перезапускает рабочий процесс для служб IIS.</span><span class="sxs-lookup"><span data-stu-id="1579f-130">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="1579f-131">Для IIS Express модуль не перезапускает рабочий процесс, а запускает нормальное завершение работы текущего процесса IIS Express.</span><span class="sxs-lookup"><span data-stu-id="1579f-131">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="1579f-132">Следующий запрос для приложения порождает новый процесс IIS Express.</span><span class="sxs-lookup"><span data-stu-id="1579f-132">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="1579f-133">Имя процесса</span><span class="sxs-lookup"><span data-stu-id="1579f-133">Process name</span></span>

<span data-ttu-id="1579f-134">`Process.GetCurrentProcess().ProcessName` сообщает `w3wp`/`iisexpress` (внутри процесса) или `dotnet` (вне процесса).</span><span class="sxs-lookup"><span data-stu-id="1579f-134">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

## <a name="configuration-with-webconfig"></a><span data-ttu-id="1579f-135">Конфигурация с помощью файла web.config</span><span class="sxs-lookup"><span data-stu-id="1579f-135">Configuration with web.config</span></span>

<span data-ttu-id="1579f-136">Модуль ASP.NET Core настроен с помощью раздела `aspNetCore` узла `system.webServer` файла *web.config* на веб-сайте.</span><span class="sxs-lookup"><span data-stu-id="1579f-136">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="1579f-137">Следующий файл *web.config* публикуется для [зависимого от платформы развертывания](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) и настраивает модуль ASP.NET Core для обработки запросов к веб-сайту.</span><span class="sxs-lookup"><span data-stu-id="1579f-137">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="1579f-138">Следующий файл *web.config* опубликован для [автономного развертывания](/dotnet/articles/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="1579f-138">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="1579f-139">Значение `false` свойства <xref:System.Configuration.SectionInformation.InheritInChildApplications*> указывает, что параметры, заданные в элементе [\<расположение>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location), не наследуются приложениями, которые находятся во вложенном каталоге приложения.</span><span class="sxs-lookup"><span data-stu-id="1579f-139">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

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

<span data-ttu-id="1579f-140">Когда приложение развернуто в [службе приложений Azure](https://azure.microsoft.com/services/app-service/), путь `stdoutLogFile` задан как `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="1579f-140">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="1579f-141">Путь сохраняет журналы stdout в папке *LogFiles*, расположение которой автоматически создается службой.</span><span class="sxs-lookup"><span data-stu-id="1579f-141">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="1579f-142">Сведения о конфигурации дочерних приложений IIS см. здесь: <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="1579f-142">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="1579f-143">Атрибуты элемента aspNetCore</span><span class="sxs-lookup"><span data-stu-id="1579f-143">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="1579f-144">Атрибут</span><span class="sxs-lookup"><span data-stu-id="1579f-144">Attribute</span></span> | <span data-ttu-id="1579f-145">Описание</span><span class="sxs-lookup"><span data-stu-id="1579f-145">Description</span></span> | <span data-ttu-id="1579f-146">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="1579f-146">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="1579f-147">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-147">Optional string attribute.</span></span></p><p><span data-ttu-id="1579f-148">Аргументы для исполняемого файла, указанного в атрибуте **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1579f-148">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="1579f-149">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-149">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1579f-150">Если значение равно true, страница **502.5 — ошибка процесса** подавляется и страница в файле *web.config* с кодом состояния 502 имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="1579f-150">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="1579f-151">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-151">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1579f-152">Если значение равно true, маркер безопасности отправляется дочернему процессу, прослушивающему порт %ASPNETCORE_PORT% как заголовок "MS-ASPNETCORE-WINAUTHTOKEN" каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="1579f-152">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="1579f-153">Этот процесс вызывает CloseHandle по этому маркеру безопасности каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="1579f-153">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="1579f-154">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-154">Optional string attribute.</span></span></p><p><span data-ttu-id="1579f-155">Указывает модель размещения — внутри процесса (`InProcess`) или вне процесса (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="1579f-155">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="1579f-156">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-156">Optional integer attribute.</span></span></p><p><span data-ttu-id="1579f-157">Указывает число экземпляров процесса, заданное в параметре **processPath**, которое может появиться для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="1579f-157">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="1579f-158">&dagger;Для внутрипроцессного размещения существует ограничение: `1`.</span><span class="sxs-lookup"><span data-stu-id="1579f-158">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="1579f-159">По умолчанию: `1`</span><span class="sxs-lookup"><span data-stu-id="1579f-159">Default: `1`</span></span><br><span data-ttu-id="1579f-160">Минимум: `1`</span><span class="sxs-lookup"><span data-stu-id="1579f-160">Min: `1`</span></span><br><span data-ttu-id="1579f-161">Максимум: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="1579f-161">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="1579f-162">Обязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-162">Required string attribute.</span></span></p><p><span data-ttu-id="1579f-163">Путь к исполняемому файлу, который запускает процесс прослушивания HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="1579f-163">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="1579f-164">Поддерживаются относительные пути.</span><span class="sxs-lookup"><span data-stu-id="1579f-164">Relative paths are supported.</span></span> <span data-ttu-id="1579f-165">Если путь начинается с `.`, то начало пути считается относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="1579f-165">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="1579f-166">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-166">Optional integer attribute.</span></span></p><p><span data-ttu-id="1579f-167">Указывает количество сбоев за минуту, которыми может завершиться процесс, указанный в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1579f-167">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="1579f-168">Если этот предел превышен, модуль останавливает запуск процесса на оставшуюся часть минуты.</span><span class="sxs-lookup"><span data-stu-id="1579f-168">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="1579f-169">Не поддерживается для внутрипроцессного размещения.</span><span class="sxs-lookup"><span data-stu-id="1579f-169">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="1579f-170">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="1579f-170">Default: `10`</span></span><br><span data-ttu-id="1579f-171">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="1579f-171">Min: `0`</span></span><br><span data-ttu-id="1579f-172">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="1579f-172">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="1579f-173">Необязательный атрибут timespan.</span><span class="sxs-lookup"><span data-stu-id="1579f-173">Optional timespan attribute.</span></span></p><p><span data-ttu-id="1579f-174">Указывает продолжительность, на протяжении которой модуль ASP.NET Core ожидает ответа от процесса, прослушивающего порт %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="1579f-174">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="1579f-175">В версиях модуля ASP.NET Core, поставляемых с выпуском ASP.NET Core 2.1 или новее, атрибут `requestTimeout` указывается в часах, минутах и секундах.</span><span class="sxs-lookup"><span data-stu-id="1579f-175">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="1579f-176">Не применяется к внутрипроцессному размещению.</span><span class="sxs-lookup"><span data-stu-id="1579f-176">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="1579f-177">Для внутрипроцессного размещения модуль ожидает, пока приложение не обработает запрос.</span><span class="sxs-lookup"><span data-stu-id="1579f-177">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="1579f-178">По умолчанию: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="1579f-178">Default: `00:02:00`</span></span><br><span data-ttu-id="1579f-179">Минимум: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="1579f-179">Min: `00:00:00`</span></span><br><span data-ttu-id="1579f-180">Максимум: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="1579f-180">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="1579f-181">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-181">Optional integer attribute.</span></span></p><p><span data-ttu-id="1579f-182">Длительность ожидания модуля в секундах, пока произойдет правильное выключение исполняемого файла при обнаружении файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="1579f-182">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="1579f-183">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="1579f-183">Default: `10`</span></span><br><span data-ttu-id="1579f-184">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="1579f-184">Min: `0`</span></span><br><span data-ttu-id="1579f-185">Максимум: `600`</span><span class="sxs-lookup"><span data-stu-id="1579f-185">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="1579f-186">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-186">Optional integer attribute.</span></span></p><p><span data-ttu-id="1579f-187">Время в секундах, которое модуль ожидает, пока запустится процесс прослушивания порта исполняемого файла.</span><span class="sxs-lookup"><span data-stu-id="1579f-187">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="1579f-188">Если этот предел превышен, модуль завершает процесс.</span><span class="sxs-lookup"><span data-stu-id="1579f-188">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="1579f-189">Модуль пытается перезапустить процесс при получении нового запроса и будет продолжать пытаться перезапустить процесс для последующих входящих запросов, если не удается запустить приложение определенное в атрибуте **rapidFailsPerMinute** количество раз за последнюю минуту.</span><span class="sxs-lookup"><span data-stu-id="1579f-189">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="1579f-190">Значение 0 (ноль) **не** считается бесконечным временем ожидания.</span><span class="sxs-lookup"><span data-stu-id="1579f-190">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="1579f-191">По умолчанию: `120`</span><span class="sxs-lookup"><span data-stu-id="1579f-191">Default: `120`</span></span><br><span data-ttu-id="1579f-192">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="1579f-192">Min: `0`</span></span><br><span data-ttu-id="1579f-193">Максимум: `3600`</span><span class="sxs-lookup"><span data-stu-id="1579f-193">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="1579f-194">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-194">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1579f-195">Если значение равно true, **stdout** и **stderr** для процесса, указанного в атрибуте **processPath**, перенаправляются к файлу, заданному в атрибуте **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1579f-195">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="1579f-196">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-196">Optional string attribute.</span></span></p><p><span data-ttu-id="1579f-197">Указывает относительный или абсолютный путь к файлу, для которого регистрируются **stdout** и **stderr** из процесса, указанного в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1579f-197">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="1579f-198">Относительные пути задаются относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="1579f-198">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="1579f-199">Любой путь, начинающийся с `.`, относится к корневому каталогу веб-сайта, а все остальные пути рассматриваются как абсолютные пути.</span><span class="sxs-lookup"><span data-stu-id="1579f-199">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="1579f-200">Все папки, указанные в пути, должны существовать, чтобы модуль мог создать файл журнала.</span><span class="sxs-lookup"><span data-stu-id="1579f-200">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="1579f-201">С помощью разделителей подчеркивания метка времени, идентификатор процесса и расширение файла (*.log*) добавляются к последнему сегменту пути журнала **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1579f-201">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="1579f-202">Если в качестве значения задано значение `.\logs\stdout`, например, журнал stdout сохраняется как *stdout_20180205194132_1934.log* в папке *журналов* с датой 5 февраля 2018 г. в 19:41:32 с идентификатором процесса 1934.</span><span class="sxs-lookup"><span data-stu-id="1579f-202">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="1579f-203">Атрибут</span><span class="sxs-lookup"><span data-stu-id="1579f-203">Attribute</span></span> | <span data-ttu-id="1579f-204">Описание:</span><span class="sxs-lookup"><span data-stu-id="1579f-204">Description</span></span> | <span data-ttu-id="1579f-205">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="1579f-205">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="1579f-206">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-206">Optional string attribute.</span></span></p><p><span data-ttu-id="1579f-207">Аргументы для исполняемого файла, указанного в атрибуте **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1579f-207">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="1579f-208">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-208">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1579f-209">Если значение равно true, страница **502.5 — ошибка процесса** подавляется и страница в файле *web.config* с кодом состояния 502 имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="1579f-209">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="1579f-210">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-210">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1579f-211">Если значение равно true, маркер безопасности отправляется дочернему процессу, прослушивающему порт %ASPNETCORE_PORT% как заголовок "MS-ASPNETCORE-WINAUTHTOKEN" каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="1579f-211">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="1579f-212">Этот процесс вызывает CloseHandle по этому маркеру безопасности каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="1579f-212">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="1579f-213">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-213">Optional integer attribute.</span></span></p><p><span data-ttu-id="1579f-214">Указывает число экземпляров процесса, заданное в параметре **processPath**, которое может появиться для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="1579f-214">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="1579f-215">По умолчанию: `1`</span><span class="sxs-lookup"><span data-stu-id="1579f-215">Default: `1`</span></span><br><span data-ttu-id="1579f-216">Минимум: `1`</span><span class="sxs-lookup"><span data-stu-id="1579f-216">Min: `1`</span></span><br><span data-ttu-id="1579f-217">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="1579f-217">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="1579f-218">Обязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-218">Required string attribute.</span></span></p><p><span data-ttu-id="1579f-219">Путь к исполняемому файлу, который запускает процесс прослушивания HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="1579f-219">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="1579f-220">Поддерживаются относительные пути.</span><span class="sxs-lookup"><span data-stu-id="1579f-220">Relative paths are supported.</span></span> <span data-ttu-id="1579f-221">Если путь начинается с `.`, то начало пути считается относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="1579f-221">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="1579f-222">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-222">Optional integer attribute.</span></span></p><p><span data-ttu-id="1579f-223">Указывает количество сбоев за минуту, которыми может завершиться процесс, указанный в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1579f-223">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="1579f-224">Если этот предел превышен, модуль останавливает запуск процесса на оставшуюся часть минуты.</span><span class="sxs-lookup"><span data-stu-id="1579f-224">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="1579f-225">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="1579f-225">Default: `10`</span></span><br><span data-ttu-id="1579f-226">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="1579f-226">Min: `0`</span></span><br><span data-ttu-id="1579f-227">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="1579f-227">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="1579f-228">Необязательный атрибут timespan.</span><span class="sxs-lookup"><span data-stu-id="1579f-228">Optional timespan attribute.</span></span></p><p><span data-ttu-id="1579f-229">Указывает продолжительность, на протяжении которой модуль ASP.NET Core ожидает ответа от процесса, прослушивающего порт %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="1579f-229">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="1579f-230">В версиях модуля ASP.NET Core, поставляемых с выпуском ASP.NET Core 2.1 или новее, атрибут `requestTimeout` указывается в часах, минутах и секундах.</span><span class="sxs-lookup"><span data-stu-id="1579f-230">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="1579f-231">По умолчанию: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="1579f-231">Default: `00:02:00`</span></span><br><span data-ttu-id="1579f-232">Минимум: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="1579f-232">Min: `00:00:00`</span></span><br><span data-ttu-id="1579f-233">Максимум: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="1579f-233">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="1579f-234">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-234">Optional integer attribute.</span></span></p><p><span data-ttu-id="1579f-235">Длительность ожидания модуля в секундах, пока произойдет правильное выключение исполняемого файла при обнаружении файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="1579f-235">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="1579f-236">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="1579f-236">Default: `10`</span></span><br><span data-ttu-id="1579f-237">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="1579f-237">Min: `0`</span></span><br><span data-ttu-id="1579f-238">Максимум: `600`</span><span class="sxs-lookup"><span data-stu-id="1579f-238">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="1579f-239">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-239">Optional integer attribute.</span></span></p><p><span data-ttu-id="1579f-240">Время в секундах, которое модуль ожидает, пока запустится процесс прослушивания порта исполняемого файла.</span><span class="sxs-lookup"><span data-stu-id="1579f-240">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="1579f-241">Если этот предел превышен, модуль завершает процесс.</span><span class="sxs-lookup"><span data-stu-id="1579f-241">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="1579f-242">Модуль пытается перезапустить процесс при получении нового запроса и будет продолжать пытаться перезапустить процесс для последующих входящих запросов, если не удается запустить приложение определенное в атрибуте **rapidFailsPerMinute** количество раз за последнюю минуту.</span><span class="sxs-lookup"><span data-stu-id="1579f-242">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="1579f-243">Значение 0 (ноль) **не** считается бесконечным временем ожидания.</span><span class="sxs-lookup"><span data-stu-id="1579f-243">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="1579f-244">По умолчанию: `120`</span><span class="sxs-lookup"><span data-stu-id="1579f-244">Default: `120`</span></span><br><span data-ttu-id="1579f-245">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="1579f-245">Min: `0`</span></span><br><span data-ttu-id="1579f-246">Максимум: `3600`</span><span class="sxs-lookup"><span data-stu-id="1579f-246">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="1579f-247">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-247">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1579f-248">Если значение равно true, **stdout** и **stderr** для процесса, указанного в атрибуте **processPath**, перенаправляются к файлу, заданному в атрибуте **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1579f-248">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="1579f-249">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-249">Optional string attribute.</span></span></p><p><span data-ttu-id="1579f-250">Указывает относительный или абсолютный путь к файлу, для которого регистрируются **stdout** и **stderr** из процесса, указанного в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1579f-250">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="1579f-251">Относительные пути задаются относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="1579f-251">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="1579f-252">Любой путь, начинающийся с `.`, относится к корневому каталогу веб-сайта, а все остальные пути рассматриваются как абсолютные пути.</span><span class="sxs-lookup"><span data-stu-id="1579f-252">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="1579f-253">Все папки, указанные в пути, должны существовать, чтобы модуль мог создать файл журнала.</span><span class="sxs-lookup"><span data-stu-id="1579f-253">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="1579f-254">С помощью разделителей подчеркивания метка времени, идентификатор процесса и расширение файла (*.log*) добавляются к последнему сегменту пути журнала **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1579f-254">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="1579f-255">Если в качестве значения задано значение `.\logs\stdout`, например, журнал stdout сохраняется как *stdout_20180205194132_1934.log* в папке *журналов* с датой 5 февраля 2018 г. в 19:41:32 с идентификатором процесса 1934.</span><span class="sxs-lookup"><span data-stu-id="1579f-255">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="1579f-256">Атрибут</span><span class="sxs-lookup"><span data-stu-id="1579f-256">Attribute</span></span> | <span data-ttu-id="1579f-257">Описание:</span><span class="sxs-lookup"><span data-stu-id="1579f-257">Description</span></span> | <span data-ttu-id="1579f-258">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="1579f-258">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="1579f-259">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-259">Optional string attribute.</span></span></p><p><span data-ttu-id="1579f-260">Аргументы для исполняемого файла, указанного в атрибуте **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1579f-260">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="1579f-261">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-261">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1579f-262">Если значение равно true, страница **502.5 — ошибка процесса** подавляется и страница в файле *web.config* с кодом состояния 502 имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="1579f-262">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="1579f-263">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-263">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1579f-264">Если значение равно true, маркер безопасности отправляется дочернему процессу, прослушивающему порт %ASPNETCORE_PORT% как заголовок "MS-ASPNETCORE-WINAUTHTOKEN" каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="1579f-264">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="1579f-265">Этот процесс вызывает CloseHandle по этому маркеру безопасности каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="1579f-265">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="1579f-266">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-266">Optional integer attribute.</span></span></p><p><span data-ttu-id="1579f-267">Указывает число экземпляров процесса, заданное в параметре **processPath**, которое может появиться для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="1579f-267">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="1579f-268">По умолчанию: `1`</span><span class="sxs-lookup"><span data-stu-id="1579f-268">Default: `1`</span></span><br><span data-ttu-id="1579f-269">Минимум: `1`</span><span class="sxs-lookup"><span data-stu-id="1579f-269">Min: `1`</span></span><br><span data-ttu-id="1579f-270">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="1579f-270">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="1579f-271">Обязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-271">Required string attribute.</span></span></p><p><span data-ttu-id="1579f-272">Путь к исполняемому файлу, который запускает процесс прослушивания HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="1579f-272">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="1579f-273">Поддерживаются относительные пути.</span><span class="sxs-lookup"><span data-stu-id="1579f-273">Relative paths are supported.</span></span> <span data-ttu-id="1579f-274">Если путь начинается с `.`, то начало пути считается относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="1579f-274">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="1579f-275">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-275">Optional integer attribute.</span></span></p><p><span data-ttu-id="1579f-276">Указывает количество сбоев за минуту, которыми может завершиться процесс, указанный в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1579f-276">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="1579f-277">Если этот предел превышен, модуль останавливает запуск процесса на оставшуюся часть минуты.</span><span class="sxs-lookup"><span data-stu-id="1579f-277">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="1579f-278">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="1579f-278">Default: `10`</span></span><br><span data-ttu-id="1579f-279">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="1579f-279">Min: `0`</span></span><br><span data-ttu-id="1579f-280">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="1579f-280">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="1579f-281">Необязательный атрибут timespan.</span><span class="sxs-lookup"><span data-stu-id="1579f-281">Optional timespan attribute.</span></span></p><p><span data-ttu-id="1579f-282">Указывает продолжительность, на протяжении которой модуль ASP.NET Core ожидает ответа от процесса, прослушивающего порт %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="1579f-282">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="1579f-283">В версиях модуля ASP.NET Core, поставляемых вместе с выпуском ASP.NET Core 2.0 или более ранними версиями, атрибут `requestTimeout` должен был быть задан в целых минутах (в противном случае для него по умолчанию задано значение 2 минуты).</span><span class="sxs-lookup"><span data-stu-id="1579f-283">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="1579f-284">По умолчанию: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="1579f-284">Default: `00:02:00`</span></span><br><span data-ttu-id="1579f-285">Минимум: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="1579f-285">Min: `00:00:00`</span></span><br><span data-ttu-id="1579f-286">Максимум: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="1579f-286">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="1579f-287">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-287">Optional integer attribute.</span></span></p><p><span data-ttu-id="1579f-288">Длительность ожидания модуля в секундах, пока произойдет правильное выключение исполняемого файла при обнаружении файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="1579f-288">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="1579f-289">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="1579f-289">Default: `10`</span></span><br><span data-ttu-id="1579f-290">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="1579f-290">Min: `0`</span></span><br><span data-ttu-id="1579f-291">Максимум: `600`</span><span class="sxs-lookup"><span data-stu-id="1579f-291">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="1579f-292">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-292">Optional integer attribute.</span></span></p><p><span data-ttu-id="1579f-293">Время в секундах, которое модуль ожидает, пока запустится процесс прослушивания порта исполняемого файла.</span><span class="sxs-lookup"><span data-stu-id="1579f-293">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="1579f-294">Если этот предел превышен, модуль завершает процесс.</span><span class="sxs-lookup"><span data-stu-id="1579f-294">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="1579f-295">Модуль пытается перезапустить процесс при получении нового запроса и будет продолжать пытаться перезапустить процесс для последующих входящих запросов, если не удается запустить приложение определенное в атрибуте **rapidFailsPerMinute** количество раз за последнюю минуту.</span><span class="sxs-lookup"><span data-stu-id="1579f-295">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="1579f-296">По умолчанию: `120`</span><span class="sxs-lookup"><span data-stu-id="1579f-296">Default: `120`</span></span><br><span data-ttu-id="1579f-297">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="1579f-297">Min: `0`</span></span><br><span data-ttu-id="1579f-298">Максимум: `3600`</span><span class="sxs-lookup"><span data-stu-id="1579f-298">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="1579f-299">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-299">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1579f-300">Если значение равно true, **stdout** и **stderr** для процесса, указанного в атрибуте **processPath**, перенаправляются к файлу, заданному в атрибуте **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1579f-300">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="1579f-301">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="1579f-301">Optional string attribute.</span></span></p><p><span data-ttu-id="1579f-302">Указывает относительный или абсолютный путь к файлу, для которого регистрируются **stdout** и **stderr** из процесса, указанного в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1579f-302">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="1579f-303">Относительные пути задаются относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="1579f-303">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="1579f-304">Любой путь, начинающийся с `.`, относится к корневому каталогу веб-сайта, а все остальные пути рассматриваются как абсолютные пути.</span><span class="sxs-lookup"><span data-stu-id="1579f-304">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="1579f-305">Все папки, указанные в пути, должны существовать, чтобы модуль мог создать файл журнала.</span><span class="sxs-lookup"><span data-stu-id="1579f-305">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="1579f-306">С помощью разделителей подчеркивания метка времени, идентификатор процесса и расширение файла (*.log*) добавляются к последнему сегменту пути журнала **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1579f-306">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="1579f-307">Если в качестве значения задано значение `.\logs\stdout`, например, журнал stdout сохраняется как *stdout_20180205194132_1934.log* в папке *журналов* с датой 5 февраля 2018 г. в 19:41:32 с идентификатором процесса 1934.</span><span class="sxs-lookup"><span data-stu-id="1579f-307">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="1579f-308">Настройка переменных среды</span><span class="sxs-lookup"><span data-stu-id="1579f-308">Setting environment variables</span></span>

<span data-ttu-id="1579f-309">Переменные среды для процесса можно указать в атрибуте `processPath`.</span><span class="sxs-lookup"><span data-stu-id="1579f-309">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="1579f-310">Укажите переменную среды с дочерним элементом `environmentVariable` элемента коллекции `environmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="1579f-310">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="1579f-311">Переменные среды, установленные в этом разделе, имеют приоритет над переменными системной среды.</span><span class="sxs-lookup"><span data-stu-id="1579f-311">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="1579f-312">В следующем примере устанавливаются две переменные среды.</span><span class="sxs-lookup"><span data-stu-id="1579f-312">The following example sets two environment variables.</span></span> <span data-ttu-id="1579f-313">`ASPNETCORE_ENVIRONMENT` настраивает среду приложения для `Development`.</span><span class="sxs-lookup"><span data-stu-id="1579f-313">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="1579f-314">Разработчик может временно задать это значение в файле *web.config*, чтобы принудительно загрузить [Страницу исключений для разработчиков](xref:fundamentals/error-handling) при отладке исключения приложения.</span><span class="sxs-lookup"><span data-stu-id="1579f-314">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="1579f-315">`CONFIG_DIR` — пример пользовательской переменной среды, где разработчик написал код, который считывает значение при запуске, чтобы сформировать путь для загрузки файла конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="1579f-315">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="1579f-316">Установите только переменную среды `ASPNETCORE_ENVIRONMENT` для `Development` на серверах промежуточных процессов и тестирования, которые недоступны для ненадежных сетей, таких как Интернет.</span><span class="sxs-lookup"><span data-stu-id="1579f-316">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="1579f-317">App_offline.htm</span><span class="sxs-lookup"><span data-stu-id="1579f-317">app_offline.htm</span></span>

<span data-ttu-id="1579f-318">Если в корневом каталоге приложения обнаружен файл с именем *app_offline.htm*, модуль ASP.NET Core пытается корректно закрыть приложение и прекратить обработку входящих запросов.</span><span class="sxs-lookup"><span data-stu-id="1579f-318">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="1579f-319">Если приложение по-прежнему выполняется через количество секунд, определенное атрибутом `shutdownTimeLimit`, модуль ASP.NET Core завершает выполняющийся процесс.</span><span class="sxs-lookup"><span data-stu-id="1579f-319">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="1579f-320">Хотя файл *app_offline.htm* присутствует, модуль ASP.NET Core отвечает на запросы, отправляя назад содержимое файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="1579f-320">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="1579f-321">Когда *app_offline.htm* файл удаляется, следующий запрос запускает приложение.</span><span class="sxs-lookup"><span data-stu-id="1579f-321">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1579f-322">При использовании модели размещения вне процесса приложение может не завершать работу немедленно при наличии открытого соединения.</span><span class="sxs-lookup"><span data-stu-id="1579f-322">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="1579f-323">Например, подключение websocket может задерживать завершение работы приложения.</span><span class="sxs-lookup"><span data-stu-id="1579f-323">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="1579f-324">Страница ошибок запуска</span><span class="sxs-lookup"><span data-stu-id="1579f-324">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1579f-325">Если при внутри- или внепроцессном размещении происходит сбой запуска приложения, открываются страницы пользовательских сообщений об ошибках.</span><span class="sxs-lookup"><span data-stu-id="1579f-325">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="1579f-326">Если модулю ASP.NET Core не удается найти внутри- или внепроцессный обработчик запросов, откроется страница кода состояния *500.0 — ошибка загрузки внутри- или внепроцессного обработчика запросов*.</span><span class="sxs-lookup"><span data-stu-id="1579f-326">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="1579f-327">Если в модели размещения внутри процесса модулю ASP.NET Core не удается запустить приложение, откроется страница кода состояния *500.30 — ошибка запуска*.</span><span class="sxs-lookup"><span data-stu-id="1579f-327">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="1579f-328">Если в модели размещения вне процесса модулю ASP.NET Core не удается запустить серверный процесс или начинается серверный процесс, но ему не удается прослушать настроенный порт, появится страница кода состояния *502.5 — ошибка процесса*.</span><span class="sxs-lookup"><span data-stu-id="1579f-328">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="1579f-329">Чтобы подавить отображение этой странице и вернуться к странице IIS кода состояния 5xx по умолчанию, используйте атрибут `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="1579f-329">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="1579f-330">Дополнительные сведения о настройке пользовательских сообщений об ошибках см. в разделе [Ошибки HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="1579f-330">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="1579f-331">Если модулю ASP.NET Core не удается запустить серверный процесс или начинается серверный процесс, но ему не удается прослушать настроенный порт, появится страница кода состояния *502.5 — ошибка процесса*.</span><span class="sxs-lookup"><span data-stu-id="1579f-331">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="1579f-332">Чтобы подавить эту страницу и вернуться к странице IIS кода состояния 502 по умолчанию, используйте атрибут `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="1579f-332">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="1579f-333">Дополнительные сведения о настройке пользовательских сообщений об ошибках см. в разделе [Ошибки HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="1579f-333">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Страница кода состояния "502.5 — ошибка процесса"](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="1579f-335">Создание и перенаправление журнала</span><span class="sxs-lookup"><span data-stu-id="1579f-335">Log creation and redirection</span></span>

<span data-ttu-id="1579f-336">Модуль ASP.NET Core перенаправляет выходные потоки консоли stdout и stderr на диск, если заданы атрибуты `stdoutLogEnabled` и `stdoutLogFile` элемента `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="1579f-336">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="1579f-337">Чтобы модуль мог создать файл журнала, все папки в пути `stdoutLogFile` должны существовать.</span><span class="sxs-lookup"><span data-stu-id="1579f-337">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="1579f-338">Пул приложений должен иметь доступ на запись в папку, где записываются журналы (используйте атрибут `IIS AppPool\<app_pool_name>` для предоставления разрешения на запись).</span><span class="sxs-lookup"><span data-stu-id="1579f-338">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="1579f-339">Журналы не выполняют циклический сдвиг, пока не произойдет процесс перезапуска или перезагрузки.</span><span class="sxs-lookup"><span data-stu-id="1579f-339">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="1579f-340">Администратор несет ответственность за ограничение дискового пространства, которое потребляют журналы.</span><span class="sxs-lookup"><span data-stu-id="1579f-340">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="1579f-341">Использование журнала stdout рекомендуется только для устранения неполадок при запуске приложений.</span><span class="sxs-lookup"><span data-stu-id="1579f-341">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="1579f-342">Не используйте журнал stdout для ведения общего журнала приложений.</span><span class="sxs-lookup"><span data-stu-id="1579f-342">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="1579f-343">Для обычного входа в приложение ASP.NET Core используйте библиотеку ведения журналов, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов.</span><span class="sxs-lookup"><span data-stu-id="1579f-343">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="1579f-344">Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="1579f-344">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="1579f-345">При создании файла журнала автоматически добавляются отметка времени и расширение файла.</span><span class="sxs-lookup"><span data-stu-id="1579f-345">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="1579f-346">Имя файла журнала составляется путем добавления метки времени, идентификатора процесса и расширения файла (*.log*) к последнему сегменту атрибута пути `stdoutLogFile` (обычно *stdout*) с символами подчеркивания в качестве разделителей.</span><span class="sxs-lookup"><span data-stu-id="1579f-346">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="1579f-347">Если атрибут пути `stdoutLogFile` заканчивается элементом *stdout*, журнал приложения с идентификатором 1934, созданный 5 февраля 2018 г. в 19:42:32, будет иметь имя *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="1579f-347">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1579f-348">Если `stdoutLogEnabled` имеет значение false, возникающие при запуске приложения ошибки записываются и передаются в журнал событий (макс. 30 КБ).</span><span class="sxs-lookup"><span data-stu-id="1579f-348">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="1579f-349">После запуска все дополнительные журналы удаляются.</span><span class="sxs-lookup"><span data-stu-id="1579f-349">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="1579f-350">В следующем примере элемент `aspNetCore` настраивает ведение журнала stdout для приложения, размещенного в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="1579f-350">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="1579f-351">Для локального ведения журнала допустим локальный или общий сетевой путь.</span><span class="sxs-lookup"><span data-stu-id="1579f-351">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="1579f-352">Убедитесь, что идентификатор пользователя AppPool имеет разрешение на запись по указанному пути.</span><span class="sxs-lookup"><span data-stu-id="1579f-352">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="1579f-353">Расширенные журналы диагностики</span><span class="sxs-lookup"><span data-stu-id="1579f-353">Enhanced diagnostic logs</span></span>

<span data-ttu-id="1579f-354">Модуль ASP.NET Core предоставляет возможности настройки, позволяющие работать с расширенными журналами диагностики.</span><span class="sxs-lookup"><span data-stu-id="1579f-354">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="1579f-355">Добавьте элемент `<handlerSettings>` в элемент `<aspNetCore>` в файле *web.config*. Задайте параметру `debugLevel` значение `TRACE`, чтобы обеспечить высокую точность диагностических сведений:</span><span class="sxs-lookup"><span data-stu-id="1579f-355">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="1579f-356">Значения уровня отладки (`debugLevel`) могут включать уровень и расположение.</span><span class="sxs-lookup"><span data-stu-id="1579f-356">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="1579f-357">Уровни (в порядке возрастания степени детализации):</span><span class="sxs-lookup"><span data-stu-id="1579f-357">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="1579f-358">ОШИБКА</span><span class="sxs-lookup"><span data-stu-id="1579f-358">ERROR</span></span>
* <span data-ttu-id="1579f-359">ПРЕДУПРЕЖДЕНИЕ</span><span class="sxs-lookup"><span data-stu-id="1579f-359">WARNING</span></span>
* <span data-ttu-id="1579f-360">ИНФОРМАЦИЯ</span><span class="sxs-lookup"><span data-stu-id="1579f-360">INFO</span></span>
* <span data-ttu-id="1579f-361">TRACE</span><span class="sxs-lookup"><span data-stu-id="1579f-361">TRACE</span></span>

<span data-ttu-id="1579f-362">Расположения (допускаются несколько расположений):</span><span class="sxs-lookup"><span data-stu-id="1579f-362">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="1579f-363">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="1579f-363">CONSOLE</span></span>
* <span data-ttu-id="1579f-364">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="1579f-364">EVENTLOG</span></span>
* <span data-ttu-id="1579f-365">ФАЙЛ</span><span class="sxs-lookup"><span data-stu-id="1579f-365">FILE</span></span>

<span data-ttu-id="1579f-366">Параметры обработчика могут быть указаны с помощью переменных среды:</span><span class="sxs-lookup"><span data-stu-id="1579f-366">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="1579f-367">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Путь к файлу журнала отладки.</span><span class="sxs-lookup"><span data-stu-id="1579f-367">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="1579f-368">(По умолчанию: *aspnetcore debug.log*)</span><span class="sxs-lookup"><span data-stu-id="1579f-368">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="1579f-369">`ASPNETCORE_MODULE_DEBUG` &ndash; Параметр уровня отладки.</span><span class="sxs-lookup"><span data-stu-id="1579f-369">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="1579f-370">**Не** оставляйте ведение журнала отладки включенным в развертывании дольше того времени, которое требуется для устранения проблемы.</span><span class="sxs-lookup"><span data-stu-id="1579f-370">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="1579f-371">Размер журнала не ограничен.</span><span class="sxs-lookup"><span data-stu-id="1579f-371">The size of the log isn't limited.</span></span> <span data-ttu-id="1579f-372">Если оставить журнал отладки включенным, он может исчерпать все доступное место на диске и привести к сбою сервера или службы приложений.</span><span class="sxs-lookup"><span data-stu-id="1579f-372">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="1579f-373">См. пример элемента `aspNetCore` в файле *web.config* в разделе [Конфигурация с помощью файла web.config](#configuration-with-webconfig).</span><span class="sxs-lookup"><span data-stu-id="1579f-373">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="1579f-374">Конфигурация прокси-сервера использует протокол HTTP и токен связывания</span><span class="sxs-lookup"><span data-stu-id="1579f-374">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1579f-375">*Применяется только к размещению вне процесса.*</span><span class="sxs-lookup"><span data-stu-id="1579f-375">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="1579f-376">Прокси-сервер, созданный между модулем ASP.NET Core и Kestrel, использует протокол HTTP.</span><span class="sxs-lookup"><span data-stu-id="1579f-376">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="1579f-377">Использование HTTP позволяет оптимизировать производительность, так как трафик между модулем и Kestrel передается на петлевой адрес в сетевом интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="1579f-377">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="1579f-378">Отсутствует риск перехвата трафика между модулем и Kestrel из расположения на сервере.</span><span class="sxs-lookup"><span data-stu-id="1579f-378">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="1579f-379">Токен связывания гарантирует, что полученные Kestrel запросы были переданы службами IIS, а не из какого-либо другого источника.</span><span class="sxs-lookup"><span data-stu-id="1579f-379">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="1579f-380">Этот токен создается и задается модулем в переменную среды (`ASPNETCORE_TOKEN`).</span><span class="sxs-lookup"><span data-stu-id="1579f-380">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="1579f-381">Он также задается в заголовке (`MSAspNetCoreToken`) каждого запроса, переданного через прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="1579f-381">The pairing token is also set into a header (`MSAspNetCoreToken`) on every proxied request.</span></span> <span data-ttu-id="1579f-382">ПО промежуточного слоя IIS проверяет каждый получаемый запрос, чтобы убедиться, что заголовок с токеном связывания соответствует значению переменной среды.</span><span class="sxs-lookup"><span data-stu-id="1579f-382">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="1579f-383">Если значения токена не совпадают, запрос заносится в журнал и отклоняется.</span><span class="sxs-lookup"><span data-stu-id="1579f-383">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="1579f-384">Переменная среды с токеном связывания и трафик между модулем и Kestrel недоступны из расположения на сервере.</span><span class="sxs-lookup"><span data-stu-id="1579f-384">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="1579f-385">Не зная значение токена связывания, злоумышленник не может отправлять запросы, обходящие проверку в ПО промежуточного слоя IIS.</span><span class="sxs-lookup"><span data-stu-id="1579f-385">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="1579f-386">Модуль ASP.NET Core с общей конфигурацией IIS</span><span class="sxs-lookup"><span data-stu-id="1579f-386">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="1579f-387">Программа установки модуля ASP.NET Core запускается с правами **системной** учетной записи.</span><span class="sxs-lookup"><span data-stu-id="1579f-387">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="1579f-388">Поскольку учетная запись локальной системы не имеет разрешения на изменение пути к общей папке, используемого общей конфигурацией IIS, установщик получает ошибку отказа в доступе при попытке настроить параметры модуля в файле *applicationHost.config* общей папки.</span><span class="sxs-lookup"><span data-stu-id="1579f-388">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="1579f-389">При использовании общей конфигурации IIS выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="1579f-389">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="1579f-390">Отключите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="1579f-390">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="1579f-391">Запустите установщик.</span><span class="sxs-lookup"><span data-stu-id="1579f-391">Run the installer.</span></span>
1. <span data-ttu-id="1579f-392">Экспортируйте обновленный файл *applicationHost.config* в общую папку.</span><span class="sxs-lookup"><span data-stu-id="1579f-392">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="1579f-393">Повторно включите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="1579f-393">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="1579f-394">Версия модуля и журналы установщика хостинга Bundle</span><span class="sxs-lookup"><span data-stu-id="1579f-394">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="1579f-395">Чтобы определить версию установщика модуля ASP.NET Core, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="1579f-395">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="1579f-396">В системе размещения перейдите к папке *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="1579f-396">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="1579f-397">Найдите файл *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="1579f-397">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="1579f-398">Щелкните правой кнопкой мыши файл и выберите **Свойства** из контекстного меню.</span><span class="sxs-lookup"><span data-stu-id="1579f-398">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="1579f-399">Выберите вкладку **Сведения**. **Версия файла** и **Версия продукта** дают представление об установленной версии модуля.</span><span class="sxs-lookup"><span data-stu-id="1579f-399">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="1579f-400">Журналы установщика хостинга Bundle для модуля находятся в папке *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. Этот файл имеет имя *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="1579f-400">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="1579f-401">Модуль, схемы и расположение файлов конфигурации</span><span class="sxs-lookup"><span data-stu-id="1579f-401">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="1579f-402">Module</span><span class="sxs-lookup"><span data-stu-id="1579f-402">Module</span></span>

<span data-ttu-id="1579f-403">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="1579f-403">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="1579f-404">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="1579f-404">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="1579f-405">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="1579f-405">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="1579f-406">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="1579f-406">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="1579f-407">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="1579f-407">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="1579f-408">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="1579f-408">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="1579f-409">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="1579f-409">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="1579f-410">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="1579f-410">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="1579f-411">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="1579f-411">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="1579f-412">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="1579f-412">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="1579f-413">Схема</span><span class="sxs-lookup"><span data-stu-id="1579f-413">Schema</span></span>

<span data-ttu-id="1579f-414">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="1579f-414">**IIS**</span></span>

   * <span data-ttu-id="1579f-415">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="1579f-415">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="1579f-416">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="1579f-416">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="1579f-417">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="1579f-417">**IIS Express**</span></span>

   * <span data-ttu-id="1579f-418">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="1579f-418">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="1579f-419">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="1579f-419">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="1579f-420">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="1579f-420">Configuration</span></span>

<span data-ttu-id="1579f-421">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="1579f-421">**IIS**</span></span>

   * <span data-ttu-id="1579f-422">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="1579f-422">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="1579f-423">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="1579f-423">**IIS Express**</span></span>

   * <span data-ttu-id="1579f-424">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="1579f-424">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span></span>

<span data-ttu-id="1579f-425">Файлы можно найти путем поиска *aspnetcore* в файле *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="1579f-425">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>
