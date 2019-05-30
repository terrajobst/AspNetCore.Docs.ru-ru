---
title: Развертывание приложений ASP.NET Core в Службе приложений Azure
author: guardrex
description: Эта статья содержит ссылки на ресурсы по размещению и развертыванию в Azure.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/30/2019
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 8ce969739d9d98941d4d7670395c74e0e25c92a7
ms.sourcegitcommit: b8ed594ab9f47fa32510574f3e1b210cff000967
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2019
ms.locfileid: "66251401"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a><span data-ttu-id="852af-103">Развертывание приложений ASP.NET Core в Службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="852af-103">Deploy ASP.NET Core apps to Azure App Service</span></span>

<span data-ttu-id="852af-104">[Служба приложений Azure](https://azure.microsoft.com/services/app-service/) — это [платформа облачных вычислений Microsoft](https://azure.microsoft.com/), предназначенная для размещения веб-приложений, включая ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="852af-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="852af-105">Полезные ресурсы</span><span class="sxs-lookup"><span data-stu-id="852af-105">Useful resources</span></span>

<span data-ttu-id="852af-106">[Документация по службе приложений](/azure/app-service/) — это место, где хранятся документация, учебники, примеры, руководства и другие ресурсы, связанные с приложениями Azure.</span><span class="sxs-lookup"><span data-stu-id="852af-106">[App Service Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="852af-107">Размещению приложений ASP.NET Core посвящены следующие два руководства.</span><span class="sxs-lookup"><span data-stu-id="852af-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="852af-108">Создание веб-приложения ASP.NET Core в Azure</span><span class="sxs-lookup"><span data-stu-id="852af-108">Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="852af-109">Создайте веб-приложение ASP.NET Core и разверните его в службе приложений Azure на базе Windows с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="852af-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="852af-110">Создание приложения ASP.NET Core в Службе приложений в Linux</span><span class="sxs-lookup"><span data-stu-id="852af-110">Create an ASP.NET Core app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="852af-111">Создайте веб-приложение ASP.NET Core и разверните его в службе приложений Azure на базе Linux с помощью командной строки.</span><span class="sxs-lookup"><span data-stu-id="852af-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="852af-112">Следующие статьи входят в документацию по ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="852af-112">The following articles are available in ASP.NET Core documentation:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>  
<span data-ttu-id="852af-113">Сведения о публикации приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="852af-113">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

<xref:host-and-deploy/azure-apps/azure-continuous-deployment>  
<span data-ttu-id="852af-114">Сведения о создании веб-приложения ASP.NET Core с помощью Visual Studio и его развертывании в службе приложений Azure с использованием Git для непрерывного развертывания.</span><span class="sxs-lookup"><span data-stu-id="852af-114">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="852af-115">Создание первого конвейера</span><span class="sxs-lookup"><span data-stu-id="852af-115">Create your first pipeline</span></span>](/azure/devops/pipelines/get-started-yaml)  
<span data-ttu-id="852af-116">Сведения о настройке сборки CI для приложения ASP.NET Core и последующем создании выпуска непрерывного развертывания в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="852af-116">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="852af-117">Песочница веб-приложений Azure</span><span class="sxs-lookup"><span data-stu-id="852af-117">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="852af-118">Сведения об ограничениях среды выполнения службы приложений Azure, создаваемых платформой приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="852af-118">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="852af-119">Настройка приложения</span><span class="sxs-lookup"><span data-stu-id="852af-119">Application configuration</span></span>

### <a name="platform"></a><span data-ttu-id="852af-120">Platform</span><span class="sxs-lookup"><span data-stu-id="852af-120">Platform</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="852af-121">Среды выполнения для 64-разрядных (x64) и 32-разрядных (x86) приложений находятся в Службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="852af-121">Runtimes for 64-bit (x64) and 32-bit (x86) apps are present on Azure App Service.</span></span> <span data-ttu-id="852af-122">[Пакет SDK для .NET Core](/dotnet/core/sdk), доступный в Службе приложений, является 32-разрядным, но вы можете развертывать 64-разрядные приложения с помощью консоли [Kudu](https://github.com/projectkudu/kudu/wiki) или [MSDeploy с использованием профиля публикации Visual Studio либо интерфейса командной строки](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="852af-122">The [.NET Core SDK](/dotnet/core/sdk) available on App Service is 32-bit, but you can deploy 64-bit apps using the [Kudu](https://github.com/projectkudu/kudu/wiki) console or via [MSDeploy with a Visual Studio publish profile or CLI command](xref:host-and-deploy/visual-studio-publish-profiles).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="852af-123">Среды выполнения для 32-разрядных (x86) приложений с собственными зависимостями находятся в Службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="852af-123">For apps with native dependencies, runtimes for 32-bit (x86) apps are present on Azure App Service.</span></span> <span data-ttu-id="852af-124">[Пакет SDK для .NET Core](/dotnet/core/sdk), доступный в Службе приложений, является 32-разрядным.</span><span class="sxs-lookup"><span data-stu-id="852af-124">The [.NET Core SDK](/dotnet/core/sdk) available on App Service is 32-bit.</span></span>

::: moniker-end

### <a name="packages"></a><span data-ttu-id="852af-125">Пакеты</span><span class="sxs-lookup"><span data-stu-id="852af-125">Packages</span></span>

<span data-ttu-id="852af-126">Включите следующие пакеты NuGet, чтобы использовать возможности автоматического ведения журнала для приложений, развернутых в Службе приложений Azure:</span><span class="sxs-lookup"><span data-stu-id="852af-126">Include the following NuGet packages to provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="852af-127">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) использует [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) для интеграции подсветки ASP.NET Core в службу приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="852af-127">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="852af-128">Пакет `Microsoft.AspNetCore.AzureAppServicesIntegration` включает дополнительные функции для ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="852af-128">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="852af-129">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) выполняет [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) для добавления поставщиков ведения журналов диагностики из службы приложений Azure в пакет `Microsoft.Extensions.Logging.AzureAppServices`.</span><span class="sxs-lookup"><span data-stu-id="852af-129">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="852af-130">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) предоставляет реализации средства ведения журналов в поддержку журналов диагностики службы приложений Azure и функций потоковой передачи журналов.</span><span class="sxs-lookup"><span data-stu-id="852af-130">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="852af-131">Предыдущие пакеты недоступны в [метапакете Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="852af-131">The preceding packages aren't available from the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="852af-132">Приложения, предназначенные для .NET Framework или ссылающиеся на метапакет `Microsoft.AspNetCore.App`, должны явно ссылаться на отдельные пакеты в файле проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="852af-132">Apps that target .NET Framework or reference the `Microsoft.AspNetCore.App` metapackage must explicitly reference the individual packages in the app's project file.</span></span>

## <a name="override-app-configuration-using-the-azure-portal"></a><span data-ttu-id="852af-133">Переопределение конфигурации приложения с помощью портала Azure</span><span class="sxs-lookup"><span data-stu-id="852af-133">Override app configuration using the Azure Portal</span></span>

<span data-ttu-id="852af-134">Параметры приложений на портале Azure позволяют задать переменные среды для приложения.</span><span class="sxs-lookup"><span data-stu-id="852af-134">App settings in the Azure Portal permit you to set environment variables for the app.</span></span> <span data-ttu-id="852af-135">Переменные среды могут использоваться [поставщиком конфигураций переменных среды](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="852af-135">Environment variables can be consumed by the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="852af-136">Когда вы создаете или изменяете параметр приложения на портале Azure, при нажатии кнопки **Сохранить** происходит перезапуск приложения Azure.</span><span class="sxs-lookup"><span data-stu-id="852af-136">When an app setting is created or modified in the Azure Portal and the **Save** button is selected, the Azure App is restarted.</span></span> <span data-ttu-id="852af-137">Переменная среды доступна в приложении после перезапуска службы.</span><span class="sxs-lookup"><span data-stu-id="852af-137">The environment variable is available to the app after the service restarts.</span></span>

<span data-ttu-id="852af-138">Если приложение использует [Веб-узел](xref:fundamentals/host/web-host) и создает узел с помощью [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), переменные среды, которые настраивают узел, используют префикс `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="852af-138">When an app uses the [Web Host](xref:fundamentals/host/web-host) and builds the host using [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), environment variables that configure the host use the `ASPNETCORE_` prefix.</span></span> <span data-ttu-id="852af-139">Дополнительные сведения см. в разделах <xref:fundamentals/host/web-host> и [Конфигурация для разных сред](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="852af-139">For more information, see <xref:fundamentals/host/web-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="852af-140">Если приложение использует [универсальный узел](xref:fundamentals/host/generic-host), переменные среды не загружаются в конфигурацию приложения по умолчанию и поставщик конфигурации должен быть добавлен разработчиком.</span><span class="sxs-lookup"><span data-stu-id="852af-140">When an app uses the [Generic Host](xref:fundamentals/host/generic-host), environment variables aren't loaded into an app's configuration by default and the configuration provider must be added by the developer.</span></span> <span data-ttu-id="852af-141">Разработчик определяет префикс переменной среды при добавлении поставщика конфигурации.</span><span class="sxs-lookup"><span data-stu-id="852af-141">The developer determines the environment variable prefix when the configuration provider is added.</span></span> <span data-ttu-id="852af-142">Дополнительные сведения см. в разделах <xref:fundamentals/host/generic-host> и [Конфигурация для разных сред](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="852af-142">For more information, see <xref:fundamentals/host/generic-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="852af-143">Сценарии использования прокси-сервера и подсистемы балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="852af-143">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="852af-144">[ПО промежуточного слоя для интеграции IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components), которое настраивает ПО промежуточного слоя переадресации заголовков при размещении [вне процесса](xref:fundamentals/servers/index#out-of-process-hosting-model), и модуль ASP.NET Core настраиваются на пересылку схемы (HTTP/HTTPS) и удаленного IP-адреса расположения, где был сформирован запрос.</span><span class="sxs-lookup"><span data-stu-id="852af-144">The [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components), which configures Forwarded Headers Middleware when hosting [out-of-process](xref:fundamentals/servers/index#out-of-process-hosting-model), and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="852af-145">Для приложений, размещенных за дополнительными прокси-серверами и подсистемами балансировки нагрузки, может потребоваться дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="852af-145">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="852af-146">Дополнительные сведения см. в разделе [Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="852af-146">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="852af-147">Мониторинг и ведение журналов</span><span class="sxs-lookup"><span data-stu-id="852af-147">Monitoring and logging</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="852af-148">Приложения ASP.NET Core, развернутые в Службе приложений, автоматически получают расширение Службы приложений для **интеграции ведения журналов ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="852af-148">ASP.NET Core apps deployed to App Service automatically receive an App Service extension, **ASP.NET Core Logging Integration**.</span></span> <span data-ttu-id="852af-149">Это расширение обеспечивает интеграцию ведения журналов для приложений ASP.NET Core, развернутых в Службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="852af-149">The extension enables logging integration for ASP.NET Core apps on Azure App Service.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="852af-150">Приложения ASP.NET Core, развернутые в Службе приложений автоматически, получают расширение службы приложений и расширения для **ведения журналов ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="852af-150">ASP.NET Core apps deployed to App Service automatically receive an App Service extension, **ASP.NET Core Logging Extensions**.</span></span> <span data-ttu-id="852af-151">Это расширение обеспечивает интеграцию ведения журналов для приложений ASP.NET Core, развернутых в Службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="852af-151">The extension enables logging integration for ASP.NET Core apps on Azure App Service.</span></span>

::: moniker-end

<span data-ttu-id="852af-152">Сведения о мониторинге, ведении журналов, а также поиске и устранении неполадок см. в следующих статьях.</span><span class="sxs-lookup"><span data-stu-id="852af-152">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="852af-153">Мониторинг приложений в Службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="852af-153">Monitor apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="852af-154">Сведения о том, как толковать квоты и параметры для приложений и планы службы приложений.</span><span class="sxs-lookup"><span data-stu-id="852af-154">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="852af-155">Включение функции ведения журналов диагностики для приложений в Службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="852af-155">Enable diagnostics logging for apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="852af-156">Сведения о том, как включать и где искать функцию ведения журнала диагностики для кодов статуса HTTP, невыполненных запросов и активности веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="852af-156">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

<xref:fundamentals/error-handling>  
<span data-ttu-id="852af-157">Сведения об основных методах обработки ошибок в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="852af-157">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

<xref:host-and-deploy/azure-apps/troubleshoot>  
<span data-ttu-id="852af-158">Сведения о диагностике проблем с развертыванием приложений ASP.NET Core в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="852af-158">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

<xref:host-and-deploy/azure-iis-errors-reference>  
<span data-ttu-id="852af-159">См. распространенные ошибки с конфигурацией развертывания для приложений, размещенных в службе приложений Azure/IIS, и рекомендации по их устранению.</span><span class="sxs-lookup"><span data-stu-id="852af-159">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="852af-160">Связка ключей для защиты данных и слоты развертывания</span><span class="sxs-lookup"><span data-stu-id="852af-160">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="852af-161">[Ключи для защиты данных](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) хранятся в папке *%HOME%\ASP.NET\DataProtection-Keys*.</span><span class="sxs-lookup"><span data-stu-id="852af-161">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="852af-162">Эта папка копируется в сетевое хранилище и синхронизируется на всех машинах, где размещается приложение.</span><span class="sxs-lookup"><span data-stu-id="852af-162">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="852af-163">Во время хранения ключи не защищаются.</span><span class="sxs-lookup"><span data-stu-id="852af-163">Keys aren't protected at rest.</span></span> <span data-ttu-id="852af-164">В этой папке хранится связка ключей для всех экземпляров приложения в одном и том же слоте развертывания.</span><span class="sxs-lookup"><span data-stu-id="852af-164">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="852af-165">Отдельные слоты развертывания, такие как промежуточное хранение и производство, не используют общую связку ключей.</span><span class="sxs-lookup"><span data-stu-id="852af-165">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="852af-166">При переключении между разными слотами развертывания ни одна система с использованием защиты данных не сможет расшифровать хранимые данные, используя связку ключей из предыдущего слота.</span><span class="sxs-lookup"><span data-stu-id="852af-166">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="852af-167">Для защиты файлов cookie ПО промежуточного слоя ASP.NET Cookie использует защиту данных.</span><span class="sxs-lookup"><span data-stu-id="852af-167">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="852af-168">В результате пользователей, применяющих ПО промежуточного слоя ASP.NET Cookie, выбрасывает из приложения.</span><span class="sxs-lookup"><span data-stu-id="852af-168">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="852af-169">Для того чтобы решение связки ключей не зависело от слота, используйте внешнего поставщика связки ключей, например:</span><span class="sxs-lookup"><span data-stu-id="852af-169">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="852af-170">Хранилище больших двоичных объектов Azure;</span><span class="sxs-lookup"><span data-stu-id="852af-170">Azure Blob Storage</span></span>
* <span data-ttu-id="852af-171">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="852af-171">Azure Key Vault</span></span>
* <span data-ttu-id="852af-172">Хранилище SQL;</span><span class="sxs-lookup"><span data-stu-id="852af-172">SQL store</span></span>
* <span data-ttu-id="852af-173">Кэш Redis.</span><span class="sxs-lookup"><span data-stu-id="852af-173">Redis cache</span></span>

<span data-ttu-id="852af-174">Для получения дополнительной информации см. <xref:security/data-protection/implementation/key-storage-providers>.</span><span class="sxs-lookup"><span data-stu-id="852af-174">For more information, see <xref:security/data-protection/implementation/key-storage-providers>.</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="852af-175">Развертывание предварительной версии ASP.NET Core в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="852af-175">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="852af-176">Воспользуйтесь одним из перечисленных ниже подходов.</span><span class="sxs-lookup"><span data-stu-id="852af-176">Use one of the following approaches:</span></span>

* <span data-ttu-id="852af-177">[Установка расширения сайта предварительной версии](#install-the-preview-site-extension).</span><span class="sxs-lookup"><span data-stu-id="852af-177">[Install the preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="852af-178">[Автономное развертывание приложения](#deploy-the-app-self-contained).</span><span class="sxs-lookup"><span data-stu-id="852af-178">[Deploy the app self-contained](#deploy-the-app-self-contained).</span></span>
* <span data-ttu-id="852af-179">[Использование Docker с веб-приложениями для контейнеров](#use-docker-with-web-apps-for-containers).</span><span class="sxs-lookup"><span data-stu-id="852af-179">[Use Docker with Web Apps for containers](#use-docker-with-web-apps-for-containers).</span></span>

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="852af-180">Установка расширения сайта предварительной версии</span><span class="sxs-lookup"><span data-stu-id="852af-180">Install the preview site extension</span></span>

<span data-ttu-id="852af-181">Если у вас возникли проблемы при использовании расширения сайта предварительной версии, создайте запрос [aspnet/AspNetCore](https://github.com/aspnet/AspNetCore/issues).</span><span class="sxs-lookup"><span data-stu-id="852af-181">If a problem occurs using the preview site extension, open an [aspnet/AspNetCore issue](https://github.com/aspnet/AspNetCore/issues).</span></span>

1. <span data-ttu-id="852af-182">Перейдите в Службу приложений на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="852af-182">From the Azure Portal, navigate to the App Service.</span></span>
1. <span data-ttu-id="852af-183">Выберите веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="852af-183">Select the web app.</span></span>
1. <span data-ttu-id="852af-184">В поле поиска введите "ex" для фильтрации расширений ("Extensions") или прокрутите вниз список инструментов управления.</span><span class="sxs-lookup"><span data-stu-id="852af-184">Type "ex" in the search box to filter for "Extensions" or scroll down the list of management tools.</span></span>
1. <span data-ttu-id="852af-185">Выберите **Расширения**.</span><span class="sxs-lookup"><span data-stu-id="852af-185">Select **Extensions**.</span></span>
1. <span data-ttu-id="852af-186">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="852af-186">Select **Add**.</span></span>
1. <span data-ttu-id="852af-187">Выберите в списке расширение **Среда выполнения ASP.NET Core {X.Y} ({x64|x86})**, где `{X.Y}` — это предварительная версия ASP.NET Core, а `{x64|x86}` — платформа.</span><span class="sxs-lookup"><span data-stu-id="852af-187">Select the **ASP.NET Core {X.Y} ({x64|x86}) Runtime** extension from the list, where `{X.Y}` is the ASP.NET Core preview version and `{x64|x86}` specifies the platform.</span></span>
1. <span data-ttu-id="852af-188">Для принятия условий нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="852af-188">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="852af-189">Нажмите **OK**, чтобы установить расширение.</span><span class="sxs-lookup"><span data-stu-id="852af-189">Select **OK** to install the extension.</span></span>

<span data-ttu-id="852af-190">По завершении операции устанавливается последняя предварительная версия .NET Core.</span><span class="sxs-lookup"><span data-stu-id="852af-190">When the operation completes, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="852af-191">Проверьте установку:</span><span class="sxs-lookup"><span data-stu-id="852af-191">Verify the installation:</span></span>

1. <span data-ttu-id="852af-192">Выберите **Дополнительные инструменты**.</span><span class="sxs-lookup"><span data-stu-id="852af-192">Select **Advanced Tools**.</span></span>
1. <span data-ttu-id="852af-193">В разделе **Дополнительные инструменты** выберите **Перейти**.</span><span class="sxs-lookup"><span data-stu-id="852af-193">Select **Go** in **Advanced Tools**.</span></span>
1. <span data-ttu-id="852af-194">Выберите пункт меню **Консоль отладки** > **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="852af-194">Select the **Debug console** > **PowerShell** menu item.</span></span>
1. <span data-ttu-id="852af-195">По запросу PowerShell выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="852af-195">At the PowerShell prompt, execute the following command.</span></span> <span data-ttu-id="852af-196">В следующей команде вместо `{X.Y}` укажите версию среды выполнения ASP.NET Core, а вместо `{PLATFORM}` — платформу:</span><span class="sxs-lookup"><span data-stu-id="852af-196">Substitute the ASP.NET Core runtime version for `{X.Y}` and the platform for `{PLATFORM}` in the command:</span></span>

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.{PLATFORM}\
   ```

   <span data-ttu-id="852af-197">Эта команда возвращает `True`, если установлена предварительная версия среды выполнения x64.</span><span class="sxs-lookup"><span data-stu-id="852af-197">The command returns `True` when the x64 preview runtime is installed.</span></span>

> [!NOTE]
> <span data-ttu-id="852af-198">Если приложение Службы приложений размещено в среде вычислений серии A или на более высоком уровне, архитектура платформы приложения (x86/x64) задается в его параметрах на портале для приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="852af-198">The platform architecture (x86/x64) of an App Services app is set in the app's settings in the Azure Portal for apps that are hosted on an A-series compute or better hosting tier.</span></span> <span data-ttu-id="852af-199">Если приложение выполняется во внутрипроцессном режиме, а архитектура платформы настроена для 64-разрядных версий (x64), модуль ASP.NET Core использует 64-разрядную предварительную версию среды выполнения при ее наличии.</span><span class="sxs-lookup"><span data-stu-id="852af-199">If the app is run in in-process mode and the platform architecture is configured for 64-bit (x64), the ASP.NET Core Module uses the 64-bit preview runtime, if present.</span></span> <span data-ttu-id="852af-200">Установите расширение **Среда выполнения ASP.NET Core {X.Y} (x64)**.</span><span class="sxs-lookup"><span data-stu-id="852af-200">Install the **ASP.NET Core {X.Y} (x64) Runtime** extension.</span></span>
>
> <span data-ttu-id="852af-201">После установки предварительной версии среды выполнения x64 выполните следующую команду в командном окне Kudu PowerShell, чтобы проверить установку.</span><span class="sxs-lookup"><span data-stu-id="852af-201">After installing the x64 preview runtime, run the following command in the Kudu PowerShell command window to verify the installation.</span></span> <span data-ttu-id="852af-202">Замените версию среды выполнения ASP.NET Core на `{X.Y}` в команде:</span><span class="sxs-lookup"><span data-stu-id="852af-202">Substitute the ASP.NET Core runtime version for `{X.Y}` in the command:</span></span>
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64\
> ```
>
> <span data-ttu-id="852af-203">Эта команда возвращает `True`, если установлена предварительная версия среды выполнения x64.</span><span class="sxs-lookup"><span data-stu-id="852af-203">The command returns `True` when the x64 preview runtime is installed.</span></span>

> [!NOTE]
> <span data-ttu-id="852af-204">**Расширения ASP.NET Core** включают дополнительные функциональные возможности для ASP.NET Core в службах приложений Azure, например включение ведения журналов Azure.</span><span class="sxs-lookup"><span data-stu-id="852af-204">**ASP.NET Core Extensions** enables additional functionality for ASP.NET Core on Azure App Services, such as enabling Azure logging.</span></span> <span data-ttu-id="852af-205">Расширение устанавливается автоматически при развертывании из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="852af-205">The extension is installed automatically when deploying from Visual Studio.</span></span> <span data-ttu-id="852af-206">Если расширение не установлено, установите его для приложения.</span><span class="sxs-lookup"><span data-stu-id="852af-206">If the extension isn't installed, install it for the app.</span></span>

<span data-ttu-id="852af-207">**Использование расширения сайта предварительной версии с шаблоном ARM**</span><span class="sxs-lookup"><span data-stu-id="852af-207">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="852af-208">Если вы используете шаблон ARM для создания и развертывания приложений, можно использовать тип ресурса `siteextensions`, чтобы добавить расширение сайта в веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="852af-208">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="852af-209">Например:</span><span class="sxs-lookup"><span data-stu-id="852af-209">For example:</span></span>

[!code-json[](index/sample/arm.json?highlight=2)]

### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="852af-210">Автономное развертывание приложения</span><span class="sxs-lookup"><span data-stu-id="852af-210">Deploy the app self-contained</span></span>

<span data-ttu-id="852af-211">Объект [автономного развертывания (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd), который предназначен для предварительной версии среды выполнения, включает в развертывание среду выполнения предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="852af-211">A [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) that targets a preview runtime carries the preview runtime in the deployment.</span></span>

<span data-ttu-id="852af-212">При развертывании автономного приложения:</span><span class="sxs-lookup"><span data-stu-id="852af-212">When deploying a self-contained app:</span></span>

* <span data-ttu-id="852af-213">Сайт в Службе приложений Azure не требует [предварительной версии расширения сайта](#install-the-preview-site-extension).</span><span class="sxs-lookup"><span data-stu-id="852af-213">The site in Azure App Service doesn't require the [preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="852af-214">Для публикации приложений следует использовать другой подход, нежели при публикации [зависящего от платформы развертывания (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="852af-214">The app must be published following a different approach than when publishing for a [framework-dependent deployment (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd).</span></span>

#### <a name="publish-from-visual-studio"></a><span data-ttu-id="852af-215">Публикация из Visual Studio</span><span class="sxs-lookup"><span data-stu-id="852af-215">Publish from Visual Studio</span></span>

1. <span data-ttu-id="852af-216">Выберите **Сборка** > **Опубликовать {имя_приложения}** на панели инструментов Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="852af-216">Select **Build** > **Publish {Application Name}** from the Visual Studio toolbar.</span></span>
1. <span data-ttu-id="852af-217">В диалоговом оке **Выберите целевой объект публикации** убедитесь, что выбран вариант **Служба приложений**.</span><span class="sxs-lookup"><span data-stu-id="852af-217">In the **Pick a publish target** dialog, confirm that **App Service** is selected.</span></span>
1. <span data-ttu-id="852af-218">Выберите **Дополнительно**.</span><span class="sxs-lookup"><span data-stu-id="852af-218">Select **Advanced**.</span></span> <span data-ttu-id="852af-219">Откроется диалоговое окно **Публикация**.</span><span class="sxs-lookup"><span data-stu-id="852af-219">The **Publish** dialog opens.</span></span>
1. <span data-ttu-id="852af-220">В диалоговом окне **Публикация**:</span><span class="sxs-lookup"><span data-stu-id="852af-220">In the **Publish** dialog:</span></span>
   * <span data-ttu-id="852af-221">Убедитесь, что выбрана конфигурация **Выпуск**.</span><span class="sxs-lookup"><span data-stu-id="852af-221">Confirm that the **Release** configuration is selected.</span></span>
   * <span data-ttu-id="852af-222">Откройте раскрывающийся список **Режим развертывания** и выберите вариант **Автономное**.</span><span class="sxs-lookup"><span data-stu-id="852af-222">Open the **Deployment Mode** drop-down list and select **Self-Contained**.</span></span>
   * <span data-ttu-id="852af-223">Выберите целевую среду выполнения из раскрывающегося списка **Целевая среда выполнения**.</span><span class="sxs-lookup"><span data-stu-id="852af-223">Select the target runtime from the **Target Runtime** drop-down list.</span></span> <span data-ttu-id="852af-224">Значение по умолчанию — `win-x86`.</span><span class="sxs-lookup"><span data-stu-id="852af-224">The default is `win-x86`.</span></span>
   * <span data-ttu-id="852af-225">Если потребуется удалить дополнительные файлы после развертывания, откройте **Параметры публикации файлов** и установите флажок для удаления дополнительных файлов в месте назначения.</span><span class="sxs-lookup"><span data-stu-id="852af-225">If you need to remove additional files upon deployment, open **File Publish Options** and select the check box to remove additional files at the destination.</span></span>
   * <span data-ttu-id="852af-226">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="852af-226">Select **Save**.</span></span>
1. <span data-ttu-id="852af-227">Создайте новый сайт или обновите существующий, следуя остальным подсказкам мастера публикации.</span><span class="sxs-lookup"><span data-stu-id="852af-227">Create a new site or update an existing site by following the remaining prompts of the publish wizard.</span></span>

#### <a name="publish-using-command-line-interface-cli-tools"></a><span data-ttu-id="852af-228">Публикация с помощью средств интерфейса командной строки</span><span class="sxs-lookup"><span data-stu-id="852af-228">Publish using command-line interface (CLI) tools</span></span>

1. <span data-ttu-id="852af-229">В файле проекта укажите один или несколько [идентификаторов сред выполнения (RID)](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="852af-229">In the project file, specify one or more [Runtime Identifiers (RIDs)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="852af-230">Используйте `<RuntimeIdentifier>` (в единственном числе) для одного RID или `<RuntimeIdentifiers>` (во множественном числе), чтобы предоставить разделенный точкой с запятой список идентификаторов RID.</span><span class="sxs-lookup"><span data-stu-id="852af-230">Use `<RuntimeIdentifier>` (singular) for a single RID, or use `<RuntimeIdentifiers>` (plural) to provide a semicolon-delimited list of RIDs.</span></span> <span data-ttu-id="852af-231">В следующем примере указан RID `win-x86`:</span><span class="sxs-lookup"><span data-stu-id="852af-231">In the following example, the `win-x86` RID is specified:</span></span>

   ```xml
   <PropertyGroup>
     <TargetFramework>netcoreapp2.1</TargetFramework>
     <RuntimeIdentifier>win-x86</RuntimeIdentifier>
   </PropertyGroup>
   ```

1. <span data-ttu-id="852af-232">Из командной оболочки опубликуйте приложение в конфигурации выпуска для среды выполнения узла, выполнив команду [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="852af-232">From a command shell, publish the app in Release configuration for the host's runtime with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="852af-233">В следующем примере публикуется приложение с RID `win-x86`.</span><span class="sxs-lookup"><span data-stu-id="852af-233">In the following example, the app is published for the `win-x86` RID.</span></span> <span data-ttu-id="852af-234">Предоставленный в параметре `--runtime` RID должены быть указан в свойстве `<RuntimeIdentifier>` (или `<RuntimeIdentifiers>`) в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="852af-234">The RID supplied to the `--runtime` option must be provided in the `<RuntimeIdentifier>` (or `<RuntimeIdentifiers>`) property in the project file.</span></span>

   ```console
   dotnet publish --configuration Release --runtime win-x86
   ```

1. <span data-ttu-id="852af-235">Переместите содержимое каталога *bin/Release/{целевая_платформа}/{идентификатор_среды_выполнения}/publish* на сайт в Службе приложений.</span><span class="sxs-lookup"><span data-stu-id="852af-235">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* directory to the site in App Service.</span></span>

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="852af-236">Использование Docker с веб-приложениями для контейнеров</span><span class="sxs-lookup"><span data-stu-id="852af-236">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="852af-237">[Центр Docker](https://hub.docker.com/r/microsoft/aspnetcore/) содержит образы Docker из последней предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="852af-237">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="852af-238">Их можно использовать в качестве базового образа.</span><span class="sxs-lookup"><span data-stu-id="852af-238">The images can be used as a base image.</span></span> <span data-ttu-id="852af-239">Использование образа и развертывание в веб-приложениях для контейнеров выполняется как обычно.</span><span class="sxs-lookup"><span data-stu-id="852af-239">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="protocol-settings-https"></a><span data-ttu-id="852af-240">Параметры протокола (HTTPS)</span><span class="sxs-lookup"><span data-stu-id="852af-240">Protocol settings (HTTPS)</span></span>

<span data-ttu-id="852af-241">Привязки безопасных протоколов позволяют указать сертификат, который следует использовать при ответе на запросы по HTTPS.</span><span class="sxs-lookup"><span data-stu-id="852af-241">Secure protocol bindings allow you specify a certificate to use when responding to requests over HTTPS.</span></span> <span data-ttu-id="852af-242">Для привязки требуется допустимый закрытый сертификат (*PFX*), выданный для определенного имени узла.</span><span class="sxs-lookup"><span data-stu-id="852af-242">Binding requires a valid private certificate (*.pfx*) issued for the specific hostname.</span></span> <span data-ttu-id="852af-243">Дополнительные сведения см. в статье [Руководство. Привязывание существующего настраиваемого SSL-сертификата к Службе приложений Azure](/azure/app-service/app-service-web-tutorial-custom-ssl).</span><span class="sxs-lookup"><span data-stu-id="852af-243">For more information, see [Tutorial: Bind an existing custom SSL certificate to Azure App Service](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

## <a name="transform-webconfig"></a><span data-ttu-id="852af-244">Преобразование web.config</span><span class="sxs-lookup"><span data-stu-id="852af-244">Transform web.config</span></span>

<span data-ttu-id="852af-245">Если вам нужно преобразовать *web.config* при публикации (например, задать переменные среды на основе конфигурации, профиля или среды), см. раздел <xref:host-and-deploy/iis/transform-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="852af-245">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="852af-246">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="852af-246">Additional resources</span></span>

* [<span data-ttu-id="852af-247">Обзор Службы приложений</span><span class="sxs-lookup"><span data-stu-id="852af-247">App Service overview</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="852af-248">Служба приложений Azure: оптимальное место для размещения приложений .NET (55-минутное видео)</span><span class="sxs-lookup"><span data-stu-id="852af-248">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="852af-249">Пятница с Azure. Практика диагностики и устранения неполадок в Службе приложений Azure (12-минутное видео)</span><span class="sxs-lookup"><span data-stu-id="852af-249">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="852af-250">Общие сведения о диагностике в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="852af-250">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="852af-251">Служба приложений Azure на Windows Server использует [службы IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="852af-251">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="852af-252">Технологии IIS посвящены следующие статьи.</span><span class="sxs-lookup"><span data-stu-id="852af-252">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="852af-253">Windows Server — содержимое для ИТ-администраторов по текущим и предыдущим выпускам</span><span class="sxs-lookup"><span data-stu-id="852af-253">Windows Server - IT administrator content for current and previous releases</span></span>](/windows-server/windows-server-versions)
