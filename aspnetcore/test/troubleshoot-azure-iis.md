---
title: Устранение неполадок ASP.NET Core в службе приложений Azure и службах IIS
author: guardrex
description: Узнайте, как диагностировать проблемы со службами приложений Azure и развертываниями службы IIS (IIS) ASP.NET Core приложений.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2019
uid: test/troubleshoot-azure-iis
ms.openlocfilehash: 46d4a11c594844e059fa8555255d7321f7b48631
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308797"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service-and-iis"></a><span data-ttu-id="3479c-103">Устранение неполадок ASP.NET Core в службе приложений Azure и службах IIS</span><span class="sxs-lookup"><span data-stu-id="3479c-103">Troubleshoot ASP.NET Core on Azure App Service and IIS</span></span>

<span data-ttu-id="3479c-104">[Люк ЛаСаМ](https://github.com/guardrex) и [Джастин коталик](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="3479c-104">By [Luke Latham](https://github.com/guardrex) and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="3479c-105">В этой статье содержатся сведения об общих ошибках запуска приложений и инструкции по диагностике ошибок при развертывании приложения в службе приложений Azure или службах IIS.</span><span class="sxs-lookup"><span data-stu-id="3479c-105">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="3479c-106">Ошибки запуска приложений</span><span class="sxs-lookup"><span data-stu-id="3479c-106">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="3479c-107">Описание распространенных сценариев кода состояния HTTP для запуска.</span><span class="sxs-lookup"><span data-stu-id="3479c-107">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="3479c-108">Устранение неполадок в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="3479c-108">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="3479c-109">Рекомендации по устранению неполадок для приложений, развернутых в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="3479c-109">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="3479c-110">Устранение неполадок в IIS</span><span class="sxs-lookup"><span data-stu-id="3479c-110">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="3479c-111">Рекомендации по устранению неполадок приложений, развернутых в службах IIS или запущенных на IIS Express локально.</span><span class="sxs-lookup"><span data-stu-id="3479c-111">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="3479c-112">Руководство относится к развертываниям Windows Server и Windows Desktop.</span><span class="sxs-lookup"><span data-stu-id="3479c-112">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="3479c-113">Очистить кэши пакетов</span><span class="sxs-lookup"><span data-stu-id="3479c-113">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="3479c-114">Объясняет, что делать, когда несогласованные пакеты нарушают работу приложения при выполнении основных обновлений или изменении версий пакета.</span><span class="sxs-lookup"><span data-stu-id="3479c-114">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="3479c-115">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="3479c-115">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="3479c-116">Список дополнительных разделов по устранению неполадок.</span><span class="sxs-lookup"><span data-stu-id="3479c-116">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="3479c-117">Ошибки при запуске приложения</span><span class="sxs-lookup"><span data-stu-id="3479c-117">App startup errors</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="3479c-118">В Visual Studio проект ASP.NET Core по умолчанию использует размещение в [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) на период отладки.</span><span class="sxs-lookup"><span data-stu-id="3479c-118">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="3479c-119">Ошибка в *502,5-процессе* или *500,30-запуск* , возникающая при локальной отладке для диагностики с помощью инструкции в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="3479c-119">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="3479c-120">В Visual Studio проект ASP.NET Core по умолчанию использует размещение в [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) на период отладки.</span><span class="sxs-lookup"><span data-stu-id="3479c-120">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="3479c-121">*Ошибка процесса 502,5* , возникающая при локальной отладке, может быть выявлена с помощью Совета в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="3479c-121">A *502.5 Process Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

### <a name="500-internal-server-error"></a><span data-ttu-id="3479c-122">500 Internal Server Error (внутренняя ошибка сервера)</span><span class="sxs-lookup"><span data-stu-id="3479c-122">500 Internal Server Error</span></span>

<span data-ttu-id="3479c-123">Приложение запускается, но ошибка не позволяет серверу выполнить запрос.</span><span class="sxs-lookup"><span data-stu-id="3479c-123">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="3479c-124">Эта ошибка возникает в коде приложения при запуске или при создании ответа.</span><span class="sxs-lookup"><span data-stu-id="3479c-124">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="3479c-125">Ответ может не иметь содержимого или ответ в браузере может выглядеть как *500 — внутренняя ошибка сервера*.</span><span class="sxs-lookup"><span data-stu-id="3479c-125">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="3479c-126">Как правило, в журнале событий приложения указывается, что приложение запускается в обычном режиме.</span><span class="sxs-lookup"><span data-stu-id="3479c-126">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="3479c-127">Сервер действует правильно.</span><span class="sxs-lookup"><span data-stu-id="3479c-127">From the server's perspective, that's correct.</span></span> <span data-ttu-id="3479c-128">Приложение запустилось, но оно не может генерировать действительный ответ.</span><span class="sxs-lookup"><span data-stu-id="3479c-128">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="3479c-129">Запустите приложение из командной строки на сервере или включите журнал ASP.NET Core модуля stdout, чтобы устранить проблему.</span><span class="sxs-lookup"><span data-stu-id="3479c-129">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

::: moniker range="= aspnetcore-2.2"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="3479c-130">500.0 In-Process Handler Load Failure (ошибка загрузки внутрипроцессного обработчика)</span><span class="sxs-lookup"><span data-stu-id="3479c-130">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="3479c-131">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="3479c-131">The worker process fails.</span></span> <span data-ttu-id="3479c-132">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="3479c-132">The app doesn't start.</span></span>

<span data-ttu-id="3479c-133">[Модулю ASP.NET Core](xref:host-and-deploy/aspnet-core-module) не удается найти среду CLR .NET Core и найти внутрипроцессный обработчик запросов (*aspnetcorev2_inprocess. dll*).</span><span class="sxs-lookup"><span data-stu-id="3479c-133">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="3479c-134">Проверьте следующие моменты:</span><span class="sxs-lookup"><span data-stu-id="3479c-134">Check that:</span></span>

* <span data-ttu-id="3479c-135">приложение предназначено для пакета NuGet [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) или [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app);</span><span class="sxs-lookup"><span data-stu-id="3479c-135">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="3479c-136">версия общей платформы ASP.NET Core, для которой предназначено приложение, установлена на целевом компьютере.</span><span class="sxs-lookup"><span data-stu-id="3479c-136">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="3479c-137">500.0 Out-Of-Process Handler Load Failure (ошибка загрузки внепроцессного обработчика)</span><span class="sxs-lookup"><span data-stu-id="3479c-137">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="3479c-138">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="3479c-138">The worker process fails.</span></span> <span data-ttu-id="3479c-139">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="3479c-139">The app doesn't start.</span></span>

<span data-ttu-id="3479c-140">[Модулю ASP.NET Core](xref:host-and-deploy/aspnet-core-module) не удается найти обработчик запросов внутрипроцессного размещения.</span><span class="sxs-lookup"><span data-stu-id="3479c-140">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="3479c-141">Убедитесь, что *aspnetcorev2_outofprocess.dll* находится во вложенной папке рядом с *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="3479c-141">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="3479c-142">500.0 In-Process Handler Load Failure (ошибка загрузки внутрипроцессного обработчика)</span><span class="sxs-lookup"><span data-stu-id="3479c-142">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="3479c-143">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="3479c-143">The worker process fails.</span></span> <span data-ttu-id="3479c-144">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="3479c-144">The app doesn't start.</span></span>

<span data-ttu-id="3479c-145">При загрузке компонентов [модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module) произошла неизвестная ошибка.</span><span class="sxs-lookup"><span data-stu-id="3479c-145">An unknown error occurred loading [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) components.</span></span> <span data-ttu-id="3479c-146">Выполните одно из указанных ниже действий.</span><span class="sxs-lookup"><span data-stu-id="3479c-146">Take one of the following actions:</span></span>

* <span data-ttu-id="3479c-147">Обратитесь в [службу поддержки Майкрософт](https://support.microsoft.com/oas/default.aspx?prid=15832) (выберите **Средства разработчика**, а затем — **ASP.NET Core**).</span><span class="sxs-lookup"><span data-stu-id="3479c-147">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="3479c-148">Задайте вопрос на сайте Stack Overflow.</span><span class="sxs-lookup"><span data-stu-id="3479c-148">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="3479c-149">Сообщите о проблеме в нашем [репозитории GitHub](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="3479c-149">File an issue on our [GitHub repository](https://github.com/aspnet/AspNetCore).</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="3479c-150">500.30 In-Process Startup Failure (ошибка внутрипроцессного запуска)</span><span class="sxs-lookup"><span data-stu-id="3479c-150">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="3479c-151">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="3479c-151">The worker process fails.</span></span> <span data-ttu-id="3479c-152">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="3479c-152">The app doesn't start.</span></span>

<span data-ttu-id="3479c-153">[Модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module) пытается запустить среду CLR .NET Core в процессе, но не запускается.</span><span class="sxs-lookup"><span data-stu-id="3479c-153">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="3479c-154">Причину сбоя запуска процесса обычно можно определить на основе записей в журнале событий приложений и в журнале stdout модуля ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3479c-154">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="3479c-155">Распространенной причиной сбоя является неправильная настройка приложения с выбором отсутствующей версии общей платформы ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3479c-155">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="3479c-156">Проверьте, какие версии общей платформы ASP.NET Core установлены на целевом компьютере.</span><span class="sxs-lookup"><span data-stu-id="3479c-156">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="3479c-157">500.31 ANCM Failed to Find Native Dependencies (ANCM не удалось найти собственные зависимости)</span><span class="sxs-lookup"><span data-stu-id="3479c-157">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="3479c-158">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="3479c-158">The worker process fails.</span></span> <span data-ttu-id="3479c-159">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="3479c-159">The app doesn't start.</span></span>

<span data-ttu-id="3479c-160">[Модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module) пытается запустить среду выполнения .NET Core в процессе, но не запускается.</span><span class="sxs-lookup"><span data-stu-id="3479c-160">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="3479c-161">Наиболее распространенная причина этой ошибки в том, что среда выполнения `Microsoft.NETCore.App` или `Microsoft.AspNetCore.App` не установлена.</span><span class="sxs-lookup"><span data-stu-id="3479c-161">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="3479c-162">Если целевой платформой развернутого приложения является ASP.NET Core 3.0, но эта версия отсутствует на компьютере, происходит данная ошибка.</span><span class="sxs-lookup"><span data-stu-id="3479c-162">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="3479c-163">Пример сообщения об ошибке:</span><span class="sxs-lookup"><span data-stu-id="3479c-163">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="3479c-164">В сообщении об ошибке перечислены все установленные версии .NET Core и указывается версия, которая требуется приложению.</span><span class="sxs-lookup"><span data-stu-id="3479c-164">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="3479c-165">Чтобы устранить эту ошибку, выполните одно из указанных ниже действий.</span><span class="sxs-lookup"><span data-stu-id="3479c-165">To fix this error, either:</span></span>

* <span data-ttu-id="3479c-166">Установите соответствующую версию .NET Core на компьютере.</span><span class="sxs-lookup"><span data-stu-id="3479c-166">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="3479c-167">Измените целевую версию .NET Core приложения на версию, имеющуюся на компьютере.</span><span class="sxs-lookup"><span data-stu-id="3479c-167">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="3479c-168">Опубликуйте приложение как [автономное развертывание](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="3479c-168">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="3479c-169">При запуске в среде разработки (переменная среды `ASPNETCORE_ENVIRONMENT` имеет значение `Development`) ошибка записывается в ответ HTTP.</span><span class="sxs-lookup"><span data-stu-id="3479c-169">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="3479c-170">Причина сбоя запуска процесса также находится в журнале событий приложений.</span><span class="sxs-lookup"><span data-stu-id="3479c-170">The cause of a process startup failure is also found in the Application Event Log.</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="3479c-171">500.32 ANCM Failed to Load dll (ANCM не удалось загрузить библиотеку DLL)</span><span class="sxs-lookup"><span data-stu-id="3479c-171">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="3479c-172">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="3479c-172">The worker process fails.</span></span> <span data-ttu-id="3479c-173">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="3479c-173">The app doesn't start.</span></span>

<span data-ttu-id="3479c-174">Наиболее распространенная причина этой ошибки в том, что приложение опубликовано для несовместимой архитектуры процессора.</span><span class="sxs-lookup"><span data-stu-id="3479c-174">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="3479c-175">Если рабочий процесс выполняется как 32-разрядное приложение, но приложение было опубликовано для 64-разрядной архитектуры, происходит эта ошибка.</span><span class="sxs-lookup"><span data-stu-id="3479c-175">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="3479c-176">Чтобы устранить эту ошибку, выполните одно из указанных ниже действий.</span><span class="sxs-lookup"><span data-stu-id="3479c-176">To fix this error, either:</span></span>

* <span data-ttu-id="3479c-177">Повторно опубликуйте приложение для той же архитектуры процессора, для которой предназначен рабочий процесс.</span><span class="sxs-lookup"><span data-stu-id="3479c-177">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="3479c-178">Опубликуйте приложение как [зависящее от платформы развертывание](/dotnet/core/deploying/#framework-dependent-executables-fde).</span><span class="sxs-lookup"><span data-stu-id="3479c-178">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="3479c-179">500.33 ANCM Request Handler Load Failure (Ошибка загрузки обработчика запросов ANCM)</span><span class="sxs-lookup"><span data-stu-id="3479c-179">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="3479c-180">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="3479c-180">The worker process fails.</span></span> <span data-ttu-id="3479c-181">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="3479c-181">The app doesn't start.</span></span>

<span data-ttu-id="3479c-182">Приложение не ссылается на платформу `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="3479c-182">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="3479c-183">Только приложения, предназначенные `Microsoft.AspNetCore.App` для платформы, могут размещаться в [модуле ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="3479c-183">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="3479c-184">Для устранения этой ошибки убедитесь в том, что приложение предназначено для платформы `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="3479c-184">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="3479c-185">В файле `.runtimeconfig.json` проверьте, является ли эта платформа целевой для приложения.</span><span class="sxs-lookup"><span data-stu-id="3479c-185">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="3479c-186">500.34 ANCM Mixed Hosting Models Not Supported (Смешанные модели размещения ANCM не поддерживаются)</span><span class="sxs-lookup"><span data-stu-id="3479c-186">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="3479c-187">В одном рабочем процессе не могут выполняться одновременно внутрипроцессное приложение и внепроцессное приложение.</span><span class="sxs-lookup"><span data-stu-id="3479c-187">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="3479c-188">Для устранения этой ошибки запускайте приложения в отдельных пулах приложений IIS.</span><span class="sxs-lookup"><span data-stu-id="3479c-188">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="3479c-189">500.35 ANCM Multiple In-Process Applications in same Process (Несколько внутрипроцессных приложений ANCM в одном процессе)</span><span class="sxs-lookup"><span data-stu-id="3479c-189">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="3479c-190">В одном рабочем процессе не могут выполняться одновременно внутрипроцессное приложение и внепроцессное приложение.</span><span class="sxs-lookup"><span data-stu-id="3479c-190">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="3479c-191">Для устранения этой ошибки запускайте приложения в отдельных пулах приложений IIS.</span><span class="sxs-lookup"><span data-stu-id="3479c-191">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="3479c-192">500.36 ANCM Out-Of-Process Handler Load Failure (Ошибка загрузки внепроцессного обработчика ANCM)</span><span class="sxs-lookup"><span data-stu-id="3479c-192">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="3479c-193">Рядом с файлом *aspnetcorev2.dll* нет внепроцессного обработчика запросов *aspnetcorev2_outofprocess.dll*.</span><span class="sxs-lookup"><span data-stu-id="3479c-193">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="3479c-194">Это указывает на поврежденную установку [модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="3479c-194">This indicates a corrupted installation of the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="3479c-195">Для устранения этой ошибки восстановите установку [пакета размещения .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (для служб IIS) или Visual Studio (для IIS Express).</span><span class="sxs-lookup"><span data-stu-id="3479c-195">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="3479c-196">500.37 ANCM Failed to Start Within Startup Time Limit (Модуль ANCM не запустился в течение предельного времени запуска)</span><span class="sxs-lookup"><span data-stu-id="3479c-196">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="3479c-197">Модуль ANCM не запустился в течение заданного предельного времени запуска.</span><span class="sxs-lookup"><span data-stu-id="3479c-197">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="3479c-198">По умолчанию время ожидания составляет 120 секунд.</span><span class="sxs-lookup"><span data-stu-id="3479c-198">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="3479c-199">Эта ошибка может произойти, если на одном компьютере запускается много приложений.</span><span class="sxs-lookup"><span data-stu-id="3479c-199">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="3479c-200">Обратите внимание на пики использования ЦП и памяти на сервере во время запуска.</span><span class="sxs-lookup"><span data-stu-id="3479c-200">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="3479c-201">Возможно, потребуется регулировать процесс запуска нескольких приложений.</span><span class="sxs-lookup"><span data-stu-id="3479c-201">You may need to stagger the startup process of multiple apps.</span></span>

::: moniker-end

### <a name="5025-process-failure"></a><span data-ttu-id="3479c-202">502.5 Process Failure (ошибка процесса)</span><span class="sxs-lookup"><span data-stu-id="3479c-202">502.5 Process Failure</span></span>

<span data-ttu-id="3479c-203">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="3479c-203">The worker process fails.</span></span> <span data-ttu-id="3479c-204">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="3479c-204">The app doesn't start.</span></span>

<span data-ttu-id="3479c-205">[Модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module) пытается запустить рабочий процесс, но он не запускается.</span><span class="sxs-lookup"><span data-stu-id="3479c-205">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="3479c-206">Причину сбоя запуска процесса обычно можно определить на основе записей в журнале событий приложений и в журнале stdout модуля ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3479c-206">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="3479c-207">Распространенной причиной сбоя является неправильная настройка приложения с выбором отсутствующей версии общей платформы ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3479c-207">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="3479c-208">Проверьте, какие версии общей платформы ASP.NET Core установлены на целевом компьютере.</span><span class="sxs-lookup"><span data-stu-id="3479c-208">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="3479c-209">*Общая платформа* — это набор сборок (*DLLl*-файлы), которые установлены на компьютере и на которые ссылается метапакет, например `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="3479c-209">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="3479c-210">В ссылках метапакета может быть указана минимальная требуемая версия.</span><span class="sxs-lookup"><span data-stu-id="3479c-210">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="3479c-211">Дополнительную информацию см. в этой публикации об [общей платформе](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span><span class="sxs-lookup"><span data-stu-id="3479c-211">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="3479c-212">Когда ошибки в конфигурации размещения или приложения приводят к сбою рабочего процесса, возвращается страница ошибки *502.5 — ошибка процесса*.</span><span class="sxs-lookup"><span data-stu-id="3479c-212">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="3479c-213">Не удалось запустить приложение (код ошибки "0x800700c1")</span><span class="sxs-lookup"><span data-stu-id="3479c-213">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="3479c-214">Не удалось запустить приложение, так как не удалось загрузить сборку приложения ( *.dll*).</span><span class="sxs-lookup"><span data-stu-id="3479c-214">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="3479c-215">Эта ошибка возникает, если существует несоответствие разрядности опубликованного приложения и процесса w3wp/iisexpress.</span><span class="sxs-lookup"><span data-stu-id="3479c-215">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="3479c-216">Убедитесь, что параметр 32-разрядности пула приложений указан правильно:</span><span class="sxs-lookup"><span data-stu-id="3479c-216">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="3479c-217">Выберите пул приложений в диспетчере служб IIS в разделе **Пулы приложений**.</span><span class="sxs-lookup"><span data-stu-id="3479c-217">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="3479c-218">Выберите **Дополнительные параметры** в разделе **Изменение пула приложений** панели **Действия**.</span><span class="sxs-lookup"><span data-stu-id="3479c-218">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="3479c-219">Установите параметр **Включить 32-разрядные приложения**:</span><span class="sxs-lookup"><span data-stu-id="3479c-219">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="3479c-220">При развертывании 32-разрядного (x86) приложения задайте значение `True`.</span><span class="sxs-lookup"><span data-stu-id="3479c-220">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="3479c-221">При развертывании 64-разрядного (x64) приложения задайте значение `False`.</span><span class="sxs-lookup"><span data-stu-id="3479c-221">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="3479c-222">Сброс подключения</span><span class="sxs-lookup"><span data-stu-id="3479c-222">Connection reset</span></span>

<span data-ttu-id="3479c-223">Если ошибка возникает после отправки заголовков, сервер уже не может отправить страницу **500 — внутренняя ошибка сервера**.</span><span class="sxs-lookup"><span data-stu-id="3479c-223">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="3479c-224">Это часто происходит, когда ошибка возникает во время сериализации составных объектов ответа.</span><span class="sxs-lookup"><span data-stu-id="3479c-224">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="3479c-225">Этот тип ошибки отображается на стороне клиента как ошибка *Сброс подключения*.</span><span class="sxs-lookup"><span data-stu-id="3479c-225">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="3479c-226">[Ведение журналов для приложений](xref:fundamentals/logging/index) может помочь устранить эти ошибки.</span><span class="sxs-lookup"><span data-stu-id="3479c-226">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="3479c-227">Ограничения по умолчанию при запуске</span><span class="sxs-lookup"><span data-stu-id="3479c-227">Default startup limits</span></span>

<span data-ttu-id="3479c-228">Для [модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module) настроено значение по умолчанию *startupTimeLimit* , равное 120 секундам.</span><span class="sxs-lookup"><span data-stu-id="3479c-228">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="3479c-229">Если оставить значение по умолчанию, то прежде чем журнал модуля зарегистрирует ошибку запуска приложения, может пройти до двух минут.</span><span class="sxs-lookup"><span data-stu-id="3479c-229">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="3479c-230">Сведения о настройке модуля см. в разделе [Атрибуты элемента aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="3479c-230">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="3479c-231">Устранение неполадок в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="3479c-231">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="3479c-232">Журнал событий приложений (служба приложений Azure)</span><span class="sxs-lookup"><span data-stu-id="3479c-232">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="3479c-233">Чтобы получить доступ к журналу событий приложения, используйте колонку **Диагностика и решение проблем** на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="3479c-233">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="3479c-234">На портале Azure откройте приложение в колонке **Службы приложений**.</span><span class="sxs-lookup"><span data-stu-id="3479c-234">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="3479c-235">Выберите колонку **Диагностика и решение проблем**.</span><span class="sxs-lookup"><span data-stu-id="3479c-235">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="3479c-236">Щелкните заголовок **Средства диагностики**.</span><span class="sxs-lookup"><span data-stu-id="3479c-236">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="3479c-237">В разделе **Средства поддержки** нажмите кнопку **События приложений**.</span><span class="sxs-lookup"><span data-stu-id="3479c-237">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="3479c-238">Изучите последние ошибки из журнала *IIS AspNetCoreModule* или *IIS AspNetCoreModule V2* в столбце **Источник**.</span><span class="sxs-lookup"><span data-stu-id="3479c-238">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="3479c-239">Вместо использования колонки **Диагностика и решение проблем** можно изучить файл журнала событий приложения непосредственно с помощью консоли [Kudu](https://github.com/projectkudu/kudu/wiki).</span><span class="sxs-lookup"><span data-stu-id="3479c-239">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="3479c-240">В области **Средства разработки** откройте колонку **Дополнительные инструменты**.</span><span class="sxs-lookup"><span data-stu-id="3479c-240">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="3479c-241">Нажмите кнопку **Перейти&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="3479c-241">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="3479c-242">Консоль Kudu открывается в новой вкладке или в окне браузера.</span><span class="sxs-lookup"><span data-stu-id="3479c-242">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="3479c-243">С помощью панели навигации в верхней части страницы откройте **Консоль отладки** и выберите **CMD**.</span><span class="sxs-lookup"><span data-stu-id="3479c-243">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="3479c-244">Откройте папку **LogFiles**.</span><span class="sxs-lookup"><span data-stu-id="3479c-244">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="3479c-245">Выберите значок с карандашом рядом с файлом *eventlog.xml*.</span><span class="sxs-lookup"><span data-stu-id="3479c-245">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="3479c-246">Изучите журнал.</span><span class="sxs-lookup"><span data-stu-id="3479c-246">Examine the log.</span></span> <span data-ttu-id="3479c-247">Прокрутите список до нижней части журнала, чтобы просмотреть последние события.</span><span class="sxs-lookup"><span data-stu-id="3479c-247">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="3479c-248">Запуск приложения в консоли Kudu</span><span class="sxs-lookup"><span data-stu-id="3479c-248">Run the app in the Kudu console</span></span>

<span data-ttu-id="3479c-249">Многие ошибки запуска не создают полезные сведения в журнале событий приложения.</span><span class="sxs-lookup"><span data-stu-id="3479c-249">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="3479c-250">Чтобы обнаружить ошибку, можно запустить приложение в удаленной консоли выполнения [Kudu](https://github.com/projectkudu/kudu/wiki).</span><span class="sxs-lookup"><span data-stu-id="3479c-250">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="3479c-251">В области **Средства разработки** откройте колонку **Дополнительные инструменты**.</span><span class="sxs-lookup"><span data-stu-id="3479c-251">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="3479c-252">Нажмите кнопку **Перейти&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="3479c-252">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="3479c-253">Консоль Kudu открывается в новой вкладке или в окне браузера.</span><span class="sxs-lookup"><span data-stu-id="3479c-253">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="3479c-254">С помощью панели навигации в верхней части страницы откройте **Консоль отладки** и выберите **CMD**.</span><span class="sxs-lookup"><span data-stu-id="3479c-254">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="3479c-255">Тестирование 32-разрядного (x86) приложения</span><span class="sxs-lookup"><span data-stu-id="3479c-255">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="3479c-256">**Текущий выпуск**</span><span class="sxs-lookup"><span data-stu-id="3479c-256">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="3479c-257">Запустите приложение:</span><span class="sxs-lookup"><span data-stu-id="3479c-257">Run the app:</span></span>
   * <span data-ttu-id="3479c-258">Если развертываемое приложение [зависит от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="3479c-258">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```console
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="3479c-259">Если приложение [развертывается автономно](/dotnet/core/deploying/#self-contained-deployments-scd), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="3479c-259">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="3479c-260">Выходные данные из приложения, отображающие любые ошибки, будут выведены на консоль Kudu.</span><span class="sxs-lookup"><span data-stu-id="3479c-260">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="3479c-261">**Развертывание, зависимое от платформы, выполняющееся в предварительной версии**</span><span class="sxs-lookup"><span data-stu-id="3479c-261">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="3479c-262">*Требует установки расширения сайта для среды выполнения ASP.NET Core {VERSION} (x86).*</span><span class="sxs-lookup"><span data-stu-id="3479c-262">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="3479c-263">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` — это версия среды выполнения).</span><span class="sxs-lookup"><span data-stu-id="3479c-263">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="3479c-264">Запустите приложение: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`.</span><span class="sxs-lookup"><span data-stu-id="3479c-264">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="3479c-265">Выходные данные из приложения, отображающие любые ошибки, будут выведены на консоль Kudu.</span><span class="sxs-lookup"><span data-stu-id="3479c-265">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="3479c-266">Тестирование 64-разрядного (x64) приложения</span><span class="sxs-lookup"><span data-stu-id="3479c-266">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="3479c-267">**Текущий выпуск**</span><span class="sxs-lookup"><span data-stu-id="3479c-267">**Current release**</span></span>

* <span data-ttu-id="3479c-268">Если приложение является 64-разрядным (x64) [зависимым от платформы развертыванием](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="3479c-268">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="3479c-269">Запустите приложение: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`.</span><span class="sxs-lookup"><span data-stu-id="3479c-269">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="3479c-270">Если приложение [развертывается автономно](/dotnet/core/deploying/#self-contained-deployments-scd), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="3479c-270">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="3479c-271">Запустите приложение: `{ASSEMBLY NAME}.exe`.</span><span class="sxs-lookup"><span data-stu-id="3479c-271">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="3479c-272">Выходные данные из приложения, отображающие любые ошибки, будут выведены на консоль Kudu.</span><span class="sxs-lookup"><span data-stu-id="3479c-272">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="3479c-273">**Развертывание, зависимое от платформы, выполняющееся в предварительной версии**</span><span class="sxs-lookup"><span data-stu-id="3479c-273">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="3479c-274">*Требует установки расширения сайта для среды выполнения ASP.NET Core {VERSION} (x64).*</span><span class="sxs-lookup"><span data-stu-id="3479c-274">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="3479c-275">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` — это версия среды выполнения).</span><span class="sxs-lookup"><span data-stu-id="3479c-275">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="3479c-276">Запустите приложение: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`.</span><span class="sxs-lookup"><span data-stu-id="3479c-276">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="3479c-277">Выходные данные из приложения, отображающие любые ошибки, будут выведены на консоль Kudu.</span><span class="sxs-lookup"><span data-stu-id="3479c-277">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="3479c-278">Журнал ASP.NET Core модуля stdout (служба приложений Azure)</span><span class="sxs-lookup"><span data-stu-id="3479c-278">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="3479c-279">В журнале stdout модуля ASP.NET Core часто записываются полезные сообщения, которые не удается найти в журнале событий приложения.</span><span class="sxs-lookup"><span data-stu-id="3479c-279">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="3479c-280">Чтобы включить и просмотреть журналы stdout выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="3479c-280">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="3479c-281">Перейдите к колонке **Диагностика и решение проблем** на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="3479c-281">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="3479c-282">В разделе **Выберите категорию проблемы** нажмите кнопку **Веб-приложение не работает**.</span><span class="sxs-lookup"><span data-stu-id="3479c-282">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="3479c-283">В разделе **Предлагаемые решения** > **Включить перенаправление журнала stdout** нажмите кнопку **Открыть консоль Kudu для редактирования файла Web.Config**.</span><span class="sxs-lookup"><span data-stu-id="3479c-283">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="3479c-284">В **консоли диагностики** Kudu откройте папки на пути **веб-сайт** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="3479c-284">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="3479c-285">Прокрутите список вниз, чтобы в его нижней части показался файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="3479c-285">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="3479c-286">Щелкните значок карандаша рядом с файлом *web.config*.</span><span class="sxs-lookup"><span data-stu-id="3479c-286">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="3479c-287">Установите для параметра **stdoutLogEnabled** значение `true` и измените путь **stdoutLogFile** на `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="3479c-287">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="3479c-288">Выберите **Сохранить** для сохранения обновленного файла *web.config*.</span><span class="sxs-lookup"><span data-stu-id="3479c-288">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="3479c-289">Сделайте запрос к приложению.</span><span class="sxs-lookup"><span data-stu-id="3479c-289">Make a request to the app.</span></span>
1. <span data-ttu-id="3479c-290">Вернитесь на портал Azure.</span><span class="sxs-lookup"><span data-stu-id="3479c-290">Return to the Azure portal.</span></span> <span data-ttu-id="3479c-291">Выберите колонку **Дополнительные инструменты** в области **Средства разработки**.</span><span class="sxs-lookup"><span data-stu-id="3479c-291">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="3479c-292">Нажмите кнопку **Перейти&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="3479c-292">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="3479c-293">Консоль Kudu открывается в новой вкладке или в окне браузера.</span><span class="sxs-lookup"><span data-stu-id="3479c-293">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="3479c-294">С помощью панели навигации в верхней части страницы откройте **Консоль отладки** и выберите **CMD**.</span><span class="sxs-lookup"><span data-stu-id="3479c-294">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="3479c-295">Выберите папку **LogFiles**.</span><span class="sxs-lookup"><span data-stu-id="3479c-295">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="3479c-296">Изучите столбец **Изменено** и выберите значок карандаша, чтобы отредактировать журнал stdout для последней даты изменения.</span><span class="sxs-lookup"><span data-stu-id="3479c-296">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="3479c-297">При открытии файла журнала отображается сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="3479c-297">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="3479c-298">Завершив устранение неполадок, отключите ведение журнала stdout.</span><span class="sxs-lookup"><span data-stu-id="3479c-298">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="3479c-299">В **консоли диагностики** Kudu вернитесь к пути **веб-сайт** > **wwwroot**, чтобы открыть файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="3479c-299">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="3479c-300">Выбрав значок карандаша, снова откройте файл **web.config**.</span><span class="sxs-lookup"><span data-stu-id="3479c-300">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="3479c-301">Задайте параметру **stdoutLogEnabled** значение `false`.</span><span class="sxs-lookup"><span data-stu-id="3479c-301">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="3479c-302">Нажмите кнопку **Сохранить** для сохранения файла.</span><span class="sxs-lookup"><span data-stu-id="3479c-302">Select **Save** to save the file.</span></span>

<span data-ttu-id="3479c-303">Дополнительные сведения см. в разделе <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="3479c-303">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="3479c-304">Ошибка при отключении журнала stdout может привести к сбоям приложения или сервера.</span><span class="sxs-lookup"><span data-stu-id="3479c-304">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="3479c-305">Ни размер файла журнала, ни количество создаваемых файлов журналов ничем не ограничены.</span><span class="sxs-lookup"><span data-stu-id="3479c-305">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="3479c-306">Для устранения неполадок при запуске приложения используйте только журнал stdout.</span><span class="sxs-lookup"><span data-stu-id="3479c-306">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="3479c-307">После запуска в приложении ASP.NET Core для ведения общего журнала используйте библиотеку ведения журналов, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов.</span><span class="sxs-lookup"><span data-stu-id="3479c-307">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="3479c-308">Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="3479c-308">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-azure-app-service"></a><span data-ttu-id="3479c-309">Журнал отладки модуля ASP.NET Core (служба приложений Azure)</span><span class="sxs-lookup"><span data-stu-id="3479c-309">ASP.NET Core Module debug log (Azure App Service)</span></span>

<span data-ttu-id="3479c-310">В журнал отладки модуля ASP.NET записываются дополнительные и более подробные сообщения, относящиеся к модулю ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3479c-310">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="3479c-311">Чтобы включить и просмотреть журналы stdout, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="3479c-311">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="3479c-312">Чтобы включить ведение расширенного журнала диагностики, выполните одно из следующих действий.</span><span class="sxs-lookup"><span data-stu-id="3479c-312">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="3479c-313">Следуйте инструкциям по настройке приложения для ведения расширенных журналов диагностики, приведенным в разделе [Расширенные журналы диагностики](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span><span class="sxs-lookup"><span data-stu-id="3479c-313">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="3479c-314">Повторно разверните приложение.</span><span class="sxs-lookup"><span data-stu-id="3479c-314">Redeploy the app.</span></span>
   * <span data-ttu-id="3479c-315">С помощью консоли Kudu добавьте раздел `<handlerSettings>` в файл *web.config* приложения, как показано в разделе [Расширенные журналы диагностики](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span><span class="sxs-lookup"><span data-stu-id="3479c-315">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="3479c-316">В области **Средства разработки** откройте колонку **Дополнительные инструменты**.</span><span class="sxs-lookup"><span data-stu-id="3479c-316">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="3479c-317">Нажмите кнопку **Перейти&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="3479c-317">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="3479c-318">Консоль Kudu открывается в новой вкладке или в окне браузера.</span><span class="sxs-lookup"><span data-stu-id="3479c-318">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="3479c-319">С помощью панели навигации в верхней части страницы откройте **Консоль отладки** и выберите **CMD**.</span><span class="sxs-lookup"><span data-stu-id="3479c-319">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="3479c-320">Откройте папки на пути **веб-сайт** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="3479c-320">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="3479c-321">Нажмите кнопку с изображением карандаша и внесите изменения в файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="3479c-321">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="3479c-322">Добавьте раздел `<handlerSettings>`, как описано в разделе [Расширенные журналы диагностики](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span><span class="sxs-lookup"><span data-stu-id="3479c-322">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="3479c-323">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="3479c-323">Select the **Save** button.</span></span>
1. <span data-ttu-id="3479c-324">В области **Средства разработки** откройте колонку **Дополнительные инструменты**.</span><span class="sxs-lookup"><span data-stu-id="3479c-324">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="3479c-325">Нажмите кнопку **Перейти&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="3479c-325">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="3479c-326">Консоль Kudu открывается в новой вкладке или в окне браузера.</span><span class="sxs-lookup"><span data-stu-id="3479c-326">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="3479c-327">С помощью панели навигации в верхней части страницы откройте **Консоль отладки** и выберите **CMD**.</span><span class="sxs-lookup"><span data-stu-id="3479c-327">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="3479c-328">Откройте папки на пути **веб-сайт** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="3479c-328">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="3479c-329">Если вы не указали путь к файлу *aspnetcore-debug.log*, файл появится в списке.</span><span class="sxs-lookup"><span data-stu-id="3479c-329">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="3479c-330">Если путь указан, перейдите к расположению файла журнала.</span><span class="sxs-lookup"><span data-stu-id="3479c-330">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="3479c-331">Откройте файл журнала, нажав кнопку с изображением карандаша рядом с именем файла.</span><span class="sxs-lookup"><span data-stu-id="3479c-331">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="3479c-332">Завершив устранение неполадок, отключите ведение журнала отладки.</span><span class="sxs-lookup"><span data-stu-id="3479c-332">Disable debug logging when troubleshooting is complete:</span></span>

<span data-ttu-id="3479c-333">Чтобы отключить ведение расширенных журналов отладки, выполните одно из следующих действий:</span><span class="sxs-lookup"><span data-stu-id="3479c-333">To disable the enhanced debug log, perform either of the following:</span></span>

* <span data-ttu-id="3479c-334">Удалите раздел `<handlerSettings>` из файла *web.config* локально и повторно разверните приложение.</span><span class="sxs-lookup"><span data-stu-id="3479c-334">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
* <span data-ttu-id="3479c-335">С помощью консоли Kudu внесите изменения в файл *web.config* и удалите раздел `<handlerSettings>`.</span><span class="sxs-lookup"><span data-stu-id="3479c-335">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="3479c-336">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="3479c-336">Save the file.</span></span>

<span data-ttu-id="3479c-337">Дополнительные сведения см. в разделе <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="3479c-337">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

> [!WARNING]
> <span data-ttu-id="3479c-338">Ошибка при отключении журнала отладки может привести к сбоям приложения или сервера.</span><span class="sxs-lookup"><span data-stu-id="3479c-338">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="3479c-339">Ограничения на размер файла журнала отсутствуют.</span><span class="sxs-lookup"><span data-stu-id="3479c-339">There's no limit on log file size.</span></span> <span data-ttu-id="3479c-340">Для устранения неполадок при запуске приложения используйте только журнал отладки.</span><span class="sxs-lookup"><span data-stu-id="3479c-340">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="3479c-341">После запуска в приложении ASP.NET Core для ведения общего журнала используйте библиотеку ведения журналов, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов.</span><span class="sxs-lookup"><span data-stu-id="3479c-341">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="3479c-342">Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="3479c-342">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker-end

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="3479c-343">Приложение с задержкой или зависанием (служба приложений Azure)</span><span class="sxs-lookup"><span data-stu-id="3479c-343">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="3479c-344">Если приложение медленно реагирует на запрос или зависает при его обработке, см. следующие статьи:</span><span class="sxs-lookup"><span data-stu-id="3479c-344">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="3479c-345">Устранение проблем с производительностью медленных веб приложений в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="3479c-345">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* <span data-ttu-id="3479c-346">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App (Использование расширения сайта средства диагностики сбоев для записи дампа при периодических проблемах с исключением или проблемах с производительностью в веб-приложении Azure)](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/).</span><span class="sxs-lookup"><span data-stu-id="3479c-346">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)</span></span>

### <a name="monitoring-blades"></a><span data-ttu-id="3479c-347">Мониторинг колонок</span><span class="sxs-lookup"><span data-stu-id="3479c-347">Monitoring blades</span></span>

<span data-ttu-id="3479c-348">Мониторинг колонок обеспечивает альтернативный поиск неисправностей методам, описанным ранее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="3479c-348">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="3479c-349">Эти колонки можно использовать для диагностики ошибок 500-й серии.</span><span class="sxs-lookup"><span data-stu-id="3479c-349">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="3479c-350">Убедитесь, что установлены расширения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3479c-350">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="3479c-351">Если расширения не установлены, установите их вручную, выполнив следующие действия.</span><span class="sxs-lookup"><span data-stu-id="3479c-351">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="3479c-352">В колонке **Средства разработки** выберите колонку **Расширения**.</span><span class="sxs-lookup"><span data-stu-id="3479c-352">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="3479c-353">В списке должны появиться **Расширения ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="3479c-353">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="3479c-354">Если расширения не установлены, нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="3479c-354">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="3479c-355">Выберите из списка **Расширения ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="3479c-355">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="3479c-356">Для принятия условий нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="3479c-356">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="3479c-357">Нажмите кнопку **ОК** в колонке **Добавить расширение**.</span><span class="sxs-lookup"><span data-stu-id="3479c-357">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="3479c-358">Информационное всплывающее сообщение указывает, когда расширения успешно установятся.</span><span class="sxs-lookup"><span data-stu-id="3479c-358">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="3479c-359">Если ведение журнала stdout не включено, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="3479c-359">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="3479c-360">На портале Azure выберите колонку **Дополнительные инструменты** в области **Средства разработки**.</span><span class="sxs-lookup"><span data-stu-id="3479c-360">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="3479c-361">Нажмите кнопку **Перейти&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="3479c-361">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="3479c-362">Консоль Kudu открывается в новой вкладке или в окне браузера.</span><span class="sxs-lookup"><span data-stu-id="3479c-362">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="3479c-363">С помощью панели навигации в верхней части страницы откройте **Консоль отладки** и выберите **CMD**.</span><span class="sxs-lookup"><span data-stu-id="3479c-363">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="3479c-364">Откройте папки на пути **веб-сайт** > **wwwroot** и прокрутите список вниз, пока в нижней части не покажется файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="3479c-364">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="3479c-365">Щелкните значок карандаша рядом с файлом *web.config*.</span><span class="sxs-lookup"><span data-stu-id="3479c-365">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="3479c-366">Установите для параметра **stdoutLogEnabled** значение `true` и измените путь **stdoutLogFile** на `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="3479c-366">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="3479c-367">Выберите **Сохранить** для сохранения обновленного файла *web.config*.</span><span class="sxs-lookup"><span data-stu-id="3479c-367">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="3479c-368">Перейдите к активации ведения журнала диагностики, выполнив следующие действия.</span><span class="sxs-lookup"><span data-stu-id="3479c-368">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="3479c-369">На портале Azure выберите колонку **Журналы диагностики**.</span><span class="sxs-lookup"><span data-stu-id="3479c-369">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="3479c-370">Выберите положение переключателя **Вкл.** для **Вход в приложения (файловая система)** и **Подробные сообщения об ошибках**.</span><span class="sxs-lookup"><span data-stu-id="3479c-370">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="3479c-371">В верхней части колонки нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="3479c-371">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="3479c-372">Чтобы включить трассировку неудачно завершенных запросов, также известную как ведение журнала буферизации событий неудачных запросов (FREB), установите положение переключателя **Вкл.** для **Трассировки неудачно завершенных запросов**.</span><span class="sxs-lookup"><span data-stu-id="3479c-372">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="3479c-373">На портале выберите колонку **Поток журнала**, которая в списке отображается сразу под колонкой **Журналы диагностики**.</span><span class="sxs-lookup"><span data-stu-id="3479c-373">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="3479c-374">Сделайте запрос к приложению.</span><span class="sxs-lookup"><span data-stu-id="3479c-374">Make a request to the app.</span></span>
1. <span data-ttu-id="3479c-375">В пределах данных потока журнала указывается причина ошибки.</span><span class="sxs-lookup"><span data-stu-id="3479c-375">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="3479c-376">После завершения устранения неполадок обязательно отключите ведение журнала stdout.</span><span class="sxs-lookup"><span data-stu-id="3479c-376">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="3479c-377">Чтобы просмотреть журналы трассировки неудачно завершенных запросов (журналы FREB), выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="3479c-377">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="3479c-378">Перейдите к колонке **Диагностика и решение проблем** на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="3479c-378">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="3479c-379">Выберите **Журналы трассировки неудачно завершенных запросов** из области боковой панели **Средства поддержки**.</span><span class="sxs-lookup"><span data-stu-id="3479c-379">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="3479c-380">Для получения дополнительной информации см. [Раздел "Трассировка неудачно завершенных запросов" раздела "Включение ведения журналов диагностики для веб-приложений в службе приложений Azure"](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) и ["Вопросы и ответы о производительности приложений для веб-приложений в Azure: как включить трассировку неудачно завершенных запросов?"](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)</span><span class="sxs-lookup"><span data-stu-id="3479c-380">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="3479c-381">Дополнительные сведения см. в разделе [Включение функции ведения журналов диагностики для веб-приложений в службе приложений Azure](/azure/app-service/web-sites-enable-diagnostic-log).</span><span class="sxs-lookup"><span data-stu-id="3479c-381">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="3479c-382">Ошибка при отключении журнала stdout может привести к сбоям приложения или сервера.</span><span class="sxs-lookup"><span data-stu-id="3479c-382">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="3479c-383">Ни размер файла журнала, ни количество создаваемых файлов журналов ничем не ограничены.</span><span class="sxs-lookup"><span data-stu-id="3479c-383">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="3479c-384">Для обычного ведения журналов для приложения ASP.NET Core используйте библиотеку, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов.</span><span class="sxs-lookup"><span data-stu-id="3479c-384">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="3479c-385">Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="3479c-385">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="3479c-386">Устранение неполадок в службах IIS</span><span class="sxs-lookup"><span data-stu-id="3479c-386">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="3479c-387">Журнал событий приложений (IIS)</span><span class="sxs-lookup"><span data-stu-id="3479c-387">Application Event Log (IIS)</span></span>

<span data-ttu-id="3479c-388">Доступ к журналу событий приложений</span><span class="sxs-lookup"><span data-stu-id="3479c-388">Access the Application Event Log:</span></span>

1. <span data-ttu-id="3479c-389">Откройте меню "Пуск", выполните поиск по фразе **Просмотр событий** и выберите приложение **Просмотр событий**.</span><span class="sxs-lookup"><span data-stu-id="3479c-389">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="3479c-390">В **средстве просмотра событий** откройте узел **Журналы Windows**.</span><span class="sxs-lookup"><span data-stu-id="3479c-390">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="3479c-391">Выберите **Приложение**, чтобы открыть журнал событий приложения.</span><span class="sxs-lookup"><span data-stu-id="3479c-391">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="3479c-392">Проверьте здесь наличие ошибок, связанных с проблемным приложением.</span><span class="sxs-lookup"><span data-stu-id="3479c-392">Search for errors associated with the failing app.</span></span> <span data-ttu-id="3479c-393">Нам нужны ошибки со значениями *Модуль IIS AspNetCore* или *Модуль IIS Express AspNetCore* в столбце *Источник*.</span><span class="sxs-lookup"><span data-stu-id="3479c-393">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="3479c-394">Запуск приложения в командной строке</span><span class="sxs-lookup"><span data-stu-id="3479c-394">Run the app at a command prompt</span></span>

<span data-ttu-id="3479c-395">Многие ошибки запуска не создают полезные сведения в журнале событий приложения.</span><span class="sxs-lookup"><span data-stu-id="3479c-395">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="3479c-396">Для некоторых ошибок можно найти причину, запустив приложение в командной строке на компьютере размещения.</span><span class="sxs-lookup"><span data-stu-id="3479c-396">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="3479c-397">развертывание, зависящее от платформы;</span><span class="sxs-lookup"><span data-stu-id="3479c-397">Framework-dependent deployment</span></span>

<span data-ttu-id="3479c-398">Если развертываемое приложение [зависит от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="3479c-398">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="3479c-399">В командной строке перейдите в папку развертывания и запустите приложение, выполнив команду *dotnet.exe* для сборки приложения.</span><span class="sxs-lookup"><span data-stu-id="3479c-399">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="3479c-400">В следующей команде укажите нужное имя сборки вместо заполнителя \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="3479c-400">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="3479c-401">Выходные данные приложения, в том числе любые ошибки, будут выведены в окно консоли.</span><span class="sxs-lookup"><span data-stu-id="3479c-401">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="3479c-402">Если ошибки возникают при выполнении запроса к приложению, выполните запрос к узлу и порту, где Kestrel ожидает передачи данных.</span><span class="sxs-lookup"><span data-stu-id="3479c-402">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="3479c-403">Создайте запрос к `http://localhost:5000/`, используя узел и порт по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3479c-403">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="3479c-404">Если приложение отвечает на запросы по адресу конечной точки Kestrel как обычно, то проблема, вероятнее, связана с конфигурацией размещения, а не с самим приложением.</span><span class="sxs-lookup"><span data-stu-id="3479c-404">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="3479c-405">автономное развертывание;</span><span class="sxs-lookup"><span data-stu-id="3479c-405">Self-contained deployment</span></span>

<span data-ttu-id="3479c-406">Если приложение [развертывается автономно](/dotnet/core/deploying/#self-contained-deployments-scd), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="3479c-406">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="3479c-407">В командной строке перейдите в папку развертывания и запустите исполняемый файл приложения.</span><span class="sxs-lookup"><span data-stu-id="3479c-407">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="3479c-408">В следующей команде укажите нужное имя сборки вместо заполнителя \<assembly_name>: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="3479c-408">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="3479c-409">Выходные данные приложения, в том числе любые ошибки, будут выведены в окно консоли.</span><span class="sxs-lookup"><span data-stu-id="3479c-409">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="3479c-410">Если ошибки возникают при выполнении запроса к приложению, выполните запрос к узлу и порту, где Kestrel ожидает передачи данных.</span><span class="sxs-lookup"><span data-stu-id="3479c-410">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="3479c-411">Создайте запрос к `http://localhost:5000/`, используя узел и порт по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3479c-411">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="3479c-412">Если приложение отвечает на запросы по адресу конечной точки Kestrel как обычно, то проблема, вероятнее, связана с конфигурацией размещения, а не с самим приложением.</span><span class="sxs-lookup"><span data-stu-id="3479c-412">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="3479c-413">Журнал stdout модуля ASP.NET Core (IIS)</span><span class="sxs-lookup"><span data-stu-id="3479c-413">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="3479c-414">Чтобы включить и просмотреть журналы stdout, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="3479c-414">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="3479c-415">Перейдите в папку развертывания сайта на компьютере размещения.</span><span class="sxs-lookup"><span data-stu-id="3479c-415">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="3479c-416">Если здесь нет папки *logs*, создайте ее.</span><span class="sxs-lookup"><span data-stu-id="3479c-416">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="3479c-417">Сведения о том, как настроить в MSBuild автоматическое создание папки *logs* в развертывании, см. в статье [о структуре каталогов](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="3479c-417">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="3479c-418">Измените файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="3479c-418">Edit the *web.config* file.</span></span> <span data-ttu-id="3479c-419">Задайте для параметра **stdoutLogEnabled** значение `true` и измените путь **stdoutLogFile** так, чтобы он указывал на папку *logs* (например, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="3479c-419">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="3479c-420">В этом пути `stdout` обозначает префикс имени для файла журнала.</span><span class="sxs-lookup"><span data-stu-id="3479c-420">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="3479c-421">При создании файла журнала автоматически добавляются метка времени, идентификатор процесса и расширение файла.</span><span class="sxs-lookup"><span data-stu-id="3479c-421">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="3479c-422">Если указан префикс файла `stdout`, стандартный файл журнала будет иметь примерно такое имя: *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="3479c-422">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="3479c-423">Убедитесь, что удостоверение пула приложений имеет разрешения на запись в папку *logs*.</span><span class="sxs-lookup"><span data-stu-id="3479c-423">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="3479c-424">Сохраните обновленный файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="3479c-424">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="3479c-425">Сделайте запрос к приложению.</span><span class="sxs-lookup"><span data-stu-id="3479c-425">Make a request to the app.</span></span>
1. <span data-ttu-id="3479c-426">Перейдите в папку *logs*.</span><span class="sxs-lookup"><span data-stu-id="3479c-426">Navigate to the *logs* folder.</span></span> <span data-ttu-id="3479c-427">Найдите и откройте последний журнал вывода stdout.</span><span class="sxs-lookup"><span data-stu-id="3479c-427">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="3479c-428">Проверьте, нет ли в нем ошибок.</span><span class="sxs-lookup"><span data-stu-id="3479c-428">Study the log for errors.</span></span>

<span data-ttu-id="3479c-429">Завершив устранение неполадок, отключите ведение журнала stdout.</span><span class="sxs-lookup"><span data-stu-id="3479c-429">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="3479c-430">Измените файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="3479c-430">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="3479c-431">Задайте параметру **stdoutLogEnabled** значение `false`.</span><span class="sxs-lookup"><span data-stu-id="3479c-431">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="3479c-432">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="3479c-432">Save the file.</span></span>

<span data-ttu-id="3479c-433">Дополнительные сведения см. в разделе <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="3479c-433">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="3479c-434">Ошибка при отключении журнала stdout может привести к сбоям приложения или сервера.</span><span class="sxs-lookup"><span data-stu-id="3479c-434">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="3479c-435">Ни размер файла журнала, ни количество создаваемых файлов журналов ничем не ограничены.</span><span class="sxs-lookup"><span data-stu-id="3479c-435">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="3479c-436">Для обычного ведения журналов для приложения ASP.NET Core используйте библиотеку, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов.</span><span class="sxs-lookup"><span data-stu-id="3479c-436">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="3479c-437">Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="3479c-437">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-iis"></a><span data-ttu-id="3479c-438">Журнал отладки модуля ASP.NET Core (IIS)</span><span class="sxs-lookup"><span data-stu-id="3479c-438">ASP.NET Core Module debug log (IIS)</span></span>

<span data-ttu-id="3479c-439">Добавьте следующие параметры обработчика в файл *Web. config* приложения, чтобы включить ASP.NET Core журнал отладки модуля:</span><span class="sxs-lookup"><span data-stu-id="3479c-439">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug log:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="3479c-440">Убедитесь, что указанный для журнала путь существует и удостоверение пула приложений имеет разрешения на запись в это расположение.</span><span class="sxs-lookup"><span data-stu-id="3479c-440">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

<span data-ttu-id="3479c-441">Дополнительные сведения см. в разделе <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="3479c-441">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

::: moniker-end

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="3479c-442">Включение страницы исключений для разработчика</span><span class="sxs-lookup"><span data-stu-id="3479c-442">Enable the Developer Exception Page</span></span>

<span data-ttu-id="3479c-443">Можно добавить переменную среды `ASPNETCORE_ENVIRONMENT` [в файл web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables), чтобы запустить приложение в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="3479c-443">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="3479c-444">Если среда не переопределяется при запуске приложения использованием `UseEnvironment` в конструкторе узла, эта переменная среды позволяет отображать [страницу исключения для разработчика](xref:fundamentals/error-handling) при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="3479c-444">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="3479c-445">Настройка переменной среды `ASPNETCORE_ENVIRONMENT` рекомендуется только на промежуточных и тестовых серверах, доступ к которым из Интернета закрыт.</span><span class="sxs-lookup"><span data-stu-id="3479c-445">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="3479c-446">Завершив устранение неполадок, удалите эту переменную среды из файла *web.config*.</span><span class="sxs-lookup"><span data-stu-id="3479c-446">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="3479c-447">Сведения о настройке переменных среды в файле *web.config* см. в статье о [дочернем элементе environmentVariables в aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="3479c-447">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="3479c-448">Получение данных из приложения</span><span class="sxs-lookup"><span data-stu-id="3479c-448">Obtain data from an app</span></span>

<span data-ttu-id="3479c-449">Если приложение способно отвечать на запросы, получите данные о запросе, подключении и дополнительные данные из приложений с помощью встроенного терминала ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="3479c-449">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="3479c-450">Дополнительные сведения и примеры с кодом см. здесь: <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="3479c-450">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="3479c-451">Низкое или зависание приложения (IIS)</span><span class="sxs-lookup"><span data-stu-id="3479c-451">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="3479c-452">*Аварийный дамп* — это моментальный снимок памяти системы, который может помочь определить причину сбоя приложения, сбоя при запуске или медленных приложений.</span><span class="sxs-lookup"><span data-stu-id="3479c-452">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="3479c-453">Аварийное завершение работы приложения или исключение</span><span class="sxs-lookup"><span data-stu-id="3479c-453">App crashes or encounters an exception</span></span>

<span data-ttu-id="3479c-454">Получите и проанализируйте дамп из [отчетов об ошибках Windows (WER)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="3479c-454">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="3479c-455">Создайте папку для хранения файлов аварийного дампа в `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="3479c-455">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="3479c-456">Пул приложений должен иметь доступ на запись к папке.</span><span class="sxs-lookup"><span data-stu-id="3479c-456">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="3479c-457">Запустите [скрипт PowerShell EnableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="3479c-457">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="3479c-458">Если приложение использует [модель размещения в процессе](xref:host-and-deploy/iis/index#in-process-hosting-model), выполните скрипт для *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="3479c-458">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="3479c-459">Если приложение использует [модель размещения вне процесса](xref:host-and-deploy/iis/index#out-of-process-hosting-model), выполните скрипт для *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="3479c-459">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="3479c-460">Запустите приложение в условиях, вызывающих аварийное завершение.</span><span class="sxs-lookup"><span data-stu-id="3479c-460">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="3479c-461">После аварийного завершения запустите [скрипт PowerShell DisableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="3479c-461">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="3479c-462">Если приложение использует [модель размещения в процессе](xref:host-and-deploy/iis/index#in-process-hosting-model), выполните скрипт для *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="3479c-462">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="3479c-463">Если приложение использует [модель размещения вне процесса](xref:host-and-deploy/iis/index#out-of-process-hosting-model), выполните скрипт для *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="3479c-463">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="3479c-464">Когда приложение аварийно завершит работу и сбор дампов будет выполнен, приложение сможет завершить работу обычным образом.</span><span class="sxs-lookup"><span data-stu-id="3479c-464">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="3479c-465">Скрипт PowerShell настраивает отчеты об ошибках Windows для сбора до пяти дампов для приложения.</span><span class="sxs-lookup"><span data-stu-id="3479c-465">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="3479c-466">Аварийные дампы могут занимать много места на диске (до нескольких гигабайтов каждый).</span><span class="sxs-lookup"><span data-stu-id="3479c-466">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="3479c-467">Приложение перестает отвечать на запросы, не запускается или работает в обычном режиме</span><span class="sxs-lookup"><span data-stu-id="3479c-467">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="3479c-468">Когда приложение *перестает отвечать на запросы* (но аварийное завершение не происходит), не запускается или работает в обычном режиме, см. раздел [Файлы дампа пользовательского режима: выбор лучшего инструмента](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool), чтобы выбрать подходящий инструмент для создания дампа.</span><span class="sxs-lookup"><span data-stu-id="3479c-468">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="3479c-469">Анализ дампа</span><span class="sxs-lookup"><span data-stu-id="3479c-469">Analyze the dump</span></span>

<span data-ttu-id="3479c-470">Дамп можно проанализировать несколькими способами.</span><span class="sxs-lookup"><span data-stu-id="3479c-470">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="3479c-471">Дополнительные сведения см. в разделе [Анализ файла дампа пользовательского режима](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="3479c-471">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="3479c-472">Очистить кэши пакетов</span><span class="sxs-lookup"><span data-stu-id="3479c-472">Clear package caches</span></span>

<span data-ttu-id="3479c-473">Иногда работающее приложение завершается сбоем сразу после обновления пакет SDK для .NET Core на компьютере разработки или при изменении версий пакета в приложении.</span><span class="sxs-lookup"><span data-stu-id="3479c-473">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="3479c-474">В некоторых случаях в результате важного обновления несогласованные версии пакетов могут привести к нарушению работы приложения.</span><span class="sxs-lookup"><span data-stu-id="3479c-474">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="3479c-475">Большинство этих проблем можно исправить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="3479c-475">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="3479c-476">Удалите папки *bin* и *obj*.</span><span class="sxs-lookup"><span data-stu-id="3479c-476">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="3479c-477">Очистите кэши пакетов, выполнив `dotnet nuget locals all --clear` команду из командной оболочки.</span><span class="sxs-lookup"><span data-stu-id="3479c-477">Clear the package caches by executing `dotnet nuget locals all --clear` from a command shell.</span></span>

   <span data-ttu-id="3479c-478">Очистка кэшей пакетов также может быть выполнена с помощью средства [NuGet. exe](https://www.nuget.org/downloads) и выполнения команды `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="3479c-478">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="3479c-479">*NuGet.exe* не входит в пакет установки операционной системы Windows для настольных компьютеров и его нужно получить отдельно на [веб-сайте NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="3479c-479">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="3479c-480">Восстановите и перестройте проект.</span><span class="sxs-lookup"><span data-stu-id="3479c-480">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="3479c-481">Удалите все файлы из папки развертывания на сервере перед повторным развертыванием приложения.</span><span class="sxs-lookup"><span data-stu-id="3479c-481">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3479c-482">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="3479c-482">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="3479c-483">Документация по Azure</span><span class="sxs-lookup"><span data-stu-id="3479c-483">Azure documentation</span></span>

* [<span data-ttu-id="3479c-484">Application Insights для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3479c-484">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="3479c-485">Раздел "Удаленная отладка веб-приложений" статьи Устранение неполадок веб-приложения в службе приложений Azure с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3479c-485">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="3479c-486">Общие сведения о диагностике в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="3479c-486">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="3479c-487">Практическое руководство. Мониторинг приложений в Службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="3479c-487">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="3479c-488">Устранение неполадок веб-приложения в службе приложений Azure с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3479c-488">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="3479c-489">Устранение ошибок HTTP "502 — недопустимый шлюз" и "503 — служба недоступна" в веб-приложениях Azure</span><span class="sxs-lookup"><span data-stu-id="3479c-489">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="3479c-490">Устранение проблем с производительностью медленных веб приложений в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="3479c-490">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="3479c-491">Вопросы и ответы о производительности приложений для веб-приложений в Azure</span><span class="sxs-lookup"><span data-stu-id="3479c-491">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [<span data-ttu-id="3479c-492">"Песочница" веб-приложений Azure (ограничения работы среды выполнения службы приложений)</span><span class="sxs-lookup"><span data-stu-id="3479c-492">Azure Web App sandbox (App Service runtime execution limitations)</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [<span data-ttu-id="3479c-493">Пятница с Azure. Практика диагностики и устранения неполадок в Службе приложений Azure (12-минутное видео)</span><span class="sxs-lookup"><span data-stu-id="3479c-493">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a><span data-ttu-id="3479c-494">Документация по Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3479c-494">Visual Studio documentation</span></span>

* [<span data-ttu-id="3479c-495">Удаленная отладка ASP.NET Core на IIS в Azure в Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="3479c-495">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="3479c-496">Удаленная отладка ASP.NET Core на удаленном компьютере IIS в Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="3479c-496">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="3479c-497">Сведения об отладке с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3479c-497">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="3479c-498">Документация по Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3479c-498">Visual Studio Code documentation</span></span>

* [<span data-ttu-id="3479c-499">Отладка с помощью Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3479c-499">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)
