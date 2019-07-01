---
title: Устранение неполадок ASP.NET Core в службах IIS
author: guardrex
description: Сведения о диагностике проблем с развертываниями приложений ASP.NET Core на платформе IIS.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 4df370dd9b1a5a651bcf767b8b9ace4220bdc345
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313658"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="720f7-103">Устранение неполадок ASP.NET Core в службах IIS</span><span class="sxs-lookup"><span data-stu-id="720f7-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="720f7-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="720f7-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="720f7-105">Эта статья содержит инструкции по диагностике проблемы запуска приложения ASP.NET Core, размещенного в [службах IIS](/iis).</span><span class="sxs-lookup"><span data-stu-id="720f7-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="720f7-106">Информация в этой статье относится к размещению в службах IIS на Windows Server и Windows Desktop.</span><span class="sxs-lookup"><span data-stu-id="720f7-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

<span data-ttu-id="720f7-107">Дополнительные статьи по устранению неполадок:</span><span class="sxs-lookup"><span data-stu-id="720f7-107">Additional troubleshooting topics:</span></span>

* <span data-ttu-id="720f7-108">Служба приложений Azure также использует [модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module) и службы IIS для размещения приложений.</span><span class="sxs-lookup"><span data-stu-id="720f7-108">Azure App Service also uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and IIS to host apps.</span></span> <span data-ttu-id="720f7-109">Советы по устранению неполадок, касающиеся Службы приложений Azure, см. в статье <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="720f7-109">For troubleshooting advice that pertains specifically to Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>
* <span data-ttu-id="720f7-110">В статье <xref:fundamentals/error-handling> описывается, как обрабатывать ошибки в приложениях ASP.NET Core при разработке в локальной системе.</span><span class="sxs-lookup"><span data-stu-id="720f7-110"><xref:fundamentals/error-handling> covers how to handle errors in ASP.NET Core apps during development on a local system.</span></span>
* <span data-ttu-id="720f7-111">Статья [Знакомство с отладчиком Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) описывает возможности отладчика Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="720f7-111">[Learn to debug using Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) introduces the features of the Visual Studio debugger.</span></span>
* <span data-ttu-id="720f7-112">[Статья об отладке с помощью Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) описывает поддержку отладки, встроенную в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="720f7-112">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) describes the debugging support built into Visual Studio Code.</span></span>

[!INCLUDE[](~/includes/azure-iis-startup-errors.md)]

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="720f7-113">Устранение неполадок при запуске приложения</span><span class="sxs-lookup"><span data-stu-id="720f7-113">Troubleshoot app startup errors</span></span>

### <a name="enable-the-aspnet-core-module-debug-log"></a><span data-ttu-id="720f7-114">Включение журнала отладки модулей ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="720f7-114">Enable the ASP.NET Core Module debug log</span></span>

<span data-ttu-id="720f7-115">Добавьте следующие параметры обработчика в файл *web.config* приложения, чтобы включить журналы отладки модулей ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="720f7-115">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug logs:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="720f7-116">Убедитесь, что указанный для журнала путь существует и удостоверение пула приложений имеет разрешения на запись в это расположение.</span><span class="sxs-lookup"><span data-stu-id="720f7-116">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

### <a name="application-event-log"></a><span data-ttu-id="720f7-117">Журнал событий приложений</span><span class="sxs-lookup"><span data-stu-id="720f7-117">Application Event Log</span></span>

<span data-ttu-id="720f7-118">Доступ к журналу событий приложений</span><span class="sxs-lookup"><span data-stu-id="720f7-118">Access the Application Event Log:</span></span>

