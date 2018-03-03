---
title: "Размещение ASP.NET Core в службе приложений Azure"
author: guardrex
description: "Узнайте, как размещать приложения ASP.NET Core в службе приложений Azure, с помощью ссылок на полезные ресурсы."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 8da97b2148c0a784e3b1fdfe210db35e3fcceade
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2018
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="dc438-103">Размещение ASP.NET Core в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="dc438-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="dc438-104">[Служба приложений Azure](https://azure.microsoft.com/services/app-service/) — это [платформа облачных вычислений Microsoft](https://azure.microsoft.com/), предназначенная для размещения веб-приложений, включая ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dc438-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

## <a name="useful-resources"></a><span data-ttu-id="dc438-105">Полезные ресурсы</span><span class="sxs-lookup"><span data-stu-id="dc438-105">Useful resources</span></span>

<span data-ttu-id="dc438-106">[Документация по веб-приложениям](/azure/app-service/) Azure — это место, где хранятся документация, учебники, примеры, руководства и другие ресурсы, связанные с приложениями Azure.</span><span class="sxs-lookup"><span data-stu-id="dc438-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="dc438-107">Размещению приложений ASP.NET Core посвящены следующие два руководства.</span><span class="sxs-lookup"><span data-stu-id="dc438-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="dc438-108">Краткое руководство. Создание веб-приложения ASP.NET Core в Azure</span><span class="sxs-lookup"><span data-stu-id="dc438-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="dc438-109">Создайте веб-приложение ASP.NET Core и разверните его в службе приложений Azure на базе Windows с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dc438-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="dc438-110">Краткое руководство. Создание веб-приложения .NET Core в службе приложений на базе Linux</span><span class="sxs-lookup"><span data-stu-id="dc438-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="dc438-111">Создайте веб-приложение ASP.NET Core и разверните его в службе приложений Azure на базе Linux с помощью командной строки.</span><span class="sxs-lookup"><span data-stu-id="dc438-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="dc438-112">Следующие статьи входят в документацию по ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dc438-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="dc438-113">Опубликовать в Azure с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc438-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="dc438-114">Сведения о публикации приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dc438-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="dc438-115">Публикация в Azure с инструментами командной строки</span><span class="sxs-lookup"><span data-stu-id="dc438-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="dc438-116">Сведения о публикации приложения ASP.NET Core в службе приложений Azure с помощью клиента командной строки Git.</span><span class="sxs-lookup"><span data-stu-id="dc438-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="dc438-117">Непрерывное развертывание в Azure с помощью Visual Studio и Git</span><span class="sxs-lookup"><span data-stu-id="dc438-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="dc438-118">Сведения о создании веб-приложения ASP.NET Core с помощью Visual Studio и его развертывании в службе приложений Azure с использованием Git для непрерывного развертывания.</span><span class="sxs-lookup"><span data-stu-id="dc438-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="dc438-119">Непрерывное развертывание в Azure с помощью VSTS</span><span class="sxs-lookup"><span data-stu-id="dc438-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="dc438-120">Сведения о настройке сборки CI для приложения ASP.NET Core и последующем создании выпуска непрерывного развертывания в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="dc438-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="dc438-121">Настройка приложения</span><span class="sxs-lookup"><span data-stu-id="dc438-121">Application configuration</span></span>

<span data-ttu-id="dc438-122">При использовании ASP.NET Core 2.0 и более поздних версий три пакета в составе [метапакета Microsoft.AspNetCore.All](xref:fundamentals/metapackage) предоставляют функции автоматического ведения журналов для приложений, развернутых в службе приложений Azure:</span><span class="sxs-lookup"><span data-stu-id="dc438-122">With ASP.NET Core 2.0 and later, three packages in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="dc438-123">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) использует [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) для интеграции подсветки ASP.NET Core в службу приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="dc438-123">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) to provide ASP.NET Core lightup integration with Azure App Service.</span></span> <span data-ttu-id="dc438-124">Пакет `Microsoft.AspNetCore.AzureAppServicesIntegration` включает дополнительные функции для ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="dc438-124">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="dc438-125">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) выполняет [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) для добавления поставщиков ведения журналов диагностики из службы приложений Azure в пакет `Microsoft.Extensions.Logging.AzureAppServices`.</span><span class="sxs-lookup"><span data-stu-id="dc438-125">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="dc438-126">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) предоставляет реализации средства ведения журналов в поддержку журналов диагностики службы приложений Azure и функций потоковой передачи журналов.</span><span class="sxs-lookup"><span data-stu-id="dc438-126">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="dc438-127">Мониторинг и ведение журналов</span><span class="sxs-lookup"><span data-stu-id="dc438-127">Monitoring and logging</span></span>

