---
title: Устранение неполадок ASP.NET Core в службах IIS
author: guardrex
description: Сведения о диагностике проблем с развертываниями приложений ASP.NET Core на платформе IIS.
ms.author: riande
ms.custom: mvc
ms.date: 03/06/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 2f36ae2bda8537e91a3bc925505986bdd6a22a47
ms.sourcegitcommit: 34bf9fc6ea814c039401fca174642f0acb14be3c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/14/2019
ms.locfileid: "57841557"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="a0192-103">Устранение неполадок ASP.NET Core в службах IIS</span><span class="sxs-lookup"><span data-stu-id="a0192-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="a0192-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="a0192-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a0192-105">Эта статья содержит инструкции по диагностике проблемы запуска приложения ASP.NET Core, размещенного в [службах IIS](/iis).</span><span class="sxs-lookup"><span data-stu-id="a0192-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="a0192-106">Информация в этой статье относится к размещению в службах IIS на Windows Server и Windows Desktop.</span><span class="sxs-lookup"><span data-stu-id="a0192-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a0192-107">В Visual Studio проект ASP.NET Core по умолчанию использует размещение в [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) на период отладки.</span><span class="sxs-lookup"><span data-stu-id="a0192-107">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="a0192-108">Рекомендации в этой статье позволяют устранять неполадки, проявляющиеся как *502.5 — ошибка процесса* или *500.30 — ошибка запуска* при локальной отладке.</span><span class="sxs-lookup"><span data-stu-id="a0192-108">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a0192-109">В Visual Studio проект ASP.NET Core по умолчанию использует размещение в [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) на период отладки.</span><span class="sxs-lookup"><span data-stu-id="a0192-109">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="a0192-110">Рекомендации в этой статье позволяют устранять неполадки, проявляющиеся как *сбой процесса 502.5* при локальной отладке.</span><span class="sxs-lookup"><span data-stu-id="a0192-110">A *502.5 Process Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

<span data-ttu-id="a0192-111">Дополнительные статьи по устранению неполадок:</span><span class="sxs-lookup"><span data-stu-id="a0192-111">Additional troubleshooting topics:</span></span>

<xref:host-and-deploy/azure-apps/troubleshoot>  
<span data-ttu-id="a0192-112">Служба приложений использует для размещения приложений [модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module) и IIS, но самые полезные рекомендации вы найдете в статье с инструкциями для самой службы приложений.</span><span class="sxs-lookup"><span data-stu-id="a0192-112">Although App Service uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and IIS to host apps, see the dedicated topic for instructions specific to App Service.</span></span>

<xref:fundamentals/error-handling>  
<span data-ttu-id="a0192-113">Узнайте, как обрабатывать ошибки в приложениях ASP.NET Core при разработке в локальной системе.</span><span class="sxs-lookup"><span data-stu-id="a0192-113">Discover how to handle errors in ASP.NET Core apps during development on a local system.</span></span>

