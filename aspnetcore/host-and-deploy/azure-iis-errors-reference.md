---
title: Справочник по общим ошибкам в Службе приложений Azure и службах IIS с ASP.NET Core
author: guardrex
description: Рекомендации по устранению распространенных ошибок при размещении приложений ASP.NET Core в службе приложений Azure и службах IIS.
ms.author: riande
ms.custom: mvc
ms.date: 02/21/2019
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: d1cdac4d27ee1bc3ebb4329c1bbd3bdacb34a58c
ms.sourcegitcommit: b3894b65e313570e97a2ab78b8addd22f427cac8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/23/2019
ms.locfileid: "56743951"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="d4713-103">Справочник по общим ошибкам в Службе приложений Azure и службах IIS с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d4713-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="d4713-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="d4713-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d4713-105">В этой статье приводятся рекомендации по устранению распространенных ошибок при размещении приложений ASP.NET Core в службе приложений Azure и службах IIS.</span><span class="sxs-lookup"><span data-stu-id="d4713-105">This topic offers troubleshooting advice for common errors when hosting ASP.NET Core apps on Azure Apps Service and IIS.</span></span>

<span data-ttu-id="d4713-106">Соберите следующие сведения:</span><span class="sxs-lookup"><span data-stu-id="d4713-106">Collect the following information:</span></span>

* <span data-ttu-id="d4713-107">поведение обозревателя (код состояния и сообщение об ошибке);</span><span class="sxs-lookup"><span data-stu-id="d4713-107">Browser behavior (status code and error message)</span></span>
* <span data-ttu-id="d4713-108">записи в журнале событий приложения;</span><span class="sxs-lookup"><span data-stu-id="d4713-108">Application Event Log entries</span></span>
  * <span data-ttu-id="d4713-109">Служба приложений Azure &ndash; см. статью <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="d4713-109">Azure App Service &ndash; See <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>
  * <span data-ttu-id="d4713-110">IIS</span><span class="sxs-lookup"><span data-stu-id="d4713-110">IIS</span></span>
    1. <span data-ttu-id="d4713-111">В меню **Windows** нажмите кнопку **Пуск**, введите *Просмотр событий* и нажмите клавишу **ВВОД**.</span><span class="sxs-lookup"><span data-stu-id="d4713-111">Select **Start** on the **Windows** menu, type *Event Viewer*, and press **Enter**.</span></span>
    1. <span data-ttu-id="d4713-112">В открывшемся окне **Просмотр событий** на боковой панели разверните узлы **Журналы Windows** > **Приложения**.</span><span class="sxs-lookup"><span data-stu-id="d4713-112">After the **Event Viewer** opens, expand **Windows Logs** > **Application** in the sidebar.</span></span>
