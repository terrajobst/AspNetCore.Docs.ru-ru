---
title: Общий Справочник ошибок для службы приложений Azure и IIS с ASP.NET Core
author: guardrex
description: Следует отличать распространенных ошибок при размещении приложения ASP.NET Core на службы приложений Azure и служб IIS.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 1500f026c245f80de4120d6db4901cb117552966
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2018
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="4e2f4-103">Общий Справочник ошибок для службы приложений Azure и IIS с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4e2f4-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="4e2f4-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="4e2f4-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4e2f4-105">Приведенный ниже перечень ошибок не является исчерпывающим.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-105">The following isn't a complete list of errors.</span></span> <span data-ttu-id="4e2f4-106">Если возникнет сообщение об ошибке, не перечисленных здесь, [открыть новый выпуск](https://github.com/aspnet/Docs/issues/new) подробные инструкции по воспроизведению ошибки.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-106">If you encounter an error not listed here, [open a new issue](https://github.com/aspnet/Docs/issues/new) with detailed instructions to reproduce the error.</span></span>

<span data-ttu-id="4e2f4-107">Соберите следующие сведения:</span><span class="sxs-lookup"><span data-stu-id="4e2f4-107">Collect the following information:</span></span>

* <span data-ttu-id="4e2f4-108">Поведение обозревателя</span><span class="sxs-lookup"><span data-stu-id="4e2f4-108">Browser behavior</span></span>
* <span data-ttu-id="4e2f4-109">Записи журнала событий приложения</span><span class="sxs-lookup"><span data-stu-id="4e2f4-109">Application Event Log entries</span></span>
* <span data-ttu-id="4e2f4-110">Записи журнала stdout модуль Core ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4e2f4-110">ASP.NET Core Module stdout log entries</span></span>

<span data-ttu-id="4e2f4-111">Сравните данными следующих распространенных ошибок.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-111">Compare the information to the following common errors.</span></span> <span data-ttu-id="4e2f4-112">Если соответствие найдено, следуйте советов по устранению неполадок.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-112">If a match is found, follow the troubleshooting advice.</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="4e2f4-113">Установщику не удается получить распространяемый компонент VC++</span><span class="sxs-lookup"><span data-stu-id="4e2f4-113">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="4e2f4-114">**Исключение установщика:** 0x80072efd or 0x80072f76 — неопределенная ошибка</span><span class="sxs-lookup"><span data-stu-id="4e2f4-114">**Installer Exception:** 0x80072efd or 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="4e2f4-115">**Исключение журнала установщика&#8224;:** ошибка 0x80072efd или 0x80072f76: не удалось выполнить пакет EXE</span><span class="sxs-lookup"><span data-stu-id="4e2f4-115">**Installer Log Exception&#8224;:** Error 0x80072efd or 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="4e2f4-116">&#8224;Журнал находится в папке C:\Users\\{ПОЛЬЗОВАТЕЛЬ}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{метка_времени}.log.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-116">&#8224;The log is located at C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.</span></span>

<span data-ttu-id="4e2f4-117">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="4e2f4-117">Troubleshooting:</span></span>

