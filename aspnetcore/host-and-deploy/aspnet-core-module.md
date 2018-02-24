---
title: "Справочник по конфигурации ASP.NET модуль Core"
author: guardrex
description: "Подробные сведения о настройке модуля ASP.NET Core для размещения приложений ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: c01abed767a226eae68725c1c53d922eac2f705e
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/23/2018
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="cabaa-103">Справочник по конфигурации ASP.NET модуль Core</span><span class="sxs-lookup"><span data-stu-id="cabaa-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="cabaa-104">По [Latham Люк](https://github.com/guardrex), [Рик Андерсон](https://twitter.com/RickAndMSFT), и [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="cabaa-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="cabaa-105">Этот документ содержит инструкции о том, как настроить модуль ASP.NET Core для размещения приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cabaa-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="cabaa-106">Введение модуль ASP.NET Core и инструкции по установке см. в разделе [Общие сведения о модуле ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="cabaa-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="cabaa-107">Конфигурация с web.config</span><span class="sxs-lookup"><span data-stu-id="cabaa-107">Configuration with web.config</span></span>

<span data-ttu-id="cabaa-108">Модуль Core ASP.NET настроена с `aspNetCore` раздел `system.webServer` узел на сайте *web.config* файла.</span><span class="sxs-lookup"><span data-stu-id="cabaa-108">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="cabaa-109">Следующие *web.config* файл публикуется для [развертывания зависит от framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) и настраивает модуль Core ASP.NET для обработки запросов для сайта:</span><span class="sxs-lookup"><span data-stu-id="cabaa-109">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="cabaa-110">Следующие *web.config* опубликована для [автономное развертывание](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="cabaa-110">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="cabaa-111">Когда приложение развертывается в [службе приложений Azure](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` задан путь к `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="cabaa-111">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="cabaa-112">Путь сохраняет журналы stdout *LogFiles* папку, в которой находится в расположении, автоматически созданный с помощью службы.</span><span class="sxs-lookup"><span data-stu-id="cabaa-112">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="cabaa-113">В разделе [конфигурации подзапроса приложений](xref:host-and-deploy/iis/index#sub-application-configuration) для важное примечание, относящиеся к конфигурации *web.config* файлы в приложениях для вложенных.</span><span class="sxs-lookup"><span data-stu-id="cabaa-113">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="cabaa-114">Атрибуты элемента aspNetCore</span><span class="sxs-lookup"><span data-stu-id="cabaa-114">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="cabaa-115">Атрибут</span><span class="sxs-lookup"><span data-stu-id="cabaa-115">Attribute</span></span> | <span data-ttu-id="cabaa-116">Описание</span><span class="sxs-lookup"><span data-stu-id="cabaa-116">Description</span></span> | <span data-ttu-id="cabaa-117">По умолчанию</span><span class="sxs-lookup"><span data-stu-id="cabaa-117">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="cabaa-118">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="cabaa-118">Optional string attribute.</span></span></p><p><span data-ttu-id="cabaa-119">Аргументы для исполняемого файла, указанного в **processPath**.</span><span class="sxs-lookup"><span data-stu-id="cabaa-119">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <span data-ttu-id="cabaa-120">true или false.</span><span class="sxs-lookup"><span data-stu-id="cabaa-120">true or false.</span></span></p><p><span data-ttu-id="cabaa-121">Если значение равно true, **502.5 - сбой процесса** подавляется страницы и кодовую страницу 502 состояния на *web.config* имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="cabaa-121">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <span data-ttu-id="cabaa-122">true или false.</span><span class="sxs-lookup"><span data-stu-id="cabaa-122">true or false.</span></span></p><p><span data-ttu-id="cabaa-123">Значение true, если маркер отправляется дочернего процесса, прослушивающего порт % ASPNETCORE_PORT % как заголовок «MS-ASPNETCORE-WINAUTHTOKEN» каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="cabaa-123">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="cabaa-124">Он отвечает этот процесс для вызова CloseHandle по этому токену каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="cabaa-124">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processPath` | <p><span data-ttu-id="cabaa-125">Обязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="cabaa-125">Required string attribute.</span></span></p><p><span data-ttu-id="cabaa-126">Путь к исполняемому файлу, который запускает процесс, который прослушивает HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="cabaa-126">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="cabaa-127">Поддерживаются относительные пути.</span><span class="sxs-lookup"><span data-stu-id="cabaa-127">Relative paths are supported.</span></span> <span data-ttu-id="cabaa-128">Если путь начинается с `.`, путь считается относительно корневого каталога сайта.</span><span class="sxs-lookup"><span data-stu-id="cabaa-128">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="cabaa-129">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="cabaa-129">Optional integer attribute.</span></span></p><p><span data-ttu-id="cabaa-130">Указывает число раз в процесс **processPath** может завершиться сбоем в минуту.</span><span class="sxs-lookup"><span data-stu-id="cabaa-130">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="cabaa-131">Если этот предел превышен, модуль останавливает запуск процесса в течение оставшейся части минуты.</span><span class="sxs-lookup"><span data-stu-id="cabaa-131">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | `10` |
| `requestTimeout` | <p><span data-ttu-id="cabaa-132">Timespan необязательный атрибут.</span><span class="sxs-lookup"><span data-stu-id="cabaa-132">Optional timespan attribute.</span></span></p><p><span data-ttu-id="cabaa-133">Указывает продолжительность, для которого модуль ASP.NET Core ожидает ответа от процесса, прослушивающего порт % ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="cabaa-133">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="cabaa-134">`requestTimeout` Должно быть указано в целых минутах, в противном случае по умолчанию на 2 минуты.</span><span class="sxs-lookup"><span data-stu-id="cabaa-134">The `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | `00:02:00` |
| `shutdownTimeLimit` | <p><span data-ttu-id="cabaa-135">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="cabaa-135">Optional integer attribute.</span></span></p><p><span data-ttu-id="cabaa-136">Длительность в секундах, для которых модуль ожидает исполняемый файл для корректного завершения работы при *app_offline.htm* обнаружен файл.</span><span class="sxs-lookup"><span data-stu-id="cabaa-136">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | `10` |
| `startupTimeLimit` | <p><span data-ttu-id="cabaa-137">Необязательный целочисленный атрибут.</span><span class="sxs-lookup"><span data-stu-id="cabaa-137">Optional integer attribute.</span></span></p><p><span data-ttu-id="cabaa-138">Длительность в секундах модуль для исполняемого файла для запуска процесса, прослушивающего порт.</span><span class="sxs-lookup"><span data-stu-id="cabaa-138">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="cabaa-139">При превышении этого ограничения модуль процесс.</span><span class="sxs-lookup"><span data-stu-id="cabaa-139">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="cabaa-140">Модуль попытается перезапустить процесс при получении нового запроса и попытаться перезапустить процесс на последующие входящие запросы, если не удается запустить приложение по-прежнему **rapidFailsPerMinute** раз за последние последовательное минуты.</span><span class="sxs-lookup"><span data-stu-id="cabaa-140">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | `120` |
| `stdoutLogEnabled` | <p><span data-ttu-id="cabaa-141">Дополнительный логический атрибут.</span><span class="sxs-lookup"><span data-stu-id="cabaa-141">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="cabaa-142">Если значение равно true, **stdout** и **stderr** для процесса, указанного в **processPath** перенаправляются к файлу, заданному в **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="cabaa-142">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="cabaa-143">Необязательный строковый атрибут.</span><span class="sxs-lookup"><span data-stu-id="cabaa-143">Optional string attribute.</span></span></p><p><span data-ttu-id="cabaa-144">Указывает относительный или абсолютный путь, для которого **stdout** и **stderr** из процесса, указанного в **processPath** регистрируются.</span><span class="sxs-lookup"><span data-stu-id="cabaa-144">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="cabaa-145">Относительные пути задаются относительно корневого каталога сайта.</span><span class="sxs-lookup"><span data-stu-id="cabaa-145">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="cabaa-146">Любой путь, начинающийся с `.` являются относительно узла корневого и другими путями, считаются абсолютные пути.</span><span class="sxs-lookup"><span data-stu-id="cabaa-146">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="cabaa-147">Все папки, путь должен существовать для модуля, который нужно создать файл журнала.</span><span class="sxs-lookup"><span data-stu-id="cabaa-147">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="cabaa-148">С помощью разделителей подчеркивания, timestamp, идентификатор процесса и расширение файла (*.log*) добавляются к последнему сегмент **stdoutLogFile** пути.</span><span class="sxs-lookup"><span data-stu-id="cabaa-148">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="cabaa-149">Если `.\logs\stdout` передается как значение, создается журнал в stdout примере сохраняется как *stdout_20180205194132_1934.log* в *журналы* папки при сохранении на 2, 5/2018 в 19:41:32 с Идентификатором процесса 1934.</span><span class="sxs-lookup"><span data-stu-id="cabaa-149">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="cabaa-150">Задание переменных среды</span><span class="sxs-lookup"><span data-stu-id="cabaa-150">Setting environment variables</span></span>

<span data-ttu-id="cabaa-151">Переменные среды можно указать в процесс `processPath` атрибута.</span><span class="sxs-lookup"><span data-stu-id="cabaa-151">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="cabaa-152">Укажите переменную среды с `environmentVariable` дочерний элемент элемента `environmentVariables` элемент коллекции.</span><span class="sxs-lookup"><span data-stu-id="cabaa-152">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="cabaa-153">Переменные среды, заданные в этом разделе имеют приоритет над системных переменных среды.</span><span class="sxs-lookup"><span data-stu-id="cabaa-153">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="cabaa-154">В следующем примере две переменные среды.</span><span class="sxs-lookup"><span data-stu-id="cabaa-154">The following example sets two environment variables.</span></span> <span data-ttu-id="cabaa-155">`ASPNETCORE_ENVIRONMENT` Настраивает среду приложения для `Development`.</span><span class="sxs-lookup"><span data-stu-id="cabaa-155">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="cabaa-156">Разработчик может временно задать это значение *web.config* файл, чтобы принудительно реализовать [страницы разработчик исключений](xref:fundamentals/error-handling) для загрузки при отладке приложения исключения.</span><span class="sxs-lookup"><span data-stu-id="cabaa-156">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="cabaa-157">`CONFIG_DIR` Примером может служить переменной пользовательской среды, где разработчик написал код, который считывает значение во время запуска, чтобы сформировать путь для загрузки файла конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="cabaa-157">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!WARNING]
> <span data-ttu-id="cabaa-158">Задать только `ASPNETCORE_ENVIRONMENT` envirnonment переменной `Development` на промежуточных и тестовых серверах, которые не доступны к ненадежным сетям, например в Интернете.</span><span class="sxs-lookup"><span data-stu-id="cabaa-158">Only set the `ASPNETCORE_ENVIRONMENT` envirnonment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="cabaa-159">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="cabaa-159">app_offline.htm</span></span>

<span data-ttu-id="cabaa-160">Если файл с именем *app_offline.htm* обнаруживается в корневом каталоге приложения, модуль ASP.NET Core пытается корректного завершения работы приложения и остановка обработки входящих запросов.</span><span class="sxs-lookup"><span data-stu-id="cabaa-160">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="cabaa-161">Если приложение по-прежнему выполняется через количество секунд, определенные в `shutdownTimeLimit`, модуль ASP.NET Core разрывает выполняющийся процесс.</span><span class="sxs-lookup"><span data-stu-id="cabaa-161">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="cabaa-162">Хотя *app_offline.htm* файл присутствует, модуль ASP.NET Core отвечает на запросы, отправляет содержимое *app_offline.htm* файла.</span><span class="sxs-lookup"><span data-stu-id="cabaa-162">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="cabaa-163">Когда *app_offline.htm* файл удаляется, следующий запрос запускает приложение.</span><span class="sxs-lookup"><span data-stu-id="cabaa-163">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="cabaa-164">При запуске страницы ошибок</span><span class="sxs-lookup"><span data-stu-id="cabaa-164">Start-up error page</span></span>

<span data-ttu-id="cabaa-165">Если модуль Core ASP.NET не удалось запустить фоновой обработки или начинается процесс серверной части, но не удается прослушать настроенный порт *502.5 сбой процесса* появится страница кода состояния.</span><span class="sxs-lookup"><span data-stu-id="cabaa-165">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 Process Failure* status code page appears.</span></span> <span data-ttu-id="cabaa-166">Отмените запуск этой страницы и вернуться к кодовой странице состояние IIS 502 по умолчанию, используйте `disableStartUpErrorPage` атрибута.</span><span class="sxs-lookup"><span data-stu-id="cabaa-166">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="cabaa-167">Дополнительные сведения о настройке пользовательские сообщения об ошибках см. в разделе [ошибки HTTP `<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="cabaa-167">For more information on configuring custom error messages, see [HTTP Errors `<httpErrors>`](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 страница кода состояния сбоя процесса](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="cabaa-169">Создание журнала и перенаправления</span><span class="sxs-lookup"><span data-stu-id="cabaa-169">Log creation and redirection</span></span>

<span data-ttu-id="cabaa-170">Модуль Core ASP.NET перенаправляет `stdout` и `stderr` журналы на диск, если `stdoutLogEnabled` и `stdoutLogFile` атрибуты `aspNetCore` задается.</span><span class="sxs-lookup"><span data-stu-id="cabaa-170">The ASP.NET Core Module redirects `stdout` and `stderr` logs to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="cabaa-171">Все папки в `stdoutLogFile` путь должен существовать в порядке для модуля, который нужно создать файл журнала.</span><span class="sxs-lookup"><span data-stu-id="cabaa-171">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="cabaa-172">Отметка времени и расширением файла добавляются автоматически при создании файла журнала.</span><span class="sxs-lookup"><span data-stu-id="cabaa-172">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="cabaa-173">Журналы не заменяется, пока не произойдет перезапуск или перезапуска процесса.</span><span class="sxs-lookup"><span data-stu-id="cabaa-173">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="cabaa-174">Он отвечает поставщика услуг размещения, чтобы ограничить дисковое пространство, которое использовать журналы.</span><span class="sxs-lookup"><span data-stu-id="cabaa-174">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span> <span data-ttu-id="cabaa-175">С помощью `stdout` журнала рекомендуется только для устранения неполадок при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="cabaa-175">Using the `stdout` log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="cabaa-176">Не используйте stdout журнала для ведения журнала Общие приложения.</span><span class="sxs-lookup"><span data-stu-id="cabaa-176">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="cabaa-177">Для процедуры входа в приложение ASP.NET Core, используйте библиотеку ведения журнала, которая ограничивает размер файла журнала и поворачивает журналы.</span><span class="sxs-lookup"><span data-stu-id="cabaa-177">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="cabaa-178">Дополнительные сведения см. в разделе [сторонних регистраторов](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="cabaa-178">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="cabaa-179">Имя файла журнала составляется путем добавления timestamp, идентификатор процесса и расширение файла (*.log*) для последнего сегмента `stdoutLogFile` пути (обычно *stdout*) с разделителями, символами подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="cabaa-179">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="cabaa-180">Если `stdoutLogFile` путь заканчивается *stdout*, имя файла имеет журналов для приложений с PID 1934, созданные на 2, 5/2018 в 19:42:32 *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="cabaa-180">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="cabaa-181">Следующий пример `aspNetCore` элемент настраивает `stdout` входа для приложения, размещенного в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="cabaa-181">The following sample `aspNetCore` element configures `stdout` logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="cabaa-182">Локальный путь или путь к сетевой папке приемлемо для ведения локального журнала.</span><span class="sxs-lookup"><span data-stu-id="cabaa-182">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="cabaa-183">Убедитесь, что удостоверение AppPool пользователя имеется разрешение на запись в указанный путь.</span><span class="sxs-lookup"><span data-stu-id="cabaa-183">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

<span data-ttu-id="cabaa-184">В разделе [конфигурации с web.config](#configuration-with-webconfig) пример `aspNetCore` элемент в *web.config* файла.</span><span class="sxs-lookup"><span data-stu-id="cabaa-184">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="cabaa-185">Модуль ASP.NET Core с IIS общей конфигурации</span><span class="sxs-lookup"><span data-stu-id="cabaa-185">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="cabaa-186">Модуль ASP.NET Core программа установки запускается с правами **системы** учетной записи.</span><span class="sxs-lookup"><span data-stu-id="cabaa-186">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="cabaa-187">Так как учетная запись локальной системы не имеют права на изменение для путь к общей папке, используемой общей конфигурации IIS, установщик достигает отказ в доступе при попытке настроить параметры для модуля *applicationHost.config* в общей папке.</span><span class="sxs-lookup"><span data-stu-id="cabaa-187">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="cabaa-188">При использовании общей конфигурации IIS, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="cabaa-188">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="cabaa-189">Отключение общей конфигурации IIS.</span><span class="sxs-lookup"><span data-stu-id="cabaa-189">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="cabaa-190">Запустите установщик.</span><span class="sxs-lookup"><span data-stu-id="cabaa-190">Run the installer.</span></span>
1. <span data-ttu-id="cabaa-191">Экспорт обновленного *applicationHost.config* файл в общую папку.</span><span class="sxs-lookup"><span data-stu-id="cabaa-191">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="cabaa-192">Повторное включение общей конфигурации IIS.</span><span class="sxs-lookup"><span data-stu-id="cabaa-192">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="cabaa-193">Версия модуля и размещения журналы установщика пакета</span><span class="sxs-lookup"><span data-stu-id="cabaa-193">Module version and hosting bundle installer logs</span></span>

<span data-ttu-id="cabaa-194">Чтобы определить версию установленного модуля ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="cabaa-194">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="cabaa-195">На принимающей системе, перейдите к *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="cabaa-195">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="cabaa-196">Найдите *aspnetcore.dll* файла.</span><span class="sxs-lookup"><span data-stu-id="cabaa-196">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="cabaa-197">Щелкните правой кнопкой мыши файл и выберите **свойства** из контекстного меню.</span><span class="sxs-lookup"><span data-stu-id="cabaa-197">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="cabaa-198">Выберите **сведения** вкладки. **Версия файла** и **версия продукта** представляют установленную версию модуля.</span><span class="sxs-lookup"><span data-stu-id="cabaa-198">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="cabaa-199">Журналы Windows Server, где размещены пакет установщика для модуля находятся в *C:\\пользователей\\% UserName %\\AppData\\локального\\Temp*. Этот файл имеет имя *dd_DotNetCoreWinSvrHosting__\<timestamp > _000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="cabaa-199">The Windows Server Hosting bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="cabaa-200">Модуль, схемы и конфигурации расположения файлов</span><span class="sxs-lookup"><span data-stu-id="cabaa-200">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="cabaa-201">выхода</span><span class="sxs-lookup"><span data-stu-id="cabaa-201">Module</span></span>

<span data-ttu-id="cabaa-202">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="cabaa-202">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="cabaa-203">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="cabaa-203">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="cabaa-204">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="cabaa-204">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="cabaa-205">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="cabaa-205">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="cabaa-206">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="cabaa-206">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="cabaa-207">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="cabaa-207">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="cabaa-208">Схема</span><span class="sxs-lookup"><span data-stu-id="cabaa-208">Schema</span></span>

<span data-ttu-id="cabaa-209">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="cabaa-209">**IIS**</span></span>

   * <span data-ttu-id="cabaa-210">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="cabaa-210">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="cabaa-211">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="cabaa-211">**IIS Express**</span></span>

   * <span data-ttu-id="cabaa-212">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="cabaa-212">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="cabaa-213">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="cabaa-213">Configuration</span></span>

<span data-ttu-id="cabaa-214">**Службы IIS**</span><span class="sxs-lookup"><span data-stu-id="cabaa-214">**IIS**</span></span>

   * <span data-ttu-id="cabaa-215">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="cabaa-215">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="cabaa-216">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="cabaa-216">**IIS Express**</span></span>

   * <span data-ttu-id="cabaa-217">.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="cabaa-217">.vs\config\applicationHost.config</span></span>

<span data-ttu-id="cabaa-218">Файлы можно найти путем поиска *aspnetcore.dll* в *applicationHost.config* файла.</span><span class="sxs-lookup"><span data-stu-id="cabaa-218">The files can be found by searching for *aspnetcore.dll* in the *applicationHost.config* file.</span></span> <span data-ttu-id="cabaa-219">Для IIS Express *applicationHost.config* файл не должен существовать по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cabaa-219">For IIS Express, the *applicationHost.config* file won't exist by default.</span></span> <span data-ttu-id="cabaa-220">Файл будет создан в  *\<application_root >\\удаляйте\\config* при запуске приложения любой веб-проект в решении Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cabaa-220">The file is created at *\<application_root>\\.vs\\config* when starting any web app project in the Visual Studio solution.</span></span>
