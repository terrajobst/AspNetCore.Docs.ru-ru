---
title: Устранение неполадок ASP.NET Core в службах IIS
author: guardrex
description: Сведения о диагностике проблем с развертываниями приложений ASP.NET Core на платформе IIS.
ms.author: riande
ms.custom: mvc
ms.date: 11/26/2018
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 2ff870623de43676be38c5de8f338a7913e885a8
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450714"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="52db3-103">Устранение неполадок ASP.NET Core в службах IIS</span><span class="sxs-lookup"><span data-stu-id="52db3-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="52db3-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="52db3-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="52db3-105">Эта статья содержит инструкции по диагностике проблемы запуска приложения ASP.NET Core, размещенного в [службах IIS](/iis).</span><span class="sxs-lookup"><span data-stu-id="52db3-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="52db3-106">Информация в этой статье относится к размещению в службах IIS на Windows Server и Windows Desktop.</span><span class="sxs-lookup"><span data-stu-id="52db3-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="52db3-107">В Visual Studio проект ASP.NET Core по умолчанию использует размещение в [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) на период отладки.</span><span class="sxs-lookup"><span data-stu-id="52db3-107">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="52db3-108">Рекомендации в этой статье позволяют устранять неполадки, проявляющиеся как *502.5 — ошибка процесса* или *500.30 — ошибка запуска* при локальной отладке.</span><span class="sxs-lookup"><span data-stu-id="52db3-108">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="52db3-109">В Visual Studio проект ASP.NET Core по умолчанию использует размещение в [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) на период отладки.</span><span class="sxs-lookup"><span data-stu-id="52db3-109">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="52db3-110">Рекомендации в этой статье позволяют устранять неполадки, проявляющиеся как *сбой процесса 502.5* при локальной отладке.</span><span class="sxs-lookup"><span data-stu-id="52db3-110">A *502.5 Process Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

<span data-ttu-id="52db3-111">Дополнительные статьи по устранению неполадок:</span><span class="sxs-lookup"><span data-stu-id="52db3-111">Additional troubleshooting topics:</span></span>

<xref:host-and-deploy/azure-apps/troubleshoot>  
<span data-ttu-id="52db3-112">Служба приложений использует для размещения приложений [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) и IIS, но самые полезные рекомендации вы найдете в статье с инструкциями для самой службы приложений.</span><span class="sxs-lookup"><span data-stu-id="52db3-112">Although App Service uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and IIS to host apps, see the dedicated topic for instructions specific to App Service.</span></span>

<xref:fundamentals/error-handling>  
<span data-ttu-id="52db3-113">Узнайте, как обрабатывать ошибки в приложениях ASP.NET Core при разработке в локальной системе.</span><span class="sxs-lookup"><span data-stu-id="52db3-113">Discover how to handle errors in ASP.NET Core apps during development on a local system.</span></span>

[<span data-ttu-id="52db3-114">Сведения об отладке с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="52db3-114">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)  
<span data-ttu-id="52db3-115">В этой статье представлены возможности отладчика Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="52db3-115">This topic introduces the features of the Visual Studio debugger.</span></span>

[<span data-ttu-id="52db3-116">Отладка с помощью Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="52db3-116">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)  
<span data-ttu-id="52db3-117">Узнайте о поддержке отладки, встроенной в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="52db3-117">Learn about the debugging support built into Visual Studio Code.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="52db3-118">Ошибки при запуске приложения</span><span class="sxs-lookup"><span data-stu-id="52db3-118">App startup errors</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="52db3-119">502.5 Process Failure (ошибка процесса)</span><span class="sxs-lookup"><span data-stu-id="52db3-119">502.5 Process Failure</span></span>

<span data-ttu-id="52db3-120">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="52db3-120">The worker process fails.</span></span> <span data-ttu-id="52db3-121">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="52db3-121">The app doesn't start.</span></span>

