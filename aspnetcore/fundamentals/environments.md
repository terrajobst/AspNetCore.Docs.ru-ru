---
title: Использование нескольких сред в ASP.NET Core
author: rick-anderson
description: Сведения об управлении поведением приложений в разных средах в приложениях ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: fundamentals/environments
ms.openlocfilehash: affbb95273c91fe5bf452e0e1ebefa669297304c
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944325"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="18e8a-103">Использование нескольких сред в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="18e8a-103">Use multiple environments in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="18e8a-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="18e8a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="18e8a-105">ASP.NET Core настраивает поведение приложения в зависимости от среды выполнения с помощью переменной среды.</span><span class="sxs-lookup"><span data-stu-id="18e8a-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="18e8a-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="18e8a-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="18e8a-107">Среды</span><span class="sxs-lookup"><span data-stu-id="18e8a-107">Environments</span></span>

<span data-ttu-id="18e8a-108">ASP.NET Core считывает переменную среды `ASPNETCORE_ENVIRONMENT` при запуске приложения и сохраняет ее значение в свойстве [IWebHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="18e8a-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IWebHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span></span> <span data-ttu-id="18e8a-109">Переменной `ASPNETCORE_ENVIRONMENT` можно присвоить любое значение, но платформа предоставляет три значения:</span><span class="sxs-lookup"><span data-stu-id="18e8a-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.Extensions.Hosting.Environments.Development>
* <xref:Microsoft.Extensions.Hosting.Environments.Staging>
* <span data-ttu-id="18e8a-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="18e8a-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="18e8a-111">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="18e8a-111">The preceding code:</span></span>

* <span data-ttu-id="18e8a-112">Вызывает [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage), если `ASPNETCORE_ENVIRONMENT` имеет значение `Development`.</span><span class="sxs-lookup"><span data-stu-id="18e8a-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="18e8a-113">Вызывает [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler), если переменная `ASPNETCORE_ENVIRONMENT` имеет одно из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="18e8a-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="18e8a-114">[Вспомогательная функция тега среды](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) использует значение `IHostingEnvironment.EnvironmentName` для включения или исключения разметки в элементе:</span><span class="sxs-lookup"><span data-stu-id="18e8a-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="18e8a-115">В ОС Windows и macOS регистр символов в переменных среды и их значениях не учитывается.</span><span class="sxs-lookup"><span data-stu-id="18e8a-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="18e8a-116">В ОС Linux в переменных среды и их значениях **регистр символов по умолчанию учитывается**.</span><span class="sxs-lookup"><span data-stu-id="18e8a-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="18e8a-117">Разработка</span><span class="sxs-lookup"><span data-stu-id="18e8a-117">Development</span></span>

<span data-ttu-id="18e8a-118">В среде разработки могут быть включены функции, которые не должны быть доступны в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="18e8a-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="18e8a-119">Например, шаблоны ASP.NET Core включают в среде разработки [страницу со сведениями об исключении для разработчика](xref:fundamentals/error-handling#developer-exception-page).</span><span class="sxs-lookup"><span data-stu-id="18e8a-119">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="18e8a-120">Среду для локального компьютера разработки можно задать в файле *Properties\launchSettings.json* проекта.</span><span class="sxs-lookup"><span data-stu-id="18e8a-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="18e8a-121">Значения среды, заданные в файле *launchSettings.json*, переопределяют значения, заданные в системной среде.</span><span class="sxs-lookup"><span data-stu-id="18e8a-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="18e8a-122">В следующем коде JSON показаны три профиля из файла *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="18e8a-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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

> [!NOTE]
> <span data-ttu-id="18e8a-123">Свойство `applicationUrl` в *launchSettings.json* может задать список URL-адресов сервера.</span><span class="sxs-lookup"><span data-stu-id="18e8a-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="18e8a-124">Для разделения URL-адресов в списке используется точка с запятой:</span><span class="sxs-lookup"><span data-stu-id="18e8a-124">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="18e8a-125">Когда приложение запускается с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run), используется первый профиль с атрибутом `"commandName": "Project"`.</span><span class="sxs-lookup"><span data-stu-id="18e8a-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="18e8a-126">Значение `commandName` определяет запускаемый веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="18e8a-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="18e8a-127">`commandName` может иметь одно из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="18e8a-127">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="18e8a-128">`Project` (запускается Kestrel)</span><span class="sxs-lookup"><span data-stu-id="18e8a-128">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="18e8a-129">Когда приложение запускается с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run), выполняются указанные ниже действия:</span><span class="sxs-lookup"><span data-stu-id="18e8a-129">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="18e8a-130">Считывается файл *launchSettings.json*, если он доступен.</span><span class="sxs-lookup"><span data-stu-id="18e8a-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="18e8a-131">Параметры `environmentVariables` в файле *launchSettings.json* переопределяют переменные среды.</span><span class="sxs-lookup"><span data-stu-id="18e8a-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="18e8a-132">Отображается среда размещения.</span><span class="sxs-lookup"><span data-stu-id="18e8a-132">The hosting environment is displayed.</span></span>

<span data-ttu-id="18e8a-133">Ниже представлены выходные данные приложения, запущенного с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="18e8a-133">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="18e8a-134">Вкладка **Отладка** в свойствах проекта Visual Studio предоставляет графический пользовательский интерфейс для изменения файла *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="18e8a-134">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Задание переменных среды в свойствах проекта](environments/_static/project-properties-debug.png)

