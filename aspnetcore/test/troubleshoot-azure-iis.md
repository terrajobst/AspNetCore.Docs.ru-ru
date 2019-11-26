---
title: Устранение неполадок ASP.NET Core в службе приложений Azure и службах IIS
author: guardrex
description: Узнайте, как диагностировать проблемы со службами приложений Azure и развертываниями службы IIS (IIS) ASP.NET Core приложений.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/20/2019
uid: test/troubleshoot-azure-iis
ms.openlocfilehash: 49a0f59fb6930235de10c726f3695f2a5352efb2
ms.sourcegitcommit: 8157e5a351f49aeef3769f7d38b787b4386aad5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2019
ms.locfileid: "74251962"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service-and-iis"></a><span data-ttu-id="93e40-103">Устранение неполадок ASP.NET Core в службе приложений Azure и службах IIS</span><span class="sxs-lookup"><span data-stu-id="93e40-103">Troubleshoot ASP.NET Core on Azure App Service and IIS</span></span>

<span data-ttu-id="93e40-104">[Люк ЛаСаМ](https://github.com/guardrex) и [Джастин коталик](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="93e40-104">By [Luke Latham](https://github.com/guardrex) and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="93e40-105">В этой статье содержатся сведения об общих ошибках запуска приложений и инструкции по диагностике ошибок при развертывании приложения в службе приложений Azure или службах IIS.</span><span class="sxs-lookup"><span data-stu-id="93e40-105">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="93e40-106">Ошибки запуска приложений</span><span class="sxs-lookup"><span data-stu-id="93e40-106">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="93e40-107">Описание распространенных сценариев кода состояния HTTP для запуска.</span><span class="sxs-lookup"><span data-stu-id="93e40-107">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="93e40-108">Устранение неполадок в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="93e40-108">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="93e40-109">Рекомендации по устранению неполадок для приложений, развернутых в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="93e40-109">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="93e40-110">Устранение неполадок в IIS</span><span class="sxs-lookup"><span data-stu-id="93e40-110">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="93e40-111">Рекомендации по устранению неполадок приложений, развернутых в службах IIS или запущенных на IIS Express локально.</span><span class="sxs-lookup"><span data-stu-id="93e40-111">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="93e40-112">Руководство относится к развертываниям Windows Server и Windows Desktop.</span><span class="sxs-lookup"><span data-stu-id="93e40-112">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="93e40-113">Очистить кэши пакетов</span><span class="sxs-lookup"><span data-stu-id="93e40-113">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="93e40-114">Объясняет, что делать, когда несогласованные пакеты нарушают работу приложения при выполнении основных обновлений или изменении версий пакета.</span><span class="sxs-lookup"><span data-stu-id="93e40-114">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="93e40-115">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="93e40-115">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="93e40-116">Список дополнительных разделов по устранению неполадок.</span><span class="sxs-lookup"><span data-stu-id="93e40-116">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="93e40-117">Ошибки запуска приложения</span><span class="sxs-lookup"><span data-stu-id="93e40-117">App startup errors</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="93e40-118">В Visual Studio проект ASP.NET Core по умолчанию использует размещение в [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) на период отладки.</span><span class="sxs-lookup"><span data-stu-id="93e40-118">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="93e40-119">Ошибка в *502,5-процессе* или *500,30-запуск* , возникающая при локальной отладке для диагностики с помощью инструкции в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="93e40-119">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="93e40-120">В Visual Studio проект ASP.NET Core по умолчанию использует размещение в [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) на период отладки.</span><span class="sxs-lookup"><span data-stu-id="93e40-120">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="93e40-121">*Ошибка процесса 502,5* , возникающая при локальной отладке, может быть выявлена с помощью Совета в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="93e40-121">A *502.5 Process Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

### <a name="40314-forbidden"></a><span data-ttu-id="93e40-122">403,14 запрещено</span><span class="sxs-lookup"><span data-stu-id="93e40-122">403.14 Forbidden</span></span>

<span data-ttu-id="93e40-123">Не удается запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="93e40-123">The app fails to start.</span></span> <span data-ttu-id="93e40-124">Регистрируется следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="93e40-124">The following error is logged:</span></span>

```
The Web server is configured to not list the contents of this directory.
```

<span data-ttu-id="93e40-125">Эта ошибка обычно вызвана неработающей развертыванием в системе размещения, которая включает в себя любой из следующих сценариев:</span><span class="sxs-lookup"><span data-stu-id="93e40-125">The error is usually caused by a broken deployment on the hosting system, which includes any of the following scenarios:</span></span>

* <span data-ttu-id="93e40-126">Приложение развертывается в неверной папке в системе размещения.</span><span class="sxs-lookup"><span data-stu-id="93e40-126">The app is deployed to the wrong folder on the hosting system.</span></span>
* <span data-ttu-id="93e40-127">Процессу развертывания не удалось переместить все файлы и папки приложения в папку развертывания в системе размещения.</span><span class="sxs-lookup"><span data-stu-id="93e40-127">The deployment process failed to move all of the app's files and folders to the deployment folder on the hosting system.</span></span>
* <span data-ttu-id="93e40-128">В развертывании отсутствует файл *Web. config* , или содержимое файла *Web. config* имеет неправильный формат.</span><span class="sxs-lookup"><span data-stu-id="93e40-128">The *web.config* file is missing from the deployment, or the *web.config* file contents are malformed.</span></span>

<span data-ttu-id="93e40-129">Выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="93e40-129">Perform the following steps:</span></span>

1. <span data-ttu-id="93e40-130">Удалите все файлы и папки из папки развертывания в системе размещения.</span><span class="sxs-lookup"><span data-stu-id="93e40-130">Delete all of the files and folders from the deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="93e40-131">Повторно разверните содержимое папки *публикации* приложения в системе размещения с помощью обычного метода развертывания, например Visual Studio, PowerShell или ручного развертывания:</span><span class="sxs-lookup"><span data-stu-id="93e40-131">Redeploy the contents of the app's *publish* folder to the hosting system using your normal method of deployment, such as Visual Studio, PowerShell, or manual deployment:</span></span>
   * <span data-ttu-id="93e40-132">Убедитесь, что файл *Web. config* находится в развертывании и что его содержимое указано правильно.</span><span class="sxs-lookup"><span data-stu-id="93e40-132">Confirm that the *web.config* file is present in the deployment and that its contents are correct.</span></span>
   * <span data-ttu-id="93e40-133">При размещении в службе приложений Azure убедитесь, что приложение развернуто в папке `D:\home\site\wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="93e40-133">When hosting on Azure App Service, confirm that the app is deployed to the `D:\home\site\wwwroot` folder.</span></span>
   * <span data-ttu-id="93e40-134">Если приложение размещено в IIS, убедитесь, что приложение развернуто на **физический путь** IIS, показанный в **основных параметрах** **диспетчера IIS**.</span><span class="sxs-lookup"><span data-stu-id="93e40-134">When the app is hosted by IIS, confirm that the app is deployed to the IIS **Physical path** shown in **IIS Manager**'s **Basic Settings**.</span></span>
1. <span data-ttu-id="93e40-135">Убедитесь, что все файлы и папки приложения развернуты путем сравнения развертывания в системе размещения с содержимым папки *публикации* проекта.</span><span class="sxs-lookup"><span data-stu-id="93e40-135">Confirm that all of the app's files and folders are deployed by comparing the deployment on the hosting system to the contents of the project's *publish* folder.</span></span>

<span data-ttu-id="93e40-136">Дополнительные сведения о макете опубликованного ASP.NET Core приложения см. в разделе <xref:host-and-deploy/directory-structure>.</span><span class="sxs-lookup"><span data-stu-id="93e40-136">For more information on the layout of a published ASP.NET Core app, see <xref:host-and-deploy/directory-structure>.</span></span> <span data-ttu-id="93e40-137">Дополнительные сведения о файле *Web. config* см. в разделе <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="93e40-137">For more information on the *web.config* file, see <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="93e40-138">500 Internal Server Error (внутренняя ошибка сервера)</span><span class="sxs-lookup"><span data-stu-id="93e40-138">500 Internal Server Error</span></span>

<span data-ttu-id="93e40-139">Приложение запускается, но ошибка не позволяет серверу выполнить запрос.</span><span class="sxs-lookup"><span data-stu-id="93e40-139">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="93e40-140">Эта ошибка возникает в коде приложения при запуске или при создании ответа.</span><span class="sxs-lookup"><span data-stu-id="93e40-140">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="93e40-141">Ответ может не иметь содержимого или ответ в браузере может выглядеть как *500 — внутренняя ошибка сервера*.</span><span class="sxs-lookup"><span data-stu-id="93e40-141">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="93e40-142">Как правило, в журнале событий приложения указывается, что приложение запускается в обычном режиме.</span><span class="sxs-lookup"><span data-stu-id="93e40-142">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="93e40-143">Сервер действует правильно.</span><span class="sxs-lookup"><span data-stu-id="93e40-143">From the server's perspective, that's correct.</span></span> <span data-ttu-id="93e40-144">Приложение запустилось, но не может сформировать допустимый ответ.</span><span class="sxs-lookup"><span data-stu-id="93e40-144">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="93e40-145">Запустите приложение из командной строки на сервере или включите журнал ASP.NET Core модуля stdout, чтобы устранить проблему.</span><span class="sxs-lookup"><span data-stu-id="93e40-145">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

::: moniker range="= aspnetcore-2.2"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="93e40-146">500.0 In-Process Handler Load Failure (ошибка загрузки внутрипроцессного обработчика)</span><span class="sxs-lookup"><span data-stu-id="93e40-146">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="93e40-147">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="93e40-147">The worker process fails.</span></span> <span data-ttu-id="93e40-148">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="93e40-148">The app doesn't start.</span></span>

<span data-ttu-id="93e40-149">[Модулю ASP.NET Core](xref:host-and-deploy/aspnet-core-module) не удается найти среду CLR .NET Core и найти обработчик внутрипроцессного запроса (*aspnetcorev2_inprocess. dll*).</span><span class="sxs-lookup"><span data-stu-id="93e40-149">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="93e40-150">Выполните следующие проверки:</span><span class="sxs-lookup"><span data-stu-id="93e40-150">Check that:</span></span>

* <span data-ttu-id="93e40-151">приложение предназначено для пакета NuGet [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) или [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app);</span><span class="sxs-lookup"><span data-stu-id="93e40-151">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="93e40-152">версия общей платформы ASP.NET Core, для которой предназначено приложение, установлена на целевом компьютере.</span><span class="sxs-lookup"><span data-stu-id="93e40-152">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="93e40-153">500.0 Out-Of-Process Handler Load Failure (ошибка загрузки внепроцессного обработчика)</span><span class="sxs-lookup"><span data-stu-id="93e40-153">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="93e40-154">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="93e40-154">The worker process fails.</span></span> <span data-ttu-id="93e40-155">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="93e40-155">The app doesn't start.</span></span>

<span data-ttu-id="93e40-156">[Модулю ASP.NET Core](xref:host-and-deploy/aspnet-core-module) не удается найти обработчик запросов внутрипроцессного размещения.</span><span class="sxs-lookup"><span data-stu-id="93e40-156">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="93e40-157">Убедитесь, что *aspnetcorev2_outofprocess.dll* находится во вложенной папке рядом с *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="93e40-157">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="93e40-158">500.0 In-Process Handler Load Failure (ошибка загрузки внутрипроцессного обработчика)</span><span class="sxs-lookup"><span data-stu-id="93e40-158">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="93e40-159">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="93e40-159">The worker process fails.</span></span> <span data-ttu-id="93e40-160">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="93e40-160">The app doesn't start.</span></span>

<span data-ttu-id="93e40-161">При загрузке компонентов [модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module) произошла неизвестная ошибка.</span><span class="sxs-lookup"><span data-stu-id="93e40-161">An unknown error occurred loading [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) components.</span></span> <span data-ttu-id="93e40-162">Выполните одно из указанных ниже действий.</span><span class="sxs-lookup"><span data-stu-id="93e40-162">Take one of the following actions:</span></span>

* <span data-ttu-id="93e40-163">Обратитесь в [службу поддержки Майкрософт](https://support.microsoft.com/oas/default.aspx?prid=15832) (выберите **Средства разработчика**, а затем — **ASP.NET Core**).</span><span class="sxs-lookup"><span data-stu-id="93e40-163">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="93e40-164">Задайте вопрос на сайте Stack Overflow.</span><span class="sxs-lookup"><span data-stu-id="93e40-164">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="93e40-165">Сообщите о проблеме в нашем [репозитории GitHub](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="93e40-165">File an issue on our [GitHub repository](https://github.com/aspnet/AspNetCore).</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="93e40-166">500.30 In-Process Startup Failure (ошибка внутрипроцессного запуска)</span><span class="sxs-lookup"><span data-stu-id="93e40-166">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="93e40-167">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="93e40-167">The worker process fails.</span></span> <span data-ttu-id="93e40-168">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="93e40-168">The app doesn't start.</span></span>

<span data-ttu-id="93e40-169">[Модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module) пытается запустить среду CLR .NET Core в процессе, но не запускается.</span><span class="sxs-lookup"><span data-stu-id="93e40-169">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="93e40-170">Причину сбоя запуска процесса обычно можно определить на основе записей в журнале событий приложений и в журнале stdout модуля ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93e40-170">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="93e40-171">Распространенной причиной сбоя является неправильная настройка приложения с выбором отсутствующей версии общей платформы ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93e40-171">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="93e40-172">Проверьте, какие версии общей платформы ASP.NET Core установлены на целевом компьютере.</span><span class="sxs-lookup"><span data-stu-id="93e40-172">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="93e40-173">500.31 ANCM Failed to Find Native Dependencies (ANCM не удалось найти собственные зависимости)</span><span class="sxs-lookup"><span data-stu-id="93e40-173">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="93e40-174">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="93e40-174">The worker process fails.</span></span> <span data-ttu-id="93e40-175">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="93e40-175">The app doesn't start.</span></span>

<span data-ttu-id="93e40-176">[Модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module) пытается запустить среду выполнения .NET Core в процессе, но не запускается.</span><span class="sxs-lookup"><span data-stu-id="93e40-176">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="93e40-177">Наиболее распространенная причина этой ошибки в том, что среда выполнения `Microsoft.NETCore.App` или `Microsoft.AspNetCore.App` не установлена.</span><span class="sxs-lookup"><span data-stu-id="93e40-177">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="93e40-178">Если целевой платформой развернутого приложения является ASP.NET Core 3.0, но эта версия отсутствует на компьютере, происходит данная ошибка.</span><span class="sxs-lookup"><span data-stu-id="93e40-178">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="93e40-179">Пример сообщения об ошибке:</span><span class="sxs-lookup"><span data-stu-id="93e40-179">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="93e40-180">В сообщении об ошибке перечислены все установленные версии .NET Core и указывается версия, которая требуется приложению.</span><span class="sxs-lookup"><span data-stu-id="93e40-180">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="93e40-181">Чтобы устранить эту ошибку, выполните одно из указанных ниже действий.</span><span class="sxs-lookup"><span data-stu-id="93e40-181">To fix this error, either:</span></span>

* <span data-ttu-id="93e40-182">Установите соответствующую версию .NET Core на компьютере.</span><span class="sxs-lookup"><span data-stu-id="93e40-182">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="93e40-183">Измените целевую версию .NET Core приложения на версию, имеющуюся на компьютере.</span><span class="sxs-lookup"><span data-stu-id="93e40-183">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="93e40-184">Опубликуйте приложение как [автономное развертывание](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="93e40-184">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="93e40-185">При запуске в среде разработки (переменная среды `ASPNETCORE_ENVIRONMENT` имеет значение `Development`) ошибка записывается в ответ HTTP.</span><span class="sxs-lookup"><span data-stu-id="93e40-185">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="93e40-186">Причина сбоя запуска процесса также находится в журнале событий приложений.</span><span class="sxs-lookup"><span data-stu-id="93e40-186">The cause of a process startup failure is also found in the Application Event Log.</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="93e40-187">500.32 ANCM Failed to Load dll (ANCM не удалось загрузить библиотеку DLL)</span><span class="sxs-lookup"><span data-stu-id="93e40-187">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="93e40-188">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="93e40-188">The worker process fails.</span></span> <span data-ttu-id="93e40-189">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="93e40-189">The app doesn't start.</span></span>

<span data-ttu-id="93e40-190">Наиболее распространенная причина этой ошибки в том, что приложение опубликовано для несовместимой архитектуры процессора.</span><span class="sxs-lookup"><span data-stu-id="93e40-190">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="93e40-191">Если рабочий процесс выполняется как 32-разрядное приложение, но приложение было опубликовано для 64-разрядной архитектуры, происходит эта ошибка.</span><span class="sxs-lookup"><span data-stu-id="93e40-191">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="93e40-192">Чтобы устранить эту ошибку, выполните одно из указанных ниже действий.</span><span class="sxs-lookup"><span data-stu-id="93e40-192">To fix this error, either:</span></span>

* <span data-ttu-id="93e40-193">Повторно опубликуйте приложение для той же архитектуры процессора, для которой предназначен рабочий процесс.</span><span class="sxs-lookup"><span data-stu-id="93e40-193">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="93e40-194">Опубликуйте приложение как [зависящее от платформы развертывание](/dotnet/core/deploying/#framework-dependent-executables-fde).</span><span class="sxs-lookup"><span data-stu-id="93e40-194">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="93e40-195">500.33 ANCM Request Handler Load Failure (Ошибка загрузки обработчика запросов ANCM)</span><span class="sxs-lookup"><span data-stu-id="93e40-195">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="93e40-196">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="93e40-196">The worker process fails.</span></span> <span data-ttu-id="93e40-197">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="93e40-197">The app doesn't start.</span></span>

<span data-ttu-id="93e40-198">Приложение не ссылается на платформу `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="93e40-198">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="93e40-199">Только приложения, предназначенные для платформы `Microsoft.AspNetCore.App` Framework, могут размещаться в [модуле ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="93e40-199">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="93e40-200">Для устранения этой ошибки убедитесь в том, что приложение предназначено для платформы `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="93e40-200">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="93e40-201">В файле `.runtimeconfig.json` проверьте, является ли эта платформа целевой для приложения.</span><span class="sxs-lookup"><span data-stu-id="93e40-201">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="93e40-202">500.34 ANCM Mixed Hosting Models Not Supported (Смешанные модели размещения ANCM не поддерживаются)</span><span class="sxs-lookup"><span data-stu-id="93e40-202">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="93e40-203">В одном рабочем процессе не могут выполняться одновременно внутрипроцессное приложение и внепроцессное приложение.</span><span class="sxs-lookup"><span data-stu-id="93e40-203">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="93e40-204">Для устранения этой ошибки запускайте приложения в отдельных пулах приложений IIS.</span><span class="sxs-lookup"><span data-stu-id="93e40-204">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="93e40-205">500.35 ANCM Multiple In-Process Applications in same Process (Несколько внутрипроцессных приложений ANCM в одном процессе)</span><span class="sxs-lookup"><span data-stu-id="93e40-205">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="93e40-206">Рабочий процесс не может запускать несколько процессов внутри процесса в одном процессе.</span><span class="sxs-lookup"><span data-stu-id="93e40-206">The worker process can't run multiple in-process apps in the same process.</span></span>

<span data-ttu-id="93e40-207">Для устранения этой ошибки запускайте приложения в отдельных пулах приложений IIS.</span><span class="sxs-lookup"><span data-stu-id="93e40-207">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="93e40-208">500.36 ANCM Out-Of-Process Handler Load Failure (Ошибка загрузки внепроцессного обработчика ANCM)</span><span class="sxs-lookup"><span data-stu-id="93e40-208">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="93e40-209">Рядом с файлом *aspnetcorev2.dll* нет внепроцессного обработчика запросов *aspnetcorev2_outofprocess.dll*.</span><span class="sxs-lookup"><span data-stu-id="93e40-209">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="93e40-210">Это указывает на поврежденную установку [модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="93e40-210">This indicates a corrupted installation of the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="93e40-211">Для устранения этой ошибки восстановите установку [пакета размещения .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (для служб IIS) или Visual Studio (для IIS Express).</span><span class="sxs-lookup"><span data-stu-id="93e40-211">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="93e40-212">500.37 ANCM Failed to Start Within Startup Time Limit (Модуль ANCM не запустился в течение предельного времени запуска)</span><span class="sxs-lookup"><span data-stu-id="93e40-212">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="93e40-213">Модуль ANCM не запустился в течение заданного предельного времени запуска.</span><span class="sxs-lookup"><span data-stu-id="93e40-213">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="93e40-214">По умолчанию время ожидания составляет 120 секунд.</span><span class="sxs-lookup"><span data-stu-id="93e40-214">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="93e40-215">Эта ошибка может произойти, если на одном компьютере запускается много приложений.</span><span class="sxs-lookup"><span data-stu-id="93e40-215">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="93e40-216">Обратите внимание на пики использования ЦП и памяти на сервере во время запуска.</span><span class="sxs-lookup"><span data-stu-id="93e40-216">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="93e40-217">Возможно, потребуется регулировать процесс запуска нескольких приложений.</span><span class="sxs-lookup"><span data-stu-id="93e40-217">You may need to stagger the startup process of multiple apps.</span></span>

::: moniker-end

### <a name="5025-process-failure"></a><span data-ttu-id="93e40-218">502.5 Process Failure (ошибка процесса)</span><span class="sxs-lookup"><span data-stu-id="93e40-218">502.5 Process Failure</span></span>

<span data-ttu-id="93e40-219">Рабочий процесс завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="93e40-219">The worker process fails.</span></span> <span data-ttu-id="93e40-220">Приложение не запускается.</span><span class="sxs-lookup"><span data-stu-id="93e40-220">The app doesn't start.</span></span>

<span data-ttu-id="93e40-221">[Модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module) пытается запустить рабочий процесс, но он не запускается.</span><span class="sxs-lookup"><span data-stu-id="93e40-221">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="93e40-222">Причину сбоя запуска процесса обычно можно определить на основе записей в журнале событий приложений и в журнале stdout модуля ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93e40-222">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="93e40-223">Распространенной причиной сбоя является неправильная настройка приложения с выбором отсутствующей версии общей платформы ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93e40-223">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="93e40-224">Проверьте, какие версии общей платформы ASP.NET Core установлены на целевом компьютере.</span><span class="sxs-lookup"><span data-stu-id="93e40-224">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="93e40-225">*Общая платформа* — это набор сборок (*DLLl*-файлы), которые установлены на компьютере и на которые ссылается метапакет, например `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="93e40-225">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="93e40-226">В ссылках метапакета может быть указана минимальная требуемая версия.</span><span class="sxs-lookup"><span data-stu-id="93e40-226">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="93e40-227">Дополнительную информацию см. в этой публикации об [общей платформе](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span><span class="sxs-lookup"><span data-stu-id="93e40-227">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="93e40-228">Когда ошибки в конфигурации размещения или приложения приводят к сбою рабочего процесса, возвращается страница ошибки *502.5 — ошибка процесса*.</span><span class="sxs-lookup"><span data-stu-id="93e40-228">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="93e40-229">Не удалось запустить приложение (код ошибки "0x800700c1")</span><span class="sxs-lookup"><span data-stu-id="93e40-229">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="93e40-230">Не удалось запустить приложение, так как не удалось загрузить сборку приложения ( *.dll*).</span><span class="sxs-lookup"><span data-stu-id="93e40-230">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="93e40-231">Эта ошибка возникает, если существует несоответствие разрядности опубликованного приложения и процесса w3wp/iisexpress.</span><span class="sxs-lookup"><span data-stu-id="93e40-231">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="93e40-232">Убедитесь, что параметр 32-разрядности пула приложений указан правильно:</span><span class="sxs-lookup"><span data-stu-id="93e40-232">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="93e40-233">Выберите пул приложений в диспетчере служб IIS в разделе **Пулы приложений**.</span><span class="sxs-lookup"><span data-stu-id="93e40-233">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="93e40-234">Выберите **Дополнительные параметры** в разделе **Изменение пула приложений** панели **Действия**.</span><span class="sxs-lookup"><span data-stu-id="93e40-234">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="93e40-235">Установите параметр **Включить 32-разрядные приложения**:</span><span class="sxs-lookup"><span data-stu-id="93e40-235">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="93e40-236">При развертывании 32-разрядного (x86) приложения задайте значение `True`.</span><span class="sxs-lookup"><span data-stu-id="93e40-236">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="93e40-237">При развертывании 64-разрядного (x64) приложения задайте значение `False`.</span><span class="sxs-lookup"><span data-stu-id="93e40-237">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

<span data-ttu-id="93e40-238">Убедитесь в отсутствии конфликта между свойством `<Platform>` MSBuild в файле проекта и опубликованной разрядностью приложения.</span><span class="sxs-lookup"><span data-stu-id="93e40-238">Confirm that there isn't a conflict between a `<Platform>` MSBuild property in the project file and the published bitness of the app.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="93e40-239">Сброс подключения</span><span class="sxs-lookup"><span data-stu-id="93e40-239">Connection reset</span></span>

<span data-ttu-id="93e40-240">Если ошибка возникает после отправки заголовков, сервер уже не может отправить страницу **500 — внутренняя ошибка сервера**.</span><span class="sxs-lookup"><span data-stu-id="93e40-240">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="93e40-241">Это часто происходит, когда ошибка возникает во время сериализации составных объектов ответа.</span><span class="sxs-lookup"><span data-stu-id="93e40-241">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="93e40-242">Этот тип ошибки отображается на стороне клиента как ошибка *Сброс подключения*.</span><span class="sxs-lookup"><span data-stu-id="93e40-242">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="93e40-243">[Ведение журналов для приложений](xref:fundamentals/logging/index) может помочь устранить эти ошибки.</span><span class="sxs-lookup"><span data-stu-id="93e40-243">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="93e40-244">Ограничения по умолчанию при запуске</span><span class="sxs-lookup"><span data-stu-id="93e40-244">Default startup limits</span></span>

<span data-ttu-id="93e40-245">Для [модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module) настроено значение по умолчанию *startupTimeLimit* , равное 120 секундам.</span><span class="sxs-lookup"><span data-stu-id="93e40-245">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="93e40-246">Если оставить значение по умолчанию, то прежде чем журнал модуля зарегистрирует ошибку запуска приложения, может пройти до двух минут.</span><span class="sxs-lookup"><span data-stu-id="93e40-246">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="93e40-247">Сведения о настройке модуля см. в разделе [Атрибуты элемента aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="93e40-247">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="93e40-248">Устранение неполадок в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="93e40-248">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="93e40-249">Журнал событий приложений (служба приложений Azure)</span><span class="sxs-lookup"><span data-stu-id="93e40-249">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="93e40-250">Чтобы получить доступ к журналу событий приложения, используйте колонку **Диагностика и решение проблем** на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="93e40-250">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="93e40-251">На портале Azure откройте приложение в колонке **Службы приложений**.</span><span class="sxs-lookup"><span data-stu-id="93e40-251">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="93e40-252">Выберите колонку **Диагностика и решение проблем**.</span><span class="sxs-lookup"><span data-stu-id="93e40-252">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="93e40-253">Щелкните заголовок **Средства диагностики**.</span><span class="sxs-lookup"><span data-stu-id="93e40-253">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="93e40-254">В разделе **Средства поддержки** нажмите кнопку **События приложений**.</span><span class="sxs-lookup"><span data-stu-id="93e40-254">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="93e40-255">Изучите последние ошибки из журнала *IIS AspNetCoreModule* или *IIS AspNetCoreModule V2* в столбце **Источник**.</span><span class="sxs-lookup"><span data-stu-id="93e40-255">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="93e40-256">Вместо использования колонки **Диагностика и решение проблем** можно изучить файл журнала событий приложения непосредственно с помощью консоли [Kudu](https://github.com/projectkudu/kudu/wiki).</span><span class="sxs-lookup"><span data-stu-id="93e40-256">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="93e40-257">В области **Средства разработки** откройте колонку **Дополнительные инструменты**.</span><span class="sxs-lookup"><span data-stu-id="93e40-257">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="93e40-258">Нажмите кнопку **Перейти&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="93e40-258">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="93e40-259">Консоль Kudu открывается в новой вкладке или в окне браузера.</span><span class="sxs-lookup"><span data-stu-id="93e40-259">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="93e40-260">С помощью панели навигации в верхней части страницы откройте **Консоль отладки** и выберите **CMD**.</span><span class="sxs-lookup"><span data-stu-id="93e40-260">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="93e40-261">Откройте папку **LogFiles**.</span><span class="sxs-lookup"><span data-stu-id="93e40-261">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="93e40-262">Выберите значок с карандашом рядом с файлом *eventlog.xml*.</span><span class="sxs-lookup"><span data-stu-id="93e40-262">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="93e40-263">Изучите журнал.</span><span class="sxs-lookup"><span data-stu-id="93e40-263">Examine the log.</span></span> <span data-ttu-id="93e40-264">Прокрутите список до нижней части журнала, чтобы просмотреть последние события.</span><span class="sxs-lookup"><span data-stu-id="93e40-264">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="93e40-265">Запуск приложения в консоли Kudu</span><span class="sxs-lookup"><span data-stu-id="93e40-265">Run the app in the Kudu console</span></span>

<span data-ttu-id="93e40-266">Многие ошибки запуска не создают полезные сведения в журнале событий приложения.</span><span class="sxs-lookup"><span data-stu-id="93e40-266">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="93e40-267">Чтобы обнаружить ошибку, можно запустить приложение в удаленной консоли выполнения [Kudu](https://github.com/projectkudu/kudu/wiki).</span><span class="sxs-lookup"><span data-stu-id="93e40-267">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="93e40-268">В области **Средства разработки** откройте колонку **Дополнительные инструменты**.</span><span class="sxs-lookup"><span data-stu-id="93e40-268">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="93e40-269">Нажмите кнопку **Перейти&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="93e40-269">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="93e40-270">Консоль Kudu открывается в новой вкладке или в окне браузера.</span><span class="sxs-lookup"><span data-stu-id="93e40-270">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="93e40-271">С помощью панели навигации в верхней части страницы откройте **Консоль отладки** и выберите **CMD**.</span><span class="sxs-lookup"><span data-stu-id="93e40-271">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="93e40-272">Тестирование 32-разрядного (x86) приложения</span><span class="sxs-lookup"><span data-stu-id="93e40-272">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="93e40-273">**Текущий выпуск**</span><span class="sxs-lookup"><span data-stu-id="93e40-273">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="93e40-274">Запустите приложение:</span><span class="sxs-lookup"><span data-stu-id="93e40-274">Run the app:</span></span>
   * <span data-ttu-id="93e40-275">Если развертываемое приложение [зависит от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="93e40-275">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```dotnetcli
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="93e40-276">Если приложение [развертывается автономно](/dotnet/core/deploying/#self-contained-deployments-scd), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="93e40-276">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="93e40-277">Выходные данные из приложения, отображающие любые ошибки, будут выведены на консоль Kudu.</span><span class="sxs-lookup"><span data-stu-id="93e40-277">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="93e40-278">**Развертывание, зависимое от платформы, выполняющееся в предварительной версии**</span><span class="sxs-lookup"><span data-stu-id="93e40-278">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="93e40-279">*Требует установки расширения сайта для среды выполнения ASP.NET Core {VERSION} (x86).*</span><span class="sxs-lookup"><span data-stu-id="93e40-279">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="93e40-280">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` — это версия среды выполнения).</span><span class="sxs-lookup"><span data-stu-id="93e40-280">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="93e40-281">Запустите приложение: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`.</span><span class="sxs-lookup"><span data-stu-id="93e40-281">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="93e40-282">Выходные данные из приложения, отображающие любые ошибки, будут выведены на консоль Kudu.</span><span class="sxs-lookup"><span data-stu-id="93e40-282">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="93e40-283">Тестирование 64-разрядного (x64) приложения</span><span class="sxs-lookup"><span data-stu-id="93e40-283">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="93e40-284">**Текущий выпуск**</span><span class="sxs-lookup"><span data-stu-id="93e40-284">**Current release**</span></span>

* <span data-ttu-id="93e40-285">Если приложение является 64-разрядным (x64) [зависимым от платформы развертыванием](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="93e40-285">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="93e40-286">Запустите приложение: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`.</span><span class="sxs-lookup"><span data-stu-id="93e40-286">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="93e40-287">Если приложение [развертывается автономно](/dotnet/core/deploying/#self-contained-deployments-scd), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="93e40-287">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="93e40-288">Запустите приложение: `{ASSEMBLY NAME}.exe`.</span><span class="sxs-lookup"><span data-stu-id="93e40-288">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="93e40-289">Выходные данные из приложения, отображающие любые ошибки, будут выведены на консоль Kudu.</span><span class="sxs-lookup"><span data-stu-id="93e40-289">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="93e40-290">**Развертывание, зависимое от платформы, выполняющееся в предварительной версии**</span><span class="sxs-lookup"><span data-stu-id="93e40-290">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="93e40-291">*Требует установки расширения сайта для среды выполнения ASP.NET Core {VERSION} (x64).*</span><span class="sxs-lookup"><span data-stu-id="93e40-291">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="93e40-292">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` — это версия среды выполнения).</span><span class="sxs-lookup"><span data-stu-id="93e40-292">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="93e40-293">Запустите приложение: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`.</span><span class="sxs-lookup"><span data-stu-id="93e40-293">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="93e40-294">Выходные данные из приложения, отображающие любые ошибки, будут выведены на консоль Kudu.</span><span class="sxs-lookup"><span data-stu-id="93e40-294">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="93e40-295">Журнал ASP.NET Core модуля stdout (служба приложений Azure)</span><span class="sxs-lookup"><span data-stu-id="93e40-295">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="93e40-296">В журнале stdout модуля ASP.NET Core часто записываются полезные сообщения, которые не удается найти в журнале событий приложения.</span><span class="sxs-lookup"><span data-stu-id="93e40-296">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="93e40-297">Чтобы включить и просмотреть журналы stdout, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="93e40-297">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="93e40-298">Перейдите к колонке **Диагностика и решение проблем** на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="93e40-298">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="93e40-299">В разделе **Выберите категорию проблемы** нажмите кнопку **Веб-приложение не работает**.</span><span class="sxs-lookup"><span data-stu-id="93e40-299">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="93e40-300">В разделе **Предлагаемые решения** > **Включить перенаправление журнала stdout** нажмите кнопку **Открыть консоль Kudu для редактирования файла Web.Config**.</span><span class="sxs-lookup"><span data-stu-id="93e40-300">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="93e40-301">В **консоли диагностики** Kudu откройте папки на пути **веб-сайт** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="93e40-301">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="93e40-302">Прокрутите список вниз, чтобы в его нижней части показался файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="93e40-302">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="93e40-303">Щелкните значок карандаша рядом с файлом *web.config*.</span><span class="sxs-lookup"><span data-stu-id="93e40-303">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="93e40-304">Установите для параметра **stdoutLogEnabled** значение `true` и измените путь **stdoutLogFile** на `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="93e40-304">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="93e40-305">Выберите **Сохранить** для сохранения обновленного файла *web.config*.</span><span class="sxs-lookup"><span data-stu-id="93e40-305">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="93e40-306">Сделайте запрос к приложению.</span><span class="sxs-lookup"><span data-stu-id="93e40-306">Make a request to the app.</span></span>
1. <span data-ttu-id="93e40-307">Вернитесь на портал Azure.</span><span class="sxs-lookup"><span data-stu-id="93e40-307">Return to the Azure portal.</span></span> <span data-ttu-id="93e40-308">Выберите колонку **Дополнительные инструменты** в области **Средства разработки**.</span><span class="sxs-lookup"><span data-stu-id="93e40-308">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="93e40-309">Нажмите кнопку **Перейти&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="93e40-309">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="93e40-310">Консоль Kudu открывается в новой вкладке или в окне браузера.</span><span class="sxs-lookup"><span data-stu-id="93e40-310">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="93e40-311">С помощью панели навигации в верхней части страницы откройте **Консоль отладки** и выберите **CMD**.</span><span class="sxs-lookup"><span data-stu-id="93e40-311">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="93e40-312">Выберите папку **LogFiles**.</span><span class="sxs-lookup"><span data-stu-id="93e40-312">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="93e40-313">Изучите столбец **Изменено** и выберите значок карандаша, чтобы отредактировать журнал stdout для последней даты изменения.</span><span class="sxs-lookup"><span data-stu-id="93e40-313">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="93e40-314">При открытии файла журнала отображается сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="93e40-314">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="93e40-315">Завершив устранение неполадок, отключите ведение журнала stdout.</span><span class="sxs-lookup"><span data-stu-id="93e40-315">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="93e40-316">В **консоли диагностики** Kudu вернитесь к пути **веб-сайт** > **wwwroot**, чтобы открыть файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="93e40-316">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="93e40-317">Выбрав значок карандаша, снова откройте файл **web.config**.</span><span class="sxs-lookup"><span data-stu-id="93e40-317">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="93e40-318">Задайте параметру **stdoutLogEnabled** значение `false`.</span><span class="sxs-lookup"><span data-stu-id="93e40-318">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="93e40-319">Нажмите кнопку **Сохранить** для сохранения файла.</span><span class="sxs-lookup"><span data-stu-id="93e40-319">Select **Save** to save the file.</span></span>

<span data-ttu-id="93e40-320">Дополнительные сведения см. в разделе <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="93e40-320">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="93e40-321">Если вы не отключите журнал stdout, это может привести к сбоям приложения или сервера.</span><span class="sxs-lookup"><span data-stu-id="93e40-321">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="93e40-322">Ни размер файла журнала, ни количество создаваемых файлов журналов ничем не ограничены.</span><span class="sxs-lookup"><span data-stu-id="93e40-322">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="93e40-323">Для устранения неполадок при запуске приложения используйте только журнал stdout.</span><span class="sxs-lookup"><span data-stu-id="93e40-323">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="93e40-324">После запуска в приложении ASP.NET Core для ведения общего журнала используйте библиотеку ведения журналов, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов.</span><span class="sxs-lookup"><span data-stu-id="93e40-324">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="93e40-325">Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="93e40-325">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-azure-app-service"></a><span data-ttu-id="93e40-326">Журнал отладки модуля ASP.NET Core (служба приложений Azure)</span><span class="sxs-lookup"><span data-stu-id="93e40-326">ASP.NET Core Module debug log (Azure App Service)</span></span>

<span data-ttu-id="93e40-327">В журнал отладки модуля ASP.NET записываются дополнительные и более подробные сообщения, относящиеся к модулю ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93e40-327">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="93e40-328">Чтобы включить и просмотреть журналы stdout, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="93e40-328">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="93e40-329">Чтобы включить ведение расширенного журнала диагностики, выполните одно из следующих действий.</span><span class="sxs-lookup"><span data-stu-id="93e40-329">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="93e40-330">Следуйте инструкциям по настройке приложения для ведения расширенных журналов диагностики, приведенным в разделе [Расширенные журналы диагностики](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span><span class="sxs-lookup"><span data-stu-id="93e40-330">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="93e40-331">Повторно разверните приложение.</span><span class="sxs-lookup"><span data-stu-id="93e40-331">Redeploy the app.</span></span>
   * <span data-ttu-id="93e40-332">С помощью консоли Kudu добавьте раздел `<handlerSettings>` в файл [web.config](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) приложения, как показано в разделе *Расширенные журналы диагностики*.</span><span class="sxs-lookup"><span data-stu-id="93e40-332">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="93e40-333">В области **Средства разработки** откройте колонку **Дополнительные инструменты**.</span><span class="sxs-lookup"><span data-stu-id="93e40-333">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="93e40-334">Нажмите кнопку **Перейти&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="93e40-334">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="93e40-335">Консоль Kudu открывается в новой вкладке или в окне браузера.</span><span class="sxs-lookup"><span data-stu-id="93e40-335">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="93e40-336">С помощью панели навигации в верхней части страницы откройте **Консоль отладки** и выберите **CMD**.</span><span class="sxs-lookup"><span data-stu-id="93e40-336">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="93e40-337">Откройте папки на пути **веб-сайт** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="93e40-337">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="93e40-338">Нажмите кнопку с изображением карандаша и внесите изменения в файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="93e40-338">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="93e40-339">Добавьте раздел `<handlerSettings>`, как описано в разделе [Расширенные журналы диагностики](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span><span class="sxs-lookup"><span data-stu-id="93e40-339">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="93e40-340">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="93e40-340">Select the **Save** button.</span></span>
1. <span data-ttu-id="93e40-341">В области **Средства разработки** откройте колонку **Дополнительные инструменты**.</span><span class="sxs-lookup"><span data-stu-id="93e40-341">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="93e40-342">Нажмите кнопку **Перейти&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="93e40-342">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="93e40-343">Консоль Kudu открывается в новой вкладке или в окне браузера.</span><span class="sxs-lookup"><span data-stu-id="93e40-343">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="93e40-344">С помощью панели навигации в верхней части страницы откройте **Консоль отладки** и выберите **CMD**.</span><span class="sxs-lookup"><span data-stu-id="93e40-344">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="93e40-345">Откройте папки на пути **веб-сайт** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="93e40-345">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="93e40-346">Если вы не указали путь к файлу *aspnetcore-debug.log*, файл появится в списке.</span><span class="sxs-lookup"><span data-stu-id="93e40-346">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="93e40-347">Если путь указан, перейдите к расположению файла журнала.</span><span class="sxs-lookup"><span data-stu-id="93e40-347">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="93e40-348">Откройте файл журнала, нажав кнопку с изображением карандаша рядом с именем файла.</span><span class="sxs-lookup"><span data-stu-id="93e40-348">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="93e40-349">Завершив устранение неполадок, отключите ведение журнала отладки.</span><span class="sxs-lookup"><span data-stu-id="93e40-349">Disable debug logging when troubleshooting is complete:</span></span>

<span data-ttu-id="93e40-350">Чтобы отключить ведение расширенных журналов отладки, выполните одно из следующих действий:</span><span class="sxs-lookup"><span data-stu-id="93e40-350">To disable the enhanced debug log, perform either of the following:</span></span>

* <span data-ttu-id="93e40-351">Удалите раздел `<handlerSettings>` из файла *web.config* локально и повторно разверните приложение.</span><span class="sxs-lookup"><span data-stu-id="93e40-351">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
* <span data-ttu-id="93e40-352">С помощью консоли Kudu внесите изменения в файл *web.config* и удалите раздел `<handlerSettings>`.</span><span class="sxs-lookup"><span data-stu-id="93e40-352">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="93e40-353">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="93e40-353">Save the file.</span></span>

<span data-ttu-id="93e40-354">Дополнительные сведения см. в разделе <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="93e40-354">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

> [!WARNING]
> <span data-ttu-id="93e40-355">Ошибка при отключении журнала отладки может привести к сбоям приложения или сервера.</span><span class="sxs-lookup"><span data-stu-id="93e40-355">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="93e40-356">Ограничения на размер файла журнала отсутствуют.</span><span class="sxs-lookup"><span data-stu-id="93e40-356">There's no limit on log file size.</span></span> <span data-ttu-id="93e40-357">Для устранения неполадок при запуске приложения используйте только журнал отладки.</span><span class="sxs-lookup"><span data-stu-id="93e40-357">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="93e40-358">После запуска в приложении ASP.NET Core для ведения общего журнала используйте библиотеку ведения журналов, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов.</span><span class="sxs-lookup"><span data-stu-id="93e40-358">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="93e40-359">Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="93e40-359">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker-end

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="93e40-360">Приложение с задержкой или зависанием (служба приложений Azure)</span><span class="sxs-lookup"><span data-stu-id="93e40-360">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="93e40-361">Если приложение медленно реагирует на запрос или зависает при его обработке, см. следующие статьи:</span><span class="sxs-lookup"><span data-stu-id="93e40-361">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="93e40-362">Устранение проблем с производительностью медленных веб приложений в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="93e40-362">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* <span data-ttu-id="93e40-363">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App (Использование расширения сайта средства диагностики сбоев для записи дампа при периодических проблемах с исключением или проблемах с производительностью в веб-приложении Azure)](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/).</span><span class="sxs-lookup"><span data-stu-id="93e40-363">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)</span></span>

### <a name="monitoring-blades"></a><span data-ttu-id="93e40-364">Мониторинг колонок</span><span class="sxs-lookup"><span data-stu-id="93e40-364">Monitoring blades</span></span>

<span data-ttu-id="93e40-365">Мониторинг колонок обеспечивает альтернативный поиск неисправностей методам, описанным ранее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="93e40-365">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="93e40-366">Эти колонки можно использовать для диагностики ошибок 500-й серии.</span><span class="sxs-lookup"><span data-stu-id="93e40-366">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="93e40-367">Убедитесь, что установлены расширения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93e40-367">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="93e40-368">Если расширения не установлены, установите их вручную, выполнив следующие действия.</span><span class="sxs-lookup"><span data-stu-id="93e40-368">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="93e40-369">В колонке **Средства разработки** выберите колонку **Расширения**.</span><span class="sxs-lookup"><span data-stu-id="93e40-369">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="93e40-370">В списке должны появиться **Расширения ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="93e40-370">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="93e40-371">Если расширения не установлены, нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="93e40-371">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="93e40-372">Выберите из списка **Расширения ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="93e40-372">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="93e40-373">Для принятия условий нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="93e40-373">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="93e40-374">Нажмите кнопку **ОК** в колонке **Добавить расширение**.</span><span class="sxs-lookup"><span data-stu-id="93e40-374">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="93e40-375">Информационное всплывающее сообщение указывает, когда расширения успешно установятся.</span><span class="sxs-lookup"><span data-stu-id="93e40-375">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="93e40-376">Если ведение журнала stdout не включено, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="93e40-376">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="93e40-377">На портале Azure выберите колонку **Дополнительные инструменты** в области **Средства разработки**.</span><span class="sxs-lookup"><span data-stu-id="93e40-377">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="93e40-378">Нажмите кнопку **Перейти&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="93e40-378">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="93e40-379">Консоль Kudu открывается в новой вкладке или в окне браузера.</span><span class="sxs-lookup"><span data-stu-id="93e40-379">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="93e40-380">С помощью панели навигации в верхней части страницы откройте **Консоль отладки** и выберите **CMD**.</span><span class="sxs-lookup"><span data-stu-id="93e40-380">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="93e40-381">Откройте папки на пути **веб-сайт** > **wwwroot** и прокрутите список вниз, пока в нижней части не покажется файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="93e40-381">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="93e40-382">Щелкните значок карандаша рядом с файлом *web.config*.</span><span class="sxs-lookup"><span data-stu-id="93e40-382">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="93e40-383">Установите для параметра **stdoutLogEnabled** значение `true` и измените путь **stdoutLogFile** на `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="93e40-383">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="93e40-384">Выберите **Сохранить** для сохранения обновленного файла *web.config*.</span><span class="sxs-lookup"><span data-stu-id="93e40-384">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="93e40-385">Перейдите к активации ведения журнала диагностики, выполнив следующие действия.</span><span class="sxs-lookup"><span data-stu-id="93e40-385">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="93e40-386">На портале Azure выберите колонку **Журналы диагностики**.</span><span class="sxs-lookup"><span data-stu-id="93e40-386">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="93e40-387">Выберите положение переключателя **Вкл.** для **Вход в приложения (файловая система)** и **Подробные сообщения об ошибках**.</span><span class="sxs-lookup"><span data-stu-id="93e40-387">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="93e40-388">В верхней части колонки нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="93e40-388">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="93e40-389">Чтобы включить трассировку неудачно завершенных запросов, также известную как ведение журнала буферизации событий неудачных запросов (FREB), установите положение переключателя **Вкл.** для **Трассировки неудачно завершенных запросов**.</span><span class="sxs-lookup"><span data-stu-id="93e40-389">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="93e40-390">На портале выберите колонку **Поток журнала**, которая в списке отображается сразу под колонкой **Журналы диагностики**.</span><span class="sxs-lookup"><span data-stu-id="93e40-390">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="93e40-391">Сделайте запрос к приложению.</span><span class="sxs-lookup"><span data-stu-id="93e40-391">Make a request to the app.</span></span>
1. <span data-ttu-id="93e40-392">В пределах данных потока журнала указывается причина ошибки.</span><span class="sxs-lookup"><span data-stu-id="93e40-392">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="93e40-393">После завершения устранения неполадок обязательно отключите ведение журнала stdout.</span><span class="sxs-lookup"><span data-stu-id="93e40-393">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="93e40-394">Чтобы просмотреть журналы трассировки неудачно завершенных запросов (журналы FREB), выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="93e40-394">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="93e40-395">Перейдите к колонке **Диагностика и решение проблем** на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="93e40-395">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="93e40-396">Выберите **Журналы трассировки неудачно завершенных запросов** из области боковой панели **Средства поддержки**.</span><span class="sxs-lookup"><span data-stu-id="93e40-396">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="93e40-397">Для получения дополнительной информации см. [Раздел "Трассировка неудачно завершенных запросов" раздела "Включение ведения журналов диагностики для веб-приложений в службе приложений Azure"](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) и [Вопросы и ответы о производительности приложений для веб-приложений в Azure: как включить трассировку неудачно завершенных запросов?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing).</span><span class="sxs-lookup"><span data-stu-id="93e40-397">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="93e40-398">Дополнительные сведения см. в разделе [Включение функции ведения журналов диагностики для веб-приложений в службе приложений Azure](/azure/app-service/web-sites-enable-diagnostic-log).</span><span class="sxs-lookup"><span data-stu-id="93e40-398">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="93e40-399">Если вы не отключите журнал stdout, это может привести к сбоям приложения или сервера.</span><span class="sxs-lookup"><span data-stu-id="93e40-399">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="93e40-400">Ни размер файла журнала, ни количество создаваемых файлов журналов ничем не ограничены.</span><span class="sxs-lookup"><span data-stu-id="93e40-400">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="93e40-401">Для обычного ведения журналов для приложения ASP.NET Core используйте библиотеку, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов.</span><span class="sxs-lookup"><span data-stu-id="93e40-401">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="93e40-402">Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="93e40-402">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="93e40-403">Устранение неполадок в службах IIS</span><span class="sxs-lookup"><span data-stu-id="93e40-403">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="93e40-404">Журнал событий приложений (IIS)</span><span class="sxs-lookup"><span data-stu-id="93e40-404">Application Event Log (IIS)</span></span>

<span data-ttu-id="93e40-405">Доступ к журналу событий приложений</span><span class="sxs-lookup"><span data-stu-id="93e40-405">Access the Application Event Log:</span></span>

1. <span data-ttu-id="93e40-406">Откройте меню "Пуск", выполните поиск по фразе **Просмотр событий** и выберите приложение **Просмотр событий**.</span><span class="sxs-lookup"><span data-stu-id="93e40-406">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="93e40-407">В **средстве просмотра событий** откройте узел **Журналы Windows**.</span><span class="sxs-lookup"><span data-stu-id="93e40-407">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="93e40-408">Выберите **Приложение**, чтобы открыть журнал событий приложения.</span><span class="sxs-lookup"><span data-stu-id="93e40-408">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="93e40-409">Проверьте здесь наличие ошибок, связанных с проблемным приложением.</span><span class="sxs-lookup"><span data-stu-id="93e40-409">Search for errors associated with the failing app.</span></span> <span data-ttu-id="93e40-410">Нам нужны ошибки со значениями *Модуль IIS AspNetCore* или *Модуль IIS Express AspNetCore* в столбце *Источник*.</span><span class="sxs-lookup"><span data-stu-id="93e40-410">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="93e40-411">Запуск приложения в командной строке</span><span class="sxs-lookup"><span data-stu-id="93e40-411">Run the app at a command prompt</span></span>

<span data-ttu-id="93e40-412">Многие ошибки запуска не создают полезные сведения в журнале событий приложения.</span><span class="sxs-lookup"><span data-stu-id="93e40-412">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="93e40-413">Для некоторых ошибок можно найти причину, запустив приложение в командной строке на компьютере размещения.</span><span class="sxs-lookup"><span data-stu-id="93e40-413">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="93e40-414">развертывание, зависящее от платформы;</span><span class="sxs-lookup"><span data-stu-id="93e40-414">Framework-dependent deployment</span></span>

<span data-ttu-id="93e40-415">Если развертываемое приложение [зависит от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="93e40-415">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="93e40-416">В командной строке перейдите в папку развертывания и запустите приложение, выполнив команду *dotnet.exe* для сборки приложения.</span><span class="sxs-lookup"><span data-stu-id="93e40-416">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="93e40-417">В следующей команде укажите нужное имя сборки вместо заполнителя \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="93e40-417">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="93e40-418">Выходные данные приложения, в том числе любые ошибки, будут выведены в окно консоли.</span><span class="sxs-lookup"><span data-stu-id="93e40-418">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="93e40-419">Если ошибки возникают при выполнении запроса к приложению, выполните запрос к узлу и порту, где Kestrel ожидает передачи данных.</span><span class="sxs-lookup"><span data-stu-id="93e40-419">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="93e40-420">Создайте запрос к `http://localhost:5000/`, используя узел и порт по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="93e40-420">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="93e40-421">Если приложение отвечает на запросы по адресу конечной точки Kestrel как обычно, то проблема, вероятнее, связана с конфигурацией размещения, а не с самим приложением.</span><span class="sxs-lookup"><span data-stu-id="93e40-421">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="93e40-422">автономное развертывание;</span><span class="sxs-lookup"><span data-stu-id="93e40-422">Self-contained deployment</span></span>

<span data-ttu-id="93e40-423">Если приложение [развертывается автономно](/dotnet/core/deploying/#self-contained-deployments-scd), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="93e40-423">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="93e40-424">В командной строке перейдите в папку развертывания и запустите исполняемый файл приложения.</span><span class="sxs-lookup"><span data-stu-id="93e40-424">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="93e40-425">В следующей команде укажите нужное имя сборки вместо заполнителя \<assembly_name>: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="93e40-425">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="93e40-426">Выходные данные приложения, в том числе любые ошибки, будут выведены в окно консоли.</span><span class="sxs-lookup"><span data-stu-id="93e40-426">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="93e40-427">Если ошибки возникают при выполнении запроса к приложению, выполните запрос к узлу и порту, где Kestrel ожидает передачи данных.</span><span class="sxs-lookup"><span data-stu-id="93e40-427">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="93e40-428">Создайте запрос к `http://localhost:5000/`, используя узел и порт по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="93e40-428">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="93e40-429">Если приложение отвечает на запросы по адресу конечной точки Kestrel как обычно, то проблема, вероятнее, связана с конфигурацией размещения, а не с самим приложением.</span><span class="sxs-lookup"><span data-stu-id="93e40-429">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="93e40-430">Журнал stdout модуля ASP.NET Core (IIS)</span><span class="sxs-lookup"><span data-stu-id="93e40-430">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="93e40-431">Чтобы включить и просмотреть журналы stdout, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="93e40-431">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="93e40-432">Перейдите в папку развертывания сайта на компьютере размещения.</span><span class="sxs-lookup"><span data-stu-id="93e40-432">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="93e40-433">Если здесь нет папки *logs*, создайте ее.</span><span class="sxs-lookup"><span data-stu-id="93e40-433">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="93e40-434">Сведения о том, как настроить в MSBuild автоматическое создание папки *logs* в развертывании, см. в статье [о структуре каталогов](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="93e40-434">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="93e40-435">Измените файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="93e40-435">Edit the *web.config* file.</span></span> <span data-ttu-id="93e40-436">Задайте для параметра **stdoutLogEnabled** значение `true` и измените путь **stdoutLogFile** так, чтобы он указывал на папку *logs* (например, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="93e40-436">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="93e40-437">В этом пути `stdout` обозначает префикс имени для файла журнала.</span><span class="sxs-lookup"><span data-stu-id="93e40-437">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="93e40-438">При создании файла журнала автоматически добавляются метка времени, идентификатор процесса и расширение файла.</span><span class="sxs-lookup"><span data-stu-id="93e40-438">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="93e40-439">Если указан префикс файла `stdout`, стандартный файл журнала будет иметь примерно такое имя: *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="93e40-439">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="93e40-440">Убедитесь, что удостоверение пула приложений имеет разрешения на запись в папку *logs*.</span><span class="sxs-lookup"><span data-stu-id="93e40-440">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="93e40-441">Сохраните обновленный файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="93e40-441">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="93e40-442">Сделайте запрос к приложению.</span><span class="sxs-lookup"><span data-stu-id="93e40-442">Make a request to the app.</span></span>
1. <span data-ttu-id="93e40-443">Перейдите в папку *logs*.</span><span class="sxs-lookup"><span data-stu-id="93e40-443">Navigate to the *logs* folder.</span></span> <span data-ttu-id="93e40-444">Найдите и откройте последний журнал вывода stdout.</span><span class="sxs-lookup"><span data-stu-id="93e40-444">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="93e40-445">Проверьте, нет ли в нем ошибок.</span><span class="sxs-lookup"><span data-stu-id="93e40-445">Study the log for errors.</span></span>

<span data-ttu-id="93e40-446">Завершив устранение неполадок, отключите ведение журнала stdout.</span><span class="sxs-lookup"><span data-stu-id="93e40-446">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="93e40-447">Измените файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="93e40-447">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="93e40-448">Задайте параметру **stdoutLogEnabled** значение `false`.</span><span class="sxs-lookup"><span data-stu-id="93e40-448">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="93e40-449">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="93e40-449">Save the file.</span></span>

<span data-ttu-id="93e40-450">Дополнительные сведения см. в разделе <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="93e40-450">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="93e40-451">Если вы не отключите журнал stdout, это может привести к сбоям приложения или сервера.</span><span class="sxs-lookup"><span data-stu-id="93e40-451">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="93e40-452">Ни размер файла журнала, ни количество создаваемых файлов журналов ничем не ограничены.</span><span class="sxs-lookup"><span data-stu-id="93e40-452">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="93e40-453">Для обычного ведения журналов для приложения ASP.NET Core используйте библиотеку, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов.</span><span class="sxs-lookup"><span data-stu-id="93e40-453">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="93e40-454">Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="93e40-454">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-iis"></a><span data-ttu-id="93e40-455">Журнал отладки модуля ASP.NET Core (IIS)</span><span class="sxs-lookup"><span data-stu-id="93e40-455">ASP.NET Core Module debug log (IIS)</span></span>

<span data-ttu-id="93e40-456">Добавьте следующие параметры обработчика в файл *Web. config* приложения, чтобы включить ASP.NET Core журнал отладки модуля:</span><span class="sxs-lookup"><span data-stu-id="93e40-456">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug log:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="93e40-457">Убедитесь, что указанный для журнала путь существует и удостоверение пула приложений имеет разрешения на запись в это расположение.</span><span class="sxs-lookup"><span data-stu-id="93e40-457">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

<span data-ttu-id="93e40-458">Дополнительные сведения см. в разделе <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="93e40-458">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

::: moniker-end

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="93e40-459">Включение страницы исключений для разработчика</span><span class="sxs-lookup"><span data-stu-id="93e40-459">Enable the Developer Exception Page</span></span>

<span data-ttu-id="93e40-460">Можно добавить переменную среды `ASPNETCORE_ENVIRONMENT` [в файл web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables), чтобы запустить приложение в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="93e40-460">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="93e40-461">Если среда не переопределяется при запуске приложения использованием `UseEnvironment` в конструкторе узла, эта переменная среды позволяет отображать [страницу исключения для разработчика](xref:fundamentals/error-handling) при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="93e40-461">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="93e40-462">Настройка переменной среды `ASPNETCORE_ENVIRONMENT` рекомендуется только на промежуточных и тестовых серверах, доступ к которым из Интернета закрыт.</span><span class="sxs-lookup"><span data-stu-id="93e40-462">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="93e40-463">Завершив устранение неполадок, удалите эту переменную среды из файла *web.config*.</span><span class="sxs-lookup"><span data-stu-id="93e40-463">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="93e40-464">Сведения о настройке переменных среды в файле *web.config* см. в статье о [дочернем элементе environmentVariables в aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="93e40-464">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="93e40-465">Получение данных из приложения</span><span class="sxs-lookup"><span data-stu-id="93e40-465">Obtain data from an app</span></span>

<span data-ttu-id="93e40-466">Если приложение способно отвечать на запросы, получите данные о запросе, подключении и дополнительные данные из приложений с помощью встроенного терминала ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="93e40-466">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="93e40-467">Дополнительные сведения и примеры с кодом см. здесь: <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="93e40-467">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="93e40-468">Низкое или зависание приложения (IIS)</span><span class="sxs-lookup"><span data-stu-id="93e40-468">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="93e40-469">*Аварийный дамп* — это моментальный снимок памяти системы, который может помочь определить причину сбоя приложения, сбоя при запуске или медленных приложений.</span><span class="sxs-lookup"><span data-stu-id="93e40-469">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="93e40-470">Аварийное завершение работы приложения или исключение</span><span class="sxs-lookup"><span data-stu-id="93e40-470">App crashes or encounters an exception</span></span>

<span data-ttu-id="93e40-471">Получите и проанализируйте дамп из [отчетов об ошибках Windows (WER)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="93e40-471">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="93e40-472">Создайте папку для хранения файлов аварийного дампа в `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="93e40-472">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="93e40-473">Пул приложений должен иметь доступ на запись к папке.</span><span class="sxs-lookup"><span data-stu-id="93e40-473">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="93e40-474">Запустите [скрипт PowerShell EnableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="93e40-474">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="93e40-475">Если приложение использует [модель размещения в процессе](xref:host-and-deploy/iis/index#in-process-hosting-model), выполните скрипт для *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="93e40-475">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="93e40-476">Если приложение использует [модель размещения вне процесса](xref:host-and-deploy/iis/index#out-of-process-hosting-model), выполните скрипт для *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="93e40-476">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="93e40-477">Запустите приложение в условиях, вызывающих аварийное завершение.</span><span class="sxs-lookup"><span data-stu-id="93e40-477">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="93e40-478">После аварийного завершения запустите [скрипт PowerShell DisableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="93e40-478">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="93e40-479">Если приложение использует [модель размещения в процессе](xref:host-and-deploy/iis/index#in-process-hosting-model), выполните скрипт для *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="93e40-479">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="93e40-480">Если приложение использует [модель размещения вне процесса](xref:host-and-deploy/iis/index#out-of-process-hosting-model), выполните скрипт для *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="93e40-480">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="93e40-481">Когда приложение аварийно завершит работу и сбор дампов будет выполнен, приложение сможет завершить работу обычным образом.</span><span class="sxs-lookup"><span data-stu-id="93e40-481">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="93e40-482">Скрипт PowerShell настраивает отчеты об ошибках Windows для сбора до пяти дампов для приложения.</span><span class="sxs-lookup"><span data-stu-id="93e40-482">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="93e40-483">Аварийные дампы могут занимать много места на диске (до нескольких гигабайтов каждый).</span><span class="sxs-lookup"><span data-stu-id="93e40-483">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="93e40-484">Приложение перестает отвечать на запросы, не запускается или работает в обычном режиме</span><span class="sxs-lookup"><span data-stu-id="93e40-484">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="93e40-485">Если приложение *зависает* (перестает отвечать, но не работает), завершается ошибкой во время запуска или обычно выполняется, см. раздел [файлы дампа пользовательского режима: выбор оптимального средства](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) для создания дампа.</span><span class="sxs-lookup"><span data-stu-id="93e40-485">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="93e40-486">Анализ дампа</span><span class="sxs-lookup"><span data-stu-id="93e40-486">Analyze the dump</span></span>

<span data-ttu-id="93e40-487">Дамп можно проанализировать несколькими способами.</span><span class="sxs-lookup"><span data-stu-id="93e40-487">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="93e40-488">Дополнительные сведения см. в разделе [Анализ файла дампа пользовательского режима](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="93e40-488">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="93e40-489">Очистить кэши пакетов</span><span class="sxs-lookup"><span data-stu-id="93e40-489">Clear package caches</span></span>

<span data-ttu-id="93e40-490">Иногда работающее приложение завершается сбоем сразу после обновления пакет SDK для .NET Core на компьютере разработки или при изменении версий пакета в приложении.</span><span class="sxs-lookup"><span data-stu-id="93e40-490">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="93e40-491">В некоторых случаях в результате важного обновления несогласованные версии пакетов могут привести к нарушению работы приложения.</span><span class="sxs-lookup"><span data-stu-id="93e40-491">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="93e40-492">Большинство этих проблем можно исправить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="93e40-492">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="93e40-493">Удалите папки *bin* и *obj*.</span><span class="sxs-lookup"><span data-stu-id="93e40-493">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="93e40-494">Очистите кэши пакетов, выполнив `dotnet nuget locals all --clear` из командной оболочки.</span><span class="sxs-lookup"><span data-stu-id="93e40-494">Clear the package caches by executing `dotnet nuget locals all --clear` from a command shell.</span></span>

   <span data-ttu-id="93e40-495">Очистка кэшей пакетов также может быть выполнена с помощью средства [NuGet. exe](https://www.nuget.org/downloads) и выполнения команды `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="93e40-495">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="93e40-496">*NuGet.exe* не входит в пакет установки операционной системы Windows для настольных компьютеров и его нужно получить отдельно на [веб-сайте NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="93e40-496">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="93e40-497">Восстановите и перестройте проект.</span><span class="sxs-lookup"><span data-stu-id="93e40-497">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="93e40-498">Удалите все файлы из папки развертывания на сервере перед повторным развертыванием приложения.</span><span class="sxs-lookup"><span data-stu-id="93e40-498">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="93e40-499">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="93e40-499">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="93e40-500">Документация по Azure</span><span class="sxs-lookup"><span data-stu-id="93e40-500">Azure documentation</span></span>

* [<span data-ttu-id="93e40-501">Application Insights для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="93e40-501">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="93e40-502">Раздел "Удаленная отладка веб-приложений" статьи Устранение неполадок веб-приложения в службе приложений Azure с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93e40-502">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="93e40-503">Общие сведения о диагностике в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="93e40-503">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="93e40-504">Мониторинг приложений в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="93e40-504">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="93e40-505">Устранение неполадок веб-приложения в службе приложений Azure с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93e40-505">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="93e40-506">Устранение ошибок HTTP "502 — недопустимый шлюз" и "503 — служба недоступна" в веб-приложениях Azure</span><span class="sxs-lookup"><span data-stu-id="93e40-506">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="93e40-507">Устранение проблем с производительностью медленных веб приложений в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="93e40-507">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="93e40-508">Вопросы и ответы о производительности приложений для веб-приложений в Azure</span><span class="sxs-lookup"><span data-stu-id="93e40-508">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [<span data-ttu-id="93e40-509">"Песочница" веб-приложений Azure (ограничения работы среды выполнения службы приложений)</span><span class="sxs-lookup"><span data-stu-id="93e40-509">Azure Web App sandbox (App Service runtime execution limitations)</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [<span data-ttu-id="93e40-510">Azure, пятница: практики диагностики и устранения неполадок в службе приложений Azure (12-минутное видео)</span><span class="sxs-lookup"><span data-stu-id="93e40-510">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a><span data-ttu-id="93e40-511">Документация по Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93e40-511">Visual Studio documentation</span></span>

* [<span data-ttu-id="93e40-512">Удаленная отладка ASP.NET Core на IIS в Azure в Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="93e40-512">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="93e40-513">Удаленная отладка ASP.NET Core на удаленном компьютере IIS в Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="93e40-513">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="93e40-514">Сведения об отладке с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93e40-514">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="93e40-515">Документация по Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="93e40-515">Visual Studio Code documentation</span></span>

* [<span data-ttu-id="93e40-516">Отладка с помощью Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="93e40-516">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)