<span data-ttu-id="52db3-122">Модуль ASP.NET Core пытается запустить процесс dotnet серверной части, но он не запускается.</span><span class="sxs-lookup"><span data-stu-id="52db3-122">The ASP.NET Core Module attempts to start the backend dotnet process but it fails to start.</span></span> <span data-ttu-id="52db3-123">Обычно причину сбоя при запуске процесса можно определить по записям в [журнале событий приложения](#application-event-log) и [журнале вывода stdout модуля ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="52db3-123">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span> 

<span data-ttu-id="52db3-124">Распространенной причиной сбоя является неправильная настройка приложения с выбором отсутствующей версии общей платформы ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="52db3-124">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="52db3-125">Проверьте, какие версии общей платформы ASP.NET Core установлены на целевом компьютере.</span><span class="sxs-lookup"><span data-stu-id="52db3-125">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

<span data-ttu-id="52db3-126">Когда ошибки в конфигурации размещения или приложения приводят к сбою рабочего процесса, возвращается страница ошибки *502.5 — ошибка процесса*.</span><span class="sxs-lookup"><span data-stu-id="52db3-126">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

![Окно браузера со страницей "502.5 — ошибка процесса"](troubleshoot/_static/process-failure-page.png)

::: moniker range=">= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="52db3-128">500.30 In-Process Startup Failure (ошибка внутрипроцессного запуска)</span><span class="sxs-lookup"><span data-stu-id="52db3-128">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="52db3-129">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="52db3-129">The worker process fails.</span></span> <span data-ttu-id="52db3-130">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="52db3-130">The app doesn't start.</span></span>

<span data-ttu-id="52db3-131">Модуль ASP.NET Core пытается запустить среду CLR .NET Core внутри процесса, но она не запускается.</span><span class="sxs-lookup"><span data-stu-id="52db3-131">The ASP.NET Core Module attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="52db3-132">Обычно причину сбоя при запуске процесса можно определить по записям в [журнале событий приложения](#application-event-log) и [журнале вывода stdout модуля ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="52db3-132">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span> 

<span data-ttu-id="52db3-133">Распространенной причиной сбоя является неправильная настройка приложения с выбором отсутствующей версии общей платформы ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="52db3-133">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="52db3-134">Проверьте, какие версии общей платформы ASP.NET Core установлены на целевом компьютере.</span><span class="sxs-lookup"><span data-stu-id="52db3-134">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="52db3-135">500.0 In-Process Handler Load Failure (ошибка загрузки внутрипроцессного обработчика)</span><span class="sxs-lookup"><span data-stu-id="52db3-135">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="52db3-136">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="52db3-136">The worker process fails.</span></span> <span data-ttu-id="52db3-137">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="52db3-137">The app doesn't start.</span></span>

<span data-ttu-id="52db3-138">Модулю ASP.NET Core не удается найти среду CLR .NET Core и внутрипроцессный обработчик запросов (*aspnetcorev2_inprocess.dll*).</span><span class="sxs-lookup"><span data-stu-id="52db3-138">The ASP.NET Core Module fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="52db3-139">Проверьте следующие моменты:</span><span class="sxs-lookup"><span data-stu-id="52db3-139">Check that:</span></span>

* <span data-ttu-id="52db3-140">приложение предназначено для пакета NuGet [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) или [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app);</span><span class="sxs-lookup"><span data-stu-id="52db3-140">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="52db3-141">версия общей платформы ASP.NET Core, для которой предназначено приложение, установлена на целевом компьютере.</span><span class="sxs-lookup"><span data-stu-id="52db3-141">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="52db3-142">500.0 Out-Of-Process Handler Load Failure (ошибка загрузки внепроцессного обработчика)</span><span class="sxs-lookup"><span data-stu-id="52db3-142">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="52db3-143">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="52db3-143">The worker process fails.</span></span> <span data-ttu-id="52db3-144">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="52db3-144">The app doesn't start.</span></span>

<span data-ttu-id="52db3-145">Модулю ASP.NET Core не удается найти внепроцессный обработчик запросов.</span><span class="sxs-lookup"><span data-stu-id="52db3-145">The ASP.NET Core Module fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="52db3-146">Убедитесь, что *aspnetcorev2_outofprocess.dll* находится во вложенной папке рядом с *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="52db3-146">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span> 

::: moniker-end

### <a name="500-internal-server-error"></a><span data-ttu-id="52db3-147">500 Internal Server Error (внутренняя ошибка сервера)</span><span class="sxs-lookup"><span data-stu-id="52db3-147">500 Internal Server Error</span></span>

<span data-ttu-id="52db3-148">Приложение запускается, но ошибка не позволяет серверу выполнить запрос.</span><span class="sxs-lookup"><span data-stu-id="52db3-148">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="52db3-149">Эта ошибка возникает в коде приложения при запуске или при создании ответа.</span><span class="sxs-lookup"><span data-stu-id="52db3-149">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="52db3-150">Ответ может не иметь содержимого или ответ в браузере может выглядеть как *500 — внутренняя ошибка сервера*.</span><span class="sxs-lookup"><span data-stu-id="52db3-150">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="52db3-151">Как правило, в журнале событий приложения указывается, что приложение запускается в обычном режиме.</span><span class="sxs-lookup"><span data-stu-id="52db3-151">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="52db3-152">Сервер действует правильно.</span><span class="sxs-lookup"><span data-stu-id="52db3-152">From the server's perspective, that's correct.</span></span> <span data-ttu-id="52db3-153">Приложение запустилось, но не может сформировать допустимый ответ.</span><span class="sxs-lookup"><span data-stu-id="52db3-153">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="52db3-154">Чтобы устранить проблему, [запустите приложение в командной строке](#run-the-app-at-a-command-prompt) на сервере или [включите журнал вывода stdout для модуля ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="52db3-154">[Run the app at a command prompt](#run-the-app-at-a-command-prompt) on the server or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="52db3-155">Сброс подключения</span><span class="sxs-lookup"><span data-stu-id="52db3-155">Connection reset</span></span>

<span data-ttu-id="52db3-156">Если ошибка возникает после отправки заголовков, сервер уже не может отправить страницу **500 — внутренняя ошибка сервера**.</span><span class="sxs-lookup"><span data-stu-id="52db3-156">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="52db3-157">Это часто происходит, когда ошибка возникает во время сериализации составных объектов ответа.</span><span class="sxs-lookup"><span data-stu-id="52db3-157">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="52db3-158">Этот тип ошибки отображается на стороне клиента как ошибка *Сброс подключения*.</span><span class="sxs-lookup"><span data-stu-id="52db3-158">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="52db3-159">[Ведение журналов для приложений](xref:fundamentals/logging/index) может помочь устранить эти ошибки.</span><span class="sxs-lookup"><span data-stu-id="52db3-159">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="52db3-160">Ограничения по умолчанию при запуске</span><span class="sxs-lookup"><span data-stu-id="52db3-160">Default startup limits</span></span>

<span data-ttu-id="52db3-161">Параметру *startupTimeLimit* модуля ASP.NET Core установлено значение по умолчанию 120 секунд.</span><span class="sxs-lookup"><span data-stu-id="52db3-161">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="52db3-162">Если оставить значение по умолчанию, то прежде чем журнал модуля зарегистрирует ошибку запуска приложения, может пройти до двух минут.</span><span class="sxs-lookup"><span data-stu-id="52db3-162">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="52db3-163">Сведения о настройке модуля см. в разделе [Атрибуты элемента aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="52db3-163">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="52db3-164">Устранение неполадок при запуске приложения</span><span class="sxs-lookup"><span data-stu-id="52db3-164">Troubleshoot app startup errors</span></span>

### <a name="application-event-log"></a><span data-ttu-id="52db3-165">Журнал событий приложений</span><span class="sxs-lookup"><span data-stu-id="52db3-165">Application Event Log</span></span>

<span data-ttu-id="52db3-166">Доступ к журналу событий приложений</span><span class="sxs-lookup"><span data-stu-id="52db3-166">Access the Application Event Log:</span></span>

1. <span data-ttu-id="52db3-167">Откройте меню "Пуск", выполните поиск по фразе **Просмотр событий** и выберите приложение **Просмотр событий**.</span><span class="sxs-lookup"><span data-stu-id="52db3-167">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="52db3-168">В **средстве просмотра событий** откройте узел **Журналы Windows**.</span><span class="sxs-lookup"><span data-stu-id="52db3-168">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="52db3-169">Выберите **Приложение**, чтобы открыть журнал событий приложения.</span><span class="sxs-lookup"><span data-stu-id="52db3-169">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="52db3-170">Проверьте здесь наличие ошибок, связанных с проблемным приложением.</span><span class="sxs-lookup"><span data-stu-id="52db3-170">Search for errors associated with the failing app.</span></span> <span data-ttu-id="52db3-171">Нам нужны ошибки со значениями *Модуль IIS AspNetCore* или *Модуль IIS Express AspNetCore* в столбце *Источник*.</span><span class="sxs-lookup"><span data-stu-id="52db3-171">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="52db3-172">Запуск приложения в командной строке</span><span class="sxs-lookup"><span data-stu-id="52db3-172">Run the app at a command prompt</span></span>

<span data-ttu-id="52db3-173">Многие ошибки запуска не создают полезные сведения в журнале событий приложения.</span><span class="sxs-lookup"><span data-stu-id="52db3-173">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="52db3-174">Для некоторых ошибок можно найти причину, запустив приложение в командной строке на компьютере размещения.</span><span class="sxs-lookup"><span data-stu-id="52db3-174">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="52db3-175">развертывание, зависящее от платформы;</span><span class="sxs-lookup"><span data-stu-id="52db3-175">Framework-dependent deployment</span></span>

<span data-ttu-id="52db3-176">Если развертываемое приложение [зависит от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="52db3-176">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="52db3-177">В командной строке перейдите в папку развертывания и запустите приложение, выполнив команду *dotnet.exe* для сборки приложения.</span><span class="sxs-lookup"><span data-stu-id="52db3-177">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="52db3-178">В следующей команде укажите нужное имя сборки вместо заполнителя \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="52db3-178">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="52db3-179">Выходные данные приложения, в том числе любые ошибки, будут выведены в окно консоли.</span><span class="sxs-lookup"><span data-stu-id="52db3-179">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="52db3-180">Если ошибки возникают при выполнении запроса к приложению, выполните запрос к узлу и порту, где Kestrel ожидает передачи данных.</span><span class="sxs-lookup"><span data-stu-id="52db3-180">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="52db3-181">Создайте запрос к `http://localhost:5000/`, используя узел и порт по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="52db3-181">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="52db3-182">Если приложение отвечает на запросы по адресу конечной точки Kestrel, значит проблема скорее связана с конфигурацией обратного прокси, а не с самим приложением.</span><span class="sxs-lookup"><span data-stu-id="52db3-182">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the reverse proxy configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="52db3-183">автономное развертывание;</span><span class="sxs-lookup"><span data-stu-id="52db3-183">Self-contained deployment</span></span>

<span data-ttu-id="52db3-184">Если приложение [развертывается автономно](/dotnet/core/deploying/#self-contained-deployments-scd), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="52db3-184">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="52db3-185">В командной строке перейдите в папку развертывания и запустите исполняемый файл приложения.</span><span class="sxs-lookup"><span data-stu-id="52db3-185">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="52db3-186">В следующей команде укажите нужное имя сборки вместо заполнителя \<assembly_name>: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="52db3-186">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="52db3-187">Выходные данные приложения, в том числе любые ошибки, будут выведены в окно консоли.</span><span class="sxs-lookup"><span data-stu-id="52db3-187">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="52db3-188">Если ошибки возникают при выполнении запроса к приложению, выполните запрос к узлу и порту, где Kestrel ожидает передачи данных.</span><span class="sxs-lookup"><span data-stu-id="52db3-188">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="52db3-189">Создайте запрос к `http://localhost:5000/`, используя узел и порт по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="52db3-189">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="52db3-190">Если приложение отвечает на запросы по адресу конечной точки Kestrel, значит проблема скорее связана с конфигурацией обратного прокси, а не с самим приложением.</span><span class="sxs-lookup"><span data-stu-id="52db3-190">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the reverse proxy configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="52db3-191">Журнал вывода stdout модуля ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="52db3-191">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="52db3-192">Чтобы включить и просмотреть журналы stdout, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="52db3-192">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="52db3-193">Перейдите в папку развертывания сайта на компьютере размещения.</span><span class="sxs-lookup"><span data-stu-id="52db3-193">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="52db3-194">Если здесь нет папки *logs*, создайте ее.</span><span class="sxs-lookup"><span data-stu-id="52db3-194">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="52db3-195">Сведения о том, как настроить в MSBuild автоматическое создание папки *logs* в развертывании, см. в статье [о структуре каталогов](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="52db3-195">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="52db3-196">Измените файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="52db3-196">Edit the *web.config* file.</span></span> <span data-ttu-id="52db3-197">Задайте для параметра **stdoutLogEnabled** значение `true` и измените путь **stdoutLogFile** так, чтобы он указывал на папку *logs* (например, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="52db3-197">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="52db3-198">В этом пути `stdout` обозначает префикс имени для файла журнала.</span><span class="sxs-lookup"><span data-stu-id="52db3-198">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="52db3-199">При создании файла журнала автоматически добавляются метка времени, идентификатор процесса и расширение файла.</span><span class="sxs-lookup"><span data-stu-id="52db3-199">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="52db3-200">Если указан префикс файла `stdout`, стандартный файл журнала будет иметь примерно такое имя: *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="52db3-200">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span> 
1. <span data-ttu-id="52db3-201">Убедитесь, что удостоверение пула приложений имеет разрешения на запись в папку *logs*.</span><span class="sxs-lookup"><span data-stu-id="52db3-201">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="52db3-202">Сохраните обновленный файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="52db3-202">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="52db3-203">Сделайте запрос к приложению.</span><span class="sxs-lookup"><span data-stu-id="52db3-203">Make a request to the app.</span></span>
1. <span data-ttu-id="52db3-204">Перейдите в папку *logs*.</span><span class="sxs-lookup"><span data-stu-id="52db3-204">Navigate to the *logs* folder.</span></span> <span data-ttu-id="52db3-205">Найдите и откройте последний журнал вывода stdout.</span><span class="sxs-lookup"><span data-stu-id="52db3-205">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="52db3-206">Проверьте, нет ли в нем ошибок.</span><span class="sxs-lookup"><span data-stu-id="52db3-206">Study the log for errors.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="52db3-207">Завершив устранение неполадок, отключите ведение журнала stdout.</span><span class="sxs-lookup"><span data-stu-id="52db3-207">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="52db3-208">Измените файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="52db3-208">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="52db3-209">Задайте параметру **stdoutLogEnabled** значение `false`.</span><span class="sxs-lookup"><span data-stu-id="52db3-209">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="52db3-210">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="52db3-210">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="52db3-211">Ошибка при отключении журнала stdout может привести к сбоям приложения или сервера.</span><span class="sxs-lookup"><span data-stu-id="52db3-211">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="52db3-212">Ни размер файла журнала, ни количество создаваемых файлов журналов ничем не ограничены.</span><span class="sxs-lookup"><span data-stu-id="52db3-212">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="52db3-213">Для обычного ведения журналов для приложения ASP.NET Core используйте библиотеку, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов.</span><span class="sxs-lookup"><span data-stu-id="52db3-213">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="52db3-214">Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="52db3-214">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enable-the-developer-exception-page"></a><span data-ttu-id="52db3-215">Включение страницы исключений для разработчика</span><span class="sxs-lookup"><span data-stu-id="52db3-215">Enable the Developer Exception Page</span></span>

<span data-ttu-id="52db3-216">Можно добавить переменную среды `ASPNETCORE_ENVIRONMENT` [в файл web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables), чтобы запустить приложение в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="52db3-216">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="52db3-217">Если среда не переопределяется при запуске приложения использованием `UseEnvironment` в конструкторе узла, эта переменная среды позволяет отображать [страницу исключения для разработчика](xref:fundamentals/error-handling) при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="52db3-217">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

<span data-ttu-id="52db3-218">Настройка переменной среды `ASPNETCORE_ENVIRONMENT` рекомендуется только на промежуточных и тестовых серверах, доступ к которым из Интернета закрыт.</span><span class="sxs-lookup"><span data-stu-id="52db3-218">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="52db3-219">Завершив устранение неполадок, удалите эту переменную среды из файла *web.config*.</span><span class="sxs-lookup"><span data-stu-id="52db3-219">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="52db3-220">Сведения о настройке переменных среды в файле *web.config* см. в статье о [дочернем элементе environmentVariables в aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="52db3-220">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="52db3-221">Стандартные ошибки запуска</span><span class="sxs-lookup"><span data-stu-id="52db3-221">Common startup errors</span></span>

<span data-ttu-id="52db3-222">См. раздел <xref:host-and-deploy/azure-iis-errors-reference>.</span><span class="sxs-lookup"><span data-stu-id="52db3-222">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="52db3-223">В этой статье рассматривается большинство распространенных ошибок, препятствующих запуску приложений.</span><span class="sxs-lookup"><span data-stu-id="52db3-223">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="52db3-224">Получение данных из приложения</span><span class="sxs-lookup"><span data-stu-id="52db3-224">Obtain data from an app</span></span>

<span data-ttu-id="52db3-225">Если приложение способно отвечать на запросы, получите данные о запросе, подключении и дополнительные данные из приложений с помощью встроенного терминала ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="52db3-225">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="52db3-226">Дополнительные сведения и примеры с кодом см. здесь: <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="52db3-226">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

## <a name="slow-or-hanging-app"></a><span data-ttu-id="52db3-227">Медленное или зависающее приложение</span><span class="sxs-lookup"><span data-stu-id="52db3-227">Slow or hanging app</span></span>

<span data-ttu-id="52db3-228">Если приложение медленно реагирует на запрос или зависает при его обработке, найдите и проанализируйте [файл дампа](/visualstudio/debugger/using-dump-files).</span><span class="sxs-lookup"><span data-stu-id="52db3-228">When an app responds slowly or hangs on a request, obtain and analyze a [dump file](/visualstudio/debugger/using-dump-files).</span></span> <span data-ttu-id="52db3-229">Чтобы получить файлы дампа, можно использовать любое из следующих средств:</span><span class="sxs-lookup"><span data-stu-id="52db3-229">Dump files can be obtained using any of the following tools:</span></span>

* <span data-ttu-id="52db3-230">[ProcDump](/sysinternals/downloads/procdump);</span><span class="sxs-lookup"><span data-stu-id="52db3-230">[ProcDump](/sysinternals/downloads/procdump)</span></span>
* <span data-ttu-id="52db3-231">[DebugDiag](https://www.microsoft.com/download/details.aspx?id=49924);</span><span class="sxs-lookup"><span data-stu-id="52db3-231">[DebugDiag](https://www.microsoft.com/download/details.aspx?id=49924)</span></span>
* <span data-ttu-id="52db3-232">WinDbg: [скачивание инструментов отладки для Windows](https://developer.microsoft.com/windows/hardware/download-windbg), [отладка с помощью WinDbg](/windows-hardware/drivers/debugger/debugging-using-windbg).</span><span class="sxs-lookup"><span data-stu-id="52db3-232">WinDbg: [Download Debugging tools for Windows](https://developer.microsoft.com/windows/hardware/download-windbg), [Debugging Using WinDbg](/windows-hardware/drivers/debugger/debugging-using-windbg)</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="52db3-233">Удаленная отладка</span><span class="sxs-lookup"><span data-stu-id="52db3-233">Remote debugging</span></span>

<span data-ttu-id="52db3-234">Изучите раздел документации по Visual Studio [об удаленной отладке ASP.NET Core на удаленном компьютере IIS в Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer).</span><span class="sxs-lookup"><span data-stu-id="52db3-234">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="52db3-235">Application Insights</span><span class="sxs-lookup"><span data-stu-id="52db3-235">Application Insights</span></span>

<span data-ttu-id="52db3-236">[Application Insights](/azure/application-insights/) предоставляет данные телеметрии из приложений, размещенных в IIS, в том числе возможности регистрации ошибок и создания отчетов.</span><span class="sxs-lookup"><span data-stu-id="52db3-236">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="52db3-237">Application Insights может сообщать только об ошибках, возникающих после запуска приложения, когда становятся доступными функции ведения журнала приложения.</span><span class="sxs-lookup"><span data-stu-id="52db3-237">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="52db3-238">Дополнительные сведения см. в статье [Application Insights для ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="52db3-238">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-advice"></a><span data-ttu-id="52db3-239">Дополнительные советы</span><span class="sxs-lookup"><span data-stu-id="52db3-239">Additional advice</span></span>

<span data-ttu-id="52db3-240">Иногда приложения-функции перестают работать сразу после обновления пакета SDK для .NET Core на компьютере разработки или обновления пакетов в самом приложении.</span><span class="sxs-lookup"><span data-stu-id="52db3-240">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="52db3-241">В некоторых случаях в результате важного обновления несогласованные версии пакетов могут привести к нарушению работы приложения.</span><span class="sxs-lookup"><span data-stu-id="52db3-241">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="52db3-242">Большинство этих проблем можно исправить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="52db3-242">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="52db3-243">Удалите папки *bin* и *obj*.</span><span class="sxs-lookup"><span data-stu-id="52db3-243">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="52db3-244">Очистите кэши пакетов, размещенные в папках *%UserProfile%\\.nuget\\packages* и *%LocalAppData%\\Nuget\\v3-cache*.</span><span class="sxs-lookup"><span data-stu-id="52db3-244">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="52db3-245">Восстановите и перестройте проект.</span><span class="sxs-lookup"><span data-stu-id="52db3-245">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="52db3-246">Убедитесь, что предыдущее развертывание на сервере полностью удалено, прежде чем развертывать его снова.</span><span class="sxs-lookup"><span data-stu-id="52db3-246">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="52db3-247">Очистить кэши пакета проще всего командой `dotnet nuget locals all --clear` из командной строки.</span><span class="sxs-lookup"><span data-stu-id="52db3-247">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
>
> <span data-ttu-id="52db3-248">Также очистку кэшей пакетов можно выполнить средством [nuget.exe](https://www.nuget.org/downloads) или командой `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="52db3-248">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="52db3-249">*NuGet.exe* не входит в пакет установки операционной системы Windows для настольных компьютеров и его нужно получить отдельно на [веб-сайте NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="52db3-249">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="52db3-250">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="52db3-250">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