<span data-ttu-id="18e8a-136">Для вступления в силу изменений, внесенных в профили проекта, может потребоваться перезапуск веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="18e8a-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="18e8a-137">Чтобы сервер Kestrel обнаружил изменения, внесенные в среду, его необходимо перезапустить.</span><span class="sxs-lookup"><span data-stu-id="18e8a-137">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="18e8a-138">В файле *launchSettings.json* не должны храниться секреты.</span><span class="sxs-lookup"><span data-stu-id="18e8a-138">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="18e8a-139">Для хранения секретов во время разработки в локальной среде можно использовать [средство Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="18e8a-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="18e8a-140">При использовании [Visual Studio Code](https://code.visualstudio.com/) переменные среды можно задавать в файле *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="18e8a-140">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="18e8a-141">В следующем примере показано, как присвоить среде значение `Development`:</span><span class="sxs-lookup"><span data-stu-id="18e8a-141">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="18e8a-142">Файл *.vscode/launch.json* в проекте не читается при запуске приложения с помощью команды `dotnet run` так же, как файл *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="18e8a-142">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="18e8a-143">При запуске приложения в среде разработки без файла *launchSettings.json* укажите среду с помощью переменной среды или задайте аргумент командной строки как команду `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="18e8a-143">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="18e8a-144">Рабочие</span><span class="sxs-lookup"><span data-stu-id="18e8a-144">Production</span></span>

<span data-ttu-id="18e8a-145">Конфигурация рабочей среды должна обеспечивать максимальный уровень безопасности, производительности и надежности приложений.</span><span class="sxs-lookup"><span data-stu-id="18e8a-145">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="18e8a-146">Некоторые общие параметры, отличные от разработки:</span><span class="sxs-lookup"><span data-stu-id="18e8a-146">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="18e8a-147">кэширование;</span><span class="sxs-lookup"><span data-stu-id="18e8a-147">Caching.</span></span>
* <span data-ttu-id="18e8a-148">ресурсы на стороне клиента объединяются в пакеты, уплотняются и могут предоставляться из сети CDN;</span><span class="sxs-lookup"><span data-stu-id="18e8a-148">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="18e8a-149">страницы с сообщениями об ошибках диагностики отключены;</span><span class="sxs-lookup"><span data-stu-id="18e8a-149">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="18e8a-150">включены страницы с понятными пользователям сообщениями об ошибках;</span><span class="sxs-lookup"><span data-stu-id="18e8a-150">Friendly error pages enabled.</span></span>
* <span data-ttu-id="18e8a-151">включены средства ведения журналов и мониторинга в рабочей среде,</span><span class="sxs-lookup"><span data-stu-id="18e8a-151">Production logging and monitoring enabled.</span></span> <span data-ttu-id="18e8a-152">например [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="18e8a-152">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="18e8a-153">Указание среды</span><span class="sxs-lookup"><span data-stu-id="18e8a-153">Set the environment</span></span>

<span data-ttu-id="18e8a-154">Часто бывает полезным указать определенную среду для тестирования с переменной среды или параметром платформы.</span><span class="sxs-lookup"><span data-stu-id="18e8a-154">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="18e8a-155">Если среда не указана, по умолчанию используется среда `Production`, в которой большинство функций отладки отключено.</span><span class="sxs-lookup"><span data-stu-id="18e8a-155">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="18e8a-156">Способ указания среды зависит от операционной системы.</span><span class="sxs-lookup"><span data-stu-id="18e8a-156">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="18e8a-157">При создании узла среду приложения определяет последний параметр среды, считанный приложением.</span><span class="sxs-lookup"><span data-stu-id="18e8a-157">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="18e8a-158">Среду приложения невозможно изменить во время его выполнения.</span><span class="sxs-lookup"><span data-stu-id="18e8a-158">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="18e8a-159">Переменная среды или параметр платформы</span><span class="sxs-lookup"><span data-stu-id="18e8a-159">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="18e8a-160">Служба приложений Azure</span><span class="sxs-lookup"><span data-stu-id="18e8a-160">Azure App Service</span></span>

<span data-ttu-id="18e8a-161">Чтобы установить среду в [службе приложений Azure](https://azure.microsoft.com/services/app-service/), выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="18e8a-161">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="18e8a-162">Выберите приложение из колонки **Службы приложений**.</span><span class="sxs-lookup"><span data-stu-id="18e8a-162">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="18e8a-163">В группе **ПАРАМЕТРЫ** выберите колонку **Параметры приложения**.</span><span class="sxs-lookup"><span data-stu-id="18e8a-163">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="18e8a-164">В области **Параметры приложения** выберите **Добавить новый параметр**.</span><span class="sxs-lookup"><span data-stu-id="18e8a-164">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="18e8a-165">В поле **Введите имя** укажите `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="18e8a-165">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="18e8a-166">В поле **Введите значение** укажите среду (например, `Staging`).</span><span class="sxs-lookup"><span data-stu-id="18e8a-166">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="18e8a-167">Установите флажок **Параметр слота**, если нужно, чтобы параметр среды оставался в текущем слоте при замене слота развертывания.</span><span class="sxs-lookup"><span data-stu-id="18e8a-167">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="18e8a-168">Дополнительные сведения см. в разделе о [переносимых параметрах](/azure/app-service/web-sites-staged-publishing) документации по Azure.</span><span class="sxs-lookup"><span data-stu-id="18e8a-168">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="18e8a-169">Нажмите **Сохранить** в верхней части колонки.</span><span class="sxs-lookup"><span data-stu-id="18e8a-169">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="18e8a-170">Служба приложений Azure автоматически перезапускает приложение после добавления, изменения или удаления параметра приложения (переменной среды) на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="18e8a-170">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="18e8a-171">Windows</span><span class="sxs-lookup"><span data-stu-id="18e8a-171">Windows</span></span>

<span data-ttu-id="18e8a-172">Если приложение запускается с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run), то, чтобы задать переменную `ASPNETCORE_ENVIRONMENT` для текущего сеанса, используйте следующие команды:</span><span class="sxs-lookup"><span data-stu-id="18e8a-172">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="18e8a-173">**Командная строка**</span><span class="sxs-lookup"><span data-stu-id="18e8a-173">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="18e8a-174">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="18e8a-174">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="18e8a-175">Эти команды действуют только для текущего окна.</span><span class="sxs-lookup"><span data-stu-id="18e8a-175">These commands only take effect for the current window.</span></span> <span data-ttu-id="18e8a-176">Когда окно закрывается, для параметра `ASPNETCORE_ENVIRONMENT` восстанавливается значение по умолчанию или значение, заданное на компьютере.</span><span class="sxs-lookup"><span data-stu-id="18e8a-176">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="18e8a-177">Чтобы задать это значение в Windows на глобальном уровне, используйте один из следующих подходов.</span><span class="sxs-lookup"><span data-stu-id="18e8a-177">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="18e8a-178">Откройте **Панель управления** > **Система** > **Дополнительные параметры системы** и добавьте или измените значение `ASPNETCORE_ENVIRONMENT`:</span><span class="sxs-lookup"><span data-stu-id="18e8a-178">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Дополнительные параметры системы](environments/_static/systemsetting_environment.png)

  ![Переменная среды ASPNET Core](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="18e8a-181">Откройте командную строку администратора и выполните команду `setx` либо откройте командную строку администратора PowerShell и используйте `[Environment]::SetEnvironmentVariable`:</span><span class="sxs-lookup"><span data-stu-id="18e8a-181">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="18e8a-182">**Командная строка**</span><span class="sxs-lookup"><span data-stu-id="18e8a-182">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="18e8a-183">Параметр `/M` указывает на установку переменной среды на уровне системы.</span><span class="sxs-lookup"><span data-stu-id="18e8a-183">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="18e8a-184">Если параметр `/M` не используется, переменная среды задается для учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="18e8a-184">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="18e8a-185">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="18e8a-185">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="18e8a-186">Значение параметра `Machine` указывает на установку переменной среды на уровне системы.</span><span class="sxs-lookup"><span data-stu-id="18e8a-186">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="18e8a-187">При изменении значения параметра на `User` переменная среды задается для учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="18e8a-187">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="18e8a-188">Если переменная среды `ASPNETCORE_ENVIRONMENT` задана глобально, она действует для `dotnet run` в любом окне командной строки, открываемом после установки значения.</span><span class="sxs-lookup"><span data-stu-id="18e8a-188">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="18e8a-189">**web.config**</span><span class="sxs-lookup"><span data-stu-id="18e8a-189">**web.config**</span></span>

<span data-ttu-id="18e8a-190">Сведения об установке переменной среды `ASPNETCORE_ENVIRONMENT` в файле *web.config* см. в разделе *Настройка переменной среды* в <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="18e8a-190">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="18e8a-191">**Файл проекта или профиль публикации**</span><span class="sxs-lookup"><span data-stu-id="18e8a-191">**Project file or publish profile**</span></span>

<span data-ttu-id="18e8a-192">**Для развертываний Windows IIS:** Включите свойство `<EnvironmentName>` в [профиле публикации (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) или файле проекта.</span><span class="sxs-lookup"><span data-stu-id="18e8a-192">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="18e8a-193">При этом подходе во время публикации проекта среда задается в файле *web.config*:</span><span class="sxs-lookup"><span data-stu-id="18e8a-193">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="18e8a-194">**Пул приложений IIS**</span><span class="sxs-lookup"><span data-stu-id="18e8a-194">**Per IIS Application Pool**</span></span>

<span data-ttu-id="18e8a-195">Чтобы задать переменную среды `ASPNETCORE_ENVIRONMENT` для приложения, выполняющегося в изолированном пуле приложений (такая возможность поддерживается в службах IIS 10.0 и более поздних версий), см. подраздел, посвященный команде *AppCmd.exe*, в разделе [Переменные среды &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).</span><span class="sxs-lookup"><span data-stu-id="18e8a-195">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="18e8a-196">Если переменная среды `ASPNETCORE_ENVIRONMENT` задана для пула приложений, ее значение переопределяет значение на уровне системы.</span><span class="sxs-lookup"><span data-stu-id="18e8a-196">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="18e8a-197">При размещении приложения в службах IIS и добавлении или изменении переменной среды `ASPNETCORE_ENVIRONMENT` используйте один из следующих подходов по применению нового значения в приложении.</span><span class="sxs-lookup"><span data-stu-id="18e8a-197">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="18e8a-198">Из командной строки выполните команду `net stop was /y`, за которой следует `net start w3svc`.</span><span class="sxs-lookup"><span data-stu-id="18e8a-198">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="18e8a-199">Перезапустите сервер.</span><span class="sxs-lookup"><span data-stu-id="18e8a-199">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="18e8a-200">macOS</span><span class="sxs-lookup"><span data-stu-id="18e8a-200">macOS</span></span>

<span data-ttu-id="18e8a-201">Задать текущую среду в macOS можно в командной строке при запуске приложения:</span><span class="sxs-lookup"><span data-stu-id="18e8a-201">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="18e8a-202">Также можно задать среду с помощью команды `export` до запуска приложения:</span><span class="sxs-lookup"><span data-stu-id="18e8a-202">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="18e8a-203">Переменные среды на уровне компьютера задаются в файле *BASHRC* или *BASH_PROFILE*.</span><span class="sxs-lookup"><span data-stu-id="18e8a-203">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="18e8a-204">Измените файл в любом текстовом редакторе.</span><span class="sxs-lookup"><span data-stu-id="18e8a-204">Edit the file using any text editor.</span></span> <span data-ttu-id="18e8a-205">Добавьте следующий оператор:</span><span class="sxs-lookup"><span data-stu-id="18e8a-205">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="18e8a-206">Linux</span><span class="sxs-lookup"><span data-stu-id="18e8a-206">Linux</span></span>

<span data-ttu-id="18e8a-207">В дистрибутивах Linux используйте команду `export` в командной строке для значений переменных на уровне сеанса или в файле *BASH_PROFILE* для значений среды на уровне компьютера.</span><span class="sxs-lookup"><span data-stu-id="18e8a-207">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="18e8a-208">Указание среды в коде</span><span class="sxs-lookup"><span data-stu-id="18e8a-208">Set the environment in code</span></span>

<span data-ttu-id="18e8a-209">Вызовите <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> при создании узла.</span><span class="sxs-lookup"><span data-stu-id="18e8a-209">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="18e8a-210">См. раздел <xref:fundamentals/host/generic-host#environmentname>.</span><span class="sxs-lookup"><span data-stu-id="18e8a-210">See <xref:fundamentals/host/generic-host#environmentname>.</span></span>


### <a name="configuration-by-environment"></a><span data-ttu-id="18e8a-211">Конфигурация для разных сред</span><span class="sxs-lookup"><span data-stu-id="18e8a-211">Configuration by environment</span></span>

<span data-ttu-id="18e8a-212">Для загрузки конфигурации среды мы рекомендуем:</span><span class="sxs-lookup"><span data-stu-id="18e8a-212">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="18e8a-213">Файлы *appsettings* (*appsettings.{Environment}.json*).</span><span class="sxs-lookup"><span data-stu-id="18e8a-213">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="18e8a-214">См. раздел <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="18e8a-214">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="18e8a-215">Переменные среды (заданные в каждой системе, где размещено приложение).</span><span class="sxs-lookup"><span data-stu-id="18e8a-215">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="18e8a-216">См. разделы <xref:fundamentals/host/generic-host#environmentname> и <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="18e8a-216">See <xref:fundamentals/host/generic-host#environmentname> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="18e8a-217">Менеджер секретов (только в среде разработки).</span><span class="sxs-lookup"><span data-stu-id="18e8a-217">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="18e8a-218">См. раздел <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="18e8a-218">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="18e8a-219">Класс Startup и его методы для разных сред</span><span class="sxs-lookup"><span data-stu-id="18e8a-219">Environment-based Startup class and methods</span></span>

### <a name="inject-iwebhostenvironment-into-startupconfigure"></a><span data-ttu-id="18e8a-220">Внедрение IWebHostEnvironment в Startup.Configure</span><span class="sxs-lookup"><span data-stu-id="18e8a-220">Inject IWebHostEnvironment into Startup.Configure</span></span>

<span data-ttu-id="18e8a-221">Внедрите <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="18e8a-221">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="18e8a-222">Этот подход удобен, когда для приложения требуется просто скорректировать `Startup.Configure` для нескольких сред с минимальными различиями в коде для каждой среды.</span><span class="sxs-lookup"><span data-stu-id="18e8a-222">This approach is useful when the app only requires adjusting `Startup.Configure` for a few environments with minimal code differences per environment.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Development environment code
    }
    else
    {
        // Code for all other environments
    }
}
```

### <a name="inject-iwebhostenvironment-into-the-startup-class"></a><span data-ttu-id="18e8a-223">Внедрение IWebHostEnvironment в класс Startup</span><span class="sxs-lookup"><span data-stu-id="18e8a-223">Inject IWebHostEnvironment into the Startup class</span></span>

<span data-ttu-id="18e8a-224">Внедрите <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> в конструктор `Startup`.</span><span class="sxs-lookup"><span data-stu-id="18e8a-224">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into the `Startup` constructor.</span></span> <span data-ttu-id="18e8a-225">Этот подход удобен, когда для приложения требуется настроить `Startup` всего для нескольких сред с минимальными различиями в коде для каждой среды.</span><span class="sxs-lookup"><span data-stu-id="18e8a-225">This approach is useful when the app requires configuring `Startup` for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="18e8a-226">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="18e8a-226">In the following example:</span></span>

* <span data-ttu-id="18e8a-227">Среда хранится в поле `_env`.</span><span class="sxs-lookup"><span data-stu-id="18e8a-227">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="18e8a-228">`_env` используется в `ConfigureServices` и `Configure` для применения конфигурации запуска на основе среды приложения.</span><span class="sxs-lookup"><span data-stu-id="18e8a-228">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

```csharp
public class Startup
{
    private readonly IWebHostEnvironment _env;

    public Startup(IWebHostEnvironment env)
    {
        _env = env;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else if (_env.IsStaging())
        {
            // Staging environment code
        }
        else
        {
            // Code for all other environments
        }
    }

    public void Configure(IApplicationBuilder app)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else
        {
            // Code for all other environments
        }
    }
}
```
### <a name="startup-class-conventions"></a><span data-ttu-id="18e8a-229">Соглашения о классе Startup</span><span class="sxs-lookup"><span data-stu-id="18e8a-229">Startup class conventions</span></span>

<span data-ttu-id="18e8a-230">При запуске приложения ASP.NET Core [класс Startup](xref:fundamentals/startup) выполняет его начальную загрузку.</span><span class="sxs-lookup"><span data-stu-id="18e8a-230">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="18e8a-231">Приложение может определять отдельные классы `Startup` для различных сред (например, `StartupDevelopment`).</span><span class="sxs-lookup"><span data-stu-id="18e8a-231">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="18e8a-232">Подходящий класс `Startup` выбирается во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="18e8a-232">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="18e8a-233">Класс, у которого суффикс имени соответствует текущей среде, получает приоритет.</span><span class="sxs-lookup"><span data-stu-id="18e8a-233">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="18e8a-234">Если соответствующий класс `Startup{EnvironmentName}` не найден, используется класс `Startup`.</span><span class="sxs-lookup"><span data-stu-id="18e8a-234">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="18e8a-235">Этот подход удобен, когда для приложения требуется настроить запуск для нескольких сред с многочисленными различиями в коде для каждой среды.</span><span class="sxs-lookup"><span data-stu-id="18e8a-235">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="18e8a-236">Чтобы реализовать классы `Startup` на основе среды, создайте класс `Startup{EnvironmentName}` для каждой используемой среды и резервный класс `Startup`:</span><span class="sxs-lookup"><span data-stu-id="18e8a-236">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}
```