* <span data-ttu-id="4e2f4-118">Если система не имеет доступа к Интернету при установке пакета размещения, это исключение возникает, когда программа установки будет запрещено получение *Visual C++ 2015 распространяемый компонент Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-118">If the system doesn't have Internet access while installing the Hosting Bundle, this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="4e2f4-119">Получить установщика из [центра загрузки Майкрософт](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="4e2f4-119">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="4e2f4-120">Если программа установки завершается ошибкой, сервер может не получить среды выполнения .NET Core, необходимые для размещения развертывания зависит от framework (дискеты).</span><span class="sxs-lookup"><span data-stu-id="4e2f4-120">If the installer fails, the server may not receive the .NET Core runtime required to host a framework-dependent deployment (FDD).</span></span> <span data-ttu-id="4e2f4-121">Если размещение системой защиты Фиксированных, убедитесь, что среда выполнения установлен в программах &amp; функции.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-121">If hosting an FDD, confirm that the runtime is installed in Programs &amp; Features.</span></span> <span data-ttu-id="4e2f4-122">В случае необходимости получения установщика среды выполнения из [всех загрузок .NET](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="4e2f4-122">If needed, obtain a runtime installer from [.NET All Downloads](https://www.microsoft.com/net/download/all).</span></span> <span data-ttu-id="4e2f4-123">После установки среды выполнения перезагрузите систему или перезапустите службы IIS, выполнив в командной строке команду **net stop was /y**, а затем — команду **net start w3svc**.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-123">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="4e2f4-124">При обновлении ОС был удален 32-разрядный модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4e2f4-124">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

* <span data-ttu-id="4e2f4-125">**Журнал приложений:** не удалось загрузить модуль DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-125">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="4e2f4-126">Ошибочные данные.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-126">The data is the error.</span></span>

<span data-ttu-id="4e2f4-127">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="4e2f4-127">Troubleshooting:</span></span>

* <span data-ttu-id="4e2f4-128">Не относящиеся к ОС файлы в каталоге **C:\Windows\SysWOW64\inetsrv** не сохраняются при обновлении ОС.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-128">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="4e2f4-129">Если модуль ASP.NET Core установлена до обновления операционной системы и затем любой пул приложений запускается в 32-разрядном режиме после обновления операционной системы, при возникновении этой проблемы.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-129">If the ASP.NET Core Module is installed prior to an OS upgrade and then any AppPool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="4e2f4-130">После обновления ОС восстановите модуль ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-130">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="4e2f4-131">В разделе [установки пакета .NET Core размещение](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="4e2f4-131">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="4e2f4-132">Выберите **восстановления** при запуске программы установки.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-132">Select **Repair** when the installer is run.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="4e2f4-133">Конфликты платформы с относительным идентификатором</span><span class="sxs-lookup"><span data-stu-id="4e2f4-133">Platform conflicts with RID</span></span>

* <span data-ttu-id="4e2f4-134">**Браузер:** ошибка HTTP 502.5 — сбой процесса</span><span class="sxs-lookup"><span data-stu-id="4e2f4-134">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="4e2f4-135">**Журнал приложений:** приложения "MACHINE/WEBROOT/APPHOST / {сборки}" с Физический корень "C:\{путь}\' не удалось запустить процесс с помощью командной строки" "C:\\{путь} {сборки}. {} exe | библиотека dll}» ", код ошибки =" 0x80004005: ff.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-135">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\\{PATH}{assembly}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="4e2f4-136">**Журнал модуля ASP.NET Core:** необработанное исключение: System.BadImageFormatException: не удалось загрузить файл или сборку '{сборки} .dll».</span><span class="sxs-lookup"><span data-stu-id="4e2f4-136">**ASP.NET Core Module Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{assembly}.dll'.</span></span> <span data-ttu-id="4e2f4-137">Была сделана попытка загрузить программу, имеющую неверный формат.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-137">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="4e2f4-138">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="4e2f4-138">Troubleshooting:</span></span>

* <span data-ttu-id="4e2f4-139">Убедитесь в том, что приложение выполняется локально в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-139">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="4e2f4-140">Сбой процесса может быть результатом проблемы в приложении.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-140">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="4e2f4-141">Дополнительные сведения см. в разделе [Устранение неполадок](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="4e2f4-141">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="4e2f4-142">Убедитесь, что `<PlatformTarget>` в *.csproj* не противоречит RID.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-142">Confirm that the `<PlatformTarget>` in the *.csproj* doesn't conflict with the RID.</span></span> <span data-ttu-id="4e2f4-143">Например, не указывайте `<PlatformTarget>` из `x86` и публикации с RID `win10-x64`, либо с помощью *dotnet публикации - c - r выпуска Windows 10-x64* или установив `<RuntimeIdentifiers>` в *CSPROJ-файл*  для `win10-x64`.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-143">For example, don't specify a `<PlatformTarget>` of `x86` and publish with an RID of `win10-x64`, either by using *dotnet publish -c Release -r win10-x64* or by setting the `<RuntimeIdentifiers>` in the *.csproj* to `win10-x64`.</span></span> <span data-ttu-id="4e2f4-144">Проект публикуется без предупреждения или сообщения об ошибке, но при запуске в системе происходит сбой, а в журнале регистрируются указанные выше исключения.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-144">The project publishes without warning or error but fails with the above logged exceptions on the system.</span></span>

* <span data-ttu-id="4e2f4-145">Если это исключение возникает по развертыванию приложения Azure, при обновлении приложения и развертывания новой сборки вручную удалить все файлы из предыдущего развертывания.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-145">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="4e2f4-146">Если останутся несовместимые сборки, то при развертывании обновленного приложения это может привести к исключению `System.BadImageFormatException`.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-146">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="4e2f4-147">Неправильная конечная точка URI, либо веб-сайт остановлен</span><span class="sxs-lookup"><span data-stu-id="4e2f4-147">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="4e2f4-148">**Браузер:** ERR_CONNECTION_REFUSED</span><span class="sxs-lookup"><span data-stu-id="4e2f4-148">**Browser:** ERR_CONNECTION_REFUSED</span></span>

* <span data-ttu-id="4e2f4-149">**Журнал приложений:** нет записи</span><span class="sxs-lookup"><span data-stu-id="4e2f4-149">**Application Log:** No entry</span></span>

* <span data-ttu-id="4e2f4-150">**Журнал модуля ASP.NET Core:** файл журнала не создан</span><span class="sxs-lookup"><span data-stu-id="4e2f4-150">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="4e2f4-151">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="4e2f4-151">Troubleshooting:</span></span>

* <span data-ttu-id="4e2f4-152">Убедитесь, что используется правильный URI конечной точки для приложения.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-152">Confirm the correct URI endpoint for the app is being used.</span></span> <span data-ttu-id="4e2f4-153">Проверьте привязки.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-153">Check the bindings.</span></span>

* <span data-ttu-id="4e2f4-154">Убедитесь, что веб-сайт IIS не в *остановлена* состояния.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-154">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="4e2f4-155">Компонент сервера CoreWebEngine или W3SVC отключен</span><span class="sxs-lookup"><span data-stu-id="4e2f4-155">CoreWebEngine or W3SVC server features disabled</span></span>

* <span data-ttu-id="4e2f4-156">**Исключение ОС:** для использования модуля ASP.NET Core должны быть установлены компоненты IIS 7.0 CoreWebEngine и W3SVC.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-156">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="4e2f4-157">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="4e2f4-157">Troubleshooting:</span></span>

* <span data-ttu-id="4e2f4-158">Убедитесь, что включены соответствующие роли и компоненты.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-158">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="4e2f4-159">См. раздел [Конфигурация IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="4e2f4-159">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="4e2f4-160">Физический путь неверный веб-сайта или приложения отсутствует</span><span class="sxs-lookup"><span data-stu-id="4e2f4-160">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="4e2f4-161">**Браузер:** 403 — запрещено. В доступе отказано **--ИЛИ--** 403.14 — запрещено. Веб-сервер настроен таким образом, чтобы не формировать список содержимого каталога.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-161">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="4e2f4-162">**Журнал приложений:** нет записи</span><span class="sxs-lookup"><span data-stu-id="4e2f4-162">**Application Log:** No entry</span></span>

* <span data-ttu-id="4e2f4-163">**Журнал модуля ASP.NET Core:** файл журнала не создан</span><span class="sxs-lookup"><span data-stu-id="4e2f4-163">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="4e2f4-164">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="4e2f4-164">Troubleshooting:</span></span>

* <span data-ttu-id="4e2f4-165">Проверьте веб-сайт IIS **основные параметры** и папка физического приложения.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-165">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="4e2f4-166">Убедитесь, что приложение в папку на веб-сайте IIS **физический путь**.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-166">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="4e2f4-167">Неправильная роль, модуль не установлен либо неверные разрешения</span><span class="sxs-lookup"><span data-stu-id="4e2f4-167">Incorrect role, module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="4e2f4-168">**Браузер:** 500.19 — внутренняя ошибка сервера. Запрашиваемая страница недоступна из-за неверной конфигурации данных для этой страницы.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-168">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span>

* <span data-ttu-id="4e2f4-169">**Журнал приложений:** нет записи</span><span class="sxs-lookup"><span data-stu-id="4e2f4-169">**Application Log:** No entry</span></span>

* <span data-ttu-id="4e2f4-170">**Журнал модуля ASP.NET Core:** файл журнала не создан</span><span class="sxs-lookup"><span data-stu-id="4e2f4-170">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="4e2f4-171">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="4e2f4-171">Troubleshooting:</span></span>

* <span data-ttu-id="4e2f4-172">Убедитесь, что включена ролью.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-172">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="4e2f4-173">См. раздел [Конфигурация IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="4e2f4-173">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="4e2f4-174">На странице **Программы и компоненты** убедитесь в том, что установлен **модуль Microsoft ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-174">Check **Programs &amp; Features** and confirm that the **Microsoft ASP.NET Core Module** has been installed.</span></span> <span data-ttu-id="4e2f4-175">Если **модуль Microsoft ASP.NET Core** отсутствует в списке установленных программ, установите его.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-175">If the **Microsoft ASP.NET Core Module** isn't present in the list of installed programs, install the module.</span></span> <span data-ttu-id="4e2f4-176">В разделе [установить .NET Core, размещение пакета](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="4e2f4-176">See [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="4e2f4-177">Убедитесь, что **пула приложений** > **модель процесса** > **удостоверение** равно **ApplicationPoolIdentity** или пользовательское удостоверение имеет правильные разрешения на доступ к папке развертывания приложения.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-177">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="4e2f4-178">Неверный processPath, отсутствующую переменную PATH, размещение пакета не установлен, системы и IIS не перезапускается, VC ++ распространяемый пакет не установлен или dotnet.exe нарушение прав доступа</span><span class="sxs-lookup"><span data-stu-id="4e2f4-178">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

* <span data-ttu-id="4e2f4-179">**Браузер:** ошибка HTTP 502.5 — сбой процесса</span><span class="sxs-lookup"><span data-stu-id="4e2f4-179">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="4e2f4-180">**Журнал приложения:** приложения "MACHINE/WEBROOT/APPHOST / {сборки}" с Физический корень "C:\\{путь}\' не удалось запустить процесс с Командная строка"».\{ .exe сборки}» ", код ошибки =" 0x80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-180">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '".\{assembly}.exe" ', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="4e2f4-181">**Журнал модуля ASP.NET Core:** файл журнала создан, но пуст</span><span class="sxs-lookup"><span data-stu-id="4e2f4-181">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="4e2f4-182">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="4e2f4-182">Troubleshooting:</span></span>

* <span data-ttu-id="4e2f4-183">Убедитесь в том, что приложение выполняется локально в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-183">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="4e2f4-184">Сбой процесса может быть результатом проблемы в приложении.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-184">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="4e2f4-185">Дополнительные сведения см. в разделе [Устранение неполадок](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="4e2f4-185">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="4e2f4-186">Проверьте *processPath* атрибут `<aspNetCore>` элемент в *web.config* для подтверждения, что это *dotnet* для развертывания зависит от framework (системой защиты Фиксированных) или *. \{сборки} .exe* автономные развертывания (SCD).</span><span class="sxs-lookup"><span data-stu-id="4e2f4-186">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's *dotnet* for a framework-dependent deployment (FDD) or *.\{assembly}.exe* for a self-contained deployment (SCD).</span></span>

* <span data-ttu-id="4e2f4-187">Для зависимого от платформы развертывания файл *dotnet.exe* может быть недоступен по пути, указанному в переменной PATH.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-187">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="4e2f4-188">Убедитесь в том, что папка *C:\Program Files\dotnet\* существует в системных параметрах PATH.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-188">Confirm that *C:\Program Files\dotnet\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="4e2f4-189">В случае с зависимым от платформы развертыванием файл *dotnet.exe* может быть недоступен для удостоверения пользователя, используемого для пула приложений.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-189">For an FDD, *dotnet.exe* might not be accessible for the user identity of the Application Pool.</span></span> <span data-ttu-id="4e2f4-190">Убедитесь в том, что удостоверение пользователя AppPool имеет доступ к каталогу *C:\Program Files\dotnet*.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-190">Confirm that the AppPool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="4e2f4-191">Убедитесь, что не настроены для идентификации пользователя AppPool на правила deny *C:\Program Files\dotnet* и каталогов приложения.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-191">Confirm that there are no deny rules configured for the AppPool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="4e2f4-192">Системой защиты Фиксированных был развернут и .NET Core устанавливается без перезапуска IIS.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-192">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="4e2f4-193">Перезагрузите сервер или перезапустите службы IIS, выполнив в командной строке команду **net stop was /y**, а затем — команду **net start w3svc**.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-193">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="4e2f4-194">Системой защиты Фиксированных развернуто без установки среды выполнения .NET Core на хост-системы.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-194">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="4e2f4-195">Если не был установлен среды выполнения .NET Core, запустите **.NET Core размещение пакета установщика** в системе.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-195">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span> <span data-ttu-id="4e2f4-196">В разделе [установить .NET Core, размещение пакета](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="4e2f4-196">See [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="4e2f4-197">При попытке установить среду выполнения .NET Core на компьютере без подключения к Интернету, получить среду выполнения из [всех загрузок .NET](https://www.microsoft.com/net/download/all) и запустите установщик размещение пакета для установки модуля ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-197">If attempting to install the .NET Core runtime on a system without an Internet connection, obtain the runtime from [.NET All Downloads](https://www.microsoft.com/net/download/all) and run the Hosting Bundle installer to install the ASP.NET Core Module.</span></span> <span data-ttu-id="4e2f4-198">Завершите установку, перезагрузив систему или перезапустив службы IIS. Для этого выполните в командной строке команду **net stop was /y**, а затем — команду **net start w3svc**.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-198">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="4e2f4-199">Системой защиты Фиксированных был развернут и *Visual C++ 2015 распространяемый компонент Microsoft (x64)* не установлен в системе.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-199">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="4e2f4-200">Получить установщика из [центра загрузки Майкрософт](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="4e2f4-200">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="4e2f4-201">Неверные аргументы элемента \<aspNetCore\></span><span class="sxs-lookup"><span data-stu-id="4e2f4-201">Incorrect arguments of \<aspNetCore\> element</span></span>

* <span data-ttu-id="4e2f4-202">**Браузер:** ошибка HTTP 502.5 — сбой процесса</span><span class="sxs-lookup"><span data-stu-id="4e2f4-202">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="4e2f4-203">**Журнал приложения:** приложения "MACHINE/WEBROOT/APPHOST / {сборки}" с Физический корень "C:\\{путь}\' не удалось запустить процесс с помощью командной строки" «dotnet».\{ DLL-файл сборки} ", код ошибки =" 0x80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-203">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="4e2f4-204">**Журнал модуля ASP.NET Core:** выполнение приложения не существует: "путь\{сборки} DLL-файл"</span><span class="sxs-lookup"><span data-stu-id="4e2f4-204">**ASP.NET Core Module Log:** The application to execute does not exist: 'PATH\{assembly}.dll'</span></span>

<span data-ttu-id="4e2f4-205">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="4e2f4-205">Troubleshooting:</span></span>

* <span data-ttu-id="4e2f4-206">Убедитесь в том, что приложение выполняется локально в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-206">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="4e2f4-207">Сбой процесса может быть результатом проблемы в приложении.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-207">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="4e2f4-208">Дополнительные сведения см. в разделе [Устранение неполадок](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="4e2f4-208">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="4e2f4-209">Изучите *аргументы* атрибут `<aspNetCore>` элемент в *web.config* для подтверждения того, что он доступен (a) *.\{ DLL-файл сборки}* для развертывания зависит от framework (системой защиты Фиксированных); или значение не указано, пустая строка (b) (*аргументов = "»*), или список аргументов, приложения (*аргументов = «аргумент1, аргумент2,...»*) автономные развертывания (SCD).</span><span class="sxs-lookup"><span data-stu-id="4e2f4-209">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) *.\{assembly}.dll* for a framework-dependent deployment (FDD); or (b) not present, an empty string (*arguments=""*), or a list of the app's arguments (*arguments="arg1, arg2, ..."*) for a self-contained deployment (SCD).</span></span>

## <a name="missing-net-framework-version"></a><span data-ttu-id="4e2f4-210">Отсутствует версия платформы .NET Framework</span><span class="sxs-lookup"><span data-stu-id="4e2f4-210">Missing .NET Framework version</span></span>

* <span data-ttu-id="4e2f4-211">**Браузер:** 502.3 — неверный шлюз. При попытке маршрутизации запроса произошла ошибка подключения.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-211">**Browser:** 502.3 Bad Gateway - There was a connection error while trying to route the request.</span></span>

* <span data-ttu-id="4e2f4-212">**Журнал приложения:** ErrorCode = приложения "MACHINE/WEBROOT/APPHOST / {сборки}" с Физический корень "C:\\{путь}\' не удалось запустить процесс с помощью командной строки" «dotnet».\{ DLL-файл сборки} ", код ошибки =" 0x80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-212">**Application Log:** ErrorCode = Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="4e2f4-213">**Журнал модуля ASP.NET Core:** Исключение из-за отсутствия метода, файла или сборки.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-213">**ASP.NET Core Module Log:** Missing method, file, or assembly exception.</span></span> <span data-ttu-id="4e2f4-214">Метод, файл или сборка, указанные в исключении, представляют собой метод, файл или сборку .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-214">The method, file, or assembly specified in the exception is a .NET Framework method, file, or assembly.</span></span>

<span data-ttu-id="4e2f4-215">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="4e2f4-215">Troubleshooting:</span></span>

* <span data-ttu-id="4e2f4-216">Установите версию .NET Framework, отсутствующую в системе.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-216">Install the .NET Framework version missing from the system.</span></span>

* <span data-ttu-id="4e2f4-217">Для развертывания зависит от framework (системой защиты Фиксированных) убедитесь, что среда выполнения правильный установлено в системе.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-217">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span> <span data-ttu-id="4e2f4-218">Если проект обновляется 1.1, 2.0, развернуты в системе размещения, и это исключение приводит к, убедитесь в наличии 2.0 framework на хост-системы.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-218">If the project is upgraded from 1.1 to 2.0, deployed to the hosting system, and this exception results, ensure that the 2.0 framework is on the hosting system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="4e2f4-219">Пул приложений остановлен</span><span class="sxs-lookup"><span data-stu-id="4e2f4-219">Stopped Application Pool</span></span>

* <span data-ttu-id="4e2f4-220">**Браузер:** 503 — служба недоступна</span><span class="sxs-lookup"><span data-stu-id="4e2f4-220">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="4e2f4-221">**Журнал приложений:** нет записи</span><span class="sxs-lookup"><span data-stu-id="4e2f4-221">**Application Log:** No entry</span></span>

* <span data-ttu-id="4e2f4-222">**Журнал модуля ASP.NET Core:** файл журнала не создан</span><span class="sxs-lookup"><span data-stu-id="4e2f4-222">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="4e2f4-223">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="4e2f4-223">Troubleshooting</span></span>

* <span data-ttu-id="4e2f4-224">Убедитесь, что пул приложений не находится в *остановлена* состояния.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-224">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="iis-integration-middleware-not-implemented"></a><span data-ttu-id="4e2f4-225">ПО промежуточного слоя для интеграции IIS не внедрено</span><span class="sxs-lookup"><span data-stu-id="4e2f4-225">IIS Integration middleware not implemented</span></span>

* <span data-ttu-id="4e2f4-226">**Браузер:** ошибка HTTP 502.5 — сбой процесса</span><span class="sxs-lookup"><span data-stu-id="4e2f4-226">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="4e2f4-227">**Журнал приложения:** приложения "MACHINE/WEBROOT/APPHOST / {сборки}" с Физический корень "C:\\{путь}\' созданных процесса с помощью командной строки" "C:\\{путь}\{сборки}. {} exe | библиотека dll}» "но аварийное завершение и не ответа либо не прослушивать данный порт «{PORT}», код ошибки ="0x800705b4»</span><span class="sxs-lookup"><span data-stu-id="4e2f4-227">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="4e2f4-228">**Журнал модуля ASP.NET Core:** файл журнала создан, и в нем нет сведений о неправильной работе.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-228">**ASP.NET Core Module Log:** Log file created and shows normal operation.</span></span>

<span data-ttu-id="4e2f4-229">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="4e2f4-229">Troubleshooting</span></span>

* <span data-ttu-id="4e2f4-230">Убедитесь в том, что приложение выполняется локально в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-230">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="4e2f4-231">Сбой процесса может быть результатом проблемы в приложении.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-231">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="4e2f4-232">Дополнительные сведения см. в разделе [Устранение неполадок](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="4e2f4-232">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="4e2f4-233">Подтверждение, либо:</span><span class="sxs-lookup"><span data-stu-id="4e2f4-233">Confirm that either:</span></span>
  * <span data-ttu-id="4e2f4-234">По промежуточного слоя интеграции IIS имеет referencedby вызов `UseIISIntegration` метода в приложении `WebHostBuilder` (ASP.NET Core 1.x)</span><span class="sxs-lookup"><span data-stu-id="4e2f4-234">The IIS Integration middleware is referencedby calling the `UseIISIntegration` method on the app's `WebHostBuilder` (ASP.NET Core 1.x)</span></span>
  * <span data-ttu-id="4e2f4-235">Приложения использует `CreateDefaultBuilder` метод (ASP.NET Core 2.x).</span><span class="sxs-lookup"><span data-stu-id="4e2f4-235">The apps uses the `CreateDefaultBuilder` method (ASP.NET Core 2.x).</span></span>
  
  <span data-ttu-id="4e2f4-236">Дополнительные сведения см. в статье [Размещение в ASP.NET Core](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="4e2f4-236">See [Hosting in ASP.NET Core](xref:fundamentals/hosting) for details.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="4e2f4-237">Дочернее приложение имеет раздел \<handlers\></span><span class="sxs-lookup"><span data-stu-id="4e2f4-237">Sub-application includes a \<handlers\> section</span></span>

* <span data-ttu-id="4e2f4-238">**Браузер:** ошибка HTTP 500.19 — внутренняя ошибка сервера</span><span class="sxs-lookup"><span data-stu-id="4e2f4-238">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="4e2f4-239">**Журнал приложений:** нет записи</span><span class="sxs-lookup"><span data-stu-id="4e2f4-239">**Application Log:** No entry</span></span>

* <span data-ttu-id="4e2f4-240">**Журнал модуля ASP.NET Core:** файл журнала создан и показан нормальной работы для корневого приложения.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-240">**ASP.NET Core Module Log:** Log file created and shows normal operation for the root app.</span></span> <span data-ttu-id="4e2f4-241">Файл журнала не создается для приложения sub.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-241">Log file not created for the sub-app.</span></span>

<span data-ttu-id="4e2f4-242">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="4e2f4-242">Troubleshooting</span></span>

* <span data-ttu-id="4e2f4-243">В файле *web.config* дочернего приложения не должно быть раздела `<handlers>`.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-243">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="4e2f4-244">Неверный путь к журналу stdout</span><span class="sxs-lookup"><span data-stu-id="4e2f4-244">stdout log path incorrect</span></span>

* <span data-ttu-id="4e2f4-245">**Обозреватель:** обычно отвечает приложение.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-245">**Browser:** The app responds normally.</span></span>

* <span data-ttu-id="4e2f4-246">**Журнал приложения:** предупреждение: не удалось создать stdoutLogFile \\? \C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log, код ошибки = - 2147024893.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-246">**Application Log:** Warning: Could not create stdoutLogFile \\?\C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="4e2f4-247">**Журнал модуля ASP.NET Core:** файл журнала не создан</span><span class="sxs-lookup"><span data-stu-id="4e2f4-247">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="4e2f4-248">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="4e2f4-248">Troubleshooting</span></span>

* <span data-ttu-id="4e2f4-249">`stdoutLogFile` Пути, указанному в `<aspNetCore>` элемент *web.config* не существует.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-249">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="4e2f4-250">Дополнительные сведения см. в разделе [создания и перенаправления журнала](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) раздела Справочник по конфигурации модуля ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-250">For more information, see the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) section of the ASP.NET Core Module configuration reference topic.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="4e2f4-251">Общая проблема с конфигурацией приложения</span><span class="sxs-lookup"><span data-stu-id="4e2f4-251">Application configuration general issue</span></span>

* <span data-ttu-id="4e2f4-252">**Браузер:** ошибка HTTP 502.5 — сбой процесса</span><span class="sxs-lookup"><span data-stu-id="4e2f4-252">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="4e2f4-253">**Журнал приложения:** приложения "MACHINE/WEBROOT/APPHOST / {сборки}" с Физический корень "C:\\{путь}\' созданных процесса с помощью командной строки" "C:\\{путь}\{сборки}. {} exe | библиотека dll}» "но аварийное завершение и не ответа либо не прослушивать данный порт «{PORT}», код ошибки ="0x800705b4»</span><span class="sxs-lookup"><span data-stu-id="4e2f4-253">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="4e2f4-254">**Журнал модуля ASP.NET Core:** файл журнала создан, но пуст</span><span class="sxs-lookup"><span data-stu-id="4e2f4-254">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="4e2f4-255">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="4e2f4-255">Troubleshooting</span></span>

* <span data-ttu-id="4e2f4-256">Это общее исключение указывает, что процессу не удалось запустить, скорее всего вызвано проблемой конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-256">This general exception indicates that the process failed to start, most likely due to an app configuration issue.</span></span> <span data-ttu-id="4e2f4-257">Ссылка на [структуру каталогов](xref:host-and-deploy/directory-structure), убедитесь, что приложение развернуто файлов и папок подходят, и что имеются файлы конфигурации приложения и содержать правильные параметры для приложения и среды.</span><span class="sxs-lookup"><span data-stu-id="4e2f4-257">Referring to [Directory Structure](xref:host-and-deploy/directory-structure), confirm that the app's deployed files and folders are appropriate and that the app's configuration files are present and contain the correct settings for the app and environment.</span></span> <span data-ttu-id="4e2f4-258">Дополнительные сведения см. в разделе [Устранение неполадок](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="4e2f4-258">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>
