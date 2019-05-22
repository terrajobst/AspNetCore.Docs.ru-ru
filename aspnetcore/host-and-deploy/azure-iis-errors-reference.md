---
title: Справочник по общим ошибкам в Службе приложений Azure и службах IIS с ASP.NET Core
author: guardrex
description: Рекомендации по устранению распространенных ошибок при размещении приложений ASP.NET Core в службе приложений Azure и службах IIS.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/12/2019
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 0191460f8c3dab98e6f977a29eacf0396b6789d8
ms.sourcegitcommit: b4ef2b00f3e1eb287138f8b43c811cb35a100d3e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/21/2019
ms.locfileid: "65970074"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="3add1-103">Справочник по общим ошибкам в Службе приложений Azure и службах IIS с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3add1-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="3add1-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="3add1-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3add1-105">В этой статье приводятся рекомендации по устранению распространенных ошибок при размещении приложений ASP.NET Core в службе приложений Azure и службах IIS.</span><span class="sxs-lookup"><span data-stu-id="3add1-105">This topic offers troubleshooting advice for common errors when hosting ASP.NET Core apps on Azure Apps Service and IIS.</span></span>

<span data-ttu-id="3add1-106">Соберите следующие сведения:</span><span class="sxs-lookup"><span data-stu-id="3add1-106">Collect the following information:</span></span>

* <span data-ttu-id="3add1-107">поведение обозревателя (код состояния и сообщение об ошибке);</span><span class="sxs-lookup"><span data-stu-id="3add1-107">Browser behavior (status code and error message)</span></span>
* <span data-ttu-id="3add1-108">записи в журнале событий приложения;</span><span class="sxs-lookup"><span data-stu-id="3add1-108">Application Event Log entries</span></span>
  * <span data-ttu-id="3add1-109">Служба приложений Azure &ndash; см. статью <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="3add1-109">Azure App Service &ndash; See <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>
  * <span data-ttu-id="3add1-110">IIS</span><span class="sxs-lookup"><span data-stu-id="3add1-110">IIS</span></span>
    1. <span data-ttu-id="3add1-111">В меню **Windows** нажмите кнопку **Пуск**, введите *Просмотр событий* и нажмите клавишу **ВВОД**.</span><span class="sxs-lookup"><span data-stu-id="3add1-111">Select **Start** on the **Windows** menu, type *Event Viewer*, and press **Enter**.</span></span>
    1. <span data-ttu-id="3add1-112">В открывшемся окне **Просмотр событий** на боковой панели разверните узлы **Журналы Windows** > **Приложения**.</span><span class="sxs-lookup"><span data-stu-id="3add1-112">After the **Event Viewer** opens, expand **Windows Logs** > **Application** in the sidebar.</span></span>
