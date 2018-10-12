---
title: Развертывание приложений ASP.NET Core в Службе приложений Azure
author: guardrex
description: Эта статья содержит ссылки на ресурсы по размещению и развертыванию в Azure.
ms.author: riande
ms.custom: mvc
ms.date: 08/29/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: f2de81af4bd2992aec76a287484d0057021231d8
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/01/2018
ms.locfileid: "47860970"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a><span data-ttu-id="af126-103">Развертывание приложений ASP.NET Core в Службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="af126-103">Deploy ASP.NET Core apps to Azure App Service</span></span>

<span data-ttu-id="af126-104">[Служба приложений Azure](https://azure.microsoft.com/services/app-service/) — это [платформа облачных вычислений Microsoft](https://azure.microsoft.com/), предназначенная для размещения веб-приложений, включая ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="af126-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="af126-105">Полезные ресурсы</span><span class="sxs-lookup"><span data-stu-id="af126-105">Useful resources</span></span>

<span data-ttu-id="af126-106">[Документация по веб-приложениям](/azure/app-service/) Azure — это место, где хранятся документация, учебники, примеры, руководства и другие ресурсы, связанные с приложениями Azure.</span><span class="sxs-lookup"><span data-stu-id="af126-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="af126-107">Размещению приложений ASP.NET Core посвящены следующие два руководства.</span><span class="sxs-lookup"><span data-stu-id="af126-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="af126-108">Краткое руководство. Создание веб-приложения ASP.NET Core в Azure</span><span class="sxs-lookup"><span data-stu-id="af126-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="af126-109">Создайте веб-приложение ASP.NET Core и разверните его в службе приложений Azure на базе Windows с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="af126-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="af126-110">Краткое руководство. Создание веб-приложения .NET Core в службе приложений на базе Linux</span><span class="sxs-lookup"><span data-stu-id="af126-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="af126-111">Создайте веб-приложение ASP.NET Core и разверните его в службе приложений Azure на базе Linux с помощью командной строки.</span><span class="sxs-lookup"><span data-stu-id="af126-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="af126-112">Следующие статьи входят в документацию по ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="af126-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="af126-113">Опубликовать в Azure с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="af126-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="af126-114">Сведения о публикации приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="af126-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="af126-115">Непрерывное развертывание в Azure с помощью Visual Studio и Git</span><span class="sxs-lookup"><span data-stu-id="af126-115">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="af126-116">Сведения о создании веб-приложения ASP.NET Core с помощью Visual Studio и его развертывании в службе приложений Azure с использованием Git для непрерывного развертывания.</span><span class="sxs-lookup"><span data-stu-id="af126-116">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="af126-117">Создание первого конвейера с помощью Azure Pipelines</span><span class="sxs-lookup"><span data-stu-id="af126-117">Create your first pipeline with Azure Pipelines</span></span>](/azure/devops/pipelines/get-started-yaml)  
<span data-ttu-id="af126-118">Сведения о настройке сборки CI для приложения ASP.NET Core и последующем создании выпуска непрерывного развертывания в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="af126-118">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="af126-119">Песочница веб-приложений Azure</span><span class="sxs-lookup"><span data-stu-id="af126-119">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="af126-120">Сведения об ограничениях среды выполнения службы приложений Azure, создаваемых платформой приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="af126-120">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a><span data-ttu-id="af126-121">Настройка приложения</span><span class="sxs-lookup"><span data-stu-id="af126-121">Application configuration</span></span>

<span data-ttu-id="af126-122">В ASP.NET Core 2.0 или более поздней версии следующие пакеты NuGet предоставляют возможности автоматического ведения журнала для приложений, развернутых в службе приложений Azure:</span><span class="sxs-lookup"><span data-stu-id="af126-122">In ASP.NET Core 2.0 or later, the following NuGet packages provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="af126-123">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) использует [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) для интеграции подсветки ASP.NET Core в службу приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="af126-123">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="af126-124">Пакет `Microsoft.AspNetCore.AzureAppServicesIntegration` включает дополнительные функции для ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="af126-124">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="af126-125">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) выполняет [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) для добавления поставщиков ведения журналов диагностики из службы приложений Azure в пакет `Microsoft.Extensions.Logging.AzureAppServices`.</span><span class="sxs-lookup"><span data-stu-id="af126-125">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="af126-126">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) предоставляет реализации средства ведения журналов в поддержку журналов диагностики службы приложений Azure и функций потоковой передачи журналов.</span><span class="sxs-lookup"><span data-stu-id="af126-126">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="af126-127">Если планируется использовать .NET Core и указана ссылка на [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage), пакеты уже включены.</span><span class="sxs-lookup"><span data-stu-id="af126-127">If targeting .NET Core and referencing the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), the packages are already included.</span></span> <span data-ttu-id="af126-128">Пакеты отсутствуют в более новом [метапакете Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="af126-128">The packages are absent from the newer [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="af126-129">Если планируется использовать .NET Framework или указана ссылка на метапакет `Microsoft.AspNetCore.App`, ссылайтесь на отдельные пакеты ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="af126-129">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, reference the individual logging packages.</span></span>

::: moniker-end

## <a name="override-app-configuration-using-the-azure-portal"></a><span data-ttu-id="af126-130">Переопределение конфигурации приложения с помощью портала Azure</span><span class="sxs-lookup"><span data-stu-id="af126-130">Override app configuration using the Azure Portal</span></span>

<span data-ttu-id="af126-131">Область **Параметры приложения** колонки **Параметры приложения** позволяет устанавливать переменные среды для приложения.</span><span class="sxs-lookup"><span data-stu-id="af126-131">The **App Settings** area of the **Application settings** blade permits you to set environment variables for the app.</span></span> <span data-ttu-id="af126-132">Переменные среды могут использоваться [поставщиком конфигураций переменных среды](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="af126-132">Environment variables can be consumed by the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="af126-133">Если приложение использует [Веб-узел](xref:fundamentals/host/web-host) и создает узел с помощью [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), переменные среды, которые настраивают узел, используют префикс `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="af126-133">When an app uses the [Web Host](xref:fundamentals/host/web-host) and builds the host using [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), environment variables that configure the host use the `ASPNETCORE_` prefix.</span></span> <span data-ttu-id="af126-134">Дополнительные сведения см. в разделах <xref:fundamentals/host/web-host> и [Конфигурация для разных сред](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="af126-134">For more information, see <xref:fundamentals/host/web-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="af126-135">Если приложение использует [универсальный узел](xref:fundamentals/host/generic-host), переменные среды не загружаются в конфигурацию приложения по умолчанию и поставщик конфигурации должен быть добавлен разработчиком.</span><span class="sxs-lookup"><span data-stu-id="af126-135">When an app uses the [Generic Host](xref:fundamentals/host/generic-host), environment variables aren't loaded into an app's configuration by default and the configuration provider must be added by the developer.</span></span> <span data-ttu-id="af126-136">Разработчик определяет префикс переменной среды при добавлении поставщика конфигурации.</span><span class="sxs-lookup"><span data-stu-id="af126-136">The developer determines the environment variable prefix when the configuration provider is added.</span></span> <span data-ttu-id="af126-137">Дополнительные сведения см. в разделах <xref:fundamentals/host/generic-host> и [Конфигурация для разных сред](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="af126-137">For more information, see <xref:fundamentals/host/generic-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="af126-138">Сценарии использования прокси-сервера и подсистемы балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="af126-138">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="af126-139">ПО промежуточного слоя для интеграции IIS, которое настраивает ПО промежуточного слоя переадресации заголовков, и модуль ASP.NET Core настраиваются на пересылку схемы (HTTP/HTTPS) и удаленного IP-адреса расположения, где был сформирован запрос.</span><span class="sxs-lookup"><span data-stu-id="af126-139">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="af126-140">Для приложений, размещенных за дополнительными прокси-серверами и подсистемами балансировки нагрузки, может потребоваться дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="af126-140">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="af126-141">Дополнительные сведения см. в разделе [Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="af126-141">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="af126-142">Мониторинг и ведение журналов</span><span class="sxs-lookup"><span data-stu-id="af126-142">Monitoring and logging</span></span>

<span data-ttu-id="af126-143">Сведения о мониторинге, ведении журналов, а также поиске и устранении неполадок см. в следующих статьях.</span><span class="sxs-lookup"><span data-stu-id="af126-143">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="af126-144">Мониторинг приложений в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="af126-144">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="af126-145">Сведения о том, как толковать квоты и параметры для приложений и планы службы приложений.</span><span class="sxs-lookup"><span data-stu-id="af126-145">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="af126-146">Включение функции ведения журналов диагностики для веб-приложений в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="af126-146">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="af126-147">Сведения о том, как включать и где искать функцию ведения журнала диагностики для кодов статуса HTTP, невыполненных запросов и активности веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="af126-147">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="af126-148">Общие сведения об обработке ошибок в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="af126-148">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="af126-149">Сведения об основных методах обработки ошибок в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="af126-149">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="af126-150">Устранение неполадок ASP.NET Core в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="af126-150">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="af126-151">Сведения о диагностике проблем с развертыванием приложений ASP.NET Core в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="af126-151">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="af126-152">Справочник по общим ошибкам для службы приложений Azure и служб IIS с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="af126-152">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="af126-153">См. распространенные ошибки с конфигурацией развертывания для приложений, размещенных в службе приложений Azure/IIS, и рекомендации по их устранению.</span><span class="sxs-lookup"><span data-stu-id="af126-153">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="af126-154">Связка ключей для защиты данных и слоты развертывания</span><span class="sxs-lookup"><span data-stu-id="af126-154">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="af126-155">[Ключи для защиты данных](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) хранятся в папке *%HOME%\ASP.NET\DataProtection-Keys*.</span><span class="sxs-lookup"><span data-stu-id="af126-155">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="af126-156">Эта папка копируется в сетевое хранилище и синхронизируется на всех машинах, где размещается приложение.</span><span class="sxs-lookup"><span data-stu-id="af126-156">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="af126-157">Во время хранения ключи не защищаются.</span><span class="sxs-lookup"><span data-stu-id="af126-157">Keys aren't protected at rest.</span></span> <span data-ttu-id="af126-158">В этой папке хранится связка ключей для всех экземпляров приложения в одном и том же слоте развертывания.</span><span class="sxs-lookup"><span data-stu-id="af126-158">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="af126-159">Отдельные слоты развертывания, такие как промежуточное хранение и производство, не используют общую связку ключей.</span><span class="sxs-lookup"><span data-stu-id="af126-159">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="af126-160">При переключении между разными слотами развертывания ни одна система с использованием защиты данных не сможет расшифровать хранимые данные, используя связку ключей из предыдущего слота.</span><span class="sxs-lookup"><span data-stu-id="af126-160">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="af126-161">Для защиты файлов cookie ПО промежуточного слоя ASP.NET Cookie использует защиту данных.</span><span class="sxs-lookup"><span data-stu-id="af126-161">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="af126-162">В результате пользователей, применяющих ПО промежуточного слоя ASP.NET Cookie, выбрасывает из приложения.</span><span class="sxs-lookup"><span data-stu-id="af126-162">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="af126-163">Для того чтобы решение связки ключей не зависело от слота, используйте внешнего поставщика связки ключей, например:</span><span class="sxs-lookup"><span data-stu-id="af126-163">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="af126-164">Хранилище больших двоичных объектов Azure;</span><span class="sxs-lookup"><span data-stu-id="af126-164">Azure Blob Storage</span></span>
* <span data-ttu-id="af126-165">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="af126-165">Azure Key Vault</span></span>
* <span data-ttu-id="af126-166">Хранилище SQL;</span><span class="sxs-lookup"><span data-stu-id="af126-166">SQL store</span></span>
* <span data-ttu-id="af126-167">Кэш Redis.</span><span class="sxs-lookup"><span data-stu-id="af126-167">Redis cache</span></span>

<span data-ttu-id="af126-168">Дополнительные сведения см. в разделе [Поставщики хранилища ключей](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="af126-168">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="af126-169">Развертывание предварительной версии ASP.NET Core в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="af126-169">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="af126-170">Предварительную версию приложения ASP.NET Core можно развернуть в службе приложений Azure, используя следующие методы.</span><span class="sxs-lookup"><span data-stu-id="af126-170">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* <span data-ttu-id="af126-171">[Установка расширения сайта предварительной версии](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span><span class="sxs-lookup"><span data-stu-id="af126-171">[Install the preview site extension](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span></span>
* [<span data-ttu-id="af126-172">Использование Docker с веб-приложениями для контейнеров</span><span class="sxs-lookup"><span data-stu-id="af126-172">Use Docker with Web Apps for containers</span></span>](#use-docker-with-web-apps-for-containers)

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="af126-173">Установка расширения сайта предварительной версии</span><span class="sxs-lookup"><span data-stu-id="af126-173">Install the preview site extension</span></span>

<span data-ttu-id="af126-174">Если у вас возникли проблемы при использовании расширения сайта предварительной версии, сообщите о них на [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span><span class="sxs-lookup"><span data-stu-id="af126-174">If a problem occurs using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

1. <span data-ttu-id="af126-175">На портале Azure перейдите к колонке "Служба приложений".</span><span class="sxs-lookup"><span data-stu-id="af126-175">From the Azure portal, navigate to the App Service blade.</span></span>
1. <span data-ttu-id="af126-176">Выберите веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="af126-176">Select the web app.</span></span>
1. <span data-ttu-id="af126-177">В поле поиска введите "ex" или прокрутите список разделов управления до пункта **СРЕДСТВА РАЗРАБОТКИ**.</span><span class="sxs-lookup"><span data-stu-id="af126-177">Type "ex" in the search box or scroll down the list of management sections to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="af126-178">Выберите **СРЕДСТВА РАЗРАБОТКИ** > **Расширения**.</span><span class="sxs-lookup"><span data-stu-id="af126-178">Select **DEVELOPMENT TOOLS** > **Extensions**.</span></span>
1. <span data-ttu-id="af126-179">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="af126-179">Select **Add**.</span></span>
1. <span data-ttu-id="af126-180">Выберите расширение **Среда выполнения ASP.NET Core &lt;x.y&gt; (x86)** из списка, где `<x.y>` — это предварительная версия ASP.NET Core (например, **Среда выполнения ASP.NET Core 2.2 (x86)**).</span><span class="sxs-lookup"><span data-stu-id="af126-180">Select the **ASP.NET Core &lt;x.y&gt; (x86) Runtime** extension from the list, where `<x.y>` is the ASP.NET Core preview version (for example, **ASP.NET Core 2.2 (x86) Runtime**).</span></span> <span data-ttu-id="af126-181">Среда выполнения x86 лучше всего подходит для [развертываний, зависимых от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd), которые используют размещение вне процесса модулем ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="af126-181">The x86 runtime is appropriate for [framework-dependent deployments](/dotnet/core/deploying/#framework-dependent-deployments-fdd) that rely on out-of-process hosting by the ASP.NET Core Module.</span></span>
1. <span data-ttu-id="af126-182">Для принятия условий нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="af126-182">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="af126-183">Нажмите **OK**, чтобы установить расширение.</span><span class="sxs-lookup"><span data-stu-id="af126-183">Select **OK** to install the extension.</span></span>

<span data-ttu-id="af126-184">По завершении операции устанавливается последняя предварительная версия .NET Core.</span><span class="sxs-lookup"><span data-stu-id="af126-184">When the operation completes, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="af126-185">Проверьте установку:</span><span class="sxs-lookup"><span data-stu-id="af126-185">Verify the installation:</span></span>

1. <span data-ttu-id="af126-186">Выберите **Дополнительные средства** в разделе **СРЕДСТВА РАЗРАБОТКИ**.</span><span class="sxs-lookup"><span data-stu-id="af126-186">Select **Advanced Tools** under **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="af126-187">Выберите **Go** в колонке **Дополнительные средства**.</span><span class="sxs-lookup"><span data-stu-id="af126-187">Select **Go** on the **Advanced Tools** blade.</span></span>
1. <span data-ttu-id="af126-188">Выберите пункт меню **Консоль отладки** > **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="af126-188">Select the **Debug console** > **PowerShell** menu item.</span></span>
1. <span data-ttu-id="af126-189">По запросу PowerShell выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="af126-189">At the PowerShell prompt, execute the following command.</span></span> <span data-ttu-id="af126-190">Замените версию среды выполнения ASP.NET Core на `<x.y>` в команде:</span><span class="sxs-lookup"><span data-stu-id="af126-190">Substitute the ASP.NET Core runtime version for `<x.y>` in the command:</span></span>

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x86\
   ```
   <span data-ttu-id="af126-191">Если установленная предварительная версия среды выполнения предназначена для ASP.NET Core 2.2, используется следующая команда:</span><span class="sxs-lookup"><span data-stu-id="af126-191">If the installed preview runtime is for ASP.NET Core 2.2, the command is:</span></span>
   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x86\
   ```
   <span data-ttu-id="af126-192">Эта команда возвращает `True`, если установлена предварительная версия среды выполнения x64.</span><span class="sxs-lookup"><span data-stu-id="af126-192">The command returns `True` when the x64 preview runtime is installed.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="af126-193">Архитектура платформы (x86/x64) приложения служб приложений задается в колонке **Параметры приложения** в разделе **Общие параметры** для приложений, размещенных на вычислительных ресурсах серии A или более высоком уровне.</span><span class="sxs-lookup"><span data-stu-id="af126-193">The platform architecture (x86/x64) of an App Services app is set in the **Application Settings** blade under **General Settings** for apps that are hosted on an A-series compute or better hosting tier.</span></span> <span data-ttu-id="af126-194">Если приложение выполняется во внутрипроцессном режиме, а архитектура платформы настроена для 64-разрядных версий (x64), модуль ASP.NET Core использует 64-разрядную предварительную версию среды выполнения при ее наличии.</span><span class="sxs-lookup"><span data-stu-id="af126-194">If the app is run in in-process mode and the platform architecture is configured for 64-bit (x64), the ASP.NET Core Module uses the 64-bit preview runtime, if present.</span></span> <span data-ttu-id="af126-195">Установите расширение **Среда выполнения ASP.NET Core &lt;x.y&gt; (x64)** (например, **Среда выполнения ASP.NET Core 2.2 (x64)**).</span><span class="sxs-lookup"><span data-stu-id="af126-195">Install the **ASP.NET Core &lt;x.y&gt; (x64) Runtime** extension (for example, **ASP.NET Core 2.2 (x64) Runtime**).</span></span>
>
> <span data-ttu-id="af126-196">После установки предварительной версии среды выполнения x64 выполните следующую команду в командном окне Kudu PowerShell, чтобы проверить установку.</span><span class="sxs-lookup"><span data-stu-id="af126-196">After installing the x64 preview runtime, run the following command in the Kudu PowerShell command window to verify the installation.</span></span> <span data-ttu-id="af126-197">Замените версию среды выполнения ASP.NET Core на `<x.y>` в команде:</span><span class="sxs-lookup"><span data-stu-id="af126-197">Substitute the ASP.NET Core runtime version for `<x.y>` in the command:</span></span>
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x64\
> ```
> <span data-ttu-id="af126-198">Если установленная предварительная версия среды выполнения предназначена для ASP.NET Core 2.2, используется следующая команда:</span><span class="sxs-lookup"><span data-stu-id="af126-198">If the installed preview runtime is for ASP.NET Core 2.2, the command is:</span></span>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x64\
> ```
> <span data-ttu-id="af126-199">Эта команда возвращает `True`, если установлена предварительная версия среды выполнения x64.</span><span class="sxs-lookup"><span data-stu-id="af126-199">The command returns `True` when the x64 preview runtime is installed.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="af126-200">**Расширения ASP.NET Core** включают дополнительные функциональные возможности для ASP.NET Core в службах приложений Azure, например включение ведения журналов Azure.</span><span class="sxs-lookup"><span data-stu-id="af126-200">**ASP.NET Core Extensions** enables additional functionality for ASP.NET Core on Azure App Services, such as enabling Azure logging.</span></span> <span data-ttu-id="af126-201">Расширение устанавливается автоматически при развертывании из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="af126-201">The extension is installed automatically when deploying from Visual Studio.</span></span> <span data-ttu-id="af126-202">Если расширение не установлено, установите его для приложения.</span><span class="sxs-lookup"><span data-stu-id="af126-202">If the extension isn't installed, install it for the app.</span></span>

<span data-ttu-id="af126-203">**Использование расширения сайта предварительной версии с шаблоном ARM**</span><span class="sxs-lookup"><span data-stu-id="af126-203">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="af126-204">Если вы используете шаблон ARM для создания и развертывания приложений, можно использовать тип ресурса `siteextensions`, чтобы добавить расширение сайта в веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="af126-204">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="af126-205">Пример:</span><span class="sxs-lookup"><span data-stu-id="af126-205">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

<!--
### Deploy the app self-contained

A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment. When deploying a self-contained app:

* The site doesn't need to be prepared.
* The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.

Self-contained apps are an option for all ASP.NET Core apps.
-->

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="af126-206">Использование Docker с веб-приложениями для контейнеров</span><span class="sxs-lookup"><span data-stu-id="af126-206">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="af126-207">[Центр Docker](https://hub.docker.com/r/microsoft/aspnetcore/) содержит образы Docker из последней предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="af126-207">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="af126-208">Их можно использовать в качестве базового образа.</span><span class="sxs-lookup"><span data-stu-id="af126-208">The images can be used as a base image.</span></span> <span data-ttu-id="af126-209">Использование образа и развертывание в веб-приложениях для контейнеров выполняется как обычно.</span><span class="sxs-lookup"><span data-stu-id="af126-209">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="af126-210">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="af126-210">Additional resources</span></span>

* [<span data-ttu-id="af126-211">Общие сведения о веб-приложениях (5-минутное видео)</span><span class="sxs-lookup"><span data-stu-id="af126-211">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="af126-212">Служба приложений Azure: оптимальное место для размещения приложений .NET (55-минутное видео)</span><span class="sxs-lookup"><span data-stu-id="af126-212">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="af126-213">Azure, пятница: практики диагностики и устранения неполадок в службе приложений Azure (12-минутное видео)</span><span class="sxs-lookup"><span data-stu-id="af126-213">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="af126-214">Общие сведения о диагностике в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="af126-214">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="af126-215">Служба приложений Azure на Windows Server использует [службы IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="af126-215">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="af126-216">Технологии IIS посвящены следующие статьи.</span><span class="sxs-lookup"><span data-stu-id="af126-216">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="af126-217">Библиотека Microsoft TechNet: Windows Server</span><span class="sxs-lookup"><span data-stu-id="af126-217">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
