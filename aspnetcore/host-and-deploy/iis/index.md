---
title: Размещение ASP.NET Core в Windows со службами IIS
author: guardrex
description: Сведения о размещении приложений ASP.NET Core в службах Windows Server Internet Information Services (IIS).
ms.author: riande
ms.custom: mvc
ms.date: 11/10/2018
uid: host-and-deploy/iis/index
ms.openlocfilehash: 1b34195dc51ca8dab5e8eda10f05ff6678fbc78c
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2018
ms.locfileid: "51570169"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="06e29-103">Размещение ASP.NET Core в Windows со службами IIS</span><span class="sxs-lookup"><span data-stu-id="06e29-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="06e29-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="06e29-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[<span data-ttu-id="06e29-105">Установка пакета размещения .NET Core</span><span class="sxs-lookup"><span data-stu-id="06e29-105">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="06e29-106">Поддерживаемые операционные системы</span><span class="sxs-lookup"><span data-stu-id="06e29-106">Supported operating systems</span></span>

<span data-ttu-id="06e29-107">Поддерживаются следующие операционные системы:</span><span class="sxs-lookup"><span data-stu-id="06e29-107">The following operating systems are supported:</span></span>

* <span data-ttu-id="06e29-108">Windows 7 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="06e29-108">Windows 7 or later</span></span>
* <span data-ttu-id="06e29-109">Windows Server 2008 R2 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="06e29-109">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="06e29-110">[Сервер HTTP.sys](xref:fundamentals/servers/httpsys) (ранее назывался [WebListener](xref:fundamentals/servers/weblistener)) не работает в конфигурации обратного прокси-сервера со службами IIS.</span><span class="sxs-lookup"><span data-stu-id="06e29-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="06e29-111">Используйте [сервер Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="06e29-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="06e29-112">Сведения о размещении в Azure см. в статье <xref:host-and-deploy/azure-apps/index>.</span><span class="sxs-lookup"><span data-stu-id="06e29-112">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="06e29-113">Настройка приложения</span><span class="sxs-lookup"><span data-stu-id="06e29-113">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="06e29-114">Включение компонентов IISIntegration</span><span class="sxs-lookup"><span data-stu-id="06e29-114">Enable the IISIntegration components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="06e29-115">Обычно *Program.cs* вызывает <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, чтобы начать настройку узла:</span><span class="sxs-lookup"><span data-stu-id="06e29-115">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="06e29-116">Обычно *Program.cs* вызывает <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, чтобы начать настройку узла:</span><span class="sxs-lookup"><span data-stu-id="06e29-116">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host:</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="06e29-117">**Модель внутрипроцессного размещения**</span><span class="sxs-lookup"><span data-stu-id="06e29-117">**In-process hosting model**</span></span>

