---
title: Использование нескольких сред в ASP.NET Core
author: rick-anderson
description: Сведения об управлении поведением приложений в разных средах в приложениях ASP.NET Core.
ms.author: riande
ms.date: 07/03/2018
uid: fundamentals/environments
ms.openlocfilehash: 865257d127084671036147dd1f28c9c4843feef6
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206852"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="1bc6c-103">Использование нескольких сред в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1bc6c-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="1bc6c-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="1bc6c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1bc6c-105">ASP.NET Core настраивает поведение приложения в зависимости от среды выполнения с помощью переменной среды.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="1bc6c-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1bc6c-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="1bc6c-107">Среды</span><span class="sxs-lookup"><span data-stu-id="1bc6c-107">Environments</span></span>

<span data-ttu-id="1bc6c-108">ASP.NET Core считывает переменную среды `ASPNETCORE_ENVIRONMENT` при запуске приложения и сохраняет ее значение в свойстве [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span><span class="sxs-lookup"><span data-stu-id="1bc6c-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span></span> <span data-ttu-id="1bc6c-109">Переменной `ASPNETCORE_ENVIRONMENT` можно присвоить любое значение, но платформа поддерживает [три значения](/dotnet/api/microsoft.aspnetcore.hosting.environmentname): [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging) и [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span><span class="sxs-lookup"><span data-stu-id="1bc6c-109">You can set `ASPNETCORE_ENVIRONMENT` to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span></span> <span data-ttu-id="1bc6c-110">Если переменная `ASPNETCORE_ENVIRONMENT` не указана, используется значение по умолчанию `Production`.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it defaults to `Production`.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="1bc6c-111">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="1bc6c-111">The preceding code:</span></span>

* <span data-ttu-id="1bc6c-112">Вызывает [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage), если `ASPNETCORE_ENVIRONMENT` имеет значение `Development`.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="1bc6c-113">Вызывает [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler), если переменная `ASPNETCORE_ENVIRONMENT` имеет одно из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="1bc6c-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="1bc6c-114">[Вспомогательная функция тега среды](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) использует значение `IHostingEnvironment.EnvironmentName` для включения или исключения разметки в элементе:</span><span class="sxs-lookup"><span data-stu-id="1bc6c-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="1bc6c-115">В ОС Windows и macOS регистр символов в переменных среды и их значениях не учитывается.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="1bc6c-116">В ОС Linux в переменных среды и их значениях **регистр символов по умолчанию учитывается**.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="1bc6c-117">Разработка</span><span class="sxs-lookup"><span data-stu-id="1bc6c-117">Development</span></span>

<span data-ttu-id="1bc6c-118">В среде разработки могут быть включены функции, которые не должны быть доступны в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="1bc6c-119">Например, шаблоны ASP.NET Core включают в среде разработки [страницу со сведениями об исключении для разработчика](xref:fundamentals/error-handling#the-developer-exception-page).</span><span class="sxs-lookup"><span data-stu-id="1bc6c-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="1bc6c-120">Среду для локального компьютера разработки можно задать в файле *Properties\launchSettings.json* проекта.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="1bc6c-121">Значения среды, заданные в файле *launchSettings.json*, переопределяют значения, заданные в системной среде.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="1bc6c-122">В следующем коде JSON показаны три профиля из файла *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="1bc6c-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:54339/",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      }
    },
    "EnvironmentsSample": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:54340/"
    },
    "Kestrel Staging": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:51997/"
    }
  }
}
```

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="1bc6c-123">Свойство `applicationUrl` в *launchSettings.json* может задать список URL-адресов сервера.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="1bc6c-124">Для разделения URL-адресов в списке используется точка с запятой:</span><span class="sxs-lookup"><span data-stu-id="1bc6c-124">Use a semicolon between the URLs in the list:</span></span>
>
> ```json
> "EnvironmentsSample": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```

::: moniker-end

<span data-ttu-id="1bc6c-125">Когда приложение запускается с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run), используется первый профиль с атрибутом `"commandName": "Project"`.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="1bc6c-126">Значение `commandName` определяет запускаемый веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="1bc6c-127">`commandName` может иметь одно из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="1bc6c-127">`commandName` can be any one of the following:</span></span>

* <span data-ttu-id="1bc6c-128">IIS Express</span><span class="sxs-lookup"><span data-stu-id="1bc6c-128">IIS Express</span></span>
* <span data-ttu-id="1bc6c-129">IIS</span><span class="sxs-lookup"><span data-stu-id="1bc6c-129">IIS</span></span>
* <span data-ttu-id="1bc6c-130">Project (запускается Kestrel)</span><span class="sxs-lookup"><span data-stu-id="1bc6c-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="1bc6c-131">Когда приложение запускается с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run), выполняются указанные ниже действия:</span><span class="sxs-lookup"><span data-stu-id="1bc6c-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="1bc6c-132">Считывается файл *launchSettings.json*, если он доступен.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="1bc6c-133">Параметры `environmentVariables` в файле *launchSettings.json* переопределяют переменные среды.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="1bc6c-134">Отображается среда размещения.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-134">The hosting environment is displayed.</span></span>

<span data-ttu-id="1bc6c-135">Ниже представлены выходные данные приложения, запущенного с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="1bc6c-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="1bc6c-136">Вкладка **Отладка** в свойствах проекта Visual Studio предоставляет графический пользовательский интерфейс для изменения файла *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-136">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Задание переменных среды в свойствах проекта](environments/_static/project-properties-debug.png)

<span data-ttu-id="1bc6c-138">Для вступления в силу изменений, внесенных в профили проекта, может потребоваться перезапуск веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="1bc6c-139">Чтобы сервер Kestrel обнаружил изменения, внесенные в среду, его необходимо перезапустить.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-139">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="1bc6c-140">В файле *launchSettings.json* не должны храниться секреты.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="1bc6c-141">Для хранения секретов во время разработки в локальной среде можно использовать [средство Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="1bc6c-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="1bc6c-142">При использовании [Visual Studio Code](https://code.visualstudio.com/) переменные среды можно задавать в файле *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-142">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="1bc6c-143">В следующем примере показано, как присвоить среде значение `Development`:</span><span class="sxs-lookup"><span data-stu-id="1bc6c-143">The following example sets the environment to `Development`:</span></span>

```json
{
   "version": "0.2.0",
   "configurations": [
        {
            "name": ".NET Core Launch (web)",

            ... additional VS Code configuration settings ...

            "env": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    ]
}
```

<span data-ttu-id="1bc6c-144">Файл *.vscode/launch.json* в проекте не читается при запуске приложения с помощью команды `dotnet run` так же, как файл *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-144">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="1bc6c-145">При запуске приложения в среде разработки без файла *launchSettings.json* укажите среду с помощью переменной среды или задайте аргумент командной строки как команду `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-145">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="1bc6c-146">Производство</span><span class="sxs-lookup"><span data-stu-id="1bc6c-146">Production</span></span>

<span data-ttu-id="1bc6c-147">Конфигурация рабочей среды должна обеспечивать максимальный уровень безопасности, производительности и надежности приложений.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-147">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="1bc6c-148">Некоторые общие параметры, отличные от разработки:</span><span class="sxs-lookup"><span data-stu-id="1bc6c-148">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="1bc6c-149">кэширование;</span><span class="sxs-lookup"><span data-stu-id="1bc6c-149">Caching.</span></span>
* <span data-ttu-id="1bc6c-150">ресурсы на стороне клиента объединяются в пакеты, уплотняются и могут предоставляться из сети CDN;</span><span class="sxs-lookup"><span data-stu-id="1bc6c-150">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="1bc6c-151">страницы с сообщениями об ошибках диагностики отключены;</span><span class="sxs-lookup"><span data-stu-id="1bc6c-151">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="1bc6c-152">включены страницы с понятными пользователям сообщениями об ошибках;</span><span class="sxs-lookup"><span data-stu-id="1bc6c-152">Friendly error pages enabled.</span></span>
* <span data-ttu-id="1bc6c-153">включены средства ведения журналов и мониторинга в рабочей среде,</span><span class="sxs-lookup"><span data-stu-id="1bc6c-153">Production logging and monitoring enabled.</span></span> <span data-ttu-id="1bc6c-154">например [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="1bc6c-154">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="1bc6c-155">Указание среды</span><span class="sxs-lookup"><span data-stu-id="1bc6c-155">Set the environment</span></span>

<span data-ttu-id="1bc6c-156">Часто бывает полезным указать определенную среду для тестирования.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-156">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="1bc6c-157">Если среда не указана, по умолчанию используется среда `Production`, в которой большинство функций отладки отключено.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-157">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="1bc6c-158">Способ указания среды зависит от операционной системы.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-158">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="1bc6c-159">Служба приложений Azure</span><span class="sxs-lookup"><span data-stu-id="1bc6c-159">Azure App Service</span></span>

<span data-ttu-id="1bc6c-160">Чтобы установить среду в [службе приложений Azure](https://azure.microsoft.com/services/app-service/), выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="1bc6c-160">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="1bc6c-161">Выберите приложение из колонки **Службы приложений**.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-161">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="1bc6c-162">В группе **ПАРАМЕТРЫ** выберите колонку **Параметры приложения**.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-162">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="1bc6c-163">В области **Параметры приложения** выберите **Добавить новый параметр**.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-163">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="1bc6c-164">В поле **Введите имя** укажите `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-164">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="1bc6c-165">В поле **Введите значение** укажите среду (например, `Staging`).</span><span class="sxs-lookup"><span data-stu-id="1bc6c-165">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="1bc6c-166">Установите флажок **Параметр слота**, если нужно, чтобы параметр среды оставался в текущем слоте при замене слота развертывания.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-166">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="1bc6c-167">Дополнительные сведения см. в разделе [Документация по Azure. Какие параметры заменяются?](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="1bc6c-167">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="1bc6c-168">Нажмите **Сохранить** в верхней части колонки.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-168">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="1bc6c-169">Служба приложений Azure автоматически перезапускает приложение после добавления, изменения или удаления параметра приложения (переменной среды) на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-169">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

### <a name="windows"></a><span data-ttu-id="1bc6c-170">Windows</span><span class="sxs-lookup"><span data-stu-id="1bc6c-170">Windows</span></span>

<span data-ttu-id="1bc6c-171">Если приложение запускается с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run), то, чтобы задать переменную `ASPNETCORE_ENVIRONMENT` для текущего сеанса, используйте следующие команды:</span><span class="sxs-lookup"><span data-stu-id="1bc6c-171">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="1bc6c-172">**Командная строка**</span><span class="sxs-lookup"><span data-stu-id="1bc6c-172">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="1bc6c-173">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="1bc6c-173">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="1bc6c-174">Эти команды действуют только для текущего окна.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-174">These commands only take effect for the current window.</span></span> <span data-ttu-id="1bc6c-175">Когда окно закрывается, для параметра `ASPNETCORE_ENVIRONMENT` восстанавливается значение по умолчанию или значение, заданное на компьютере.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-175">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="1bc6c-176">Чтобы задать это значение в Windows на глобальном уровне, используйте один из следующих подходов.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-176">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="1bc6c-177">Откройте **Панель управления** > **Система** > **Дополнительные параметры системы** и добавьте или измените значение `ASPNETCORE_ENVIRONMENT`:</span><span class="sxs-lookup"><span data-stu-id="1bc6c-177">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Дополнительные параметры системы](environments/_static/systemsetting_environment.png)

  ![Переменная среды ASPNET Core](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="1bc6c-180">Откройте командную строку администратора и выполните команду `setx` либо откройте командную строку администратора PowerShell и используйте `[Environment]::SetEnvironmentVariable`:</span><span class="sxs-lookup"><span data-stu-id="1bc6c-180">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="1bc6c-181">**Командная строка**</span><span class="sxs-lookup"><span data-stu-id="1bc6c-181">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="1bc6c-182">Параметр `/M` указывает на установку переменной среды на уровне системы.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-182">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="1bc6c-183">Если параметр `/M` не используется, переменная среды задается для учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-183">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="1bc6c-184">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="1bc6c-184">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="1bc6c-185">Значение параметра `Machine` указывает на установку переменной среды на уровне системы.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-185">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="1bc6c-186">При изменении значения параметра на `User` переменная среды задается для учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-186">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="1bc6c-187">Если переменная среды `ASPNETCORE_ENVIRONMENT` задана глобально, она действует для `dotnet run` в любом окне командной строки, открываемом после установки значения.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-187">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="1bc6c-188">**web.config**</span><span class="sxs-lookup"><span data-stu-id="1bc6c-188">**web.config**</span></span>

<span data-ttu-id="1bc6c-189">Сведения об установке переменной среды `ASPNETCORE_ENVIRONMENT` в файле *web.config* см. в разделе *Настройка переменной среды* в <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-189">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span> <span data-ttu-id="1bc6c-190">Если переменная среды `ASPNETCORE_ENVIRONMENT` задана в файле *web.config*, ее значение переопределяет значение на уровне системы.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-190">When the `ASPNETCORE_ENVIRONMENT` environment variable is set with *web.config*, its value overrides a setting at the system level.</span></span>

<span data-ttu-id="1bc6c-191">**Пул приложений IIS**</span><span class="sxs-lookup"><span data-stu-id="1bc6c-191">**Per IIS Application Pool**</span></span>

<span data-ttu-id="1bc6c-192">Чтобы задать переменную среды `ASPNETCORE_ENVIRONMENT` для приложения, выполняющегося в изолированном пуле приложений (такая возможность поддерживается в службах IIS 10.0 и более поздних версий), см. подраздел, посвященный команде *AppCmd.exe*, в разделе [Переменные среды &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).</span><span class="sxs-lookup"><span data-stu-id="1bc6c-192">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="1bc6c-193">Если переменная среды `ASPNETCORE_ENVIRONMENT` задана для пула приложений, ее значение переопределяет значение на уровне системы.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-193">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1bc6c-194">При размещении приложения в службах IIS и добавлении или изменении переменной среды `ASPNETCORE_ENVIRONMENT` используйте один из следующих подходов по применению нового значения в приложении.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-194">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="1bc6c-195">Из командной строки выполните команду `net stop was /y`, за которой следует `net start w3svc`.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-195">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="1bc6c-196">Перезапустите сервер.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-196">Restart the server.</span></span>

### <a name="macos"></a><span data-ttu-id="1bc6c-197">macOS</span><span class="sxs-lookup"><span data-stu-id="1bc6c-197">macOS</span></span>

<span data-ttu-id="1bc6c-198">Задать текущую среду в macOS можно в командной строке при запуске приложения:</span><span class="sxs-lookup"><span data-stu-id="1bc6c-198">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="1bc6c-199">Также можно задать среду с помощью команды `export` до запуска приложения:</span><span class="sxs-lookup"><span data-stu-id="1bc6c-199">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="1bc6c-200">Переменные среды на уровне компьютера задаются в файле *BASHRC* или *BASH_PROFILE*.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-200">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="1bc6c-201">Измените файл в любом текстовом редакторе.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-201">Edit the file using any text editor.</span></span> <span data-ttu-id="1bc6c-202">Добавьте следующий оператор:</span><span class="sxs-lookup"><span data-stu-id="1bc6c-202">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="1bc6c-203">Linux</span><span class="sxs-lookup"><span data-stu-id="1bc6c-203">Linux</span></span>

<span data-ttu-id="1bc6c-204">В дистрибутивах Linux используйте команду `export` в командной строке для значений переменных на уровне сеанса или в файле *BASH_PROFILE* для значений среды на уровне компьютера.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-204">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="1bc6c-205">Конфигурация для разных сред</span><span class="sxs-lookup"><span data-stu-id="1bc6c-205">Configuration by environment</span></span>

<span data-ttu-id="1bc6c-206">Для загрузки конфигурации среды мы рекомендуем:</span><span class="sxs-lookup"><span data-stu-id="1bc6c-206">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="1bc6c-207">Файлы *appsettings* (\*appsettings.&lt;<Environment>&gt;.json).</span><span class="sxs-lookup"><span data-stu-id="1bc6c-207">*appsettings* files (\*appsettings.&lt;<Environment>&gt;.json).</span></span> <span data-ttu-id="1bc6c-208">См. раздел [Конфигурация: поставщик конфигурации файлов](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1bc6c-208">See [Configuration: File configuration provider](xref:fundamentals/configuration/index#file-configuration-provider).</span></span>
* <span data-ttu-id="1bc6c-209">Переменные среды (заданные в каждой системе, где размещено приложение).</span><span class="sxs-lookup"><span data-stu-id="1bc6c-209">environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="1bc6c-210">См. разделы [Конфигурация: поставщик конфигурации файлов](xref:fundamentals/configuration/index#file-configuration-provider) и [Безопасное хранение секретов приложения во время разработки: переменные среды](xref:security/app-secrets#environment-variables).</span><span class="sxs-lookup"><span data-stu-id="1bc6c-210">See [Configuration: File configuration provider](xref:fundamentals/configuration/index#file-configuration-provider) and [Safe storage of app secrets in development: Environment variables](xref:security/app-secrets#environment-variables).</span></span>
* <span data-ttu-id="1bc6c-211">Менеджер секретов (только в среде разработки).</span><span class="sxs-lookup"><span data-stu-id="1bc6c-211">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="1bc6c-212">См. раздел <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-212">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="1bc6c-213">Класс Startup и его методы для разных сред</span><span class="sxs-lookup"><span data-stu-id="1bc6c-213">Environment-based Startup class and methods</span></span>

### <a name="startup-class-conventions"></a><span data-ttu-id="1bc6c-214">Соглашения о классе Startup</span><span class="sxs-lookup"><span data-stu-id="1bc6c-214">Startup class conventions</span></span>

<span data-ttu-id="1bc6c-215">При запуске приложения ASP.NET Core [класс Startup](xref:fundamentals/startup) выполняет его начальную загрузку.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-215">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="1bc6c-216">Приложение может определять отдельные классы `Startup` для различных сред (например, `StartupDevelopment`), при этом подходящий класс `Startup` выбирается во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-216">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`), and the appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="1bc6c-217">Класс, у которого суффикс имени соответствует текущей среде, получает приоритет.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-217">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="1bc6c-218">Если соответствующий класс `Startup{EnvironmentName}` не найден, используется класс `Startup`.</span><span class="sxs-lookup"><span data-stu-id="1bc6c-218">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span>

<span data-ttu-id="1bc6c-219">Чтобы реализовать классы `Startup` на основе среды, создайте класс `Startup{EnvironmentName}` для каждой используемой среды и резервный класс `Startup`:</span><span class="sxs-lookup"><span data-stu-id="1bc6c-219">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}
```

<span data-ttu-id="1bc6c-220">Используйте перегрузку [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup), которая принимает имя сборки:</span><span class="sxs-lookup"><span data-stu-id="1bc6c-220">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

    return WebHost.CreateDefaultBuilder(args)
        .UseStartup(assemblyName);
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```csharp
public static void Main(string[] args)
{
    CreateWebHost(args).Run();
}

public static IWebHost CreateWebHost(string[] args)
{
    var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

    return WebHost.CreateDefaultBuilder(args)
        .UseStartup(assemblyName)
        .Build();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

        var host = new WebHostBuilder()
            .UseStartup(assemblyName)
            .Build();

        host.Run();
    }
}
```

::: moniker-end

### <a name="startup-method-conventions"></a><span data-ttu-id="1bc6c-221">Соглашения о методах Startup</span><span class="sxs-lookup"><span data-stu-id="1bc6c-221">Startup method conventions</span></span>

<span data-ttu-id="1bc6c-222">Методы [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) и [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) поддерживают версии для конкретных сред в формате `Configure<EnvironmentName>` и `Configure<EnvironmentName>Services`:</span><span class="sxs-lookup"><span data-stu-id="1bc6c-222">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`:</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,51)]

## <a name="additional-resources"></a><span data-ttu-id="1bc6c-223">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="1bc6c-223">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="1bc6c-224">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="1bc6c-224">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