1. <span data-ttu-id="720f7-119">Откройте меню "Пуск", выполните поиск по фразе **Просмотр событий** и выберите приложение **Просмотр событий**.</span><span class="sxs-lookup"><span data-stu-id="720f7-119">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="720f7-120">В **средстве просмотра событий** откройте узел **Журналы Windows**.</span><span class="sxs-lookup"><span data-stu-id="720f7-120">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="720f7-121">Выберите **Приложение**, чтобы открыть журнал событий приложения.</span><span class="sxs-lookup"><span data-stu-id="720f7-121">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="720f7-122">Проверьте здесь наличие ошибок, связанных с проблемным приложением.</span><span class="sxs-lookup"><span data-stu-id="720f7-122">Search for errors associated with the failing app.</span></span> <span data-ttu-id="720f7-123">Нам нужны ошибки со значениями *Модуль IIS AspNetCore* или *Модуль IIS Express AspNetCore* в столбце *Источник*.</span><span class="sxs-lookup"><span data-stu-id="720f7-123">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="720f7-124">Запуск приложения в командной строке</span><span class="sxs-lookup"><span data-stu-id="720f7-124">Run the app at a command prompt</span></span>

<span data-ttu-id="720f7-125">Многие ошибки запуска не создают полезные сведения в журнале событий приложения.</span><span class="sxs-lookup"><span data-stu-id="720f7-125">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="720f7-126">Для некоторых ошибок можно найти причину, запустив приложение в командной строке на компьютере размещения.</span><span class="sxs-lookup"><span data-stu-id="720f7-126">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="720f7-127">развертывание, зависящее от платформы;</span><span class="sxs-lookup"><span data-stu-id="720f7-127">Framework-dependent deployment</span></span>