<span data-ttu-id="06e29-118">`CreateDefaultBuilder` вызывает метод `UseIIS` для загрузки [CoreCLR](/dotnet/standard/glossary#coreclr) и размещения приложения внутри рабочего процесса IIS (*w3wp.exe* или *iisexpress.exe*).</span><span class="sxs-lookup"><span data-stu-id="06e29-118">`CreateDefaultBuilder` calls the `UseIIS` method to boot the [CoreCLR](/dotnet/standard/glossary#coreclr) and host the app inside of the IIS worker process (*w3wp.exe* or *iisexpress.exe*).</span></span> <span data-ttu-id="06e29-119">Тесты производительности показывают, что размещение приложения .NET Core внутри процесса позволяет обрабатывать значительно больше запросов, чем при размещении приложения вне процесса с перенаправлением запросов к [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="06e29-119">Performance tests indicate that hosting a .NET Core app in-process delivers significantly higher request throughput compared to hosting the app out-of-process and proxying requests to [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="06e29-120">**Модель размещения вне процесса**</span><span class="sxs-lookup"><span data-stu-id="06e29-120">**Out-of-process hosting model**</span></span>

<span data-ttu-id="06e29-121">Для внепроцессного размещения в IIS метод `CreateDefaultBuilder` настраивает [Kestrel](xref:fundamentals/servers/kestrel) в качестве веб-сервера и активирует интеграцию с IIS, задавая базовый путь и порт для [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="06e29-121">For out-of-process hosting with IIS, `CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="06e29-122">Модуль ASP.NET Core создает динамический порт для назначения серверному процессу.</span><span class="sxs-lookup"><span data-stu-id="06e29-122">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="06e29-123">`CreateDefaultBuilder` вызывает метод <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*>.</span><span class="sxs-lookup"><span data-stu-id="06e29-123">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="06e29-124">`UseIISIntegration` настраивает Kestrel для прослушивания динамического порта по IP-адресу localhost (`127.0.0.1`).</span><span class="sxs-lookup"><span data-stu-id="06e29-124">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="06e29-125">Если динамический порт — 1234, Kestrel прослушивает `127.0.0.1:1234`.</span><span class="sxs-lookup"><span data-stu-id="06e29-125">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="06e29-126">Эта конфигурация заменяет другие конфигурации URL-адресов, предоставляемые:</span><span class="sxs-lookup"><span data-stu-id="06e29-126">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* <span data-ttu-id="06e29-127">[API прослушивания Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration);</span><span class="sxs-lookup"><span data-stu-id="06e29-127">[Kestrel's Listen API](xref:fundamentals/servers/kestrel#endpoint-configuration)</span></span>
* <span data-ttu-id="06e29-128">[Конфигурацией](xref:fundamentals/configuration/index) (или [параметром командной строки--urls](xref:fundamentals/host/web-host#override-configuration)).</span><span class="sxs-lookup"><span data-stu-id="06e29-128">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="06e29-129">Вызовы `UseUrls` или API `Listen` Kestrel при работе с этим модулем не требуются.</span><span class="sxs-lookup"><span data-stu-id="06e29-129">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="06e29-130">При вызове `UseUrls` или `Listen` Kestrel ожидает передачи данных на порты, указанные только при выполнении приложения без IIS.</span><span class="sxs-lookup"><span data-stu-id="06e29-130">If `UseUrls` or `Listen` is called, Kestrel listens on the ports specified only when running the app without IIS.</span></span>

<span data-ttu-id="06e29-131">Дополнительные сведения о моделях внутри- и внепроцессного размещения см. в разделах [Модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) и [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="06e29-131">For more information on the in-process and out-of-process hosting models, see [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="06e29-132">`CreateDefaultBuilder` настраивает [Kestrel](xref:fundamentals/servers/kestrel) в качестве веб-сервера и активирует интеграцию IIS, задавая базовый путь и порт для [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="06e29-132">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="06e29-133">Модуль ASP.NET Core создает динамический порт для назначения серверному процессу.</span><span class="sxs-lookup"><span data-stu-id="06e29-133">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="06e29-134">`CreateDefaultBuilder` вызывает метод [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration).</span><span class="sxs-lookup"><span data-stu-id="06e29-134">`CreateDefaultBuilder` calls the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) method.</span></span> <span data-ttu-id="06e29-135">`UseIISIntegration` настраивает Kestrel для прослушивания динамического порта по IP-адресу localhost (`127.0.0.1`).</span><span class="sxs-lookup"><span data-stu-id="06e29-135">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="06e29-136">Если динамический порт — 1234, Kestrel прослушивает `127.0.0.1:1234`.</span><span class="sxs-lookup"><span data-stu-id="06e29-136">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="06e29-137">Эта конфигурация заменяет другие конфигурации URL-адресов, предоставляемые:</span><span class="sxs-lookup"><span data-stu-id="06e29-137">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* <span data-ttu-id="06e29-138">[API прослушивания Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration);</span><span class="sxs-lookup"><span data-stu-id="06e29-138">[Kestrel's Listen API](xref:fundamentals/servers/kestrel#endpoint-configuration)</span></span>
* <span data-ttu-id="06e29-139">[Конфигурацией](xref:fundamentals/configuration/index) (или [параметром командной строки--urls](xref:fundamentals/host/web-host#override-configuration)).</span><span class="sxs-lookup"><span data-stu-id="06e29-139">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="06e29-140">Вызовы `UseUrls` или API `Listen` Kestrel при работе с этим модулем не требуются.</span><span class="sxs-lookup"><span data-stu-id="06e29-140">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="06e29-141">При вызове `UseUrls` или `Listen` Kestrel ожидает передачи данных на порт, указанный только при выполнении приложения без IIS.</span><span class="sxs-lookup"><span data-stu-id="06e29-141">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="06e29-142">`CreateDefaultBuilder` настраивает [Kestrel](xref:fundamentals/servers/kestrel) в качестве веб-сервера и активирует интеграцию IIS, задавая базовый путь и порт для [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="06e29-142">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="06e29-143">Модуль ASP.NET Core создает динамический порт для назначения серверному процессу.</span><span class="sxs-lookup"><span data-stu-id="06e29-143">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="06e29-144">`CreateDefaultBuilder` вызывает метод [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration).</span><span class="sxs-lookup"><span data-stu-id="06e29-144">`CreateDefaultBuilder` calls the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) method.</span></span> <span data-ttu-id="06e29-145">`UseIISIntegration` настраивает Kestrel для прослушивания динамического порта по IP-адресу localhost (`localhost`).</span><span class="sxs-lookup"><span data-stu-id="06e29-145">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`localhost`).</span></span> <span data-ttu-id="06e29-146">Если динамический порт — 1234, Kestrel прослушивает `localhost:1234`.</span><span class="sxs-lookup"><span data-stu-id="06e29-146">If the dynamic port is 1234, Kestrel listens at `localhost:1234`.</span></span> <span data-ttu-id="06e29-147">Эта конфигурация заменяет другие конфигурации URL-адресов, предоставляемые:</span><span class="sxs-lookup"><span data-stu-id="06e29-147">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* <span data-ttu-id="06e29-148">[API прослушивания Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration);</span><span class="sxs-lookup"><span data-stu-id="06e29-148">[Kestrel's Listen API](xref:fundamentals/servers/kestrel#endpoint-configuration)</span></span>
* <span data-ttu-id="06e29-149">[Конфигурацией](xref:fundamentals/configuration/index) (или [параметром командной строки--urls](xref:fundamentals/host/web-host#override-configuration)).</span><span class="sxs-lookup"><span data-stu-id="06e29-149">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="06e29-150">Вызовы `UseUrls` или API `Listen` Kestrel при работе с этим модулем не требуются.</span><span class="sxs-lookup"><span data-stu-id="06e29-150">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="06e29-151">При вызове `UseUrls` или `Listen` Kestrel ожидает передачи данных на порт, указанный только при выполнении приложения без IIS.</span><span class="sxs-lookup"><span data-stu-id="06e29-151">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="06e29-152">Включите в зависимости приложения зависимость от пакета [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/).</span><span class="sxs-lookup"><span data-stu-id="06e29-152">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="06e29-153">Используйте ПО промежуточного слоя для интеграции IIS, добавив метод расширения [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) в [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="06e29-153">Use IIS Integration middleware by adding the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) extension method to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder):</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="06e29-154">Оба метода, [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) и [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration), — обязательные.</span><span class="sxs-lookup"><span data-stu-id="06e29-154">Both [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) and [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) are required.</span></span> <span data-ttu-id="06e29-155">Код, вызывающий `UseIISIntegration`, не влияет на переносимость кода.</span><span class="sxs-lookup"><span data-stu-id="06e29-155">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="06e29-156">Если приложение запускается не в IIS (например, запускается непосредственно в Kestrel), `UseIISIntegration` не работает.</span><span class="sxs-lookup"><span data-stu-id="06e29-156">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` doesn't operate.</span></span>

<span data-ttu-id="06e29-157">Модуль ASP.NET Core создает динамический порт для назначения серверному процессу.</span><span class="sxs-lookup"><span data-stu-id="06e29-157">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="06e29-158">`UseIISIntegration` настраивает Kestrel для прослушивания динамического порта по IP-адресу localhost (`localhost`).</span><span class="sxs-lookup"><span data-stu-id="06e29-158">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`localhost`).</span></span> <span data-ttu-id="06e29-159">Если динамический порт — 1234, Kestrel прослушивает `localhost:1234`.</span><span class="sxs-lookup"><span data-stu-id="06e29-159">If the dynamic port is 1234, Kestrel listens at `localhost:1234`.</span></span> <span data-ttu-id="06e29-160">Эта конфигурация заменяет другие конфигурации URL-адресов, предоставляемые:</span><span class="sxs-lookup"><span data-stu-id="06e29-160">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* <span data-ttu-id="06e29-161">[Конфигурацией](xref:fundamentals/configuration/index) (или [параметром командной строки--urls](xref:fundamentals/host/web-host#override-configuration)).</span><span class="sxs-lookup"><span data-stu-id="06e29-161">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="06e29-162">При использовании этого модуля вызов `UseUrls` не требуется.</span><span class="sxs-lookup"><span data-stu-id="06e29-162">A call to `UseUrls` isn't required when using the module.</span></span> <span data-ttu-id="06e29-163">При вызове `UseUrls` Kestrel ожидает передачи данных на порт, указанный только при выполнении приложения без IIS.</span><span class="sxs-lookup"><span data-stu-id="06e29-163">If `UseUrls` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

<span data-ttu-id="06e29-164">При вызове `UseUrls` в приложении ASP.NET Core 1.0 следует выполнять вызов **до** вызова `UseIISIntegration`, чтобы исключить перезапись порта, настроенного в модуле.</span><span class="sxs-lookup"><span data-stu-id="06e29-164">If `UseUrls` is called in an ASP.NET Core 1.0 app, call it **before** calling `UseIISIntegration` so that the module-configured port isn't overwritten.</span></span> <span data-ttu-id="06e29-165">В ASP.NET Core 1.1 соблюдать этот порядок вызовов не требуется, так как параметр модуля переопределяет `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="06e29-165">This calling order isn't required with ASP.NET Core 1.1 because the module setting overrides `UseUrls`.</span></span>

::: moniker-end

<span data-ttu-id="06e29-166">Дополнительные сведения о размещении см. в разделе [Размещение в ASP.NET Core](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="06e29-166">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/host/index).</span></span>

### <a name="iis-options"></a><span data-ttu-id="06e29-167">Параметры служб IIS</span><span class="sxs-lookup"><span data-stu-id="06e29-167">IIS options</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="06e29-168">**Модель внутрипроцессного размещения**</span><span class="sxs-lookup"><span data-stu-id="06e29-168">**In-process hosting model**</span></span>

<span data-ttu-id="06e29-169">Чтобы настроить параметры сервера IIS, включите конфигурацию службы для [IISServerOptions](/dotnet/api/microsoft.aspnetcore.builder.iisserveroptions) в [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span><span class="sxs-lookup"><span data-stu-id="06e29-169">To configure IIS Server options, include a service configuration for [IISServerOptions](/dotnet/api/microsoft.aspnetcore.builder.iisserveroptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span></span> <span data-ttu-id="06e29-170">В следующем примере показано, как отключить AutomaticAuthentication:</span><span class="sxs-lookup"><span data-stu-id="06e29-170">The following example disables AutomaticAuthentication:</span></span>

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

| <span data-ttu-id="06e29-171">Параметр</span><span class="sxs-lookup"><span data-stu-id="06e29-171">Option</span></span>                         | <span data-ttu-id="06e29-172">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="06e29-172">Default</span></span> | <span data-ttu-id="06e29-173">Параметр</span><span class="sxs-lookup"><span data-stu-id="06e29-173">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="06e29-174">Если значение — `true`, сервер IIS задает свойство `HttpContext.User`, использующее [проверку подлинности Windows](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="06e29-174">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="06e29-175">Если значение — `false`, сервер только предоставляет идентификатор для `HttpContext.User` и отвечает на явные запросы защиты от `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="06e29-175">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="06e29-176">Для работы `AutomaticAuthentication` необходимо включить в службах IIS проверку подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="06e29-176">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="06e29-177">Дополнительные сведения: [Проверка подлинности Windows](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="06e29-177">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="06e29-178">Задает отображаемое имя для пользователей на страницах входа.</span><span class="sxs-lookup"><span data-stu-id="06e29-178">Sets the display name shown to users on login pages.</span></span> |

<span data-ttu-id="06e29-179">**Модель размещения вне процесса**</span><span class="sxs-lookup"><span data-stu-id="06e29-179">**Out-of-process hosting model**</span></span>

::: moniker-end

<span data-ttu-id="06e29-180">Чтобы настроить параметры IIS, включите конфигурацию службы для [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) в [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span><span class="sxs-lookup"><span data-stu-id="06e29-180">To configure IIS options, include a service configuration for [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span></span> <span data-ttu-id="06e29-181">В следующем примере приложению запрещается заполнение `HttpContext.Connection.ClientCertificate`:</span><span class="sxs-lookup"><span data-stu-id="06e29-181">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="06e29-182">Параметр</span><span class="sxs-lookup"><span data-stu-id="06e29-182">Option</span></span>                         | <span data-ttu-id="06e29-183">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="06e29-183">Default</span></span> | <span data-ttu-id="06e29-184">Параметр</span><span class="sxs-lookup"><span data-stu-id="06e29-184">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="06e29-185">Если значение — `true`, ПО промежуточного слоя для интеграции IIS задает свойство `HttpContext.User`, которое прошло [проверку подлинности Windows](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="06e29-185">If `true`, IIS Integration Middleware sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="06e29-186">Если значение — `false`, ПО промежуточного слоя только предоставляет идентификатор для `HttpContext.User` и отвечает на явные запросы защиты от `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="06e29-186">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="06e29-187">Для работы `AutomaticAuthentication` необходимо включить в службах IIS проверку подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="06e29-187">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="06e29-188">Дополнительные сведения см. в статье о [проверке подлинности Windows](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="06e29-188">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="06e29-189">Задает отображаемое имя для пользователей на страницах входа.</span><span class="sxs-lookup"><span data-stu-id="06e29-189">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="06e29-190">Если значение — `true` и если присутствует заголовок запроса `MS-ASPNETCORE-CLIENTCERT`, происходит заполнение `HttpContext.Connection.ClientCertificate`.</span><span class="sxs-lookup"><span data-stu-id="06e29-190">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="06e29-191">Сценарии использования прокси-сервера и подсистемы балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="06e29-191">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="06e29-192">ПО промежуточного слоя для интеграции IIS, которое настраивает ПО промежуточного слоя переадресации заголовков, и модуль ASP.NET Core настраиваются на пересылку схемы (HTTP/HTTPS) и удаленного IP-адреса расположения, где был сформирован запрос.</span><span class="sxs-lookup"><span data-stu-id="06e29-192">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="06e29-193">Для приложений, размещенных за дополнительными прокси-серверами и подсистемами балансировки нагрузки, может потребоваться дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="06e29-193">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="06e29-194">Дополнительные сведения см. в разделе [Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="06e29-194">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="06e29-195">Файл web.config</span><span class="sxs-lookup"><span data-stu-id="06e29-195">web.config file</span></span>

<span data-ttu-id="06e29-196">В файле *web.config* содержится конфигурация [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="06e29-196">The *web.config* file configures the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="06e29-197">Создание, преобразование и публикация файла *web.config* обрабатываются целевым объектом MSBuild (`_TransformWebConfig`) при публикации проекта.</span><span class="sxs-lookup"><span data-stu-id="06e29-197">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="06e29-198">Этот целевой объект присутствует в целевых веб-пакетах SDK (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="06e29-198">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="06e29-199">Пакет SDK задается в начале файла проекта:</span><span class="sxs-lookup"><span data-stu-id="06e29-199">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="06e29-200">Если в проекте нет файла *web.config*, он создается с соответствующими аргументами *processPath* и *arguments* для настройки [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) и переносится в [опубликованные выходные данные](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="06e29-200">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="06e29-201">Если в проекте есть файл *web.config*, он преобразуется с соответствующими аргументами *processPath* и *arguments* для настройки модуля ASP.NET Core и переносится в опубликованные выходные данные.</span><span class="sxs-lookup"><span data-stu-id="06e29-201">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="06e29-202">Преобразование не изменяет параметры конфигурации служб IIS, включенные в файл.</span><span class="sxs-lookup"><span data-stu-id="06e29-202">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="06e29-203">Файл *web.config* может содержать дополнительные параметры конфигурации IIS, управляющие активными модулями IIS.</span><span class="sxs-lookup"><span data-stu-id="06e29-203">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="06e29-204">Сведения о модулях IIS, которые могут обрабатывать запросы к приложениям ASP.NET Core, см. в статье [Модули IIS](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="06e29-204">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="06e29-205">Чтобы пакет SDK не преобразовывал файл *web.config*, добавьте в файл проекта свойство **\<IsTransformWebConfigDisabled>**.</span><span class="sxs-lookup"><span data-stu-id="06e29-205">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="06e29-206">Если пакет SDK не преобразует файл, аргументы *processPath* и *arguments* нужно задать вручную.</span><span class="sxs-lookup"><span data-stu-id="06e29-206">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="06e29-207">Дополнительные сведения см. в разделе [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="06e29-207">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="06e29-208">Расположение файла web.config</span><span class="sxs-lookup"><span data-stu-id="06e29-208">web.config file location</span></span>

<span data-ttu-id="06e29-209">Для корректной настройки [модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module) необходимо наличие файла *web.config* в корневой папке контента развертываемого приложения (как правило, это основной путь приложения).</span><span class="sxs-lookup"><span data-stu-id="06e29-209">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="06e29-210">Это расположение соответствует физическому пути веб-сайта, указанному в службах IIS.</span><span class="sxs-lookup"><span data-stu-id="06e29-210">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="06e29-211">Файл *web.config* требуется в корне приложения, чтобы разрешить публикацию нескольких приложений с помощью веб-развертывания.</span><span class="sxs-lookup"><span data-stu-id="06e29-211">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="06e29-212">По физическому пути приложения находятся файлы с конфиденциальной информацией: *\<имя_сборки>.runtimeconfig.json*, *\<имя_сборки>.xml* (комментарии к XML-документации) и *\<имя_сборки>.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="06e29-212">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="06e29-213">Когда файл *web.config* присутствует и сайт запускается нормально, службы IIS не обрабатывают запросы к этим файлам.</span><span class="sxs-lookup"><span data-stu-id="06e29-213">When the *web.config* file is present and and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="06e29-214">Если файл *web.config* отсутствует, неправильно именован или не может настроить нормальный запуск сайта, службы IIS могут свободно передавать содержимое этих конфиденциальных файлов.</span><span class="sxs-lookup"><span data-stu-id="06e29-214">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="06e29-215">**Файл *web.config* должен постоянно находиться в развертывании, у этого файла должно быть правильное имя, и файл должен быть в состоянии настроить нормальный запуск сайта. Никогда не удаляйте файл *web.config* из развертывания в рабочей среде.**</span><span class="sxs-lookup"><span data-stu-id="06e29-215">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="06e29-216">Конфигурация IIS</span><span class="sxs-lookup"><span data-stu-id="06e29-216">IIS configuration</span></span>

<span data-ttu-id="06e29-217">**Операционные системы семейства Windows Server**</span><span class="sxs-lookup"><span data-stu-id="06e29-217">**Windows Server operating systems**</span></span>

<span data-ttu-id="06e29-218">Включите роль сервера **Веб-сервер (IIS)** и настройте службы роли.</span><span class="sxs-lookup"><span data-stu-id="06e29-218">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="06e29-219">В меню **Управление** запустите мастер **Добавить роли и компоненты** или в окне **Диспетчер серверов** щелкните соответствующую ссылку.</span><span class="sxs-lookup"><span data-stu-id="06e29-219">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="06e29-220">На этапе **Роли сервера** установите флажок **Веб-сервер (IIS)**.</span><span class="sxs-lookup"><span data-stu-id="06e29-220">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![Роль "Веб-сервер (IIS)" выбрана на странице "Выбор ролей сервера".](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="06e29-222">После этапа выбора **компонентов** запускается этап выбора **служб роли** для веб-сервера (IIS).</span><span class="sxs-lookup"><span data-stu-id="06e29-222">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="06e29-223">Выберите нужные службы роли IIS или оставьте службы по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="06e29-223">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![Службы роли по умолчанию, выбранные на странице "Выбор служб ролей".](index/_static/role-services-ws2016.png)

   <span data-ttu-id="06e29-225">**Проверка подлинности Windows (необязательный компонент)**</span><span class="sxs-lookup"><span data-stu-id="06e29-225">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="06e29-226">Чтобы включить проверку подлинности Windows, разверните такие узлы: **Веб-сервер** > **Безопасность**.</span><span class="sxs-lookup"><span data-stu-id="06e29-226">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="06e29-227">Выберите компонент **Проверка подлинности Windows**.</span><span class="sxs-lookup"><span data-stu-id="06e29-227">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="06e29-228">Дополнительные сведения см. в статьях [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) (Проверка подлинности Windows <windowsAuthentication>) и [Configure Windows authentication in an ASP.NET Core app](xref:security/authentication/windowsauth) (Настройка проверки подлинности Windows в приложении ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="06e29-228">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="06e29-229">**Протокол WebSocket (необязательный компонент)**</span><span class="sxs-lookup"><span data-stu-id="06e29-229">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="06e29-230">Протокол WebSocket поддерживается в ASP.NET Core начиная с версии 1.1.</span><span class="sxs-lookup"><span data-stu-id="06e29-230">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="06e29-231">Чтобы включить протокол WebSocket, разверните такие узлы: **Веб-сервер** > **Разработка приложений**.</span><span class="sxs-lookup"><span data-stu-id="06e29-231">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="06e29-232">Выберите компонент **Протокол WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="06e29-232">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="06e29-233">Дополнительные сведения см. в разделе [Протокол WebSocket](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="06e29-233">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="06e29-234">Пройдите этап **Подтверждение**, чтобы установить роль и службы веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="06e29-234">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="06e29-235">После установки роли **Веб-сервер (IIS)** перезагружать сервер или службы IIS не требуется.</span><span class="sxs-lookup"><span data-stu-id="06e29-235">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="06e29-236">**Операционные системы Windows для настольных компьютеров**</span><span class="sxs-lookup"><span data-stu-id="06e29-236">**Windows desktop operating systems**</span></span>

<span data-ttu-id="06e29-237">Включите компоненты **Консоль управления IIS** и **Службы Интернета**.</span><span class="sxs-lookup"><span data-stu-id="06e29-237">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="06e29-238">Последовательно выберите **Панель управления** > **Программы** > **Программы и компоненты** > **Включение или отключение компонентов Windows** (в левой части экрана).</span><span class="sxs-lookup"><span data-stu-id="06e29-238">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="06e29-239">Разверните узел **Службы IIS**.</span><span class="sxs-lookup"><span data-stu-id="06e29-239">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="06e29-240">Разверните узел **Средства управления веб-сайтом**.</span><span class="sxs-lookup"><span data-stu-id="06e29-240">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="06e29-241">Установите флажок **Консоль управления IIS**.</span><span class="sxs-lookup"><span data-stu-id="06e29-241">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="06e29-242">Установите флажок **Службы Интернета**.</span><span class="sxs-lookup"><span data-stu-id="06e29-242">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="06e29-243">В группе **Службы Интернета** оставьте компоненты по умолчанию или настройте компоненты IIS.</span><span class="sxs-lookup"><span data-stu-id="06e29-243">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="06e29-244">**Проверка подлинности Windows (необязательный компонент)**</span><span class="sxs-lookup"><span data-stu-id="06e29-244">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="06e29-245">Чтобы включить проверку подлинности Windows, разверните такие узлы: **Службы Интернета** > **Безопасность**.</span><span class="sxs-lookup"><span data-stu-id="06e29-245">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="06e29-246">Выберите компонент **Проверка подлинности Windows**.</span><span class="sxs-lookup"><span data-stu-id="06e29-246">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="06e29-247">Дополнительные сведения см. в статьях [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) (Проверка подлинности Windows <windowsAuthentication>) и [Configure Windows authentication in an ASP.NET Core app](xref:security/authentication/windowsauth) (Настройка проверки подлинности Windows в приложении ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="06e29-247">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="06e29-248">**Протокол WebSocket (необязательный компонент)**</span><span class="sxs-lookup"><span data-stu-id="06e29-248">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="06e29-249">Протокол WebSocket поддерживается в ASP.NET Core начиная с версии 1.1.</span><span class="sxs-lookup"><span data-stu-id="06e29-249">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="06e29-250">Чтобы включить протокол WebSocket, разверните такие узлы: **Службы Интернета** > **Компоненты разработки приложений**.</span><span class="sxs-lookup"><span data-stu-id="06e29-250">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="06e29-251">Выберите компонент **Протокол WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="06e29-251">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="06e29-252">Дополнительные сведения см. в разделе [Протокол WebSocket](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="06e29-252">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="06e29-253">Если во время установки IIS потребуется перезагрузка, перезагрузите систему.</span><span class="sxs-lookup"><span data-stu-id="06e29-253">If the IIS installation requires a restart, restart the system.</span></span>

![Группы "Консоль управления IIS" и "Службы Интернета" выбраны в окне "Компоненты Windows".](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="06e29-255">Установка пакета размещения .NET Core</span><span class="sxs-lookup"><span data-stu-id="06e29-255">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="06e29-256">Установите *пакет размещения .NET Core* в размещающей системе.</span><span class="sxs-lookup"><span data-stu-id="06e29-256">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="06e29-257">В составе пакета устанавливаются среда выполнения .NET Core, библиотека .NET Core и [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="06e29-257">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="06e29-258">Модуль позволяет запускать приложения ASP.NET Core под управлением IIS.</span><span class="sxs-lookup"><span data-stu-id="06e29-258">The module allows ASP.NET Core apps to run behind IIS.</span></span> <span data-ttu-id="06e29-259">Если система не подключена к Интернету, перед установкой пакета размещения .NET Core получите и установите [Распространяемый компонент Microsoft Visual C++ 2015](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="06e29-259">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Hosting Bundle.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="06e29-260">Если пакет размещения устанавливается до установки служб IIS, его нужно восстановить.</span><span class="sxs-lookup"><span data-stu-id="06e29-260">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="06e29-261">После установки служб IIS запустите установщик пакета размещения еще раз.</span><span class="sxs-lookup"><span data-stu-id="06e29-261">Run the Hosting Bundle installer again after installing IIS.</span></span>

### <a name="direct-download-current-version"></a><span data-ttu-id="06e29-262">Прямая загрузка (текущая версия)</span><span class="sxs-lookup"><span data-stu-id="06e29-262">Direct download (current version)</span></span>

<span data-ttu-id="06e29-263">Скачайте установщик по следующей ссылке:</span><span class="sxs-lookup"><span data-stu-id="06e29-263">Download the installer using the following link:</span></span>

[<span data-ttu-id="06e29-264">Текущий установщик пакета размещения .NET Core (прямая загрузка)</span><span class="sxs-lookup"><span data-stu-id="06e29-264">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a><span data-ttu-id="06e29-265">Более ранние версии установщика</span><span class="sxs-lookup"><span data-stu-id="06e29-265">Earlier versions of the installer</span></span>

<span data-ttu-id="06e29-266">Получение более ранней версии установщика:</span><span class="sxs-lookup"><span data-stu-id="06e29-266">To obtain an earlier version of the installer:</span></span>

1. <span data-ttu-id="06e29-267">Перейдите в [архивы загрузок .NET](https://www.microsoft.com/net/download/archives).</span><span class="sxs-lookup"><span data-stu-id="06e29-267">Navigate to the [.NET download archives](https://www.microsoft.com/net/download/archives).</span></span>
1. <span data-ttu-id="06e29-268">В разделе **.NET Core** выберите версию .NET Core.</span><span class="sxs-lookup"><span data-stu-id="06e29-268">Under **.NET Core**, select the .NET Core version.</span></span>
1. <span data-ttu-id="06e29-269">В столбце **Запуск приложений — среда выполнения** найдите строку, содержащую нужную версию среды выполнения .NET Core.</span><span class="sxs-lookup"><span data-stu-id="06e29-269">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="06e29-270">Скачайте установщик по ссылке **Среда выполнения и пакет размещения**.</span><span class="sxs-lookup"><span data-stu-id="06e29-270">Download the installer using the **Runtime & Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="06e29-271">Некоторые установщики содержат версии выпусков, которые достигли конца своего жизненного цикла и больше не поддерживаются корпорацией Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="06e29-271">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="06e29-272">Дополнительные сведения см. в разделе [Политика поддержки](https://www.microsoft.com/net/download/dotnet-core/2.0).</span><span class="sxs-lookup"><span data-stu-id="06e29-272">For more information, see the [support policy](https://www.microsoft.com/net/download/dotnet-core/2.0).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="06e29-273">Установка пакета размещения .NET Core</span><span class="sxs-lookup"><span data-stu-id="06e29-273">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="06e29-274">Запустите установщик на сервере.</span><span class="sxs-lookup"><span data-stu-id="06e29-274">Run the installer on the server.</span></span> <span data-ttu-id="06e29-275">При запуске установщика из командной строки администратора доступны следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="06e29-275">The following switches are available when running the installer from an administrator command prompt:</span></span>

   * <span data-ttu-id="06e29-276">`OPT_NO_ANCM=1` — пропуск установки модуля ASP.NET Core;</span><span class="sxs-lookup"><span data-stu-id="06e29-276">`OPT_NO_ANCM=1` &ndash; Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="06e29-277">`OPT_NO_RUNTIME=1` — пропуск установки среды выполнения .NET Core;</span><span class="sxs-lookup"><span data-stu-id="06e29-277">`OPT_NO_RUNTIME=1` &ndash; Skip installing the .NET Core runtime.</span></span>
   * <span data-ttu-id="06e29-278">`OPT_NO_SHAREDFX=1` — пропуск установки общей платформы ASP.NET (среды выполнения ASP.NET);</span><span class="sxs-lookup"><span data-stu-id="06e29-278">`OPT_NO_SHAREDFX=1` &ndash; Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span>
   * <span data-ttu-id="06e29-279">`OPT_NO_X86=1` — пропуск установки 32-разрядных сред выполнения.</span><span class="sxs-lookup"><span data-stu-id="06e29-279">`OPT_NO_X86=1` &ndash; Skip installing x86 runtimes.</span></span> <span data-ttu-id="06e29-280">Этот параметр следует использовать, если вы наверняка не будете размещать 32-разрядные приложения.</span><span class="sxs-lookup"><span data-stu-id="06e29-280">Use this switch when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="06e29-281">Если есть хоть малейшая возможность, что в будущем придется размещать и 32-разрядные, и 64-разрядные приложения, не указывайте этот параметр и установите обе среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="06e29-281">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this switch and install both runtimes.</span></span>
1. <span data-ttu-id="06e29-282">Перезагрузите систему или в командой строке выполните команду **net stop was /y**, а затем — команду **net start w3svc**.</span><span class="sxs-lookup"><span data-stu-id="06e29-282">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span> <span data-ttu-id="06e29-283">Перезапуск служб IIS позволит обнаружить внесенные установщиком изменения в системном пути, который является переменной среды.</span><span class="sxs-lookup"><span data-stu-id="06e29-283">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

<span data-ttu-id="06e29-284">Если установщик пакета размещения Windows обнаруживает, что для завершения установки требуется сброс IIS, установщик выполняет этот сброс.</span><span class="sxs-lookup"><span data-stu-id="06e29-284">If the Windows Hosting Bundle installer detects that IIS requires a reset in order to complete installation, the installer resets IIS.</span></span> <span data-ttu-id="06e29-285">Если установщик применяет сброс IIS, перезапускаются все пулы приложений и веб-сайты IIS.</span><span class="sxs-lookup"><span data-stu-id="06e29-285">If the installer triggers an IIS reset, all of the IIS app pools and websites are restarted.</span></span>

> [!NOTE]
> <span data-ttu-id="06e29-286">Сведения об общей конфигурации IIS см. в разделе [Модуль ASP.NET Core с общей конфигурацией IIS](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span><span class="sxs-lookup"><span data-stu-id="06e29-286">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="06e29-287">Установка веб-развертывания при публикации с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="06e29-287">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="06e29-288">При развертывании приложений на серверах с помощью [веб-развертывания](/iis/publish/using-web-deploy/introduction-to-web-deploy) установите на сервере последнюю версию веб-развертывания.</span><span class="sxs-lookup"><span data-stu-id="06e29-288">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="06e29-289">Чтобы установить веб-развертывание, можно использовать [установщик веб-платформы (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) или получить установщик непосредственно в [Центре загрузки Майкрософт](https://www.microsoft.com/download/details.aspx?id=43717).</span><span class="sxs-lookup"><span data-stu-id="06e29-289">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="06e29-290">Предпочтительный метод — использовать WebPI.</span><span class="sxs-lookup"><span data-stu-id="06e29-290">The preferred method is to use WebPI.</span></span> <span data-ttu-id="06e29-291">WebPI обеспечивает автономную установку и настройку поставщиков размещения.</span><span class="sxs-lookup"><span data-stu-id="06e29-291">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="06e29-292">Создание сайта IIS</span><span class="sxs-lookup"><span data-stu-id="06e29-292">Create the IIS site</span></span>

1. <span data-ttu-id="06e29-293">В размещающей системе создайте папку, в которой будут храниться файлы и папки опубликованного приложения.</span><span class="sxs-lookup"><span data-stu-id="06e29-293">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="06e29-294">Макет развертывания приложения описан в статье [Directory structure of published ASP.NET Core apps](xref:host-and-deploy/directory-structure) (Структура каталогов опубликованных приложений ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="06e29-294">An app's deployment layout is described in the [Directory Structure](xref:host-and-deploy/directory-structure) topic.</span></span>

1. <span data-ttu-id="06e29-295">В созданной только что папке создайте папку *logs*, в которой будут храниться журналы StdOut модуля ASP.NET Core (если включено ведение таких журналов).</span><span class="sxs-lookup"><span data-stu-id="06e29-295">Within the new folder, create a *logs* folder to hold ASP.NET Core Module stdout logs when stdout logging is enabled.</span></span> <span data-ttu-id="06e29-296">Если приложение развертывается с папкой *logs* в полезных данных, пропустите этот шаг.</span><span class="sxs-lookup"><span data-stu-id="06e29-296">If the app is deployed with a *logs* folder in the payload, skip this step.</span></span> <span data-ttu-id="06e29-297">Сведения о том, как в MSBuild настроить автоматическое создание папки *logs* при создании проекта локально, см. в статье [Directory structure of published ASP.NET Core apps](xref:host-and-deploy/directory-structure) (Структура каталогов опубликованных приложений ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="06e29-297">For instructions on how to enable MSBuild to create the *logs* folder automatically when the project is built locally, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="06e29-298">Журнал StdOut следует использовать только для устранения ошибок, возникающих при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="06e29-298">Only use the stdout log to troubleshoot app startup failures.</span></span> <span data-ttu-id="06e29-299">Никогда не используйте журнал StdOut как журнал повседневных операций приложения.</span><span class="sxs-lookup"><span data-stu-id="06e29-299">Never use stdout logging for routine app logging.</span></span> <span data-ttu-id="06e29-300">Ни размер файла журнала, ни количество создаваемых файлов журналов ничем не ограничены.</span><span class="sxs-lookup"><span data-stu-id="06e29-300">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="06e29-301">Пул приложений должен иметь доступ на запись к папке, куда записываются журналы.</span><span class="sxs-lookup"><span data-stu-id="06e29-301">The app pool must have write access to the location where the logs are written.</span></span> <span data-ttu-id="06e29-302">Все папки в пути к папке журнала должны существовать.</span><span class="sxs-lookup"><span data-stu-id="06e29-302">All of the folders on the path to the log location must exist.</span></span> <span data-ttu-id="06e29-303">Дополнительные сведения о журнале StdOut см. в разделе о [создании и перенаправлении журналов](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span><span class="sxs-lookup"><span data-stu-id="06e29-303">For more information on the stdout log, see [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span> <span data-ttu-id="06e29-304">Сведения о ведении журнала приложения ASP.NET Core см. в статье [Общие сведения о ведении журналов в ASP.NET Core](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="06e29-304">For information on logging in an ASP.NET Core app, see the [Logging](xref:fundamentals/logging/index) topic.</span></span>

1. <span data-ttu-id="06e29-305">В окне **Диспетчер служб IIS** в области **Подключения** разверните узел сервера.</span><span class="sxs-lookup"><span data-stu-id="06e29-305">In **IIS Manager**, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="06e29-306">Щелкните правой кнопкой мыши папку **Сайты**.</span><span class="sxs-lookup"><span data-stu-id="06e29-306">Right-click the **Sites** folder.</span></span> <span data-ttu-id="06e29-307">В контекстном меню выберите пункт **Добавить веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="06e29-307">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="06e29-308">Укажите имя в поле **Имя сайта** и задайте значение в поле **Физический путь** для созданной папки развертывания приложения.</span><span class="sxs-lookup"><span data-stu-id="06e29-308">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="06e29-309">Укажите конфигурацию **привязки** и нажмите кнопку **ОК**, чтобы создать веб-сайт.</span><span class="sxs-lookup"><span data-stu-id="06e29-309">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![В окне "Добавить веб-сайт" укажите имя сайта, физический путь и имя узла.](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="06e29-311">**Не используйте** привязки с подстановочными знаками (`http://*:80/` и `http://+:80`) на верхнем уровне.</span><span class="sxs-lookup"><span data-stu-id="06e29-311">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="06e29-312">Это может создать уязвимость и поставить ваше приложение под угрозу.</span><span class="sxs-lookup"><span data-stu-id="06e29-312">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="06e29-313">Сюда относятся и строгие, и нестрогие подстановочные знаки.</span><span class="sxs-lookup"><span data-stu-id="06e29-313">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="06e29-314">Вместо этого используйте имена узлов в явном виде.</span><span class="sxs-lookup"><span data-stu-id="06e29-314">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="06e29-315">Привязки с подстановочными знаками на уровне дочерних доменов (например `*.mysub.com`) не создают таких угроз безопасности, если вы полностью контролируете родительский домен (в отличие от варианта `*.com`, создающего уязвимость).</span><span class="sxs-lookup"><span data-stu-id="06e29-315">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="06e29-316">Дополнительные сведения см. в документе [rfc7230, раздел 5.4](https://tools.ietf.org/html/rfc7230#section-5.4).</span><span class="sxs-lookup"><span data-stu-id="06e29-316">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="06e29-317">Разверните узел сервера и выберите **Пулы приложений**.</span><span class="sxs-lookup"><span data-stu-id="06e29-317">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="06e29-318">Щелкните правой кнопкой мыши пул приложений сайта и в контекстном меню выберите пункт **Основные параметры**.</span><span class="sxs-lookup"><span data-stu-id="06e29-318">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="06e29-319">В окне **Изменение пула приложений** задайте для параметра **Версия среды CLR .NET** значение **Без управляемого кода**.</span><span class="sxs-lookup"><span data-stu-id="06e29-319">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![Выбор значения "Без управляемого кода" для параметра "Версия среды CLR .NET".](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="06e29-321">ASP.NET Core выполняется в отдельном процессе и управляет средой выполнения.</span><span class="sxs-lookup"><span data-stu-id="06e29-321">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="06e29-322">Для ASP.NET Core не требуется загрузка классической среды CLR.</span><span class="sxs-lookup"><span data-stu-id="06e29-322">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span> <span data-ttu-id="06e29-323">Задавать для параметра **Версия среды CLR .NET** значение **Без управляемого кода** необязательно.</span><span class="sxs-lookup"><span data-stu-id="06e29-323">Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span>

1. <span data-ttu-id="06e29-324">Убедитесь в том, что удостоверение модели процесса имеет соответствующие разрешения.</span><span class="sxs-lookup"><span data-stu-id="06e29-324">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="06e29-325">При изменении удостоверения по умолчанию для пула приложений (**Модель процесса** > **Удостоверение**) с **ApplicationPoolIdentity** на другое, убедитесь, что у нового удостоверения есть соответствующие разрешения для доступа к папке приложения, базе данных и другим необходимым ресурсам.</span><span class="sxs-lookup"><span data-stu-id="06e29-325">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="06e29-326">Например, пулу приложений требуются права на чтение и запись в папках, в которых приложение считывает и записывает файлы.</span><span class="sxs-lookup"><span data-stu-id="06e29-326">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="06e29-327">**Настройка проверки подлинности Windows (необязательный этап)**</span><span class="sxs-lookup"><span data-stu-id="06e29-327">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="06e29-328">См. статью [Configure Windows authentication in an ASP.NET Core app](xref:security/authentication/windowsauth) (Настройка проверки подлинности Windows в приложении ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="06e29-328">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="06e29-329">Развертывание приложения</span><span class="sxs-lookup"><span data-stu-id="06e29-329">Deploy the app</span></span>

<span data-ttu-id="06e29-330">Разверните приложение в папке, созданной в размещающей системе.</span><span class="sxs-lookup"><span data-stu-id="06e29-330">Deploy the app to the folder created on the hosting system.</span></span> <span data-ttu-id="06e29-331">Рекомендуемый механизм развертывания — [веб-развертывание](/iis/publish/using-web-deploy/introduction-to-web-deploy).</span><span class="sxs-lookup"><span data-stu-id="06e29-331">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="06e29-332">Веб-развертывание с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="06e29-332">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="06e29-333">Сведения о создании профиля публикации для веб-развертывания см. в статье [Профили публикации Visual Studio для развертывания приложений ASP.NET Core](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="06e29-333">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="06e29-334">Если поставщик услуг размещения предоставляет профиль публикации или позволяет его создать, скачайте этот профиль и импортируйте его с помощью диалогового окна **Публикация** в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="06e29-334">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![Диалоговое окно "Публикация"](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="06e29-336">Веб-развертывание вне Visual Studio</span><span class="sxs-lookup"><span data-stu-id="06e29-336">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="06e29-337">[Веб-развертывание](/iis/publish/using-web-deploy/introduction-to-web-deploy) можно также использовать вне Visual Studio с помощью командной строки.</span><span class="sxs-lookup"><span data-stu-id="06e29-337">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="06e29-338">Дополнительные сведения см. в статье [Средство веб-развертывания](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span><span class="sxs-lookup"><span data-stu-id="06e29-338">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="06e29-339">Альтернативы веб-развертыванию</span><span class="sxs-lookup"><span data-stu-id="06e29-339">Alternatives to Web Deploy</span></span>

<span data-ttu-id="06e29-340">Переместить приложение в размещающую систему можно несколькими способами: с помощью копирования вручную, Xcopy, Robocopy или PowerShell.</span><span class="sxs-lookup"><span data-stu-id="06e29-340">Use any of several methods to move the app to the hosting system, such as manual copy, Xcopy, Robocopy, or PowerShell.</span></span>

<span data-ttu-id="06e29-341">Дополнительные сведения о развертывании ASP.NET Core в службах IIS см. в разделе [Ресурсы развертывания для администраторов IIS](#deployment-resources-for-iis-administrators).</span><span class="sxs-lookup"><span data-stu-id="06e29-341">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="06e29-342">Обзор веб-сайта</span><span class="sxs-lookup"><span data-stu-id="06e29-342">Browse the website</span></span>

![Начальная страница IIS, загруженная в браузере Microsoft Edge.](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="06e29-344">Заблокированные файлы развертывания</span><span class="sxs-lookup"><span data-stu-id="06e29-344">Locked deployment files</span></span>

<span data-ttu-id="06e29-345">Во время выполнения приложения файлы в папке развертывания блокируются.</span><span class="sxs-lookup"><span data-stu-id="06e29-345">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="06e29-346">Заблокированные файлы невозможно перезаписать во время развертывания.</span><span class="sxs-lookup"><span data-stu-id="06e29-346">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="06e29-347">Чтобы снять блокировку с файлов в развертывании, остановите пул приложений с помощью **одного** из следующих методов:</span><span class="sxs-lookup"><span data-stu-id="06e29-347">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="06e29-348">Запустите веб-развертывание и добавьте ссылку на `Microsoft.NET.Sdk.Web` в файл проекта.</span><span class="sxs-lookup"><span data-stu-id="06e29-348">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="06e29-349">Файл *app_offline.htm* помещается в корень каталога веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="06e29-349">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="06e29-350">Если файл присутствует, модуль ASP.NET Core корректно завершает работу приложения и обслуживает файл *app_offline.htm* во время развертывания.</span><span class="sxs-lookup"><span data-stu-id="06e29-350">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="06e29-351">Дополнительные сведения см. в разделе [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="06e29-351">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="06e29-352">Вручную остановите пул приложений в диспетчере служб IIS на сервере.</span><span class="sxs-lookup"><span data-stu-id="06e29-352">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="06e29-353">С помощью PowerShell удалите *app_offline.html* (требуется PowerShell 5 или более поздняя версия):</span><span class="sxs-lookup"><span data-stu-id="06e29-353">Use PowerShell to drop *app_offline.html* (requires PowerShell 5 or later):</span></span>

  ```PowerShell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="06e29-354">Защита данных</span><span class="sxs-lookup"><span data-stu-id="06e29-354">Data protection</span></span>

<span data-ttu-id="06e29-355">[Стек защиты данных ASP.NET Core](xref:security/data-protection/introduction) используют несколько [ПО промежуточного слоя](xref:fundamentals/middleware/index) ASP.NET Core, включая ПО, которое применяется для аутентификации.</span><span class="sxs-lookup"><span data-stu-id="06e29-355">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="06e29-356">Даже если API-интерфейсы защиты данных не вызываются из пользовательского кода, защиту данных следует настроить для создания постоянного [хранилища криптографических ключей](xref:security/data-protection/implementation/key-management). Это можно сделать с помощью скрипта развертывания или в пользовательском коде.</span><span class="sxs-lookup"><span data-stu-id="06e29-356">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="06e29-357">Если защита данных не настроена, ключи хранятся в памяти и удаляются при перезапуске приложения.</span><span class="sxs-lookup"><span data-stu-id="06e29-357">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="06e29-358">Если набор ключей хранится в памяти, при перезапуске приложения происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="06e29-358">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="06e29-359">Все токены аутентификации, использующие файлы cookie, становятся недействительными.</span><span class="sxs-lookup"><span data-stu-id="06e29-359">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="06e29-360">При выполнении следующего запроса пользователю требуется выполнить вход снова.</span><span class="sxs-lookup"><span data-stu-id="06e29-360">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="06e29-361">Все данные, защищенные с помощью набора ключей, больше не могут быть расшифрованы.</span><span class="sxs-lookup"><span data-stu-id="06e29-361">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="06e29-362">Это могут быть [токены CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) и [файлы cookie временных данных ASP.NET Core MVC](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="06e29-362">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="06e29-363">Чтобы настроить защиту данных в службах IIS для хранения набора ключей, воспользуйтесь **одним** из следующих методов:</span><span class="sxs-lookup"><span data-stu-id="06e29-363">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="06e29-364">**Создание раздела реестра для защиты данных**.</span><span class="sxs-lookup"><span data-stu-id="06e29-364">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="06e29-365">Ключи защиты данных, используемые приложениями ASP.NET Core, хранятся во внешнем для приложений реестре.</span><span class="sxs-lookup"><span data-stu-id="06e29-365">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="06e29-366">Чтобы хранить эти ключи для определенного приложения, нужно создать разделы реестра для пула приложений.</span><span class="sxs-lookup"><span data-stu-id="06e29-366">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="06e29-367">При автономной установке служб IIS, не поддерживающей веб-ферму, можно выполнить [скрипт PowerShell Provision-AutoGenKeys.ps1 для защиты данных](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) применительно к каждому пулу приложений, в который входит приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="06e29-367">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="06e29-368">Этот скрипт создает раздел в реестре HKLM, который будет доступен только для учетной записи рабочего процесса пула приложений, к которому относится соответствующее приложение.</span><span class="sxs-lookup"><span data-stu-id="06e29-368">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="06e29-369">Неактивные ключи шифруются с помощью API защиты данных с ключом компьютера.</span><span class="sxs-lookup"><span data-stu-id="06e29-369">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="06e29-370">В случаях, когда используется веб-ферма, в приложении можно настроить UNC-путь, по которому это приложение будет хранить набор ключей для защиты данных.</span><span class="sxs-lookup"><span data-stu-id="06e29-370">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="06e29-371">По умолчанию ключи защиты данных не шифруются.</span><span class="sxs-lookup"><span data-stu-id="06e29-371">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="06e29-372">Разрешения на доступ к файлам в сетевой папке должны быть предоставлены только учетной записи Windows, с помощью которой выполняется приложение.</span><span class="sxs-lookup"><span data-stu-id="06e29-372">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="06e29-373">Для защиты неактивных ключей можно использовать сертификат X509.</span><span class="sxs-lookup"><span data-stu-id="06e29-373">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="06e29-374">Рассмотрите возможность реализации механизма, позволяющего пользователям отправлять сертификаты: поместить сертификаты в хранилище доверенных сертификатов пользователя и обеспечивать их доступность на всех компьютерах, где выполняется приложение.</span><span class="sxs-lookup"><span data-stu-id="06e29-374">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="06e29-375">Дополнительные сведения см. в разделе [Настройка защиты данных в ASP.NET Core](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="06e29-375">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="06e29-376">**Настройка пула приложений IIS для загрузки профиля пользователя**.</span><span class="sxs-lookup"><span data-stu-id="06e29-376">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="06e29-377">Этот параметр находится на странице **Дополнительные параметры** пула приложений в разделе **Модель процесса**.</span><span class="sxs-lookup"><span data-stu-id="06e29-377">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="06e29-378">Задайте для параметра "Загрузить профиль пользователя" значение `True`.</span><span class="sxs-lookup"><span data-stu-id="06e29-378">Set Load User Profile to `True`.</span></span> <span data-ttu-id="06e29-379">В результате ключи будут храниться в каталоге профиля пользователя, а их безопасность будет обеспечиваться за счет применения API защиты данных и ключа на уровне учетной записи пользователя, которую использует пул приложений.</span><span class="sxs-lookup"><span data-stu-id="06e29-379">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used by the app pool.</span></span>

* <span data-ttu-id="06e29-380">**Использование файловой системы в качестве хранилища набора ключей**.</span><span class="sxs-lookup"><span data-stu-id="06e29-380">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="06e29-381">Измените код приложения так, чтобы [в качестве хранилища набора ключей использовалась файловая система](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="06e29-381">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="06e29-382">Для защиты набора ключей используйте доверенный сертификат X509.</span><span class="sxs-lookup"><span data-stu-id="06e29-382">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="06e29-383">Если сертификат — самозаверяющий, поместите его в доверенное корневое хранилище.</span><span class="sxs-lookup"><span data-stu-id="06e29-383">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="06e29-384">При использовании служб IIS в веб-ферме:</span><span class="sxs-lookup"><span data-stu-id="06e29-384">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="06e29-385">используйте общую папку, которая доступна всем компьютерам;</span><span class="sxs-lookup"><span data-stu-id="06e29-385">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="06e29-386">разверните сертификат X509 на каждом компьютере;</span><span class="sxs-lookup"><span data-stu-id="06e29-386">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="06e29-387">настройте [защиту данных в коде](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="06e29-387">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="06e29-388">**Настройка политики защиты данных на уровне компьютера**.</span><span class="sxs-lookup"><span data-stu-id="06e29-388">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="06e29-389">Система защиты данных обеспечивает ограниченную поддержку задания [политики по умолчанию на уровне компьютера](xref:security/data-protection/configuration/machine-wide-policy) для всех приложений, использующих интерфейсы API защиты данных.</span><span class="sxs-lookup"><span data-stu-id="06e29-389">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="06e29-390">Дополнительные сведения см. в разделе <xref:security/data-protection/introduction>.</span><span class="sxs-lookup"><span data-stu-id="06e29-390">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="sub-application-configuration"></a><span data-ttu-id="06e29-391">Конфигурация дочерних приложений</span><span class="sxs-lookup"><span data-stu-id="06e29-391">Sub-application configuration</span></span>

<span data-ttu-id="06e29-392">Дочерние приложения, добавленные в корневое приложение, не должны включать в себя модуль ASP.NET Core в качестве обработчика.</span><span class="sxs-lookup"><span data-stu-id="06e29-392">Sub-apps added under the root app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="06e29-393">Если модуль добавляется в качестве обработчика в файл *web.config* дочернего приложения, при попытке работы с этим приложением выводится ошибка *500.19* (внутренняя ошибка сервера) с указанием некорректного файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="06e29-393">If the module is added as a handler in a sub-app's *web.config* file, a *500.19 Internal Server Error* referencing the faulty config file is received when attempting to browse the sub-app.</span></span>

<span data-ttu-id="06e29-394">Вот пример опубликованного файла *web.config* для дочернего приложения ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="06e29-394">The following example shows a published *web.config* file for an ASP.NET Core sub-app:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <aspNetCore processPath="dotnet" 
        arguments=".\MyApp.dll" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
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
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="06e29-395">При размещении дочернего приложения не на основе ASP.NET Core в приложении ASP.NET Core, нужно явным образом удалить унаследованный обработчик из файла *web.config* дочернего приложения.</span><span class="sxs-lookup"><span data-stu-id="06e29-395">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="06e29-396">Дополнительные сведения о настройке модуля ASP.NET Core см. в статье [Введение в модуль ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) и в [справочнике по настройке модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="06e29-396">For more information on configuring the ASP.NET Core Module, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="06e29-397">Настройка служб IIS с помощью файла web.config</span><span class="sxs-lookup"><span data-stu-id="06e29-397">Configuration of IIS with web.config</span></span>

<span data-ttu-id="06e29-398">Раздел **\<system.webServer>** файла *web.config* действует для тех компонентов IIS, которые относятся к конфигурации прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="06e29-398">IIS configuration is influenced by the **\<system.webServer>** section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="06e29-399">Если в службах IIS на уровне сервера настроено динамическое сжатие, элемент **\<urlCompression>** в файле *web.config* приложения может отключить это сжатие.</span><span class="sxs-lookup"><span data-stu-id="06e29-399">If IIS is configured at the server level to use dynamic compression, the **\<urlCompression>** element in the app's *web.config* file can disable it.</span></span>

<span data-ttu-id="06e29-400">Дополнительные сведения см. в [справочнике по настройке \<system.webServer>](/iis/configuration/system.webServer/), [справочнике по настройке модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module) и статье [Модули IIS с ASP.NET Core](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="06e29-400">For more information, see the [configuration reference for \<system.webServer>](/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module), and [IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="06e29-401">Сведения о настройке переменных среды для отдельных приложений, выполняющихся в изолированных пулах приложений (такая возможность поддерживается в службах IIS начиная с версии 10.0), см. в справочной документации по службам IIS, в частности в разделе *AppCmd.exe* статьи [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) (Переменные среды <environmentVariables>).</span><span class="sxs-lookup"><span data-stu-id="06e29-401">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="06e29-402">Разделы конфигурации файла web.config</span><span class="sxs-lookup"><span data-stu-id="06e29-402">Configuration sections of web.config</span></span>

<span data-ttu-id="06e29-403">Разделы конфигурации приложений ASP.NET 4.x в файле *web.config* не используются для конфигурации приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="06e29-403">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* <span data-ttu-id="06e29-404">**\<system.web>**</span><span class="sxs-lookup"><span data-stu-id="06e29-404">**\<system.web>**</span></span>
* <span data-ttu-id="06e29-405">**\<appSettings>**</span><span class="sxs-lookup"><span data-stu-id="06e29-405">**\<appSettings>**</span></span>
* <span data-ttu-id="06e29-406">**\<connectionStrings >**</span><span class="sxs-lookup"><span data-stu-id="06e29-406">**\<connectionStrings>**</span></span>
* <span data-ttu-id="06e29-407">**\<location>**</span><span class="sxs-lookup"><span data-stu-id="06e29-407">**\<location>**</span></span>

<span data-ttu-id="06e29-408">Для настройки приложений ASP.NET Core используются другие поставщики конфигураций.</span><span class="sxs-lookup"><span data-stu-id="06e29-408">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="06e29-409">Дополнительные сведения см. в разделе [Конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="06e29-409">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="06e29-410">Пулы приложений</span><span class="sxs-lookup"><span data-stu-id="06e29-410">Application Pools</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="06e29-411">Модель размещения определяет изоляцию пула приложений:</span><span class="sxs-lookup"><span data-stu-id="06e29-411">App pool isolation is determined by the hosting model:</span></span>

* <span data-ttu-id="06e29-412">Внутрипроцессное размещение &ndash; Приложения должны работать в отдельных пулах.</span><span class="sxs-lookup"><span data-stu-id="06e29-412">In-process hosting &ndash; Apps are required to run in separate app pools.</span></span>
* <span data-ttu-id="06e29-413">Внепроцессное размещение &ndash; Рекомендуем изолировать приложения друг от друга, запуская каждое из них в собственном пуле.</span><span class="sxs-lookup"><span data-stu-id="06e29-413">Out-of-process hosting &ndash; We recommend isolating the apps from each other by running each app in its own app pool.</span></span>

<span data-ttu-id="06e29-414">В диалоговом окне **Добавление веб-сайта** IIS на каждое приложение по умолчанию задан один пул приложений.</span><span class="sxs-lookup"><span data-stu-id="06e29-414">The IIS **Add Website** dialog defaults to a single app pool per app.</span></span> <span data-ttu-id="06e29-415">Если указано **Имя сайта**, оно автоматически переносится в текстовое поле **Пул приложений**.</span><span class="sxs-lookup"><span data-stu-id="06e29-415">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="06e29-416">При добавлении веб-сайта создается пул приложений с именем сайта.</span><span class="sxs-lookup"><span data-stu-id="06e29-416">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="06e29-417">При размещении на сервере множества веб-сайтов рекомендуем изолировать приложения друг от друга, запуская каждое из них в собственном пуле.</span><span class="sxs-lookup"><span data-stu-id="06e29-417">When hosting multiple websites on a server, we recommend isolating the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="06e29-418">В диалоговом окне **Добавить веб-сайт** служб IIS такая конфигурация задана по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="06e29-418">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="06e29-419">Если указано **Имя сайта**, оно автоматически переносится в текстовое поле **Пул приложений**.</span><span class="sxs-lookup"><span data-stu-id="06e29-419">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="06e29-420">При добавлении веб-сайта создается пул приложений с именем сайта.</span><span class="sxs-lookup"><span data-stu-id="06e29-420">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

## <a name="application-pool-identity"></a><span data-ttu-id="06e29-421">Удостоверение пула приложений</span><span class="sxs-lookup"><span data-stu-id="06e29-421">Application Pool Identity</span></span>

<span data-ttu-id="06e29-422">Учетная запись удостоверения пула приложений позволяет запускать приложение от имени уникальной учетной записи, не создавая домены или локальные учетные записи и не управляя ими.</span><span class="sxs-lookup"><span data-stu-id="06e29-422">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="06e29-423">В службах IIS начиная с версии 8.0 рабочий процесс администратора IIS (WAS) создает виртуальную учетную запись с именем нового пула приложений и по умолчанию выполняет рабочие процессы пула приложений с помощью этой учетной записи.</span><span class="sxs-lookup"><span data-stu-id="06e29-423">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="06e29-424">В консоли управления IIS в окне **Дополнительные параметры** для пула приложений должно быть выбрано **Удостоверение** **ApplicationPoolIdentity**.</span><span class="sxs-lookup"><span data-stu-id="06e29-424">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![Диалоговое окно дополнительных параметров для пула приложений](index/_static/apppool-identity.png)

<span data-ttu-id="06e29-426">Процесс управления IIS создает в системе безопасности Windows безопасный идентификатор с именем пула приложений.</span><span class="sxs-lookup"><span data-stu-id="06e29-426">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="06e29-427">С помощью этого идентификатора можно защищать ресурсы,</span><span class="sxs-lookup"><span data-stu-id="06e29-427">Resources can be secured using this identity.</span></span> <span data-ttu-id="06e29-428">но он не является реальной учетной записью и не отображается в консоли управления пользователями Windows.</span><span class="sxs-lookup"><span data-stu-id="06e29-428">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="06e29-429">Если рабочему процессу IIS требуется предоставить доступ к приложению с повышенными правами, измените список управления доступом (ACL) для каталога, содержащего приложение.</span><span class="sxs-lookup"><span data-stu-id="06e29-429">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="06e29-430">Откройте проводник и перейдите к каталогу.</span><span class="sxs-lookup"><span data-stu-id="06e29-430">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="06e29-431">Щелкните каталог правой кнопкой мыши и выберите пункт **Свойства**.</span><span class="sxs-lookup"><span data-stu-id="06e29-431">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="06e29-432">На вкладке **Безопасность** нажмите кнопку **Изменить**, а затем — кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="06e29-432">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="06e29-433">Нажмите кнопку **Размещение** и выберите систему.</span><span class="sxs-lookup"><span data-stu-id="06e29-433">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="06e29-434">В поле **Введите имена выбираемых объектов** введите **IIS AppPool\\<имя_пула_приложений>**.</span><span class="sxs-lookup"><span data-stu-id="06e29-434">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="06e29-435">Нажмите кнопку **Проверить имена**.</span><span class="sxs-lookup"><span data-stu-id="06e29-435">Select the **Check Names** button.</span></span> <span data-ttu-id="06e29-436">В случае с *DefaultAppPool* проверьте имена с помощью **IIS AppPool\DefaultAppPool**.</span><span class="sxs-lookup"><span data-stu-id="06e29-436">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="06e29-437">После нажатия кнопки **Проверить имена** в области имен объектов отобразится значение **DefaultAppPool**.</span><span class="sxs-lookup"><span data-stu-id="06e29-437">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="06e29-438">Вручную ввести имя пула приложений в области имен объектов нельзя.</span><span class="sxs-lookup"><span data-stu-id="06e29-438">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="06e29-439">При поиске имени объекта используйте формат **IIS AppPool\\<имя_пула_приложений>**.</span><span class="sxs-lookup"><span data-stu-id="06e29-439">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![Диалоговое окно выбора пользователей и групп для папки приложения. До нажатия кнопки "Проверить имена" в области имен объектов к строке IIS AppPool\" добавилось имя пула приложений, DefaultAppPool.](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="06e29-441">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="06e29-441">Select **OK**.</span></span>

   ![Диалоговое окно выбора пользователей и групп для папки приложения. После нажатия кнопки "Проверить имена" в области имен объектов отображается имя пула приложений, DefaultAppPool.](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="06e29-443">Разрешения на чтение и выполнение предоставляются по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="06e29-443">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="06e29-444">При необходимости предоставьте дополнительные разрешения.</span><span class="sxs-lookup"><span data-stu-id="06e29-444">Provide additional permissions as needed.</span></span>

<span data-ttu-id="06e29-445">Кроме того, доступ можно предоставить через командную строку, используя средство **ICACLS**.</span><span class="sxs-lookup"><span data-stu-id="06e29-445">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="06e29-446">В случае с *DefaultAppPool* выполняется такая команда:</span><span class="sxs-lookup"><span data-stu-id="06e29-446">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="06e29-447">Дополнительные сведения об ICACLS см. [здесь](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="06e29-447">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="06e29-448">Поддержка HTTP/2</span><span class="sxs-lookup"><span data-stu-id="06e29-448">HTTP/2 support</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="06e29-449">[HTTP/2](https://httpwg.org/specs/rfc7540.html) поддерживается в ASP.NET Core для следующих сценариев развертывания IIS:</span><span class="sxs-lookup"><span data-stu-id="06e29-449">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="06e29-450">Внутрипроцессно</span><span class="sxs-lookup"><span data-stu-id="06e29-450">In-process</span></span>
  * <span data-ttu-id="06e29-451">Windows Server 2016 / Windows 10 или более поздних версий; IIS 10 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="06e29-451">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="06e29-452">Подключение TLS 1.2 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="06e29-452">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="06e29-453">Внепроцессно</span><span class="sxs-lookup"><span data-stu-id="06e29-453">Out-of-process</span></span>
  * <span data-ttu-id="06e29-454">Windows Server 2016 / Windows 10 или более поздних версий; IIS 10 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="06e29-454">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="06e29-455">Подключения к пограничным серверам, открытых для общего доступа, выполняются по протоколу HTTP/2, а подключения к [серверу Kestrel](xref:fundamentals/servers/kestrel) через обратный прокси-сервер — по HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="06e29-455">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
  * <span data-ttu-id="06e29-456">Подключение TLS 1.2 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="06e29-456">TLS 1.2 or later connection</span></span>

<span data-ttu-id="06e29-457">При внутрипроцессном развертывании и установленном подключении HTTP/2 [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) возвращает `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="06e29-457">For an in-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="06e29-458">При внепроцессном развертывании и установленном подключении HTTP/2 [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) возвращает `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="06e29-458">For an out-of-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="06e29-459">Дополнительные сведения о моделях размещения внутри и вне процесса см. в статьях <xref:fundamentals/servers/aspnet-core-module> и <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="06e29-459">For more information on the in-process and out-of-process hosting models, see the <xref:fundamentals/servers/aspnet-core-module> topic and the <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="06e29-460">[HTTP/2](https://httpwg.org/specs/rfc7540.html) поддерживается для внепроцессных развертываний, которые удовлетворяют следующим базовым требованиям:</span><span class="sxs-lookup"><span data-stu-id="06e29-460">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported for out-of-process deployments that meet the following base requirements:</span></span>

* <span data-ttu-id="06e29-461">Windows Server 2016 / Windows 10 или более поздних версий; IIS 10 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="06e29-461">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
* <span data-ttu-id="06e29-462">Подключения к пограничным серверам, открытых для общего доступа, выполняются по протоколу HTTP/2, а подключения к [серверу Kestrel](xref:fundamentals/servers/kestrel) через обратный прокси-сервер — по HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="06e29-462">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
* <span data-ttu-id="06e29-463">Целевая платформа: неприменимо к развертываниям вне процесса, так как IIS полностью обрабатывает подключение HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="06e29-463">Target framework: Not applicable to out-of-process deployments, since the HTTP/2 connection is handled entirely by IIS.</span></span>
* <span data-ttu-id="06e29-464">Подключение TLS 1.2 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="06e29-464">TLS 1.2 or later connection</span></span>

<span data-ttu-id="06e29-465">Если установлено подключение HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) возвращает `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="06e29-465">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="06e29-466">Протокол HTTP/2 по умолчанию включен.</span><span class="sxs-lookup"><span data-stu-id="06e29-466">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="06e29-467">Если не удается установить подключение HTTP/2, применяется резервный вариант HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="06e29-467">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="06e29-468">Дополнительные сведения о настройке HTTP/2 для развертываний IIS см. в статье [об HTTP/2 в IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span><span class="sxs-lookup"><span data-stu-id="06e29-468">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="06e29-469">Ресурсы развертывания для администраторов IIS</span><span class="sxs-lookup"><span data-stu-id="06e29-469">Deployment resources for IIS administrators</span></span>

<span data-ttu-id="06e29-470">Дополнительные сведения о службах IIS см. в документации по ним.</span><span class="sxs-lookup"><span data-stu-id="06e29-470">Learn about IIS in-depth in the IIS documentation.</span></span>  
[<span data-ttu-id="06e29-471">Документация по службам IIS</span><span class="sxs-lookup"><span data-stu-id="06e29-471">IIS documentation</span></span>](/iis)

<span data-ttu-id="06e29-472">Дополнительные сведения о моделях развертывания приложения .NET Core.</span><span class="sxs-lookup"><span data-stu-id="06e29-472">Learn about .NET Core app deployment models.</span></span>  
[<span data-ttu-id="06e29-473">Развертывание приложений .NET Core</span><span class="sxs-lookup"><span data-stu-id="06e29-473">.NET Core application deployment</span></span>](/dotnet/core/deploying/)

<span data-ttu-id="06e29-474">Сведения о том, как модуль ASP.NET Core позволяет веб-серверу Kestrel использовать IIS или IIS Express в качестве обратного прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="06e29-474">Learn how the ASP.NET Core Module allows the Kestrel web server to use IIS or IIS Express as a reverse proxy server.</span></span>  
[<span data-ttu-id="06e29-475">Модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="06e29-475">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)

<span data-ttu-id="06e29-476">Сведения о настройке модуля ASP.NET Core для размещения приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="06e29-476">Learn how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span>  
[<span data-ttu-id="06e29-477">Справочник по конфигурации модуля ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="06e29-477">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)

<span data-ttu-id="06e29-478">Сведения о структуре каталогов опубликованных приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="06e29-478">Learn about the directory structure of published ASP.NET Core apps.</span></span>  
[<span data-ttu-id="06e29-479">Структура каталогов</span><span class="sxs-lookup"><span data-stu-id="06e29-479">Directory structure</span></span>](xref:host-and-deploy/directory-structure)

<span data-ttu-id="06e29-480">Сведения об обнаружении активных и неактивных модулей IIS для приложения ASP.NET Core и управлении модулями IIS.</span><span class="sxs-lookup"><span data-stu-id="06e29-480">Discover active and inactive IIS modules for ASP.NET Core apps and how to manage IIS modules.</span></span>  
[<span data-ttu-id="06e29-481">Модули IIS</span><span class="sxs-lookup"><span data-stu-id="06e29-481">IIS modules</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="06e29-482">Сведения о диагностике проблем с развертываниями IIS приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="06e29-482">Learn how to diagnose problems with IIS deployments of ASP.NET Core apps.</span></span>  
[<span data-ttu-id="06e29-483">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="06e29-483">Troubleshoot</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="06e29-484">Распознавание распространенных ошибок при размещении приложений ASP.NET Core в IIS.</span><span class="sxs-lookup"><span data-stu-id="06e29-484">Distinguish common errors when hosting ASP.NET Core apps on IIS.</span></span>  
[<span data-ttu-id="06e29-485">Справочник по общим ошибкам для службы приложений Azure и служб IIS</span><span class="sxs-lookup"><span data-stu-id="06e29-485">Common errors reference for Azure App Service and IIS</span></span>](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a><span data-ttu-id="06e29-486">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="06e29-486">Additional resources</span></span>

* [<span data-ttu-id="06e29-487">Введение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="06e29-487">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="06e29-488">Официальный веб-сайт Microsoft IIS</span><span class="sxs-lookup"><span data-stu-id="06e29-488">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="06e29-489">Библиотека технического содержимого по Windows Server</span><span class="sxs-lookup"><span data-stu-id="06e29-489">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="06e29-490">HTTP/2 в IIS</span><span class="sxs-lookup"><span data-stu-id="06e29-490">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
