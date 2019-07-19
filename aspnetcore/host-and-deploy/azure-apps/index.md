---
title: Развертывание приложений ASP.NET Core в Службе приложений Azure
author: guardrex
description: Эта статья содержит ссылки на ресурсы по размещению и развертыванию в Azure.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/16/2019
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: bbdb3e92b6b8afb44d9c0c95c240002c7b7c17db
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308146"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a><span data-ttu-id="87a8d-103">Развертывание приложений ASP.NET Core в Службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="87a8d-103">Deploy ASP.NET Core apps to Azure App Service</span></span>

<span data-ttu-id="87a8d-104">[Служба приложений Azure](https://azure.microsoft.com/services/app-service/) — это [платформа облачных вычислений Microsoft](https://azure.microsoft.com/), предназначенная для размещения веб-приложений, включая ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="87a8d-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="87a8d-105">Полезные ресурсы</span><span class="sxs-lookup"><span data-stu-id="87a8d-105">Useful resources</span></span>

<span data-ttu-id="87a8d-106">[Документация по службе приложений](/azure/app-service/) — это место, где хранятся документация, учебники, примеры, руководства и другие ресурсы, связанные с приложениями Azure.</span><span class="sxs-lookup"><span data-stu-id="87a8d-106">[App Service Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="87a8d-107">Размещению приложений ASP.NET Core посвящены следующие два руководства.</span><span class="sxs-lookup"><span data-stu-id="87a8d-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="87a8d-108">Создание веб-приложения ASP.NET Core в Azure</span><span class="sxs-lookup"><span data-stu-id="87a8d-108">Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="87a8d-109">Создайте веб-приложение ASP.NET Core и разверните его в службе приложений Azure на базе Windows с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="87a8d-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="87a8d-110">Создание приложения ASP.NET Core в Службе приложений в Linux</span><span class="sxs-lookup"><span data-stu-id="87a8d-110">Create an ASP.NET Core app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="87a8d-111">Создайте веб-приложение ASP.NET Core и разверните его в службе приложений Azure на базе Linux с помощью командной строки.</span><span class="sxs-lookup"><span data-stu-id="87a8d-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="87a8d-112">Следующие статьи входят в документацию по ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="87a8d-112">The following articles are available in ASP.NET Core documentation:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>  
<span data-ttu-id="87a8d-113">Сведения о публикации приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="87a8d-113">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

<xref:host-and-deploy/azure-apps/azure-continuous-deployment>  
<span data-ttu-id="87a8d-114">Сведения о создании веб-приложения ASP.NET Core с помощью Visual Studio и его развертывании в службе приложений Azure с использованием Git для непрерывного развертывания.</span><span class="sxs-lookup"><span data-stu-id="87a8d-114">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="87a8d-115">Создание первого конвейера</span><span class="sxs-lookup"><span data-stu-id="87a8d-115">Create your first pipeline</span></span>](/azure/devops/pipelines/get-started-yaml)  
<span data-ttu-id="87a8d-116">Сведения о настройке сборки CI для приложения ASP.NET Core и последующем создании выпуска непрерывного развертывания в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="87a8d-116">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="87a8d-117">Песочница веб-приложений Azure</span><span class="sxs-lookup"><span data-stu-id="87a8d-117">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="87a8d-118">Сведения об ограничениях среды выполнения службы приложений Azure, создаваемых платформой приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="87a8d-118">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="87a8d-119">Настройка приложения</span><span class="sxs-lookup"><span data-stu-id="87a8d-119">Application configuration</span></span>

### <a name="platform"></a><span data-ttu-id="87a8d-120">Platform</span><span class="sxs-lookup"><span data-stu-id="87a8d-120">Platform</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="87a8d-121">Среды выполнения для 64-разрядных (x64) и 32-разрядных (x86) приложений находятся в Службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="87a8d-121">Runtimes for 64-bit (x64) and 32-bit (x86) apps are present on Azure App Service.</span></span> <span data-ttu-id="87a8d-122">[Пакет SDK для .NET Core](/dotnet/core/sdk), доступный в Службе приложений, является 32-разрядным, но вы можете развертывать созданные локально 64-разрядные приложения с помощью консоли [Kudu](https://github.com/projectkudu/kudu/wiki) или процесса публикации в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="87a8d-122">The [.NET Core SDK](/dotnet/core/sdk) available on App Service is 32-bit, but you can deploy 64-bit apps built locally using the [Kudu](https://github.com/projectkudu/kudu/wiki) console or the publish process in Visual Studio.</span></span> <span data-ttu-id="87a8d-123">Дополнительные сведения см. в разделе [Публикация и развертывание приложения](#publish-and-deploy-the-app).</span><span class="sxs-lookup"><span data-stu-id="87a8d-123">For more information, see the [Publish and deploy the app](#publish-and-deploy-the-app) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="87a8d-124">Среды выполнения для 32-разрядных (x86) приложений с собственными зависимостями находятся в Службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="87a8d-124">For apps with native dependencies, runtimes for 32-bit (x86) apps are present on Azure App Service.</span></span> <span data-ttu-id="87a8d-125">[Пакет SDK для .NET Core](/dotnet/core/sdk), доступный в Службе приложений, является 32-разрядным.</span><span class="sxs-lookup"><span data-stu-id="87a8d-125">The [.NET Core SDK](/dotnet/core/sdk) available on App Service is 32-bit.</span></span>

::: moniker-end

<span data-ttu-id="87a8d-126">Дополнительные сведения о компонентах платформы .NET Core и методах распространения, например сведения о среде выполнения .NET Core и пакете SDK для .NET Core, см. в разделе ["Композиция" статьи сведений о .NET Core](/dotnet/core/about#composition).</span><span class="sxs-lookup"><span data-stu-id="87a8d-126">For more information on .NET Core framework components and distribution methods, such as information on the .NET Core runtime and the .NET Core SDK, see [About .NET Core: Composition](/dotnet/core/about#composition).</span></span>

### <a name="packages"></a><span data-ttu-id="87a8d-127">Пакеты</span><span class="sxs-lookup"><span data-stu-id="87a8d-127">Packages</span></span>

<span data-ttu-id="87a8d-128">Включите следующие пакеты NuGet, чтобы использовать возможности автоматического ведения журнала для приложений, развернутых в Службе приложений Azure:</span><span class="sxs-lookup"><span data-stu-id="87a8d-128">Include the following NuGet packages to provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="87a8d-129">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) использует [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) для интеграции подсветки ASP.NET Core в службу приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="87a8d-129">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="87a8d-130">Пакет `Microsoft.AspNetCore.AzureAppServicesIntegration` включает дополнительные функции для ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="87a8d-130">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="87a8d-131">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) выполняет [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) для добавления поставщиков ведения журналов диагностики из службы приложений Azure в пакет `Microsoft.Extensions.Logging.AzureAppServices`.</span><span class="sxs-lookup"><span data-stu-id="87a8d-131">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="87a8d-132">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) предоставляет реализации средства ведения журналов в поддержку журналов диагностики службы приложений Azure и функций потоковой передачи журналов.</span><span class="sxs-lookup"><span data-stu-id="87a8d-132">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="87a8d-133">Предыдущие пакеты недоступны в [метапакете Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="87a8d-133">The preceding packages aren't available from the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="87a8d-134">Приложения, предназначенные для .NET Framework или ссылающиеся на метапакет `Microsoft.AspNetCore.App`, должны явно ссылаться на отдельные пакеты в файле проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="87a8d-134">Apps that target .NET Framework or reference the `Microsoft.AspNetCore.App` metapackage must explicitly reference the individual packages in the app's project file.</span></span>

## <a name="override-app-configuration-using-the-azure-portal"></a><span data-ttu-id="87a8d-135">Переопределение конфигурации приложения с помощью портала Azure</span><span class="sxs-lookup"><span data-stu-id="87a8d-135">Override app configuration using the Azure Portal</span></span>

<span data-ttu-id="87a8d-136">Параметры приложений на портале Azure позволяют задать переменные среды для приложения.</span><span class="sxs-lookup"><span data-stu-id="87a8d-136">App settings in the Azure Portal permit you to set environment variables for the app.</span></span> <span data-ttu-id="87a8d-137">Переменные среды могут использоваться [поставщиком конфигураций переменных среды](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="87a8d-137">Environment variables can be consumed by the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="87a8d-138">Когда вы создаете или изменяете параметр приложения на портале Azure, при нажатии кнопки **Сохранить** происходит перезапуск приложения Azure.</span><span class="sxs-lookup"><span data-stu-id="87a8d-138">When an app setting is created or modified in the Azure Portal and the **Save** button is selected, the Azure App is restarted.</span></span> <span data-ttu-id="87a8d-139">Переменная среды доступна в приложении после перезапуска службы.</span><span class="sxs-lookup"><span data-stu-id="87a8d-139">The environment variable is available to the app after the service restarts.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="87a8d-140">Если приложение использует [универсальный узел](xref:fundamentals/host/generic-host), переменные среды не загружаются в конфигурацию приложения по умолчанию и поставщик конфигурации должен быть добавлен разработчиком.</span><span class="sxs-lookup"><span data-stu-id="87a8d-140">When an app uses the [Generic Host](xref:fundamentals/host/generic-host), environment variables aren't loaded into an app's configuration by default and the configuration provider must be added by the developer.</span></span> <span data-ttu-id="87a8d-141">Разработчик определяет префикс переменной среды при добавлении поставщика конфигурации.</span><span class="sxs-lookup"><span data-stu-id="87a8d-141">The developer determines the environment variable prefix when the configuration provider is added.</span></span> <span data-ttu-id="87a8d-142">Дополнительные сведения см. в разделах <xref:fundamentals/host/generic-host> и [Конфигурация для разных сред](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="87a8d-142">For more information, see <xref:fundamentals/host/generic-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="87a8d-143">Если приложение создает узел с помощью [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), переменные среды, которые настраивают узел, используют префикс `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="87a8d-143">When an app builds the host using [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), environment variables that configure the host use the `ASPNETCORE_` prefix.</span></span> <span data-ttu-id="87a8d-144">Дополнительные сведения см. в разделах <xref:fundamentals/host/web-host> и [Конфигурация для разных сред](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="87a8d-144">For more information, see <xref:fundamentals/host/web-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="87a8d-145">Сценарии использования прокси-сервера и подсистемы балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="87a8d-145">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="87a8d-146">[ПО промежуточного слоя для интеграции IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components), которое настраивает ПО промежуточного слоя переадресации заголовков при размещении [вне процесса](xref:host-and-deploy/iis/index#out-of-process-hosting-model), и модуль ASP.NET Core настраиваются на пересылку схемы (HTTP/HTTPS) и удаленного IP-адреса расположения, где был сформирован запрос.</span><span class="sxs-lookup"><span data-stu-id="87a8d-146">The [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components), which configures Forwarded Headers Middleware when hosting [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="87a8d-147">Для приложений, размещенных за дополнительными прокси-серверами и подсистемами балансировки нагрузки, может потребоваться дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="87a8d-147">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="87a8d-148">Дополнительные сведения см. в разделе [Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="87a8d-148">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="87a8d-149">Мониторинг и ведение журналов</span><span class="sxs-lookup"><span data-stu-id="87a8d-149">Monitoring and logging</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="87a8d-150">Приложения ASP.NET Core, развернутые в Службе приложений, автоматически получают расширение Службы приложений для **интеграции ведения журналов ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-150">ASP.NET Core apps deployed to App Service automatically receive an App Service extension, **ASP.NET Core Logging Integration**.</span></span> <span data-ttu-id="87a8d-151">Это расширение обеспечивает интеграцию ведения журналов для приложений ASP.NET Core, развернутых в Службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="87a8d-151">The extension enables logging integration for ASP.NET Core apps on Azure App Service.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="87a8d-152">Приложения ASP.NET Core, развернутые в Службе приложений автоматически, получают расширение службы приложений и расширения для **ведения журналов ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-152">ASP.NET Core apps deployed to App Service automatically receive an App Service extension, **ASP.NET Core Logging Extensions**.</span></span> <span data-ttu-id="87a8d-153">Это расширение обеспечивает интеграцию ведения журналов для приложений ASP.NET Core, развернутых в Службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="87a8d-153">The extension enables logging integration for ASP.NET Core apps on Azure App Service.</span></span>

::: moniker-end

<span data-ttu-id="87a8d-154">Сведения о мониторинге, ведении журналов, а также поиске и устранении неполадок см. в следующих статьях.</span><span class="sxs-lookup"><span data-stu-id="87a8d-154">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="87a8d-155">Мониторинг приложений в Службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="87a8d-155">Monitor apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="87a8d-156">Сведения о том, как толковать квоты и параметры для приложений и планы службы приложений.</span><span class="sxs-lookup"><span data-stu-id="87a8d-156">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="87a8d-157">Включение функции ведения журналов диагностики для приложений в Службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="87a8d-157">Enable diagnostics logging for apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="87a8d-158">Сведения о том, как включать и где искать функцию ведения журнала диагностики для кодов статуса HTTP, невыполненных запросов и активности веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="87a8d-158">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

<xref:fundamentals/error-handling>  
<span data-ttu-id="87a8d-159">Сведения об основных методах обработки ошибок в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="87a8d-159">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

<xref:test/troubleshoot-azure-iis>  
<span data-ttu-id="87a8d-160">Сведения о диагностике проблем с развертыванием приложений ASP.NET Core в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="87a8d-160">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

<xref:host-and-deploy/azure-iis-errors-reference>  
<span data-ttu-id="87a8d-161">См. распространенные ошибки с конфигурацией развертывания для приложений, размещенных в службе приложений Azure/IIS, и рекомендации по их устранению.</span><span class="sxs-lookup"><span data-stu-id="87a8d-161">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="87a8d-162">Связка ключей для защиты данных и слоты развертывания</span><span class="sxs-lookup"><span data-stu-id="87a8d-162">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="87a8d-163">[Ключи для защиты данных](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) хранятся в папке *%HOME%\ASP.NET\DataProtection-Keys*.</span><span class="sxs-lookup"><span data-stu-id="87a8d-163">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="87a8d-164">Эта папка копируется в сетевое хранилище и синхронизируется на всех машинах, где размещается приложение.</span><span class="sxs-lookup"><span data-stu-id="87a8d-164">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="87a8d-165">Во время хранения ключи не защищаются.</span><span class="sxs-lookup"><span data-stu-id="87a8d-165">Keys aren't protected at rest.</span></span> <span data-ttu-id="87a8d-166">В этой папке хранится связка ключей для всех экземпляров приложения в одном и том же слоте развертывания.</span><span class="sxs-lookup"><span data-stu-id="87a8d-166">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="87a8d-167">Отдельные слоты развертывания, такие как промежуточное хранение и производство, не используют общую связку ключей.</span><span class="sxs-lookup"><span data-stu-id="87a8d-167">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="87a8d-168">При переключении между разными слотами развертывания ни одна система с использованием защиты данных не сможет расшифровать хранимые данные, используя связку ключей из предыдущего слота.</span><span class="sxs-lookup"><span data-stu-id="87a8d-168">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="87a8d-169">Для защиты файлов cookie ПО промежуточного слоя ASP.NET Cookie использует защиту данных.</span><span class="sxs-lookup"><span data-stu-id="87a8d-169">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="87a8d-170">В результате пользователей, применяющих ПО промежуточного слоя ASP.NET Cookie, выбрасывает из приложения.</span><span class="sxs-lookup"><span data-stu-id="87a8d-170">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="87a8d-171">Для того чтобы решение связки ключей не зависело от слота, используйте внешнего поставщика связки ключей, например:</span><span class="sxs-lookup"><span data-stu-id="87a8d-171">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="87a8d-172">Хранилище больших двоичных объектов Azure;</span><span class="sxs-lookup"><span data-stu-id="87a8d-172">Azure Blob Storage</span></span>
* <span data-ttu-id="87a8d-173">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="87a8d-173">Azure Key Vault</span></span>
* <span data-ttu-id="87a8d-174">Хранилище SQL;</span><span class="sxs-lookup"><span data-stu-id="87a8d-174">SQL store</span></span>
* <span data-ttu-id="87a8d-175">Кэш Redis.</span><span class="sxs-lookup"><span data-stu-id="87a8d-175">Redis cache</span></span>

<span data-ttu-id="87a8d-176">Дополнительные сведения можно найти по адресу: <xref:security/data-protection/implementation/key-storage-providers>.</span><span class="sxs-lookup"><span data-stu-id="87a8d-176">For more information, see <xref:security/data-protection/implementation/key-storage-providers>.</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="87a8d-177">Развертывание предварительной версии ASP.NET Core в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="87a8d-177">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="87a8d-178">Если приложение предназначено для предварительной версии .NET Core, воспользуйтесь одним из приведенных ниже подходов.</span><span class="sxs-lookup"><span data-stu-id="87a8d-178">Use one of the following approaches if the app relies on a preview release of .NET Core:</span></span>

* <span data-ttu-id="87a8d-179">[Установка расширения сайта предварительной версии](#install-the-preview-site-extension).</span><span class="sxs-lookup"><span data-stu-id="87a8d-179">[Install the preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="87a8d-180">[Развертывание автономного приложения для предварительной версии](#deploy-a-self-contained-preview-app).</span><span class="sxs-lookup"><span data-stu-id="87a8d-180">[Deploy a self-contained preview app](#deploy-a-self-contained-preview-app).</span></span>
* <span data-ttu-id="87a8d-181">[Использование Docker с веб-приложениями для контейнеров](#use-docker-with-web-apps-for-containers).</span><span class="sxs-lookup"><span data-stu-id="87a8d-181">[Use Docker with Web Apps for containers](#use-docker-with-web-apps-for-containers).</span></span>

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="87a8d-182">Установка расширения сайта предварительной версии</span><span class="sxs-lookup"><span data-stu-id="87a8d-182">Install the preview site extension</span></span>

<span data-ttu-id="87a8d-183">Если у вас возникли проблемы при использовании расширения сайта предварительной версии, создайте запрос [aspnet/AspNetCore](https://github.com/aspnet/AspNetCore/issues).</span><span class="sxs-lookup"><span data-stu-id="87a8d-183">If a problem occurs using the preview site extension, open an [aspnet/AspNetCore issue](https://github.com/aspnet/AspNetCore/issues).</span></span>

1. <span data-ttu-id="87a8d-184">Перейдите в Службу приложений на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="87a8d-184">From the Azure Portal, navigate to the App Service.</span></span>
1. <span data-ttu-id="87a8d-185">Выберите веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="87a8d-185">Select the web app.</span></span>
1. <span data-ttu-id="87a8d-186">В поле поиска введите "ex" для фильтрации расширений ("Extensions") или прокрутите вниз список инструментов управления.</span><span class="sxs-lookup"><span data-stu-id="87a8d-186">Type "ex" in the search box to filter for "Extensions" or scroll down the list of management tools.</span></span>
1. <span data-ttu-id="87a8d-187">Выберите **Расширения**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-187">Select **Extensions**.</span></span>
1. <span data-ttu-id="87a8d-188">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-188">Select **Add**.</span></span>
1. <span data-ttu-id="87a8d-189">Выберите в списке расширение **Среда выполнения ASP.NET Core {X.Y} ({x64|x86})** , где `{X.Y}` — это предварительная версия ASP.NET Core, а `{x64|x86}` — платформа.</span><span class="sxs-lookup"><span data-stu-id="87a8d-189">Select the **ASP.NET Core {X.Y} ({x64|x86}) Runtime** extension from the list, where `{X.Y}` is the ASP.NET Core preview version and `{x64|x86}` specifies the platform.</span></span>
1. <span data-ttu-id="87a8d-190">Для принятия условий нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-190">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="87a8d-191">Нажмите **OK**, чтобы установить расширение.</span><span class="sxs-lookup"><span data-stu-id="87a8d-191">Select **OK** to install the extension.</span></span>

<span data-ttu-id="87a8d-192">По завершении операции устанавливается последняя предварительная версия .NET Core.</span><span class="sxs-lookup"><span data-stu-id="87a8d-192">When the operation completes, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="87a8d-193">Проверьте установку:</span><span class="sxs-lookup"><span data-stu-id="87a8d-193">Verify the installation:</span></span>

1. <span data-ttu-id="87a8d-194">Выберите **Дополнительные инструменты**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-194">Select **Advanced Tools**.</span></span>
1. <span data-ttu-id="87a8d-195">В разделе **Дополнительные инструменты** выберите **Перейти**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-195">Select **Go** in **Advanced Tools**.</span></span>
1. <span data-ttu-id="87a8d-196">Выберите пункт меню **Консоль отладки** > **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-196">Select the **Debug console** > **PowerShell** menu item.</span></span>
1. <span data-ttu-id="87a8d-197">По запросу PowerShell выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="87a8d-197">At the PowerShell prompt, execute the following command.</span></span> <span data-ttu-id="87a8d-198">В следующей команде вместо `{X.Y}` укажите версию среды выполнения ASP.NET Core, а вместо `{PLATFORM}` — платформу:</span><span class="sxs-lookup"><span data-stu-id="87a8d-198">Substitute the ASP.NET Core runtime version for `{X.Y}` and the platform for `{PLATFORM}` in the command:</span></span>

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.{PLATFORM}\
   ```

   <span data-ttu-id="87a8d-199">Эта команда возвращает `True`, если установлена предварительная версия среды выполнения x64.</span><span class="sxs-lookup"><span data-stu-id="87a8d-199">The command returns `True` when the x64 preview runtime is installed.</span></span>

> [!NOTE]
> <span data-ttu-id="87a8d-200">Если приложение Службы приложений размещено в среде вычислений серии A или на более высоком уровне, архитектура платформы приложения (x86/x64) задается в его параметрах на портале для приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="87a8d-200">The platform architecture (x86/x64) of an App Services app is set in the app's settings in the Azure Portal for apps that are hosted on an A-series compute or better hosting tier.</span></span> <span data-ttu-id="87a8d-201">Если приложение выполняется во внутрипроцессном режиме, а архитектура платформы настроена для 64-разрядных версий (x64), модуль ASP.NET Core использует 64-разрядную предварительную версию среды выполнения при ее наличии.</span><span class="sxs-lookup"><span data-stu-id="87a8d-201">If the app is run in in-process mode and the platform architecture is configured for 64-bit (x64), the ASP.NET Core Module uses the 64-bit preview runtime, if present.</span></span> <span data-ttu-id="87a8d-202">Установите расширение **Среда выполнения ASP.NET Core {X.Y} (x64)** .</span><span class="sxs-lookup"><span data-stu-id="87a8d-202">Install the **ASP.NET Core {X.Y} (x64) Runtime** extension.</span></span>
>
> <span data-ttu-id="87a8d-203">После установки предварительной версии среды выполнения x64 выполните следующую команду в командном окне Kudu PowerShell, чтобы проверить установку.</span><span class="sxs-lookup"><span data-stu-id="87a8d-203">After installing the x64 preview runtime, run the following command in the Kudu PowerShell command window to verify the installation.</span></span> <span data-ttu-id="87a8d-204">Замените версию среды выполнения ASP.NET Core на `{X.Y}` в команде:</span><span class="sxs-lookup"><span data-stu-id="87a8d-204">Substitute the ASP.NET Core runtime version for `{X.Y}` in the command:</span></span>
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64\
> ```
>
> <span data-ttu-id="87a8d-205">Эта команда возвращает `True`, если установлена предварительная версия среды выполнения x64.</span><span class="sxs-lookup"><span data-stu-id="87a8d-205">The command returns `True` when the x64 preview runtime is installed.</span></span>

> [!NOTE]
> <span data-ttu-id="87a8d-206">**Расширения ASP.NET Core** включают дополнительные функциональные возможности для ASP.NET Core в службах приложений Azure, например включение ведения журналов Azure.</span><span class="sxs-lookup"><span data-stu-id="87a8d-206">**ASP.NET Core Extensions** enables additional functionality for ASP.NET Core on Azure App Services, such as enabling Azure logging.</span></span> <span data-ttu-id="87a8d-207">Расширение устанавливается автоматически при развертывании из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="87a8d-207">The extension is installed automatically when deploying from Visual Studio.</span></span> <span data-ttu-id="87a8d-208">Если расширение не установлено, установите его для приложения.</span><span class="sxs-lookup"><span data-stu-id="87a8d-208">If the extension isn't installed, install it for the app.</span></span>

<span data-ttu-id="87a8d-209">**Использование расширения сайта предварительной версии с шаблоном ARM**</span><span class="sxs-lookup"><span data-stu-id="87a8d-209">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="87a8d-210">Если вы используете шаблон ARM для создания и развертывания приложений, можно использовать тип ресурса `siteextensions`, чтобы добавить расширение сайта в веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="87a8d-210">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="87a8d-211">Например:</span><span class="sxs-lookup"><span data-stu-id="87a8d-211">For example:</span></span>

[!code-json[](index/sample/arm.json?highlight=2)]

### <a name="deploy-a-self-contained-preview-app"></a><span data-ttu-id="87a8d-212">Развертывание автономного приложения для предварительной версии</span><span class="sxs-lookup"><span data-stu-id="87a8d-212">Deploy a self-contained preview app</span></span>

<span data-ttu-id="87a8d-213">Объект [автономного развертывания (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd), который предназначен для предварительной версии среды выполнения, включает в развертывание среду выполнения предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="87a8d-213">A [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) that targets a preview runtime carries the preview runtime in the deployment.</span></span>

<span data-ttu-id="87a8d-214">При развертывании автономного приложения:</span><span class="sxs-lookup"><span data-stu-id="87a8d-214">When deploying a self-contained app:</span></span>

* <span data-ttu-id="87a8d-215">Сайт в Службе приложений Azure не требует [предварительной версии расширения сайта](#install-the-preview-site-extension).</span><span class="sxs-lookup"><span data-stu-id="87a8d-215">The site in Azure App Service doesn't require the [preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="87a8d-216">Для публикации приложений следует использовать другой подход, нежели при публикации [зависящего от платформы развертывания (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="87a8d-216">The app must be published following a different approach than when publishing for a [framework-dependent deployment (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="87a8d-217">Следуйте указаниям в разделе [Развертывание автономного приложения](#deploy-the-app-self-contained).</span><span class="sxs-lookup"><span data-stu-id="87a8d-217">Follow the guidance in the [Deploy the app self-contained](#deploy-the-app-self-contained) section.</span></span>

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="87a8d-218">Использование Docker с веб-приложениями для контейнеров</span><span class="sxs-lookup"><span data-stu-id="87a8d-218">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="87a8d-219">[Центр Docker](https://hub.docker.com/r/microsoft/aspnetcore/) содержит образы Docker из последней предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="87a8d-219">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="87a8d-220">Их можно использовать в качестве базового образа.</span><span class="sxs-lookup"><span data-stu-id="87a8d-220">The images can be used as a base image.</span></span> <span data-ttu-id="87a8d-221">Использование образа и развертывание в веб-приложениях для контейнеров выполняется как обычно.</span><span class="sxs-lookup"><span data-stu-id="87a8d-221">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="87a8d-222">Публикация и развертывание приложения</span><span class="sxs-lookup"><span data-stu-id="87a8d-222">Publish and deploy the app</span></span>

### <a name="deploy-the-app-framework-dependent"></a><span data-ttu-id="87a8d-223">Развертывание приложения, зависимого от платформы</span><span class="sxs-lookup"><span data-stu-id="87a8d-223">Deploy the app framework-dependent</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="87a8d-224">64-разрядное [развертывание, зависимое от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd)</span><span class="sxs-lookup"><span data-stu-id="87a8d-224">For a 64-bit [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

* <span data-ttu-id="87a8d-225">Для создания 64-разрядной версии приложения используйте 64-разрядную версию пакета SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="87a8d-225">Use a 64-bit .NET Core SDK to build a 64-bit app.</span></span>
* <span data-ttu-id="87a8d-226">В разделе **Конфигурация** > **Общие параметры** службы приложений выберите для параметра **Платформа** значение **64-разрядная версия**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-226">Set the **Platform** to **64 Bit** in the App Service's **Configuration** > **General settings**.</span></span> <span data-ttu-id="87a8d-227">Чтобы включить возможность выбора разрядности платформы, приложение должно использовать план обслуживания "Базовый" или более высокий.</span><span class="sxs-lookup"><span data-stu-id="87a8d-227">The app must use a Basic or higher service plan to enable the choice of platform bitness.</span></span>

::: moniker-end

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="87a8d-228">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="87a8d-228">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="87a8d-229">Выберите **Сборка** > **Опубликовать {имя_приложения}** на панели инструментов Visual Studio или щелкните правой кнопкой мыши проект в **обозревателе решений** и выберите пункт **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-229">Select **Build** > **Publish {Application Name}** from the Visual Studio toolbar or right-click the project in **Solution Explorer** and select **Publish**.</span></span>
1. <span data-ttu-id="87a8d-230">В диалоговом оке **Выберите целевой объект публикации** убедитесь, что выбран вариант **Служба приложений**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-230">In the **Pick a publish target** dialog, confirm that **App Service** is selected.</span></span>
1. <span data-ttu-id="87a8d-231">Выберите **Дополнительно**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-231">Select **Advanced**.</span></span> <span data-ttu-id="87a8d-232">Откроется диалоговое окно **Публикация**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-232">The **Publish** dialog opens.</span></span>
1. <span data-ttu-id="87a8d-233">В диалоговом окне **Публикация**:</span><span class="sxs-lookup"><span data-stu-id="87a8d-233">In the **Publish** dialog:</span></span>
   * <span data-ttu-id="87a8d-234">Убедитесь, что выбрана конфигурация **Выпуск**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-234">Confirm that the **Release** configuration is selected.</span></span>
   * <span data-ttu-id="87a8d-235">Откройте раскрывающийся список **Режим развертывания** и выберите вариант **Зависит от платформы**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-235">Open the **Deployment Mode** drop-down list and select **Framework-Dependent**.</span></span>
   * <span data-ttu-id="87a8d-236">Для параметра **Целевая среда выполнения** выберите значение **Переносимая**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-236">Select **Portable** as the **Target Runtime**.</span></span>
   * <span data-ttu-id="87a8d-237">Если потребуется удалить дополнительные файлы после развертывания, откройте **Параметры публикации файлов** и установите флажок для удаления дополнительных файлов в месте назначения.</span><span class="sxs-lookup"><span data-stu-id="87a8d-237">If you need to remove additional files upon deployment, open **File Publish Options** and select the check box to remove additional files at the destination.</span></span>
   * <span data-ttu-id="87a8d-238">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-238">Select **Save**.</span></span>
1. <span data-ttu-id="87a8d-239">Создайте новый сайт или обновите существующий, следуя остальным подсказкам мастера публикации.</span><span class="sxs-lookup"><span data-stu-id="87a8d-239">Create a new site or update an existing site by following the remaining prompts of the publish wizard.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="87a8d-240">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="87a8d-240">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="87a8d-241">В файле проекта не указывайте [идентификатор среды выполнения (RID)](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="87a8d-241">In the project file, don't specify a [Runtime Identifier (RID)](/dotnet/core/rid-catalog).</span></span>

1. <span data-ttu-id="87a8d-242">Из командной оболочки опубликуйте приложение в конфигурации выпуска, выполнив команду [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="87a8d-242">From a command shell, publish the app in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="87a8d-243">В следующем примере приложение публикуется как зависимое от платформы:</span><span class="sxs-lookup"><span data-stu-id="87a8d-243">In the following example, the app is published as a framework-dependent app:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="87a8d-244">Переместите содержимое каталога *bin/Release/{целевая_платформа}/publish* на сайт в Службе приложений.</span><span class="sxs-lookup"><span data-stu-id="87a8d-244">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* directory to the site in App Service.</span></span> <span data-ttu-id="87a8d-245">Если вы перетаскиваете содержимое папки *publish* с локального жесткого диска или общей папки непосредственно в Службу приложений на консоли [Kudu](https://github.com/projectkudu/kudu/wiki), перетащите файлы в папку `D:\home\site\wwwroot` на консоли Kudu.</span><span class="sxs-lookup"><span data-stu-id="87a8d-245">If dragging the *publish* folder contents from your local hard drive or network share directly to App Service in the [Kudu](https://github.com/projectkudu/kudu/wiki) console, drag the files to the `D:\home\site\wwwroot` folder in the Kudu console.</span></span>

---

### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="87a8d-246">Автономное развертывание приложения</span><span class="sxs-lookup"><span data-stu-id="87a8d-246">Deploy the app self-contained</span></span>

<span data-ttu-id="87a8d-247">Используйте Visual Studio или средство интерфейса командной строки (CLI) для [автономного развертывания (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="87a8d-247">Use Visual Studio or the command-line interface (CLI) tools for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="87a8d-248">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="87a8d-248">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="87a8d-249">Выберите **Сборка** > **Опубликовать {имя_приложения}** на панели инструментов Visual Studio или щелкните правой кнопкой мыши проект в **обозревателе решений** и выберите пункт **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-249">Select **Build** > **Publish {Application Name}** from the Visual Studio toolbar or right-click the project in **Solution Explorer** and select **Publish**.</span></span>
1. <span data-ttu-id="87a8d-250">В диалоговом оке **Выберите целевой объект публикации** убедитесь, что выбран вариант **Служба приложений**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-250">In the **Pick a publish target** dialog, confirm that **App Service** is selected.</span></span>
1. <span data-ttu-id="87a8d-251">Выберите **Дополнительно**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-251">Select **Advanced**.</span></span> <span data-ttu-id="87a8d-252">Откроется диалоговое окно **Публикация**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-252">The **Publish** dialog opens.</span></span>
1. <span data-ttu-id="87a8d-253">В диалоговом окне **Публикация**:</span><span class="sxs-lookup"><span data-stu-id="87a8d-253">In the **Publish** dialog:</span></span>
   * <span data-ttu-id="87a8d-254">Убедитесь, что выбрана конфигурация **Выпуск**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-254">Confirm that the **Release** configuration is selected.</span></span>
   * <span data-ttu-id="87a8d-255">Откройте раскрывающийся список **Режим развертывания** и выберите вариант **Автономное**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-255">Open the **Deployment Mode** drop-down list and select **Self-Contained**.</span></span>
   * <span data-ttu-id="87a8d-256">Выберите целевую среду выполнения из раскрывающегося списка **Целевая среда выполнения**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-256">Select the target runtime from the **Target Runtime** drop-down list.</span></span> <span data-ttu-id="87a8d-257">Значение по умолчанию — `win-x86`.</span><span class="sxs-lookup"><span data-stu-id="87a8d-257">The default is `win-x86`.</span></span>
   * <span data-ttu-id="87a8d-258">Если потребуется удалить дополнительные файлы после развертывания, откройте **Параметры публикации файлов** и установите флажок для удаления дополнительных файлов в месте назначения.</span><span class="sxs-lookup"><span data-stu-id="87a8d-258">If you need to remove additional files upon deployment, open **File Publish Options** and select the check box to remove additional files at the destination.</span></span>
   * <span data-ttu-id="87a8d-259">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="87a8d-259">Select **Save**.</span></span>
1. <span data-ttu-id="87a8d-260">Создайте новый сайт или обновите существующий, следуя остальным подсказкам мастера публикации.</span><span class="sxs-lookup"><span data-stu-id="87a8d-260">Create a new site or update an existing site by following the remaining prompts of the publish wizard.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="87a8d-261">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="87a8d-261">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="87a8d-262">В файле проекта укажите один или несколько [идентификаторов сред выполнения (RID)](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="87a8d-262">In the project file, specify one or more [Runtime Identifiers (RIDs)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="87a8d-263">Используйте `<RuntimeIdentifier>` (в единственном числе) для одного RID или `<RuntimeIdentifiers>` (во множественном числе), чтобы предоставить разделенный точкой с запятой список идентификаторов RID.</span><span class="sxs-lookup"><span data-stu-id="87a8d-263">Use `<RuntimeIdentifier>` (singular) for a single RID, or use `<RuntimeIdentifiers>` (plural) to provide a semicolon-delimited list of RIDs.</span></span> <span data-ttu-id="87a8d-264">В следующем примере указан RID `win-x86`:</span><span class="sxs-lookup"><span data-stu-id="87a8d-264">In the following example, the `win-x86` RID is specified:</span></span>

   ```xml
   <PropertyGroup>
     <TargetFramework>{TARGET FRAMEWORK}</TargetFramework>
     <RuntimeIdentifier>win-x86</RuntimeIdentifier>
   </PropertyGroup>
   ```

1. <span data-ttu-id="87a8d-265">Из командной оболочки опубликуйте приложение в конфигурации выпуска для среды выполнения узла, выполнив команду [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="87a8d-265">From a command shell, publish the app in Release configuration for the host's runtime with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="87a8d-266">В следующем примере публикуется приложение с RID `win-x86`.</span><span class="sxs-lookup"><span data-stu-id="87a8d-266">In the following example, the app is published for the `win-x86` RID.</span></span> <span data-ttu-id="87a8d-267">Предоставленный в параметре `--runtime` RID должены быть указан в свойстве `<RuntimeIdentifier>` (или `<RuntimeIdentifiers>`) в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="87a8d-267">The RID supplied to the `--runtime` option must be provided in the `<RuntimeIdentifier>` (or `<RuntimeIdentifiers>`) property in the project file.</span></span>

   ```console
   dotnet publish --configuration Release --runtime win-x86
   ```

1. <span data-ttu-id="87a8d-268">Переместите содержимое каталога *bin/Release/{целевая_платформа}/{идентификатор_среды_выполнения}/publish* на сайт в Службе приложений.</span><span class="sxs-lookup"><span data-stu-id="87a8d-268">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* directory to the site in App Service.</span></span> <span data-ttu-id="87a8d-269">Если вы перетаскиваете содержимое папки *publish* с локального жесткого диска или общей папки непосредственно в Службу приложений на консоли Kudu, перетащите файлы в папку `D:\home\site\wwwroot` на консоли Kudu.</span><span class="sxs-lookup"><span data-stu-id="87a8d-269">If dragging the *publish* folder contents from your local hard drive or network share directly to App Service in the Kudu console, drag the files to the `D:\home\site\wwwroot` folder in the Kudu console.</span></span>

---

## <a name="protocol-settings-https"></a><span data-ttu-id="87a8d-270">Параметры протокола (HTTPS)</span><span class="sxs-lookup"><span data-stu-id="87a8d-270">Protocol settings (HTTPS)</span></span>

<span data-ttu-id="87a8d-271">Привязки безопасных протоколов позволяют указать сертификат, который следует использовать при ответе на запросы по HTTPS.</span><span class="sxs-lookup"><span data-stu-id="87a8d-271">Secure protocol bindings allow you specify a certificate to use when responding to requests over HTTPS.</span></span> <span data-ttu-id="87a8d-272">Для привязки требуется допустимый закрытый сертификат (*PFX*), выданный для определенного имени узла.</span><span class="sxs-lookup"><span data-stu-id="87a8d-272">Binding requires a valid private certificate (*.pfx*) issued for the specific hostname.</span></span> <span data-ttu-id="87a8d-273">Дополнительные сведения см. в статье [Руководство. Привязывание существующего настраиваемого SSL-сертификата к Службе приложений Azure](/azure/app-service/app-service-web-tutorial-custom-ssl).</span><span class="sxs-lookup"><span data-stu-id="87a8d-273">For more information, see [Tutorial: Bind an existing custom SSL certificate to Azure App Service](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

## <a name="transform-webconfig"></a><span data-ttu-id="87a8d-274">Преобразование web.config</span><span class="sxs-lookup"><span data-stu-id="87a8d-274">Transform web.config</span></span>

<span data-ttu-id="87a8d-275">Если вам нужно преобразовать *web.config* при публикации (например, задать переменные среды на основе конфигурации, профиля или среды), см. раздел <xref:host-and-deploy/iis/transform-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="87a8d-275">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="87a8d-276">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="87a8d-276">Additional resources</span></span>

* [<span data-ttu-id="87a8d-277">Обзор Службы приложений</span><span class="sxs-lookup"><span data-stu-id="87a8d-277">App Service overview</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="87a8d-278">Служба приложений Azure: оптимальное место для размещения приложений .NET (55-минутное видео)</span><span class="sxs-lookup"><span data-stu-id="87a8d-278">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="87a8d-279">Пятница с Azure. Практика диагностики и устранения неполадок в Службе приложений Azure (12-минутное видео)</span><span class="sxs-lookup"><span data-stu-id="87a8d-279">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="87a8d-280">Общие сведения о диагностике в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="87a8d-280">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="87a8d-281">Служба приложений Azure на Windows Server использует [службы IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="87a8d-281">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="87a8d-282">Технологии IIS посвящены следующие статьи.</span><span class="sxs-lookup"><span data-stu-id="87a8d-282">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="87a8d-283">Windows Server — содержимое для ИТ-администраторов по текущим и предыдущим выпускам</span><span class="sxs-lookup"><span data-stu-id="87a8d-283">Windows Server - IT administrator content for current and previous releases</span></span>](/windows-server/windows-server-versions)
