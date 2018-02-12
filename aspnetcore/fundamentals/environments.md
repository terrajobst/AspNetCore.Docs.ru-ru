---
title: "Работа с несколькими средами в ASP.NET Core"
author: rick-anderson
description: "Сведения о поддержке управления поведением приложений в разных средах в ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 12/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/environments
ms.openlocfilehash: b40ee9b1c6feae4942f05d22dab776d3cf6c26a0
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="working-with-multiple-environments"></a><span data-ttu-id="6dcae-103">Работа с несколькими средами</span><span class="sxs-lookup"><span data-stu-id="6dcae-103">Working with multiple environments</span></span>

<span data-ttu-id="6dcae-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="6dcae-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6dcae-105">ASP.NET Core дает возможность настраивать поведение приложения во время выполнения с помощью переменных среды.</span><span class="sxs-lookup"><span data-stu-id="6dcae-105">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="6dcae-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6dcae-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="6dcae-107">Среды</span><span class="sxs-lookup"><span data-stu-id="6dcae-107">Environments</span></span>

<span data-ttu-id="6dcae-108">ASP.NET Core считывает переменную среды `ASPNETCORE_ENVIRONMENT` при запуске приложения и сохраняет ее значение в свойстве [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="6dcae-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="6dcae-109">Переменной `ASPNETCORE_ENVIRONMENT` можно присвоить любое значение, но платформа поддерживает [три значения](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0): [Development](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0) и [Production](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="6dcae-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="6dcae-110">Если переменная `ASPNETCORE_ENVIRONMENT` не задана, по умолчанию используется значение `Production`.</span><span class="sxs-lookup"><span data-stu-id="6dcae-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it will default to `Production`.</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="6dcae-111">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="6dcae-111">The preceding code:</span></span>

* <span data-ttu-id="6dcae-112">Вызывает [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) и [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_), если переменная `ASPNETCORE_ENVIRONMENT` имеет значение `Development`.</span><span class="sxs-lookup"><span data-stu-id="6dcae-112">Calls [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="6dcae-113">Вызывает [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_), если переменная `ASPNETCORE_ENVIRONMENT` имеет одно из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="6dcae-113">Calls [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="6dcae-114">[Вспомогательная функция тега среды](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) использует значение `IHostingEnvironment.EnvironmentName` для включения или исключения разметки в элементе:</span><span class="sxs-lookup"><span data-stu-id="6dcae-114">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="6dcae-115">Примечание. В ОС Windows и macOS регистр символов в переменных среды и их значениях не учитывается.</span><span class="sxs-lookup"><span data-stu-id="6dcae-115">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="6dcae-116">В ОС Linux в переменных среды и их значениях **регистр символов по умолчанию учитывается**.</span><span class="sxs-lookup"><span data-stu-id="6dcae-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="6dcae-117">Разработка</span><span class="sxs-lookup"><span data-stu-id="6dcae-117">Development</span></span>

<span data-ttu-id="6dcae-118">В среде разработки могут быть включены функции, которые не должны быть доступны в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="6dcae-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="6dcae-119">Например, шаблоны ASP.NET Core включают в среде разработки [страницу со сведениями об исключении для разработчика](xref:fundamentals/error-handling#the-developer-exception-page).</span><span class="sxs-lookup"><span data-stu-id="6dcae-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="6dcae-120">Среду для локального компьютера разработки можно задать в файле *Properties\launchSettings.json* проекта.</span><span class="sxs-lookup"><span data-stu-id="6dcae-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="6dcae-121">Значения среды, заданные в файле *launchSettings.json*, переопределяют значения, заданные в системной среде.</span><span class="sxs-lookup"><span data-stu-id="6dcae-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="6dcae-122">В следующем коде JSON показаны три профиля из файла *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="6dcae-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

[!code-json[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

<span data-ttu-id="6dcae-123">Когда приложение запускается с помощью команды `dotnet run`, используется первый профиль с атрибутом `"commandName": "Project"`.</span><span class="sxs-lookup"><span data-stu-id="6dcae-123">When the application is launched with `dotnet run`, the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="6dcae-124">Значение `commandName` определяет запускаемый веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="6dcae-124">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="6dcae-125">`commandName` может иметь одно из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="6dcae-125">`commandName` can be one of :</span></span>

* <span data-ttu-id="6dcae-126">IIS Express</span><span class="sxs-lookup"><span data-stu-id="6dcae-126">IIS Express</span></span>
* <span data-ttu-id="6dcae-127">IIS</span><span class="sxs-lookup"><span data-stu-id="6dcae-127">IIS</span></span>
* <span data-ttu-id="6dcae-128">Project (запускается Kestrel)</span><span class="sxs-lookup"><span data-stu-id="6dcae-128">Project (which launches Kestrel)</span></span>

<span data-ttu-id="6dcae-129">Когда приложение запускается с помощью команды `dotnet run`, выполняются указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="6dcae-129">When an app is launched with `dotnet run`:</span></span>

* <span data-ttu-id="6dcae-130">Считывается файл *launchSettings.json*, если он доступен.</span><span class="sxs-lookup"><span data-stu-id="6dcae-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="6dcae-131">Параметры `environmentVariables` в файле *launchSettings.json* переопределяют переменные среды.</span><span class="sxs-lookup"><span data-stu-id="6dcae-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="6dcae-132">Отображается среда размещения.</span><span class="sxs-lookup"><span data-stu-id="6dcae-132">The hosting environment is displayed.</span></span>


<span data-ttu-id="6dcae-133">Ниже представлены выходные данные приложения, запущенного с помощью команды `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="6dcae-133">The following output shows an app started with `dotnet run`:</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="6dcae-134">Вкладка **Отладка** в Visual Studio предоставляет графический пользовательский интерфейс для изменения файла *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="6dcae-134">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Задание переменных среды в свойствах проекта](environments/_static/project-properties-debug.png)

<span data-ttu-id="6dcae-136">Для вступления в силу изменений, внесенных в профили проекта, может потребоваться перезапуск веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="6dcae-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="6dcae-137">Чтобы сервер Kestrel обнаружил изменения, внесенные в среду, его необходимо перезапустить.</span><span class="sxs-lookup"><span data-stu-id="6dcae-137">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="6dcae-138">В файле *launchSettings.json* не должны храниться секреты.</span><span class="sxs-lookup"><span data-stu-id="6dcae-138">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="6dcae-139">Для хранения секретов во время разработки в локальной среде можно использовать [средство Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="6dcae-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="6dcae-140">Производство</span><span class="sxs-lookup"><span data-stu-id="6dcae-140">Production</span></span>

<span data-ttu-id="6dcae-141">Конфигурация рабочей среды должна обеспечивать максимальный уровень безопасности, производительности и надежности приложений.</span><span class="sxs-lookup"><span data-stu-id="6dcae-141">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="6dcae-142">Вот некоторые параметры, которые в рабочей среде могут отличаться от аналогичных параметров в среде разработки:</span><span class="sxs-lookup"><span data-stu-id="6dcae-142">Some common settings that a production environment might have that would differ from development include:</span></span>

* <span data-ttu-id="6dcae-143">кэширование;</span><span class="sxs-lookup"><span data-stu-id="6dcae-143">Caching.</span></span>
* <span data-ttu-id="6dcae-144">ресурсы на стороне клиента объединяются в пакеты, уплотняются и могут предоставляться из сети CDN;</span><span class="sxs-lookup"><span data-stu-id="6dcae-144">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="6dcae-145">страницы с сообщениями об ошибках диагностики отключены;</span><span class="sxs-lookup"><span data-stu-id="6dcae-145">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="6dcae-146">включены страницы с понятными пользователям сообщениями об ошибках;</span><span class="sxs-lookup"><span data-stu-id="6dcae-146">Friendly error pages enabled.</span></span>
* <span data-ttu-id="6dcae-147">включены средства ведения журналов и мониторинга в рабочей среде,</span><span class="sxs-lookup"><span data-stu-id="6dcae-147">Production logging and monitoring enabled.</span></span> <span data-ttu-id="6dcae-148">например [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="6dcae-148">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="6dcae-149">Указание среды</span><span class="sxs-lookup"><span data-stu-id="6dcae-149">Setting the environment</span></span>

<span data-ttu-id="6dcae-150">Часто бывает полезным указать определенную среду для тестирования.</span><span class="sxs-lookup"><span data-stu-id="6dcae-150">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="6dcae-151">Если среда не указана, по умолчанию используется среда `Production`, в которой большинство функций отладки отключено.</span><span class="sxs-lookup"><span data-stu-id="6dcae-151">If the environment isn't set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="6dcae-152">Способ указания среды зависит от операционной системы.</span><span class="sxs-lookup"><span data-stu-id="6dcae-152">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="6dcae-153">Azure</span><span class="sxs-lookup"><span data-stu-id="6dcae-153">Azure</span></span>

<span data-ttu-id="6dcae-154">Для службы приложений Azure:</span><span class="sxs-lookup"><span data-stu-id="6dcae-154">For Azure app service:</span></span>

* <span data-ttu-id="6dcae-155">Выберите колонку **Параметры приложения**.</span><span class="sxs-lookup"><span data-stu-id="6dcae-155">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="6dcae-156">Добавьте ключ и значение в поле **Параметры приложения**.</span><span class="sxs-lookup"><span data-stu-id="6dcae-156">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="6dcae-157">Windows</span><span class="sxs-lookup"><span data-stu-id="6dcae-157">Windows</span></span>
<span data-ttu-id="6dcae-158">Если приложение запускается с помощью команды `dotnet run`, то, чтобы задать переменную `ASPNETCORE_ENVIRONMENT` для текущего сеанса, используйте следующие команды:</span><span class="sxs-lookup"><span data-stu-id="6dcae-158">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using `dotnet run`, the following commands are used</span></span>

<span data-ttu-id="6dcae-159">**командная строка;**</span><span class="sxs-lookup"><span data-stu-id="6dcae-159">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="6dcae-160">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="6dcae-160">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="6dcae-161">Эти команды действуют только для текущего окна.</span><span class="sxs-lookup"><span data-stu-id="6dcae-161">These commands take effect only for the current window.</span></span> <span data-ttu-id="6dcae-162">Когда окно закрывается, для переменной ASPNETCORE_ENVIRONMENT восстанавливается значение по умолчанию или значение, заданное на компьютере.</span><span class="sxs-lookup"><span data-stu-id="6dcae-162">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="6dcae-163">Чтобы задать это значение в Windows на глобальном уровне, откройте **Панель управления** > **Система** > **Дополнительные параметры системы** и добавьте или удалите значение `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="6dcae-163">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Дополнительные параметры системы](environments/_static/systemsetting_environment.png)

![Переменная среды ASPNET Core](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="6dcae-166">**web.config**</span><span class="sxs-lookup"><span data-stu-id="6dcae-166">**web.config**</span></span>

<span data-ttu-id="6dcae-167">См. раздел *Задание переменных среды* в статье [Справочник по настройке модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="6dcae-167">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="6dcae-168">**Пул приложений IIS**</span><span class="sxs-lookup"><span data-stu-id="6dcae-168">**Per IIS Application Pool**</span></span>

<span data-ttu-id="6dcae-169">Чтобы задать переменные среды для отдельных приложений, выполняющихся в изолированных пулах приложений (такая возможность поддерживается в службах IIS 10.0 и более поздних версий), см. подраздел, посвященный *команде AppCmd.exe*, в разделе [Переменные среды \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).</span><span class="sxs-lookup"><span data-stu-id="6dcae-169">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="6dcae-170">macOS</span><span class="sxs-lookup"><span data-stu-id="6dcae-170">macOS</span></span>
<span data-ttu-id="6dcae-171">Задать текущую среду в macOS можно в командной строке при запуске приложения;</span><span class="sxs-lookup"><span data-stu-id="6dcae-171">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="6dcae-172">или с помощью команды `export` перед запуском приложения.</span><span class="sxs-lookup"><span data-stu-id="6dcae-172">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="6dcae-173">Переменные среды на уровне компьютера задаются в файле *BASHRC* или *BASH_PROFILE*.</span><span class="sxs-lookup"><span data-stu-id="6dcae-173">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="6dcae-174">Измените этот файл в любом текстовом редакторе, добавив следующий оператор.</span><span class="sxs-lookup"><span data-stu-id="6dcae-174">Edit the file using any text editor and add the following statment.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="6dcae-175">Linux</span><span class="sxs-lookup"><span data-stu-id="6dcae-175">Linux</span></span>
<span data-ttu-id="6dcae-176">В дистрибутивах Linux используйте команду `export` в командной строке для значений переменных на уровне сеанса или в файле *BASH_PROFILE* для значений среды на уровне компьютера.</span><span class="sxs-lookup"><span data-stu-id="6dcae-176">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="6dcae-177">Конфигурация для разных сред</span><span class="sxs-lookup"><span data-stu-id="6dcae-177">Configuration by environment</span></span>

<span data-ttu-id="6dcae-178">Дополнительные сведения см. в разделе [Конфигурация для разных сред](xref:fundamentals/configuration/index#configuration-by-environment).</span><span class="sxs-lookup"><span data-stu-id="6dcae-178">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="6dcae-179">Класс Startup и его методы для разных сред</span><span class="sxs-lookup"><span data-stu-id="6dcae-179">Environment based Startup class and methods</span></span>

<span data-ttu-id="6dcae-180">При запуске приложения ASP.NET Core [класс Startup](xref:fundamentals/startup) выполняет его начальную загрузку.</span><span class="sxs-lookup"><span data-stu-id="6dcae-180">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="6dcae-181">Если класс `Startup{EnvironmentName}` существует, он вызывается для данной среды `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="6dcae-181">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="6dcae-182">Примечание. Вызов [WebHostBuilder.UseStartup<TStartup>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) переопределяет разделы конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6dcae-182">Note: Calling [WebHostBuilder.UseStartup<TStartup>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="6dcae-183">Методы [Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) и [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) поддерживают версии для конкретных сред в формате `Configure{EnvironmentName}` и `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="6dcae-183">[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="6dcae-184">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6dcae-184">Additional resources</span></span>

* [<span data-ttu-id="6dcae-185">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="6dcae-185">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="6dcae-186">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="6dcae-186">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="6dcae-187">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="6dcae-187">IHostingEnvironment.EnvironmentName</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
