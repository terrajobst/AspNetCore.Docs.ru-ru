---
title: "Работа с несколькими средами в ASP.NET Core"
author: rick-anderson
description: "Узнайте, как ASP.NET Core предоставляет поддержку для управления поведением приложения в нескольких средах."
ms.author: riande
manager: wpickett
ms.date: 12/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 60a1543ce11d08490e6df0eb84f980672ecfe672
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="working-with-multiple-environments"></a><span data-ttu-id="fde05-103">Работа с несколькими средами</span><span class="sxs-lookup"><span data-stu-id="fde05-103">Working with multiple environments</span></span>

<span data-ttu-id="fde05-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="fde05-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fde05-105">ASP.NET Core обеспечивает поддержку установки поведение приложения во время выполнения с переменными среды.</span><span class="sxs-lookup"><span data-stu-id="fde05-105">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="fde05-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fde05-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="fde05-107">Среды</span><span class="sxs-lookup"><span data-stu-id="fde05-107">Environments</span></span>

<span data-ttu-id="fde05-108">ASP.NET Core считывает переменной среды `ASPNETCORE_ENVIRONMENT` при запуске приложения и сохраняет это значение в [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="fde05-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="fde05-109">`ASPNETCORE_ENVIRONMENT`можно задать любое значение, но [три значения](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) поддерживаются платформой: [разработки](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [промежуточной](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), и [рабочей](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="fde05-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="fde05-110">Если `ASPNETCORE_ENVIRONMENT` не задано, то по умолчанию `Production`.</span><span class="sxs-lookup"><span data-stu-id="fde05-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it will default to `Production`.</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="fde05-111">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="fde05-111">The preceding code:</span></span>

* <span data-ttu-id="fde05-112">Вызовы [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) и [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) при `ASPNETCORE_ENVIRONMENT` равно `Development`.</span><span class="sxs-lookup"><span data-stu-id="fde05-112">Calls [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="fde05-113">Вызовы [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) при значение `ASPNETCORE_ENVIRONMENT` задано одно из следующих:</span><span class="sxs-lookup"><span data-stu-id="fde05-113">Calls [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="fde05-114">[Вспомогательный тега среды ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) использует значение `IHostingEnvironment.EnvironmentName` для включения или исключения разметки в элементе:</span><span class="sxs-lookup"><span data-stu-id="fde05-114">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="fde05-115">Обратите внимание: В Windows и macOS, переменные среды и значения не учитывается регистр.</span><span class="sxs-lookup"><span data-stu-id="fde05-115">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="fde05-116">Переменные среды для Linux и значения являются **регистра** по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fde05-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="fde05-117">Разработка</span><span class="sxs-lookup"><span data-stu-id="fde05-117">Development</span></span>

<span data-ttu-id="fde05-118">В среде разработки можно включить функции, которые не должны предоставляться в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="fde05-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="fde05-119">Например, включить шаблоны ASP.NET Core [страницы разработчик исключений](xref:fundamentals/error-handling#the-developer-exception-page) в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="fde05-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="fde05-120">Среда для локального компьютера может быть задано в *Properties\launchSettings.json* файла проекта.</span><span class="sxs-lookup"><span data-stu-id="fde05-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="fde05-121">Задание значений среды в *launchSettings.json* переопределяют значения, заданные в среде системы.</span><span class="sxs-lookup"><span data-stu-id="fde05-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="fde05-122">Следующий код XML показывает три профиля из *launchSettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="fde05-122">The following XML shows three profiles from a *launchSettings.json* file:</span></span>

[!code-xml[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

<span data-ttu-id="fde05-123">При запуске приложения с `dotnet run`, первый профиль с `"commandName": "Project"` будет использоваться.</span><span class="sxs-lookup"><span data-stu-id="fde05-123">When the application is launched with `dotnet run`, the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="fde05-124">Значение `commandName` указывает веб-сервера для запуска.</span><span class="sxs-lookup"><span data-stu-id="fde05-124">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="fde05-125">`commandName`может принимать одно из:</span><span class="sxs-lookup"><span data-stu-id="fde05-125">`commandName` can be one of :</span></span>

* <span data-ttu-id="fde05-126">IIS Express</span><span class="sxs-lookup"><span data-stu-id="fde05-126">IIS Express</span></span>
* <span data-ttu-id="fde05-127">IIS</span><span class="sxs-lookup"><span data-stu-id="fde05-127">IIS</span></span>
* <span data-ttu-id="fde05-128">Проект (которое запускает Kestrel)</span><span class="sxs-lookup"><span data-stu-id="fde05-128">Project (which launches Kestrel)</span></span>

<span data-ttu-id="fde05-129">При запуске приложения с `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="fde05-129">When an app is launched with `dotnet run`:</span></span>

* <span data-ttu-id="fde05-130">*launchSettings.json* считывается если он доступен.</span><span class="sxs-lookup"><span data-stu-id="fde05-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="fde05-131">`environmentVariables`параметры в *launchSettings.json* переопределение переменных среды.</span><span class="sxs-lookup"><span data-stu-id="fde05-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="fde05-132">Среда размещения отображается.</span><span class="sxs-lookup"><span data-stu-id="fde05-132">The hosting environment is displayed.</span></span>


<span data-ttu-id="fde05-133">Ниже показано приложение к работе с `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="fde05-133">The following output shows an app started with `dotnet run`:</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="fde05-134">Visual Studio **отладки** вкладка предоставляет графический интерфейс пользователя для редактирования *launchSettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="fde05-134">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Переменные среды параметр свойства проекта](environments/_static/project-properties-debug.png)

<span data-ttu-id="fde05-136">Изменения, внесенные в проект профилей могут не вступают в силу только после перезапуска веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="fde05-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="fde05-137">Необходимо перезапустить kestrel, прежде чем он обнаруживает изменения, внесенные в своей среде.</span><span class="sxs-lookup"><span data-stu-id="fde05-137">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="fde05-138">*launchSettings.json* не следует хранить секреты.</span><span class="sxs-lookup"><span data-stu-id="fde05-138">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="fde05-139">[Секрет диспетчера](xref:security/app-secrets) может использоваться для хранения конфиденциальных данных для локальной разработки.</span><span class="sxs-lookup"><span data-stu-id="fde05-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="fde05-140">Производство</span><span class="sxs-lookup"><span data-stu-id="fde05-140">Production</span></span>

<span data-ttu-id="fde05-141">В рабочей среде должны быть настроены для повышения безопасности, производительности и надежности приложений.</span><span class="sxs-lookup"><span data-stu-id="fde05-141">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="fde05-142">Ниже перечислены некоторые общие параметры, которые, возможно, в производственную среду, будут отличаться от разработки.</span><span class="sxs-lookup"><span data-stu-id="fde05-142">Some common settings that a production environment might have that would differ from development include:</span></span>

* <span data-ttu-id="fde05-143">Кэширование.</span><span class="sxs-lookup"><span data-stu-id="fde05-143">Caching.</span></span>
* <span data-ttu-id="fde05-144">Клиентские ресурсы объединяются в один пакет, уменьшено и потенциально полученном из CDN.</span><span class="sxs-lookup"><span data-stu-id="fde05-144">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="fde05-145">Страницы диагностики ошибок отключена.</span><span class="sxs-lookup"><span data-stu-id="fde05-145">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="fde05-146">Страницы ошибок включено.</span><span class="sxs-lookup"><span data-stu-id="fde05-146">Friendly error pages enabled.</span></span>
* <span data-ttu-id="fde05-147">Ведение журнала производства и мониторинг включен.</span><span class="sxs-lookup"><span data-stu-id="fde05-147">Production logging and monitoring enabled.</span></span> <span data-ttu-id="fde05-148">Например [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).</span><span class="sxs-lookup"><span data-stu-id="fde05-148">For example, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="fde05-149">Настройка среды</span><span class="sxs-lookup"><span data-stu-id="fde05-149">Setting the environment</span></span>

<span data-ttu-id="fde05-150">Часто полезно задать конкретную среду для тестирования.</span><span class="sxs-lookup"><span data-stu-id="fde05-150">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="fde05-151">Если среда не настроена, то по умолчанию `Production` отключающее большинство функций отладки.</span><span class="sxs-lookup"><span data-stu-id="fde05-151">If the environment isn't set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="fde05-152">Метод для настройки среды, зависит от операционной системы.</span><span class="sxs-lookup"><span data-stu-id="fde05-152">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="fde05-153">Azure</span><span class="sxs-lookup"><span data-stu-id="fde05-153">Azure</span></span>

<span data-ttu-id="fde05-154">Для службы приложения Azure:</span><span class="sxs-lookup"><span data-stu-id="fde05-154">For Azure app service:</span></span>

* <span data-ttu-id="fde05-155">Выберите **параметры приложения** колонку.</span><span class="sxs-lookup"><span data-stu-id="fde05-155">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="fde05-156">Добавить ключ и значение в **параметры приложения**.</span><span class="sxs-lookup"><span data-stu-id="fde05-156">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="fde05-157">Windows</span><span class="sxs-lookup"><span data-stu-id="fde05-157">Windows</span></span>
<span data-ttu-id="fde05-158">Чтобы задать `ASPNETCORE_ENVIRONMENT` для текущего сеанса, если приложение запускается с помощью `dotnet run`, используются следующие команды</span><span class="sxs-lookup"><span data-stu-id="fde05-158">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using `dotnet run`, the following commands are used</span></span>

<span data-ttu-id="fde05-159">**командная строка;**</span><span class="sxs-lookup"><span data-stu-id="fde05-159">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="fde05-160">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="fde05-160">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="fde05-161">Эти команды вступают в силу только для текущего окна.</span><span class="sxs-lookup"><span data-stu-id="fde05-161">These commands take effect only for the current window.</span></span> <span data-ttu-id="fde05-162">При закрытии окна параметр ASPNETCORE_ENVIRONMENT возвращается по умолчанию или значение машины.</span><span class="sxs-lookup"><span data-stu-id="fde05-162">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="fde05-163">Чтобы установить значение глобально при открытии Windows **панели управления** > **системы** > **расширенных параметров системы** и добавление или изменение `ASPNETCORE_ENVIRONMENT` значение.</span><span class="sxs-lookup"><span data-stu-id="fde05-163">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Дополнительные свойства системы](environments/_static/systemsetting_environment.png)

![Переменная среды ASPNET Core](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="fde05-166">**web.config**</span><span class="sxs-lookup"><span data-stu-id="fde05-166">**web.config**</span></span>

<span data-ttu-id="fde05-167">В разделе *задание переменных среды* раздел [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) раздела.</span><span class="sxs-lookup"><span data-stu-id="fde05-167">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="fde05-168">**На пул приложений IIS**</span><span class="sxs-lookup"><span data-stu-id="fde05-168">**Per IIS Application Pool**</span></span>

<span data-ttu-id="fde05-169">Для установки переменных среды для отдельных приложений, выполняющихся в изолированной пулы приложений (поддерживается в IIS 10.0 +), в разделе *команды AppCmd.exe* раздел [переменных среды \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) раздела.</span><span class="sxs-lookup"><span data-stu-id="fde05-169">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="fde05-170">MacOS</span><span class="sxs-lookup"><span data-stu-id="fde05-170">macOS</span></span>
<span data-ttu-id="fde05-171">Установка текущей среды для macOS можно сделать в строки при запуске приложения;</span><span class="sxs-lookup"><span data-stu-id="fde05-171">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="fde05-172">или с помощью `export` задайте его перед запуском приложения.</span><span class="sxs-lookup"><span data-stu-id="fde05-172">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="fde05-173">Переменные среды уровня машины задаются *.bashrc* или *.bash_profile* файла.</span><span class="sxs-lookup"><span data-stu-id="fde05-173">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="fde05-174">Измените файл, используя любой текстовый редактор и добавьте следующий оператор.</span><span class="sxs-lookup"><span data-stu-id="fde05-174">Edit the file using any text editor and add the following statment.</span></span>

```
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="fde05-175">Linux</span><span class="sxs-lookup"><span data-stu-id="fde05-175">Linux</span></span>
<span data-ttu-id="fde05-176">Дистрибутивы Linux, используйте `export` команды из командной строки для параметров переменных на основе сеансов и *bash_profile* файла параметров уровня среды компьютера.</span><span class="sxs-lookup"><span data-stu-id="fde05-176">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="fde05-177">Конфигурация для разных сред</span><span class="sxs-lookup"><span data-stu-id="fde05-177">Configuration by environment</span></span>

<span data-ttu-id="fde05-178">В разделе [конфигурации средой](xref:fundamentals/configuration/index#configuration-by-environment) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="fde05-178">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="fde05-179">Класс запуска и методы на основе среды</span><span class="sxs-lookup"><span data-stu-id="fde05-179">Environment based Startup class and methods</span></span>

<span data-ttu-id="fde05-180">При запуске приложения ASP.NET Core [класс запуска](xref:fundamentals/startup) загружает приложение.</span><span class="sxs-lookup"><span data-stu-id="fde05-180">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="fde05-181">Если класс `Startup{EnvironmentName}` существует, что будет вызван для этого `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="fde05-181">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="fde05-182">Примечание: Вызов [WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) переопределяет разделы конфигурации.</span><span class="sxs-lookup"><span data-stu-id="fde05-182">Note: Calling [WebHostBuilder.UseStartup<TStartup>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="fde05-183">[Настройка](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) и [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) поддержки конкретных версий среды формы `Configure{EnvironmentName}` и `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="fde05-183">[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="fde05-184">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="fde05-184">Additional Resources</span></span>

* [<span data-ttu-id="fde05-185">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="fde05-185">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="fde05-186">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="fde05-186">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="fde05-187">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="fde05-187">IHostingEnvironment.EnvironmentName</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
