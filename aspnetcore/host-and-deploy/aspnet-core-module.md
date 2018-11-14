---
title: Справочник по конфигурации модуля ASP.NET Core
author: guardrex
description: Сведения о настройке модуля ASP.NET Core для размещения приложений ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: ca86b1548c7c28a64fd391617b2e8290c1c264cf
ms.sourcegitcommit: 09affee3d234cb27ea6fe33bc113b79e68900d22
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191364"
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="14573-103">Справочник по конфигурации модуля ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="14573-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="14573-104">Авторы [Люк Латэм](https://github.com/guardrex) (Luke Latham), [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson), [Сураб Ширхатти](https://twitter.com/sshirhatti) (Sourabh Shirhatti) и [Джастин Коталик](https://github.com/jkotalik) (Justin Kotalik)</span><span class="sxs-lookup"><span data-stu-id="14573-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="14573-105">Этот документ содержит инструкции о том, как настроить модуль ASP.NET Core для размещения приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="14573-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="14573-106">Для ознакомления с модулем ASP.NET Core и инструкциями по установке см. [Обзор модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="14573-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a><span data-ttu-id="14573-107">Модель размещения</span><span class="sxs-lookup"><span data-stu-id="14573-107">Hosting model</span></span>

<span data-ttu-id="14573-108">Для приложений, выполняющихся на .NET Core 2.2 или более поздней версии, модуль поддерживает модель внутрипроцессного размещения для повышения производительности по сравнению с размещением на обратном прокси-сервере (вне процесса).</span><span class="sxs-lookup"><span data-stu-id="14573-108">For apps running on .NET Core 2.2 or later, the module supports an in-process hosting model for improved performance compared to reverse-proxy (out-of-process) hosting.</span></span> <span data-ttu-id="14573-109">Дополнительные сведения см. в разделе <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span><span class="sxs-lookup"><span data-stu-id="14573-109">For more information, see <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span></span>

