---
title: Ведение журнала и диагностики в ASP.NET Core SignalR
author: anurse
description: Узнайте, как для сбора диагностических данных из приложения ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: signalr
ms.date: 02/27/2019
uid: signalr/diagnostics
ms.openlocfilehash: b6bd21314ed183488999bcff3553e53493537a11
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896891"
---
# <a name="logging-and-diagnostics-in-aspnet-core-signalr"></a><span data-ttu-id="42546-103">Ведение журнала и диагностики в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="42546-103">Logging and diagnostics in ASP.NET Core SignalR</span></span>

<span data-ttu-id="42546-104">По [Andrew Stanton медсестра](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="42546-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="42546-105">Эта статья содержит рекомендации для сбора диагностики из приложения ASP.NET Core SignalR для устранения проблем.</span><span class="sxs-lookup"><span data-stu-id="42546-105">This article provides guidance for gathering diagnostics from your ASP.NET Core SignalR app to help troubleshoot issues.</span></span>

## <a name="server-side-logging"></a><span data-ttu-id="42546-106">Ведение журнала на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="42546-106">Server-side logging</span></span>

> [!WARNING]
> <span data-ttu-id="42546-107">Журналы на стороне сервера могут содержать конфиденциальные сведения из вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="42546-107">Server-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="42546-108">**Никогда не** публиковать необработанные журналы из рабочих приложений на открытых форумах, таких как GitHub.</span><span class="sxs-lookup"><span data-stu-id="42546-108">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="42546-109">Поскольку SignalR является частью ASP.NET Core, он использует ASP.NET Core, ведение журнала система.</span><span class="sxs-lookup"><span data-stu-id="42546-109">Since SignalR is part of ASP.NET Core, it uses the ASP.NET Core logging system.</span></span> <span data-ttu-id="42546-110">В конфигурации по умолчанию SignalR журналы очень мало сведений, но его можно настроить.</span><span class="sxs-lookup"><span data-stu-id="42546-110">In the default configuration, SignalR logs very little information, but this can configured.</span></span> <span data-ttu-id="42546-111">См. в документации на [ведение журнала ASP.NET Core](xref:fundamentals/logging/index#configuration) Дополнительные сведения о настройке ведения журнала ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="42546-111">See the documentation on [ASP.NET Core logging](xref:fundamentals/logging/index#configuration) for details on configuring ASP.NET Core logging.</span></span>

<span data-ttu-id="42546-112">SignalR использует две категории средства ведения журнала:</span><span class="sxs-lookup"><span data-stu-id="42546-112">SignalR uses two logger categories:</span></span>

* <span data-ttu-id="42546-113">`Microsoft.AspNetCore.SignalR` — для журналов, связанных с протоколы концентратора, активация концентраторов, вызов методов и других действий, связанных с центром.</span><span class="sxs-lookup"><span data-stu-id="42546-113">`Microsoft.AspNetCore.SignalR` - for logs related to Hub Protocols, activating Hubs, invoking methods, and other Hub-related activities.</span></span>
* <span data-ttu-id="42546-114">`Microsoft.AspNetCore.Http.Connections` — для журналов, связанных с транспортов, таких как WebSockets, длинный опрос Server-Sent событий и низкоуровневой инфраструктурой SignalR.</span><span class="sxs-lookup"><span data-stu-id="42546-114">`Microsoft.AspNetCore.Http.Connections` - for logs related to transports such as WebSockets, Long Polling and Server-Sent Events and low-level SignalR infrastructure.</span></span>

<span data-ttu-id="42546-115">Чтобы включить подробные журналы из SignalR, настройте оба выше префиксами `Debug` уровня в вашей `appsettings.json` файл, добавив следующие элементы во `LogLevel` пункту в `Logging`:</span><span class="sxs-lookup"><span data-stu-id="42546-115">To enable detailed logs from SignalR, configure both of the preceding prefixes to the `Debug` level in your `appsettings.json` file by adding the following items to the `LogLevel` sub-section in `Logging`:</span></span>

[!code-json[Configuring logging](diagnostics/logging-config.json?highlight=7-8)]

<span data-ttu-id="42546-116">Это также можно настроить в коде в вашей `CreateWebHostBuilder` метод:</span><span class="sxs-lookup"><span data-stu-id="42546-116">You can also configure this in code in your `CreateWebHostBuilder` method:</span></span>

[!code-csharp[Configuring logging in code](diagnostics/logging-config-code.cs?highlight=5-6)]

<span data-ttu-id="42546-117">Если вы не используете конфигурация на основе JSON, задайте следующие значения конфигурации в системе конфигурации:</span><span class="sxs-lookup"><span data-stu-id="42546-117">If you aren't using JSON-based configuration, set the following configuration values in your configuration system:</span></span>

* `Logging:LogLevel:Microsoft.AspNetCore.SignalR` = `Debug`
* `Logging:LogLevel:Microsoft.AspNetCore.Http.Connections` = `Debug`

<span data-ttu-id="42546-118">Обратитесь к документации для вашей системы конфигурации определить способ указания значений вложенной конфигурации.</span><span class="sxs-lookup"><span data-stu-id="42546-118">Check the documentation for your configuration system to determine how to specify nested configuration values.</span></span> <span data-ttu-id="42546-119">Например, при использовании переменных среды два `_` символы используются вместо `:` (например: `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).</span><span class="sxs-lookup"><span data-stu-id="42546-119">For example, when using environment variables, two `_` characters are used instead of the `:` (such as: `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).</span></span>

<span data-ttu-id="42546-120">Мы рекомендуем использовать `Debug` уровня при сборе более подробные диагностики для приложения.</span><span class="sxs-lookup"><span data-stu-id="42546-120">We recommend using the `Debug` level when gathering more detailed diagnostics for your app.</span></span> <span data-ttu-id="42546-121">`Trace` Уровень создает очень низкого уровня диагностики и редко требуется для диагностики проблем в приложении.</span><span class="sxs-lookup"><span data-stu-id="42546-121">The `Trace` level produces very low-level diagnostics and is rarely needed to diagnose issues in your app.</span></span>

## <a name="access-server-side-logs"></a><span data-ttu-id="42546-122">Журналы событий на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="42546-122">Access server-side logs</span></span>

<span data-ttu-id="42546-123">Способ доступа к серверных журналов зависит от среды, в которой вы работаете.</span><span class="sxs-lookup"><span data-stu-id="42546-123">How you access server-side logs depends on the environment in which you're running.</span></span>

### <a name="as-a-console-app-outside-iis"></a><span data-ttu-id="42546-124">Консольного приложения за пределами IIS</span><span class="sxs-lookup"><span data-stu-id="42546-124">As a console app outside IIS</span></span>

<span data-ttu-id="42546-125">Если у вас в консольном приложении, [ведения журнала консоли](xref:fundamentals/logging/index#console-provider) должен быть включен по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="42546-125">If you're running in a console app, the [Console logger](xref:fundamentals/logging/index#console-provider) should be enabled by default.</span></span> <span data-ttu-id="42546-126">SignalR журналы будут отображаться в консоли.</span><span class="sxs-lookup"><span data-stu-id="42546-126">SignalR logs will appear in the console.</span></span>

### <a name="within-iis-express-from-visual-studio"></a><span data-ttu-id="42546-127">В IIS Express из Visual Studio</span><span class="sxs-lookup"><span data-stu-id="42546-127">Within IIS Express from Visual Studio</span></span>

<span data-ttu-id="42546-128">Visual Studio отображает выходные данные журнала в **вывода** окна.</span><span class="sxs-lookup"><span data-stu-id="42546-128">Visual Studio displays the log output in the **Output** window.</span></span> <span data-ttu-id="42546-129">Выберите **веб-сервера ASP.NET Core** раскрывающийся список параметра.</span><span class="sxs-lookup"><span data-stu-id="42546-129">Select the **ASP.NET Core Web Server** drop down option.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="42546-130">Служба приложений Azure</span><span class="sxs-lookup"><span data-stu-id="42546-130">Azure App Service</span></span>

<span data-ttu-id="42546-131">Включите параметр «Ведение журнала приложения (файловая система)» в разделе «Журналы диагностики» на портале службы приложений Azure и настроить уровень `Verbose`.</span><span class="sxs-lookup"><span data-stu-id="42546-131">Enable the "Application Logging (Filesystem)" option in the "Diagnostics logs" section of the Azure App Service portal and configure the Level to `Verbose`.</span></span> <span data-ttu-id="42546-132">Журналы должны быть доступны из «Потоковая передача журналов» службы, а также, как и в журналы в файловой системе приложения службы.</span><span class="sxs-lookup"><span data-stu-id="42546-132">Logs should be available from the "Log streaming" service, as well as in logs on the file system of your App Service.</span></span> <span data-ttu-id="42546-133">Дополнительные сведения см. в документации на [потоковая передача журналов Azure](xref:fundamentals/logging/index#azure-log-streaming).</span><span class="sxs-lookup"><span data-stu-id="42546-133">For more information, see the documentation on [Azure log streaming](xref:fundamentals/logging/index#azure-log-streaming).</span></span>

### <a name="other-environments"></a><span data-ttu-id="42546-134">Другие среды</span><span class="sxs-lookup"><span data-stu-id="42546-134">Other environments</span></span>

<span data-ttu-id="42546-135">Если у вас в другой среде (Docker, Kubernetes, служба Windows, и т.д.), см. в полной документации на [ведения журнала ASP.NET Core](xref:fundamentals/logging/index) Дополнительные сведения о том, как настроить поставщики ведения журналов, подходящий для вашей среды.</span><span class="sxs-lookup"><span data-stu-id="42546-135">If you're running in another environment (Docker, Kubernetes, Windows Service, etc.), see the full documentation on [ASP.NET Core Logging](xref:fundamentals/logging/index) for more information on how to configure logging providers suitable to your environment.</span></span>

## <a name="javascript-client-logging"></a><span data-ttu-id="42546-136">Ведение журнала клиента JavaScript</span><span class="sxs-lookup"><span data-stu-id="42546-136">JavaScript client logging</span></span>

> [!WARNING]
> <span data-ttu-id="42546-137">Журналы на стороне клиента могут содержать конфиденциальные сведения из вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="42546-137">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="42546-138">**Никогда не** публиковать необработанные журналы из рабочих приложений на открытых форумах, таких как GitHub.</span><span class="sxs-lookup"><span data-stu-id="42546-138">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="42546-139">Если вы используете клиент JavaScript, можно настроить параметры ведения журнала с помощью `configureLogging` метод `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="42546-139">When using the JavaScript client, you can configure logging options using the `configureLogging` method on `HubConnectionBuilder`:</span></span>

[!code-javascript[Configuring logging in the JavaScript client](diagnostics/logging-config-js.js?highlight=3)]

<span data-ttu-id="42546-140">Чтобы отключить ведение журнала полностью, укажите `signalR.LogLevel.None` в `configureLogging` метод.</span><span class="sxs-lookup"><span data-stu-id="42546-140">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="42546-141">Ниже приведены доступные уровни ведения журнала для клиента JavaScript.</span><span class="sxs-lookup"><span data-stu-id="42546-141">The following table shows log levels available to the JavaScript client.</span></span> <span data-ttu-id="42546-142">Установка уровня журнала в одно из следующих значений включающем ведение журнала на этом уровне и все вышестоящие уровни в таблице.</span><span class="sxs-lookup"><span data-stu-id="42546-142">Setting the log level to one of these values enables logging at that level and all levels above it in the table.</span></span>

| <span data-ttu-id="42546-143">Уровень</span><span class="sxs-lookup"><span data-stu-id="42546-143">Level</span></span> | <span data-ttu-id="42546-144">Описание</span><span class="sxs-lookup"><span data-stu-id="42546-144">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="42546-145">Сообщения не регистрируются.</span><span class="sxs-lookup"><span data-stu-id="42546-145">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="42546-146">Сообщения, которые означают сбой всего приложения.</span><span class="sxs-lookup"><span data-stu-id="42546-146">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="42546-147">Сообщения, которые указывают на сбой в текущей операции.</span><span class="sxs-lookup"><span data-stu-id="42546-147">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="42546-148">Сообщения, которые указывают на проблему, не являющиеся неустранимыми.</span><span class="sxs-lookup"><span data-stu-id="42546-148">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="42546-149">Информационные сообщения.</span><span class="sxs-lookup"><span data-stu-id="42546-149">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="42546-150">Диагностические сообщения, полезны для отладки.</span><span class="sxs-lookup"><span data-stu-id="42546-150">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="42546-151">Очень подробные диагностические сообщения, предназначены для диагностики конкретных проблем.</span><span class="sxs-lookup"><span data-stu-id="42546-151">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

<span data-ttu-id="42546-152">После настройки уровень детализации, будут записаны журналы консоли браузера (или стандартный вывод в приложении NodeJS).</span><span class="sxs-lookup"><span data-stu-id="42546-152">Once you've configured the verbosity, the logs will be written to the Browser Console (or Standard Output in a NodeJS app).</span></span>

<span data-ttu-id="42546-153">Если вы хотите отправить журналы системы настраиваемого протоколирования, можно предоставить объект JavaScript, реализующий интерфейс `ILogger` интерфейс.</span><span class="sxs-lookup"><span data-stu-id="42546-153">If you want to send logs to a custom logging system, you can provide a JavaScript object implementing the `ILogger` interface.</span></span> <span data-ttu-id="42546-154">Является единственным способом, который должен быть реализован `log`, который принимает уровень события и сообщение, связанное с событием.</span><span class="sxs-lookup"><span data-stu-id="42546-154">The only method that needs to be implemented is `log`, which takes the level of the event and the message associated with the event.</span></span> <span data-ttu-id="42546-155">Пример:</span><span class="sxs-lookup"><span data-stu-id="42546-155">For example:</span></span>

[!code-typescript[Creating a custom logger](diagnostics/custom-logger.ts?highlight=3-7,13)]

## <a name="net-client-logging"></a><span data-ttu-id="42546-156">Ведение журнала клиента .NET</span><span class="sxs-lookup"><span data-stu-id="42546-156">.NET client logging</span></span>

> [!WARNING]
> <span data-ttu-id="42546-157">Журналы на стороне клиента могут содержать конфиденциальные сведения из вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="42546-157">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="42546-158">**Никогда не** публиковать необработанные журналы из рабочих приложений на открытых форумах, таких как GitHub.</span><span class="sxs-lookup"><span data-stu-id="42546-158">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="42546-159">Чтобы получить журналы из клиента .NET, можно использовать `ConfigureLogging` метод `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="42546-159">To get logs from the .NET client, you can use the `ConfigureLogging` method on `HubConnectionBuilder`.</span></span> <span data-ttu-id="42546-160">Это работает так же, как `ConfigureLogging` метод `WebHostBuilder` и `HostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="42546-160">This works the same way as the `ConfigureLogging` method on `WebHostBuilder` and `HostBuilder`.</span></span> <span data-ttu-id="42546-161">Можно настроить те же поставщики ведения журналов, используемых в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="42546-161">You can configure the same logging providers you use in ASP.NET Core.</span></span> <span data-ttu-id="42546-162">Тем не менее необходимо вручную установить и включить пакеты NuGet для отдельных регистраторов.</span><span class="sxs-lookup"><span data-stu-id="42546-162">However, you have to manually install and enable the NuGet packages for the individual logging providers.</span></span>

### <a name="console-logging"></a><span data-ttu-id="42546-163">Журнал консоли</span><span class="sxs-lookup"><span data-stu-id="42546-163">Console logging</span></span>

<span data-ttu-id="42546-164">Чтобы включить ведение журнала консоли, добавьте [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) пакета.</span><span class="sxs-lookup"><span data-stu-id="42546-164">In order to enable Console logging, add the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) package.</span></span> <span data-ttu-id="42546-165">Затем с помощью `AddConsole` метод, чтобы настроить средство ведения журнала консоли:</span><span class="sxs-lookup"><span data-stu-id="42546-165">Then, use the `AddConsole` method to configure the console logger:</span></span>

[!code-csharp[Configuring console logging in .NET client](diagnostics/net-client-console-log.cs?highlight=6)]

### <a name="debug-output-window-logging"></a><span data-ttu-id="42546-166">Ведение журнала выходных данных окна отладки</span><span class="sxs-lookup"><span data-stu-id="42546-166">Debug output window logging</span></span>

<span data-ttu-id="42546-167">Можно также настроить журналы, чтобы перейти к **вывода** окно в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="42546-167">You can also configure logs to go to the **Output** window in Visual Studio.</span></span> <span data-ttu-id="42546-168">Установка [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) упаковки и использовать `AddDebug` метод:</span><span class="sxs-lookup"><span data-stu-id="42546-168">Install the [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) package and use the `AddDebug` method:</span></span>

[!code-csharp[Configuring debug output window logging in .NET client](diagnostics/net-client-debug-log.cs?highlight=6)]

### <a name="other-logging-providers"></a><span data-ttu-id="42546-169">Другие поставщики ведения журналов</span><span class="sxs-lookup"><span data-stu-id="42546-169">Other logging providers</span></span>

<span data-ttu-id="42546-170">SignalR поддерживает другие поставщики ведения журналов, например Serilog, Seq, NLog или любая другая система ведения журнала, который интегрируется с `Microsoft.Extensions.Logging`.</span><span class="sxs-lookup"><span data-stu-id="42546-170">SignalR supports other logging providers such as Serilog, Seq, NLog, or any other logging system that integrates with `Microsoft.Extensions.Logging`.</span></span> <span data-ttu-id="42546-171">Если система ведения журнала предоставляет `ILoggerProvider`, его можно зарегистрировать с помощью `AddProvider`:</span><span class="sxs-lookup"><span data-stu-id="42546-171">If your logging system provides an `ILoggerProvider`, you can register it with `AddProvider`:</span></span>

[!code-csharp[Configuring a custom logging provider in .NET client](diagnostics/net-client-custom-log.cs?highlight=6)]

### <a name="control-verbosity"></a><span data-ttu-id="42546-172">Уровень детализации для элемента управления</span><span class="sxs-lookup"><span data-stu-id="42546-172">Control verbosity</span></span>

<span data-ttu-id="42546-173">При входе в систему из других мест в приложении изменение уровня по умолчанию для `Debug` может быть слишком verbose.</span><span class="sxs-lookup"><span data-stu-id="42546-173">If you are logging from other places in your app, changing the default level to `Debug` may be too verbose.</span></span> <span data-ttu-id="42546-174">Фильтр можно использовать для настройки уровня ведения журнала для журналов SignalR.</span><span class="sxs-lookup"><span data-stu-id="42546-174">You can use a Filter to configure the logging level for SignalR logs.</span></span> <span data-ttu-id="42546-175">Это можно сделать в коде, во многом так же, как на сервере:</span><span class="sxs-lookup"><span data-stu-id="42546-175">This can be done in code, in much the same way as on the server:</span></span>

[!code-csharp[Controlling verbosity in .NET client](diagnostics/logging-config-client-code.cs?highlight=9-10)]

## <a name="network-traces"></a><span data-ttu-id="42546-176">Данные трассировки сети</span><span class="sxs-lookup"><span data-stu-id="42546-176">Network traces</span></span>

> [!WARNING]
> <span data-ttu-id="42546-177">Трассировка сети содержит все содержимое каждого сообщения, отправленные приложением.</span><span class="sxs-lookup"><span data-stu-id="42546-177">A network trace contains the full contents of every message sent by your app.</span></span> <span data-ttu-id="42546-178">**Никогда не** публиковать необработанные сети трассировок из рабочих приложений на открытых форумах, таких как GitHub.</span><span class="sxs-lookup"><span data-stu-id="42546-178">**Never** post raw network traces from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="42546-179">Если возникла проблема, сетевой трассировки иногда можно предоставить массу полезной информации.</span><span class="sxs-lookup"><span data-stu-id="42546-179">If you encounter an issue, a network trace can sometimes provide a lot of helpful information.</span></span> <span data-ttu-id="42546-180">Это особенно полезно в том случае, если вы собираетесь сообщите о проблеме на средства отслеживания проблем.</span><span class="sxs-lookup"><span data-stu-id="42546-180">This is particularly useful if you're going to file an issue on our issue tracker.</span></span>

## <a name="collect-a-network-trace-with-fiddler-preferred-option"></a><span data-ttu-id="42546-181">Сбор трассировки сети с помощью Fiddler (предпочтительный вариант)</span><span class="sxs-lookup"><span data-stu-id="42546-181">Collect a network trace with Fiddler (preferred option)</span></span>

<span data-ttu-id="42546-182">Этот метод работает для всех приложений.</span><span class="sxs-lookup"><span data-stu-id="42546-182">This method works for all apps.</span></span>

<span data-ttu-id="42546-183">Fiddler является очень мощным инструментом для сбора трассировок HTTP.</span><span class="sxs-lookup"><span data-stu-id="42546-183">Fiddler is a very powerful tool for collecting HTTP traces.</span></span> <span data-ttu-id="42546-184">Установите его из [telerik.com/fiddler](https://www.telerik.com/fiddler), запустите его и затем запустите приложение и воспроизведите проблему.</span><span class="sxs-lookup"><span data-stu-id="42546-184">Install it from [telerik.com/fiddler](https://www.telerik.com/fiddler), launch it, and then run your app and reproduce the issue.</span></span> <span data-ttu-id="42546-185">Fiddler — для Windows и существует бета-версии для macOS и Linux.</span><span class="sxs-lookup"><span data-stu-id="42546-185">Fiddler is available for Windows, and there are beta versions for macOS and Linux.</span></span>

<span data-ttu-id="42546-186">При подключении по протоколу HTTPS, существуют некоторые дополнительные действия, чтобы убедиться, что Fiddler может расшифровать трафик HTTPS.</span><span class="sxs-lookup"><span data-stu-id="42546-186">If you connect using HTTPS, there are some extra steps to ensure Fiddler can decrypt the HTTPS traffic.</span></span> <span data-ttu-id="42546-187">Дополнительные сведения см. в разделе [документации Fiddler](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).</span><span class="sxs-lookup"><span data-stu-id="42546-187">For more details, see the [Fiddler documentation](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).</span></span>

<span data-ttu-id="42546-188">После сбора данных трассировки, вы можете экспортировать трассировки, выбрав **файл** > **Сохранить** > **все сеансы...**  в строке меню.</span><span class="sxs-lookup"><span data-stu-id="42546-188">Once you've collected the trace, you can export the trace by choosing **File** > **Save** > **All Sessions...** from the menu bar.</span></span>

![Экспорт всех сеансов с Fiddler](diagnostics/fiddler-export.png)

## <a name="collect-a-network-trace-with-tcpdump-macos-and-linux-only"></a><span data-ttu-id="42546-190">Сбор трассировки сети с помощью tcpdump (macOS и Linux только)</span><span class="sxs-lookup"><span data-stu-id="42546-190">Collect a network trace with tcpdump (macOS and Linux only)</span></span>

<span data-ttu-id="42546-191">Этот метод работает для всех приложений.</span><span class="sxs-lookup"><span data-stu-id="42546-191">This method works for all apps.</span></span>

<span data-ttu-id="42546-192">Можно собирать трассировки необработанные TCP, с помощью tcpdump, выполнив следующую команду из командной оболочки.</span><span class="sxs-lookup"><span data-stu-id="42546-192">You can collect raw TCP traces using tcpdump by running the following command from a command shell.</span></span> <span data-ttu-id="42546-193">Может потребоваться `root` или префикса командой `sudo` появление ошибки разрешений:</span><span class="sxs-lookup"><span data-stu-id="42546-193">You may need to be `root` or prefix the command with `sudo` if you get a permissions error:</span></span>

```console
tcpdump -i [interface] -w trace.pcap
```

<span data-ttu-id="42546-194">Замените `[interface]` с сетевым интерфейсом, вы хотите записать на.</span><span class="sxs-lookup"><span data-stu-id="42546-194">Replace `[interface]` with the network interface you wish to capture on.</span></span> <span data-ttu-id="42546-195">Обычно это что-нибудь вроде `/dev/eth0` (для стандартного интерфейса Ethernet) или `/dev/lo0` (для трафика localhost).</span><span class="sxs-lookup"><span data-stu-id="42546-195">Usually, this is something like `/dev/eth0` (for your standard Ethernet interface) or `/dev/lo0` (for localhost traffic).</span></span> <span data-ttu-id="42546-196">Дополнительные сведения см. в разделе `tcpdump` справочную страницу в системе узла.</span><span class="sxs-lookup"><span data-stu-id="42546-196">For more information, see the `tcpdump` man page on your host system.</span></span>

## <a name="collect-a-network-trace-in-the-browser"></a><span data-ttu-id="42546-197">Сбор трассировки сети в браузере</span><span class="sxs-lookup"><span data-stu-id="42546-197">Collect a network trace in the browser</span></span>

<span data-ttu-id="42546-198">Этот метод работает только для браузерных приложений.</span><span class="sxs-lookup"><span data-stu-id="42546-198">This method only works for browser-based apps.</span></span>

<span data-ttu-id="42546-199">Большинство средств разработчика браузера имеет вкладку «Network», которая позволяет записывать сетевой активности между браузером и сервером.</span><span class="sxs-lookup"><span data-stu-id="42546-199">Most browser Developer Tools have a "Network" tab that allows you to capture network activity between the browser and the server.</span></span> <span data-ttu-id="42546-200">Тем не менее эти трассировки не включать в себя WebSocket и Server-Sent сообщения о событиях.</span><span class="sxs-lookup"><span data-stu-id="42546-200">However, these traces don't include WebSocket and Server-Sent Event messages.</span></span> <span data-ttu-id="42546-201">Если вы используете эти транспорты, с помощью такого средства, как Fiddler или TcpDump (как описано ниже) подход лучше.</span><span class="sxs-lookup"><span data-stu-id="42546-201">If you are using those transports, using a tool like Fiddler or TcpDump (described below) is a better approach.</span></span>

### <a name="microsoft-edge-and-internet-explorer"></a><span data-ttu-id="42546-202">Microsoft Edge и Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="42546-202">Microsoft Edge and Internet Explorer</span></span>

<span data-ttu-id="42546-203">(Инструкции одинаковы для Microsoft Edge и Internet Explorer)</span><span class="sxs-lookup"><span data-stu-id="42546-203">(The instructions are the same for both Edge and Internet Explorer)</span></span>

1. <span data-ttu-id="42546-204">Нажмите клавишу F12, чтобы открыть средства разработчика</span><span class="sxs-lookup"><span data-stu-id="42546-204">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="42546-205">Щелкните вкладку "Сеть"</span><span class="sxs-lookup"><span data-stu-id="42546-205">Click the Network Tab</span></span>
3. <span data-ttu-id="42546-206">Обновите страницу, (при необходимости) и воспроизвести проблему</span><span class="sxs-lookup"><span data-stu-id="42546-206">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="42546-207">Щелкните значок "Сохранить" в панели инструментов, чтобы экспортировать трассировки в формате «HAR»:</span><span class="sxs-lookup"><span data-stu-id="42546-207">Click the Save icon in the toolbar to export the trace as a "HAR" file:</span></span>

![Сохранить значок в Microsoft Edge разработки средств вкладку "Сеть"](diagnostics/ie-edge-har-export.png)

### <a name="google-chrome"></a><span data-ttu-id="42546-209">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="42546-209">Google Chrome</span></span>

1. <span data-ttu-id="42546-210">Нажмите клавишу F12, чтобы открыть средства разработчика</span><span class="sxs-lookup"><span data-stu-id="42546-210">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="42546-211">Щелкните вкладку "Сеть"</span><span class="sxs-lookup"><span data-stu-id="42546-211">Click the Network Tab</span></span>
3. <span data-ttu-id="42546-212">Обновите страницу, (при необходимости) и воспроизвести проблему</span><span class="sxs-lookup"><span data-stu-id="42546-212">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="42546-213">Щелкните правой кнопкой мыши список запросов и выберите «Сохранить как HAR с содержимым»:</span><span class="sxs-lookup"><span data-stu-id="42546-213">Right click anywhere in the list of requests and choose "Save as HAR with content":</span></span>

![Параметр «Сохранить как HAR с содержимым» в Google Chrome Dev Tools вкладку "Сеть"](diagnostics/chrome-har-export.png)

### <a name="mozilla-firefox"></a><span data-ttu-id="42546-215">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="42546-215">Mozilla Firefox</span></span>

1. <span data-ttu-id="42546-216">Нажмите клавишу F12, чтобы открыть средства разработчика</span><span class="sxs-lookup"><span data-stu-id="42546-216">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="42546-217">Щелкните вкладку "Сеть"</span><span class="sxs-lookup"><span data-stu-id="42546-217">Click the Network Tab</span></span>
3. <span data-ttu-id="42546-218">Обновите страницу, (при необходимости) и воспроизвести проблему</span><span class="sxs-lookup"><span data-stu-id="42546-218">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="42546-219">Щелкните правой кнопкой мыши список запросов и выберите команду «Сохранить все как HAR»</span><span class="sxs-lookup"><span data-stu-id="42546-219">Right click anywhere in the list of requests and choose "Save All As HAR"</span></span>

![Параметр «Сохранить все как HAR» в Mozilla Firefox разработки средств вкладку "Сеть"](diagnostics/firefox-har-export.png)

## <a name="attach-diagnostics-files-to-github-issues"></a><span data-ttu-id="42546-221">Вложения файлов диагностики на проблемы GitHub</span><span class="sxs-lookup"><span data-stu-id="42546-221">Attach diagnostics files to GitHub issues</span></span>

<span data-ttu-id="42546-222">Можно также присоединить файлы диагностики на проблемы GitHub, переименовав их, чтобы они имели `.txt` расширения и затем перетащите ее на проблему.</span><span class="sxs-lookup"><span data-stu-id="42546-222">You can attach Diagnostics files to GitHub issues by renaming them so they have a `.txt` extension and then dragging and dropping them on to the issue.</span></span>

> [!NOTE]
> <span data-ttu-id="42546-223">Пожалуйста не вставьте содержимое файлов журнала и трассировки сети на сайте github.</span><span class="sxs-lookup"><span data-stu-id="42546-223">Please don't paste the content of log files or network traces in GitHub issue.</span></span> <span data-ttu-id="42546-224">Эти журналы и трассировки могут быть довольно большими, и GitHub обычно удалит их.</span><span class="sxs-lookup"><span data-stu-id="42546-224">These logs and traces can be quite large and GitHub will usually truncate them.</span></span>

![Перетаскивание файлов журнала на вопроса в GitHub](diagnostics/attaching-diagnostics-files.png)

## <a name="additional-resources"></a><span data-ttu-id="42546-226">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="42546-226">Additional resources</span></span>

* <xref:signalr/configuration>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