<span data-ttu-id="720f7-128">Если развертываемое приложение [зависит от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="720f7-128">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="720f7-129">В командной строке перейдите в папку развертывания и запустите приложение, выполнив команду *dotnet.exe* для сборки приложения.</span><span class="sxs-lookup"><span data-stu-id="720f7-129">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="720f7-130">В следующей команде укажите нужное имя сборки вместо заполнителя \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="720f7-130">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="720f7-131">Выходные данные приложения, в том числе любые ошибки, будут выведены в окно консоли.</span><span class="sxs-lookup"><span data-stu-id="720f7-131">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="720f7-132">Если ошибки возникают при выполнении запроса к приложению, выполните запрос к узлу и порту, где Kestrel ожидает передачи данных.</span><span class="sxs-lookup"><span data-stu-id="720f7-132">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="720f7-133">Создайте запрос к `http://localhost:5000/`, используя узел и порт по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="720f7-133">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="720f7-134">Если приложение отвечает на запросы по адресу конечной точки Kestrel как обычно, то проблема, вероятнее, связана с конфигурацией размещения, а не с самим приложением.</span><span class="sxs-lookup"><span data-stu-id="720f7-134">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="720f7-135">автономное развертывание;</span><span class="sxs-lookup"><span data-stu-id="720f7-135">Self-contained deployment</span></span>

<span data-ttu-id="720f7-136">Если приложение [развертывается автономно](/dotnet/core/deploying/#self-contained-deployments-scd), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="720f7-136">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="720f7-137">В командной строке перейдите в папку развертывания и запустите исполняемый файл приложения.</span><span class="sxs-lookup"><span data-stu-id="720f7-137">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="720f7-138">В следующей команде укажите нужное имя сборки вместо заполнителя \<assembly_name>: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="720f7-138">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="720f7-139">Выходные данные приложения, в том числе любые ошибки, будут выведены в окно консоли.</span><span class="sxs-lookup"><span data-stu-id="720f7-139">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="720f7-140">Если ошибки возникают при выполнении запроса к приложению, выполните запрос к узлу и порту, где Kestrel ожидает передачи данных.</span><span class="sxs-lookup"><span data-stu-id="720f7-140">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="720f7-141">Создайте запрос к `http://localhost:5000/`, используя узел и порт по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="720f7-141">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="720f7-142">Если приложение отвечает на запросы по адресу конечной точки Kestrel как обычно, то проблема, вероятнее, связана с конфигурацией размещения, а не с самим приложением.</span><span class="sxs-lookup"><span data-stu-id="720f7-142">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="720f7-143">Журнал stdout модуля ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="720f7-143">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="720f7-144">Чтобы включить и просмотреть журналы stdout, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="720f7-144">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="720f7-145">Перейдите в папку развертывания сайта на компьютере размещения.</span><span class="sxs-lookup"><span data-stu-id="720f7-145">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="720f7-146">Если здесь нет папки *logs*, создайте ее.</span><span class="sxs-lookup"><span data-stu-id="720f7-146">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="720f7-147">Сведения о том, как настроить в MSBuild автоматическое создание папки *logs* в развертывании, см. в статье [о структуре каталогов](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="720f7-147">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="720f7-148">Измените файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="720f7-148">Edit the *web.config* file.</span></span> <span data-ttu-id="720f7-149">Задайте для параметра **stdoutLogEnabled** значение `true` и измените путь **stdoutLogFile** так, чтобы он указывал на папку *logs* (например, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="720f7-149">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="720f7-150">В этом пути `stdout` обозначает префикс имени для файла журнала.</span><span class="sxs-lookup"><span data-stu-id="720f7-150">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="720f7-151">При создании файла журнала автоматически добавляются метка времени, идентификатор процесса и расширение файла.</span><span class="sxs-lookup"><span data-stu-id="720f7-151">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="720f7-152">Если указан префикс файла `stdout`, стандартный файл журнала будет иметь примерно такое имя: *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="720f7-152">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="720f7-153">Убедитесь, что удостоверение пула приложений имеет разрешения на запись в папку *logs*.</span><span class="sxs-lookup"><span data-stu-id="720f7-153">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="720f7-154">Сохраните обновленный файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="720f7-154">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="720f7-155">Сделайте запрос к приложению.</span><span class="sxs-lookup"><span data-stu-id="720f7-155">Make a request to the app.</span></span>
1. <span data-ttu-id="720f7-156">Перейдите в папку *logs*.</span><span class="sxs-lookup"><span data-stu-id="720f7-156">Navigate to the *logs* folder.</span></span> <span data-ttu-id="720f7-157">Найдите и откройте последний журнал вывода stdout.</span><span class="sxs-lookup"><span data-stu-id="720f7-157">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="720f7-158">Проверьте, нет ли в нем ошибок.</span><span class="sxs-lookup"><span data-stu-id="720f7-158">Study the log for errors.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="720f7-159">Завершив устранение неполадок, отключите ведение журнала stdout.</span><span class="sxs-lookup"><span data-stu-id="720f7-159">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="720f7-160">Измените файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="720f7-160">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="720f7-161">Задайте параметру **stdoutLogEnabled** значение `false`.</span><span class="sxs-lookup"><span data-stu-id="720f7-161">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="720f7-162">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="720f7-162">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="720f7-163">Ошибка при отключении журнала stdout может привести к сбоям приложения или сервера.</span><span class="sxs-lookup"><span data-stu-id="720f7-163">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="720f7-164">Ни размер файла журнала, ни количество создаваемых файлов журналов ничем не ограничены.</span><span class="sxs-lookup"><span data-stu-id="720f7-164">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="720f7-165">Для обычного ведения журналов для приложения ASP.NET Core используйте библиотеку, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов.</span><span class="sxs-lookup"><span data-stu-id="720f7-165">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="720f7-166">Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="720f7-166">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enable-the-developer-exception-page"></a><span data-ttu-id="720f7-167">Включение страницы исключений для разработчика</span><span class="sxs-lookup"><span data-stu-id="720f7-167">Enable the Developer Exception Page</span></span>

<span data-ttu-id="720f7-168">Можно добавить переменную среды `ASPNETCORE_ENVIRONMENT` [в файл web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables), чтобы запустить приложение в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="720f7-168">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="720f7-169">Если среда не переопределяется при запуске приложения использованием `UseEnvironment` в конструкторе узла, эта переменная среды позволяет отображать [страницу исключения для разработчика](xref:fundamentals/error-handling) при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="720f7-169">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="720f7-170">Настройка переменной среды `ASPNETCORE_ENVIRONMENT` рекомендуется только на промежуточных и тестовых серверах, доступ к которым из Интернета закрыт.</span><span class="sxs-lookup"><span data-stu-id="720f7-170">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="720f7-171">Завершив устранение неполадок, удалите эту переменную среды из файла *web.config*.</span><span class="sxs-lookup"><span data-stu-id="720f7-171">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="720f7-172">Сведения о настройке переменных среды в файле *web.config* см. в статье о [дочернем элементе environmentVariables в aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="720f7-172">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="720f7-173">Стандартные ошибки запуска</span><span class="sxs-lookup"><span data-stu-id="720f7-173">Common startup errors</span></span>

<span data-ttu-id="720f7-174">См. раздел <xref:host-and-deploy/azure-iis-errors-reference>.</span><span class="sxs-lookup"><span data-stu-id="720f7-174">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="720f7-175">В этой статье рассматривается большинство распространенных ошибок, препятствующих запуску приложений.</span><span class="sxs-lookup"><span data-stu-id="720f7-175">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="720f7-176">Получение данных из приложения</span><span class="sxs-lookup"><span data-stu-id="720f7-176">Obtain data from an app</span></span>

<span data-ttu-id="720f7-177">Если приложение способно отвечать на запросы, получите данные о запросе, подключении и дополнительные данные из приложений с помощью встроенного терминала ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="720f7-177">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="720f7-178">Дополнительные сведения и примеры с кодом см. здесь: <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="720f7-178">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

## <a name="create-a-dump"></a><span data-ttu-id="720f7-179">Создание дампа</span><span class="sxs-lookup"><span data-stu-id="720f7-179">Create a dump</span></span>

<span data-ttu-id="720f7-180">*Дамп* представляет собой моментальный снимок системной памяти и может помочь определить причину аварийного завершения, сбоя запуска или медленной работы приложения.</span><span class="sxs-lookup"><span data-stu-id="720f7-180">A *dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="720f7-181">Аварийное завершение работы приложения или исключение</span><span class="sxs-lookup"><span data-stu-id="720f7-181">App crashes or encounters an exception</span></span>

<span data-ttu-id="720f7-182">Получите и проанализируйте дамп из [отчетов об ошибках Windows (WER)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="720f7-182">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="720f7-183">Создайте папку для хранения файлов аварийного дампа в `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="720f7-183">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="720f7-184">Пул приложений должен иметь доступ на запись к папке.</span><span class="sxs-lookup"><span data-stu-id="720f7-184">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="720f7-185">Запустите [скрипт PowerShell EnableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="720f7-185">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="720f7-186">Если приложение использует [модель размещения в процессе](xref:host-and-deploy/iis/index#in-process-hosting-model), выполните скрипт для *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="720f7-186">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="720f7-187">Если приложение использует [модель размещения вне процесса](xref:host-and-deploy/iis/index#out-of-process-hosting-model), выполните скрипт для *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="720f7-187">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="720f7-188">Запустите приложение в условиях, вызывающих аварийное завершение.</span><span class="sxs-lookup"><span data-stu-id="720f7-188">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="720f7-189">После аварийного завершения запустите [скрипт PowerShell DisableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="720f7-189">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="720f7-190">Если приложение использует [модель размещения в процессе](xref:host-and-deploy/iis/index#in-process-hosting-model), выполните скрипт для *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="720f7-190">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="720f7-191">Если приложение использует [модель размещения вне процесса](xref:host-and-deploy/iis/index#out-of-process-hosting-model), выполните скрипт для *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="720f7-191">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="720f7-192">Когда приложение аварийно завершит работу и сбор дампов будет выполнен, приложение сможет завершить работу обычным образом.</span><span class="sxs-lookup"><span data-stu-id="720f7-192">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="720f7-193">Скрипт PowerShell настраивает отчеты об ошибках Windows для сбора до пяти дампов для приложения.</span><span class="sxs-lookup"><span data-stu-id="720f7-193">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="720f7-194">Аварийные дампы могут занимать много места на диске (до нескольких гигабайтов каждый).</span><span class="sxs-lookup"><span data-stu-id="720f7-194">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="720f7-195">Приложение перестает отвечать на запросы, не запускается или работает в обычном режиме</span><span class="sxs-lookup"><span data-stu-id="720f7-195">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="720f7-196">Когда приложение *перестает отвечать на запросы* (но аварийное завершение не происходит), не запускается или работает в обычном режиме, см. раздел [Файлы дампа пользовательского режима: выбор лучшего инструмента](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool), чтобы выбрать подходящий инструмент для создания дампа.</span><span class="sxs-lookup"><span data-stu-id="720f7-196">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

### <a name="analyze-the-dump"></a><span data-ttu-id="720f7-197">Анализ дампа</span><span class="sxs-lookup"><span data-stu-id="720f7-197">Analyze the dump</span></span>

<span data-ttu-id="720f7-198">Дамп можно проанализировать несколькими способами.</span><span class="sxs-lookup"><span data-stu-id="720f7-198">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="720f7-199">Дополнительные сведения см. в разделе [Анализ файла дампа пользовательского режима](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="720f7-199">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="720f7-200">Удаленная отладка</span><span class="sxs-lookup"><span data-stu-id="720f7-200">Remote debugging</span></span>

<span data-ttu-id="720f7-201">Изучите раздел документации по Visual Studio [об удаленной отладке ASP.NET Core на удаленном компьютере IIS в Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer).</span><span class="sxs-lookup"><span data-stu-id="720f7-201">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="720f7-202">Application Insights</span><span class="sxs-lookup"><span data-stu-id="720f7-202">Application Insights</span></span>

<span data-ttu-id="720f7-203">[Application Insights](/azure/application-insights/) предоставляет данные телеметрии из приложений, размещенных в IIS, в том числе возможности регистрации ошибок и создания отчетов.</span><span class="sxs-lookup"><span data-stu-id="720f7-203">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="720f7-204">Application Insights может сообщать только об ошибках, возникающих после запуска приложения, когда становятся доступными функции ведения журнала приложения.</span><span class="sxs-lookup"><span data-stu-id="720f7-204">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="720f7-205">Дополнительные сведения см. в статье [Application Insights для ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="720f7-205">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-advice"></a><span data-ttu-id="720f7-206">Дополнительные советы</span><span class="sxs-lookup"><span data-stu-id="720f7-206">Additional advice</span></span>

<span data-ttu-id="720f7-207">Иногда приложения-функции перестают работать сразу после обновления пакета SDK для .NET Core на компьютере разработки или обновления пакетов в самом приложении.</span><span class="sxs-lookup"><span data-stu-id="720f7-207">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="720f7-208">В некоторых случаях в результате важного обновления несогласованные версии пакетов могут привести к нарушению работы приложения.</span><span class="sxs-lookup"><span data-stu-id="720f7-208">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="720f7-209">Большинство этих проблем можно исправить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="720f7-209">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="720f7-210">Удалите папки *bin* и *obj*.</span><span class="sxs-lookup"><span data-stu-id="720f7-210">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="720f7-211">Очистите кэши пакетов, размещенные в папках *%UserProfile%\\.nuget\\packages* и *%LocalAppData%\\Nuget\\v3-cache*.</span><span class="sxs-lookup"><span data-stu-id="720f7-211">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="720f7-212">Восстановите и перестройте проект.</span><span class="sxs-lookup"><span data-stu-id="720f7-212">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="720f7-213">Убедитесь, что предыдущее развертывание на сервере полностью удалено, прежде чем развертывать его снова.</span><span class="sxs-lookup"><span data-stu-id="720f7-213">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="720f7-214">Очистить кэши пакета проще всего командой `dotnet nuget locals all --clear` из командной строки.</span><span class="sxs-lookup"><span data-stu-id="720f7-214">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
>
> <span data-ttu-id="720f7-215">Также очистку кэшей пакетов можно выполнить средством [nuget.exe](https://www.nuget.org/downloads) или командой `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="720f7-215">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="720f7-216">*NuGet.exe* не входит в пакет установки операционной системы Windows для настольных компьютеров и его нужно получить отдельно на [веб-сайте NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="720f7-216">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="720f7-217">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="720f7-217">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