* <span data-ttu-id="d4713-113">Записи в журнале вывода stdout и отладки модуля ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d4713-113">ASP.NET Core Module stdout and debug log entries</span></span>
  * <span data-ttu-id="d4713-114">Служба приложений Azure &ndash; см. статью <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="d4713-114">Azure App Service &ndash; See <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>
  * <span data-ttu-id="d4713-115">IIS &ndash; следуйте инструкциям в разделах [Создание и перенаправление журнала](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) и [Расширенные журналы диагностики](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) в статье "Модуль ASP.NET Core".</span><span class="sxs-lookup"><span data-stu-id="d4713-115">IIS &ndash; Follow the instructions in the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) and [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sections of the ASP.NET Core Module topic.</span></span>

<span data-ttu-id="d4713-116">Сравните собранные данные с представленной здесь информацией о распространенных ошибках.</span><span class="sxs-lookup"><span data-stu-id="d4713-116">Compare error information to the following common errors.</span></span> <span data-ttu-id="d4713-117">Если вы найдете соответствие, выполните рекомендации по устранению неполадок.</span><span class="sxs-lookup"><span data-stu-id="d4713-117">If a match is found, follow the troubleshooting advice.</span></span>

<span data-ttu-id="d4713-118">Приведенный ниже перечень ошибок не является исчерпывающим.</span><span class="sxs-lookup"><span data-stu-id="d4713-118">The list of errors in this topic isn't exhaustive.</span></span> <span data-ttu-id="d4713-119">Если вы встретите сообщение об ошибке, которое здесь не указано, воспользуйтесь кнопкой **Обратная связь** в нижней части этой статьи, чтобы создать запрос с подробным описанием, которое поможет воспроизвести эту ошибку.</span><span class="sxs-lookup"><span data-stu-id="d4713-119">If you encounter an error not listed here, open a new issue using the **Content feedback** button at the bottom of this topic with detailed instructions on how to reproduce the error.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="d4713-120">Установщику не удается получить распространяемый компонент VC++</span><span class="sxs-lookup"><span data-stu-id="d4713-120">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="d4713-121">**Исключение установщика:** 0x80072efd**или**0x80072f76 — неопределенная ошибка</span><span class="sxs-lookup"><span data-stu-id="d4713-121">**Installer Exception:** 0x80072efd **--OR--** 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="d4713-122">**Исключение журнала установщика&#8224;:** ошибка 0x80072efd**или**0x80072f76: ошибка выполнения пакета EXE</span><span class="sxs-lookup"><span data-stu-id="d4713-122">**Installer Log Exception&#8224;:** Error 0x80072efd **--OR--** 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="d4713-123">&#8224;Журнал находится в папке *C:\Users\{ПОЛЬЗОВАТЕЛЬ}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{МЕТКА_ВРЕМЕНИ}.log*.</span><span class="sxs-lookup"><span data-stu-id="d4713-123">&#8224;The log is located at *C:\Users\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*.</span></span>

<span data-ttu-id="d4713-124">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="d4713-124">Troubleshooting:</span></span>

<span data-ttu-id="d4713-125">Если при [установке пакета размещения .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) у системы нет доступа к Интернету, это исключение возникает при попытке установщика получить распространяемый компонент *Microsoft Visual C++ 2015*.</span><span class="sxs-lookup"><span data-stu-id="d4713-125">If the system doesn't have Internet access while [installing the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="d4713-126">Получите установщик в [Центре загрузки Майкрософт](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="d4713-126">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="d4713-127">Если работа установщика завершается сбоем, это может быть связано с тем, что сервер не может получить среду выполнения .NET Core, необходимую для размещения [зависимых от платформы развертываний (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="d4713-127">If the installer fails, the server may not receive the .NET Core runtime required to host a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span> <span data-ttu-id="d4713-128">Если вы размещаете зависимые от платформы развертывания, в окне **Программы и компоненты** или **Приложения и компоненты** убедитесь, что установлена среда выполнения.</span><span class="sxs-lookup"><span data-stu-id="d4713-128">If hosting an FDD, confirm that the runtime is installed in **Programs & Features** or **Apps & features**.</span></span> <span data-ttu-id="d4713-129">Если требуется конкретная среда выполнения, скачайте ее на странице [скачивания версий .NET](https://dotnet.microsoft.com/download/archives) и установите в системе.</span><span class="sxs-lookup"><span data-stu-id="d4713-129">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="d4713-130">После установки среды выполнения перезагрузите систему или перезапустите службы IIS, выполнив в командной строке команду **net stop was /y**, а затем — команду **net start w3svc**.</span><span class="sxs-lookup"><span data-stu-id="d4713-130">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="d4713-131">При обновлении ОС был удален 32-разрядный модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d4713-131">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

<span data-ttu-id="d4713-132">**Журнал приложений:** не удалось загрузить DLL модуля **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**.</span><span class="sxs-lookup"><span data-stu-id="d4713-132">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="d4713-133">Ошибочные данные.</span><span class="sxs-lookup"><span data-stu-id="d4713-133">The data is the error.</span></span>

<span data-ttu-id="d4713-134">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="d4713-134">Troubleshooting:</span></span>

<span data-ttu-id="d4713-135">Не относящиеся к ОС файлы в каталоге **C:\Windows\SysWOW64\inetsrv** не сохраняются при обновлении ОС.</span><span class="sxs-lookup"><span data-stu-id="d4713-135">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="d4713-136">Эта ошибка возникает, если модуль ASP.NET Core был установлен до обновления ОС, а после обновления ОС выполняется любой пул приложений в 32-разрядном режиме.</span><span class="sxs-lookup"><span data-stu-id="d4713-136">If the ASP.NET Core Module is installed prior to an OS upgrade and then any app pool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="d4713-137">После обновления ОС восстановите модуль ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4713-137">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="d4713-138">См. статью [Установка пакета размещения .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="d4713-138">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="d4713-139">Выберите вариант **Восстановить** при запуске установщика.</span><span class="sxs-lookup"><span data-stu-id="d4713-139">Select **Repair** when the installer is run.</span></span>

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a><span data-ttu-id="d4713-140">Приложение x86 развернуто, но для 32-разрядных приложений не включен пул приложений</span><span class="sxs-lookup"><span data-stu-id="d4713-140">An x86 app is deployed but the app pool isn't enabled for 32-bit apps</span></span>

* <span data-ttu-id="d4713-141">**Браузер:** ошибка HTTP 500.30 — сбой в процессе запуска ANCM</span><span class="sxs-lookup"><span data-stu-id="d4713-141">**Browser:** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="d4713-142">**Журнал приложений:** для приложения "/LM/W3SVC/5/ROOT" с физическим корневым каталогом "{ПУТЬ}" возникло непредвиденное управляемое исключение, код исключения — 0xe0434352.</span><span class="sxs-lookup"><span data-stu-id="d4713-142">**Application Log:** Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' hit unexpected managed exception, exception code = '0xe0434352'.</span></span> <span data-ttu-id="d4713-143">Дополнительные сведения см. в журналах sderr.</span><span class="sxs-lookup"><span data-stu-id="d4713-143">Please check the stderr logs for more information.</span></span> <span data-ttu-id="d4713-144">Приложению /LM/W3SVC/5/ROOTс физическим корневым каталогом {ПУТЬ} не удалось загрузить clr и управляемое приложение.</span><span class="sxs-lookup"><span data-stu-id="d4713-144">Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' failed to load clr and managed application.</span></span> <span data-ttu-id="d4713-145">Рабочий поток CLR преждевременно завершил работу.</span><span class="sxs-lookup"><span data-stu-id="d4713-145">CLR worker thread exited prematurely</span></span>

* <span data-ttu-id="d4713-146">**Журнал stdout модуля ASP.NET Core:** файл журнала создан, но пуст.</span><span class="sxs-lookup"><span data-stu-id="d4713-146">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d4713-147">**Журнал отладки модуля ASP.NET Core:** возвращена ошибка HRESULT: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="d4713-147">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

<span data-ttu-id="d4713-148">Пакет SDK перехватывает этот сценарий при публикации автономного приложения.</span><span class="sxs-lookup"><span data-stu-id="d4713-148">This scenario is trapped by the SDK when publishing a self-contained app.</span></span> <span data-ttu-id="d4713-149">Пакет SDK выводит сообщение об ошибке, если идентификатор RID не соответствует целевой платформе (например, RID `win10-x64` с `<PlatformTarget>x86</PlatformTarget>` в файле проекта).</span><span class="sxs-lookup"><span data-stu-id="d4713-149">The SDK produces an error if the RID doesn't match the platform target (for example, `win10-x64` RID with `<PlatformTarget>x86</PlatformTarget>` in the project file).</span></span>

<span data-ttu-id="d4713-150">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="d4713-150">Troubleshooting:</span></span>

<span data-ttu-id="d4713-151">Если используется зависимое от платформы (x86) развертывание (`<PlatformTarget>x86</PlatformTarget>`), включите пул приложений IIS для 32-разрядных приложений.</span><span class="sxs-lookup"><span data-stu-id="d4713-151">For an x86 framework-dependent deployment (`<PlatformTarget>x86</PlatformTarget>`), enable the IIS app pool for 32-bit apps.</span></span> <span data-ttu-id="d4713-152">В диспетчере IIS откройте раздел **Дополнительные параметры** пула приложений и задайте параметру **Разрешены 32-разрядные приложения** значение **True**.</span><span class="sxs-lookup"><span data-stu-id="d4713-152">In IIS Manager, open the app pool's **Advanced Settings** and set **Enable 32-Bit Applications** to **True**.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="d4713-153">Конфликты платформы с относительным идентификатором</span><span class="sxs-lookup"><span data-stu-id="d4713-153">Platform conflicts with RID</span></span>

* <span data-ttu-id="d4713-154">**Браузер:** ошибка HTTP 502.5 — сбой процесса</span><span class="sxs-lookup"><span data-stu-id="d4713-154">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="d4713-155">**Журнал приложений:** приложению MACHINE/WEBROOT/APPHOST/{СБОРКА} с физическим корневым каталогом C:\{ПУТЬ}\' не удалось запустить процесс с помощью команды "C:\{ПУТЬ}\{СБОРКА}.{exe|dll}"; код ошибки — 0x80004005 : ff.</span><span class="sxs-lookup"><span data-stu-id="d4713-155">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="d4713-156">**Журнал stdout модуля ASP.NET Core:** необработанное исключение: System.BadImageFormatException: не удалось загрузить файл или сборку {СБОРКА}.dll.</span><span class="sxs-lookup"><span data-stu-id="d4713-156">**ASP.NET Core Module stdout Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{ASSEMBLY}.dll'.</span></span> <span data-ttu-id="d4713-157">Была сделана попытка загрузить программу, имеющую неверный формат.</span><span class="sxs-lookup"><span data-stu-id="d4713-157">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="d4713-158">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="d4713-158">Troubleshooting:</span></span>

* <span data-ttu-id="d4713-159">Убедитесь в том, что приложение выполняется локально в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d4713-159">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="d4713-160">Сбой процесса может быть результатом проблемы в приложении.</span><span class="sxs-lookup"><span data-stu-id="d4713-160">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="d4713-161">Дополнительные сведения см. в статьях [Устранение неполадок ASP.NET Core в службах IIS](xref:host-and-deploy/iis/troubleshoot) или [Устранение неполадок ASP.NET Core в службе приложений Azure](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="d4713-161">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="d4713-162">Если это исключение возникает для развертывания приложений Azure при обновлении приложения и развертывании более новых сборок, вручную удалите все файлы предыдущего развертывания.</span><span class="sxs-lookup"><span data-stu-id="d4713-162">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="d4713-163">Если останутся несовместимые сборки, то при развертывании обновленного приложения это может привести к исключению `System.BadImageFormatException`.</span><span class="sxs-lookup"><span data-stu-id="d4713-163">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="d4713-164">Неправильная конечная точка URI, либо веб-сайт остановлен</span><span class="sxs-lookup"><span data-stu-id="d4713-164">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="d4713-165">**Браузер:** ERR_CONNECTION_REFUSED**или** не удается подключиться</span><span class="sxs-lookup"><span data-stu-id="d4713-165">**Browser:** ERR_CONNECTION_REFUSED **--OR--** Unable to connect</span></span>

* <span data-ttu-id="d4713-166">**Журнал приложений:** нет записи</span><span class="sxs-lookup"><span data-stu-id="d4713-166">**Application Log:** No entry</span></span>

* <span data-ttu-id="d4713-167">**Журнал stdout модуля ASP.NET Core:** файл журнала не создан.</span><span class="sxs-lookup"><span data-stu-id="d4713-167">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d4713-168">**Журнал отладки модуля ASP.NET Core:** файл журнала не создан.</span><span class="sxs-lookup"><span data-stu-id="d4713-168">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="d4713-169">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="d4713-169">Troubleshooting:</span></span>

* <span data-ttu-id="d4713-170">Убедитесь, что используется правильный URI конечной точки для приложения.</span><span class="sxs-lookup"><span data-stu-id="d4713-170">Confirm the correct URI endpoint for the app is in use.</span></span> <span data-ttu-id="d4713-171">Проверьте все привязки.</span><span class="sxs-lookup"><span data-stu-id="d4713-171">Check the bindings.</span></span>

* <span data-ttu-id="d4713-172">Убедитесь, что веб-сайт IIS не находится в состоянии *Остановлен*.</span><span class="sxs-lookup"><span data-stu-id="d4713-172">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="d4713-173">Компонент сервера CoreWebEngine или W3SVC отключен</span><span class="sxs-lookup"><span data-stu-id="d4713-173">CoreWebEngine or W3SVC server features disabled</span></span>

<span data-ttu-id="d4713-174">**Исключение ОС:** для использования модуля ASP.NET Core должны быть установлены компоненты IIS 7.0 CoreWebEngine и W3SVC.</span><span class="sxs-lookup"><span data-stu-id="d4713-174">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="d4713-175">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="d4713-175">Troubleshooting:</span></span>

<span data-ttu-id="d4713-176">Убедитесь, что включены правильная роль и необходимые компоненты.</span><span class="sxs-lookup"><span data-stu-id="d4713-176">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="d4713-177">См. раздел [Конфигурация IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="d4713-177">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="d4713-178">Неправильный физический путь к веб-сайту, либо отсутствует приложение</span><span class="sxs-lookup"><span data-stu-id="d4713-178">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="d4713-179">**Браузер:** 403 — запрещено. В доступе отказано**или**403.14 — запрещено. Веб-сервер настроен таким образом, чтобы не формировать список содержимого каталога.</span><span class="sxs-lookup"><span data-stu-id="d4713-179">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="d4713-180">**Журнал приложений:** нет записи</span><span class="sxs-lookup"><span data-stu-id="d4713-180">**Application Log:** No entry</span></span>

* <span data-ttu-id="d4713-181">**Журнал stdout модуля ASP.NET Core:** файл журнала не создан.</span><span class="sxs-lookup"><span data-stu-id="d4713-181">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d4713-182">**Журнал отладки модуля ASP.NET Core:** файл журнала не создан.</span><span class="sxs-lookup"><span data-stu-id="d4713-182">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="d4713-183">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="d4713-183">Troubleshooting:</span></span>

<span data-ttu-id="d4713-184">Проверьте **основные настройки** веб-сайта IIS и физическую папку приложения.</span><span class="sxs-lookup"><span data-stu-id="d4713-184">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="d4713-185">Убедитесь в том, что приложение находится в папке по **физическому пути**, указанному для веб-сайта IIS.</span><span class="sxs-lookup"><span data-stu-id="d4713-185">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="d4713-186">Неправильная роль, модуль ASP.NET Core не установлен либо неверные разрешения</span><span class="sxs-lookup"><span data-stu-id="d4713-186">Incorrect role, ASP.NET Core Module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="d4713-187">**Браузер:** 500.19 — внутренняя ошибка сервера. Запрашиваемая страница недоступна из-за неверной конфигурации данных для этой страницы.</span><span class="sxs-lookup"><span data-stu-id="d4713-187">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span> <span data-ttu-id="d4713-188">**или**не удается отобразить эту страницу</span><span class="sxs-lookup"><span data-stu-id="d4713-188">**--OR--** This page can't be displayed</span></span>

* <span data-ttu-id="d4713-189">**Журнал приложений:** нет записи</span><span class="sxs-lookup"><span data-stu-id="d4713-189">**Application Log:** No entry</span></span>

* <span data-ttu-id="d4713-190">**Журнал stdout модуля ASP.NET Core:** файл журнала не создан.</span><span class="sxs-lookup"><span data-stu-id="d4713-190">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d4713-191">**Журнал отладки модуля ASP.NET Core:** файл журнала не создан.</span><span class="sxs-lookup"><span data-stu-id="d4713-191">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="d4713-192">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="d4713-192">Troubleshooting:</span></span>

* <span data-ttu-id="d4713-193">Убедитесь, что включена соответствующая роль.</span><span class="sxs-lookup"><span data-stu-id="d4713-193">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="d4713-194">См. раздел [Конфигурация IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="d4713-194">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="d4713-195">Откройте окно **Программы и компоненты** или **Приложения и возможности** и убедитесь, что установлено ПО **Windows Server Hosting**.</span><span class="sxs-lookup"><span data-stu-id="d4713-195">Open **Programs & Features** or **Apps & features** and confirm that **Windows Server Hosting** is installed.</span></span> <span data-ttu-id="d4713-196">Если **Windows Server Hosting** отсутствует в списке установленных программ, скачайте и установите пакет размещения .NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4713-196">If **Windows Server Hosting** isn't present in the list of installed programs, download and install the .NET Core Hosting Bundle.</span></span>

  [<span data-ttu-id="d4713-197">Текущий установщик пакета размещения .NET Core (прямая загрузка)</span><span class="sxs-lookup"><span data-stu-id="d4713-197">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="d4713-198">См. дополнительные сведения об [установке пакета размещения .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="d4713-198">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="d4713-199">Убедитесь, что параметр **Пул приложений** > **Модель процесса** > **Удостоверение** имеет значение **ApplicationPoolIdentity** или что у пользовательского удостоверения есть нужные разрешения на доступ к папке развертывания приложения.</span><span class="sxs-lookup"><span data-stu-id="d4713-199">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

* <span data-ttu-id="d4713-200">Если вы удалили пакет размещения ASP.NET Core и установили его более раннюю версию, файл *applicationHost.config* не будет включать раздел модуля ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4713-200">If you uninstalled the ASP.NET Core Hosting Bundle and installed an earlier version of the hosting bundle, the *applicationHost.config* file doesn't include a section for the ASP.NET Core Module.</span></span> <span data-ttu-id="d4713-201">Откройте файл *applicationHost.config* в папке *%windir%/System32/inetsrv/config* и найдите группу раздела `<configuration><configSections><sectionGroup name="system.webServer">`.</span><span class="sxs-lookup"><span data-stu-id="d4713-201">Open *applicationHost.config* at *%windir%/System32/inetsrv/config* and find the `<configuration><configSections><sectionGroup name="system.webServer">` section group.</span></span> <span data-ttu-id="d4713-202">Если раздел модуля ASP.NET Core отсутствует в группе раздела, добавьте его:</span><span class="sxs-lookup"><span data-stu-id="d4713-202">If the section for the ASP.NET Core Module is missing from the section group, add the section element:</span></span>

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```
  
  <span data-ttu-id="d4713-203">Кроме того, вы можете установить последнюю версию пакета размещения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4713-203">Alternatively, install the lastest version of the ASP.NET Core Hosting Bundle.</span></span> <span data-ttu-id="d4713-204">Последняя версия имеет обратную совместимость с поддерживаемыми приложениями ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4713-204">The latest version is backwards-compatible with supported ASP.NET Core apps.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="d4713-205">Неправильное значение processPath, отсутствует переменная PATH, пакет размещения не установлен, система или службы IIS не перезапущены, не установлен распространяемый компонент VC++ либо нарушены права доступа к dotnet.exe</span><span class="sxs-lookup"><span data-stu-id="d4713-205">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d4713-206">**Браузер:** ошибка HTTP 500.0 — ошибка загрузки внутрипроцессного обработчика ANCM</span><span class="sxs-lookup"><span data-stu-id="d4713-206">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="d4713-207">**Журнал приложений:** приложению MACHINE/WEBROOT/APPHOST/{СБОРКА} с физическим корневым каталогом C:\{ПУТЬ}\' не удалось запустить процесс с помощью команды "{...}"</span><span class="sxs-lookup"><span data-stu-id="d4713-207">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="d4713-208">, код ошибки — 0x80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="d4713-208">', ErrorCode = '0x80070002 : 0.</span></span> <span data-ttu-id="d4713-209">не удалось запустить приложение на пути {ПУТЬ}.</span><span class="sxs-lookup"><span data-stu-id="d4713-209">Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="d4713-210">Исполняемый файл не найден на пути {ПУТЬ}.</span><span class="sxs-lookup"><span data-stu-id="d4713-210">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="d4713-211">Не удалось запустить приложение /LM/W3SVC/2/ROOT, код ошибки — 0x8007023e.</span><span class="sxs-lookup"><span data-stu-id="d4713-211">Failed to start application '/LM/W3SVC/2/ROOT', ErrorCode '0x8007023e'.</span></span>

* <span data-ttu-id="d4713-212">**Журнал stdout модуля ASP.NET Core:** файл журнала не создан.</span><span class="sxs-lookup"><span data-stu-id="d4713-212">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="d4713-213">**Журнал отладки модуля ASP.NET Core:** Журнал событий: не удалось запустить приложение на пути {ПУТЬ}.</span><span class="sxs-lookup"><span data-stu-id="d4713-213">**ASP.NET Core Module Debug Log:** Event Log: 'Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="d4713-214">Исполняемый файл не найден на пути {ПУТЬ}.</span><span class="sxs-lookup"><span data-stu-id="d4713-214">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="d4713-215">возвращена ошибка HRESULT: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="d4713-215">Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="d4713-216">**Браузер:** ошибка HTTP 502.5 — сбой процесса</span><span class="sxs-lookup"><span data-stu-id="d4713-216">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="d4713-217">**Журнал приложений:** приложению MACHINE/WEBROOT/APPHOST/{СБОРКА} с физическим корневым каталогом C:\{ПУТЬ}\' не удалось запустить процесс с помощью команды "{...}"</span><span class="sxs-lookup"><span data-stu-id="d4713-217">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="d4713-218">, код ошибки — 0x80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="d4713-218">', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="d4713-219">**Журнал stdout модуля ASP.NET Core:** файл журнала создан, но пуст.</span><span class="sxs-lookup"><span data-stu-id="d4713-219">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="d4713-220">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="d4713-220">Troubleshooting:</span></span>

* <span data-ttu-id="d4713-221">Убедитесь в том, что приложение выполняется локально в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d4713-221">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="d4713-222">Сбой процесса может быть результатом проблемы в приложении.</span><span class="sxs-lookup"><span data-stu-id="d4713-222">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="d4713-223">Дополнительные сведения см. в статьях [Устранение неполадок ASP.NET Core в службах IIS](xref:host-and-deploy/iis/troubleshoot) или [Устранение неполадок ASP.NET Core в службе приложений Azure](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="d4713-223">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="d4713-224">Проверьте атрибут *processPath* для элемента `<aspNetCore>` в файле *web.config*. Он должен иметь значение `dotnet` для зависимого от платформы развертывания (FDD) или `.\{ASSEMBLY}.exe` для [автономного развертывания (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="d4713-224">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's `dotnet` for a framework-dependent deployment (FDD) or `.\{ASSEMBLY}.exe` for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

* <span data-ttu-id="d4713-225">Для зависимого от платформы развертывания файл *dotnet.exe* может быть недоступен по пути, указанному в переменной PATH.</span><span class="sxs-lookup"><span data-stu-id="d4713-225">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="d4713-226">Убедитесь в том, что папка *C:\Program Files\dotnet\\* существует в системных параметрах PATH.</span><span class="sxs-lookup"><span data-stu-id="d4713-226">Confirm that *C:\Program Files\dotnet\\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="d4713-227">В случае с зависимым от платформы развертыванием файл *dotnet.exe* может быть недоступен для удостоверения пользователя пула приложений.</span><span class="sxs-lookup"><span data-stu-id="d4713-227">For an FDD, *dotnet.exe* might not be accessible for the user identity of the app pool.</span></span> <span data-ttu-id="d4713-228">Убедитесь в том, что удостоверение пользователя пула приложений имеет доступ к каталогу *C:\Program Files\dotnet*.</span><span class="sxs-lookup"><span data-stu-id="d4713-228">Confirm that the app pool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="d4713-229">Убедитесь в отсутствии запрещающих правил для удостоверения пользователя пула приложений в каталоге *C:\Program Files\dotnet* и каталоге приложения.</span><span class="sxs-lookup"><span data-stu-id="d4713-229">Confirm that there are no deny rules configured for the app pool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="d4713-230">Возможно, зависимое от платформы развертывание и .NET Core были установлены без перезапуска служб IIS.</span><span class="sxs-lookup"><span data-stu-id="d4713-230">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="d4713-231">Перезагрузите сервер или перезапустите службы IIS, выполнив в командной строке команду **net stop was /y**, а затем — команду **net start w3svc**.</span><span class="sxs-lookup"><span data-stu-id="d4713-231">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="d4713-232">Возможно, зависимое от платформы развертывание было развернуто без установки среды выполнения .NET Core в размещающей системе.</span><span class="sxs-lookup"><span data-stu-id="d4713-232">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="d4713-233">Если среда выполнения .NET Core не была установлена, запустите в целевой системе **установщик пакета размещения .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="d4713-233">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span>

  [<span data-ttu-id="d4713-234">Текущий установщик пакета размещения .NET Core (прямая загрузка)</span><span class="sxs-lookup"><span data-stu-id="d4713-234">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="d4713-235">См. дополнительные сведения об [установке пакета размещения .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="d4713-235">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

  <span data-ttu-id="d4713-236">Если требуется конкретная среда выполнения, скачайте ее на странице [скачивания версий .NET](https://dotnet.microsoft.com/download/archives) и установите в системе.</span><span class="sxs-lookup"><span data-stu-id="d4713-236">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="d4713-237">Завершите установку, перезагрузив систему или перезапустив службы IIS. Для этого выполните в командной строке команду **net stop was /y**, а затем — команду **net start w3svc**.</span><span class="sxs-lookup"><span data-stu-id="d4713-237">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="d4713-238">Возможно, зависимое от платформы развертывание было развернуто в системе, где отсутствует *распространяемый компонент Microsoft Visual C++ 2015 (64-разрядный)*.</span><span class="sxs-lookup"><span data-stu-id="d4713-238">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="d4713-239">Получите установщик в [Центре загрузки Майкрософт](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="d4713-239">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="d4713-240">Неверные аргументы элемента \<aspNetCore></span><span class="sxs-lookup"><span data-stu-id="d4713-240">Incorrect arguments of \<aspNetCore> element</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d4713-241">**Браузер:** ошибка HTTP 500.0 — ошибка загрузки внутрипроцессного обработчика ANCM</span><span class="sxs-lookup"><span data-stu-id="d4713-241">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="d4713-242">**Журнал приложений:** вызов hostfxr для поиска внутрипроцессного обработчика запросов завершился ошибкой без обнаружения каких-либо собственных зависимостей.</span><span class="sxs-lookup"><span data-stu-id="d4713-242">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="d4713-243">Скорее всего, это означает, что приложение настроено неправильно. Проверьте версии Microsoft.NetCore.App и Microsoft.AspNetCore.App, которые являются целевыми для приложения и установлены на компьютере.</span><span class="sxs-lookup"><span data-stu-id="d4713-243">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="d4713-244">Не удалось найти внутрипроцессный обработчик запросов.</span><span class="sxs-lookup"><span data-stu-id="d4713-244">Could not find inprocess request handler.</span></span> <span data-ttu-id="d4713-245">Выходные данные, записанные в результате вызова hostfxr: вы хотели запустить команды пакета SDK для .NET?</span><span class="sxs-lookup"><span data-stu-id="d4713-245">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="d4713-246">Установите пакет SDK для .NET со страницы по адресу: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Не удалось запустить приложение /LM/W3SVC/3/ROOT, код ошибки — 0x8000ffff.</span><span class="sxs-lookup"><span data-stu-id="d4713-246">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed to start application '/LM/W3SVC/3/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="d4713-247">**Журнал stdout модуля ASP.NET Core:** вы хотели запустить команды пакета SDK для .NET?</span><span class="sxs-lookup"><span data-stu-id="d4713-247">**ASP.NET Core Module stdout Log:** Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="d4713-248">Установите пакет SDK для .NET со страницы по адресу: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="d4713-248">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span></span>

* <span data-ttu-id="d4713-249">**Журнал отладки модуля ASP.NET Core:** вызов hostfxr для поиска внутрипроцессного обработчика запросов завершился ошибкой без обнаружения каких-либо собственных зависимостей.</span><span class="sxs-lookup"><span data-stu-id="d4713-249">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="d4713-250">Скорее всего, это означает, что приложение настроено неправильно. Проверьте версии Microsoft.NetCore.App и Microsoft.AspNetCore.App, которые являются целевыми для приложения и установлены на компьютере.</span><span class="sxs-lookup"><span data-stu-id="d4713-250">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="d4713-251">возвращена ошибка HRESULT: 0x8000ffff Не удалось найти внутрипроцессный обработчик запросов.</span><span class="sxs-lookup"><span data-stu-id="d4713-251">Failed HRESULT returned: 0x8000ffff Could not find inprocess request handler.</span></span> <span data-ttu-id="d4713-252">Выходные данные, записанные в результате вызова hostfxr: вы хотели запустить команды пакета SDK для .NET?</span><span class="sxs-lookup"><span data-stu-id="d4713-252">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="d4713-253">Установите пакет SDK для .NET со страницы по адресу: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409Возвращена ошибка HRESULT: 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="d4713-253">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="d4713-254">**Браузер:** ошибка HTTP 502.5 — сбой процесса</span><span class="sxs-lookup"><span data-stu-id="d4713-254">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="d4713-255">**Журнал приложений:** приложению MACHINE/WEBROOT/APPHOST/{СБОРКА} с физическим корневым каталогом C:\{ПУТЬ}\' не удалось запустить процесс с помощью команды "dotnet" .\{СБОРКА}.dll, код ошибки — 0x80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="d4713-255">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"dotnet" .\{ASSEMBLY}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="d4713-256">**Журнал stdout модуля ASP.NET Core:** приложение для выполнения не существует: ПУТЬ\{СБОРКА}.dll</span><span class="sxs-lookup"><span data-stu-id="d4713-256">**ASP.NET Core Module stdout Log:** The application to execute does not exist: 'PATH\{ASSEMBLY}.dll'</span></span>

::: moniker-end

<span data-ttu-id="d4713-257">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="d4713-257">Troubleshooting:</span></span>

* <span data-ttu-id="d4713-258">Убедитесь в том, что приложение выполняется локально в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d4713-258">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="d4713-259">Сбой процесса может быть результатом проблемы в приложении.</span><span class="sxs-lookup"><span data-stu-id="d4713-259">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="d4713-260">Дополнительные сведения см. в статьях [Устранение неполадок ASP.NET Core в службах IIS](xref:host-and-deploy/iis/troubleshoot) или [Устранение неполадок ASP.NET Core в службе приложений Azure](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="d4713-260">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="d4713-261">Проверьте атрибут *arguments* для элемента `<aspNetCore>` в файле *web.config* и удостоверьтесь, что он имеет значение: а) `.\{ASSEMBLY}.dll`для развертывания, зависящего от платформы (FDD); б) отсутствует, содержит пустую строку (`arguments=""`) или список аргументов вашего приложения (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) для автономного развертывания (SCD).</span><span class="sxs-lookup"><span data-stu-id="d4713-261">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) `.\{ASSEMBLY}.dll` for a framework-dependent deployment (FDD); or (b) not present, an empty string (`arguments=""`), or a list of the app's arguments (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) for a self-contained deployment (SCD).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="missing-net-core-shared-framework"></a><span data-ttu-id="d4713-262">Отсутствует общая платформа .NET Core</span><span class="sxs-lookup"><span data-stu-id="d4713-262">Missing .NET Core shared framework</span></span>

* <span data-ttu-id="d4713-263">**Браузер:** ошибка HTTP 500.0 — ошибка загрузки внутрипроцессного обработчика ANCM</span><span class="sxs-lookup"><span data-stu-id="d4713-263">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="d4713-264">**Журнал приложений:** вызов hostfxr для поиска внутрипроцессного обработчика запросов завершился ошибкой без обнаружения каких-либо собственных зависимостей.</span><span class="sxs-lookup"><span data-stu-id="d4713-264">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="d4713-265">Скорее всего, это означает, что приложение настроено неправильно. Проверьте версии Microsoft.NetCore.App и Microsoft.AspNetCore.App, которые являются целевыми для приложения и установлены на компьютере.</span><span class="sxs-lookup"><span data-stu-id="d4713-265">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="d4713-266">Не удалось найти внутрипроцессный обработчик запросов.</span><span class="sxs-lookup"><span data-stu-id="d4713-266">Could not find inprocess request handler.</span></span> <span data-ttu-id="d4713-267">Выходные данные, записанные в результате вызова hostfxr: не удалось найти совместимую версию платформы.</span><span class="sxs-lookup"><span data-stu-id="d4713-267">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="d4713-268">Указанная версия {ВЕРСИЯ} платформы Microsoft.AspNetCore.App не найдена.</span><span class="sxs-lookup"><span data-stu-id="d4713-268">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

<span data-ttu-id="d4713-269">Не удалось запустить приложение /LM/W3SVC/5/ROOT, код ошибки — 0x8000ffff.</span><span class="sxs-lookup"><span data-stu-id="d4713-269">Failed to start application '/LM/W3SVC/5/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="d4713-270">**Журнал stdout модуля ASP.NET Core:** не удалось найти совместимую версию платформы.</span><span class="sxs-lookup"><span data-stu-id="d4713-270">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="d4713-271">Указанная версия {ВЕРСИЯ} платформы Microsoft.AspNetCore.App не найдена.</span><span class="sxs-lookup"><span data-stu-id="d4713-271">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

* <span data-ttu-id="d4713-272">**Журнал отладки модуля ASP.NET Core:** возвращена ошибка HRESULT: 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="d4713-272">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

<span data-ttu-id="d4713-273">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="d4713-273">Troubleshooting:</span></span>

<span data-ttu-id="d4713-274">Если используется зависимое от платформы развертывание (FDD), убедитесь в том, что в системе установлена нужная среда выполнения.</span><span class="sxs-lookup"><span data-stu-id="d4713-274">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="d4713-275">Пул приложений остановлен</span><span class="sxs-lookup"><span data-stu-id="d4713-275">Stopped Application Pool</span></span>

* <span data-ttu-id="d4713-276">**Браузер:** 503 — служба недоступна</span><span class="sxs-lookup"><span data-stu-id="d4713-276">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="d4713-277">**Журнал приложений:** нет записи</span><span class="sxs-lookup"><span data-stu-id="d4713-277">**Application Log:** No entry</span></span>

* <span data-ttu-id="d4713-278">**Журнал stdout модуля ASP.NET Core:** файл журнала не создан.</span><span class="sxs-lookup"><span data-stu-id="d4713-278">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d4713-279">**Журнал отладки модуля ASP.NET Core:** файл журнала не создан.</span><span class="sxs-lookup"><span data-stu-id="d4713-279">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="d4713-280">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="d4713-280">Troubleshooting:</span></span>

<span data-ttu-id="d4713-281">Убедитесь, что пул приложений не находится в состоянии *Остановлен*.</span><span class="sxs-lookup"><span data-stu-id="d4713-281">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="d4713-282">Дочернее приложение имеет раздел \<handlers></span><span class="sxs-lookup"><span data-stu-id="d4713-282">Sub-application includes a \<handlers> section</span></span>

* <span data-ttu-id="d4713-283">**Браузер:** ошибка HTTP 500.19 — внутренняя ошибка сервера</span><span class="sxs-lookup"><span data-stu-id="d4713-283">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="d4713-284">**Журнал приложений:** нет записи</span><span class="sxs-lookup"><span data-stu-id="d4713-284">**Application Log:** No entry</span></span>

* <span data-ttu-id="d4713-285">**Журнал stdout модуля ASP.NET Core:** корневой файл журнала приложения создан и работает нормально.</span><span class="sxs-lookup"><span data-stu-id="d4713-285">**ASP.NET Core Module stdout Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="d4713-286">Файл журнала дочернего приложения не создан.</span><span class="sxs-lookup"><span data-stu-id="d4713-286">The sub-app's log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d4713-287">**Журнал отладки модуля ASP.NET Core:** корневой файл журнала приложения создан и работает нормально.</span><span class="sxs-lookup"><span data-stu-id="d4713-287">**ASP.NET Core Module Debug Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="d4713-288">Файл журнала дочернего приложения не создан.</span><span class="sxs-lookup"><span data-stu-id="d4713-288">The sub-app's log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="d4713-289">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="d4713-289">Troubleshooting:</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d4713-290">Убедитесь, что файл *web.config* дочернего приложения не содержит раздел `<handlers>` или что дочернее приложение не наследует обработчики родительского приложения.</span><span class="sxs-lookup"><span data-stu-id="d4713-290">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section or that the sub-app doesn't inherit the parent app's handlers.</span></span>

<span data-ttu-id="d4713-291">Раздел `<system.webServer>` файла *web.config* находится внутри элемента `<location>`.</span><span class="sxs-lookup"><span data-stu-id="d4713-291">The parent app's `<system.webServer>` section of *web.config* is placed inside of a `<location>` element.</span></span> <span data-ttu-id="d4713-292">Значение `false` свойства <xref:System.Configuration.SectionInformation.InheritInChildApplications*> указывает, что параметры, заданные в элементе [\<расположение>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location), не наследуются приложениями, которые находятся во вложенном каталоге родительского приложения.</span><span class="sxs-lookup"><span data-stu-id="d4713-292">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the parent app.</span></span> <span data-ttu-id="d4713-293">Для получения дополнительной информации см. <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="d4713-293">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="d4713-294">В файле *web.config* дочернего приложения не должно быть раздела `<handlers>`.</span><span class="sxs-lookup"><span data-stu-id="d4713-294">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

::: moniker-end

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="d4713-295">Неверный путь к журналу stdout</span><span class="sxs-lookup"><span data-stu-id="d4713-295">stdout log path incorrect</span></span>

* <span data-ttu-id="d4713-296">**Браузер:** приложение работает правильно.</span><span class="sxs-lookup"><span data-stu-id="d4713-296">**Browser:** The app responds normally.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d4713-297">**Журнал приложений:** не удалось запустить перенаправление stdout в C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="d4713-297">**Application Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="d4713-298">Сообщение об исключении: по пути {ПУТЬ}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84 возвращено значение HRESULT — 0x80070005.</span><span class="sxs-lookup"><span data-stu-id="d4713-298">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="d4713-299">Не удалось остановить перенаправление stdout в C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="d4713-299">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="d4713-300">Сообщение об исключении: по пути {ПУТЬ} возвращено значение HRESULT — 0x80070002.</span><span class="sxs-lookup"><span data-stu-id="d4713-300">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="d4713-301">Не удалось запустить перенаправление stdout по пути {ПУТЬ}\aspnetcorev2_inprocess.dll.</span><span class="sxs-lookup"><span data-stu-id="d4713-301">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

* <span data-ttu-id="d4713-302">**Журнал stdout модуля ASP.NET Core:** файл журнала не создан.</span><span class="sxs-lookup"><span data-stu-id="d4713-302">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="d4713-303">**Журнал отладки модуля ASP.NET Core:** не удалось запустить перенаправление stdout в C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="d4713-303">**ASP.NET Core Module debug Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="d4713-304">Сообщение об исключении: по пути {ПУТЬ}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84 возвращено значение HRESULT — 0x80070005.</span><span class="sxs-lookup"><span data-stu-id="d4713-304">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="d4713-305">Не удалось остановить перенаправление stdout в C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="d4713-305">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="d4713-306">Сообщение об исключении: по пути {ПУТЬ} возвращено значение HRESULT — 0x80070002.</span><span class="sxs-lookup"><span data-stu-id="d4713-306">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="d4713-307">Не удалось запустить перенаправление stdout по пути {ПУТЬ}\aspnetcorev2_inprocess.dll.</span><span class="sxs-lookup"><span data-stu-id="d4713-307">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="d4713-308">**Журнал приложений:** Предупреждение: не удалось создать stdoutLogFile \\?\{ ПУТЬ} \путь_не_существует\stdout_{ИД_ПРОЦЕССА} _ {МЕТКА_ВРЕМЕНИ}.log, код ошибки — -2147024893.</span><span class="sxs-lookup"><span data-stu-id="d4713-308">**Application Log:** Warning: Could not create stdoutLogFile \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="d4713-309">**Журнал stdout модуля ASP.NET Core:** файл журнала не создан.</span><span class="sxs-lookup"><span data-stu-id="d4713-309">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="d4713-310">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="d4713-310">Troubleshooting:</span></span>

* <span data-ttu-id="d4713-311">Не существует путь `stdoutLogFile`, указанный в элементе `<aspNetCore>` в файле *web.config*.</span><span class="sxs-lookup"><span data-stu-id="d4713-311">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="d4713-312">Дополнительные сведения см. в разделе [Создание и и перенаправление журнала](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span><span class="sxs-lookup"><span data-stu-id="d4713-312">For more information, see [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span>

* <span data-ttu-id="d4713-313">Пользователь пула приложения не имеет прав на запись в путь к папке журнала stdout.</span><span class="sxs-lookup"><span data-stu-id="d4713-313">The app pool user doesn't have write access to the stdout log path.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="d4713-314">Общая проблема с конфигурацией приложения</span><span class="sxs-lookup"><span data-stu-id="d4713-314">Application configuration general issue</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d4713-315">**Браузер:** ошибка HTTP 500.0 — ошибка загрузки внутрипроцессного обработчика ANCM**или**ошибка HTTP 500.30 — сбой в процессе запуска ANCM</span><span class="sxs-lookup"><span data-stu-id="d4713-315">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure **--OR--** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="d4713-316">**Журнал приложений:** Переменная</span><span class="sxs-lookup"><span data-stu-id="d4713-316">**Application Log:** Variable</span></span>

* <span data-ttu-id="d4713-317">**Журнал stdout модуля ASP.NET Core:** файл журнала создан, но пуст, или создан с обычными записями до точки сбоя приложения.</span><span class="sxs-lookup"><span data-stu-id="d4713-317">**ASP.NET Core Module stdout Log:** The log file is created but empty or created with normal entries until the point of the app failing.</span></span>

* <span data-ttu-id="d4713-318">**Журнал отладки модуля ASP.NET Core:** Переменная</span><span class="sxs-lookup"><span data-stu-id="d4713-318">**ASP.NET Core Module Debug Log:** Variable</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="d4713-319">**Браузер:** ошибка HTTP 502.5 — сбой процесса</span><span class="sxs-lookup"><span data-stu-id="d4713-319">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="d4713-320">**Журнал приложений:** приложение MACHINE/WEBROOT/APPHOST/{СБОРКА} с физическим корневым каталогом C:\{ПУТЬ}\' создало процесс с помощью команды "C:\{ПУТЬ}\{СБОРКА}.{exe|dll}", однако при этом либо произошел сбой, либо не был получен ответ, либо не прослушивался указанный порт {ПОРТ}; код ошибки — {КОД ОШИБКИ}</span><span class="sxs-lookup"><span data-stu-id="d4713-320">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' created process with commandline '"C:\{PATH}\{ASSEMBLY}.{exe|dll}" ' but either crashed or did not respond or did not listen on the given port '{PORT}', ErrorCode = '{ERROR CODE}'</span></span>

* <span data-ttu-id="d4713-321">**Журнал stdout модуля ASP.NET Core:** файл журнала создан, но пуст.</span><span class="sxs-lookup"><span data-stu-id="d4713-321">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="d4713-322">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="d4713-322">Troubleshooting:</span></span>

<span data-ttu-id="d4713-323">Процесс не удалось запустить, скорее всего, из-за проблемы с конфигурацией приложения или ошибки программирования.</span><span class="sxs-lookup"><span data-stu-id="d4713-323">The process failed to start, most likely due to an app configuration or programming issue.</span></span>

<span data-ttu-id="d4713-324">Дополнительные сведения см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="d4713-324">For more information, see the following topics:</span></span>

* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:test/troubleshoot>