<span data-ttu-id="14573-110">Внутрипроцессное размещение необходимо явно выбирать в существующих приложениях, но в шаблонах [dotnet new](/dotnet/core/tools/dotnet-new) оно включено по умолчанию для всех сценариев IIS и IIS Express.</span><span class="sxs-lookup"><span data-stu-id="14573-110">In-procsess hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="14573-111">Чтобы настроить приложение для внутрипроцессного размещения, добавьте свойство `<AspNetCoreHostingModel>` в файл проекта приложения (например, *MyApp.csproj*) со значением `inprocess` (размещение вне процесса имеет значение `outofprocess`):</span><span class="sxs-lookup"><span data-stu-id="14573-111">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file (for example, *MyApp.csproj*) with a value of `inprocess` (out-of-process hosting is set with `outofprocess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>inprocess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="14573-112">При внутрипроцессном размещении применимы следующие характеристики:</span><span class="sxs-lookup"><span data-stu-id="14573-112">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="14573-113">[Сервер Kestrel](xref:fundamentals/servers/kestrel) не используется.</span><span class="sxs-lookup"><span data-stu-id="14573-113">The [Kestrel server](xref:fundamentals/servers/kestrel) isn't used.</span></span> <span data-ttu-id="14573-114">Пользовательская реализация <xref:Microsoft.AspNetCore.Hosting.Server.IServer> `IISHttpServer` выступает в качестве сервера приложения.</span><span class="sxs-lookup"><span data-stu-id="14573-114">A custom <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation, `IISHttpServer` acts as the app's server.</span></span>

* <span data-ttu-id="14573-115">[Атрибут requestTimeout](#attributes-of-the-aspnetcore-element) не применяется к внутрипроцессному размещению.</span><span class="sxs-lookup"><span data-stu-id="14573-115">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="14573-116">Совместное использование пула приложений среди приложений не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="14573-116">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="14573-117">Используйте один пул приложений для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="14573-117">Use one app pool per app.</span></span>

* <span data-ttu-id="14573-118">При использовании [веб-развертывания](/iis/publish/using-web-deploy/introduction-to-web-deploy) или размещении [файла app_offline.htm в развертывании](xref:host-and-deploy/iis/index#locked-deployment-files) вручную приложение может не завершить работу сразу при наличии открытого соединения.</span><span class="sxs-lookup"><span data-stu-id="14573-118">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="14573-119">Например, подключение websocket может задерживать завершение работы приложения.</span><span class="sxs-lookup"><span data-stu-id="14573-119">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="14573-120">Архитектура (разрядность) приложения и установленная среда выполнения (x64 или x86) должны соответствовать архитектуре пула приложений.</span><span class="sxs-lookup"><span data-stu-id="14573-120">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="14573-121">Если вы настраиваете узел приложения вручную с помощью `WebHostBuilder` (не используя [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) и приложение запускается напрямую на сервере Kestrel (с локальным размещением), вызывайте `UseKestrel` перед вызовом `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="14573-121">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="14573-122">Если изменить порядок, узел не запустится.</span><span class="sxs-lookup"><span data-stu-id="14573-122">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="14573-123">Обнаружены отключения клиентов.</span><span class="sxs-lookup"><span data-stu-id="14573-123">Client disconnects are detected.</span></span> <span data-ttu-id="14573-124">При отключении клиента происходит отмена токена отмены [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*).</span><span class="sxs-lookup"><span data-stu-id="14573-124">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="14573-125">`Directory.GetCurrentDirectory()` возвращает рабочий каталог процесса, запущенного службами IIS, а не каталог приложения (например, *C:\Windows\System32\inetsrv* для *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="14573-125">`Directory.GetCurrentDirectory()` returns the worker directory of the process started by IIS rather than the application directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="14573-126">Изменения модели размещения</span><span class="sxs-lookup"><span data-stu-id="14573-126">Hosting model changes</span></span>

<span data-ttu-id="14573-127">Если параметр `hostingModel` изменяется в файле *web.config* (как описано в разделе [Конфигурация с помощью web.config](#configuration-with-webconfig)), модуль перезапускает рабочий процесс для служб IIS.</span><span class="sxs-lookup"><span data-stu-id="14573-127">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="14573-128">Для IIS Express модуль не перезапускает рабочий процесс, а запускает нормальное завершение работы текущего процесса IIS Express.</span><span class="sxs-lookup"><span data-stu-id="14573-128">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="14573-129">Следующий запрос для приложения порождает новый процесс IIS Express.</span><span class="sxs-lookup"><span data-stu-id="14573-129">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="14573-130">Имя процесса</span><span class="sxs-lookup"><span data-stu-id="14573-130">Process name</span></span>

<span data-ttu-id="14573-131">`Process.GetCurrentProcess().ProcessName` сообщает `w3wp`/`iisexpress` (внутри процесса) или `dotnet` (вне процесса).</span><span class="sxs-lookup"><span data-stu-id="14573-131">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

## <a name="configuration-with-webconfig"></a><span data-ttu-id="14573-132">Конфигурация с помощью файла web.config</span><span class="sxs-lookup"><span data-stu-id="14573-132">Configuration with web.config</span></span>

<span data-ttu-id="14573-133">Модуль ASP.NET Core настроен с помощью раздела `aspNetCore` узла `system.webServer` файла *web.config* на веб-сайте.</span><span class="sxs-lookup"><span data-stu-id="14573-133">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="14573-134">Следующий файл *web.config* публикуется для [зависимого от платформы развертывания](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) и настраивает модуль ASP.NET Core для обработки запросов к веб-сайту.</span><span class="sxs-lookup"><span data-stu-id="14573-134">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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
                  hostingModel="inprocess" />
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

<span data-ttu-id="14573-135">Следующий файл *web.config* опубликован для [автономного развертывания](/dotnet/articles/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="14573-135">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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
                  hostingModel="inprocess" />
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
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="14573-136">Когда приложение развернуто в [службе приложений Azure](https://azure.microsoft.com/services/app-service/), путь `stdoutLogFile` задан как `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="14573-136">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="14573-137">Путь сохраняет журналы stdout в папке *LogFiles*, расположение которой автоматически создается службой.</span><span class="sxs-lookup"><span data-stu-id="14573-137">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="14573-138">Важное примечание, относящееся к конфигурации файла *web.config* для дочерних приложений, см. в разделе [Конфигурация дочерних приложений](xref:host-and-deploy/iis/index#sub-application-configuration).</span><span class="sxs-lookup"><span data-stu-id="14573-138">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="14573-139">Атрибуты элемента aspNetCore</span><span class="sxs-lookup"><span data-stu-id="14573-139">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="14573-140">Атрибут</span><span class="sxs-lookup"><span data-stu-id="14573-140">Attribute</span></span> | <span data-ttu-id="14573-141">Описание:</span><span class="sxs-lookup"><span data-stu-id="14573-141">Description</span></span> | <span data-ttu-id="14573-142">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="14573-142">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="14573-143">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-143">Optional string attribute.</span></span></p><p><span data-ttu-id="14573-144">Аргументы для исполняемого файла, указанного в атрибуте **processPath**.</span><span class="sxs-lookup"><span data-stu-id="14573-144">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="14573-145">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-145">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="14573-146">Если значение равно true, страница **502.5 — ошибка процесса** подавляется и страница в файле *web.config* с кодом состояния 502 имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="14573-146">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="14573-147">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-147">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="14573-148">Если значение равно true, маркер безопасности отправляется дочернему процессу, прослушивающему порт %ASPNETCORE_PORT% как заголовок "MS-ASPNETCORE-WINAUTHTOKEN" каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="14573-148">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="14573-149">Этот процесс вызывает CloseHandle по этому маркеру безопасности каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="14573-149">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="14573-150">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-150">Optional string attribute.</span></span></p><p><span data-ttu-id="14573-151">Указывает модель размещения — внутри процесса (`inprocess`) или вне процесса (`outofprocess`).</span><span class="sxs-lookup"><span data-stu-id="14573-151">Specifies the hosting model as in-process (`inprocess`) or out-of-process (`outofprocess`).</span></span></p> | `outofprocess` |
| `processesPerApplication` | <p><span data-ttu-id="14573-152">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-152">Optional integer attribute.</span></span></p><p><span data-ttu-id="14573-153">Указывает число экземпляров процесса, заданное в параметре **processPath**, которое может появиться для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="14573-153">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="14573-154">&dagger;Для внутрипроцессного размещения существует ограничение: `1`.</span><span class="sxs-lookup"><span data-stu-id="14573-154">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="14573-155">По умолчанию: `1`</span><span class="sxs-lookup"><span data-stu-id="14573-155">Default: `1`</span></span><br><span data-ttu-id="14573-156">Минимум: `1`</span><span class="sxs-lookup"><span data-stu-id="14573-156">Min: `1`</span></span><br><span data-ttu-id="14573-157">Максимум: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="14573-157">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="14573-158">Обязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-158">Required string attribute.</span></span></p><p><span data-ttu-id="14573-159">Путь к исполняемому файлу, который запускает процесс прослушивания HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="14573-159">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="14573-160">Поддерживаются относительные пути.</span><span class="sxs-lookup"><span data-stu-id="14573-160">Relative paths are supported.</span></span> <span data-ttu-id="14573-161">Если путь начинается с `.`, то начало пути считается относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="14573-161">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="14573-162">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-162">Optional integer attribute.</span></span></p><p><span data-ttu-id="14573-163">Указывает количество сбоев за минуту, которыми может завершиться процесс, указанный в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="14573-163">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="14573-164">Если этот предел превышен, модуль останавливает запуск процесса на оставшуюся часть минуты.</span><span class="sxs-lookup"><span data-stu-id="14573-164">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="14573-165">Не поддерживается для внутрипроцессного размещения.</span><span class="sxs-lookup"><span data-stu-id="14573-165">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="14573-166">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="14573-166">Default: `10`</span></span><br><span data-ttu-id="14573-167">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="14573-167">Min: `0`</span></span><br><span data-ttu-id="14573-168">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="14573-168">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="14573-169">Необязательный атрибут timespan.</span><span class="sxs-lookup"><span data-stu-id="14573-169">Optional timespan attribute.</span></span></p><p><span data-ttu-id="14573-170">Указывает продолжительность, на протяжении которой модуль ASP.NET Core ожидает ответа от процесса, прослушивающего порт %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="14573-170">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="14573-171">В версиях модуля ASP.NET Core, поставляемых с выпуском ASP.NET Core 2.1 или новее, атрибут `requestTimeout` указывается в часах, минутах и секундах.</span><span class="sxs-lookup"><span data-stu-id="14573-171">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="14573-172">Не применяется к внутрипроцессному размещению.</span><span class="sxs-lookup"><span data-stu-id="14573-172">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="14573-173">Для внутрипроцессного размещения модуль ожидает, пока приложение не обработает запрос.</span><span class="sxs-lookup"><span data-stu-id="14573-173">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="14573-174">По умолчанию: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="14573-174">Default: `00:02:00`</span></span><br><span data-ttu-id="14573-175">Минимум: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="14573-175">Min: `00:00:00`</span></span><br><span data-ttu-id="14573-176">Максимум: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="14573-176">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="14573-177">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-177">Optional integer attribute.</span></span></p><p><span data-ttu-id="14573-178">Длительность ожидания модуля в секундах, пока произойдет правильное выключение исполняемого файла при обнаружении файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="14573-178">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="14573-179">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="14573-179">Default: `10`</span></span><br><span data-ttu-id="14573-180">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="14573-180">Min: `0`</span></span><br><span data-ttu-id="14573-181">Максимум: `600`</span><span class="sxs-lookup"><span data-stu-id="14573-181">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="14573-182">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-182">Optional integer attribute.</span></span></p><p><span data-ttu-id="14573-183">Время в секундах, которое модуль ожидает, пока запустится процесс прослушивания порта исполняемого файла.</span><span class="sxs-lookup"><span data-stu-id="14573-183">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="14573-184">Если этот предел превышен, модуль завершает процесс.</span><span class="sxs-lookup"><span data-stu-id="14573-184">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="14573-185">Модуль пытается перезапустить процесс при получении нового запроса и будет продолжать пытаться перезапустить процесс для последующих входящих запросов, если не удается запустить приложение определенное в атрибуте **rapidFailsPerMinute** количество раз за последнюю минуту.</span><span class="sxs-lookup"><span data-stu-id="14573-185">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="14573-186">Значение 0 (ноль) **не** считается бесконечным временем ожидания.</span><span class="sxs-lookup"><span data-stu-id="14573-186">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="14573-187">По умолчанию: `120`</span><span class="sxs-lookup"><span data-stu-id="14573-187">Default: `120`</span></span><br><span data-ttu-id="14573-188">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="14573-188">Min: `0`</span></span><br><span data-ttu-id="14573-189">Максимум: `3600`</span><span class="sxs-lookup"><span data-stu-id="14573-189">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="14573-190">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-190">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="14573-191">Если значение равно true, **stdout** и **stderr** для процесса, указанного в атрибуте **processPath**, перенаправляются к файлу, заданному в атрибуте **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="14573-191">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="14573-192">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-192">Optional string attribute.</span></span></p><p><span data-ttu-id="14573-193">Указывает относительный или абсолютный путь к файлу, для которого регистрируются **stdout** и **stderr** из процесса, указанного в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="14573-193">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="14573-194">Относительные пути задаются относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="14573-194">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="14573-195">Любой путь, начинающийся с `.`, относится к корневому каталогу веб-сайта, а все остальные пути рассматриваются как абсолютные пути.</span><span class="sxs-lookup"><span data-stu-id="14573-195">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="14573-196">Все папки, указанные в пути, должны существовать, чтобы модуль мог создать файл журнала.</span><span class="sxs-lookup"><span data-stu-id="14573-196">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="14573-197">С помощью разделителей подчеркивания метка времени, идентификатор процесса и расширение файла (*.log*) добавляются к последнему сегменту пути журнала **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="14573-197">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="14573-198">Если в качестве значения задано значение `.\logs\stdout`, например, журнал stdout сохраняется как *stdout_20180205194132_1934.log* в папке *журналов* с датой 5 февраля 2018 г. в 19:41:32 с идентификатором процесса 1934.</span><span class="sxs-lookup"><span data-stu-id="14573-198">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="14573-199">Атрибут</span><span class="sxs-lookup"><span data-stu-id="14573-199">Attribute</span></span> | <span data-ttu-id="14573-200">Описание:</span><span class="sxs-lookup"><span data-stu-id="14573-200">Description</span></span> | <span data-ttu-id="14573-201">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="14573-201">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="14573-202">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-202">Optional string attribute.</span></span></p><p><span data-ttu-id="14573-203">Аргументы для исполняемого файла, указанного в атрибуте **processPath**.</span><span class="sxs-lookup"><span data-stu-id="14573-203">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="14573-204">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-204">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="14573-205">Если значение равно true, страница **502.5 — ошибка процесса** подавляется и страница в файле *web.config* с кодом состояния 502 имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="14573-205">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="14573-206">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-206">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="14573-207">Если значение равно true, маркер безопасности отправляется дочернему процессу, прослушивающему порт %ASPNETCORE_PORT% как заголовок "MS-ASPNETCORE-WINAUTHTOKEN" каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="14573-207">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="14573-208">Этот процесс вызывает CloseHandle по этому маркеру безопасности каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="14573-208">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="14573-209">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-209">Optional integer attribute.</span></span></p><p><span data-ttu-id="14573-210">Указывает число экземпляров процесса, заданное в параметре **processPath**, которое может появиться для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="14573-210">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="14573-211">По умолчанию: `1`</span><span class="sxs-lookup"><span data-stu-id="14573-211">Default: `1`</span></span><br><span data-ttu-id="14573-212">Минимум: `1`</span><span class="sxs-lookup"><span data-stu-id="14573-212">Min: `1`</span></span><br><span data-ttu-id="14573-213">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="14573-213">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="14573-214">Обязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-214">Required string attribute.</span></span></p><p><span data-ttu-id="14573-215">Путь к исполняемому файлу, который запускает процесс прослушивания HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="14573-215">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="14573-216">Поддерживаются относительные пути.</span><span class="sxs-lookup"><span data-stu-id="14573-216">Relative paths are supported.</span></span> <span data-ttu-id="14573-217">Если путь начинается с `.`, то начало пути считается относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="14573-217">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="14573-218">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-218">Optional integer attribute.</span></span></p><p><span data-ttu-id="14573-219">Указывает количество сбоев за минуту, которыми может завершиться процесс, указанный в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="14573-219">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="14573-220">Если этот предел превышен, модуль останавливает запуск процесса на оставшуюся часть минуты.</span><span class="sxs-lookup"><span data-stu-id="14573-220">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="14573-221">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="14573-221">Default: `10`</span></span><br><span data-ttu-id="14573-222">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="14573-222">Min: `0`</span></span><br><span data-ttu-id="14573-223">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="14573-223">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="14573-224">Необязательный атрибут timespan.</span><span class="sxs-lookup"><span data-stu-id="14573-224">Optional timespan attribute.</span></span></p><p><span data-ttu-id="14573-225">Указывает продолжительность, на протяжении которой модуль ASP.NET Core ожидает ответа от процесса, прослушивающего порт %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="14573-225">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="14573-226">В версиях модуля ASP.NET Core, поставляемых с выпуском ASP.NET Core 2.1 или новее, атрибут `requestTimeout` указывается в часах, минутах и секундах.</span><span class="sxs-lookup"><span data-stu-id="14573-226">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="14573-227">По умолчанию: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="14573-227">Default: `00:02:00`</span></span><br><span data-ttu-id="14573-228">Минимум: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="14573-228">Min: `00:00:00`</span></span><br><span data-ttu-id="14573-229">Максимум: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="14573-229">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="14573-230">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-230">Optional integer attribute.</span></span></p><p><span data-ttu-id="14573-231">Длительность ожидания модуля в секундах, пока произойдет правильное выключение исполняемого файла при обнаружении файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="14573-231">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="14573-232">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="14573-232">Default: `10`</span></span><br><span data-ttu-id="14573-233">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="14573-233">Min: `0`</span></span><br><span data-ttu-id="14573-234">Максимум: `600`</span><span class="sxs-lookup"><span data-stu-id="14573-234">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="14573-235">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-235">Optional integer attribute.</span></span></p><p><span data-ttu-id="14573-236">Время в секундах, которое модуль ожидает, пока запустится процесс прослушивания порта исполняемого файла.</span><span class="sxs-lookup"><span data-stu-id="14573-236">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="14573-237">Если этот предел превышен, модуль завершает процесс.</span><span class="sxs-lookup"><span data-stu-id="14573-237">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="14573-238">Модуль пытается перезапустить процесс при получении нового запроса и будет продолжать пытаться перезапустить процесс для последующих входящих запросов, если не удается запустить приложение определенное в атрибуте **rapidFailsPerMinute** количество раз за последнюю минуту.</span><span class="sxs-lookup"><span data-stu-id="14573-238">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="14573-239">Значение 0 (ноль) **не** считается бесконечным временем ожидания.</span><span class="sxs-lookup"><span data-stu-id="14573-239">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="14573-240">По умолчанию: `120`</span><span class="sxs-lookup"><span data-stu-id="14573-240">Default: `120`</span></span><br><span data-ttu-id="14573-241">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="14573-241">Min: `0`</span></span><br><span data-ttu-id="14573-242">Максимум: `3600`</span><span class="sxs-lookup"><span data-stu-id="14573-242">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="14573-243">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-243">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="14573-244">Если значение равно true, **stdout** и **stderr** для процесса, указанного в атрибуте **processPath**, перенаправляются к файлу, заданному в атрибуте **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="14573-244">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="14573-245">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-245">Optional string attribute.</span></span></p><p><span data-ttu-id="14573-246">Указывает относительный или абсолютный путь к файлу, для которого регистрируются **stdout** и **stderr** из процесса, указанного в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="14573-246">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="14573-247">Относительные пути задаются относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="14573-247">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="14573-248">Любой путь, начинающийся с `.`, относится к корневому каталогу веб-сайта, а все остальные пути рассматриваются как абсолютные пути.</span><span class="sxs-lookup"><span data-stu-id="14573-248">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="14573-249">Все папки, указанные в пути, должны существовать, чтобы модуль мог создать файл журнала.</span><span class="sxs-lookup"><span data-stu-id="14573-249">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="14573-250">С помощью разделителей подчеркивания метка времени, идентификатор процесса и расширение файла (*.log*) добавляются к последнему сегменту пути журнала **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="14573-250">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="14573-251">Если в качестве значения задано значение `.\logs\stdout`, например, журнал stdout сохраняется как *stdout_20180205194132_1934.log* в папке *журналов* с датой 5 февраля 2018 г. в 19:41:32 с идентификатором процесса 1934.</span><span class="sxs-lookup"><span data-stu-id="14573-251">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="14573-252">Атрибут</span><span class="sxs-lookup"><span data-stu-id="14573-252">Attribute</span></span> | <span data-ttu-id="14573-253">Описание:</span><span class="sxs-lookup"><span data-stu-id="14573-253">Description</span></span> | <span data-ttu-id="14573-254">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="14573-254">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="14573-255">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-255">Optional string attribute.</span></span></p><p><span data-ttu-id="14573-256">Аргументы для исполняемого файла, указанного в атрибуте **processPath**.</span><span class="sxs-lookup"><span data-stu-id="14573-256">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="14573-257">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-257">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="14573-258">Если значение равно true, страница **502.5 — ошибка процесса** подавляется и страница в файле *web.config* с кодом состояния 502 имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="14573-258">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="14573-259">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-259">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="14573-260">Если значение равно true, маркер безопасности отправляется дочернему процессу, прослушивающему порт %ASPNETCORE_PORT% как заголовок "MS-ASPNETCORE-WINAUTHTOKEN" каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="14573-260">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="14573-261">Этот процесс вызывает CloseHandle по этому маркеру безопасности каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="14573-261">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="14573-262">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-262">Optional integer attribute.</span></span></p><p><span data-ttu-id="14573-263">Указывает число экземпляров процесса, заданное в параметре **processPath**, которое может появиться для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="14573-263">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="14573-264">По умолчанию: `1`</span><span class="sxs-lookup"><span data-stu-id="14573-264">Default: `1`</span></span><br><span data-ttu-id="14573-265">Минимум: `1`</span><span class="sxs-lookup"><span data-stu-id="14573-265">Min: `1`</span></span><br><span data-ttu-id="14573-266">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="14573-266">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="14573-267">Обязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-267">Required string attribute.</span></span></p><p><span data-ttu-id="14573-268">Путь к исполняемому файлу, который запускает процесс прослушивания HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="14573-268">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="14573-269">Поддерживаются относительные пути.</span><span class="sxs-lookup"><span data-stu-id="14573-269">Relative paths are supported.</span></span> <span data-ttu-id="14573-270">Если путь начинается с `.`, то начало пути считается относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="14573-270">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="14573-271">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-271">Optional integer attribute.</span></span></p><p><span data-ttu-id="14573-272">Указывает количество сбоев за минуту, которыми может завершиться процесс, указанный в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="14573-272">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="14573-273">Если этот предел превышен, модуль останавливает запуск процесса на оставшуюся часть минуты.</span><span class="sxs-lookup"><span data-stu-id="14573-273">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="14573-274">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="14573-274">Default: `10`</span></span><br><span data-ttu-id="14573-275">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="14573-275">Min: `0`</span></span><br><span data-ttu-id="14573-276">Максимум: `100`</span><span class="sxs-lookup"><span data-stu-id="14573-276">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="14573-277">Необязательный атрибут timespan.</span><span class="sxs-lookup"><span data-stu-id="14573-277">Optional timespan attribute.</span></span></p><p><span data-ttu-id="14573-278">Указывает продолжительность, на протяжении которой модуль ASP.NET Core ожидает ответа от процесса, прослушивающего порт %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="14573-278">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="14573-279">В версиях модуля ASP.NET Core, поставляемых вместе с выпуском ASP.NET Core 2.0 или более ранними версиями, атрибут `requestTimeout` должен был быть задан в целых минутах (в противном случае для него по умолчанию задано значение 2 минуты).</span><span class="sxs-lookup"><span data-stu-id="14573-279">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="14573-280">По умолчанию: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="14573-280">Default: `00:02:00`</span></span><br><span data-ttu-id="14573-281">Минимум: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="14573-281">Min: `00:00:00`</span></span><br><span data-ttu-id="14573-282">Максимум: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="14573-282">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="14573-283">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-283">Optional integer attribute.</span></span></p><p><span data-ttu-id="14573-284">Длительность ожидания модуля в секундах, пока произойдет правильное выключение исполняемого файла при обнаружении файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="14573-284">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="14573-285">По умолчанию: `10`</span><span class="sxs-lookup"><span data-stu-id="14573-285">Default: `10`</span></span><br><span data-ttu-id="14573-286">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="14573-286">Min: `0`</span></span><br><span data-ttu-id="14573-287">Максимум: `600`</span><span class="sxs-lookup"><span data-stu-id="14573-287">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="14573-288">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-288">Optional integer attribute.</span></span></p><p><span data-ttu-id="14573-289">Время в секундах, которое модуль ожидает, пока запустится процесс прослушивания порта исполняемого файла.</span><span class="sxs-lookup"><span data-stu-id="14573-289">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="14573-290">Если этот предел превышен, модуль завершает процесс.</span><span class="sxs-lookup"><span data-stu-id="14573-290">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="14573-291">Модуль пытается перезапустить процесс при получении нового запроса и будет продолжать пытаться перезапустить процесс для последующих входящих запросов, если не удается запустить приложение определенное в атрибуте **rapidFailsPerMinute** количество раз за последнюю минуту.</span><span class="sxs-lookup"><span data-stu-id="14573-291">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="14573-292">По умолчанию: `120`</span><span class="sxs-lookup"><span data-stu-id="14573-292">Default: `120`</span></span><br><span data-ttu-id="14573-293">Минимум: `0`</span><span class="sxs-lookup"><span data-stu-id="14573-293">Min: `0`</span></span><br><span data-ttu-id="14573-294">Максимум: `3600`</span><span class="sxs-lookup"><span data-stu-id="14573-294">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="14573-295">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-295">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="14573-296">Если значение равно true, **stdout** и **stderr** для процесса, указанного в атрибуте **processPath**, перенаправляются к файлу, заданному в атрибуте **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="14573-296">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="14573-297">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="14573-297">Optional string attribute.</span></span></p><p><span data-ttu-id="14573-298">Указывает относительный или абсолютный путь к файлу, для которого регистрируются **stdout** и **stderr** из процесса, указанного в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="14573-298">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="14573-299">Относительные пути задаются относительно корневого каталога веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="14573-299">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="14573-300">Любой путь, начинающийся с `.`, относится к корневому каталогу веб-сайта, а все остальные пути рассматриваются как абсолютные пути.</span><span class="sxs-lookup"><span data-stu-id="14573-300">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="14573-301">Все папки, указанные в пути, должны существовать, чтобы модуль мог создать файл журнала.</span><span class="sxs-lookup"><span data-stu-id="14573-301">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="14573-302">С помощью разделителей подчеркивания метка времени, идентификатор процесса и расширение файла (*.log*) добавляются к последнему сегменту пути журнала **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="14573-302">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="14573-303">Если в качестве значения задано значение `.\logs\stdout`, например, журнал stdout сохраняется как *stdout_20180205194132_1934.log* в папке *журналов* с датой 5 февраля 2018 г. в 19:41:32 с идентификатором процесса 1934.</span><span class="sxs-lookup"><span data-stu-id="14573-303">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="14573-304">Настройка переменных среды</span><span class="sxs-lookup"><span data-stu-id="14573-304">Setting environment variables</span></span>

<span data-ttu-id="14573-305">Переменные среды для процесса можно указать в атрибуте `processPath`.</span><span class="sxs-lookup"><span data-stu-id="14573-305">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="14573-306">Укажите переменную среды с дочерним элементом `environmentVariable` элемента коллекции `environmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="14573-306">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="14573-307">Переменные среды, установленные в этом разделе, имеют приоритет над переменными системной среды.</span><span class="sxs-lookup"><span data-stu-id="14573-307">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="14573-308">В следующем примере устанавливаются две переменные среды.</span><span class="sxs-lookup"><span data-stu-id="14573-308">The following example sets two environment variables.</span></span> <span data-ttu-id="14573-309">`ASPNETCORE_ENVIRONMENT` настраивает среду приложения для `Development`.</span><span class="sxs-lookup"><span data-stu-id="14573-309">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="14573-310">Разработчик может временно задать это значение в файле *web.config*, чтобы принудительно загрузить [Страницу исключений для разработчиков](xref:fundamentals/error-handling) при отладке исключения приложения.</span><span class="sxs-lookup"><span data-stu-id="14573-310">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="14573-311">`CONFIG_DIR` — пример пользовательской переменной среды, где разработчик написал код, который считывает значение при запуске, чтобы сформировать путь для загрузки файла конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="14573-311">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="inprocess">
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
> <span data-ttu-id="14573-312">Установите только переменную среды `ASPNETCORE_ENVIRONMENT` для `Development` на серверах промежуточных процессов и тестирования, которые недоступны для ненадежных сетей, таких как Интернет.</span><span class="sxs-lookup"><span data-stu-id="14573-312">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="14573-313">App_offline.htm</span><span class="sxs-lookup"><span data-stu-id="14573-313">app_offline.htm</span></span>

<span data-ttu-id="14573-314">Если в корневом каталоге приложения обнаружен файл с именем *app_offline.htm*, модуль ASP.NET Core пытается корректно закрыть приложение и прекратить обработку входящих запросов.</span><span class="sxs-lookup"><span data-stu-id="14573-314">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="14573-315">Если приложение по-прежнему выполняется через количество секунд, определенное атрибутом `shutdownTimeLimit`, модуль ASP.NET Core завершает выполняющийся процесс.</span><span class="sxs-lookup"><span data-stu-id="14573-315">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="14573-316">Хотя файл *app_offline.htm* присутствует, модуль ASP.NET Core отвечает на запросы, отправляя назад содержимое файла *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="14573-316">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="14573-317">Когда *app_offline.htm* файл удаляется, следующий запрос запускает приложение.</span><span class="sxs-lookup"><span data-stu-id="14573-317">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="14573-318">При использовании модели размещения вне процесса приложение может не завершать работу немедленно при наличии открытого соединения.</span><span class="sxs-lookup"><span data-stu-id="14573-318">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="14573-319">Например, подключение websocket может задерживать завершение работы приложения.</span><span class="sxs-lookup"><span data-stu-id="14573-319">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="14573-320">Страница ошибок запуска</span><span class="sxs-lookup"><span data-stu-id="14573-320">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="14573-321">Если при внутри- или внепроцессном размещении происходит сбой запуска приложения, открываются страницы пользовательских сообщений об ошибках.</span><span class="sxs-lookup"><span data-stu-id="14573-321">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="14573-322">Если модулю ASP.NET Core не удается найти внутри- или внепроцессный обработчик запросов, откроется страница кода состояния *500.0 — ошибка загрузки внутри- или внепроцессного обработчика запросов*.</span><span class="sxs-lookup"><span data-stu-id="14573-322">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="14573-323">Если в модели размещения внутри процесса модулю ASP.NET Core не удается запустить приложение, откроется страница кода состояния *500.30 — ошибка запуска*.</span><span class="sxs-lookup"><span data-stu-id="14573-323">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="14573-324">Если в модели размещения вне процесса модулю ASP.NET Core не удается запустить серверный процесс или начинается серверный процесс, но ему не удается прослушать настроенный порт, появится страница кода состояния *502.5 — ошибка процесса*.</span><span class="sxs-lookup"><span data-stu-id="14573-324">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="14573-325">Чтобы подавить отображение этой странице и вернуться к странице IIS кода состояния 5xx по умолчанию, используйте атрибут `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="14573-325">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="14573-326">Дополнительные сведения о настройке пользовательских сообщений об ошибках см. в разделе [Ошибки HTTP &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="14573-326">For more information on configuring custom error messages, see [HTTP Errors &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="14573-327">Если модулю ASP.NET Core не удается запустить серверный процесс или начинается серверный процесс, но ему не удается прослушать настроенный порт, появится страница кода состояния *502.5 — ошибка процесса*.</span><span class="sxs-lookup"><span data-stu-id="14573-327">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="14573-328">Чтобы подавить эту страницу и вернуться к странице IIS кода состояния 502 по умолчанию, используйте атрибут `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="14573-328">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="14573-329">Дополнительные сведения о настройке пользовательских сообщений об ошибках см. в разделе [Ошибки HTTP &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="14573-329">For more information on configuring custom error messages, see [HTTP Errors &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Страница кода состояния "502.5 — ошибка процесса"](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="14573-331">Создание и перенаправление журнала</span><span class="sxs-lookup"><span data-stu-id="14573-331">Log creation and redirection</span></span>

<span data-ttu-id="14573-332">Модуль ASP.NET Core перенаправляет выходные потоки консоли stdout и stderr на диск, если заданы атрибуты `stdoutLogEnabled` и `stdoutLogFile` элемента `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="14573-332">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="14573-333">Чтобы модуль мог создать файл журнала, все папки в пути `stdoutLogFile` должны существовать.</span><span class="sxs-lookup"><span data-stu-id="14573-333">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="14573-334">Пул приложений должен иметь доступ на запись в папку, где записываются журналы (используйте атрибут `IIS AppPool\<app_pool_name>` для предоставления разрешения на запись).</span><span class="sxs-lookup"><span data-stu-id="14573-334">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="14573-335">Журналы не выполняют циклический сдвиг, пока не произойдет процесс перезапуска или перезагрузки.</span><span class="sxs-lookup"><span data-stu-id="14573-335">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="14573-336">Администратор несет ответственность за ограничение дискового пространства, которое потребляют журналы.</span><span class="sxs-lookup"><span data-stu-id="14573-336">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="14573-337">Использование журнала stdout рекомендуется только для устранения неполадок при запуске приложений.</span><span class="sxs-lookup"><span data-stu-id="14573-337">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="14573-338">Не используйте журнал stdout для ведения общего журнала приложений.</span><span class="sxs-lookup"><span data-stu-id="14573-338">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="14573-339">Для обычного входа в приложение ASP.NET Core используйте библиотеку ведения журналов, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов.</span><span class="sxs-lookup"><span data-stu-id="14573-339">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="14573-340">Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="14573-340">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="14573-341">При создании файла журнала автоматически добавляются отметка времени и расширение файла.</span><span class="sxs-lookup"><span data-stu-id="14573-341">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="14573-342">Имя файла журнала составляется путем добавления метки времени, идентификатора процесса и расширения файла (*.log*) к последнему сегменту атрибута пути `stdoutLogFile` (обычно *stdout*) с символами подчеркивания в качестве разделителей.</span><span class="sxs-lookup"><span data-stu-id="14573-342">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="14573-343">Если атрибут пути `stdoutLogFile` заканчивается элементом *stdout*, журнал приложения с идентификатором 1934, созданный 5 февраля 2018 г. в 19:42:32, будет иметь имя *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="14573-343">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="14573-344">Если `stdoutLogEnabled` имеет значение false, возникающие при запуске приложения ошибки записываются и передаются в журнал событий (макс. 30 КБ).</span><span class="sxs-lookup"><span data-stu-id="14573-344">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="14573-345">После запуска все дополнительные журналы удаляются.</span><span class="sxs-lookup"><span data-stu-id="14573-345">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="14573-346">В следующем примере элемент `aspNetCore` настраивает ведение журнала stdout для приложения, размещенного в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="14573-346">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="14573-347">Для локального ведения журнала допустим локальный или общий сетевой путь.</span><span class="sxs-lookup"><span data-stu-id="14573-347">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="14573-348">Убедитесь, что идентификатор пользователя AppPool имеет разрешение на запись по указанному пути.</span><span class="sxs-lookup"><span data-stu-id="14573-348">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="14573-349">Расширенные журналы диагностики</span><span class="sxs-lookup"><span data-stu-id="14573-349">Enhanced diagnostic logs</span></span>

<span data-ttu-id="14573-350">Модуль ASP.NET Core предоставляет возможности настройки, позволяющие работать с расширенными журналами диагностики.</span><span class="sxs-lookup"><span data-stu-id="14573-350">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="14573-351">Добавьте элемент `<handlerSettings>` в элемент `<aspNetCore>` в файле *web.config*. Задайте параметру `debugLevel` значение `TRACE`, чтобы обеспечить высокую точность диагностических сведений:</span><span class="sxs-lookup"><span data-stu-id="14573-351">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="debugFile" value="aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="14573-352">Значения уровня отладки (`debugLevel`) могут включать уровень и расположение.</span><span class="sxs-lookup"><span data-stu-id="14573-352">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="14573-353">Уровни (в порядке возрастания степени детализации):</span><span class="sxs-lookup"><span data-stu-id="14573-353">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="14573-354">ОШИБКА</span><span class="sxs-lookup"><span data-stu-id="14573-354">ERROR</span></span>
* <span data-ttu-id="14573-355">ПРЕДУПРЕЖДЕНИЕ</span><span class="sxs-lookup"><span data-stu-id="14573-355">WARNING</span></span>
* <span data-ttu-id="14573-356">ИНФОРМАЦИЯ</span><span class="sxs-lookup"><span data-stu-id="14573-356">INFO</span></span>
* <span data-ttu-id="14573-357">TRACE</span><span class="sxs-lookup"><span data-stu-id="14573-357">TRACE</span></span>

<span data-ttu-id="14573-358">Расположения (допускаются несколько расположений):</span><span class="sxs-lookup"><span data-stu-id="14573-358">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="14573-359">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="14573-359">CONSOLE</span></span>
* <span data-ttu-id="14573-360">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="14573-360">EVENTLOG</span></span>
* <span data-ttu-id="14573-361">ФАЙЛ</span><span class="sxs-lookup"><span data-stu-id="14573-361">FILE</span></span>

<span data-ttu-id="14573-362">Параметры обработчика могут быть указаны с помощью переменных среды:</span><span class="sxs-lookup"><span data-stu-id="14573-362">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="14573-363">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Путь к файлу журнала отладки.</span><span class="sxs-lookup"><span data-stu-id="14573-363">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="14573-364">(По умолчанию: *aspnetcore debug.log*)</span><span class="sxs-lookup"><span data-stu-id="14573-364">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="14573-365">`ASPNETCORE_MODULE_DEBUG` &ndash; Параметр уровня отладки.</span><span class="sxs-lookup"><span data-stu-id="14573-365">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="14573-366">**Не** оставляйте ведение журнала отладки включенным в развертывании дольше того времени, которое требуется для устранения проблемы.</span><span class="sxs-lookup"><span data-stu-id="14573-366">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="14573-367">Размер журнала не ограничен.</span><span class="sxs-lookup"><span data-stu-id="14573-367">The size of the log isn't limited.</span></span> <span data-ttu-id="14573-368">Если оставить журнал отладки включенным, он может исчерпать все доступное место на диске и привести к сбою сервера или службы приложений.</span><span class="sxs-lookup"><span data-stu-id="14573-368">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="14573-369">См. пример элемента `aspNetCore` в файле *web.config* в разделе [Конфигурация с помощью файла web.config](#configuration-with-webconfig).</span><span class="sxs-lookup"><span data-stu-id="14573-369">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="14573-370">Конфигурация прокси-сервера использует протокол HTTP и токен связывания</span><span class="sxs-lookup"><span data-stu-id="14573-370">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="14573-371">*Применяется только к размещению вне процесса.*</span><span class="sxs-lookup"><span data-stu-id="14573-371">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="14573-372">Прокси-сервер, созданный между модулем ASP.NET Core и Kestrel, использует протокол HTTP.</span><span class="sxs-lookup"><span data-stu-id="14573-372">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="14573-373">Использование HTTP позволяет оптимизировать производительность, так как трафик между модулем и Kestrel передается на петлевой адрес в сетевом интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="14573-373">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="14573-374">Отсутствует риск перехвата трафика между модулем и Kestrel из расположения на сервере.</span><span class="sxs-lookup"><span data-stu-id="14573-374">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="14573-375">Токен связывания гарантирует, что полученные Kestrel запросы были переданы службами IIS, а не из какого-либо другого источника.</span><span class="sxs-lookup"><span data-stu-id="14573-375">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="14573-376">Этот токен создается и задается модулем в переменную среды (`ASPNETCORE_TOKEN`).</span><span class="sxs-lookup"><span data-stu-id="14573-376">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="14573-377">Он также задается в заголовке (`MSAspNetCoreToken`) каждого запроса, переданного через прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="14573-377">The pairing token is also set into a header (`MSAspNetCoreToken`) on every proxied request.</span></span> <span data-ttu-id="14573-378">ПО промежуточного слоя IIS проверяет каждый получаемый запрос, чтобы убедиться, что заголовок с токеном связывания соответствует значению переменной среды.</span><span class="sxs-lookup"><span data-stu-id="14573-378">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="14573-379">Если значения токена не совпадают, запрос заносится в журнал и отклоняется.</span><span class="sxs-lookup"><span data-stu-id="14573-379">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="14573-380">Переменная среды с токеном связывания и трафик между модулем и Kestrel недоступны из расположения на сервере.</span><span class="sxs-lookup"><span data-stu-id="14573-380">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="14573-381">Не зная значение токена связывания, злоумышленник не может отправлять запросы, обходящие проверку в ПО промежуточного слоя IIS.</span><span class="sxs-lookup"><span data-stu-id="14573-381">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="14573-382">Модуль ASP.NET Core с общей конфигурацией IIS</span><span class="sxs-lookup"><span data-stu-id="14573-382">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="14573-383">Программа установки модуля ASP.NET Core запускается с правами **системной** учетной записи.</span><span class="sxs-lookup"><span data-stu-id="14573-383">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="14573-384">Поскольку учетная запись локальной системы не имеет разрешения на изменение пути к общей папке, используемого общей конфигурацией IIS, установщик получает ошибку отказа в доступе при попытке настроить параметры модуля в файле *applicationHost.config* общей папки.</span><span class="sxs-lookup"><span data-stu-id="14573-384">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="14573-385">При использовании общей конфигурации IIS выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="14573-385">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="14573-386">Отключите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="14573-386">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="14573-387">Запустите установщик.</span><span class="sxs-lookup"><span data-stu-id="14573-387">Run the installer.</span></span>
1. <span data-ttu-id="14573-388">Экспортируйте обновленный файл *applicationHost.config* в общую папку.</span><span class="sxs-lookup"><span data-stu-id="14573-388">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="14573-389">Повторно включите общую конфигурацию IIS.</span><span class="sxs-lookup"><span data-stu-id="14573-389">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="14573-390">Версия модуля и журналы установщика хостинга Bundle</span><span class="sxs-lookup"><span data-stu-id="14573-390">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="14573-391">Чтобы определить версию установщика модуля ASP.NET Core, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="14573-391">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="14573-392">В системе размещения перейдите к папке *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="14573-392">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="14573-393">Найдите файл *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="14573-393">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="14573-394">Щелкните правой кнопкой мыши файл и выберите **Свойства** из контекстного меню.</span><span class="sxs-lookup"><span data-stu-id="14573-394">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="14573-395">Выберите вкладку **Сведения**. **Версия файла** и **Версия продукта** дают представление об установленной версии модуля.</span><span class="sxs-lookup"><span data-stu-id="14573-395">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="14573-396">Журналы установщика хостинга Bundle для модуля находятся в папке *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. Этот файл имеет имя *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="14573-396">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="14573-397">Модуль, схемы и расположение файлов конфигурации</span><span class="sxs-lookup"><span data-stu-id="14573-397">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="14573-398">Module</span><span class="sxs-lookup"><span data-stu-id="14573-398">Module</span></span>

<span data-ttu-id="14573-399">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="14573-399">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="14573-400">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="14573-400">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="14573-401">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="14573-401">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="14573-402">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="14573-402">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="14573-403">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="14573-403">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="14573-404">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="14573-404">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="14573-405">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="14573-405">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="14573-406">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="14573-406">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="14573-407">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="14573-407">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="14573-408">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="14573-408">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="14573-409">Схема</span><span class="sxs-lookup"><span data-stu-id="14573-409">Schema</span></span>

<span data-ttu-id="14573-410">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="14573-410">**IIS**</span></span>

   * <span data-ttu-id="14573-411">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="14573-411">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="14573-412">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="14573-412">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="14573-413">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="14573-413">**IIS Express**</span></span>

   * <span data-ttu-id="14573-414">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="14573-414">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="14573-415">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="14573-415">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="14573-416">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="14573-416">Configuration</span></span>

<span data-ttu-id="14573-417">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="14573-417">**IIS**</span></span>

   * <span data-ttu-id="14573-418">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="14573-418">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="14573-419">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="14573-419">**IIS Express**</span></span>

   * <span data-ttu-id="14573-420">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="14573-420">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span></span>

<span data-ttu-id="14573-421">Файлы можно найти путем поиска *aspnetcore* в файле *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="14573-421">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>