* <span data-ttu-id="3add1-113">Записи в журнале вывода stdout и отладки модуля ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3add1-113">ASP.NET Core Module stdout and debug log entries</span></span>
  * <span data-ttu-id="3add1-114">Служба приложений Azure &ndash; см. статью <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="3add1-114">Azure App Service &ndash; See <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>
  * <span data-ttu-id="3add1-115">IIS &ndash; следуйте инструкциям в разделах [Создание и перенаправление журнала](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) и [Расширенные журналы диагностики](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) в статье "Модуль ASP.NET Core".</span><span class="sxs-lookup"><span data-stu-id="3add1-115">IIS &ndash; Follow the instructions in the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) and [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sections of the ASP.NET Core Module topic.</span></span>

<span data-ttu-id="3add1-116">Сравните собранные данные с представленной здесь информацией о распространенных ошибках.</span><span class="sxs-lookup"><span data-stu-id="3add1-116">Compare error information to the following common errors.</span></span> <span data-ttu-id="3add1-117">Если вы найдете соответствие, выполните рекомендации по устранению неполадок.</span><span class="sxs-lookup"><span data-stu-id="3add1-117">If a match is found, follow the troubleshooting advice.</span></span>

<span data-ttu-id="3add1-118">Приведенный ниже перечень ошибок не является исчерпывающим.</span><span class="sxs-lookup"><span data-stu-id="3add1-118">The list of errors in this topic isn't exhaustive.</span></span> <span data-ttu-id="3add1-119">Если вы встретите сообщение об ошибке, которое здесь не указано, воспользуйтесь кнопкой **Обратная связь** в нижней части этой статьи, чтобы создать запрос с подробным описанием, которое поможет воспроизвести эту ошибку.</span><span class="sxs-lookup"><span data-stu-id="3add1-119">If you encounter an error not listed here, open a new issue using the **Content feedback** button at the bottom of this topic with detailed instructions on how to reproduce the error.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="3add1-120">Установщику не удается получить распространяемый компонент VC++</span><span class="sxs-lookup"><span data-stu-id="3add1-120">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="3add1-121">**Исключение установщика:** 0x80072efd**или**0x80072f76 — неопределенная ошибка</span><span class="sxs-lookup"><span data-stu-id="3add1-121">**Installer Exception:** 0x80072efd **--OR--** 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="3add1-122">**Исключение журнала установщика&#8224;:** ошибка 0x80072efd**или**0x80072f76: ошибка выполнения пакета EXE</span><span class="sxs-lookup"><span data-stu-id="3add1-122">**Installer Log Exception&#8224;:** Error 0x80072efd **--OR--** 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="3add1-123">&#8224;Журнал находится в папке *C:\Users\{ПОЛЬЗОВАТЕЛЬ}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{МЕТКА_ВРЕМЕНИ}.log*.</span><span class="sxs-lookup"><span data-stu-id="3add1-123">&#8224;The log is located at *C:\Users\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*.</span></span>

<span data-ttu-id="3add1-124">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="3add1-124">Troubleshooting:</span></span>

<span data-ttu-id="3add1-125">Если при [установке пакета размещения .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) у системы нет доступа к Интернету, это исключение возникает при попытке установщика получить распространяемый компонент *Microsoft Visual C++ 2015*.</span><span class="sxs-lookup"><span data-stu-id="3add1-125">If the system doesn't have Internet access while [installing the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="3add1-126">Получите установщик в [Центре загрузки Майкрософт](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="3add1-126">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="3add1-127">Если работа установщика завершается сбоем, это может быть связано с тем, что сервер не может получить среду выполнения .NET Core, необходимую для размещения [зависимых от платформы развертываний (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="3add1-127">If the installer fails, the server may not receive the .NET Core runtime required to host a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span> <span data-ttu-id="3add1-128">Если вы размещаете зависимые от платформы развертывания, в окне **Программы и компоненты** или **Приложения и компоненты** убедитесь, что установлена среда выполнения.</span><span class="sxs-lookup"><span data-stu-id="3add1-128">If hosting an FDD, confirm that the runtime is installed in **Programs & Features** or **Apps & features**.</span></span> <span data-ttu-id="3add1-129">Если требуется конкретная среда выполнения, скачайте ее на странице [скачивания версий .NET](https://dotnet.microsoft.com/download/archives) и установите в системе.</span><span class="sxs-lookup"><span data-stu-id="3add1-129">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="3add1-130">После установки среды выполнения перезагрузите систему или перезапустите службы IIS, выполнив в командной строке команду **net stop was /y**, а затем — команду **net start w3svc**.</span><span class="sxs-lookup"><span data-stu-id="3add1-130">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="3add1-131">При обновлении ОС был удален 32-разрядный модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3add1-131">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

<span data-ttu-id="3add1-132">**Журнал приложений:** не удалось загрузить DLL модуля **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**.</span><span class="sxs-lookup"><span data-stu-id="3add1-132">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="3add1-133">Ошибочные данные.</span><span class="sxs-lookup"><span data-stu-id="3add1-133">The data is the error.</span></span>

<span data-ttu-id="3add1-134">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="3add1-134">Troubleshooting:</span></span>

<span data-ttu-id="3add1-135">Не относящиеся к ОС файлы в каталоге **C:\Windows\SysWOW64\inetsrv** не сохраняются при обновлении ОС.</span><span class="sxs-lookup"><span data-stu-id="3add1-135">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="3add1-136">Эта ошибка возникает, если модуль ASP.NET Core был установлен до обновления ОС, а после обновления ОС выполняется любой пул приложений в 32-разрядном режиме.</span><span class="sxs-lookup"><span data-stu-id="3add1-136">If the ASP.NET Core Module is installed prior to an OS upgrade and then any app pool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="3add1-137">После обновления ОС восстановите модуль ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3add1-137">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="3add1-138">См. статью [Установка пакета размещения .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="3add1-138">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="3add1-139">Выберите вариант **Восстановить** при запуске установщика.</span><span class="sxs-lookup"><span data-stu-id="3add1-139">Select **Repair** when the installer is run.</span></span>

## <a name="missing-site-extension-32-bit-x86-and-64-bit-x64-site-extensions-installed-or-wrong-process-bitness-set"></a><span data-ttu-id="3add1-140">Отсутствует расширение сайта, установлены 32-разрядное (x86) и 64-разрядное (x64) расширения сайта или неправильно задана разрядность</span><span class="sxs-lookup"><span data-stu-id="3add1-140">Missing site extension, 32-bit (x86) and 64-bit (x64) site extensions installed, or wrong process bitness set</span></span>

<span data-ttu-id="3add1-141">*Относится к приложениям, размещенным в Службе приложений Azure*.</span><span class="sxs-lookup"><span data-stu-id="3add1-141">*Applies to apps hosted by Azure App Services.*</span></span>

* <span data-ttu-id="3add1-142">**Браузер:** ошибка HTTP 500.0 — ошибка загрузки внутрипроцессного обработчика ANCM</span><span class="sxs-lookup"><span data-stu-id="3add1-142">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="3add1-143">**Журнал приложений:** вызов hostfxr для поиска внутрипроцессного обработчика запросов завершился ошибкой без обнаружения каких-либо собственных зависимостей.</span><span class="sxs-lookup"><span data-stu-id="3add1-143">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="3add1-144">Не удалось найти внутрипроцессный обработчик запросов.</span><span class="sxs-lookup"><span data-stu-id="3add1-144">Could not find inprocess request handler.</span></span> <span data-ttu-id="3add1-145">Выходные данные, записанные в результате вызова hostfxr: не удалось найти совместимую версию платформы.</span><span class="sxs-lookup"><span data-stu-id="3add1-145">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="3add1-146">Указанная версия {версия} — предварительная версия — \* платформы Microsoft.AspNetCore.App не найдена.</span><span class="sxs-lookup"><span data-stu-id="3add1-146">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span> <span data-ttu-id="3add1-147">Не удалось запустить приложение /LM/W3SVC/1416782824/ROOT, код ошибки — 0x8000ffff.</span><span class="sxs-lookup"><span data-stu-id="3add1-147">Failed to start application '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="3add1-148">**Журнал stdout модуля ASP.NET Core:** не удалось найти совместимую версию платформы.</span><span class="sxs-lookup"><span data-stu-id="3add1-148">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="3add1-149">Указанная версия {версия} — предварительная версия — \* платформы Microsoft.AspNetCore.App не найдена.</span><span class="sxs-lookup"><span data-stu-id="3add1-149">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="3add1-150">**Журнал отладки модуля ASP.NET Core:** вызов hostfxr для поиска внутрипроцессного обработчика запросов завершился ошибкой без обнаружения каких-либо собственных зависимостей.</span><span class="sxs-lookup"><span data-stu-id="3add1-150">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="3add1-151">Скорее всего, это означает, что приложение настроено неправильно. Проверьте версии Microsoft.NetCore.App и Microsoft.AspNetCore.App, которые являются целевыми для приложения и установлены на компьютере.</span><span class="sxs-lookup"><span data-stu-id="3add1-151">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="3add1-152">возвращена ошибка HRESULT: 0x8000ffff.</span><span class="sxs-lookup"><span data-stu-id="3add1-152">Failed HRESULT returned: 0x8000ffff.</span></span> <span data-ttu-id="3add1-153">Не удалось найти внутрипроцессный обработчик запросов.</span><span class="sxs-lookup"><span data-stu-id="3add1-153">Could not find inprocess request handler.</span></span> <span data-ttu-id="3add1-154">не удалось найти совместимую версию платформы.</span><span class="sxs-lookup"><span data-stu-id="3add1-154">It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="3add1-155">Указанная версия {версия} — предварительная версия — \* платформы Microsoft.AspNetCore.App не найдена.</span><span class="sxs-lookup"><span data-stu-id="3add1-155">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

::: moniker-end

<span data-ttu-id="3add1-156">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="3add1-156">Troubleshooting:</span></span>

* <span data-ttu-id="3add1-157">Если приложение запускается в среде выполнения предварительной версии, установите 32-разрядное (x86) **или** 64-разрядное (x64) расширение сайта в соответствии с разрядностью и версией среды выполнения приложения.</span><span class="sxs-lookup"><span data-stu-id="3add1-157">If running the app on a preview runtime, install either the 32-bit (x86) **or** 64-bit (x64) site extension that matches the bitness of the app and the app's runtime version.</span></span> <span data-ttu-id="3add1-158">**Не следует устанавливать оба расширения или несколько версий среды выполнения расширения**.</span><span class="sxs-lookup"><span data-stu-id="3add1-158">**Don't install both extensions or multiple runtime versions of the extension.**</span></span>

  * <span data-ttu-id="3add1-159">Среда выполнения ASP.NET Core {версия среды выполнения} (x86)</span><span class="sxs-lookup"><span data-stu-id="3add1-159">ASP.NET Core {RUNTIME VERSION} (x86) Runtime</span></span>
  * <span data-ttu-id="3add1-160">Среда выполнения ASP.NET Core {версия среды выполнения} (x64)</span><span class="sxs-lookup"><span data-stu-id="3add1-160">ASP.NET Core {RUNTIME VERSION} (x64) Runtime</span></span>

  <span data-ttu-id="3add1-161">Перезапустите приложение.</span><span class="sxs-lookup"><span data-stu-id="3add1-161">Restart the app.</span></span> <span data-ttu-id="3add1-162">Подождите несколько секунд, пока приложение перезагрузится.</span><span class="sxs-lookup"><span data-stu-id="3add1-162">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="3add1-163">Если приложение запускается в среде выполнения предварительной версии и установлены оба [расширения сайта](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) — 32-разрядное (x86) и 64-разрядное (x64), удалите расширение сайта, не соответствующее разрядности приложения.</span><span class="sxs-lookup"><span data-stu-id="3add1-163">If running the app on a preview runtime and both the 32-bit (x86) and 64-bit (x64) [site extensions](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) are installed, uninstall the site extension that doesn't match the bitness of the app.</span></span> <span data-ttu-id="3add1-164">После удаления расширения сайта перезапустите приложение.</span><span class="sxs-lookup"><span data-stu-id="3add1-164">After removing the site extension, restart the app.</span></span> <span data-ttu-id="3add1-165">Подождите несколько секунд, пока приложение перезагрузится.</span><span class="sxs-lookup"><span data-stu-id="3add1-165">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="3add1-166">Если приложение запускается в среде выполнения предварительной версии, и разрядность расширения сайта соответствует разрядности приложения, убедитесь, что предварительная *версия среды выполнения* для расширения сайта соответствует версии среды выполнения приложения.</span><span class="sxs-lookup"><span data-stu-id="3add1-166">If running the app on a preview runtime and the site extension's bitness matches that of the app, confirm that the preview site extension's *runtime version* matches the app's runtime version.</span></span>

* <span data-ttu-id="3add1-167">Убедитесь, что заданная для приложения **платформа** в разделе **Параметры приложения** соответствует разрядности приложения.</span><span class="sxs-lookup"><span data-stu-id="3add1-167">Confirm that the app's **Platform** in **Application Settings** matches the bitness of the app.</span></span>

<span data-ttu-id="3add1-168">Для получения дополнительной информации см. <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.</span><span class="sxs-lookup"><span data-stu-id="3add1-168">For more information, see <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.</span></span>

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a><span data-ttu-id="3add1-169">Приложение x86 развернуто, но для 32-разрядных приложений не включен пул приложений</span><span class="sxs-lookup"><span data-stu-id="3add1-169">An x86 app is deployed but the app pool isn't enabled for 32-bit apps</span></span>

* <span data-ttu-id="3add1-170">**Браузер:** ошибка HTTP 500.30 — сбой в процессе запуска ANCM</span><span class="sxs-lookup"><span data-stu-id="3add1-170">**Browser:** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="3add1-171">**Журнал приложений:** для приложения "/LM/W3SVC/5/ROOT" с физическим корневым каталогом "{ПУТЬ}" возникло непредвиденное управляемое исключение, код исключения — 0xe0434352.</span><span class="sxs-lookup"><span data-stu-id="3add1-171">**Application Log:** Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' hit unexpected managed exception, exception code = '0xe0434352'.</span></span> <span data-ttu-id="3add1-172">Дополнительные сведения см. в журналах sderr.</span><span class="sxs-lookup"><span data-stu-id="3add1-172">Please check the stderr logs for more information.</span></span> <span data-ttu-id="3add1-173">Приложению /LM/W3SVC/5/ROOTс физическим корневым каталогом {ПУТЬ} не удалось загрузить clr и управляемое приложение.</span><span class="sxs-lookup"><span data-stu-id="3add1-173">Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' failed to load clr and managed application.</span></span> <span data-ttu-id="3add1-174">Рабочий поток CLR преждевременно завершил работу.</span><span class="sxs-lookup"><span data-stu-id="3add1-174">CLR worker thread exited prematurely</span></span>

* <span data-ttu-id="3add1-175">**Журнал stdout модуля ASP.NET Core:** файл журнала создан, но пуст.</span><span class="sxs-lookup"><span data-stu-id="3add1-175">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="3add1-176">**Журнал отладки модуля ASP.NET Core:** возвращена ошибка HRESULT: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="3add1-176">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

<span data-ttu-id="3add1-177">Пакет SDK перехватывает этот сценарий при публикации автономного приложения.</span><span class="sxs-lookup"><span data-stu-id="3add1-177">This scenario is trapped by the SDK when publishing a self-contained app.</span></span> <span data-ttu-id="3add1-178">Пакет SDK выводит сообщение об ошибке, если идентификатор RID не соответствует целевой платформе (например, RID `win10-x64` с `<PlatformTarget>x86</PlatformTarget>` в файле проекта).</span><span class="sxs-lookup"><span data-stu-id="3add1-178">The SDK produces an error if the RID doesn't match the platform target (for example, `win10-x64` RID with `<PlatformTarget>x86</PlatformTarget>` in the project file).</span></span>

<span data-ttu-id="3add1-179">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="3add1-179">Troubleshooting:</span></span>

<span data-ttu-id="3add1-180">Если используется зависимое от платформы (x86) развертывание (`<PlatformTarget>x86</PlatformTarget>`), включите пул приложений IIS для 32-разрядных приложений.</span><span class="sxs-lookup"><span data-stu-id="3add1-180">For an x86 framework-dependent deployment (`<PlatformTarget>x86</PlatformTarget>`), enable the IIS app pool for 32-bit apps.</span></span> <span data-ttu-id="3add1-181">В диспетчере IIS откройте раздел **Дополнительные параметры** пула приложений и задайте параметру **Разрешены 32-разрядные приложения** значение **True**.</span><span class="sxs-lookup"><span data-stu-id="3add1-181">In IIS Manager, open the app pool's **Advanced Settings** and set **Enable 32-Bit Applications** to **True**.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="3add1-182">Конфликты платформы с относительным идентификатором</span><span class="sxs-lookup"><span data-stu-id="3add1-182">Platform conflicts with RID</span></span>

* <span data-ttu-id="3add1-183">**Браузер:** ошибка HTTP 502.5 — сбой процесса</span><span class="sxs-lookup"><span data-stu-id="3add1-183">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="3add1-184">**Журнал приложений:** приложению MACHINE/WEBROOT/APPHOST/{СБОРКА} с физическим корневым каталогом C:\{ПУТЬ}\' не удалось запустить процесс с помощью команды "C:\{ПУТЬ}\{СБОРКА}.{exe|dll}"; код ошибки — 0x80004005 : ff.</span><span class="sxs-lookup"><span data-stu-id="3add1-184">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="3add1-185">**Журнал stdout модуля ASP.NET Core:** необработанное исключение: System.BadImageFormatException: не удалось загрузить файл или сборку {СБОРКА}.dll.</span><span class="sxs-lookup"><span data-stu-id="3add1-185">**ASP.NET Core Module stdout Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{ASSEMBLY}.dll'.</span></span> <span data-ttu-id="3add1-186">Была сделана попытка загрузить программу, имеющую неверный формат.</span><span class="sxs-lookup"><span data-stu-id="3add1-186">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="3add1-187">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="3add1-187">Troubleshooting:</span></span>

* <span data-ttu-id="3add1-188">Убедитесь в том, что приложение выполняется локально в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3add1-188">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="3add1-189">Сбой процесса может быть результатом проблемы в приложении.</span><span class="sxs-lookup"><span data-stu-id="3add1-189">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="3add1-190">Дополнительные сведения см. в статьях [Устранение неполадок ASP.NET Core в службах IIS](xref:host-and-deploy/iis/troubleshoot) или [Устранение неполадок ASP.NET Core в службе приложений Azure](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="3add1-190">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="3add1-191">Если это исключение возникает для развертывания приложений Azure при обновлении приложения и развертывании более новых сборок, вручную удалите все файлы предыдущего развертывания.</span><span class="sxs-lookup"><span data-stu-id="3add1-191">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="3add1-192">Если останутся несовместимые сборки, то при развертывании обновленного приложения это может привести к исключению `System.BadImageFormatException`.</span><span class="sxs-lookup"><span data-stu-id="3add1-192">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="3add1-193">Неправильная конечная точка URI, либо веб-сайт остановлен</span><span class="sxs-lookup"><span data-stu-id="3add1-193">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="3add1-194">**Браузер:** ERR_CONNECTION_REFUSED**или** не удается подключиться</span><span class="sxs-lookup"><span data-stu-id="3add1-194">**Browser:** ERR_CONNECTION_REFUSED **--OR--** Unable to connect</span></span>

* <span data-ttu-id="3add1-195">**Журнал приложений:** нет записи</span><span class="sxs-lookup"><span data-stu-id="3add1-195">**Application Log:** No entry</span></span>

* <span data-ttu-id="3add1-196">**Журнал stdout модуля ASP.NET Core:** файл журнала не создан.</span><span class="sxs-lookup"><span data-stu-id="3add1-196">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="3add1-197">**Журнал отладки модуля ASP.NET Core:** файл журнала не создан.</span><span class="sxs-lookup"><span data-stu-id="3add1-197">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="3add1-198">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="3add1-198">Troubleshooting:</span></span>

* <span data-ttu-id="3add1-199">Убедитесь, что используется правильный URI конечной точки для приложения.</span><span class="sxs-lookup"><span data-stu-id="3add1-199">Confirm the correct URI endpoint for the app is in use.</span></span> <span data-ttu-id="3add1-200">Проверьте все привязки.</span><span class="sxs-lookup"><span data-stu-id="3add1-200">Check the bindings.</span></span>

* <span data-ttu-id="3add1-201">Убедитесь, что веб-сайт IIS не находится в состоянии *Остановлен*.</span><span class="sxs-lookup"><span data-stu-id="3add1-201">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="3add1-202">Компонент сервера CoreWebEngine или W3SVC отключен</span><span class="sxs-lookup"><span data-stu-id="3add1-202">CoreWebEngine or W3SVC server features disabled</span></span>

<span data-ttu-id="3add1-203">**Исключение ОС:** для использования модуля ASP.NET Core должны быть установлены компоненты IIS 7.0 CoreWebEngine и W3SVC.</span><span class="sxs-lookup"><span data-stu-id="3add1-203">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="3add1-204">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="3add1-204">Troubleshooting:</span></span>

<span data-ttu-id="3add1-205">Убедитесь, что включены правильная роль и необходимые компоненты.</span><span class="sxs-lookup"><span data-stu-id="3add1-205">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="3add1-206">См. раздел [Конфигурация IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="3add1-206">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="3add1-207">Неправильный физический путь к веб-сайту, либо отсутствует приложение</span><span class="sxs-lookup"><span data-stu-id="3add1-207">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="3add1-208">**Браузер:** 403 — запрещено. В доступе отказано**или**403.14 — запрещено. Веб-сервер настроен таким образом, чтобы не формировать список содержимого каталога.</span><span class="sxs-lookup"><span data-stu-id="3add1-208">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="3add1-209">**Журнал приложений:** нет записи</span><span class="sxs-lookup"><span data-stu-id="3add1-209">**Application Log:** No entry</span></span>

* <span data-ttu-id="3add1-210">**Журнал stdout модуля ASP.NET Core:** файл журнала не создан.</span><span class="sxs-lookup"><span data-stu-id="3add1-210">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="3add1-211">**Журнал отладки модуля ASP.NET Core:** файл журнала не создан.</span><span class="sxs-lookup"><span data-stu-id="3add1-211">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="3add1-212">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="3add1-212">Troubleshooting:</span></span>

<span data-ttu-id="3add1-213">Проверьте **основные настройки** веб-сайта IIS и физическую папку приложения.</span><span class="sxs-lookup"><span data-stu-id="3add1-213">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="3add1-214">Убедитесь в том, что приложение находится в папке по **физическому пути**, указанному для веб-сайта IIS.</span><span class="sxs-lookup"><span data-stu-id="3add1-214">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="3add1-215">Неправильная роль, модуль ASP.NET Core не установлен либо неверные разрешения</span><span class="sxs-lookup"><span data-stu-id="3add1-215">Incorrect role, ASP.NET Core Module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="3add1-216">**Браузер:** 500.19 — внутренняя ошибка сервера. Запрашиваемая страница недоступна из-за неверной конфигурации данных для этой страницы.</span><span class="sxs-lookup"><span data-stu-id="3add1-216">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span> <span data-ttu-id="3add1-217">**или**не удается отобразить эту страницу</span><span class="sxs-lookup"><span data-stu-id="3add1-217">**--OR--** This page can't be displayed</span></span>

* <span data-ttu-id="3add1-218">**Журнал приложений:** нет записи</span><span class="sxs-lookup"><span data-stu-id="3add1-218">**Application Log:** No entry</span></span>

* <span data-ttu-id="3add1-219">**Журнал stdout модуля ASP.NET Core:** файл журнала не создан.</span><span class="sxs-lookup"><span data-stu-id="3add1-219">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="3add1-220">**Журнал отладки модуля ASP.NET Core:** файл журнала не создан.</span><span class="sxs-lookup"><span data-stu-id="3add1-220">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="3add1-221">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="3add1-221">Troubleshooting:</span></span>

* <span data-ttu-id="3add1-222">Убедитесь, что включена соответствующая роль.</span><span class="sxs-lookup"><span data-stu-id="3add1-222">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="3add1-223">См. раздел [Конфигурация IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="3add1-223">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="3add1-224">Откройте окно **Программы и компоненты** или **Приложения и возможности** и убедитесь, что установлено ПО **Windows Server Hosting**.</span><span class="sxs-lookup"><span data-stu-id="3add1-224">Open **Programs & Features** or **Apps & features** and confirm that **Windows Server Hosting** is installed.</span></span> <span data-ttu-id="3add1-225">Если **Windows Server Hosting** отсутствует в списке установленных программ, скачайте и установите пакет размещения .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3add1-225">If **Windows Server Hosting** isn't present in the list of installed programs, download and install the .NET Core Hosting Bundle.</span></span>

  [<span data-ttu-id="3add1-226">Текущий установщик пакета размещения .NET Core (прямая загрузка)</span><span class="sxs-lookup"><span data-stu-id="3add1-226">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="3add1-227">См. дополнительные сведения об [установке пакета размещения .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="3add1-227">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="3add1-228">Убедитесь, что параметр **Пул приложений** > **Модель процесса** > **Удостоверение** имеет значение **ApplicationPoolIdentity** или что у пользовательского удостоверения есть нужные разрешения на доступ к папке развертывания приложения.</span><span class="sxs-lookup"><span data-stu-id="3add1-228">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

* <span data-ttu-id="3add1-229">Если вы удалили пакет размещения ASP.NET Core и установили его более раннюю версию, файл *applicationHost.config* не будет включать раздел модуля ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3add1-229">If you uninstalled the ASP.NET Core Hosting Bundle and installed an earlier version of the hosting bundle, the *applicationHost.config* file doesn't include a section for the ASP.NET Core Module.</span></span> <span data-ttu-id="3add1-230">Откройте файл *applicationHost.config* в папке *%windir%/System32/inetsrv/config* и найдите группу раздела `<configuration><configSections><sectionGroup name="system.webServer">`.</span><span class="sxs-lookup"><span data-stu-id="3add1-230">Open *applicationHost.config* at *%windir%/System32/inetsrv/config* and find the `<configuration><configSections><sectionGroup name="system.webServer">` section group.</span></span> <span data-ttu-id="3add1-231">Если раздел модуля ASP.NET Core отсутствует в группе раздела, добавьте его:</span><span class="sxs-lookup"><span data-stu-id="3add1-231">If the section for the ASP.NET Core Module is missing from the section group, add the section element:</span></span>

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```

  <span data-ttu-id="3add1-232">Кроме того, вы можете установить последнюю версию пакета размещения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3add1-232">Alternatively, install the latest version of the ASP.NET Core Hosting Bundle.</span></span> <span data-ttu-id="3add1-233">Последняя версия имеет обратную совместимость с поддерживаемыми приложениями ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3add1-233">The latest version is backwards-compatible with supported ASP.NET Core apps.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="3add1-234">Неправильное значение processPath, отсутствует переменная PATH, пакет размещения не установлен, система или службы IIS не перезапущены, не установлен распространяемый компонент VC++ либо нарушены права доступа к dotnet.exe</span><span class="sxs-lookup"><span data-stu-id="3add1-234">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="3add1-235">**Браузер:** ошибка HTTP 500.0 — ошибка загрузки внутрипроцессного обработчика ANCM</span><span class="sxs-lookup"><span data-stu-id="3add1-235">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="3add1-236">**Журнал приложений:** приложению MACHINE/WEBROOT/APPHOST/{СБОРКА} с физическим корневым каталогом C:\{ПУТЬ}\' не удалось запустить процесс с помощью команды "{...}"</span><span class="sxs-lookup"><span data-stu-id="3add1-236">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="3add1-237">, код ошибки — 0x80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="3add1-237">', ErrorCode = '0x80070002 : 0.</span></span> <span data-ttu-id="3add1-238">не удалось запустить приложение на пути {ПУТЬ}.</span><span class="sxs-lookup"><span data-stu-id="3add1-238">Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="3add1-239">Исполняемый файл не найден на пути {ПУТЬ}.</span><span class="sxs-lookup"><span data-stu-id="3add1-239">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="3add1-240">Не удалось запустить приложение /LM/W3SVC/2/ROOT, код ошибки — 0x8007023e.</span><span class="sxs-lookup"><span data-stu-id="3add1-240">Failed to start application '/LM/W3SVC/2/ROOT', ErrorCode '0x8007023e'.</span></span>

* <span data-ttu-id="3add1-241">**Журнал stdout модуля ASP.NET Core:** файл журнала не создан.</span><span class="sxs-lookup"><span data-stu-id="3add1-241">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="3add1-242">**Журнал отладки модуля ASP.NET Core:** Журнал событий: не удалось запустить приложение на пути {ПУТЬ}.</span><span class="sxs-lookup"><span data-stu-id="3add1-242">**ASP.NET Core Module Debug Log:** Event Log: 'Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="3add1-243">Исполняемый файл не найден на пути {ПУТЬ}.</span><span class="sxs-lookup"><span data-stu-id="3add1-243">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="3add1-244">возвращена ошибка HRESULT: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="3add1-244">Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="3add1-245">**Браузер:** ошибка HTTP 502.5 — сбой процесса</span><span class="sxs-lookup"><span data-stu-id="3add1-245">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="3add1-246">**Журнал приложений:** приложению MACHINE/WEBROOT/APPHOST/{СБОРКА} с физическим корневым каталогом C:\{ПУТЬ}\' не удалось запустить процесс с помощью команды "{...}"</span><span class="sxs-lookup"><span data-stu-id="3add1-246">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="3add1-247">, код ошибки — 0x80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="3add1-247">', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="3add1-248">**Журнал stdout модуля ASP.NET Core:** файл журнала создан, но пуст.</span><span class="sxs-lookup"><span data-stu-id="3add1-248">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="3add1-249">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="3add1-249">Troubleshooting:</span></span>

* <span data-ttu-id="3add1-250">Убедитесь в том, что приложение выполняется локально в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3add1-250">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="3add1-251">Сбой процесса может быть результатом проблемы в приложении.</span><span class="sxs-lookup"><span data-stu-id="3add1-251">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="3add1-252">Дополнительные сведения см. в статьях [Устранение неполадок ASP.NET Core в службах IIS](xref:host-and-deploy/iis/troubleshoot) или [Устранение неполадок ASP.NET Core в службе приложений Azure](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="3add1-252">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="3add1-253">Проверьте атрибут *processPath* для элемента `<aspNetCore>` в файле *web.config*. Он должен иметь значение `dotnet` для зависимого от платформы развертывания (FDD) или `.\{ASSEMBLY}.exe` для [автономного развертывания (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="3add1-253">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's `dotnet` for a framework-dependent deployment (FDD) or `.\{ASSEMBLY}.exe` for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

* <span data-ttu-id="3add1-254">Для зависимого от платформы развертывания файл *dotnet.exe* может быть недоступен по пути, указанному в переменной PATH.</span><span class="sxs-lookup"><span data-stu-id="3add1-254">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="3add1-255">Убедитесь в том, что папка *C:\Program Files\dotnet\\* существует в системных параметрах PATH.</span><span class="sxs-lookup"><span data-stu-id="3add1-255">Confirm that *C:\Program Files\dotnet\\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="3add1-256">В случае с зависимым от платформы развертыванием файл *dotnet.exe* может быть недоступен для удостоверения пользователя пула приложений.</span><span class="sxs-lookup"><span data-stu-id="3add1-256">For an FDD, *dotnet.exe* might not be accessible for the user identity of the app pool.</span></span> <span data-ttu-id="3add1-257">Убедитесь в том, что удостоверение пользователя пула приложений имеет доступ к каталогу *C:\Program Files\dotnet*.</span><span class="sxs-lookup"><span data-stu-id="3add1-257">Confirm that the app pool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="3add1-258">Убедитесь в отсутствии запрещающих правил для удостоверения пользователя пула приложений в каталоге *C:\Program Files\dotnet* и каталоге приложения.</span><span class="sxs-lookup"><span data-stu-id="3add1-258">Confirm that there are no deny rules configured for the app pool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="3add1-259">Возможно, зависимое от платформы развертывание и .NET Core были установлены без перезапуска служб IIS.</span><span class="sxs-lookup"><span data-stu-id="3add1-259">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="3add1-260">Перезагрузите сервер или перезапустите службы IIS, выполнив в командной строке команду **net stop was /y**, а затем — команду **net start w3svc**.</span><span class="sxs-lookup"><span data-stu-id="3add1-260">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="3add1-261">Возможно, зависимое от платформы развертывание было развернуто без установки среды выполнения .NET Core в размещающей системе.</span><span class="sxs-lookup"><span data-stu-id="3add1-261">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="3add1-262">Если среда выполнения .NET Core не была установлена, запустите в целевой системе **установщик пакета размещения .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="3add1-262">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span>

  [<span data-ttu-id="3add1-263">Текущий установщик пакета размещения .NET Core (прямая загрузка)</span><span class="sxs-lookup"><span data-stu-id="3add1-263">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="3add1-264">См. дополнительные сведения об [установке пакета размещения .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="3add1-264">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

  <span data-ttu-id="3add1-265">Если требуется конкретная среда выполнения, скачайте ее на странице [скачивания версий .NET](https://dotnet.microsoft.com/download/archives) и установите в системе.</span><span class="sxs-lookup"><span data-stu-id="3add1-265">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="3add1-266">Завершите установку, перезагрузив систему или перезапустив службы IIS. Для этого выполните в командной строке команду **net stop was /y**, а затем — команду **net start w3svc**.</span><span class="sxs-lookup"><span data-stu-id="3add1-266">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="3add1-267">Возможно, зависимое от платформы развертывание было развернуто в системе, где отсутствует *распространяемый компонент Microsoft Visual C++ 2015 (64-разрядный)*.</span><span class="sxs-lookup"><span data-stu-id="3add1-267">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="3add1-268">Получите установщик в [Центре загрузки Майкрософт](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="3add1-268">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="3add1-269">Неверные аргументы элемента \<aspNetCore></span><span class="sxs-lookup"><span data-stu-id="3add1-269">Incorrect arguments of \<aspNetCore> element</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="3add1-270">**Браузер:** ошибка HTTP 500.0 — ошибка загрузки внутрипроцессного обработчика ANCM</span><span class="sxs-lookup"><span data-stu-id="3add1-270">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="3add1-271">**Журнал приложений:** вызов hostfxr для поиска внутрипроцессного обработчика запросов завершился ошибкой без обнаружения каких-либо собственных зависимостей.</span><span class="sxs-lookup"><span data-stu-id="3add1-271">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="3add1-272">Скорее всего, это означает, что приложение настроено неправильно. Проверьте версии Microsoft.NetCore.App и Microsoft.AspNetCore.App, которые являются целевыми для приложения и установлены на компьютере.</span><span class="sxs-lookup"><span data-stu-id="3add1-272">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="3add1-273">Не удалось найти внутрипроцессный обработчик запросов.</span><span class="sxs-lookup"><span data-stu-id="3add1-273">Could not find inprocess request handler.</span></span> <span data-ttu-id="3add1-274">Выходные данные, записанные в результате вызова hostfxr: вы хотели запустить команды пакета SDK для .NET?</span><span class="sxs-lookup"><span data-stu-id="3add1-274">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="3add1-275">Установите пакет SDK для .NET со страницы по адресу: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Не удалось запустить приложение /LM/W3SVC/3/ROOT, код ошибки — 0x8000ffff.</span><span class="sxs-lookup"><span data-stu-id="3add1-275">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed to start application '/LM/W3SVC/3/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="3add1-276">**Журнал stdout модуля ASP.NET Core:** вы хотели запустить команды пакета SDK для .NET?</span><span class="sxs-lookup"><span data-stu-id="3add1-276">**ASP.NET Core Module stdout Log:** Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="3add1-277">Установите пакет SDK для .NET со страницы по адресу: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="3add1-277">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span></span>

* <span data-ttu-id="3add1-278">**Журнал отладки модуля ASP.NET Core:** вызов hostfxr для поиска внутрипроцессного обработчика запросов завершился ошибкой без обнаружения каких-либо собственных зависимостей.</span><span class="sxs-lookup"><span data-stu-id="3add1-278">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="3add1-279">Скорее всего, это означает, что приложение настроено неправильно. Проверьте версии Microsoft.NetCore.App и Microsoft.AspNetCore.App, которые являются целевыми для приложения и установлены на компьютере.</span><span class="sxs-lookup"><span data-stu-id="3add1-279">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="3add1-280">возвращена ошибка HRESULT: 0x8000ffff Не удалось найти внутрипроцессный обработчик запросов.</span><span class="sxs-lookup"><span data-stu-id="3add1-280">Failed HRESULT returned: 0x8000ffff Could not find inprocess request handler.</span></span> <span data-ttu-id="3add1-281">Выходные данные, записанные в результате вызова hostfxr: вы хотели запустить команды пакета SDK для .NET?</span><span class="sxs-lookup"><span data-stu-id="3add1-281">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="3add1-282">Установите пакет SDK для .NET со страницы по адресу: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409Возвращена ошибка HRESULT: 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="3add1-282">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="3add1-283">**Браузер:** ошибка HTTP 502.5 — сбой процесса</span><span class="sxs-lookup"><span data-stu-id="3add1-283">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="3add1-284">**Журнал приложений:** приложению MACHINE/WEBROOT/APPHOST/{СБОРКА} с физическим корневым каталогом C:\{ПУТЬ}\' не удалось запустить процесс с помощью команды "dotnet" .\{СБОРКА}.dll, код ошибки — 0x80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="3add1-284">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"dotnet" .\{ASSEMBLY}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="3add1-285">**Журнал stdout модуля ASP.NET Core:** приложение для выполнения не существует: ПУТЬ\{СБОРКА}.dll</span><span class="sxs-lookup"><span data-stu-id="3add1-285">**ASP.NET Core Module stdout Log:** The application to execute does not exist: 'PATH\{ASSEMBLY}.dll'</span></span>

::: moniker-end

<span data-ttu-id="3add1-286">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="3add1-286">Troubleshooting:</span></span>

* <span data-ttu-id="3add1-287">Убедитесь в том, что приложение выполняется локально в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3add1-287">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="3add1-288">Сбой процесса может быть результатом проблемы в приложении.</span><span class="sxs-lookup"><span data-stu-id="3add1-288">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="3add1-289">Дополнительные сведения см. в статьях [Устранение неполадок ASP.NET Core в службах IIS](xref:host-and-deploy/iis/troubleshoot) или [Устранение неполадок ASP.NET Core в службе приложений Azure](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="3add1-289">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="3add1-290">Проверьте атрибут *arguments* для элемента `<aspNetCore>` в файле *web.config* и удостоверьтесь, что он имеет значение: а) `.\{ASSEMBLY}.dll`для развертывания, зависящего от платформы (FDD); б) отсутствует, содержит пустую строку (`arguments=""`) или список аргументов вашего приложения (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) для автономного развертывания (SCD).</span><span class="sxs-lookup"><span data-stu-id="3add1-290">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) `.\{ASSEMBLY}.dll` for a framework-dependent deployment (FDD); or (b) not present, an empty string (`arguments=""`), or a list of the app's arguments (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) for a self-contained deployment (SCD).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="missing-net-core-shared-framework"></a><span data-ttu-id="3add1-291">Отсутствует общая платформа .NET Core</span><span class="sxs-lookup"><span data-stu-id="3add1-291">Missing .NET Core shared framework</span></span>

* <span data-ttu-id="3add1-292">**Браузер:** ошибка HTTP 500.0 — ошибка загрузки внутрипроцессного обработчика ANCM</span><span class="sxs-lookup"><span data-stu-id="3add1-292">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="3add1-293">**Журнал приложений:** вызов hostfxr для поиска внутрипроцессного обработчика запросов завершился ошибкой без обнаружения каких-либо собственных зависимостей.</span><span class="sxs-lookup"><span data-stu-id="3add1-293">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="3add1-294">Скорее всего, это означает, что приложение настроено неправильно. Проверьте версии Microsoft.NetCore.App и Microsoft.AspNetCore.App, которые являются целевыми для приложения и установлены на компьютере.</span><span class="sxs-lookup"><span data-stu-id="3add1-294">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="3add1-295">Не удалось найти внутрипроцессный обработчик запросов.</span><span class="sxs-lookup"><span data-stu-id="3add1-295">Could not find inprocess request handler.</span></span> <span data-ttu-id="3add1-296">Выходные данные, записанные в результате вызова hostfxr: не удалось найти совместимую версию платформы.</span><span class="sxs-lookup"><span data-stu-id="3add1-296">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="3add1-297">Указанная версия {ВЕРСИЯ} платформы Microsoft.AspNetCore.App не найдена.</span><span class="sxs-lookup"><span data-stu-id="3add1-297">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

<span data-ttu-id="3add1-298">Не удалось запустить приложение /LM/W3SVC/5/ROOT, код ошибки — 0x8000ffff.</span><span class="sxs-lookup"><span data-stu-id="3add1-298">Failed to start application '/LM/W3SVC/5/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="3add1-299">**Журнал stdout модуля ASP.NET Core:** не удалось найти совместимую версию платформы.</span><span class="sxs-lookup"><span data-stu-id="3add1-299">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="3add1-300">Указанная версия {ВЕРСИЯ} платформы Microsoft.AspNetCore.App не найдена.</span><span class="sxs-lookup"><span data-stu-id="3add1-300">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

* <span data-ttu-id="3add1-301">**Журнал отладки модуля ASP.NET Core:** возвращена ошибка HRESULT: 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="3add1-301">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

<span data-ttu-id="3add1-302">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="3add1-302">Troubleshooting:</span></span>

<span data-ttu-id="3add1-303">Если используется зависимое от платформы развертывание (FDD), убедитесь в том, что в системе установлена нужная среда выполнения.</span><span class="sxs-lookup"><span data-stu-id="3add1-303">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="3add1-304">Пул приложений остановлен</span><span class="sxs-lookup"><span data-stu-id="3add1-304">Stopped Application Pool</span></span>

* <span data-ttu-id="3add1-305">**Браузер:** 503 — служба недоступна</span><span class="sxs-lookup"><span data-stu-id="3add1-305">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="3add1-306">**Журнал приложений:** нет записи</span><span class="sxs-lookup"><span data-stu-id="3add1-306">**Application Log:** No entry</span></span>

* <span data-ttu-id="3add1-307">**Журнал stdout модуля ASP.NET Core:** файл журнала не создан.</span><span class="sxs-lookup"><span data-stu-id="3add1-307">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="3add1-308">**Журнал отладки модуля ASP.NET Core:** файл журнала не создан.</span><span class="sxs-lookup"><span data-stu-id="3add1-308">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="3add1-309">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="3add1-309">Troubleshooting:</span></span>

<span data-ttu-id="3add1-310">Убедитесь, что пул приложений не находится в состоянии *Остановлен*.</span><span class="sxs-lookup"><span data-stu-id="3add1-310">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="3add1-311">Дочернее приложение имеет раздел \<handlers></span><span class="sxs-lookup"><span data-stu-id="3add1-311">Sub-application includes a \<handlers> section</span></span>

* <span data-ttu-id="3add1-312">**Браузер:** ошибка HTTP 500.19 — внутренняя ошибка сервера</span><span class="sxs-lookup"><span data-stu-id="3add1-312">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="3add1-313">**Журнал приложений:** нет записи</span><span class="sxs-lookup"><span data-stu-id="3add1-313">**Application Log:** No entry</span></span>

* <span data-ttu-id="3add1-314">**Журнал stdout модуля ASP.NET Core:** корневой файл журнала приложения создан и работает нормально.</span><span class="sxs-lookup"><span data-stu-id="3add1-314">**ASP.NET Core Module stdout Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="3add1-315">Файл журнала дочернего приложения не создан.</span><span class="sxs-lookup"><span data-stu-id="3add1-315">The sub-app's log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="3add1-316">**Журнал отладки модуля ASP.NET Core:** корневой файл журнала приложения создан и работает нормально.</span><span class="sxs-lookup"><span data-stu-id="3add1-316">**ASP.NET Core Module Debug Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="3add1-317">Файл журнала дочернего приложения не создан.</span><span class="sxs-lookup"><span data-stu-id="3add1-317">The sub-app's log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="3add1-318">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="3add1-318">Troubleshooting:</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="3add1-319">Убедитесь, что файл *web.config* дочернего приложения не содержит раздел `<handlers>` или что дочернее приложение не наследует обработчики родительского приложения.</span><span class="sxs-lookup"><span data-stu-id="3add1-319">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section or that the sub-app doesn't inherit the parent app's handlers.</span></span>

<span data-ttu-id="3add1-320">Раздел `<system.webServer>` файла *web.config* находится внутри элемента `<location>`.</span><span class="sxs-lookup"><span data-stu-id="3add1-320">The parent app's `<system.webServer>` section of *web.config* is placed inside of a `<location>` element.</span></span> <span data-ttu-id="3add1-321">Значение `false` свойства <xref:System.Configuration.SectionInformation.InheritInChildApplications*> указывает, что параметры, заданные в элементе [\<расположение>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location), не наследуются приложениями, которые находятся во вложенном каталоге родительского приложения.</span><span class="sxs-lookup"><span data-stu-id="3add1-321">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the parent app.</span></span> <span data-ttu-id="3add1-322">Для получения дополнительной информации см. <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="3add1-322">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="3add1-323">В файле *web.config* дочернего приложения не должно быть раздела `<handlers>`.</span><span class="sxs-lookup"><span data-stu-id="3add1-323">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

::: moniker-end

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="3add1-324">Неверный путь к журналу stdout</span><span class="sxs-lookup"><span data-stu-id="3add1-324">stdout log path incorrect</span></span>

* <span data-ttu-id="3add1-325">**Браузер:** приложение работает правильно.</span><span class="sxs-lookup"><span data-stu-id="3add1-325">**Browser:** The app responds normally.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="3add1-326">**Журнал приложений:** не удалось запустить перенаправление stdout в C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="3add1-326">**Application Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="3add1-327">Сообщение об исключении: по пути {ПУТЬ}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84 возвращено значение HRESULT — 0x80070005.</span><span class="sxs-lookup"><span data-stu-id="3add1-327">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="3add1-328">Не удалось остановить перенаправление stdout в C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="3add1-328">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="3add1-329">Сообщение об исключении: по пути {ПУТЬ} возвращено значение HRESULT — 0x80070002.</span><span class="sxs-lookup"><span data-stu-id="3add1-329">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="3add1-330">Не удалось запустить перенаправление stdout по пути {ПУТЬ}\aspnetcorev2_inprocess.dll.</span><span class="sxs-lookup"><span data-stu-id="3add1-330">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

* <span data-ttu-id="3add1-331">**Журнал stdout модуля ASP.NET Core:** файл журнала не создан.</span><span class="sxs-lookup"><span data-stu-id="3add1-331">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="3add1-332">**Журнал отладки модуля ASP.NET Core:** не удалось запустить перенаправление stdout в C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="3add1-332">**ASP.NET Core Module debug Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="3add1-333">Сообщение об исключении: по пути {ПУТЬ}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84 возвращено значение HRESULT — 0x80070005.</span><span class="sxs-lookup"><span data-stu-id="3add1-333">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="3add1-334">Не удалось остановить перенаправление stdout в C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="3add1-334">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="3add1-335">Сообщение об исключении: по пути {ПУТЬ} возвращено значение HRESULT — 0x80070002.</span><span class="sxs-lookup"><span data-stu-id="3add1-335">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="3add1-336">Не удалось запустить перенаправление stdout по пути {ПУТЬ}\aspnetcorev2_inprocess.dll.</span><span class="sxs-lookup"><span data-stu-id="3add1-336">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="3add1-337">**Журнал приложений:** Предупреждение: не удалось создать stdoutLogFile \\?\{ ПУТЬ} \путь_не_существует\stdout_{ИД_ПРОЦЕССА} _ {МЕТКА_ВРЕМЕНИ}.log, код ошибки — -2147024893.</span><span class="sxs-lookup"><span data-stu-id="3add1-337">**Application Log:** Warning: Could not create stdoutLogFile \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="3add1-338">**Журнал stdout модуля ASP.NET Core:** файл журнала не создан.</span><span class="sxs-lookup"><span data-stu-id="3add1-338">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="3add1-339">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="3add1-339">Troubleshooting:</span></span>

* <span data-ttu-id="3add1-340">Не существует путь `stdoutLogFile`, указанный в элементе `<aspNetCore>` в файле *web.config*.</span><span class="sxs-lookup"><span data-stu-id="3add1-340">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="3add1-341">Дополнительные сведения см. в разделе [Создание и и перенаправление журнала](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span><span class="sxs-lookup"><span data-stu-id="3add1-341">For more information, see [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span>

* <span data-ttu-id="3add1-342">Пользователь пула приложения не имеет прав на запись в путь к папке журнала stdout.</span><span class="sxs-lookup"><span data-stu-id="3add1-342">The app pool user doesn't have write access to the stdout log path.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="3add1-343">Общая проблема с конфигурацией приложения</span><span class="sxs-lookup"><span data-stu-id="3add1-343">Application configuration general issue</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="3add1-344">**Браузер:** ошибка HTTP 500.0 — ошибка загрузки внутрипроцессного обработчика ANCM**или**ошибка HTTP 500.30 — сбой в процессе запуска ANCM</span><span class="sxs-lookup"><span data-stu-id="3add1-344">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure **--OR--** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="3add1-345">**Журнал приложений:** Переменная</span><span class="sxs-lookup"><span data-stu-id="3add1-345">**Application Log:** Variable</span></span>

* <span data-ttu-id="3add1-346">**Журнал stdout модуля ASP.NET Core:** файл журнала создан, но пуст, или создан с обычными записями до точки сбоя приложения.</span><span class="sxs-lookup"><span data-stu-id="3add1-346">**ASP.NET Core Module stdout Log:** The log file is created but empty or created with normal entries until the point of the app failing.</span></span>

* <span data-ttu-id="3add1-347">**Журнал отладки модуля ASP.NET Core:** Переменная</span><span class="sxs-lookup"><span data-stu-id="3add1-347">**ASP.NET Core Module Debug Log:** Variable</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="3add1-348">**Браузер:** ошибка HTTP 502.5 — сбой процесса</span><span class="sxs-lookup"><span data-stu-id="3add1-348">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="3add1-349">**Журнал приложений:** приложение MACHINE/WEBROOT/APPHOST/{СБОРКА} с физическим корневым каталогом C:\{ПУТЬ}\' создало процесс с помощью команды "C:\{ПУТЬ}\{СБОРКА}.{exe|dll}", однако при этом либо произошел сбой, либо не был получен ответ, либо не прослушивался указанный порт {ПОРТ}; код ошибки — {КОД ОШИБКИ}</span><span class="sxs-lookup"><span data-stu-id="3add1-349">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' created process with commandline '"C:\{PATH}\{ASSEMBLY}.{exe|dll}" ' but either crashed or did not respond or did not listen on the given port '{PORT}', ErrorCode = '{ERROR CODE}'</span></span>

* <span data-ttu-id="3add1-350">**Журнал stdout модуля ASP.NET Core:** файл журнала создан, но пуст.</span><span class="sxs-lookup"><span data-stu-id="3add1-350">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="3add1-351">Устранение неполадок:</span><span class="sxs-lookup"><span data-stu-id="3add1-351">Troubleshooting:</span></span>

<span data-ttu-id="3add1-352">Процесс не удалось запустить, скорее всего, из-за проблемы с конфигурацией приложения или ошибки программирования.</span><span class="sxs-lookup"><span data-stu-id="3add1-352">The process failed to start, most likely due to an app configuration or programming issue.</span></span>

<span data-ttu-id="3add1-353">Дополнительные сведения см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="3add1-353">For more information, see the following topics:</span></span>

* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:test/troubleshoot>