<span data-ttu-id="18e8a-237">Используйте перегрузку [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup), которая принимает имя сборки:</span><span class="sxs-lookup"><span data-stu-id="18e8a-237">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

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

### <a name="startup-method-conventions"></a><span data-ttu-id="18e8a-238">Соглашения о методах Startup</span><span class="sxs-lookup"><span data-stu-id="18e8a-238">Startup method conventions</span></span>

<span data-ttu-id="18e8a-239">Методы [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) и [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) поддерживают версии для конкретных сред в формате `Configure<EnvironmentName>` и `Configure<EnvironmentName>Services`.</span><span class="sxs-lookup"><span data-stu-id="18e8a-239">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="18e8a-240">Этот подход удобен, когда для приложения требуется настроить запуск для нескольких сред с многочисленными различиями в коде для каждой среды.</span><span class="sxs-lookup"><span data-stu-id="18e8a-240">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="18e8a-241">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="18e8a-241">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="18e8a-242">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="18e8a-242">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="18e8a-243">ASP.NET Core настраивает поведение приложения в зависимости от среды выполнения с помощью переменной среды.</span><span class="sxs-lookup"><span data-stu-id="18e8a-243">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="18e8a-244">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="18e8a-244">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="18e8a-245">Среды</span><span class="sxs-lookup"><span data-stu-id="18e8a-245">Environments</span></span>

