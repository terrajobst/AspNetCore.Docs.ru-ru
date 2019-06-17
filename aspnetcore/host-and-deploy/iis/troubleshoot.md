---
title: Устранение неполадок ASP.NET Core в службах IIS
author: guardrex
description: Сведения о диагностике проблем с развертываниями приложений ASP.NET Core на платформе IIS.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/28/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: cb42a262c89c27fa350e936184f8ddb3a02788f0
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034739"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="354fa-103">Устранение неполадок ASP.NET Core в службах IIS</span><span class="sxs-lookup"><span data-stu-id="354fa-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="354fa-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="354fa-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="354fa-105">Эта статья содержит инструкции по диагностике проблемы запуска приложения ASP.NET Core, размещенного в [службах IIS](/iis).</span><span class="sxs-lookup"><span data-stu-id="354fa-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="354fa-106">Информация в этой статье относится к размещению в службах IIS на Windows Server и Windows Desktop.</span><span class="sxs-lookup"><span data-stu-id="354fa-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="354fa-107">В Visual Studio проект ASP.NET Core по умолчанию использует размещение в [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) на период отладки.</span><span class="sxs-lookup"><span data-stu-id="354fa-107">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="354fa-108">Рекомендации в этой статье позволяют устранять неполадки, проявляющиеся как *502.5 — ошибка процесса* или *500.30 — ошибка запуска* при локальной отладке.</span><span class="sxs-lookup"><span data-stu-id="354fa-108">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="354fa-109">В Visual Studio проект ASP.NET Core по умолчанию использует размещение в [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) на период отладки.</span><span class="sxs-lookup"><span data-stu-id="354fa-109">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="354fa-110">Рекомендации в этой статье позволяют устранять неполадки, проявляющиеся как *сбой процесса 502.5* при локальной отладке.</span><span class="sxs-lookup"><span data-stu-id="354fa-110">A *502.5 Process Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

<span data-ttu-id="354fa-111">Дополнительные статьи по устранению неполадок:</span><span class="sxs-lookup"><span data-stu-id="354fa-111">Additional troubleshooting topics:</span></span>

<span data-ttu-id="354fa-112"><xref:host-and-deploy/azure-apps/troubleshoot> Служба приложений использует для размещения приложений [модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module) и IIS. Инструкции по использованию Службы приложений см. в посвященных ей статьях.</span><span class="sxs-lookup"><span data-stu-id="354fa-112"><xref:host-and-deploy/azure-apps/troubleshoot> Although App Service uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and IIS to host apps, see the dedicated topic for instructions specific to App Service.</span></span>

<span data-ttu-id="354fa-113"><xref:fundamentals/error-handling> — узнайте, как обрабатывать ошибки в приложениях ASP.NET Core при разработке в локальной системе.</span><span class="sxs-lookup"><span data-stu-id="354fa-113"><xref:fundamentals/error-handling> Discover how to handle errors in ASP.NET Core apps during development on a local system.</span></span>

<span data-ttu-id="354fa-114">[Знакомство с отладчиком Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger). В этой статье описаны возможности отладчика Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="354fa-114">[Learn to debug using Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) This topic introduces the features of the Visual Studio debugger.</span></span>

<span data-ttu-id="354fa-115">[Статья об отладке с помощью Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging). Узнайте о поддержке отладки, встроенной в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="354fa-115">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) Learn about the debugging support built into Visual Studio Code.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="354fa-116">Ошибки при запуске приложения</span><span class="sxs-lookup"><span data-stu-id="354fa-116">App startup errors</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="354fa-117">502.5 Process Failure (ошибка процесса)</span><span class="sxs-lookup"><span data-stu-id="354fa-117">502.5 Process Failure</span></span>

<span data-ttu-id="354fa-118">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="354fa-118">The worker process fails.</span></span> <span data-ttu-id="354fa-119">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="354fa-119">The app doesn't start.</span></span>