<span data-ttu-id="dc438-128">Сведения о мониторинге, ведении журналов, а также поиске и устранении неполадок см. в следующих статьях.</span><span class="sxs-lookup"><span data-stu-id="dc438-128">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="dc438-129">Мониторинг приложений в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="dc438-129">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="dc438-130">Сведения о том, как толковать квоты и параметры для приложений и планы службы приложений.</span><span class="sxs-lookup"><span data-stu-id="dc438-130">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="dc438-131">Включение функции ведения журналов диагностики для веб-приложений в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="dc438-131">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="dc438-132">Сведения о том, как включать и где искать функцию ведения журнала диагностики для кодов статуса HTTP, невыполненных запросов и активности веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="dc438-132">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="dc438-133">Общие сведения об обработке ошибок в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dc438-133">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="dc438-134">Сведения об основных методах обработки ошибок в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dc438-134">Understand common appoaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="dc438-135">Устранение неполадок ASP.NET Core в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="dc438-135">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="dc438-136">Сведения о диагностике проблем с развертыванием приложений ASP.NET Core в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="dc438-136">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="dc438-137">Справочник по общим ошибкам для службы приложений Azure и служб IIS с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dc438-137">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="dc438-138">См. распространенные ошибки с конфигурацией развертывания для приложений, размещенных в службе приложений Azure/IIS, и рекомендации по их устранению.</span><span class="sxs-lookup"><span data-stu-id="dc438-138">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="dc438-139">Связка ключей для защиты данных и слоты развертывания</span><span class="sxs-lookup"><span data-stu-id="dc438-139">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="dc438-140">[Ключи для защиты данных](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) хранятся в папке *%HOME%\ASP.NET\DataProtection-Keys*.</span><span class="sxs-lookup"><span data-stu-id="dc438-140">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="dc438-141">Эта папка копируется в сетевое хранилище и синхронизируется на всех машинах, где размещается приложение.</span><span class="sxs-lookup"><span data-stu-id="dc438-141">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="dc438-142">Во время хранения ключи не защищаются.</span><span class="sxs-lookup"><span data-stu-id="dc438-142">Keys aren't protected at rest.</span></span> <span data-ttu-id="dc438-143">В этой папке хранится связка ключей для всех экземпляров приложения в одном и том же слоте развертывания.</span><span class="sxs-lookup"><span data-stu-id="dc438-143">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="dc438-144">Отдельные слоты развертывания, такие как промежуточное хранение и производство, не используют общую связку ключей.</span><span class="sxs-lookup"><span data-stu-id="dc438-144">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="dc438-145">При переключении между разными слотами развертывания ни одна система с использованием защиты данных не сможет расшифровать хранимые данные, используя связку ключей из предыдущего слота.</span><span class="sxs-lookup"><span data-stu-id="dc438-145">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="dc438-146">Для защиты файлов cookie ПО промежуточного слоя ASP.NET Cookie использует защиту данных.</span><span class="sxs-lookup"><span data-stu-id="dc438-146">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="dc438-147">В результате пользователей, применяющих ПО промежуточного слоя ASP.NET Cookie, выбрасывает из приложения.</span><span class="sxs-lookup"><span data-stu-id="dc438-147">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="dc438-148">Для того чтобы решение связки ключей не зависело от слота, используйте внешнего поставщика связки ключей, например:</span><span class="sxs-lookup"><span data-stu-id="dc438-148">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="dc438-149">Хранилище больших двоичных объектов Azure;</span><span class="sxs-lookup"><span data-stu-id="dc438-149">Azure Blob Storage</span></span>
* <span data-ttu-id="dc438-150">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="dc438-150">Azure Key Vault</span></span>
* <span data-ttu-id="dc438-151">Хранилище SQL;</span><span class="sxs-lookup"><span data-stu-id="dc438-151">SQL store</span></span>
* <span data-ttu-id="dc438-152">Кэш Redis.</span><span class="sxs-lookup"><span data-stu-id="dc438-152">Redis cache</span></span>

<span data-ttu-id="dc438-153">Дополнительные сведения см. в разделе [Поставщики хранилища ключей](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="dc438-153">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dc438-154">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="dc438-154">Additional resources</span></span>

* [<span data-ttu-id="dc438-155">Общие сведения о веб-приложениях (5-минутное видео)</span><span class="sxs-lookup"><span data-stu-id="dc438-155">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="dc438-156">Служба приложений Azure: оптимальное место для размещения приложений .NET (55-минутное видео)</span><span class="sxs-lookup"><span data-stu-id="dc438-156">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="dc438-157">Azure, пятница: практики диагностики и устранения неполадок в службе приложений Azure (12-минутное видео)</span><span class="sxs-lookup"><span data-stu-id="dc438-157">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="dc438-158">Общие сведения о диагностике в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="dc438-158">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)

<span data-ttu-id="dc438-159">Служба приложений Azure на Windows Server использует [службы IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="dc438-159">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="dc438-160">Технологии IIS посвящены следующие статьи.</span><span class="sxs-lookup"><span data-stu-id="dc438-160">The following topics pertain to the underlying IIS technology:</span></span>

* [<span data-ttu-id="dc438-161">Размещение ASP.NET Core в Windows со службами IIS</span><span class="sxs-lookup"><span data-stu-id="dc438-161">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="dc438-162">Введение в модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dc438-162">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="dc438-163">Справочник по конфигурации модуля ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dc438-163">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="dc438-164">Использование модулей IIS с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dc438-164">Using IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="dc438-165">Библиотека Microsoft TechNet: Windows Server</span><span class="sxs-lookup"><span data-stu-id="dc438-165">Microsoft TechNet Library: Windows Server</span></span>](https://docs.microsoft.com/windows-server/windows-server-versions)
