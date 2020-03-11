---
title: Использование нескольких сред в ASP.NET Core
author: rick-anderson
description: Сведения об управлении поведением приложений в разных средах в приложениях ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/17/2019
uid: fundamentals/environments
ms.openlocfilehash: b0218b2c77c283c0849dca9491046534b88c5a77
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78645358"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="a5fbf-103">Использование нескольких сред в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a5fbf-103">Use multiple environments in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a5fbf-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="a5fbf-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a5fbf-105">ASP.NET Core настраивает поведение приложения в зависимости от среды выполнения с помощью переменной среды.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="a5fbf-106">[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a5fbf-106">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="a5fbf-107">Среды</span><span class="sxs-lookup"><span data-stu-id="a5fbf-107">Environments</span></span>

<span data-ttu-id="a5fbf-108">ASP.NET Core считывает переменную среды `ASPNETCORE_ENVIRONMENT` при запуске приложения и сохраняет ее значение в свойстве [IWebHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="a5fbf-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IWebHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span></span> <span data-ttu-id="a5fbf-109">Переменной `ASPNETCORE_ENVIRONMENT` можно присвоить любое значение, но платформа предоставляет три значения:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.Extensions.Hosting.Environments.Development>
* <xref:Microsoft.Extensions.Hosting.Environments.Staging>
* <span data-ttu-id="a5fbf-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="a5fbf-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="a5fbf-111">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-111">The preceding code:</span></span>

* <span data-ttu-id="a5fbf-112">Вызывает [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage), если `ASPNETCORE_ENVIRONMENT` имеет значение `Development`.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="a5fbf-113">Вызывает [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler), если переменная `ASPNETCORE_ENVIRONMENT` имеет одно из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="a5fbf-114">[Вспомогательная функция тега среды](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) использует значение `IHostingEnvironment.EnvironmentName` для включения или исключения разметки в элементе:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="a5fbf-115">В ОС Windows и macOS регистр символов в переменных среды и их значениях не учитывается.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="a5fbf-116">В ОС Linux в переменных среды и их значениях **регистр символов по умолчанию учитывается**.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="a5fbf-117">Разработка</span><span class="sxs-lookup"><span data-stu-id="a5fbf-117">Development</span></span>

<span data-ttu-id="a5fbf-118">В среде разработки могут быть включены функции, которые не должны быть доступны в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="a5fbf-119">Например, шаблоны ASP.NET Core включают в среде разработки [страницу со сведениями об исключении для разработчика](xref:fundamentals/error-handling#developer-exception-page).</span><span class="sxs-lookup"><span data-stu-id="a5fbf-119">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="a5fbf-120">Среду для локального компьютера разработки можно задать в файле *Properties\launchSettings.json* проекта.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="a5fbf-121">Значения среды, заданные в файле *launchSettings.json*, переопределяют значения, заданные в системной среде.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="a5fbf-122">В следующем коде JSON показаны три профиля из файла *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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
> <span data-ttu-id="a5fbf-123">Свойство `applicationUrl` в *launchSettings.json* может задать список URL-адресов сервера.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="a5fbf-124">Для разделения URL-адресов в списке используется точка с запятой:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-124">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="a5fbf-125">Когда приложение запускается с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run), используется первый профиль с атрибутом `"commandName": "Project"`.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="a5fbf-126">Значение `commandName` определяет запускаемый веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="a5fbf-127">`commandName` может иметь одно из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-127">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="a5fbf-128">`Project` (запускается Kestrel)</span><span class="sxs-lookup"><span data-stu-id="a5fbf-128">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="a5fbf-129">Когда приложение запускается с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run), выполняются указанные ниже действия:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-129">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="a5fbf-130">Считывается файл *launchSettings.json*, если он доступен.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="a5fbf-131">Параметры `environmentVariables` в файле *launchSettings.json* переопределяют переменные среды.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="a5fbf-132">Отображается среда размещения.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-132">The hosting environment is displayed.</span></span>

<span data-ttu-id="a5fbf-133">Ниже представлены выходные данные приложения, запущенного с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="a5fbf-133">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="a5fbf-134">Вкладка **Отладка** в свойствах проекта Visual Studio предоставляет графический пользовательский интерфейс для изменения файла *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-134">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Задание переменных среды в свойствах проекта](environments/_static/project-properties-debug.png)

<span data-ttu-id="a5fbf-136">Для вступления в силу изменений, внесенных в профили проекта, может потребоваться перезапуск веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="a5fbf-137">Чтобы сервер Kestrel обнаружил изменения, внесенные в среду, его необходимо перезапустить.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-137">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="a5fbf-138">В файле *launchSettings.json* не должны храниться секреты.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-138">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="a5fbf-139">Для хранения секретов во время разработки в локальной среде можно использовать [средство Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="a5fbf-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="a5fbf-140">При использовании [Visual Studio Code](https://code.visualstudio.com/) переменные среды можно задавать в файле *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-140">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="a5fbf-141">В следующем примере показано, как присвоить среде значение `Development`:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-141">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="a5fbf-142">Файл *.vscode/launch.json* в проекте не читается при запуске приложения с помощью команды `dotnet run` так же, как файл *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-142">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="a5fbf-143">При запуске приложения в среде разработки без файла *launchSettings.json* укажите среду с помощью переменной среды или задайте аргумент командной строки как команду `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-143">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="a5fbf-144">Рабочие</span><span class="sxs-lookup"><span data-stu-id="a5fbf-144">Production</span></span>

<span data-ttu-id="a5fbf-145">Конфигурация рабочей среды должна обеспечивать максимальный уровень безопасности, производительности и надежности приложений.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-145">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="a5fbf-146">Некоторые общие параметры, отличные от разработки:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-146">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="a5fbf-147">кэширование;</span><span class="sxs-lookup"><span data-stu-id="a5fbf-147">Caching.</span></span>
* <span data-ttu-id="a5fbf-148">ресурсы на стороне клиента объединяются в пакеты, уплотняются и могут предоставляться из сети CDN;</span><span class="sxs-lookup"><span data-stu-id="a5fbf-148">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="a5fbf-149">страницы с сообщениями об ошибках диагностики отключены;</span><span class="sxs-lookup"><span data-stu-id="a5fbf-149">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="a5fbf-150">включены страницы с понятными пользователям сообщениями об ошибках;</span><span class="sxs-lookup"><span data-stu-id="a5fbf-150">Friendly error pages enabled.</span></span>
* <span data-ttu-id="a5fbf-151">включены средства ведения журналов и мониторинга в рабочей среде,</span><span class="sxs-lookup"><span data-stu-id="a5fbf-151">Production logging and monitoring enabled.</span></span> <span data-ttu-id="a5fbf-152">например [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="a5fbf-152">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="a5fbf-153">Указание среды</span><span class="sxs-lookup"><span data-stu-id="a5fbf-153">Set the environment</span></span>

<span data-ttu-id="a5fbf-154">Часто бывает полезным указать определенную среду для тестирования с переменной среды или параметром платформы.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-154">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="a5fbf-155">Если среда не указана, по умолчанию используется среда `Production`, в которой большинство функций отладки отключено.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-155">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="a5fbf-156">Способ указания среды зависит от операционной системы.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-156">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="a5fbf-157">При создании узла среду приложения определяет последний параметр среды, считанный приложением.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-157">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="a5fbf-158">Среду приложения невозможно изменить во время его выполнения.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-158">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="a5fbf-159">Переменная среды или параметр платформы</span><span class="sxs-lookup"><span data-stu-id="a5fbf-159">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="a5fbf-160">Служба приложений Azure</span><span class="sxs-lookup"><span data-stu-id="a5fbf-160">Azure App Service</span></span>

<span data-ttu-id="a5fbf-161">Чтобы установить среду в [службе приложений Azure](https://azure.microsoft.com/services/app-service/), выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-161">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="a5fbf-162">Выберите приложение из колонки **Службы приложений**.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-162">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="a5fbf-163">В группе **Параметры** выберите колонку **Конфигурация**.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-163">In the **Settings** group, select the **Configuration** blade.</span></span>
1. <span data-ttu-id="a5fbf-164">На вкладке **Параметры приложения** выберите **Новый параметр приложения**.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-164">In the **Application settings** tab, select **New application setting**.</span></span>
1. <span data-ttu-id="a5fbf-165">В окне **Добавить или изменить параметр приложения** в поле **Имя** введите `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-165">In the **Add/Edit application setting** window, provide `ASPNETCORE_ENVIRONMENT` for the **Name**.</span></span> <span data-ttu-id="a5fbf-166">В поле **Значение** укажите среду (например, `Staging`).</span><span class="sxs-lookup"><span data-stu-id="a5fbf-166">For **Value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="a5fbf-167">Установите флажок **Параметр слота развертывания**, если нужно, чтобы параметр среды оставался в текущем слоте при замене слота развертывания.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-167">Select the **Deployment slot setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="a5fbf-168">Дополнительные сведения см. в статье [Set up staging environments in Azure App Service](/azure/app-service/web-sites-staged-publishing) (Настройка промежуточных сред в Службе приложений Azure) в документации по Azure.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-168">For more information, see [Set up staging environments in Azure App Service](/azure/app-service/web-sites-staged-publishing) in the Azure documentation.</span></span>
1. <span data-ttu-id="a5fbf-169">Нажмите кнопку **ОК**, чтобы закрыть окно **Добавить или изменить параметр приложения**.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-169">Select **OK** to close the **Add/Edit application setting** window.</span></span>
1. <span data-ttu-id="a5fbf-170">Нажмите кнопку **Сохранить** в верхней части колонки **Конфигурация**.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-170">Select **Save** at the top of the **Configuration** blade.</span></span>

<span data-ttu-id="a5fbf-171">Служба приложений Azure автоматически перезапускает приложение после добавления, изменения или удаления параметра приложения (переменной среды) на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-171">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="a5fbf-172">Windows</span><span class="sxs-lookup"><span data-stu-id="a5fbf-172">Windows</span></span>

<span data-ttu-id="a5fbf-173">Если приложение запускается с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run), то, чтобы задать переменную `ASPNETCORE_ENVIRONMENT` для текущего сеанса, используйте следующие команды:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-173">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="a5fbf-174">**Командная строка**</span><span class="sxs-lookup"><span data-stu-id="a5fbf-174">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="a5fbf-175">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="a5fbf-175">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="a5fbf-176">Эти команды действуют только для текущего окна.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-176">These commands only take effect for the current window.</span></span> <span data-ttu-id="a5fbf-177">Когда окно закрывается, для параметра `ASPNETCORE_ENVIRONMENT` восстанавливается значение по умолчанию или значение, заданное на компьютере.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-177">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="a5fbf-178">Чтобы задать это значение в Windows на глобальном уровне, используйте один из следующих подходов.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-178">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="a5fbf-179">Последовательно откройте **Панель управления** > **Система** > **Дополнительные параметры системы** и добавьте или измените значение `ASPNETCORE_ENVIRONMENT`:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-179">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Дополнительные параметры системы](environments/_static/systemsetting_environment.png)

  ![Переменная среды ASPNET Core](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="a5fbf-182">Откройте командную строку администратора и выполните команду `setx` либо откройте командную строку администратора PowerShell и используйте `[Environment]::SetEnvironmentVariable`:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-182">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="a5fbf-183">**Командная строка**</span><span class="sxs-lookup"><span data-stu-id="a5fbf-183">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="a5fbf-184">Параметр `/M` указывает на установку переменной среды на уровне системы.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-184">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="a5fbf-185">Если параметр `/M` не используется, переменная среды задается для учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-185">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="a5fbf-186">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="a5fbf-186">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="a5fbf-187">Значение параметра `Machine` указывает на установку переменной среды на уровне системы.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-187">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="a5fbf-188">При изменении значения параметра на `User` переменная среды задается для учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-188">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="a5fbf-189">Если переменная среды `ASPNETCORE_ENVIRONMENT` задана глобально, она действует для `dotnet run` в любом окне командной строки, открываемом после установки значения.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-189">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="a5fbf-190">**web.config**</span><span class="sxs-lookup"><span data-stu-id="a5fbf-190">**web.config**</span></span>

<span data-ttu-id="a5fbf-191">Сведения об установке переменной среды `ASPNETCORE_ENVIRONMENT` в файле *web.config* см. в разделе *Настройка переменной среды* в <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-191">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="a5fbf-192">**Файл проекта или профиль публикации**</span><span class="sxs-lookup"><span data-stu-id="a5fbf-192">**Project file or publish profile**</span></span>

<span data-ttu-id="a5fbf-193">**Для развертываний Windows IIS:** Включите свойство `<EnvironmentName>` в [профиле публикации (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) или файле проекта.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-193">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="a5fbf-194">При этом подходе во время публикации проекта среда задается в файле *web.config*:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-194">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="a5fbf-195">**Пул приложений IIS**</span><span class="sxs-lookup"><span data-stu-id="a5fbf-195">**Per IIS Application Pool**</span></span>

<span data-ttu-id="a5fbf-196">Чтобы задать переменную среды `ASPNETCORE_ENVIRONMENT` для приложения, выполняющегося в изолированном пуле приложений (такая возможность поддерживается в службах IIS 10.0 и более поздних версий), см. подраздел, посвященный команде *AppCmd.exe*, в разделе [Переменные среды &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).</span><span class="sxs-lookup"><span data-stu-id="a5fbf-196">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="a5fbf-197">Если переменная среды `ASPNETCORE_ENVIRONMENT` задана для пула приложений, ее значение переопределяет значение на уровне системы.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-197">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a5fbf-198">При размещении приложения в службах IIS и добавлении или изменении переменной среды `ASPNETCORE_ENVIRONMENT` используйте один из следующих подходов по применению нового значения в приложении.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-198">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="a5fbf-199">Из командной строки выполните команду `net stop was /y`, за которой следует `net start w3svc`.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-199">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="a5fbf-200">Перезапустите сервер.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-200">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="a5fbf-201">macOS</span><span class="sxs-lookup"><span data-stu-id="a5fbf-201">macOS</span></span>

<span data-ttu-id="a5fbf-202">Задать текущую среду в macOS можно в командной строке при запуске приложения:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-202">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="a5fbf-203">Также можно задать среду с помощью команды `export` до запуска приложения:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-203">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="a5fbf-204">Переменные среды на уровне компьютера задаются в файле *BASHRC* или *BASH_PROFILE*.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-204">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="a5fbf-205">Измените файл в любом текстовом редакторе.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-205">Edit the file using any text editor.</span></span> <span data-ttu-id="a5fbf-206">Добавьте следующий оператор:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-206">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="a5fbf-207">Linux</span><span class="sxs-lookup"><span data-stu-id="a5fbf-207">Linux</span></span>

<span data-ttu-id="a5fbf-208">В дистрибутивах Linux используйте команду `export` в командной строке для значений переменных на уровне сеанса или в файле *BASH_PROFILE* для значений среды на уровне компьютера.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-208">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="a5fbf-209">Указание среды в коде</span><span class="sxs-lookup"><span data-stu-id="a5fbf-209">Set the environment in code</span></span>

<span data-ttu-id="a5fbf-210">Вызовите <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> при создании узла.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-210">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="a5fbf-211">См. раздел <xref:fundamentals/host/generic-host#environmentname>.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-211">See <xref:fundamentals/host/generic-host#environmentname>.</span></span>


### <a name="configuration-by-environment"></a><span data-ttu-id="a5fbf-212">Конфигурация для разных сред</span><span class="sxs-lookup"><span data-stu-id="a5fbf-212">Configuration by environment</span></span>

<span data-ttu-id="a5fbf-213">Для загрузки конфигурации среды мы рекомендуем:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-213">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="a5fbf-214">Файлы *appsettings* (*appsettings.{Environment}.json*).</span><span class="sxs-lookup"><span data-stu-id="a5fbf-214">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="a5fbf-215">См. раздел <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-215">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="a5fbf-216">Переменные среды (заданные в каждой системе, где размещено приложение).</span><span class="sxs-lookup"><span data-stu-id="a5fbf-216">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="a5fbf-217">См. разделы <xref:fundamentals/host/generic-host#environmentname> и <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-217">See <xref:fundamentals/host/generic-host#environmentname> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="a5fbf-218">Менеджер секретов (только в среде разработки).</span><span class="sxs-lookup"><span data-stu-id="a5fbf-218">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="a5fbf-219">См. раздел <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-219">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="a5fbf-220">Класс Startup и его методы для разных сред</span><span class="sxs-lookup"><span data-stu-id="a5fbf-220">Environment-based Startup class and methods</span></span>

### <a name="inject-iwebhostenvironment-into-startupconfigure"></a><span data-ttu-id="a5fbf-221">Внедрение IWebHostEnvironment в Startup.Configure</span><span class="sxs-lookup"><span data-stu-id="a5fbf-221">Inject IWebHostEnvironment into Startup.Configure</span></span>

<span data-ttu-id="a5fbf-222">Внедрите <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-222">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="a5fbf-223">Этот подход удобен, когда для приложения требуется просто скорректировать `Startup.Configure` для нескольких сред с минимальными различиями в коде для каждой среды.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-223">This approach is useful when the app only requires adjusting `Startup.Configure` for a few environments with minimal code differences per environment.</span></span>

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

### <a name="inject-iwebhostenvironment-into-the-startup-class"></a><span data-ttu-id="a5fbf-224">Внедрение IWebHostEnvironment в класс Startup</span><span class="sxs-lookup"><span data-stu-id="a5fbf-224">Inject IWebHostEnvironment into the Startup class</span></span>

<span data-ttu-id="a5fbf-225">Внедрите <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> в конструктор `Startup`.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-225">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into the `Startup` constructor.</span></span> <span data-ttu-id="a5fbf-226">Этот подход удобен, когда для приложения требуется настроить `Startup` всего для нескольких сред с минимальными различиями в коде для каждой среды.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-226">This approach is useful when the app requires configuring `Startup` for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="a5fbf-227">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-227">In the following example:</span></span>

* <span data-ttu-id="a5fbf-228">Среда хранится в поле `_env`.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-228">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="a5fbf-229">`_env` используется в `ConfigureServices` и `Configure` для применения конфигурации запуска на основе среды приложения.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-229">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

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
### <a name="startup-class-conventions"></a><span data-ttu-id="a5fbf-230">Соглашения о классе Startup</span><span class="sxs-lookup"><span data-stu-id="a5fbf-230">Startup class conventions</span></span>

<span data-ttu-id="a5fbf-231">При запуске приложения ASP.NET Core [класс Startup](xref:fundamentals/startup) выполняет его начальную загрузку.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-231">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="a5fbf-232">Приложение может определять отдельные классы `Startup` для различных сред (например, `StartupDevelopment`).</span><span class="sxs-lookup"><span data-stu-id="a5fbf-232">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="a5fbf-233">Подходящий класс `Startup` выбирается во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-233">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="a5fbf-234">Класс, у которого суффикс имени соответствует текущей среде, получает приоритет.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-234">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="a5fbf-235">Если соответствующий класс `Startup{EnvironmentName}` не найден, используется класс `Startup`.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-235">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="a5fbf-236">Этот подход удобен, когда для приложения требуется настроить запуск для нескольких сред с многочисленными различиями в коде для каждой среды.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-236">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="a5fbf-237">Чтобы реализовать классы `Startup` на основе среды, создайте класс `Startup{EnvironmentName}` для каждой используемой среды и резервный класс `Startup`:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-237">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

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

[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="a5fbf-238">Используйте перегрузку [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup), которая принимает имя сборки:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-238">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

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

### <a name="startup-method-conventions"></a><span data-ttu-id="a5fbf-239">Соглашения о методах Startup</span><span class="sxs-lookup"><span data-stu-id="a5fbf-239">Startup method conventions</span></span>

<span data-ttu-id="a5fbf-240">Методы [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) и [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) поддерживают версии для конкретных сред в формате `Configure<EnvironmentName>` и `Configure<EnvironmentName>Services`.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-240">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="a5fbf-241">Этот подход удобен, когда для приложения требуется настроить запуск для нескольких сред с многочисленными различиями в коде для каждой среды.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-241">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="a5fbf-242">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a5fbf-242">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a5fbf-243">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="a5fbf-243">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a5fbf-244">ASP.NET Core настраивает поведение приложения в зависимости от среды выполнения с помощью переменной среды.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-244">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="a5fbf-245">[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a5fbf-245">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="a5fbf-246">Среды</span><span class="sxs-lookup"><span data-stu-id="a5fbf-246">Environments</span></span>

<span data-ttu-id="a5fbf-247">ASP.NET Core считывает переменную среды `ASPNETCORE_ENVIRONMENT` при запуске приложения и сохраняет ее значение в свойстве [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="a5fbf-247">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span></span> <span data-ttu-id="a5fbf-248">Переменной `ASPNETCORE_ENVIRONMENT` можно присвоить любое значение, но платформа предоставляет три значения:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-248">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Staging>
* <span data-ttu-id="a5fbf-249"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="a5fbf-249"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="a5fbf-250">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-250">The preceding code:</span></span>

* <span data-ttu-id="a5fbf-251">Вызывает [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage), если `ASPNETCORE_ENVIRONMENT` имеет значение `Development`.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-251">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="a5fbf-252">Вызывает [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler), если переменная `ASPNETCORE_ENVIRONMENT` имеет одно из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-252">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="a5fbf-253">[Вспомогательная функция тега среды](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) использует значение `IHostingEnvironment.EnvironmentName` для включения или исключения разметки в элементе:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-253">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="a5fbf-254">В ОС Windows и macOS регистр символов в переменных среды и их значениях не учитывается.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-254">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="a5fbf-255">В ОС Linux в переменных среды и их значениях **регистр символов по умолчанию учитывается**.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-255">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="a5fbf-256">Разработка</span><span class="sxs-lookup"><span data-stu-id="a5fbf-256">Development</span></span>

<span data-ttu-id="a5fbf-257">В среде разработки могут быть включены функции, которые не должны быть доступны в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-257">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="a5fbf-258">Например, шаблоны ASP.NET Core включают в среде разработки [страницу со сведениями об исключении для разработчика](xref:fundamentals/error-handling#developer-exception-page).</span><span class="sxs-lookup"><span data-stu-id="a5fbf-258">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="a5fbf-259">Среду для локального компьютера разработки можно задать в файле *Properties\launchSettings.json* проекта.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-259">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="a5fbf-260">Значения среды, заданные в файле *launchSettings.json*, переопределяют значения, заданные в системной среде.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-260">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="a5fbf-261">В следующем коде JSON показаны три профиля из файла *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-261">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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
> <span data-ttu-id="a5fbf-262">Свойство `applicationUrl` в *launchSettings.json* может задать список URL-адресов сервера.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-262">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="a5fbf-263">Для разделения URL-адресов в списке используется точка с запятой:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-263">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="a5fbf-264">Когда приложение запускается с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run), используется первый профиль с атрибутом `"commandName": "Project"`.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-264">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="a5fbf-265">Значение `commandName` определяет запускаемый веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-265">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="a5fbf-266">`commandName` может иметь одно из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-266">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="a5fbf-267">`Project` (запускается Kestrel)</span><span class="sxs-lookup"><span data-stu-id="a5fbf-267">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="a5fbf-268">Когда приложение запускается с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run), выполняются указанные ниже действия:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-268">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="a5fbf-269">Считывается файл *launchSettings.json*, если он доступен.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-269">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="a5fbf-270">Параметры `environmentVariables` в файле *launchSettings.json* переопределяют переменные среды.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-270">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="a5fbf-271">Отображается среда размещения.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-271">The hosting environment is displayed.</span></span>

<span data-ttu-id="a5fbf-272">Ниже представлены выходные данные приложения, запущенного с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="a5fbf-272">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="a5fbf-273">Вкладка **Отладка** в свойствах проекта Visual Studio предоставляет графический пользовательский интерфейс для изменения файла *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-273">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Задание переменных среды в свойствах проекта](environments/_static/project-properties-debug.png)

<span data-ttu-id="a5fbf-275">Для вступления в силу изменений, внесенных в профили проекта, может потребоваться перезапуск веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-275">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="a5fbf-276">Чтобы сервер Kestrel обнаружил изменения, внесенные в среду, его необходимо перезапустить.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-276">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="a5fbf-277">В файле *launchSettings.json* не должны храниться секреты.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-277">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="a5fbf-278">Для хранения секретов во время разработки в локальной среде можно использовать [средство Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="a5fbf-278">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="a5fbf-279">При использовании [Visual Studio Code](https://code.visualstudio.com/) переменные среды можно задавать в файле *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-279">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="a5fbf-280">В следующем примере показано, как присвоить среде значение `Development`:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-280">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="a5fbf-281">Файл *.vscode/launch.json* в проекте не читается при запуске приложения с помощью команды `dotnet run` так же, как файл *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-281">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="a5fbf-282">При запуске приложения в среде разработки без файла *launchSettings.json* укажите среду с помощью переменной среды или задайте аргумент командной строки как команду `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-282">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="a5fbf-283">Рабочие</span><span class="sxs-lookup"><span data-stu-id="a5fbf-283">Production</span></span>

<span data-ttu-id="a5fbf-284">Конфигурация рабочей среды должна обеспечивать максимальный уровень безопасности, производительности и надежности приложений.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-284">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="a5fbf-285">Некоторые общие параметры, отличные от разработки:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-285">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="a5fbf-286">кэширование;</span><span class="sxs-lookup"><span data-stu-id="a5fbf-286">Caching.</span></span>
* <span data-ttu-id="a5fbf-287">ресурсы на стороне клиента объединяются в пакеты, уплотняются и могут предоставляться из сети CDN;</span><span class="sxs-lookup"><span data-stu-id="a5fbf-287">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="a5fbf-288">страницы с сообщениями об ошибках диагностики отключены;</span><span class="sxs-lookup"><span data-stu-id="a5fbf-288">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="a5fbf-289">включены страницы с понятными пользователям сообщениями об ошибках;</span><span class="sxs-lookup"><span data-stu-id="a5fbf-289">Friendly error pages enabled.</span></span>
* <span data-ttu-id="a5fbf-290">включены средства ведения журналов и мониторинга в рабочей среде,</span><span class="sxs-lookup"><span data-stu-id="a5fbf-290">Production logging and monitoring enabled.</span></span> <span data-ttu-id="a5fbf-291">например [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="a5fbf-291">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="a5fbf-292">Указание среды</span><span class="sxs-lookup"><span data-stu-id="a5fbf-292">Set the environment</span></span>

<span data-ttu-id="a5fbf-293">Часто бывает полезным указать определенную среду для тестирования с переменной среды или параметром платформы.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-293">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="a5fbf-294">Если среда не указана, по умолчанию используется среда `Production`, в которой большинство функций отладки отключено.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-294">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="a5fbf-295">Способ указания среды зависит от операционной системы.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-295">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="a5fbf-296">При создании узла среду приложения определяет последний параметр среды, считанный приложением.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-296">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="a5fbf-297">Среду приложения невозможно изменить во время его выполнения.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-297">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="a5fbf-298">Переменная среды или параметр платформы</span><span class="sxs-lookup"><span data-stu-id="a5fbf-298">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="a5fbf-299">Служба приложений Azure</span><span class="sxs-lookup"><span data-stu-id="a5fbf-299">Azure App Service</span></span>

<span data-ttu-id="a5fbf-300">Чтобы установить среду в [службе приложений Azure](https://azure.microsoft.com/services/app-service/), выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-300">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="a5fbf-301">Выберите приложение из колонки **Службы приложений**.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-301">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="a5fbf-302">В группе **Параметры** выберите колонку **Конфигурация**.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-302">In the **Settings** group, select the **Configuration** blade.</span></span>
1. <span data-ttu-id="a5fbf-303">На вкладке **Параметры приложения** выберите **Новый параметр приложения**.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-303">In the **Application settings** tab, select **New application setting**.</span></span>
1. <span data-ttu-id="a5fbf-304">В окне **Добавить или изменить параметр приложения** в поле **Имя** введите `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-304">In the **Add/Edit application setting** window, provide `ASPNETCORE_ENVIRONMENT` for the **Name**.</span></span> <span data-ttu-id="a5fbf-305">В поле **Значение** укажите среду (например, `Staging`).</span><span class="sxs-lookup"><span data-stu-id="a5fbf-305">For **Value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="a5fbf-306">Установите флажок **Параметр слота развертывания**, если нужно, чтобы параметр среды оставался в текущем слоте при замене слота развертывания.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-306">Select the **Deployment slot setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="a5fbf-307">Дополнительные сведения см. в статье [Set up staging environments in Azure App Service](/azure/app-service/web-sites-staged-publishing) (Настройка промежуточных сред в Службе приложений Azure) в документации по Azure.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-307">For more information, see [Set up staging environments in Azure App Service](/azure/app-service/web-sites-staged-publishing) in the Azure documentation.</span></span>
1. <span data-ttu-id="a5fbf-308">Нажмите кнопку **ОК**, чтобы закрыть окно **Добавить или изменить параметр приложения**.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-308">Select **OK** to close the **Add/Edit application setting** window.</span></span>
1. <span data-ttu-id="a5fbf-309">Нажмите кнопку **Сохранить** в верхней части колонки **Конфигурация**.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-309">Select **Save** at the top of the **Configuration** blade.</span></span>

<span data-ttu-id="a5fbf-310">Служба приложений Azure автоматически перезапускает приложение после добавления, изменения или удаления параметра приложения (переменной среды) на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-310">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="a5fbf-311">Windows</span><span class="sxs-lookup"><span data-stu-id="a5fbf-311">Windows</span></span>

<span data-ttu-id="a5fbf-312">Если приложение запускается с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run), то, чтобы задать переменную `ASPNETCORE_ENVIRONMENT` для текущего сеанса, используйте следующие команды:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-312">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="a5fbf-313">**Командная строка**</span><span class="sxs-lookup"><span data-stu-id="a5fbf-313">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="a5fbf-314">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="a5fbf-314">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="a5fbf-315">Эти команды действуют только для текущего окна.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-315">These commands only take effect for the current window.</span></span> <span data-ttu-id="a5fbf-316">Когда окно закрывается, для параметра `ASPNETCORE_ENVIRONMENT` восстанавливается значение по умолчанию или значение, заданное на компьютере.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-316">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="a5fbf-317">Чтобы задать это значение в Windows на глобальном уровне, используйте один из следующих подходов.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-317">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="a5fbf-318">Последовательно откройте **Панель управления** > **Система** > **Дополнительные параметры системы** и добавьте или измените значение `ASPNETCORE_ENVIRONMENT`:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-318">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Дополнительные параметры системы](environments/_static/systemsetting_environment.png)

  ![Переменная среды ASPNET Core](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="a5fbf-321">Откройте командную строку администратора и выполните команду `setx` либо откройте командную строку администратора PowerShell и используйте `[Environment]::SetEnvironmentVariable`:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-321">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="a5fbf-322">**Командная строка**</span><span class="sxs-lookup"><span data-stu-id="a5fbf-322">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="a5fbf-323">Параметр `/M` указывает на установку переменной среды на уровне системы.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-323">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="a5fbf-324">Если параметр `/M` не используется, переменная среды задается для учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-324">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="a5fbf-325">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="a5fbf-325">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="a5fbf-326">Значение параметра `Machine` указывает на установку переменной среды на уровне системы.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-326">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="a5fbf-327">При изменении значения параметра на `User` переменная среды задается для учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-327">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="a5fbf-328">Если переменная среды `ASPNETCORE_ENVIRONMENT` задана глобально, она действует для `dotnet run` в любом окне командной строки, открываемом после установки значения.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-328">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="a5fbf-329">**web.config**</span><span class="sxs-lookup"><span data-stu-id="a5fbf-329">**web.config**</span></span>

<span data-ttu-id="a5fbf-330">Сведения об установке переменной среды `ASPNETCORE_ENVIRONMENT` в файле *web.config* см. в разделе *Настройка переменной среды* в <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-330">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="a5fbf-331">**Файл проекта или профиль публикации**</span><span class="sxs-lookup"><span data-stu-id="a5fbf-331">**Project file or publish profile**</span></span>

<span data-ttu-id="a5fbf-332">**Для развертываний Windows IIS:** Включите свойство `<EnvironmentName>` в [профиле публикации (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) или файле проекта.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-332">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="a5fbf-333">При этом подходе во время публикации проекта среда задается в файле *web.config*:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-333">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="a5fbf-334">**Пул приложений IIS**</span><span class="sxs-lookup"><span data-stu-id="a5fbf-334">**Per IIS Application Pool**</span></span>

<span data-ttu-id="a5fbf-335">Чтобы задать переменную среды `ASPNETCORE_ENVIRONMENT` для приложения, выполняющегося в изолированном пуле приложений (такая возможность поддерживается в службах IIS 10.0 и более поздних версий), см. подраздел, посвященный команде *AppCmd.exe*, в разделе [Переменные среды &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).</span><span class="sxs-lookup"><span data-stu-id="a5fbf-335">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="a5fbf-336">Если переменная среды `ASPNETCORE_ENVIRONMENT` задана для пула приложений, ее значение переопределяет значение на уровне системы.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-336">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a5fbf-337">При размещении приложения в службах IIS и добавлении или изменении переменной среды `ASPNETCORE_ENVIRONMENT` используйте один из следующих подходов по применению нового значения в приложении.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-337">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="a5fbf-338">Из командной строки выполните команду `net stop was /y`, за которой следует `net start w3svc`.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-338">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="a5fbf-339">Перезапустите сервер.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-339">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="a5fbf-340">macOS</span><span class="sxs-lookup"><span data-stu-id="a5fbf-340">macOS</span></span>

<span data-ttu-id="a5fbf-341">Задать текущую среду в macOS можно в командной строке при запуске приложения:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-341">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="a5fbf-342">Также можно задать среду с помощью команды `export` до запуска приложения:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-342">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="a5fbf-343">Переменные среды на уровне компьютера задаются в файле *BASHRC* или *BASH_PROFILE*.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-343">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="a5fbf-344">Измените файл в любом текстовом редакторе.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-344">Edit the file using any text editor.</span></span> <span data-ttu-id="a5fbf-345">Добавьте следующий оператор:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-345">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="a5fbf-346">Linux</span><span class="sxs-lookup"><span data-stu-id="a5fbf-346">Linux</span></span>

<span data-ttu-id="a5fbf-347">В дистрибутивах Linux используйте команду `export` в командной строке для значений переменных на уровне сеанса или в файле *BASH_PROFILE* для значений среды на уровне компьютера.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-347">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="a5fbf-348">Указание среды в коде</span><span class="sxs-lookup"><span data-stu-id="a5fbf-348">Set the environment in code</span></span>

<span data-ttu-id="a5fbf-349">Вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> при создании узла.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-349">Call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="a5fbf-350">См. раздел <xref:fundamentals/host/web-host#environment>.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-350">See <xref:fundamentals/host/web-host#environment>.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="a5fbf-351">Конфигурация для разных сред</span><span class="sxs-lookup"><span data-stu-id="a5fbf-351">Configuration by environment</span></span>

<span data-ttu-id="a5fbf-352">Для загрузки конфигурации среды мы рекомендуем:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-352">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="a5fbf-353">Файлы *appsettings* (*appsettings.{Environment}.json*).</span><span class="sxs-lookup"><span data-stu-id="a5fbf-353">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="a5fbf-354">См. раздел <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-354">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="a5fbf-355">Переменные среды (заданные в каждой системе, где размещено приложение).</span><span class="sxs-lookup"><span data-stu-id="a5fbf-355">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="a5fbf-356">См. разделы <xref:fundamentals/host/web-host#environment> и <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-356">See <xref:fundamentals/host/web-host#environment> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="a5fbf-357">Менеджер секретов (только в среде разработки).</span><span class="sxs-lookup"><span data-stu-id="a5fbf-357">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="a5fbf-358">См. раздел <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-358">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="a5fbf-359">Класс Startup и его методы для разных сред</span><span class="sxs-lookup"><span data-stu-id="a5fbf-359">Environment-based Startup class and methods</span></span>

### <a name="inject-ihostingenvironment-into-startupconfigure"></a><span data-ttu-id="a5fbf-360">Внедрение IHostingEnvironment в Startup.Configure</span><span class="sxs-lookup"><span data-stu-id="a5fbf-360">Inject IHostingEnvironment into Startup.Configure</span></span>

<span data-ttu-id="a5fbf-361">Внедрите <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-361">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="a5fbf-362">Этот подход удобен, когда для приложения требуется просто настроить `Startup.Configure` всего для нескольких сред с минимальными различиями в коде для каждой среды.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-362">This approach is useful when the app only requires configuring `Startup.Configure` for only a few environments with minimal code differences per environment.</span></span>

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

### <a name="inject-ihostingenvironment-into-the-startup-class"></a><span data-ttu-id="a5fbf-363">Внедрение IHostingEnvironment в класс Startup</span><span class="sxs-lookup"><span data-stu-id="a5fbf-363">Inject IHostingEnvironment into the Startup class</span></span>

<span data-ttu-id="a5fbf-364">Внедрите <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> в конструктор `Startup` и назначьте службу полю для использования в рамках всего класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-364">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into the `Startup` constructor and assign the service to a field for use throughout the `Startup` class.</span></span> <span data-ttu-id="a5fbf-365">Этот подход удобен, когда для приложения требуется настроить запуск всего для нескольких сред с минимальными различиями в коде для каждой среды.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-365">This approach is useful when the app requires configuring startup for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="a5fbf-366">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-366">In the following example:</span></span>

* <span data-ttu-id="a5fbf-367">Среда хранится в поле `_env`.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-367">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="a5fbf-368">`_env` используется в `ConfigureServices` и `Configure` для применения конфигурации запуска на основе среды приложения.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-368">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

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

### <a name="startup-class-conventions"></a><span data-ttu-id="a5fbf-369">Соглашения о классе Startup</span><span class="sxs-lookup"><span data-stu-id="a5fbf-369">Startup class conventions</span></span>

<span data-ttu-id="a5fbf-370">При запуске приложения ASP.NET Core [класс Startup](xref:fundamentals/startup) выполняет его начальную загрузку.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-370">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="a5fbf-371">Приложение может определять отдельные классы `Startup` для различных сред (например, `StartupDevelopment`).</span><span class="sxs-lookup"><span data-stu-id="a5fbf-371">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="a5fbf-372">Подходящий класс `Startup` выбирается во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-372">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="a5fbf-373">Класс, у которого суффикс имени соответствует текущей среде, получает приоритет.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-373">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="a5fbf-374">Если соответствующий класс `Startup{EnvironmentName}` не найден, используется класс `Startup`.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-374">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="a5fbf-375">Этот подход удобен, когда для приложения требуется настроить запуск для нескольких сред с многочисленными различиями в коде для каждой среды.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-375">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="a5fbf-376">Чтобы реализовать классы `Startup` на основе среды, создайте класс `Startup{EnvironmentName}` для каждой используемой среды и резервный класс `Startup`:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-376">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

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

<span data-ttu-id="a5fbf-377">Используйте перегрузку [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup), которая принимает имя сборки:</span><span class="sxs-lookup"><span data-stu-id="a5fbf-377">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

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

### <a name="startup-method-conventions"></a><span data-ttu-id="a5fbf-378">Соглашения о методах Startup</span><span class="sxs-lookup"><span data-stu-id="a5fbf-378">Startup method conventions</span></span>

<span data-ttu-id="a5fbf-379">Методы [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) и [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) поддерживают версии для конкретных сред в формате `Configure<EnvironmentName>` и `Configure<EnvironmentName>Services`.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-379">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="a5fbf-380">Этот подход удобен, когда для приложения требуется настроить запуск для нескольких сред с многочисленными различиями в коде для каждой среды.</span><span class="sxs-lookup"><span data-stu-id="a5fbf-380">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="a5fbf-381">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a5fbf-381">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>

::: moniker-end