[<span data-ttu-id="a0192-114">Сведения об отладке с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a0192-114">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)  
<span data-ttu-id="a0192-115">В этой статье представлены возможности отладчика Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a0192-115">This topic introduces the features of the Visual Studio debugger.</span></span>

[<span data-ttu-id="a0192-116">Отладка с помощью Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a0192-116">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)  
<span data-ttu-id="a0192-117">Узнайте о поддержке отладки, встроенной в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a0192-117">Learn about the debugging support built into Visual Studio Code.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="a0192-118">Ошибки при запуске приложения</span><span class="sxs-lookup"><span data-stu-id="a0192-118">App startup errors</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="a0192-119">502.5 Process Failure (ошибка процесса)</span><span class="sxs-lookup"><span data-stu-id="a0192-119">502.5 Process Failure</span></span>

<span data-ttu-id="a0192-120">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="a0192-120">The worker process fails.</span></span> <span data-ttu-id="a0192-121">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="a0192-121">The app doesn't start.</span></span>

<span data-ttu-id="a0192-122">Модуль ASP.NET Core пытается запустить процесс dotnet серверной части, но он не запускается.</span><span class="sxs-lookup"><span data-stu-id="a0192-122">The ASP.NET Core Module attempts to start the backend dotnet process but it fails to start.</span></span> <span data-ttu-id="a0192-123">Обычно причину сбоя при запуске процесса можно определить по записям в [журнале событий приложения](#application-event-log) и [журнале вывода stdout модуля ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="a0192-123">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span> 

<span data-ttu-id="a0192-124">Распространенной причиной сбоя является неправильная настройка приложения с выбором отсутствующей версии общей платформы ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a0192-124">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="a0192-125">Проверьте, какие версии общей платформы ASP.NET Core установлены на целевом компьютере.</span><span class="sxs-lookup"><span data-stu-id="a0192-125">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

<span data-ttu-id="a0192-126">Когда ошибки в конфигурации размещения или приложения приводят к сбою рабочего процесса, возвращается страница ошибки *502.5 — ошибка процесса*.</span><span class="sxs-lookup"><span data-stu-id="a0192-126">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

![Окно браузера со страницей "502.5 — ошибка процесса"](troubleshoot/_static/process-failure-page.png)

::: moniker range=">= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="a0192-128">500.30 In-Process Startup Failure (ошибка внутрипроцессного запуска)</span><span class="sxs-lookup"><span data-stu-id="a0192-128">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="a0192-129">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="a0192-129">The worker process fails.</span></span> <span data-ttu-id="a0192-130">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="a0192-130">The app doesn't start.</span></span>

<span data-ttu-id="a0192-131">Модуль ASP.NET Core пытается запустить среду CLR .NET Core внутри процесса, но она не запускается.</span><span class="sxs-lookup"><span data-stu-id="a0192-131">The ASP.NET Core Module attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="a0192-132">Обычно причину сбоя при запуске процесса можно определить по записям в [журнале событий приложения](#application-event-log) и [журнале вывода stdout модуля ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="a0192-132">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span> 

<span data-ttu-id="a0192-133">Распространенной причиной сбоя является неправильная настройка приложения с выбором отсутствующей версии общей платформы ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a0192-133">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="a0192-134">Проверьте, какие версии общей платформы ASP.NET Core установлены на целевом компьютере.</span><span class="sxs-lookup"><span data-stu-id="a0192-134">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="a0192-135">500.0 In-Process Handler Load Failure (ошибка загрузки внутрипроцессного обработчика)</span><span class="sxs-lookup"><span data-stu-id="a0192-135">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="a0192-136">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="a0192-136">The worker process fails.</span></span> <span data-ttu-id="a0192-137">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="a0192-137">The app doesn't start.</span></span>

<span data-ttu-id="a0192-138">Модулю ASP.NET Core не удается найти среду CLR .NET Core и внутрипроцессный обработчик запросов (*aspnetcorev2_inprocess.dll*).</span><span class="sxs-lookup"><span data-stu-id="a0192-138">The ASP.NET Core Module fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="a0192-139">Проверьте следующие моменты:</span><span class="sxs-lookup"><span data-stu-id="a0192-139">Check that:</span></span>

* <span data-ttu-id="a0192-140">приложение предназначено для пакета NuGet [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) или [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app);</span><span class="sxs-lookup"><span data-stu-id="a0192-140">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="a0192-141">версия общей платформы ASP.NET Core, для которой предназначено приложение, установлена на целевом компьютере.</span><span class="sxs-lookup"><span data-stu-id="a0192-141">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="a0192-142">500.0 Out-Of-Process Handler Load Failure (ошибка загрузки внепроцессного обработчика)</span><span class="sxs-lookup"><span data-stu-id="a0192-142">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="a0192-143">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="a0192-143">The worker process fails.</span></span> <span data-ttu-id="a0192-144">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="a0192-144">The app doesn't start.</span></span>

<span data-ttu-id="a0192-145">Модулю ASP.NET Core не удается найти внепроцессный обработчик запросов.</span><span class="sxs-lookup"><span data-stu-id="a0192-145">The ASP.NET Core Module fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="a0192-146">Убедитесь, что *aspnetcorev2_outofprocess.dll* находится во вложенной папке рядом с *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="a0192-146">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span> 

::: moniker-end

### <a name="500-internal-server-error"></a><span data-ttu-id="a0192-147">500 Internal Server Error (внутренняя ошибка сервера)</span><span class="sxs-lookup"><span data-stu-id="a0192-147">500 Internal Server Error</span></span>

<span data-ttu-id="a0192-148">Приложение запускается, но ошибка не позволяет серверу выполнить запрос.</span><span class="sxs-lookup"><span data-stu-id="a0192-148">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="a0192-149">Эта ошибка возникает в коде приложения при запуске или при создании ответа.</span><span class="sxs-lookup"><span data-stu-id="a0192-149">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="a0192-150">Ответ может не иметь содержимого или ответ в браузере может выглядеть как *500 — внутренняя ошибка сервера*.</span><span class="sxs-lookup"><span data-stu-id="a0192-150">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="a0192-151">Как правило, в журнале событий приложения указывается, что приложение запускается в обычном режиме.</span><span class="sxs-lookup"><span data-stu-id="a0192-151">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="a0192-152">Сервер действует правильно.</span><span class="sxs-lookup"><span data-stu-id="a0192-152">From the server's perspective, that's correct.</span></span> <span data-ttu-id="a0192-153">Приложение запустилось, но не может сформировать допустимый ответ.</span><span class="sxs-lookup"><span data-stu-id="a0192-153">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="a0192-154">Чтобы устранить проблему, [запустите приложение в командной строке](#run-the-app-at-a-command-prompt) на сервере или [включите журнал вывода stdout для модуля ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="a0192-154">[Run the app at a command prompt](#run-the-app-at-a-command-prompt) on the server or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="a0192-155">Не удалось запустить приложение (код ошибки "0x800700c1")</span><span class="sxs-lookup"><span data-stu-id="a0192-155">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="a0192-156">Не удалось запустить приложение, так как не удалось загрузить сборку приложения (*.dll*).</span><span class="sxs-lookup"><span data-stu-id="a0192-156">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="a0192-157">Эта ошибка возникает, если существует несоответствие разрядности опубликованного приложения и процесса w3wp/iisexpress.</span><span class="sxs-lookup"><span data-stu-id="a0192-157">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="a0192-158">Убедитесь, что параметр 32-разрядности пула приложений указан правильно:</span><span class="sxs-lookup"><span data-stu-id="a0192-158">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="a0192-159">Выберите пул приложений в диспетчере служб IIS в разделе **Пулы приложений**.</span><span class="sxs-lookup"><span data-stu-id="a0192-159">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="a0192-160">Выберите **Дополнительные параметры** в разделе **Изменение пула приложений** панели **Действия**.</span><span class="sxs-lookup"><span data-stu-id="a0192-160">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="a0192-161">Установите параметр **Включить 32-разрядные приложения**:</span><span class="sxs-lookup"><span data-stu-id="a0192-161">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="a0192-162">При развертывании 32-разрядного (x86) приложения задайте значение `True`.</span><span class="sxs-lookup"><span data-stu-id="a0192-162">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="a0192-163">При развертывании 64-разрядного (x64) приложения задайте значение `False`.</span><span class="sxs-lookup"><span data-stu-id="a0192-163">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="a0192-164">Сброс подключения</span><span class="sxs-lookup"><span data-stu-id="a0192-164">Connection reset</span></span>

<span data-ttu-id="a0192-165">Если ошибка возникает после отправки заголовков, сервер уже не может отправить страницу **500 — внутренняя ошибка сервера**.</span><span class="sxs-lookup"><span data-stu-id="a0192-165">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="a0192-166">Это часто происходит, когда ошибка возникает во время сериализации составных объектов ответа.</span><span class="sxs-lookup"><span data-stu-id="a0192-166">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="a0192-167">Этот тип ошибки отображается на стороне клиента как ошибка *Сброс подключения*.</span><span class="sxs-lookup"><span data-stu-id="a0192-167">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="a0192-168">[Ведение журналов для приложений](xref:fundamentals/logging/index) может помочь устранить эти ошибки.</span><span class="sxs-lookup"><span data-stu-id="a0192-168">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="a0192-169">Ограничения по умолчанию при запуске</span><span class="sxs-lookup"><span data-stu-id="a0192-169">Default startup limits</span></span>

<span data-ttu-id="a0192-170">Параметру *startupTimeLimit* модуля ASP.NET Core установлено значение по умолчанию 120 секунд.</span><span class="sxs-lookup"><span data-stu-id="a0192-170">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="a0192-171">Если оставить значение по умолчанию, то прежде чем журнал модуля зарегистрирует ошибку запуска приложения, может пройти до двух минут.</span><span class="sxs-lookup"><span data-stu-id="a0192-171">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="a0192-172">Сведения о настройке модуля см. в разделе [Атрибуты элемента aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="a0192-172">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="a0192-173">Устранение неполадок при запуске приложения</span><span class="sxs-lookup"><span data-stu-id="a0192-173">Troubleshoot app startup errors</span></span>

### <a name="enable-the-aspnet-core-module-debug-log"></a><span data-ttu-id="a0192-174">Включение журнала отладки модулей ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a0192-174">Enable the ASP.NET Core Module debug log</span></span>

<span data-ttu-id="a0192-175">Добавьте следующие параметры обработчика в файл *web.config* приложения, чтобы включить журналы отладки модулей ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="a0192-175">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug logs:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="a0192-176">Убедитесь, что указанный для журнала путь существует и удостоверение пула приложений имеет разрешения на запись в это расположение.</span><span class="sxs-lookup"><span data-stu-id="a0192-176">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

### <a name="application-event-log"></a><span data-ttu-id="a0192-177">Журнал событий приложений</span><span class="sxs-lookup"><span data-stu-id="a0192-177">Application Event Log</span></span>

<span data-ttu-id="a0192-178">Доступ к журналу событий приложений</span><span class="sxs-lookup"><span data-stu-id="a0192-178">Access the Application Event Log:</span></span>

1. <span data-ttu-id="a0192-179">Откройте меню "Пуск", выполните поиск по фразе **Просмотр событий** и выберите приложение **Просмотр событий**.</span><span class="sxs-lookup"><span data-stu-id="a0192-179">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="a0192-180">В **средстве просмотра событий** откройте узел **Журналы Windows**.</span><span class="sxs-lookup"><span data-stu-id="a0192-180">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="a0192-181">Выберите **Приложение**, чтобы открыть журнал событий приложения.</span><span class="sxs-lookup"><span data-stu-id="a0192-181">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="a0192-182">Проверьте здесь наличие ошибок, связанных с проблемным приложением.</span><span class="sxs-lookup"><span data-stu-id="a0192-182">Search for errors associated with the failing app.</span></span> <span data-ttu-id="a0192-183">Нам нужны ошибки со значениями *Модуль IIS AspNetCore* или *Модуль IIS Express AspNetCore* в столбце *Источник*.</span><span class="sxs-lookup"><span data-stu-id="a0192-183">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="a0192-184">Запуск приложения в командной строке</span><span class="sxs-lookup"><span data-stu-id="a0192-184">Run the app at a command prompt</span></span>

<span data-ttu-id="a0192-185">Многие ошибки запуска не создают полезные сведения в журнале событий приложения.</span><span class="sxs-lookup"><span data-stu-id="a0192-185">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="a0192-186">Для некоторых ошибок можно найти причину, запустив приложение в командной строке на компьютере размещения.</span><span class="sxs-lookup"><span data-stu-id="a0192-186">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="a0192-187">развертывание, зависящее от платформы;</span><span class="sxs-lookup"><span data-stu-id="a0192-187">Framework-dependent deployment</span></span>

<span data-ttu-id="a0192-188">Если развертываемое приложение [зависит от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="a0192-188">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="a0192-189">В командной строке перейдите в папку развертывания и запустите приложение, выполнив команду *dotnet.exe* для сборки приложения.</span><span class="sxs-lookup"><span data-stu-id="a0192-189">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="a0192-190">В следующей команде укажите нужное имя сборки вместо заполнителя \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="a0192-190">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="a0192-191">Выходные данные приложения, в том числе любые ошибки, будут выведены в окно консоли.</span><span class="sxs-lookup"><span data-stu-id="a0192-191">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="a0192-192">Если ошибки возникают при выполнении запроса к приложению, выполните запрос к узлу и порту, где Kestrel ожидает передачи данных.</span><span class="sxs-lookup"><span data-stu-id="a0192-192">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="a0192-193">Создайте запрос к `http://localhost:5000/`, используя узел и порт по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a0192-193">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="a0192-194">Если приложение отвечает на запросы по адресу конечной точки Kestrel как обычно, то проблема, вероятнее, связана с конфигурацией размещения, а не с самим приложением.</span><span class="sxs-lookup"><span data-stu-id="a0192-194">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="a0192-195">автономное развертывание;</span><span class="sxs-lookup"><span data-stu-id="a0192-195">Self-contained deployment</span></span>

<span data-ttu-id="a0192-196">Если приложение [развертывается автономно](/dotnet/core/deploying/#self-contained-deployments-scd), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="a0192-196">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="a0192-197">В командной строке перейдите в папку развертывания и запустите исполняемый файл приложения.</span><span class="sxs-lookup"><span data-stu-id="a0192-197">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="a0192-198">В следующей команде укажите нужное имя сборки вместо заполнителя \<assembly_name>: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="a0192-198">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="a0192-199">Выходные данные приложения, в том числе любые ошибки, будут выведены в окно консоли.</span><span class="sxs-lookup"><span data-stu-id="a0192-199">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="a0192-200">Если ошибки возникают при выполнении запроса к приложению, выполните запрос к узлу и порту, где Kestrel ожидает передачи данных.</span><span class="sxs-lookup"><span data-stu-id="a0192-200">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="a0192-201">Создайте запрос к `http://localhost:5000/`, используя узел и порт по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a0192-201">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="a0192-202">Если приложение отвечает на запросы по адресу конечной точки Kestrel как обычно, то проблема, вероятнее, связана с конфигурацией размещения, а не с самим приложением.</span><span class="sxs-lookup"><span data-stu-id="a0192-202">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="a0192-203">Журнал stdout модуля ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a0192-203">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="a0192-204">Чтобы включить и просмотреть журналы stdout, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="a0192-204">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="a0192-205">Перейдите в папку развертывания сайта на компьютере размещения.</span><span class="sxs-lookup"><span data-stu-id="a0192-205">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="a0192-206">Если здесь нет папки *logs*, создайте ее.</span><span class="sxs-lookup"><span data-stu-id="a0192-206">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="a0192-207">Сведения о том, как настроить в MSBuild автоматическое создание папки *logs* в развертывании, см. в статье [о структуре каталогов](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="a0192-207">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="a0192-208">Измените файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="a0192-208">Edit the *web.config* file.</span></span> <span data-ttu-id="a0192-209">Задайте для параметра **stdoutLogEnabled** значение `true` и измените путь **stdoutLogFile** так, чтобы он указывал на папку *logs* (например, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="a0192-209">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="a0192-210">В этом пути `stdout` обозначает префикс имени для файла журнала.</span><span class="sxs-lookup"><span data-stu-id="a0192-210">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="a0192-211">При создании файла журнала автоматически добавляются метка времени, идентификатор процесса и расширение файла.</span><span class="sxs-lookup"><span data-stu-id="a0192-211">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="a0192-212">Если указан префикс файла `stdout`, стандартный файл журнала будет иметь примерно такое имя: *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="a0192-212">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span> 
1. <span data-ttu-id="a0192-213">Убедитесь, что удостоверение пула приложений имеет разрешения на запись в папку *logs*.</span><span class="sxs-lookup"><span data-stu-id="a0192-213">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="a0192-214">Сохраните обновленный файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="a0192-214">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="a0192-215">Сделайте запрос к приложению.</span><span class="sxs-lookup"><span data-stu-id="a0192-215">Make a request to the app.</span></span>
1. <span data-ttu-id="a0192-216">Перейдите в папку *logs*.</span><span class="sxs-lookup"><span data-stu-id="a0192-216">Navigate to the *logs* folder.</span></span> <span data-ttu-id="a0192-217">Найдите и откройте последний журнал вывода stdout.</span><span class="sxs-lookup"><span data-stu-id="a0192-217">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="a0192-218">Проверьте, нет ли в нем ошибок.</span><span class="sxs-lookup"><span data-stu-id="a0192-218">Study the log for errors.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a0192-219">Завершив устранение неполадок, отключите ведение журнала stdout.</span><span class="sxs-lookup"><span data-stu-id="a0192-219">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="a0192-220">Измените файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="a0192-220">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="a0192-221">Задайте параметру **stdoutLogEnabled** значение `false`.</span><span class="sxs-lookup"><span data-stu-id="a0192-221">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="a0192-222">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="a0192-222">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="a0192-223">Ошибка при отключении журнала stdout может привести к сбоям приложения или сервера.</span><span class="sxs-lookup"><span data-stu-id="a0192-223">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="a0192-224">Ни размер файла журнала, ни количество создаваемых файлов журналов ничем не ограничены.</span><span class="sxs-lookup"><span data-stu-id="a0192-224">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="a0192-225">Для обычного ведения журналов для приложения ASP.NET Core используйте библиотеку, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов.</span><span class="sxs-lookup"><span data-stu-id="a0192-225">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="a0192-226">Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="a0192-226">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enable-the-developer-exception-page"></a><span data-ttu-id="a0192-227">Включение страницы исключений для разработчика</span><span class="sxs-lookup"><span data-stu-id="a0192-227">Enable the Developer Exception Page</span></span>

<span data-ttu-id="a0192-228">Можно добавить переменную среды `ASPNETCORE_ENVIRONMENT` [в файл web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables), чтобы запустить приложение в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="a0192-228">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="a0192-229">Если среда не переопределяется при запуске приложения использованием `UseEnvironment` в конструкторе узла, эта переменная среды позволяет отображать [страницу исключения для разработчика](xref:fundamentals/error-handling) при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="a0192-229">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="InProcess">
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

<span data-ttu-id="a0192-230">Настройка переменной среды `ASPNETCORE_ENVIRONMENT` рекомендуется только на промежуточных и тестовых серверах, доступ к которым из Интернета закрыт.</span><span class="sxs-lookup"><span data-stu-id="a0192-230">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="a0192-231">Завершив устранение неполадок, удалите эту переменную среды из файла *web.config*.</span><span class="sxs-lookup"><span data-stu-id="a0192-231">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="a0192-232">Сведения о настройке переменных среды в файле *web.config* см. в статье о [дочернем элементе environmentVariables в aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="a0192-232">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="a0192-233">Стандартные ошибки запуска</span><span class="sxs-lookup"><span data-stu-id="a0192-233">Common startup errors</span></span>

<span data-ttu-id="a0192-234">См. раздел <xref:host-and-deploy/azure-iis-errors-reference>.</span><span class="sxs-lookup"><span data-stu-id="a0192-234">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="a0192-235">В этой статье рассматривается большинство распространенных ошибок, препятствующих запуску приложений.</span><span class="sxs-lookup"><span data-stu-id="a0192-235">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="a0192-236">Получение данных из приложения</span><span class="sxs-lookup"><span data-stu-id="a0192-236">Obtain data from an app</span></span>

<span data-ttu-id="a0192-237">Если приложение способно отвечать на запросы, получите данные о запросе, подключении и дополнительные данные из приложений с помощью встроенного терминала ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="a0192-237">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="a0192-238">Дополнительные сведения и примеры с кодом см. здесь: <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="a0192-238">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

## <a name="create-a-dump"></a><span data-ttu-id="a0192-239">Создание дампа</span><span class="sxs-lookup"><span data-stu-id="a0192-239">Create a dump</span></span>

<span data-ttu-id="a0192-240">*Дамп* представляет собой моментальный снимок системной памяти и может помочь определить причину аварийного завершения, сбоя запуска или медленной работы приложения.</span><span class="sxs-lookup"><span data-stu-id="a0192-240">A *dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="a0192-241">Аварийное завершение работы приложения или исключение</span><span class="sxs-lookup"><span data-stu-id="a0192-241">App crashes or encounters an exception</span></span>

<span data-ttu-id="a0192-242">Получите и проанализируйте дамп из [отчетов об ошибках Windows (WER)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="a0192-242">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="a0192-243">Создайте папку для хранения файлов аварийного дампа в `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="a0192-243">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="a0192-244">Пул приложений должен иметь доступ на запись к папке.</span><span class="sxs-lookup"><span data-stu-id="a0192-244">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="a0192-245">Запустите [скрипт PowerShell EnableDumps](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/troubleshoot/scripts/EnableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="a0192-245">Run the [EnableDumps PowerShell script](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/troubleshoot/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="a0192-246">Если приложение использует [модель размещения в процессе](xref:fundamentals/servers/index#in-process-hosting-model), выполните скрипт для *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="a0192-246">If the app uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```
   * <span data-ttu-id="a0192-247">Если приложение использует [модель размещения вне процесса](xref:fundamentals/servers/index#out-of-process-hosting-model), выполните скрипт для *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="a0192-247">If the app uses the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```
1. <span data-ttu-id="a0192-248">Запустите приложение в условиях, вызывающих аварийное завершение.</span><span class="sxs-lookup"><span data-stu-id="a0192-248">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="a0192-249">После аварийного завершения запустите [скрипт PowerShell DisableDumps](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/troubleshoot/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="a0192-249">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/troubleshoot/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="a0192-250">Если приложение использует [модель размещения в процессе](xref:fundamentals/servers/index#in-process-hosting-model), выполните скрипт для *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="a0192-250">If the app uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```
   * <span data-ttu-id="a0192-251">Если приложение использует [модель размещения вне процесса](xref:fundamentals/servers/index#out-of-process-hosting-model), выполните скрипт для *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="a0192-251">If the app uses the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="a0192-252">Когда приложение аварийно завершит работу и сбор дампов будет выполнен, приложение сможет завершить работу обычным образом.</span><span class="sxs-lookup"><span data-stu-id="a0192-252">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="a0192-253">Скрипт PowerShell настраивает отчеты об ошибках Windows для сбора до пяти дампов для приложения.</span><span class="sxs-lookup"><span data-stu-id="a0192-253">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="a0192-254">Аварийные дампы могут занимать много места на диске (до нескольких гигабайтов каждый).</span><span class="sxs-lookup"><span data-stu-id="a0192-254">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="a0192-255">Приложение перестает отвечать на запросы, не запускается или работает в обычном режиме</span><span class="sxs-lookup"><span data-stu-id="a0192-255">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="a0192-256">Когда приложение *перестает отвечать на запросы* (но аварийное завершение не происходит), не запускается или работает в обычном режиме, см. раздел [Файлы дампа пользовательского режима: выбор лучшего инструмента](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool), чтобы выбрать подходящий инструмент для создания дампа.</span><span class="sxs-lookup"><span data-stu-id="a0192-256">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

### <a name="analyze-the-dump"></a><span data-ttu-id="a0192-257">Анализ дампа</span><span class="sxs-lookup"><span data-stu-id="a0192-257">Analyze the dump</span></span>

<span data-ttu-id="a0192-258">Дамп можно проанализировать несколькими способами.</span><span class="sxs-lookup"><span data-stu-id="a0192-258">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="a0192-259">Дополнительные сведения см. в разделе [Анализ файла дампа пользовательского режима](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="a0192-259">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="a0192-260">Удаленная отладка</span><span class="sxs-lookup"><span data-stu-id="a0192-260">Remote debugging</span></span>

<span data-ttu-id="a0192-261">Изучите раздел документации по Visual Studio [об удаленной отладке ASP.NET Core на удаленном компьютере IIS в Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer).</span><span class="sxs-lookup"><span data-stu-id="a0192-261">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="a0192-262">Application Insights</span><span class="sxs-lookup"><span data-stu-id="a0192-262">Application Insights</span></span>

<span data-ttu-id="a0192-263">[Application Insights](/azure/application-insights/) предоставляет данные телеметрии из приложений, размещенных в IIS, в том числе возможности регистрации ошибок и создания отчетов.</span><span class="sxs-lookup"><span data-stu-id="a0192-263">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="a0192-264">Application Insights может сообщать только об ошибках, возникающих после запуска приложения, когда становятся доступными функции ведения журнала приложения.</span><span class="sxs-lookup"><span data-stu-id="a0192-264">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="a0192-265">Дополнительные сведения см. в статье [Application Insights для ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="a0192-265">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-advice"></a><span data-ttu-id="a0192-266">Дополнительные советы</span><span class="sxs-lookup"><span data-stu-id="a0192-266">Additional advice</span></span>

<span data-ttu-id="a0192-267">Иногда приложения-функции перестают работать сразу после обновления пакета SDK для .NET Core на компьютере разработки или обновления пакетов в самом приложении.</span><span class="sxs-lookup"><span data-stu-id="a0192-267">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="a0192-268">В некоторых случаях в результате важного обновления несогласованные версии пакетов могут привести к нарушению работы приложения.</span><span class="sxs-lookup"><span data-stu-id="a0192-268">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="a0192-269">Большинство этих проблем можно исправить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a0192-269">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="a0192-270">Удалите папки *bin* и *obj*.</span><span class="sxs-lookup"><span data-stu-id="a0192-270">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="a0192-271">Очистите кэши пакетов, размещенные в папках *%UserProfile%\\.nuget\\packages* и *%LocalAppData%\\Nuget\\v3-cache*.</span><span class="sxs-lookup"><span data-stu-id="a0192-271">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="a0192-272">Восстановите и перестройте проект.</span><span class="sxs-lookup"><span data-stu-id="a0192-272">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="a0192-273">Убедитесь, что предыдущее развертывание на сервере полностью удалено, прежде чем развертывать его снова.</span><span class="sxs-lookup"><span data-stu-id="a0192-273">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="a0192-274">Очистить кэши пакета проще всего командой `dotnet nuget locals all --clear` из командной строки.</span><span class="sxs-lookup"><span data-stu-id="a0192-274">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
>
> <span data-ttu-id="a0192-275">Также очистку кэшей пакетов можно выполнить средством [nuget.exe](https://www.nuget.org/downloads) или командой `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="a0192-275">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="a0192-276">*NuGet.exe* не входит в пакет установки операционной системы Windows для настольных компьютеров и его нужно получить отдельно на [веб-сайте NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="a0192-276">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a0192-277">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a0192-277">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