<span data-ttu-id="18e8a-246">ASP.NET Core считывает переменную среды `ASPNETCORE_ENVIRONMENT` при запуске приложения и сохраняет ее значение в свойстве [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="18e8a-246">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span></span> <span data-ttu-id="18e8a-247">Переменной `ASPNETCORE_ENVIRONMENT` можно присвоить любое значение, но платформа предоставляет три значения:</span><span class="sxs-lookup"><span data-stu-id="18e8a-247">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Staging>
* <span data-ttu-id="18e8a-248"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="18e8a-248"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="18e8a-249">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="18e8a-249">The preceding code:</span></span>

* <span data-ttu-id="18e8a-250">Вызывает [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage), если `ASPNETCORE_ENVIRONMENT` имеет значение `Development`.</span><span class="sxs-lookup"><span data-stu-id="18e8a-250">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="18e8a-251">Вызывает [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler), если переменная `ASPNETCORE_ENVIRONMENT` имеет одно из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="18e8a-251">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="18e8a-252">[Вспомогательная функция тега среды](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) использует значение `IHostingEnvironment.EnvironmentName` для включения или исключения разметки в элементе:</span><span class="sxs-lookup"><span data-stu-id="18e8a-252">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="18e8a-253">В ОС Windows и macOS регистр символов в переменных среды и их значениях не учитывается.</span><span class="sxs-lookup"><span data-stu-id="18e8a-253">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="18e8a-254">В ОС Linux в переменных среды и их значениях **регистр символов по умолчанию учитывается**.</span><span class="sxs-lookup"><span data-stu-id="18e8a-254">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="18e8a-255">Разработка</span><span class="sxs-lookup"><span data-stu-id="18e8a-255">Development</span></span>

<span data-ttu-id="18e8a-256">В среде разработки могут быть включены функции, которые не должны быть доступны в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="18e8a-256">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="18e8a-257">Например, шаблоны ASP.NET Core включают в среде разработки [страницу со сведениями об исключении для разработчика](xref:fundamentals/error-handling#developer-exception-page).</span><span class="sxs-lookup"><span data-stu-id="18e8a-257">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="18e8a-258">Среду для локального компьютера разработки можно задать в файле *Properties\launchSettings.json* проекта.</span><span class="sxs-lookup"><span data-stu-id="18e8a-258">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="18e8a-259">Значения среды, заданные в файле *launchSettings.json*, переопределяют значения, заданные в системной среде.</span><span class="sxs-lookup"><span data-stu-id="18e8a-259">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="18e8a-260">В следующем коде JSON показаны три профиля из файла *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="18e8a-260">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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

> [!NOTE]
> <span data-ttu-id="18e8a-261">Свойство `applicationUrl` в *launchSettings.json* может задать список URL-адресов сервера.</span><span class="sxs-lookup"><span data-stu-id="18e8a-261">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="18e8a-262">Для разделения URL-адресов в списке используется точка с запятой:</span><span class="sxs-lookup"><span data-stu-id="18e8a-262">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="18e8a-263">Когда приложение запускается с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run), используется первый профиль с атрибутом `"commandName": "Project"`.</span><span class="sxs-lookup"><span data-stu-id="18e8a-263">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="18e8a-264">Значение `commandName` определяет запускаемый веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="18e8a-264">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="18e8a-265">`commandName` может иметь одно из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="18e8a-265">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="18e8a-266">`Project` (запускается Kestrel)</span><span class="sxs-lookup"><span data-stu-id="18e8a-266">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="18e8a-267">Когда приложение запускается с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run), выполняются указанные ниже действия:</span><span class="sxs-lookup"><span data-stu-id="18e8a-267">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="18e8a-268">Считывается файл *launchSettings.json*, если он доступен.</span><span class="sxs-lookup"><span data-stu-id="18e8a-268">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="18e8a-269">Параметры `environmentVariables` в файле *launchSettings.json* переопределяют переменные среды.</span><span class="sxs-lookup"><span data-stu-id="18e8a-269">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="18e8a-270">Отображается среда размещения.</span><span class="sxs-lookup"><span data-stu-id="18e8a-270">The hosting environment is displayed.</span></span>