<span data-ttu-id="354fa-120">Модуль ASP.NET Core пытается запустить процесс dotnet серверной части, но он не запускается.</span><span class="sxs-lookup"><span data-stu-id="354fa-120">The ASP.NET Core Module attempts to start the backend dotnet process but it fails to start.</span></span> <span data-ttu-id="354fa-121">Обычно причину сбоя при запуске процесса можно определить по записям в [журнале событий приложения](#application-event-log) и [журнале вывода stdout модуля ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="354fa-121">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="354fa-122">Распространенной причиной сбоя является неправильная настройка приложения с выбором отсутствующей версии общей платформы ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="354fa-122">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="354fa-123">Проверьте, какие версии общей платформы ASP.NET Core установлены на целевом компьютере.</span><span class="sxs-lookup"><span data-stu-id="354fa-123">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="354fa-124">*Общая платформа* — это набор сборок (*DLLl*-файлы), которые установлены на компьютере и на которые ссылается метапакет, например `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="354fa-124">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="354fa-125">В ссылках метапакета может быть указана минимальная требуемая версия.</span><span class="sxs-lookup"><span data-stu-id="354fa-125">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="354fa-126">Дополнительную информацию см. в этой публикации об [общей платформе](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span><span class="sxs-lookup"><span data-stu-id="354fa-126">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="354fa-127">Когда ошибки в конфигурации размещения или приложения приводят к сбою рабочего процесса, возвращается страница ошибки *502.5 — ошибка процесса*.</span><span class="sxs-lookup"><span data-stu-id="354fa-127">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

![Окно браузера со страницей "502.5 — ошибка процесса"](troubleshoot/_static/process-failure-page.png)

::: moniker range="= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="354fa-129">500.30 In-Process Startup Failure (ошибка внутрипроцессного запуска)</span><span class="sxs-lookup"><span data-stu-id="354fa-129">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="354fa-130">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="354fa-130">The worker process fails.</span></span> <span data-ttu-id="354fa-131">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="354fa-131">The app doesn't start.</span></span>

<span data-ttu-id="354fa-132">Модуль ASP.NET Core пытается запустить среду CLR .NET Core внутри процесса, но она не запускается.</span><span class="sxs-lookup"><span data-stu-id="354fa-132">The ASP.NET Core Module attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="354fa-133">Обычно причину сбоя при запуске процесса можно определить по записям в [журнале событий приложения](#application-event-log) и [журнале вывода stdout модуля ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="354fa-133">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="354fa-134">Распространенной причиной сбоя является неправильная настройка приложения с выбором отсутствующей версии общей платформы ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="354fa-134">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="354fa-135">Проверьте, какие версии общей платформы ASP.NET Core установлены на целевом компьютере.</span><span class="sxs-lookup"><span data-stu-id="354fa-135">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="354fa-136">500.0 In-Process Handler Load Failure (ошибка загрузки внутрипроцессного обработчика)</span><span class="sxs-lookup"><span data-stu-id="354fa-136">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="354fa-137">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="354fa-137">The worker process fails.</span></span> <span data-ttu-id="354fa-138">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="354fa-138">The app doesn't start.</span></span>

<span data-ttu-id="354fa-139">Модулю ASP.NET Core не удается найти среду CLR .NET Core и внутрипроцессный обработчик запросов (*aspnetcorev2_inprocess.dll*).</span><span class="sxs-lookup"><span data-stu-id="354fa-139">The ASP.NET Core Module fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="354fa-140">Проверьте следующие моменты:</span><span class="sxs-lookup"><span data-stu-id="354fa-140">Check that:</span></span>

* <span data-ttu-id="354fa-141">приложение предназначено для пакета NuGet [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) или [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app);</span><span class="sxs-lookup"><span data-stu-id="354fa-141">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="354fa-142">версия общей платформы ASP.NET Core, для которой предназначено приложение, установлена на целевом компьютере.</span><span class="sxs-lookup"><span data-stu-id="354fa-142">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="354fa-143">500.0 Out-Of-Process Handler Load Failure (ошибка загрузки внепроцессного обработчика)</span><span class="sxs-lookup"><span data-stu-id="354fa-143">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="354fa-144">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="354fa-144">The worker process fails.</span></span> <span data-ttu-id="354fa-145">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="354fa-145">The app doesn't start.</span></span>

<span data-ttu-id="354fa-146">Модулю ASP.NET Core не удается найти внепроцессный обработчик запросов.</span><span class="sxs-lookup"><span data-stu-id="354fa-146">The ASP.NET Core Module fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="354fa-147">Убедитесь, что *aspnetcorev2_outofprocess.dll* находится во вложенной папке рядом с *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="354fa-147">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="354fa-148">500.31 ANCM Failed to Find Native Dependencies (ANCM не удалось найти собственные зависимости)</span><span class="sxs-lookup"><span data-stu-id="354fa-148">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="354fa-149">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="354fa-149">The worker process fails.</span></span> <span data-ttu-id="354fa-150">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="354fa-150">The app doesn't start.</span></span>

<span data-ttu-id="354fa-151">Модуль ASP.NET Core пытается запустить среду выполнения .NET Core внутри процесса, но она не запускается.</span><span class="sxs-lookup"><span data-stu-id="354fa-151">The ASP.NET Core Module attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="354fa-152">Наиболее распространенная причина этой ошибки в том, что среда выполнения `Microsoft.NETCore.App` или `Microsoft.AspNetCore.App` не установлена.</span><span class="sxs-lookup"><span data-stu-id="354fa-152">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="354fa-153">Если целевой платформой развернутого приложения является ASP.NET Core 3.0, но эта версия отсутствует на компьютере, происходит данная ошибка.</span><span class="sxs-lookup"><span data-stu-id="354fa-153">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="354fa-154">Пример сообщения об ошибке:</span><span class="sxs-lookup"><span data-stu-id="354fa-154">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="354fa-155">В сообщении об ошибке перечислены все установленные версии .NET Core и указывается версия, которая требуется приложению.</span><span class="sxs-lookup"><span data-stu-id="354fa-155">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="354fa-156">Чтобы устранить эту ошибку, выполните одно из указанных ниже действий.</span><span class="sxs-lookup"><span data-stu-id="354fa-156">To fix this error, either:</span></span>

* <span data-ttu-id="354fa-157">Установите соответствующую версию .NET Core на компьютере.</span><span class="sxs-lookup"><span data-stu-id="354fa-157">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="354fa-158">Измените целевую версию .NET Core приложения на версию, имеющуюся на компьютере.</span><span class="sxs-lookup"><span data-stu-id="354fa-158">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="354fa-159">Опубликуйте приложение как [автономное развертывание](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="354fa-159">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="354fa-160">При запуске в среде разработки (переменная среды `ASPNETCORE_ENVIRONMENT` имеет значение `Development`) ошибка записывается в ответ HTTP.</span><span class="sxs-lookup"><span data-stu-id="354fa-160">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="354fa-161">Причину сбоя при запуске процесса можно также узнать в [журнале событий приложения](#application-event-log).</span><span class="sxs-lookup"><span data-stu-id="354fa-161">The cause of a process startup failure is also found in the [Application Event Log](#application-event-log).</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="354fa-162">500.32 ANCM Failed to Load dll (ANCM не удалось загрузить библиотеку DLL)</span><span class="sxs-lookup"><span data-stu-id="354fa-162">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="354fa-163">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="354fa-163">The worker process fails.</span></span> <span data-ttu-id="354fa-164">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="354fa-164">The app doesn't start.</span></span>

<span data-ttu-id="354fa-165">Наиболее распространенная причина этой ошибки в том, что приложение опубликовано для несовместимой архитектуры процессора.</span><span class="sxs-lookup"><span data-stu-id="354fa-165">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="354fa-166">Если рабочий процесс выполняется как 32-разрядное приложение, но приложение было опубликовано для 64-разрядной архитектуры, происходит эта ошибка.</span><span class="sxs-lookup"><span data-stu-id="354fa-166">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="354fa-167">Чтобы устранить эту ошибку, выполните одно из указанных ниже действий.</span><span class="sxs-lookup"><span data-stu-id="354fa-167">To fix this error, either:</span></span>

* <span data-ttu-id="354fa-168">Повторно опубликуйте приложение для той же архитектуры процессора, для которой предназначен рабочий процесс.</span><span class="sxs-lookup"><span data-stu-id="354fa-168">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="354fa-169">Опубликуйте приложение как [зависящее от платформы развертывание](/dotnet/core/deploying/#framework-dependent-executables-fde).</span><span class="sxs-lookup"><span data-stu-id="354fa-169">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="354fa-170">500.33 ANCM Request Handler Load Failure (Ошибка загрузки обработчика запросов ANCM)</span><span class="sxs-lookup"><span data-stu-id="354fa-170">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="354fa-171">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="354fa-171">The worker process fails.</span></span> <span data-ttu-id="354fa-172">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="354fa-172">The app doesn't start.</span></span>

<span data-ttu-id="354fa-173">Приложение не ссылается на платформу `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="354fa-173">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="354fa-174">В модуле ASP.NET Core могут размещаться только приложения, предназначенные для платформы `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="354fa-174">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the ASP.NET Core Module.</span></span>

<span data-ttu-id="354fa-175">Для устранения этой ошибки убедитесь в том, что приложение предназначено для платформы `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="354fa-175">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="354fa-176">В файле `.runtimeconfig.json` проверьте, является ли эта платформа целевой для приложения.</span><span class="sxs-lookup"><span data-stu-id="354fa-176">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="354fa-177">500.34 ANCM Mixed Hosting Models Not Supported (Смешанные модели размещения ANCM не поддерживаются)</span><span class="sxs-lookup"><span data-stu-id="354fa-177">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="354fa-178">В одном рабочем процессе не могут выполняться одновременно внутрипроцессное приложение и внепроцессное приложение.</span><span class="sxs-lookup"><span data-stu-id="354fa-178">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="354fa-179">Для устранения этой ошибки запускайте приложения в отдельных пулах приложений IIS.</span><span class="sxs-lookup"><span data-stu-id="354fa-179">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="354fa-180">500.35 ANCM Multiple In-Process Applications in same Process (Несколько внутрипроцессных приложений ANCM в одном процессе)</span><span class="sxs-lookup"><span data-stu-id="354fa-180">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="354fa-181">В одном рабочем процессе не могут выполняться одновременно внутрипроцессное приложение и внепроцессное приложение.</span><span class="sxs-lookup"><span data-stu-id="354fa-181">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="354fa-182">Для устранения этой ошибки запускайте приложения в отдельных пулах приложений IIS.</span><span class="sxs-lookup"><span data-stu-id="354fa-182">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="354fa-183">500.36 ANCM Out-Of-Process Handler Load Failure (Ошибка загрузки внепроцессного обработчика ANCM)</span><span class="sxs-lookup"><span data-stu-id="354fa-183">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="354fa-184">Рядом с файлом *aspnetcorev2.dll* нет внепроцессного обработчика запросов *aspnetcorev2_outofprocess.dll*.</span><span class="sxs-lookup"><span data-stu-id="354fa-184">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="354fa-185">Это свидетельствует о поврежденной установке модуля ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="354fa-185">This indicates a corrupted installation of the ASP.NET Core Module.</span></span>

<span data-ttu-id="354fa-186">Для устранения этой ошибки восстановите установку [пакета размещения .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (для служб IIS) или Visual Studio (для IIS Express).</span><span class="sxs-lookup"><span data-stu-id="354fa-186">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="354fa-187">500.37 ANCM Failed to Start Within Startup Time Limit (Модуль ANCM не запустился в течение предельного времени запуска)</span><span class="sxs-lookup"><span data-stu-id="354fa-187">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="354fa-188">Модуль ANCM не запустился в течение заданного предельного времени запуска.</span><span class="sxs-lookup"><span data-stu-id="354fa-188">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="354fa-189">По умолчанию время ожидания составляет 120 секунд.</span><span class="sxs-lookup"><span data-stu-id="354fa-189">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="354fa-190">Эта ошибка может произойти, если на одном компьютере запускается много приложений.</span><span class="sxs-lookup"><span data-stu-id="354fa-190">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="354fa-191">Обратите внимание на пики использования ЦП и памяти на сервере во время запуска.</span><span class="sxs-lookup"><span data-stu-id="354fa-191">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="354fa-192">Возможно, потребуется регулировать процесс запуска нескольких приложений.</span><span class="sxs-lookup"><span data-stu-id="354fa-192">You may need to stagger the startup process of multiple apps.</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="354fa-193">500.30 In-Process Startup Failure (ошибка внутрипроцессного запуска)</span><span class="sxs-lookup"><span data-stu-id="354fa-193">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="354fa-194">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="354fa-194">The worker process fails.</span></span> <span data-ttu-id="354fa-195">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="354fa-195">The app doesn't start.</span></span>

<span data-ttu-id="354fa-196">Модуль ASP.NET Core пытается запустить среду выполнения .NET Core внутри процесса, но она не запускается.</span><span class="sxs-lookup"><span data-stu-id="354fa-196">The ASP.NET Core Module attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="354fa-197">Обычно причина сбоя при запуске процесса определяется по записям в [журнале событий приложения](#application-event-log) и [журнале вывода stdout модуля ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="354fa-197">The cause of a process startup failure is usually determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="354fa-198">500.0 In-Process Handler Load Failure (ошибка загрузки внутрипроцессного обработчика)</span><span class="sxs-lookup"><span data-stu-id="354fa-198">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="354fa-199">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="354fa-199">The worker process fails.</span></span> <span data-ttu-id="354fa-200">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="354fa-200">The app doesn't start.</span></span>

<span data-ttu-id="354fa-201">При загрузке компонентов модуля ASP.NET Core произошла неизвестная ошибка.</span><span class="sxs-lookup"><span data-stu-id="354fa-201">An unknown error occurred loading ASP.NET Core Module components.</span></span> <span data-ttu-id="354fa-202">Выполните одно из указанных ниже действий.</span><span class="sxs-lookup"><span data-stu-id="354fa-202">Take one of the following actions:</span></span>

* <span data-ttu-id="354fa-203">Обратитесь в [службу поддержки Майкрософт](https://support.microsoft.com/oas/default.aspx?prid=15832) (выберите **Средства разработчика**, а затем — **ASP.NET Core**).</span><span class="sxs-lookup"><span data-stu-id="354fa-203">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="354fa-204">Задайте вопрос на сайте Stack Overflow.</span><span class="sxs-lookup"><span data-stu-id="354fa-204">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="354fa-205">Сообщите о проблеме в нашем [репозитории GitHub](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="354fa-205">File an issue on our [GitHub repository](https://github.com/aspnet/AspNetCore).</span></span>

::: moniker-end

### <a name="500-internal-server-error"></a><span data-ttu-id="354fa-206">500 Internal Server Error (внутренняя ошибка сервера)</span><span class="sxs-lookup"><span data-stu-id="354fa-206">500 Internal Server Error</span></span>

<span data-ttu-id="354fa-207">Приложение запускается, но ошибка не позволяет серверу выполнить запрос.</span><span class="sxs-lookup"><span data-stu-id="354fa-207">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="354fa-208">Эта ошибка возникает в коде приложения при запуске или при создании ответа.</span><span class="sxs-lookup"><span data-stu-id="354fa-208">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="354fa-209">Ответ может не иметь содержимого или ответ в браузере может выглядеть как *500 — внутренняя ошибка сервера*.</span><span class="sxs-lookup"><span data-stu-id="354fa-209">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="354fa-210">Как правило, в журнале событий приложения указывается, что приложение запускается в обычном режиме.</span><span class="sxs-lookup"><span data-stu-id="354fa-210">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="354fa-211">Сервер действует правильно.</span><span class="sxs-lookup"><span data-stu-id="354fa-211">From the server's perspective, that's correct.</span></span> <span data-ttu-id="354fa-212">Приложение запустилось, но не может сформировать допустимый ответ.</span><span class="sxs-lookup"><span data-stu-id="354fa-212">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="354fa-213">Чтобы устранить проблему, [запустите приложение в командной строке](#run-the-app-at-a-command-prompt) на сервере или [включите журнал вывода stdout для модуля ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="354fa-213">[Run the app at a command prompt](#run-the-app-at-a-command-prompt) on the server or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="354fa-214">Не удалось запустить приложение (код ошибки "0x800700c1")</span><span class="sxs-lookup"><span data-stu-id="354fa-214">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="354fa-215">Не удалось запустить приложение, так как не удалось загрузить сборку приложения ( *.dll*).</span><span class="sxs-lookup"><span data-stu-id="354fa-215">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="354fa-216">Эта ошибка возникает, если существует несоответствие разрядности опубликованного приложения и процесса w3wp/iisexpress.</span><span class="sxs-lookup"><span data-stu-id="354fa-216">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="354fa-217">Убедитесь, что параметр 32-разрядности пула приложений указан правильно:</span><span class="sxs-lookup"><span data-stu-id="354fa-217">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="354fa-218">Выберите пул приложений в диспетчере служб IIS в разделе **Пулы приложений**.</span><span class="sxs-lookup"><span data-stu-id="354fa-218">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="354fa-219">Выберите **Дополнительные параметры** в разделе **Изменение пула приложений** панели **Действия**.</span><span class="sxs-lookup"><span data-stu-id="354fa-219">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="354fa-220">Установите параметр **Включить 32-разрядные приложения**:</span><span class="sxs-lookup"><span data-stu-id="354fa-220">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="354fa-221">При развертывании 32-разрядного (x86) приложения задайте значение `True`.</span><span class="sxs-lookup"><span data-stu-id="354fa-221">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="354fa-222">При развертывании 64-разрядного (x64) приложения задайте значение `False`.</span><span class="sxs-lookup"><span data-stu-id="354fa-222">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="354fa-223">Сброс подключения</span><span class="sxs-lookup"><span data-stu-id="354fa-223">Connection reset</span></span>

<span data-ttu-id="354fa-224">Если ошибка возникает после отправки заголовков, сервер уже не может отправить страницу **500 — внутренняя ошибка сервера**.</span><span class="sxs-lookup"><span data-stu-id="354fa-224">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="354fa-225">Это часто происходит, когда ошибка возникает во время сериализации составных объектов ответа.</span><span class="sxs-lookup"><span data-stu-id="354fa-225">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="354fa-226">Этот тип ошибки отображается на стороне клиента как ошибка *Сброс подключения*.</span><span class="sxs-lookup"><span data-stu-id="354fa-226">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="354fa-227">[Ведение журналов для приложений](xref:fundamentals/logging/index) может помочь устранить эти ошибки.</span><span class="sxs-lookup"><span data-stu-id="354fa-227">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="354fa-228">Ограничения по умолчанию при запуске</span><span class="sxs-lookup"><span data-stu-id="354fa-228">Default startup limits</span></span>

<span data-ttu-id="354fa-229">Параметру *startupTimeLimit* модуля ASP.NET Core установлено значение по умолчанию 120 секунд.</span><span class="sxs-lookup"><span data-stu-id="354fa-229">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="354fa-230">Если оставить значение по умолчанию, то прежде чем журнал модуля зарегистрирует ошибку запуска приложения, может пройти до двух минут.</span><span class="sxs-lookup"><span data-stu-id="354fa-230">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="354fa-231">Сведения о настройке модуля см. в разделе [Атрибуты элемента aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="354fa-231">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="354fa-232">Устранение неполадок при запуске приложения</span><span class="sxs-lookup"><span data-stu-id="354fa-232">Troubleshoot app startup errors</span></span>

### <a name="enable-the-aspnet-core-module-debug-log"></a><span data-ttu-id="354fa-233">Включение журнала отладки модулей ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="354fa-233">Enable the ASP.NET Core Module debug log</span></span>

<span data-ttu-id="354fa-234">Добавьте следующие параметры обработчика в файл *web.config* приложения, чтобы включить журналы отладки модулей ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="354fa-234">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug logs:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="354fa-235">Убедитесь, что указанный для журнала путь существует и удостоверение пула приложений имеет разрешения на запись в это расположение.</span><span class="sxs-lookup"><span data-stu-id="354fa-235">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

### <a name="application-event-log"></a><span data-ttu-id="354fa-236">Журнал событий приложений</span><span class="sxs-lookup"><span data-stu-id="354fa-236">Application Event Log</span></span>

<span data-ttu-id="354fa-237">Доступ к журналу событий приложений</span><span class="sxs-lookup"><span data-stu-id="354fa-237">Access the Application Event Log:</span></span>

1. <span data-ttu-id="354fa-238">Откройте меню "Пуск", выполните поиск по фразе **Просмотр событий** и выберите приложение **Просмотр событий**.</span><span class="sxs-lookup"><span data-stu-id="354fa-238">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="354fa-239">В **средстве просмотра событий** откройте узел **Журналы Windows**.</span><span class="sxs-lookup"><span data-stu-id="354fa-239">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="354fa-240">Выберите **Приложение**, чтобы открыть журнал событий приложения.</span><span class="sxs-lookup"><span data-stu-id="354fa-240">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="354fa-241">Проверьте здесь наличие ошибок, связанных с проблемным приложением.</span><span class="sxs-lookup"><span data-stu-id="354fa-241">Search for errors associated with the failing app.</span></span> <span data-ttu-id="354fa-242">Нам нужны ошибки со значениями *Модуль IIS AspNetCore* или *Модуль IIS Express AspNetCore* в столбце *Источник*.</span><span class="sxs-lookup"><span data-stu-id="354fa-242">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="354fa-243">Запуск приложения в командной строке</span><span class="sxs-lookup"><span data-stu-id="354fa-243">Run the app at a command prompt</span></span>

<span data-ttu-id="354fa-244">Многие ошибки запуска не создают полезные сведения в журнале событий приложения.</span><span class="sxs-lookup"><span data-stu-id="354fa-244">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="354fa-245">Для некоторых ошибок можно найти причину, запустив приложение в командной строке на компьютере размещения.</span><span class="sxs-lookup"><span data-stu-id="354fa-245">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="354fa-246">развертывание, зависящее от платформы;</span><span class="sxs-lookup"><span data-stu-id="354fa-246">Framework-dependent deployment</span></span>

<span data-ttu-id="354fa-247">Если развертываемое приложение [зависит от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="354fa-247">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="354fa-248">В командной строке перейдите в папку развертывания и запустите приложение, выполнив команду *dotnet.exe* для сборки приложения.</span><span class="sxs-lookup"><span data-stu-id="354fa-248">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="354fa-249">В следующей команде укажите нужное имя сборки вместо заполнителя \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="354fa-249">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="354fa-250">Выходные данные приложения, в том числе любые ошибки, будут выведены в окно консоли.</span><span class="sxs-lookup"><span data-stu-id="354fa-250">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="354fa-251">Если ошибки возникают при выполнении запроса к приложению, выполните запрос к узлу и порту, где Kestrel ожидает передачи данных.</span><span class="sxs-lookup"><span data-stu-id="354fa-251">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="354fa-252">Создайте запрос к `http://localhost:5000/`, используя узел и порт по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="354fa-252">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="354fa-253">Если приложение отвечает на запросы по адресу конечной точки Kestrel как обычно, то проблема, вероятнее, связана с конфигурацией размещения, а не с самим приложением.</span><span class="sxs-lookup"><span data-stu-id="354fa-253">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="354fa-254">автономное развертывание;</span><span class="sxs-lookup"><span data-stu-id="354fa-254">Self-contained deployment</span></span>

<span data-ttu-id="354fa-255">Если приложение [развертывается автономно](/dotnet/core/deploying/#self-contained-deployments-scd), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="354fa-255">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="354fa-256">В командной строке перейдите в папку развертывания и запустите исполняемый файл приложения.</span><span class="sxs-lookup"><span data-stu-id="354fa-256">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="354fa-257">В следующей команде укажите нужное имя сборки вместо заполнителя \<assembly_name>: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="354fa-257">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="354fa-258">Выходные данные приложения, в том числе любые ошибки, будут выведены в окно консоли.</span><span class="sxs-lookup"><span data-stu-id="354fa-258">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="354fa-259">Если ошибки возникают при выполнении запроса к приложению, выполните запрос к узлу и порту, где Kestrel ожидает передачи данных.</span><span class="sxs-lookup"><span data-stu-id="354fa-259">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="354fa-260">Создайте запрос к `http://localhost:5000/`, используя узел и порт по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="354fa-260">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="354fa-261">Если приложение отвечает на запросы по адресу конечной точки Kestrel как обычно, то проблема, вероятнее, связана с конфигурацией размещения, а не с самим приложением.</span><span class="sxs-lookup"><span data-stu-id="354fa-261">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="354fa-262">Журнал stdout модуля ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="354fa-262">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="354fa-263">Чтобы включить и просмотреть журналы stdout, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="354fa-263">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="354fa-264">Перейдите в папку развертывания сайта на компьютере размещения.</span><span class="sxs-lookup"><span data-stu-id="354fa-264">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="354fa-265">Если здесь нет папки *logs*, создайте ее.</span><span class="sxs-lookup"><span data-stu-id="354fa-265">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="354fa-266">Сведения о том, как настроить в MSBuild автоматическое создание папки *logs* в развертывании, см. в статье [о структуре каталогов](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="354fa-266">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="354fa-267">Измените файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="354fa-267">Edit the *web.config* file.</span></span> <span data-ttu-id="354fa-268">Задайте для параметра **stdoutLogEnabled** значение `true` и измените путь **stdoutLogFile** так, чтобы он указывал на папку *logs* (например, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="354fa-268">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="354fa-269">В этом пути `stdout` обозначает префикс имени для файла журнала.</span><span class="sxs-lookup"><span data-stu-id="354fa-269">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="354fa-270">При создании файла журнала автоматически добавляются метка времени, идентификатор процесса и расширение файла.</span><span class="sxs-lookup"><span data-stu-id="354fa-270">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="354fa-271">Если указан префикс файла `stdout`, стандартный файл журнала будет иметь примерно такое имя: *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="354fa-271">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="354fa-272">Убедитесь, что удостоверение пула приложений имеет разрешения на запись в папку *logs*.</span><span class="sxs-lookup"><span data-stu-id="354fa-272">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="354fa-273">Сохраните обновленный файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="354fa-273">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="354fa-274">Сделайте запрос к приложению.</span><span class="sxs-lookup"><span data-stu-id="354fa-274">Make a request to the app.</span></span>
1. <span data-ttu-id="354fa-275">Перейдите в папку *logs*.</span><span class="sxs-lookup"><span data-stu-id="354fa-275">Navigate to the *logs* folder.</span></span> <span data-ttu-id="354fa-276">Найдите и откройте последний журнал вывода stdout.</span><span class="sxs-lookup"><span data-stu-id="354fa-276">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="354fa-277">Проверьте, нет ли в нем ошибок.</span><span class="sxs-lookup"><span data-stu-id="354fa-277">Study the log for errors.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="354fa-278">Завершив устранение неполадок, отключите ведение журнала stdout.</span><span class="sxs-lookup"><span data-stu-id="354fa-278">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="354fa-279">Измените файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="354fa-279">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="354fa-280">Задайте параметру **stdoutLogEnabled** значение `false`.</span><span class="sxs-lookup"><span data-stu-id="354fa-280">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="354fa-281">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="354fa-281">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="354fa-282">Ошибка при отключении журнала stdout может привести к сбоям приложения или сервера.</span><span class="sxs-lookup"><span data-stu-id="354fa-282">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="354fa-283">Ни размер файла журнала, ни количество создаваемых файлов журналов ничем не ограничены.</span><span class="sxs-lookup"><span data-stu-id="354fa-283">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="354fa-284">Для обычного ведения журналов для приложения ASP.NET Core используйте библиотеку, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов.</span><span class="sxs-lookup"><span data-stu-id="354fa-284">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="354fa-285">Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="354fa-285">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enable-the-developer-exception-page"></a><span data-ttu-id="354fa-286">Включение страницы исключений для разработчика</span><span class="sxs-lookup"><span data-stu-id="354fa-286">Enable the Developer Exception Page</span></span>

<span data-ttu-id="354fa-287">Можно добавить переменную среды `ASPNETCORE_ENVIRONMENT` [в файл web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables), чтобы запустить приложение в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="354fa-287">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="354fa-288">Если среда не переопределяется при запуске приложения использованием `UseEnvironment` в конструкторе узла, эта переменная среды позволяет отображать [страницу исключения для разработчика](xref:fundamentals/error-handling) при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="354fa-288">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="354fa-289">Настройка переменной среды `ASPNETCORE_ENVIRONMENT` рекомендуется только на промежуточных и тестовых серверах, доступ к которым из Интернета закрыт.</span><span class="sxs-lookup"><span data-stu-id="354fa-289">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="354fa-290">Завершив устранение неполадок, удалите эту переменную среды из файла *web.config*.</span><span class="sxs-lookup"><span data-stu-id="354fa-290">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="354fa-291">Сведения о настройке переменных среды в файле *web.config* см. в статье о [дочернем элементе environmentVariables в aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="354fa-291">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="354fa-292">Стандартные ошибки запуска</span><span class="sxs-lookup"><span data-stu-id="354fa-292">Common startup errors</span></span>

<span data-ttu-id="354fa-293">См. раздел <xref:host-and-deploy/azure-iis-errors-reference>.</span><span class="sxs-lookup"><span data-stu-id="354fa-293">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="354fa-294">В этой статье рассматривается большинство распространенных ошибок, препятствующих запуску приложений.</span><span class="sxs-lookup"><span data-stu-id="354fa-294">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="354fa-295">Получение данных из приложения</span><span class="sxs-lookup"><span data-stu-id="354fa-295">Obtain data from an app</span></span>

<span data-ttu-id="354fa-296">Если приложение способно отвечать на запросы, получите данные о запросе, подключении и дополнительные данные из приложений с помощью встроенного терминала ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="354fa-296">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="354fa-297">Дополнительные сведения и примеры с кодом см. здесь: <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="354fa-297">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

## <a name="create-a-dump"></a><span data-ttu-id="354fa-298">Создание дампа</span><span class="sxs-lookup"><span data-stu-id="354fa-298">Create a dump</span></span>

<span data-ttu-id="354fa-299">*Дамп* представляет собой моментальный снимок системной памяти и может помочь определить причину аварийного завершения, сбоя запуска или медленной работы приложения.</span><span class="sxs-lookup"><span data-stu-id="354fa-299">A *dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="354fa-300">Аварийное завершение работы приложения или исключение</span><span class="sxs-lookup"><span data-stu-id="354fa-300">App crashes or encounters an exception</span></span>

<span data-ttu-id="354fa-301">Получите и проанализируйте дамп из [отчетов об ошибках Windows (WER)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="354fa-301">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="354fa-302">Создайте папку для хранения файлов аварийного дампа в `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="354fa-302">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="354fa-303">Пул приложений должен иметь доступ на запись к папке.</span><span class="sxs-lookup"><span data-stu-id="354fa-303">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="354fa-304">Запустите [скрипт PowerShell EnableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="354fa-304">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="354fa-305">Если приложение использует [модель размещения в процессе](xref:host-and-deploy/iis/index#in-process-hosting-model), выполните скрипт для *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="354fa-305">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="354fa-306">Если приложение использует [модель размещения вне процесса](xref:host-and-deploy/iis/index#out-of-process-hosting-model), выполните скрипт для *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="354fa-306">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="354fa-307">Запустите приложение в условиях, вызывающих аварийное завершение.</span><span class="sxs-lookup"><span data-stu-id="354fa-307">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="354fa-308">После аварийного завершения запустите [скрипт PowerShell DisableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="354fa-308">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="354fa-309">Если приложение использует [модель размещения в процессе](xref:host-and-deploy/iis/index#in-process-hosting-model), выполните скрипт для *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="354fa-309">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="354fa-310">Если приложение использует [модель размещения вне процесса](xref:host-and-deploy/iis/index#out-of-process-hosting-model), выполните скрипт для *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="354fa-310">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="354fa-311">Когда приложение аварийно завершит работу и сбор дампов будет выполнен, приложение сможет завершить работу обычным образом.</span><span class="sxs-lookup"><span data-stu-id="354fa-311">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="354fa-312">Скрипт PowerShell настраивает отчеты об ошибках Windows для сбора до пяти дампов для приложения.</span><span class="sxs-lookup"><span data-stu-id="354fa-312">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="354fa-313">Аварийные дампы могут занимать много места на диске (до нескольких гигабайтов каждый).</span><span class="sxs-lookup"><span data-stu-id="354fa-313">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="354fa-314">Приложение перестает отвечать на запросы, не запускается или работает в обычном режиме</span><span class="sxs-lookup"><span data-stu-id="354fa-314">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="354fa-315">Когда приложение *перестает отвечать на запросы* (но аварийное завершение не происходит), не запускается или работает в обычном режиме, см. раздел [Файлы дампа пользовательского режима: выбор лучшего инструмента](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool), чтобы выбрать подходящий инструмент для создания дампа.</span><span class="sxs-lookup"><span data-stu-id="354fa-315">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

### <a name="analyze-the-dump"></a><span data-ttu-id="354fa-316">Анализ дампа</span><span class="sxs-lookup"><span data-stu-id="354fa-316">Analyze the dump</span></span>

<span data-ttu-id="354fa-317">Дамп можно проанализировать несколькими способами.</span><span class="sxs-lookup"><span data-stu-id="354fa-317">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="354fa-318">Дополнительные сведения см. в разделе [Анализ файла дампа пользовательского режима](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="354fa-318">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="354fa-319">Удаленная отладка</span><span class="sxs-lookup"><span data-stu-id="354fa-319">Remote debugging</span></span>

<span data-ttu-id="354fa-320">Изучите раздел документации по Visual Studio [об удаленной отладке ASP.NET Core на удаленном компьютере IIS в Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer).</span><span class="sxs-lookup"><span data-stu-id="354fa-320">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="354fa-321">Application Insights</span><span class="sxs-lookup"><span data-stu-id="354fa-321">Application Insights</span></span>

<span data-ttu-id="354fa-322">[Application Insights](/azure/application-insights/) предоставляет данные телеметрии из приложений, размещенных в IIS, в том числе возможности регистрации ошибок и создания отчетов.</span><span class="sxs-lookup"><span data-stu-id="354fa-322">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="354fa-323">Application Insights может сообщать только об ошибках, возникающих после запуска приложения, когда становятся доступными функции ведения журнала приложения.</span><span class="sxs-lookup"><span data-stu-id="354fa-323">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="354fa-324">Дополнительные сведения см. в статье [Application Insights для ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="354fa-324">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-advice"></a><span data-ttu-id="354fa-325">Дополнительные советы</span><span class="sxs-lookup"><span data-stu-id="354fa-325">Additional advice</span></span>

<span data-ttu-id="354fa-326">Иногда приложения-функции перестают работать сразу после обновления пакета SDK для .NET Core на компьютере разработки или обновления пакетов в самом приложении.</span><span class="sxs-lookup"><span data-stu-id="354fa-326">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="354fa-327">В некоторых случаях в результате важного обновления несогласованные версии пакетов могут привести к нарушению работы приложения.</span><span class="sxs-lookup"><span data-stu-id="354fa-327">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="354fa-328">Большинство этих проблем можно исправить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="354fa-328">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="354fa-329">Удалите папки *bin* и *obj*.</span><span class="sxs-lookup"><span data-stu-id="354fa-329">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="354fa-330">Очистите кэши пакетов, размещенные в папках *%UserProfile%\\.nuget\\packages* и *%LocalAppData%\\Nuget\\v3-cache*.</span><span class="sxs-lookup"><span data-stu-id="354fa-330">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="354fa-331">Восстановите и перестройте проект.</span><span class="sxs-lookup"><span data-stu-id="354fa-331">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="354fa-332">Убедитесь, что предыдущее развертывание на сервере полностью удалено, прежде чем развертывать его снова.</span><span class="sxs-lookup"><span data-stu-id="354fa-332">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="354fa-333">Очистить кэши пакета проще всего командой `dotnet nuget locals all --clear` из командной строки.</span><span class="sxs-lookup"><span data-stu-id="354fa-333">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
>
> <span data-ttu-id="354fa-334">Также очистку кэшей пакетов можно выполнить средством [nuget.exe](https://www.nuget.org/downloads) или командой `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="354fa-334">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="354fa-335">*NuGet.exe* не входит в пакет установки операционной системы Windows для настольных компьютеров и его нужно получить отдельно на [веб-сайте NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="354fa-335">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="354fa-336">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="354fa-336">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