<span data-ttu-id="18e8a-271">Ниже представлены выходные данные приложения, запущенного с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="18e8a-271">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="18e8a-272">Вкладка **Отладка** в свойствах проекта Visual Studio предоставляет графический пользовательский интерфейс для изменения файла *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="18e8a-272">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Задание переменных среды в свойствах проекта](environments/_static/project-properties-debug.png)

<span data-ttu-id="18e8a-274">Для вступления в силу изменений, внесенных в профили проекта, может потребоваться перезапуск веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="18e8a-274">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="18e8a-275">Чтобы сервер Kestrel обнаружил изменения, внесенные в среду, его необходимо перезапустить.</span><span class="sxs-lookup"><span data-stu-id="18e8a-275">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="18e8a-276">В файле *launchSettings.json* не должны храниться секреты.</span><span class="sxs-lookup"><span data-stu-id="18e8a-276">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="18e8a-277">Для хранения секретов во время разработки в локальной среде можно использовать [средство Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="18e8a-277">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="18e8a-278">При использовании [Visual Studio Code](https://code.visualstudio.com/) переменные среды можно задавать в файле *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="18e8a-278">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="18e8a-279">В следующем примере показано, как присвоить среде значение `Development`:</span><span class="sxs-lookup"><span data-stu-id="18e8a-279">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="18e8a-280">Файл *.vscode/launch.json* в проекте не читается при запуске приложения с помощью команды `dotnet run` так же, как файл *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="18e8a-280">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="18e8a-281">При запуске приложения в среде разработки без файла *launchSettings.json* укажите среду с помощью переменной среды или задайте аргумент командной строки как команду `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="18e8a-281">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="18e8a-282">Рабочие</span><span class="sxs-lookup"><span data-stu-id="18e8a-282">Production</span></span>

<span data-ttu-id="18e8a-283">Конфигурация рабочей среды должна обеспечивать максимальный уровень безопасности, производительности и надежности приложений.</span><span class="sxs-lookup"><span data-stu-id="18e8a-283">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="18e8a-284">Некоторые общие параметры, отличные от разработки:</span><span class="sxs-lookup"><span data-stu-id="18e8a-284">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="18e8a-285">кэширование;</span><span class="sxs-lookup"><span data-stu-id="18e8a-285">Caching.</span></span>
* <span data-ttu-id="18e8a-286">ресурсы на стороне клиента объединяются в пакеты, уплотняются и могут предоставляться из сети CDN;</span><span class="sxs-lookup"><span data-stu-id="18e8a-286">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="18e8a-287">страницы с сообщениями об ошибках диагностики отключены;</span><span class="sxs-lookup"><span data-stu-id="18e8a-287">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="18e8a-288">включены страницы с понятными пользователям сообщениями об ошибках;</span><span class="sxs-lookup"><span data-stu-id="18e8a-288">Friendly error pages enabled.</span></span>
* <span data-ttu-id="18e8a-289">включены средства ведения журналов и мониторинга в рабочей среде,</span><span class="sxs-lookup"><span data-stu-id="18e8a-289">Production logging and monitoring enabled.</span></span> <span data-ttu-id="18e8a-290">например [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="18e8a-290">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="18e8a-291">Указание среды</span><span class="sxs-lookup"><span data-stu-id="18e8a-291">Set the environment</span></span>

<span data-ttu-id="18e8a-292">Часто бывает полезным указать определенную среду для тестирования с переменной среды или параметром платформы.</span><span class="sxs-lookup"><span data-stu-id="18e8a-292">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="18e8a-293">Если среда не указана, по умолчанию используется среда `Production`, в которой большинство функций отладки отключено.</span><span class="sxs-lookup"><span data-stu-id="18e8a-293">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="18e8a-294">Способ указания среды зависит от операционной системы.</span><span class="sxs-lookup"><span data-stu-id="18e8a-294">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="18e8a-295">При создании узла среду приложения определяет последний параметр среды, считанный приложением.</span><span class="sxs-lookup"><span data-stu-id="18e8a-295">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="18e8a-296">Среду приложения невозможно изменить во время его выполнения.</span><span class="sxs-lookup"><span data-stu-id="18e8a-296">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="18e8a-297">Переменная среды или параметр платформы</span><span class="sxs-lookup"><span data-stu-id="18e8a-297">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="18e8a-298">Служба приложений Azure</span><span class="sxs-lookup"><span data-stu-id="18e8a-298">Azure App Service</span></span>

<span data-ttu-id="18e8a-299">Чтобы установить среду в [службе приложений Azure](https://azure.microsoft.com/services/app-service/), выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="18e8a-299">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="18e8a-300">Выберите приложение из колонки **Службы приложений**.</span><span class="sxs-lookup"><span data-stu-id="18e8a-300">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="18e8a-301">В группе **ПАРАМЕТРЫ** выберите колонку **Параметры приложения**.</span><span class="sxs-lookup"><span data-stu-id="18e8a-301">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="18e8a-302">В области **Параметры приложения** выберите **Добавить новый параметр**.</span><span class="sxs-lookup"><span data-stu-id="18e8a-302">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="18e8a-303">В поле **Введите имя** укажите `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="18e8a-303">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="18e8a-304">В поле **Введите значение** укажите среду (например, `Staging`).</span><span class="sxs-lookup"><span data-stu-id="18e8a-304">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="18e8a-305">Установите флажок **Параметр слота**, если нужно, чтобы параметр среды оставался в текущем слоте при замене слота развертывания.</span><span class="sxs-lookup"><span data-stu-id="18e8a-305">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="18e8a-306">Дополнительные сведения см. в разделе о [переносимых параметрах](/azure/app-service/web-sites-staged-publishing) документации по Azure.</span><span class="sxs-lookup"><span data-stu-id="18e8a-306">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="18e8a-307">Нажмите **Сохранить** в верхней части колонки.</span><span class="sxs-lookup"><span data-stu-id="18e8a-307">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="18e8a-308">Служба приложений Azure автоматически перезапускает приложение после добавления, изменения или удаления параметра приложения (переменной среды) на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="18e8a-308">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="18e8a-309">Windows</span><span class="sxs-lookup"><span data-stu-id="18e8a-309">Windows</span></span>

<span data-ttu-id="18e8a-310">Если приложение запускается с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run), то, чтобы задать переменную `ASPNETCORE_ENVIRONMENT` для текущего сеанса, используйте следующие команды:</span><span class="sxs-lookup"><span data-stu-id="18e8a-310">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="18e8a-311">**Командная строка**</span><span class="sxs-lookup"><span data-stu-id="18e8a-311">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="18e8a-312">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="18e8a-312">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="18e8a-313">Эти команды действуют только для текущего окна.</span><span class="sxs-lookup"><span data-stu-id="18e8a-313">These commands only take effect for the current window.</span></span> <span data-ttu-id="18e8a-314">Когда окно закрывается, для параметра `ASPNETCORE_ENVIRONMENT` восстанавливается значение по умолчанию или значение, заданное на компьютере.</span><span class="sxs-lookup"><span data-stu-id="18e8a-314">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="18e8a-315">Чтобы задать это значение в Windows на глобальном уровне, используйте один из следующих подходов.</span><span class="sxs-lookup"><span data-stu-id="18e8a-315">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="18e8a-316">Откройте **Панель управления** > **Система** > **Дополнительные параметры системы** и добавьте или измените значение `ASPNETCORE_ENVIRONMENT`:</span><span class="sxs-lookup"><span data-stu-id="18e8a-316">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Дополнительные параметры системы](environments/_static/systemsetting_environment.png)

  ![Переменная среды ASPNET Core](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="18e8a-319">Откройте командную строку администратора и выполните команду `setx` либо откройте командную строку администратора PowerShell и используйте `[Environment]::SetEnvironmentVariable`:</span><span class="sxs-lookup"><span data-stu-id="18e8a-319">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="18e8a-320">**Командная строка**</span><span class="sxs-lookup"><span data-stu-id="18e8a-320">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="18e8a-321">Параметр `/M` указывает на установку переменной среды на уровне системы.</span><span class="sxs-lookup"><span data-stu-id="18e8a-321">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="18e8a-322">Если параметр `/M` не используется, переменная среды задается для учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="18e8a-322">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="18e8a-323">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="18e8a-323">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="18e8a-324">Значение параметра `Machine` указывает на установку переменной среды на уровне системы.</span><span class="sxs-lookup"><span data-stu-id="18e8a-324">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="18e8a-325">При изменении значения параметра на `User` переменная среды задается для учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="18e8a-325">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="18e8a-326">Если переменная среды `ASPNETCORE_ENVIRONMENT` задана глобально, она действует для `dotnet run` в любом окне командной строки, открываемом после установки значения.</span><span class="sxs-lookup"><span data-stu-id="18e8a-326">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="18e8a-327">**web.config**</span><span class="sxs-lookup"><span data-stu-id="18e8a-327">**web.config**</span></span>

<span data-ttu-id="18e8a-328">Сведения об установке переменной среды `ASPNETCORE_ENVIRONMENT` в файле *web.config* см. в разделе *Настройка переменной среды* в <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="18e8a-328">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="18e8a-329">**Файл проекта или профиль публикации**</span><span class="sxs-lookup"><span data-stu-id="18e8a-329">**Project file or publish profile**</span></span>

<span data-ttu-id="18e8a-330">**Для развертываний Windows IIS:** Включите свойство `<EnvironmentName>` в [профиле публикации (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) или файле проекта.</span><span class="sxs-lookup"><span data-stu-id="18e8a-330">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="18e8a-331">При этом подходе во время публикации проекта среда задается в файле *web.config*:</span><span class="sxs-lookup"><span data-stu-id="18e8a-331">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="18e8a-332">**Пул приложений IIS**</span><span class="sxs-lookup"><span data-stu-id="18e8a-332">**Per IIS Application Pool**</span></span>

<span data-ttu-id="18e8a-333">Чтобы задать переменную среды `ASPNETCORE_ENVIRONMENT` для приложения, выполняющегося в изолированном пуле приложений (такая возможность поддерживается в службах IIS 10.0 и более поздних версий), см. подраздел, посвященный команде *AppCmd.exe*, в разделе [Переменные среды &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).</span><span class="sxs-lookup"><span data-stu-id="18e8a-333">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="18e8a-334">Если переменная среды `ASPNETCORE_ENVIRONMENT` задана для пула приложений, ее значение переопределяет значение на уровне системы.</span><span class="sxs-lookup"><span data-stu-id="18e8a-334">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="18e8a-335">При размещении приложения в службах IIS и добавлении или изменении переменной среды `ASPNETCORE_ENVIRONMENT` используйте один из следующих подходов по применению нового значения в приложении.</span><span class="sxs-lookup"><span data-stu-id="18e8a-335">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="18e8a-336">Из командной строки выполните команду `net stop was /y`, за которой следует `net start w3svc`.</span><span class="sxs-lookup"><span data-stu-id="18e8a-336">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="18e8a-337">Перезапустите сервер.</span><span class="sxs-lookup"><span data-stu-id="18e8a-337">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="18e8a-338">macOS</span><span class="sxs-lookup"><span data-stu-id="18e8a-338">macOS</span></span>

<span data-ttu-id="18e8a-339">Задать текущую среду в macOS можно в командной строке при запуске приложения:</span><span class="sxs-lookup"><span data-stu-id="18e8a-339">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="18e8a-340">Также можно задать среду с помощью команды `export` до запуска приложения:</span><span class="sxs-lookup"><span data-stu-id="18e8a-340">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="18e8a-341">Переменные среды на уровне компьютера задаются в файле *BASHRC* или *BASH_PROFILE*.</span><span class="sxs-lookup"><span data-stu-id="18e8a-341">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="18e8a-342">Измените файл в любом текстовом редакторе.</span><span class="sxs-lookup"><span data-stu-id="18e8a-342">Edit the file using any text editor.</span></span> <span data-ttu-id="18e8a-343">Добавьте следующий оператор:</span><span class="sxs-lookup"><span data-stu-id="18e8a-343">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="18e8a-344">Linux</span><span class="sxs-lookup"><span data-stu-id="18e8a-344">Linux</span></span>

<span data-ttu-id="18e8a-345">В дистрибутивах Linux используйте команду `export` в командной строке для значений переменных на уровне сеанса или в файле *BASH_PROFILE* для значений среды на уровне компьютера.</span><span class="sxs-lookup"><span data-stu-id="18e8a-345">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="18e8a-346">Указание среды в коде</span><span class="sxs-lookup"><span data-stu-id="18e8a-346">Set the environment in code</span></span>

<span data-ttu-id="18e8a-347">Вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> при создании узла.</span><span class="sxs-lookup"><span data-stu-id="18e8a-347">Call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="18e8a-348">См. раздел <xref:fundamentals/host/web-host#environment>.</span><span class="sxs-lookup"><span data-stu-id="18e8a-348">See <xref:fundamentals/host/web-host#environment>.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="18e8a-349">Конфигурация для разных сред</span><span class="sxs-lookup"><span data-stu-id="18e8a-349">Configuration by environment</span></span>

<span data-ttu-id="18e8a-350">Для загрузки конфигурации среды мы рекомендуем:</span><span class="sxs-lookup"><span data-stu-id="18e8a-350">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="18e8a-351">Файлы *appsettings* (*appsettings.{Environment}.json*).</span><span class="sxs-lookup"><span data-stu-id="18e8a-351">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="18e8a-352">См. раздел <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="18e8a-352">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="18e8a-353">Переменные среды (заданные в каждой системе, где размещено приложение).</span><span class="sxs-lookup"><span data-stu-id="18e8a-353">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="18e8a-354">См. разделы <xref:fundamentals/host/web-host#environment> и <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="18e8a-354">See <xref:fundamentals/host/web-host#environment> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="18e8a-355">Менеджер секретов (только в среде разработки).</span><span class="sxs-lookup"><span data-stu-id="18e8a-355">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="18e8a-356">См. раздел <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="18e8a-356">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="18e8a-357">Класс Startup и его методы для разных сред</span><span class="sxs-lookup"><span data-stu-id="18e8a-357">Environment-based Startup class and methods</span></span>

### <a name="inject-ihostingenvironment-into-startupconfigure"></a><span data-ttu-id="18e8a-358">Внедрение IHostingEnvironment в Startup.Configure</span><span class="sxs-lookup"><span data-stu-id="18e8a-358">Inject IHostingEnvironment into Startup.Configure</span></span>

<span data-ttu-id="18e8a-359">Внедрите <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="18e8a-359">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="18e8a-360">Этот подход удобен, когда для приложения требуется просто настроить `Startup.Configure` всего для нескольких сред с минимальными различиями в коде для каждой среды.</span><span class="sxs-lookup"><span data-stu-id="18e8a-360">This approach is useful when the app only requires configuring `Startup.Configure` for only a few environments with minimal code differences per environment.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Development environment code
    }
    else
    {
        // Code for all other environments
    }
}
```

### <a name="inject-ihostingenvironment-into-the-startup-class"></a><span data-ttu-id="18e8a-361">Внедрение IHostingEnvironment в класс Startup</span><span class="sxs-lookup"><span data-stu-id="18e8a-361">Inject IHostingEnvironment into the Startup class</span></span>

<span data-ttu-id="18e8a-362">Внедрите <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> в конструктор `Startup` и назначьте службу полю для использования в рамках всего класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="18e8a-362">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into the `Startup` constructor and assign the service to a field for use throughout the `Startup` class.</span></span> <span data-ttu-id="18e8a-363">Этот подход удобен, когда для приложения требуется настроить запуск всего для нескольких сред с минимальными различиями в коде для каждой среды.</span><span class="sxs-lookup"><span data-stu-id="18e8a-363">This approach is useful when the app requires configuring startup for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="18e8a-364">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="18e8a-364">In the following example:</span></span>

* <span data-ttu-id="18e8a-365">Среда хранится в поле `_env`.</span><span class="sxs-lookup"><span data-stu-id="18e8a-365">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="18e8a-366">`_env` используется в `ConfigureServices` и `Configure` для применения конфигурации запуска на основе среды приложения.</span><span class="sxs-lookup"><span data-stu-id="18e8a-366">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

```csharp
public class Startup
{
    private readonly IHostingEnvironment _env;

    public Startup(IHostingEnvironment env)
    {
        _env = env;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else if (_env.IsStaging())
        {
            // Staging environment code
        }
        else
        {
            // Code for all other environments
        }
    }

    public void Configure(IApplicationBuilder app)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else
        {
            // Code for all other environments
        }
    }
}
```

### <a name="startup-class-conventions"></a><span data-ttu-id="18e8a-367">Соглашения о классе Startup</span><span class="sxs-lookup"><span data-stu-id="18e8a-367">Startup class conventions</span></span>

<span data-ttu-id="18e8a-368">При запуске приложения ASP.NET Core [класс Startup](xref:fundamentals/startup) выполняет его начальную загрузку.</span><span class="sxs-lookup"><span data-stu-id="18e8a-368">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="18e8a-369">Приложение может определять отдельные классы `Startup` для различных сред (например, `StartupDevelopment`).</span><span class="sxs-lookup"><span data-stu-id="18e8a-369">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="18e8a-370">Подходящий класс `Startup` выбирается во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="18e8a-370">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="18e8a-371">Класс, у которого суффикс имени соответствует текущей среде, получает приоритет.</span><span class="sxs-lookup"><span data-stu-id="18e8a-371">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="18e8a-372">Если соответствующий класс `Startup{EnvironmentName}` не найден, используется класс `Startup`.</span><span class="sxs-lookup"><span data-stu-id="18e8a-372">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="18e8a-373">Этот подход удобен, когда для приложения требуется настроить запуск для нескольких сред с многочисленными различиями в коде для каждой среды.</span><span class="sxs-lookup"><span data-stu-id="18e8a-373">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="18e8a-374">Чтобы реализовать классы `Startup` на основе среды, создайте класс `Startup{EnvironmentName}` для каждой используемой среды и резервный класс `Startup`:</span><span class="sxs-lookup"><span data-stu-id="18e8a-374">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}
```

<span data-ttu-id="18e8a-375">Используйте перегрузку [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup), которая принимает имя сборки:</span><span class="sxs-lookup"><span data-stu-id="18e8a-375">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

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

### <a name="startup-method-conventions"></a><span data-ttu-id="18e8a-376">Соглашения о методах Startup</span><span class="sxs-lookup"><span data-stu-id="18e8a-376">Startup method conventions</span></span>

<span data-ttu-id="18e8a-377">Методы [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) и [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) поддерживают версии для конкретных сред в формате `Configure<EnvironmentName>` и `Configure<EnvironmentName>Services`.</span><span class="sxs-lookup"><span data-stu-id="18e8a-377">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="18e8a-378">Этот подход удобен, когда для приложения требуется настроить запуск для нескольких сред с многочисленными различиями в коде для каждой среды.</span><span class="sxs-lookup"><span data-stu-id="18e8a-378">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="18e8a-379">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="18e8a-379">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>

::: moniker-end